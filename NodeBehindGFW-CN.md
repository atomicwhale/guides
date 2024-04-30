## 如何在墙内安装运行Rocket Pool节点

# Work in progress. Please check back later.  

这是一篇简短的实用指南，简述如何在防火长城（GFW）后面运行一个以太坊节点火箭池（Rocket pool）节点。

这是作者在测试期间运行的验证器Validator：https://holesky.beaconcha.in/validator/1566743 （请忽略其中由于测试不同的客户端、服务停机或账户余额不足暂停的几段时间…）  

\* *这个指南是基于Rocket Pool标准的 Docker 模式。*  
** *这个指南是基于 Holešky 测试网编写的。*  
*** *本指南经在阿里云虚拟机上的 Debian 12 上测试。该虚拟机位于 GFW 后面（2024 年 2 月）。根据你的网络状况和 GFW 的抽风程度，你遇到的问题可能会有所不同。* (以下正在翻译中) 
---
### Main challenges
* (Manual) Installation of Rocket Pool smartnode
* Github is blocked by GFW
* Docker Hub is slow/or blocked by GFW

---
### 0. Preparation
Please read the excellent official guide by Rocket Pool: [Link](https://docs.rocketpool.net/guides/node/responsibilities)

---
### 1. Manually install Smartnode
Follow the RP guide to secure the node.  
When it comes to setup the node, use a github mirror (kkgithub) to download the smartnode (cli):  
`wget https://kkgithub.com/rocket-pool/smartnode-install/releases/latest/download/rocketpool-cli-linux-amd64 -O ~/bin/rocketpool`  
  
Manually download the latest installation script:  
`wget https://kkgithub.com/rocket-pool/smartnode-install/releases/latest/download/install.sh`  
  
Modify the script:  
`nano install.sh`
  
Replace the github URL with a mirror URL (This is on line 117 and 119 on v1.11.7 at the time of writing this guide).  
Find the two `PACKAGE_URL=...`, and modify them so they look like this:  
`   PACKAGE_URL="https://kkgithub.com/...`
  
Save the script and exit (`Ctrl+O` and `Ctrl+X`).  
  
Manually run the installation script:  
`sudo bash install.sh`  
*(The error `stat: cannot statx...` in the end can be ignored.)*
  
\*  *__kgithub/kkgithub__ is a github mirror/proxy [GitHub page](https://github.com/kgithub666/kgithub). Please donate to keep it running, see [About page](https://help.kkgithub.com/donate/).*

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
