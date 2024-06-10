
# 离线安装docker
[离线安装手册](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package)
## 删除原有docker
运行以下命令以卸载所有冲突的包：
```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
## 下载所需文件
下载以下内容 debDocker 引擎、CLI、containerd 的文件、 和 Docker Compose 包：
containerd.io_<version>_<arch>.deb
docker-ce_<version>_<arch>.deb
docker-ce-cli_<version>_<arch>.deb
docker-buildx-plugin_<version>_<arch>.deb
docker-compose-plugin_<version>_<arch>.deb
## 安装
我使用了filezilla传递文件到云服务器
```shell
# 更换为自己下载的版本（进入自己文件夹，containerd.io使用较新版本）
sudo dpkg -i ./containerd.io_1.6.32-1_amd64.deb \
  ./docker-buildx-plugin_0.10.2-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-ce_23.0.0-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-ce-cli_23.0.0-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-compose-plugin_2.15.1-1~ubuntu.22.04~jammy_amd64.deb
```
## 启动
```shell
sudo service docker start
sudo docker run hello-world
```
# 安装v2ray
[v2ray安装及使用教程](https://www.v2ray.com/chapter_00/install.html)



todo
