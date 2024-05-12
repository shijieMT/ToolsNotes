## 安装mysql
todo
## 配置远程连接（新版本 MySQL 8.0+）
### 获取本地ip
```shell
ipconfig
```
> 我的IP 10.111.4.43
### 运行telnet命令
> 如果未识别命令 运行 appwiz.cpl -> 启动或关闭windows功能 -> Telnet客户端
```shell
telnet 10.111.4.43 3306
```
观察是否有mysql响应
> 如果没有正确响应，可能是未开放3306端口，在windows防火墙入站规则中设置
响应成功继续连接数据库
### 本地连接数据库
```shell
mysql [-h 127.0.0.1] [-P 3306] -u root  -p
```
### 授权
```mysql
USE mysql;

CREATE USER 'weipulei'@'%' IDENTIFIED BY '135789';

GRANT ALL PRIVILEGES ON database_name.* TO 'shijie'@'%';

FLUSH PRIVILEGES;
```
建需要的数据库
### 远程连接数据库
```shell
mysql -h 10.111.4.43 -P 3306 -u weipulei  -p
```
