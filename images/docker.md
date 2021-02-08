## docker 安装nacos

1.cmd调出终端，查看已安装服务
``` 
docker images
```

2.查看nacos镜像列表
``` 
docker search nacos
```
3.下载nacos
``` 
search pull 镜像名称
```
4.启动nacos
```
docker run --env MODE=standalone --name nacos -d -p 8848:8848 nacos/nacos-server
```