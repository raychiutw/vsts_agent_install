# TFS 部屬集區 agent 安裝步驟

### on Windows

請使用 `EVERTRUST\COMUser` 登入

#### 加入帳號

請將 `EVERTRUST\TFSAgent` 加入管理者群組

#### 安裝 agent

請用 `管理者身分` 開啟 PowerShell 命令列視窗執行

請記得修改 `deployGroupName`

```powershell
$deployGroupName="{集區名稱}"
$version="2.193.1";
$ErrorActionPreference="Stop";

If($PSVersionTable.PSVersion -lt (New-Object System.Version("3.0"))){ throw "指令碼 (3.0) 需要的 Windows PowerShell 最低版本，與目前正在執行的 Windows PowerShell 版本不符。" };
If(-NOT (Test-Path $env:SystemDrive\'vstsagent')){mkdir $env:SystemDrive\'vstsagent'}; 
cd $env:SystemDrive\'vstsagent'; 
for($i=1; $i -lt 100; $i++){$destFolder="A"+$i.ToString();
if(-NOT (Test-Path ($destFolder))){mkdir $destFolder;cd $destFolder;break;}}; 
$parentFolder=(Get-Item $PWD).Parent.FullName;
$agentZip="$parentFolder\vsts-agent-win-x64-$version.zip";
$DefaultProxy=[System.Net.WebRequest]::DefaultWebProxy;$securityProtocol=@();
$securityProtocol+=[Net.ServicePointManager]::SecurityProtocol;$securityProtocol+=[Net.SecurityProtocolType]::Tls12;
[Net.ServicePointManager]::SecurityProtocol=$securityProtocol;
$WebClient=New-Object Net.WebClient; 
$Uri="https://vstsagentpackage.azureedge.net/agent/$version/vsts-agent-win-x64-$version.zip";
if($DefaultProxy -and (-not $DefaultProxy.IsBypassed($Uri))){$WebClient.Proxy= New-Object Net.WebProxy($DefaultProxy.GetProxy($Uri).OriginalString, $True);};
$WebClient.DownloadFile($Uri, $agentZip);
Add-Type -AssemblyName System.IO.Compression.FileSystem;
[System.IO.Compression.ZipFile]::ExtractToDirectory( $agentZip, "$PWD");
.\config.cmd --deploymentpool --deploymentpoolname $deployGroupName --agent $env:COMPUTERNAME --runasservice --work '_work' --url 'http://tfs.evertrust.com.tw:8080/tfs/' --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'  --windowsLogonAccount 'evertrust\tfsagent' --windowsLogonPassword '1q2w3e4r5t_'
```

移除 agent

```powershell
c:\vstsagent\a1\config.cmd remove --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'

# 刪除 a1 資料夾 (也可用檔案總管刪除)
Remove-Item –path c:\vstsagent\a1 –recurse -force
```

重新安裝

> 請記得修改 `deploymentpoolname`

```powershell
cd c:\vstsagent\a1

.\config.cmd remove --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'
.\config.cmd --deploymentpool --deploymentpoolname "{集區名稱}" --agent $env:COMPUTERNAME --runasservice --work '_work' --url 'http://tfs.evertrust.com.tw:8080/tfs/' --auth negotiate --userName 'evertrust\tfsagent' --password '1q2w3e4r5t_'  --windowsLogonAccount 'evertrust\tfsagent' --windowsLogonPassword '1q2w3e4r5t_';
```
