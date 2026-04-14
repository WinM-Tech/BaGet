# 在本地计算机上运行 BaGet

## 运行步骤

1. 安装 [.NET Core SDK](https://www.microsoft.com/zh-cn/dotnet/download)
2. 下载并解压 [BaGet 最新发行版](https://github.com/WinM-Tech/BaGet/releases)
3. 执行 `dotnet BaGet.dll` 启动服务
4. 在浏览器中访问 `http://localhost:5000/`

## 配置 BaGet

通过编辑 `appsettings.json` 文件修改 BaGet 配置。完整配置项请参阅 [BaGet 配置指南](../configuration.md)。

## 发布包

使用以下命令发布您的第一个包：

```
dotnet nuget push -s http://localhost:5000/v3/index.json package.1.0.0.nupkg
```

发布第一个[符号包](https://docs.microsoft.com/zh-cn/nuget/create-packages/symbol-packages-snupkg)：

```
dotnet nuget push -s http://localhost:5000/v3/index.json symbol.package.1.0.0.snupkg
```

!!! warning
    建议通过设置 API 密钥来保护您的服务器，防止未授权发布。详见[设置 API 密钥](../configuration.md#设置-api-密钥)。

## 还原包

使用以下包源地址还原包：

`http://localhost:5000/v3/index.json`

相关配置指南：

* [Visual Studio 配置](https://docs.microsoft.com/zh-cn/nuget/consume-packages/install-use-packages-visual-studio#package-sources)
* [NuGet.config 配置](https://docs.microsoft.com/zh-cn/nuget/reference/nuget-config-file#package-source-sections)

## 符号服务器

使用以下地址加载调试符号：

`http://localhost:5000/api/download/symbols`

Visual Studio 调试符号配置请参阅[配置调试符号位置](https://docs.microsoft.com/zh-cn/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger?view=vs-2017#configure-symbol-locations-and-loading-options)。
