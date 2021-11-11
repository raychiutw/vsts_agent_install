# TFS Build Server agent 安裝步驟

> Windows 請用管理者身分開啟 PowerShell 命令列視窗執行

### on Windows

前置安裝
1. Net 5 SDK
2. Net Framework 4.8
3. [Visual Studio build tools](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=160)

下載 agent

https://vstsagentpackage.azureedge.net/agent/2.193.1/vsts-agent-win-x64-2.193.1.zip

解壓縮到 C:\vstsagent\


設定

修改 agent name `DEV-BUD-U01`

```powershell
.\config.cmd --pool 'EPK-UAT' --agent 'DEV-BUD-U01' --runasservice --work '_work' --url 'http://dev-tfs-p01.fetcp.net.tw:8080/tfs/' --auth negotiate --userName 'fetcu\tfsagent' --password 'P@ssw0rd'  --windowsLogonAccount 'fetcu\tfsagent' --windowsLogonPassword 'P@ssw0rd'
```

移除 agent

```powershell
.\config.cmd remove --auth negotiate --userName 'fetcu\tfsagent' --password 'P@ssw0rd'
```
