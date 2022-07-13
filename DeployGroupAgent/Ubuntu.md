# TFS 部屬集區 agent 安裝步驟

### on Ubuntu

### install 前置套件

```bash
sudo apt-get update
sudo apt-get install wget gss-ntlmssp
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

### 安裝 .Net Core SDK (如果需要)

```bash
wget https://packages.microsoft.com/config/ubuntu/21.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-5.0
```

### 安裝 Docker

> 如果是 Docker 主機 請先安裝 Docker

[Docker 安裝說明](https://github.evertrust.com.tw/EA/docker_install)

## Agent 安裝

### 下載並安裝Agent

> Agent 需在以上套件都裝完後才安裝

```bash
# 下載 Agent

cd /data
sudo wget https://vstsagentpackage.azureedge.net/agent/2.193.1/vsts-agent-linux-x64-2.206.1.tar.gz
sudo mkdir myagent && cd myagent
sudo tar xzf ../vsts-agent-linux-x64-2.206.1.tar.gz
```

> 安裝必要套件

```bash
./bin/installdependencies.sh
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

```
sudo ./config.sh --deploymentpool --deploymentpoolname "{部署集區名稱}" --acceptteeeula --agent $HOSTNAME --url 'http://{tfs url}' --work _work --auth Negotiate --runasservice  --userName '{account}' --password '{password}';

sudo ./svc.sh install
sudo ./svc.sh start
```

#### 移除 agent

```bash
sudo ./svc.sh stop
sudo ./svc.sh uninstall
sudo ./config.sh remove --auth negotiate --userName '{account}' --password '{password}'
```

#### 更新環境參數

```bash
./env.sh
sudo ./svc.sh stop
sudo ./svc.sh start
```
