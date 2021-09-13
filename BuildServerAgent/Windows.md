# TFS Build Server agent 安裝步驟

> Windows 請用管理者身分開啟 PowerShell 命令列視窗執行

### on Windows

下載 agent

https://vstsagentpackage.azureedge.net/agent/2.191.1/vsts-agent-win-x64-2.191.1.zip

解壓縮到 C:\vstsagent\


設定

修改 agent name `Agent5-SRVBUILDTFS1`

```powershell
.\config.cmd --pool 'Production' --agent 'Agent5-SRVBUILDTFS1' --runasservice --work '_work' --url 'http://tfs.evertrust.com.tw:8080/tfs/' --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'  --windowsLogonAccount 'evertrust\tfsagent' --windowsLogonPassword '1q2w3e4r5t_'
```

移除 agent

```powershell
.\config.cmd remove --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'
```
