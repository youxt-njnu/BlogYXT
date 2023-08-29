---
title: 基于geopandas绘制地图及图面整饰
date: 2023-05-10 15:49:36
tags:
 - geopandas
 - 绘图细节
 - 毕业论文
categories:
 - 科研绘图
---

# 前言

输入数据：

* 经QGIS/ArcGIS处理的路网矢量线数据`road21Final.shp`
* 城市底图矢量面数据

实现目标：

* 将数据处理从osmnx库需要的格式
* 利用osmnx库进行基本的指标求解
* 保存包含了指标信息的数据
* 地图可视化

相关知识点：

* momepy
* osmnx
* geopandas

# 绘图准备

导入包

```python
import pandas as pd
import geopandas as gpd
import numpy as np 
import matplotlib.pyplot as plt
import shapely
import momepy
import osmnx as ox
```

设置中文显示

```python
plt.rcParams['font.sans-serif'] = ['SimHei']  # 指定默认字体
plt.rcParams['axes.unicode_minus'] = False  # 解决保存图像是负号'-'显示为方块的问题
```

读取数据

```python
s21="basic/road21Final.shp"
streets=gpd.read_file(s21)
```

![](https://s2.loli.net/2023/08/28/wRPloUCSOV87a5F.png)



# 数据处理

## 几个痛点

> 将LINESTRING Z 转换为LINESTRING：`shapely.ops.transform`
> 
> 将数据结构调整为osm路网结构数据，方便后续调用函数进行指标求解
> 
> 数据索引的设置

## 处理流程

集成数据处理函数

```python
# streets: geopandas直接读取的矢量路网数据
# uniText：
def RoadWork(streets,uniText):
    for i in range(len(streets)):
        streets.geometry[i]=shapely.ops.transform(lambda x, y, z=None: (x, y), streets.geometry[i])  # 'LINESTRING (2 3, 4 5)'
    graph = momepy.gdf_to_nx(streets)
    nodes, edges = momepy.nx_to_gdf(graph)
    # 查看结构
    # node.head()
    # edges.head()

    # 构建node结构
    nodes["osmid"]=nodes["nodeID"]
    nodes["x"]=nodes.geometry.x
    nodes["y"]=nodes.geometry.y
    nodes=nodes[['osmid','y','x','nodeID','geometry']]
    # 构建节点的street_count列
    t1=pd.DataFrame(edges['node_start'].value_counts())
    t2=pd.DataFrame(edges['node_end'].value_counts())
    t1.columns=['street_count']
    t2.columns=['street_count']
    tt=t1.add(t2,fill_value=0).astype(int)
    # 设置索引
    nodes.set_index(['osmid'],drop=True,inplace=True)
    nodes=pd.merge(nodes,tt,how='left',left_index=True,right_index=True)

    # 构建edges结构
    edges["u"]=edges["node_start"]
    edges["v"]=edges["node_end"]
    edges["osmid"]=edges["ID"]
    edges["key"]=0
    edges['length']=edges['mm_len']
    edges=edges[['u','v','key','osmid',uniText,'type','name','lengthQGIS','ID','geometry','mm_len','length','node_start','node_end']]
    edges.set_index(['u','v','key'],drop=True,inplace=True) # 重要：设置索引
    if(len(edges.loc[edges['node_start']>edges['node_end']])>0): # 保证edges唯一
        return
    return nodes,edges
```

数据处理

```python

```

处理结果

`nodes`

![](https://s2.loli.net/2023/08/28/qOoIRlJhSrbN4vB.png)

`edges`

![](https://s2.loli.net/2023/08/28/7wZCETQM3XP9jmp.png)

绘图验证

```python
G = ox.graph_from_gdfs(nodes, edges)
fig, ax = ox.plot_graph(G)
```

![](https://s2.loli.net/2023/08/28/53yIqH6PF9krhfX.png)

```python
G.graph
```

> {'crs': <Projected CRS: EPSG:32650>
>  Name: WGS 84 / UTM zone 50N
>  Axis Info [cartesian]:
> 
> - E[east]: Easting (metre)
> - N[north]: Northing (metre)
>   Area of Use:
> - name: Between 114°E and 120°E, northern hemisphere between equator and 84°N, onshore and offshore. Brunei. China. Hong Kong. Indonesia. Malaysia - East Malaysia - Sarawak. Mongolia. Philippines. Russian Federation. Taiwan.
> - bounds: (114.0, 0.0, 120.0, 84.0)
>   Coordinate Operation:
> - name: UTM zone 50N
> - method: Transverse Mercator
>   Datum: World Geodetic System 1984
> - Ellipsoid: WGS 84
> - Prime Meridian: Greenwich}

# 指标求解

osmnx中的基本指标

```python
# nary_union包含所有地理实体的集合体，convex_hull求凸包，来自于geopandas
area_m=nodes.unary_union.convex_hull.area
ox.basic_stats(G,area=area_m,clean_int_tol=15)
```

> 'n': 1929,节点的数量  
>  'm': 4945, 边的数量  
>  'k_avg': 5.127008812856403, 平均节点度（包括了入度和出度）
>  'intersection_count': 1706, 交叉点数量  
>  'streets_per_node_avg': 3.0031104199066876,无向图中，每个节点连接的街道数目（边）的平均值  
>  'streets_per_node_counts': {0: 0, 1: 223, 2: 12, 3: 1246, 4: 432, 5: 16},无向图中，连接到一个节点的街道数目：这个数字下的街道数目一共有多少  
>  'streets_per_node_proportion': {0: 0.0,
>   1: 0.11560393986521514,
>   2: 0.006220839813374806,
>   3: 0.6459305339554173,
>   4: 0.223950233281493,
>   5: 0.00829445308449974},无向图中，对应的改成了占比  
>  'edge_length_total': 774358.6850000017,米制的所有边长度的总和  
>  'edge_length_avg': 156.59427401415604,上面的均值  
>  'street_length_total': 486759.9220000005,在无向图中，米制的所有边长度的总和  
>  'street_length_avg': 171.636079689704,上面的均值  
>  'street_segments_count': 2836,无向图中，边的数量  
>  'node_density_km': 35.29736508882025,平方千米下的，面积除以n  
>  'intersection_density_km': 31.21685061769173,平方千米下的，面积除以intersection_count  
>  'edge_density_km': 14169.425201733446,km下的，edge_length_total divided by area  
>  'street_density_km': 8906.86504792104,km下的，street_length_total divided by area  
>  'circuity_avg': 2.0995288875627176e-05,每个边的节点之间最大圆环距离，除以，边的总长度  
>  'self_loop_proportion': 0.0008088978766430738,表示proportion of edges that have a single node as its endpoints (ie, the edge links nodes u and v, and u==v)  
>  'clean_intersection_count': 8906.86504792104,表示number of intersections in street network, merging complex ones into single points  
>  'clean_intersection_density_km': 23.29369920065743,km下的，clean_intersection_count divided by area

```python
# 格式优化
# unpack dicts into individiual keys:values
stats = ox.basic_stats(G, area=area_m,clean_int_tol=15)
for k, count in stats["streets_per_node_counts"].items():
    stats["{}way_int_count".format(k)] = count
for k, proportion in stats["streets_per_node_proportions"].items():
    stats["{}way_int_prop".format(k)] = proportion

# delete the no longer needed dict elements
del stats["streets_per_node_counts"]
del stats["streets_per_node_proportions"]

# load as a pandas dataframe
stats2021=pd.DataFrame(pd.Series(stats, name="value")).round(3)
```

# 数据保存

```python
import os
path = "./"
filep_nodes = os.path.join(path, "nodes.shp")
filep_edges = os.path.join(path, "edges.shp")


def stringify_nonnumeric_cols(gdf):  # 转换数据类型
    for col in (c for c in gdf.columns if not c == "geometry"):  # 除 geometry 外的所有列
        if not pd.api.types.is_numeric_dtype(gdf[col]):  # 如果不是数字类型
            gdf[col] = gdf[col].fillna("").astype(str)  # 全部转换为字符串
    return gdf


gdf_nodes, gdf_edges = ox.graph_to_gdfs(G)
gdf_nodes = stringify_nonnumeric_cols(gdf_nodes)
gdf_edges = stringify_nonnumeric_cols(gdf_edges)

gdf_edges["fid"] = np.arange(0, gdf_edges.shape[0], dtype='int')  # 增加 fid 列
gdf_nodes.to_file(filep_nodes, encoding="utf-8")
gdf_edges.to_file(filep_edges, encoding="utf-8")
```

# 地图可视化

# 细节调整
