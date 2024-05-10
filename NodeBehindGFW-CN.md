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
### 2. Configure mirrors for Docker Hub
Go through the smartnode configuration first  
`rocketpool s c`  
  
Save and exiting the config, for a quick trial, you can let the smartnode start the service, or manually start it:   
`rockerpool s s`  
If it runs and you can see all the sevices started sucessfully, you can skip the rest of the guide and open your \<insert drink of choice\> <s>beer/horse piss</s> and celebrate.  
If it fails, it is most likely becasue Docker Hub (hub.docker.com) is blocked by GFW.  
  
You can get around by using a docker hub mirror:  
`sudo nano /etc/docker/daemon.json`  

Add the following mirrors to docker configuration file:
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

Restart docker:
`sudo systemctl restart docker`  

Try to start the service again `rockerpool s s` and hopefully it should run this time.  
  
---
--by atomicwhale.eth
