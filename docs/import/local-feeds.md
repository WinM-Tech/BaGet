# 从本地 Feed 导入包

[本地 Feed](https://docs.microsoft.com/zh-cn/nuget/hosting-packages/local-feeds) 允许您使用本地文件夹作为 NuGet 包来源。

!!! info
    请参阅 [BaGet vs 本地 Feed](../vs/local-feeds.md) 页面了解升级到 BaGet 的理由。

## 迁移步骤

确保已安装 [nuget.exe](https://www.nuget.org/downloads)。在 PowerShell 中设置变量：

```ps1
$source = "C:\path\to\local\feed"
$destination = "http://localhost:5000/v3/index.json"
```

如果已[配置 BaGet 需要 API 密钒](https://loic-sharma.github.io/BaGet/configuration/#requiring-an-api-key)，请设置 API 密钒：

```ps1
& nuget.exe setapikey "MY-API-KEY" -Source $destination
```

然后执行以下 PowerShell 脚本：

```ps1
$packages = nuget list -AllVersions -Source $source

$packages | % {
  $id, $version = $_ -Split " "
  $nupkg = $id + "." + $version + ".nupkg"
  $path = [IO.Path]::Combine($source, $id, $version, $nupkg)

  Write-Host "nuget.exe push -Source $destination ""$path"""
  & nuget.exe push -Source $destination $path
}
```
