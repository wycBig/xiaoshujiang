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
## docker 安装mysql、redis
1.cmd调出终端，查看已安装服务
``` 
docker images
```

2.查看mysql、redis镜像列表
``` 
docker search nacos/redis
```
3.下载nacos
``` 
search pull 镜像名称
```
4.启动nacos
```
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
docker run -itd --name redis-test -p 6379:6379 redis
```