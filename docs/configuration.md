# 配置说明

您可以通过编辑 `appsettings.json` 文件来修改 BaGet 的配置。

## 设置 API 密钥

您可以要求用户在发布包时提供密码（即 API 密钥）。
只需在 `ApiKey` 字段中填入所需的 API 密钥：

```json
{
    "ApiKey": "NUGET-SERVER-API-KEY",
    ...
}
```

之后用户推送包时必须提供该 API 密钥：

```c#
dotnet nuget push -s http://localhost:5000/v3/index.json -k NUGET-SERVER-API-KEY package.1.0.0.nupkg
```

## 启用透传缓存

透传缓存（Read-through caching）允许您从上游源索引包，可用于：

1. 在从 [nuget.org](https://nuget.org) 还原包速度较慢时加速构建
2. 在离线场景中仍能还原包

以下 `Mirror` 配置会让 BaGet 从 [nuget.org](https://nuget.org) 索引包：

```json
{
    ...

    "Mirror": {
        "Enabled":  true,
        "PackageSource": "https://api.nuget.org/v3/index.json"
    },

    ...
}
```

!!! info
    `PackageSource` 的值为 [NuGet 服务索引地址](https://docs.microsoft.com/zh-cn/nuget/api/service-index)。

## 启用包硬删除

为避免类似 ["left-pad" 问题](https://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm)，
BaGet 默认不允许删除包。当收到删除请求时，BaGet 会将包设为"下架"状态。
下架的包无法被搜索发现，但如果知道包的 ID 和版本仍可下载。
可通过设置 `PackageDeletionBehavior` 来覆盖此行为：

```json
{
    ...

    "PackageDeletionBehavior": "HardDelete",

    ...
}
```

## 允许包覆盖上传

默认情况下，如果同一 ID 和版本的包已存在，BaGet 会拒绝上传。
可通过设置 `AllowPackageOverwrites` 来允许覆盖：

```json
{
    ...

    "AllowPackageOverwrites": true,

    ...
}
```

## 私有 Feed

私有 Feed 要求用户在访问包之前进行身份验证。

!!! warning
    当前版本**不支持**私有 Feed！详见[此 PR](https://github.com/loic-sharma/BaGet/pull/69)。

## 数据库配置

BaGet 支持多种数据库引擎存储包信息：

- MySQL：`MySql`
- SQLite：`Sqlite`
- SQL Server：`SqlServer`
- PostgreSQL：`PostgreSql`
- Azure 表存储：`AzureTable`

每种数据库引擎都需要配置连接字符串，可参考 [ConnectionStrings.com](https://www.connectionstrings.com/) 了解各引擎的连接字符串格式。

可通过环境变量或编辑 `appsettings.json` 来配置数据库：

### 环境变量

与数据库配置相关的两个环境变量：

- **Database__Type**：数据库引擎类型，如 `PostgreSql` 或 `Sqlite`。
- **Database__ConnectionString**：数据库连接字符串。

### `appsettings.json`

数据库配置位于 `appsettings.json` 中的 `Database` 节点下：

```json
{
    ...

    "Database": {
        "Type": "Sqlite",
        "ConnectionString": "Data Source=baget.db"
    },

    ...
}
```

两个关键配置项：

- **Type**：数据库引擎类型，如 `PostgreSql` 或 `Sqlite`。
- **ConnectionString**：数据库连接字符串。

## IIS 服务器选项

IIS 服务器选项可在 `IISServerOptions` 节点下配置。详细选项说明请参阅 [docs.microsoft.com](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions)。

> 注意：如未指定，BaGet 中的 `MaxRequestBodySize` 默认为 250MB（262144000 字节），而非 ASP.NET Core 默认的 30MB。

```json
{
    ...

    "IISServerOptions": {
        "MaxRequestBodySize": 262144000
    },

    ...
}
```
