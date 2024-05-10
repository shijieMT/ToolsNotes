
# Windows 安装与使用 Docker
## 安装Docker
### 安装WSL
> 在微软商店中搜索ubuntu，安装  
> 确保电脑开启虚拟化  
> 控制面板->程序->启动或关闭windows功能->勾选“适用于Linux的Windows”->重启电脑  
> 打开ubuntu，等待安装结束
### 安装Docker
> [!TIP]
> Docker环境正常运行的前提是WSL2子系统正在运行，若处于Stop状态，Docker检测不到可用WSL2，会提示需要安装WSL2  
[知乎Docker安装](https://zhuanlan.zhihu.com/p/441965046)
### 配置镜像（registry-mirrors）
```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://cr.console.aliyun.com",
    "https://mirror.ccs.tencentyun.com"
   ]
}
```
