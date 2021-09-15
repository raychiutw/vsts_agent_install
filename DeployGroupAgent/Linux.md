# TFS 部屬集區 agent 安裝步驟

### on Linux

#### 前置安裝

```bash
sudo yum update
sudo yum install wget
```

修改 wget proxy

```bash
sudo vim /etc/wgetrc
https_proxy = http://secproxy.evertrust.com.tw:6588/
http_proxy = http://secproxy.evertrust.com.tw:6588/

use_proxy = on
```

> 如果是 Docker 主機 請先安裝 Docker

[Docker 安裝說明](https://github.evertrust.com.tw/EA/docker_install)

#### 安裝 lib

```bash
sudo wget -P /etc/pki/rpm-gpg https://archive.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
sudo yum install libunwind libicu gssntlmssp

```

#### 安裝 agent

請記得修改`deploymentpoolname`

```bash
cd /data
sudo wget https://vstsagentpackage.azureedge.net/agent/2.181.0/vsts-agent-linux-x64-2.181.0.tar.gz
sudo mkdir vstsagent
cd vstsagent
sudo tar xzf ../vsts-agent-linux-x64-2.181.0.tar.gz

# 如果是用 root 安裝, 請先註解程式判斷

sudo vim config.sh

#### 註解 sudo 判斷
# we want to snapshot the environment of the config user
#if [ $user_id -eq 0 ]; then
#    echo "Must not run with sudo"
#    exit 1
#fi

sudo ./config.sh --deploymentpool --deploymentpoolname "{集區名稱}" --acceptteeeula --agent $HOSTNAME --url http://{tfs domain}:8080/tfs/ --work _work --auth Negotiate --runasservice  --userName '{account}' --password '{password}';

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


#### 錯誤排除

> GSSAPI operation failed with error - An unsupported mechanism was requested. NTLM authentication requires the GSSAPI plugin 'gss-ntlmssp'.
Failed to connect.

> 缺少 gssntlmssp

解法

```bash
yum install gssntlmssp
```


> Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7


这是因为在你的 /etc/yum.repos.d 目录下有关于yum repository的配置文件中列有如下的GPG key：

```
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
```

这个配置告诉YUM，这个repository的GPG key存在于磁盘上。而当YUM在路径 /etc/pki/rpm-gpg 下找不到这个GPG key的时候，就会报如上的错误了。

解決方法

```bash
cd /etc/pki/rpm-gpg
wget https://archive.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
```
