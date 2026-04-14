# 从 NuGet.Server 迁移

[NuGet.Server](https://github.com/NuGet/NuGet.Server) 是一个轻量级的独立 NuGet 服务器。如果您目前使用 NuGet.Server，建议升级到 BaGet。如需迁移帮助，请在 [GitHub](https://github.com/WinM-Tech/BaGet/issues) 提交 Issue。

!!! info
    请参阅 [BaGet vs NuGet.Server](../vs/nugetserver.md) 页面了解升级到 BaGet 的理由。

## 迁移步骤

确保已安装 [nuget.exe](https://www.nuget.org/downloads)。在 PowerShell 中执行：

```ps1
$source = "<NuGet.Server 包来源地址>"
$destination = "<BaGet 包来源地址>"
```

如果已[配置 BaGet 需要 API 密钒](https://loic-sharma.github.io/BaGet/configuration/#requiring-an-api-key)，请设置 API 密钒：

```ps1
& nuget.exe setapikey "MY-API-KEY" -Source $destination
```

执行以下 PowerShell 脚本进行包迁移：

```ps1
if (!(Test-Path "Web.config")) {
  throw "请在 NuGet.Server 的 Web.config 文件相同目录下执行此脚本"
}

(& nuget.exe list -AllVersions -Source $source).Split([Environment]::NewLine) | % {
  $id = $_.Split(" ")[0].Trim()
  $version = $_.Split(" ")[1].Trim()

  $path = [IO.Path]::Combine("Packages", $id, $version, "${id}.${version}.nupkg")

  Write-Host "nuget.exe push -Source $destination ""$path"""
  & nuget.exe push -Source $destination $path
}
```
