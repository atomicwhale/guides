## Running a Rocket Pool node behind GFW (China)
*Chinese version is here: [中文版](https://github.com/atomicwhale/guides/blob/main/NodeBehindGFW-CN.md)*
This is a short and practical guide on how to run a Rocket Pool (RP) node behind the Great Firewall ([GFW](https://en.wikipedia.org/wiki/Great_Firewall)).  
  
Example validator ran during the test: https://holesky.beaconcha.in/validator/1566743  
(Please ignore the periods when it missed attestations due to me testing different clients, forgetting to start the service or my account balance running low...)  

\* *This guide is based on standard Docker mode.*  
\**  *This guide is written based on Holešky testnet.*  
\*** *This instructions were tested on Debian 12 on a Aliyun virtual machine behind the GFW (Feb/2024). YMMV depending on internet connection and how hard GFW wants to f\*k you over.*

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

