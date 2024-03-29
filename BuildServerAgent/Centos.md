## 安裝前置套件

### install wget


```bash
sudo yum update
sudo yum install wget
```


修改 wget proxy

```bah
sudo vim /etc/wgetrc
```

```bash
https_proxy = http://proxydomain:port/
http_proxy = http://proxydomain:port/

use_proxy = on
```


### 安裝 lib

```bash
sudo yum install libunwind libicu
```

### 安裝 .Net Core SDK

```bash
sudo rpm --httpproxy proxydomain.com.tw:port -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
sudo yum install dotnet-sdk-6.0
```

[.NET Core on CentOS 7](https://docs.microsoft.com/zh-tw/dotnet/core/install/linux-centos#centos-7-)

### 重新安裝新版 git (Agent 需 git 2.9.0 以上)

[請參考 Git 安裝文件](/EA/git_install/src/branch/master/README.md)

### 安裝 docker

[請參考 Docker 安裝文件](/EA/docker_install)

## Agent 安裝

### 安裝前置套件

```bash
yum install gssntlmssp
```

```bash
./bin/installdependencies.sh
```

### 下載並安裝Agent

> Agent 需在以上套件都裝完後才安裝

```bash
# 下載 Agent

cd /data
sudo wget https://vstsagentpackage.azureedge.net/agent/2.206.1/vsts-agent-linux-x64-2.193.1.tar.gz
sudo mkdir myagent && cd myagent
sudo tar xzf ../vsts-agent-linux-x64-2.206.1.tar.gz
```

> 預設禁止使用 root 安裝, 修改安裝檔

```bash
# 移除 config.sh 判斷
sudo vim ./config.sh
```

```bash
# we want to snapshot the environment of the config user
#if [ $user_id -eq 0 -a -z "$AGENT_ALLOW_RUNASROOT" ]; then
#    echo "Must not run with sudo"
#    exit 1
#fi
```

> Agent 名稱建議為 : agent.{主機名稱} 

> ex: agent.srvbuildtfs7

```bash
# 安裝
sudo ./config.sh --pool '{pool name}' --agent '{agent name}' --work '_work' --url '{tfs url}' --auth negotiate --userName '{account}' --password '{password}'
```

### 啟動服務

```bash
sudo ./svc.sh install
sudo ./svc.sh start
```
    
### 移除步驟
 
```bash
sudo ./svc.sh stop
sudo ./svc.sh uninstall
sudo ./config.sh remove --auth negotiate --userName '{account}' --password '{password}'
```

### 更新環境參數

```bash
./env.sh
sudo ./svc.sh stop
sudo ./svc.sh start
```
