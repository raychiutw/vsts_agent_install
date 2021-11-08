## 安裝前置套件

### install 前置套件

```bash
sudo apt-get update
sudo apt-get install wget gss-ntlmssp rpm
```

### 安裝 .Net Core SDK

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
sudo yum install dotnet-sdk-5.0
```

### 安裝 docker

[請參考 Docker 安裝文件](https://docs.docker.com/engine/install/ubuntu/)

## Agent 安裝

### 下載並安裝Agent

> Agent 需在以上套件都裝完後才安裝

```bash
# 下載 Agent

cd /data
sudo wget https://vstsagentpackage.azureedge.net/agent/2.193.1/vsts-agent-linux-x64-2.193.1.tar.gz
sudo mkdir myagent && cd myagent
sudo tar xzf ../vsts-agent-linux-x64-2.193.1.tar.gz
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

> 安裝參數

- `--pool` : 集區名稱
- `--agent` : Agent 名稱
- `--url` : TFS URL
- `--userName` : 登入帳號
- `--password` : 登入密碼

```bash
# 安裝
sudo ./config.sh --pool 'EPK-SIT' --agent 'DEV-BUD-S04' --work '_work' --url 'http://dev-tfs-p01.fetcp.net.tw:8080/tfs/' --auth negotiate --userName 'fetcs\tfsagent' --password 'P@ssw0rd'
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
sudo ./config.sh remove --auth negotiate --userName --userName 'fetcs\tfsagent' --password 'P@ssw0rd'
```

### 更新環境參數

```bash
./env.sh
sudo ./svc.sh stop
sudo ./svc.sh start
```
