

### 拉取镜像并运行
```shell
# 拉取镜像
docker pull mysql:5.7
# 运行
docker run -itd -p 3307:3306 --name=mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
# 拉取镜像
docker pull nginx:latest
# 运行
docker run -itd -p 81:80 --name=ng nginx:latest
```
> -p 3307:3306：将容器的 3306 端口映射到宿主机的 3307 端口。  
> -e MYSQL_ROOT_PASSWORD=123456： 初始化 root 用户的密码。

### 列出本地镜像、列出正在运行的容器
```shell
# 列出正在运行的容器
docker ps

# 列出本地镜像
docker images
```
### start and stop
```shell
# 这里使用的名字“ng”是自己run时添加的name选项“--name=ng”
docker stop ng
# 运行之后可用docker ps查看，运行的Nginx已经关闭
docker start ng
# 运行之后可用docker ps查看，运行的Nginx已经开启，不需要重新run
```





