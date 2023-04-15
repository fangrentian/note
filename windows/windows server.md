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
