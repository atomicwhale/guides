## Running a Rocket Pool node behind GFW (China)
This is a short and practical guide on how to run a Rocket Pool (RP) node behind the Great Firewall ([GFW](https://en.wikipedia.org/wiki/Great_Firewall)).  
  
Example validator ran during the test: https://holesky.beaconcha.in/validator/1566743  
(Please ignore the first ~520 missed attestations due to me forgetting to start the service.)  

\* *This guide is based on standard Docker mode.*  
\**  *This guide is written based on Holešky testnet.*  
\*** *This instructions were tested on Debian 12 on a Aliyun Virtual Machine behind the GFW (Feb/2024). YMMV depending on internet connection and how hard GFW wants to f\*k you over.*

---
### Main challenges
* (Manual) Installation of Rocket Pool smartnode
* Github is blocked by GFW
* Docker Hub is block by GFW

---
### 0. Preparation
Please read the excellent official guide by Rocket Pool: [Link](https://docs.rocketpool.net/guides/node/responsibilities)

---
### 1. Manually install Smartnode
Follow the RP guide and use a github mirror to download the smartnode (cli)  
`wget https://kkgithub.com/rocket-pool/smartnode-install/releases/latest/download/rocketpool-cli-linux-amd64 -O ~/bin/rocketpool`  
  
Manually download the latest installation script  
`wget https://github.com/rocket-pool/smartnode-install/releases/latest/download/install.sh`  
  
Modify the script  
`nano install.sh`
  
Replace the github URL with a mirror (This is on line 117 and 119 at the time of writing this guide).  
Find the `PACKAGE_URL=...` and add `kk` before github.com. The section should look like this:  
`   PACKAGE_URL="https://kkgithub.com/...`
  
Save the script (`Ctrl+O`) and exit (`Ctrl+X`)  
  
Manually run the installation script  
`sudo bash  install.sh`  
*(The error `stat: cannot statx...` in the end can be ignored.)*
  
\*  *kgithub/kkgithub is a github mirror/proxy [GitHub page](https://github.com/kgithub666/kgithub). Please donate to keep it running, see [About page](https://help.kkgithub.com/donate/).*

---
### 2. Configure mirrors for Docker Hub
Go through the smartnode configuration  
`rocketpool s c`  
  
At this stage, you can try to start the service by  
`rockerpool s s`  
If it runs sucessfully you can open your \<insert drink of choice\> <s>beer/horse piss</s> and celebrate now.  
If it fails, most probably it is becasue Docker Hub (hub.docker.com) is blocked by GFW.  
  
You can get around using a docker hub mirror.  
`sudo nano /etc/docker/daemon.json`  

Add the following mirrors to docker configuration
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

Restart docker
`sudo systemctl restart docker`  

Try to start the service again `rockerpool s s` and it should work this time.  
  
---
--by atomicwhale.eth

