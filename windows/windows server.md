# windows server

## CMD命令行激活系统

```shell
slmgr -ipk WX4NM-KYWYW-QJJR4-XV3QB-6VM33
slmgr -skms kms.0t.net.cn
slmgr -ato
```

## 客户端激活码

[适用于 Windows Server 和 Windows 的密钥管理服务 (KMS) 客户端激活和产品密钥 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys)

## 在windows server 2012 上通过powershell安装chrome

```shell
$Path = $env:TEMP; $Installer = "chrome_installer.exe"; Invoke-WebRequest "http://dl.google.com/chrome/install/375.126/chrome_installer.exe" -OutFile $Path\$Installer; Start-Process -FilePath $Path\$Installer -Args "/silent /install" -Verb RunAs -Wait; Remove-Item $Path\$Installer
```

全程静默安装, 就是这么任性

## 命令行安装 `Docker`

### 开启容器功能

```shell
PS C:\Windows\system32> Enable-WindowsOptionalFeature -Online -FeatureName Containers
Do you want to restart the computer to complete this operation now?
[Y] Yes  [N] No  [?] Help (default is "Y"): Y
```

### 安装 `docker`

```shell
PS C:\Windows\system32> Install-Module -Name DockerMsftProvider -Repository PSGallery -Force

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\Administrator\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the
NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

```shell
PS C:\Windows\system32> Install-Package -Name docker -ProviderName DockerMsftProvider

The package(s) come(s) from a package source that is not marked as trusted.
Are you sure you want to install software from 'DockerDefault'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y

Name                           Version          Source           Summary
----                           -------          ------           -------
Docker                         20.10.7          DockerDefault    Contains Docker EE for use with Windows Server.
```

### 重启电脑

```shell
PS C:\Windows\system32> Restart-Computer -Force 
```

### 验证安装

```shell
PS C:\Windows\system32> docker version
Client: Mirantis Container Runtime
 Version:           20.10.9
 API version:       1.41
 Go version:        go1.16.12m2
 Git commit:        591094d
 Built:             12/21/2021 21:34:30
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Mirantis Container Runtime
 Engine:
  Version:          20.10.9
  API version:      1.41 (minimum version 1.24)
  Go version:       go1.16.12m2
  Git commit:       9b96ce992b
  Built:            12/21/2021 21:33:06
  OS/Arch:          windows/amd64
  Experimental:     false
```
