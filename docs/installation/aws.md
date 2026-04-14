# 在 AWS 上运行 BaGet

!!! warning
    本页内容正在建设中！

利用 Amazon Web Services 扩展 BaGet。可将元数据存储到 [Amazon RDS](https://aws.amazon.com/rds/postgresql/)，将包文件上传到 [Amazon S3](https://aws.amazon.com/s3/)。

## 配置 BaGet

通过编辑 `appsettings.json` 文件修改 BaGet 配置。完整配置项请参阅 [BaGet 配置指南](../configuration.md)。

### Amazon S3

修改 `appsettings.json` 文件：

```json
{
    ...

    "Storage": {
        "Type": "AwsS3",
        "Region": "us-west-1",
        "Bucket": "foo",
        "AccessKey": "",
        "SecretKey": ""
    },

    ...
}
```

### Amazon RDS

使用 PostgreSQL，修改 `appsettings.json`：

```json
{
    ...

    "Database": {
        "Type": "PostgreSql",
        "ConnectionString": "..."
    },

    ...
}
```

使用 MySQL，修改 `appsettings.json`：

```json
{
    ...

    "Database": {
        "Type": "MySql",
        "ConnectionString": "..."
    },

    ...
}
```

## 发布包

使用以下命令发布您的第一个包：

```
dotnet nuget push -s http://localhost:5000/v3/index.json package.1.0.0.nupkg
```

发布[符号包](https://docs.microsoft.com/zh-cn/nuget/create-packages/symbol-packages-snupkg)：

```
dotnet nuget push -s http://localhost:5000/v3/index.json symbol.package.1.0.0.snupkg
```

!!! warning
    建议通过设置 API 密钒保护服务器安全。详见[设置 API 密钒](../configuration.md#设置-api-密钒)。

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
