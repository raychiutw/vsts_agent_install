# TFS Agent Install

### 部署集區 Agent

[Windows](DeployGroupAgent/Windows.md)

[Centos](DeployGroupAgent/Centos.md)

[Ubuntu](DeployGroupAgent/Ubuntu.md)

### Build Server Agent

[Windows](BuildServerAgent/Windows.md)

[CentOS](BuildServerAgent/Centos.md)

[Ubuntu](BuildServerAgent/Ubuntu.md)

### dotnet nuget 使用 proxy

Nuget.Config 路徑

Windows 路徑

`C:\Users\{帳號}\AppData\Roaming\NuGet`

Linux / Mac 路徑

`./{帳號}/.nuget/NuGet/NuGet.Config`

增加 Proxy 設定

```xml
  <config>
    <add key="http_proxy" value="http://proxtdomain:port" />
    <add key="http_proxy.user" value="accout" />
    <add key="http_proxy.password" value="password" />
  </config>
```
