# 02-SpringBoot-ES-Docker

> 目录:[https://github.com/dolyw/Elasticsearch](https://github.com/dolyw/Elasticsearch)

#### 软件架构

1. `SpringBoot` + `REST Client`

#### 项目介绍

`SpringBoot`整合ES的方式(`TransportClient`、`Data-ES`、`Elasticsearch SQL`、`REST Client`)

详细的过程查看: [https://github.com/dolyw/Elasticsearch/tree/master/01-SpringBoot-ES-Local](https://github.com/dolyw/Elasticsearch/tree/master/01-SpringBoot-ES-Local)

这个项目只是测试Docker版本的`Elasticsearch`是否安装无误

#### 预览图示

* 查询

![查询](https://docs.dolyw.com/Project/Elasticsearch/image/20190815001.gif)

* 添加

![添加](https://docs.dolyw.com/Project/Elasticsearch/image/20190815002.gif)

* 修改

![修改](https://docs.dolyw.com/Project/Elasticsearch/image/20190815003.gif)

* 删除

![删除](https://docs.dolyw.com/Project/Elasticsearch/image/20190815004.gif)

#### 安装教程

```
运行项目src\main\java\com\example\Application.java即可，访问http://localhost:8080即可
```

#### 搭建参考

1. 感谢MaxZing的在SpringBoot整合Elasticsearch的Java Rest Client:[https://www.jianshu.com/p/0b4f5e41405e](https://www.jianshu.com/p/0b4f5e41405e)