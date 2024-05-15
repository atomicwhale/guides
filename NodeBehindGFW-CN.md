## 如何在墙内安装运行Rocket Pool节点

# Work in progress. Please check back later.  

这是一篇简短的实用指南，简述如何在防火长城（GFW）后面运行一个以太坊节点火箭池（Rocket pool）节点。

这是作者在测试期间运行的验证器Validator：https://holesky.beaconcha.in/validator/1566743 （请忽略其中由于测试不同的客户端、服务停机或账户余额不足暂停的几段时间…）  

\* *这个指南是基于Rocket Pool标准的 Docker 模式。*  
** *这个指南是基于 Holešky 测试网编写的。*  
*** *本指南经在阿里云虚拟机上的 Debian 12 上测试。该虚拟机位于 GFW 后面（2024 年 2 月）。根据你的网络状况和 GFW 的抽风程度，你遇到的问题可能会有所不同。* (以下正在翻译中) 
---
### 需解决的问题
* (手动) 安装 Rocket Pool smartnode
* Github 连不上 （被墙）
* Docker Hub 连不上/被墙

---
### 0. 准备工作
阅读并熟悉官方说明文档和安装过程 Rocket Pool: [Link](https://docs.rocketpool.net/guides/node/responsibilities)

---
### 1. 手动安装 Smartnode
首先确保你的节点服务器已经做好了安全防护措施，参考RP官方文档：Secure the node，或网上现有教程。  
第一步，使用一个Github镜像(kkgithub)来下载RP smartnode (cli):  
`wget https://kkgithub.com/rocket-pool/smartnode-install/releases/latest/download/rocketpool-cli-linux-amd64 -O ~/bin/rocketpool`  
  
手动下载安装脚本:  
`wget https://kkgithub.com/rocket-pool/smartnode-install/releases/latest/download/install.sh`  
  
手动更改安装脚本:  
`nano install.sh`
  
用镜像网址替换其中Github网址(此教程编写时， RP smartnode v1.11.7，Github网址位于安装脚本117-119行).  
找到其中以`PACKAGE_URL=...`起头的两行, 替换掉网址后这两行应该是这样的:  
`   PACKAGE_URL="https://kkgithub.com/...`
  
保存脚本并退出 (`Ctrl+O` 然后 `Ctrl+X`).  
  
手动运行更改后的安装脚本:  
`sudo bash install.sh`  
*(安装最后如果报错 `stat: cannot statx...`，这个不影响安装，可以忽略。)*
  
\*  *__kgithub/kkgithub__ 是一个github 镜像/代理 [GitHub网址](https://github.com/kgithub666/kgithub). 如果觉得它有用，可以考虑支持一下项目方/作者 [About page](https://help.kkgithub.com/donate/).*

---
### 2. 配置Docker Hub镜像
首先请完成Rocketpool Smartnode的初始设置  
`rocketpool s c`  
  
保存设置并退出
在这里你可以先试着运行一下smartnode，可以选择自动启动Smartnode，或者手动启动smartnode服务:   
`rockerpool s s`  
如果一切运行顺利，服务启动成功，那恭喜你，Rocketpool Smartnode已经可以正常运行，你不需要教程以下的内容了。  
如果启动失败，或者下载进度停滞不前，那大概率是因为Docker Hub (hub.docker.com)被墙.  
  
这时你需要配置一个docker hub镜像。编辑Docker设置文件:  
`sudo nano /etc/docker/daemon.json`  

讲以下镜像加入到配置文件中:
```
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn"
    ]
}
```

重启Docker服务:
`sudo systemctl restart docker`  

这是再次试着启动Rocketpool Smartnode
`rockerpool s s`
and hopefully it should run this time.  
  
---
--by atomicwhale.eth
