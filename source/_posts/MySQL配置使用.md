---
title: MySQL配置使用
date: 2023-09-01 09:32:18
photos: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
tags: 
  - MySQL
  - 软件配置
categories:
 - 开发工具
comments: true
cover: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
---

# 安装配置

[MySQL安装配置教程](https://blog.csdn.net/SoloVersion/article/details/123760428)

[完美解决MySQL ERROR:Access denied for user `root`@`localhost` (using password:YES)_xl132598798的博客-CSDN博客](https://blog.csdn.net/xl132598798/article/details/106342240)

[Can‘t connect to MySQL server on ‘localhost:3306‘ (10061)_Where-to-go的博客-CSDN博客](https://blog.csdn.net/weixin_45523183/article/details/116358192)

[MySql错误 1251 - Client does not support authentication protocol requested by server 解决方案_1251 client does not support authentication_阿燃i的博客-CSDN博客](https://blog.csdn.net/OCEAN_C/article/details/89719578)

[Can‘t connect to MySQL server on ‘localhost:3306‘ (10061)_Where-to-go的博客-CSDN博客](https://blog.csdn.net/weixin_45523183/article/details/116358192)

# 联动

## unity

[Unity连接MySql，The given key was not present in the dictionary 错误原因及解决办法_ 木头的博客-CSDN博客](https://blog.csdn.net/pstj123456/article/details/105999672)



## 前端mysql模块

标记删除

> 模拟删除的动作，在表中设置类似于status的状态字段，来标记当前数据就是否被删除；
> 
> 相对于DELETE语句会真正的把数据从表中删掉，更加保险；
> 
> 当用户执行标记删除的时候，我们是执行了UPDATE语句，将数据的status字段标记为删除。

```js
db.query('UPDATE USERS SET status=1 WHERE id=?', 6, (err,results)=> {
    if (err) return console.log(err.message)
    if(results.affectedRows === 1) { console.log('删除数据成功')}
})
```
