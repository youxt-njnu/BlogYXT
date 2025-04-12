---
title: AI助力| python帮写mdx文档
tags:
 - AI
 - python
 - 前端文档建设
categories:
 - AI提效
comments: true
date: 2025-04-12 16:44:05
photos: https://wallroom.io/img/1920x1200/bg-0e129fe.png
cover: https://wallroom.io/img/1920x1200/bg-0e129fe.png
---

# 背景

实习过程中，需要完善文档站中组件库的组件文档，组件文档包括了组件介绍、组件API表格、demo示例等内容。

```
__dockit__
  index.mdx - 组件文档
  demo - 组件demo文件夹，包含多个demo
  apis - 组件API表格文件夹，包含index.ts
```

流程中重复的步骤比较多，但都是基于已有组件里的代码，抽离出需要的内容，按照格式和需要填充在不同的地方；

填充过程中，有部分需要人工明确：
1. 组件代码中的props格式处理和传入；
2. 组件的路径；
3. 组件demo是否传入数据后，能正常运行（能万事大吉，不能得找源码）
4. 组件的API表格，对每个字段的解释（常规通用的字段能解读，但有些字段需要结合源码来理解）

基于一套数据，可以自动化生成的：
1. mdx文档中，组件的名称、gitlab路径、组件的demo路径、组件的props和default内容；
2. demo文件夹中，常规demo的代码；
3. 组件API表格的内容

所以通过deepseek的多次问答修正，加上google colab提供的python环境，编写了一套半自动化的代码；
传入人工从组件源码中阅读拿到的信息，多次运行，拿到index.mdx demo和apis的内容;
人工阅读源码，补充apis里对props的解释。

现阶段的代码有挺多情况未考虑并兼容（源码并不是规范化的一套风格；多数是我传入数据的时候小心点来解决的；）
代码也有冗余的内容，此外多数是通过正则处理字符串实现的。
但能用，所有目的达到了，又有什么大问题呢（嘿哈🤭

# 实现代码

``` python
import re

def parse_props_string(input_str):
    # 预处理字符串
    cleaned = re.sub(r'[\n\t]', '', input_str.strip('{} '))

    # 优化后的正则表达式
    prop_pattern = re.compile(
        r'\s*(\w+)\s*:\s*'                # 属性名
        r'('
        r'{.*?}(?=\s*,|\s*$)|'            # 匹配对象类型
        r'[^,]+(?=\s*,|\s*$)'             # 匹配直接类型
        r')',
        re.DOTALL
    )

    props_dict = {}
    for match in re.finditer(prop_pattern, cleaned):
        prop_name = match.group(1)
        prop_value = match.group(2).strip()

        # 处理对象类型属性
        if prop_value.startswith('{'):
            # 优化内部解析
            inner = prop_value[1:-1].strip()
            type_match = re.search(r'type\s*:\s*([^,]+?)\s*(?:,|$)', inner)
            default_match = re.search(r'default\s*:\s*([^,]+?)\s*(?:,|$)', inner)

            prop_data = {}
            if type_match:
                prop_data["type"] = type_match.group(1).strip()
            if default_match:
                prop_data["default"] = default_match.group(1).strip()

            props_dict[prop_name] = prop_data
        else:
            # 直接类型处理
            props_dict[prop_name] = prop_value.strip()

    return props_dict

# 运行测试
# test_props = parse_props_string(props)
```

```python
def generate_props_definition(title, props_data):
    # 正则表达式匹配 definePropType<...> 模式
    pattern = re.compile(r'definePropType<(.+?)>')

    prop_lines = []
    for prop_name, prop_def in props_data.items():
        # 获取类型表达式
        type_expr = None
        if isinstance(prop_def, dict):
            type_expr = prop_def.get('type', 'any')
        else:
            type_expr = prop_def  # 处理直接写类型的格式

        # 提取泛型类型
        match = pattern.search(str(type_expr))
        if match:
            ts_type = match.group(1)
        else:
            # 处理普通类型声明（取第一个单词）
            ts_type = type_expr.split('(')[0].strip() if '(' in type_expr else type_expr

        prop_lines.append(f"  {prop_name}: {ts_type};")

    return f"type {title}Props = {{\n" + "\n".join(prop_lines) + "\n};"

# 生成结果
# props_definition = generate_props_definition(params.get("title",''), test_props)
# print(props_definition)
```

```python
def generate_types_docs(ts_code):
    # 清除前后空白
    ts_code = ts_code.strip()

    # 分割不同的export定义
    exports = re.split(r'\n\s*\n', ts_code)

    sections = []
    for export in exports:
        # 匹配interface或type定义
        match = re.match(
            r'export (interface|type|enum) (\w+)(\s*=\s*)?([\s\S]*)',
            export.strip()
        )
        if not match:
            continue

        type_kind = match.group(1)
        type_name = match.group(2)
        equals_sign = match.group(3) or ''
        type_body = match.group(4).strip()

        # 构建完整定义
        if type_kind == 'type':
            full_def = f"export type {type_name} = {type_body}"
        elif type_kind == 'enum':
            full_def = f"export enum {type_name} {type_body}"
        else:
            full_def = f"export interface {type_name} {type_body}"

        # 生成Markdown段落
        section = f"### {type_name}\n\n```ts\n{full_def}\n```"
        sections.append(section)

    return "\n\n".join(sections)

# 测试验证

# print(generate_types_docs(params.get("types",'')))
```

```python
def generate_defaults_docs(ts_code):
    # 匹配所有export的常量定义（带或不带类型注解）
    pattern = re.compile(
        r'export const (\w+)(?::\s*(\w+))?\s*=\s*(\{.*?\});',
        re.DOTALL
    )

    sections = []
    for match in re.finditer(pattern, ts_code):
        const_name = match.group(1)
        const_type = match.group(2)  # 可能为None
        const_value = match.group(3).strip()

        # 构造完整的导出语句（根据是否有类型注解）
        if const_type:
            full_definition = f"export const {const_name}: {const_type} = {const_value};"
        else:
            full_definition = f"export const {const_name} = {const_value};"

        # 生成Markdown段落
        section = f"### {const_name}\n\n```ts\n{full_definition}\n```"
        sections.append(section)

    return "\n\n".join(sections)

# 生成文档
# generate_props_definition(params.get("title",''), parse_props_string(props))
# type_definition = generate_types_docs(params.get("types",''))
# default_definiton = (generate_defaults_docs(params.get("defaults",'')))
# print(default_definiton)
```

```python
def generate_document(params):
    # 从relative_path提取text部分（新增路径处理逻辑）
    def extract_gitlab_text(path):
        # 查找components/后的路径部分
        components_index = path.find('components/')
        if components_index != -1:
            sub_path = path[components_index:]
            # 去除最后的文件名
            last_slash = sub_path.rfind('/')
            if last_slash != -1:
                return sub_path[:last_slash]
        return ""  # 默认值

    gitlab_text = extract_gitlab_text(relative_path)

    # 生成GitLab链接
    gitlab_base = "打码---------blob/master/"
    gitlab_link = gitlab_base + relative_path


    # 生成文件地址部分
    file_address = f"""<FileAddress
  filePath='{relative_path}'
  gitlab="""

    file_address +="{{\ntext:' "+gitlab_text + "',\nlink: '" + gitlab_link + "' \n}}/>"

    # 构建文档内容
    doc = f"""import {{ APITable }} from 'docSrc/components/table';
import {{ FileAddress }} from 'docSrc/components/file-address';
import {{ data, config }} from './apis/index.ts';

# {title}

！ {component_name}

！```tsx
！import {{ {title} }} from '--打码---';
！```
！
！{file_address}
！
！## 使用示例
！
！<demo
！  title="常规"
！  desc="{title}使用方法展示"
！  src="./packages/brand-biz/src/{gitlab_text}/__dockit__/demos/demo"
！/>
！
！## API
！
！### Props
！
！<APITable data={{data}} />
！
！### Config
！
！<APITable data={{config}} />
！
！## Types
！
！
！### {title}Props
！
！```tsx
！{props_definition}
！```
！
！{ type_definition}
！
！## Default
！
！{default_definiton}
！
！"""
！    return doc
```

```python
def generate_component_code(title):
    return f"""import {{ {title} }} from "打码"

const Demo = () => {{
  return (
    <FormContainer>
      <{title}.Component />
    </FormContainer>
  );
}};

export default Demo;"""

import re
from typing import Dict, Optional

def parse_params_to_data(props_definition: str, type_definition: str, title: str) -> str:
    # 提取所有desc不为undefined的映射关系
    desc_map = {
        'configKeyId': '配置的KeyId',
        'fieldPathMap': '组件字段映射配置',
        'validator': '组件校验配置',
        'config': '组件配置项',
        'showErrorConfig': '错误信息配置',
        'showErrConfigMini': '错误信息AuditInfoBanner的miniSize配置',
        'componentKey': 'KeyField属性配置',
        'imagesOptions': '图片素材配置',
        'uploadImageService': '上传图片服务',
        'uploadImage': '上传图片服务',
        'title': '标题',
        'hasSearchWords': '有搜索词',
        'videoDuration': '视频时长',
        'disabled': '是否禁用'
    }

    # 初始化变量
    data = []
    config = []

    # 解析props_definition (InteractCreativeProps)
    if props_definition:
        # 提取type定义内容
        props_match = re.search(r'type \w+Props\s*=\s*({.*?});', props_definition, re.DOTALL)
        if props_match:
            props_content = props_match.group(1)
            # 提取属性定义
            prop_items = re.finditer(r'(\w+)\s*:\s*([^;\n]+);', props_content)

            for match in prop_items:
                name = match.group(1)
                type_ = match.group(2).strip()
                type_ = type_.replace("InteractCreative", title)
                desc = desc_map.get(name, '')

                data.append({
                    'name': name,
                    'desc': desc,
                    'default': 'undefined',
                    'type': [type_]
                })

    # 解析type_definition (InteractCreativeConfig等)
    if type_definition:
        # 提取InteractCreativeConfig部分
        config_match = re.search(r'### \w+Config.*?```ts.*?export (?:interface|type) \w+Config (.*?```)', type_definition, re.DOTALL)
        if config_match:
            config_content = config_match.group(1)
            # 提取配置项定义
            config_items = re.finditer(r'(\w+)\??\s*:\s*([^;\n]+);', config_content)

            for match in config_items:
                name = match.group(1)
                type_ = match.group(2).strip()
                type_ = type_.replace("InteractCreative", title)
                desc = desc_map.get(name, '')

                config.append({
                    'name': name,
                    'desc': desc,
                    'default': 'undefined',
                    'type': [type_]
                })

    # 生成输出字符串
    output = f"export const data = {format_array(data)};\n\nexport const config = {format_array(config)};"
    return output

def format_array(arr: list) -> str:
    """格式化数组为JavaScript数组字符串"""
    if not arr:
        return "[]"

    items = []
    for item in arr:
        props = []
        for k, v in item.items():
            if isinstance(v, list):
                props.append(f"'{k}': {v}")
            elif isinstance(v, str):
                props.append(f"'{k}': '{v}'")
            else:
                props.append(f"'{k}': {v}")
        items.append("{\n    " + ",\n    ".join(props) + "\n  }")
    return "[\n  " + ",\n  ".join(items) + "\n]"
```


# 效果

```python
# 使用示例
params = {
    "title": 'SplashVideoFullScreen',
    "name": "视频全屏 组件",
    "relativePath": "(-------------打-----码----------------)",
    "props":"""
{
    fieldPathMap: definePropType<FieldPathMap>(Object),
    config: definePropType<SplashVideoFullScreenConfig>(Object),
    showErrorConfig: definePropType<any>(Object),
    validator: definePropType<any>(Object)
  }
""",
    "types": """
export type SplashVideoFullScreenConfig = {
      uploadService: UploadImageService;
      videoServices: VideoUploadService;
      imageTitle?: string;
      imageHint?: string;
      videoTitle?: string;
      videoHint?: string;
    }
""",
    "defaults": """
export const defaultFieldMap = {
  videoFullScreen: {
    path: 'materialComponent.videoFullScreen',
    key: 'VideoFullScreen',
    childPath: {
      imageInfoBkList: {
        path: 'imageInfoBkList'
      },
      videoList: {
        path: 'videoList'
      }
    }
  }
};

export const baseConfig = {
  imageTitle: '静态底图',
  imageHint: '要求：文件＜300K，支持PNG、JPG、JPEG格式，请分别上传以上分辨率图',
  videoTitle: '视频内容',
  videoHint: '要求：时长=5s，文件≤5M，支持MP4格式，请分别上传以上分辨率视频'
};
"""
}
```

```python
# 解析参数
title = params.get('title', '')
component_name = params.get('name', '')
relative_path = params.get('relativePath', '')
props = params.get('props', '')
types_config = params.get('types', '')
default_config = params.get('defaults', '')
props_definition = generate_props_definition(title, parse_props_string(props))
type_definition = generate_types_docs(types_config)
default_definiton = generate_defaults_docs(default_config)

print(generate_document(params))
```

```python
# 使用示例
title = params.get('title','')  # 可以动态传入不同的组件名
print(generate_component_code(title))
```

```python
# 使用示例
print(parse_params_to_data(props_definition, type_definition,params.get("title",'')))
```

![image.png](https://s2.loli.net/2025/04/12/RxG53rSQvIfNcZj.png)