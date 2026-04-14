# 在 Azure 上运行 BaGet

!!! warning
    本页内容正在建设中！

利用 Azure 扩展 BaGet。可将元数据存储到 [Azure SQL 数据库](https://azure.microsoft.com/zh-cn/services/sql-database/)，将包文件上传到 [Azure Blob 存储](https://azure.microsoft.com/zh-cn/services/storage/blobs/)，并使用 [Azure 搜索](https://azure.microsoft.com/zh-cn/services/search/) 提供强大的搜索功能。

## 待完善

* 应用服务（App Service）
* 表存储（Table Storage）
* 高可用配置

## 配置 BaGet

通过编辑 `appsettings.json` 文件修改 BaGet 配置。完整配置项请参阅 [BaGet 配置指南](../configuration.md)。

### Azure SQL 数据库

修改 `appsettings.json` 文件：

```json
{
    ...

    "Database": {
        "Type": "SqlServer",
        "ConnectionString": "..."
    },

    ...
}
```

### Azure Blob 存储

修改 `appsettings.json` 文件：

```json
{
    ...

    "Storage": {
        "Type": "AzureBlobStorage",

        "AccountName": "my-account",
        "AccessKey": "abcd1234",
        "Container": "my-container"
    },

    ...
}
```

也可以使用完整的 Azure Storage 连接字符串：

```json
{
    ...

    "Storage": {
        "Type": "AzureBlobStorage",

        "ConnectionString": "AccountName=my-account;AccountKey=abcd1234;...",
        "Container": "my-container"
    },

    ...
}
```

### Azure Search

修改 `appsettings.json` 文件：

```json
{
    ...

    "Search": {
        "Type": "Azure",
        "AccountName": "my-account",
        "ApiKey": "ABCD1234"
    },

    ...
}
```

## Publish packages

Publish your first package with:

```
dotnet nuget push -s http://localhost:5000/v3/index.json package.1.0.0.nupkg
```

Publish your first [symbol package](https://docs.microsoft.com/en-us/nuget/create-packages/symbol-packages-snupkg) with:

```
dotnet nuget push -s http://localhost:5000/v3/index.json symbol.package.1.0.0.snupkg
```

!!! warning
    You should secure your server by requiring an API Key to publish packages. For more information, please refer to the [Require an API Key](../configuration.md#require-an-api-key) guide.

## Restore packages

You can restore packages by using the following package source:

`http://localhost:5000/v3/index.json`

Some helpful guides:

* [Visual Studio](https://docs.microsoft.com/en-us/nuget/consume-packages/install-use-packages-visual-studio#package-sources)
* [NuGet.config](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file#package-source-sections)

## Symbol server

You can load symbols by using the following symbol location:

`http://localhost:5000/api/download/symbols`

For Visual Studio, please refer to the [Configure Debugging](https://docs.microsoft.com/en-us/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger?view=vs-2017#configure-symbol-locations-and-loading-options) guide.
