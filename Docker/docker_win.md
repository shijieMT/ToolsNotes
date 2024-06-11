
# Windows 安装与使用 Docker
## 安装Docker
### 安装WSL
[win10家庭版命令行开启虚拟化](https://www.cnblogs.com/Attempts-blog/p/14720757.html)  
[Windows开启 CPU 虚拟化 + 关闭 Hyper-V](https://blog.csdn.net/u014727709/article/details/126538711)  
以管理员身份打开终端
查看虚拟化开启状态命令：systeminfo  
开启：bcdedit /set hypervisorlaunchtype auto 然后重启计算机  
安装Ubuntu：  
> wsl --set-default-version 2  
> 在微软商店中搜索ubuntu，安装  
> 确保电脑开启虚拟化  
> 控制面板->程序->启动或关闭windows功能->勾选“适用于Linux的Windows”->重启电脑  
> 打开ubuntu，等待安装结束
### 安装Docker
> [!TIP]
> Docker环境正常运行的前提是WSL2子系统正在运行，若处于Stop状态，Docker检测不到可用WSL2，会提示需要安装WSL2  
### 配置镜像（registry-mirrors）
> Settings -> Docker Engine
```json
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://docker.chenby.cn"
  ]
}
```
