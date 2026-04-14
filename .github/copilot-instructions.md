# BaGet 项目指南

BaGet 是一个轻量级跨平台 NuGet 和 Symbol 包服务器，基于 ASP.NET Core 3.1 构建。

## 架构

```
src/
├── BaGet/             # 主应用入口（ASP.NET Core 宿主、DI 配置、CLI 命令）
├── BaGet.Core/        # 核心抽象和业务逻辑（接口 + 实现）
├── BaGet.Web/         # Razor Pages UI 和 NuGet API 端点
├── BaGet.Protocol/    # NuGet V3 协议客户端库
├── BaGet.Database.*/  # 数据库驱动（Sqlite/MySql/PostgreSql/SqlServer）
├── BaGet.Azure/       # Azure Blob Storage + Azure Search + Table Storage
├── BaGet.Aws/         # Amazon S3 存储
├── BaGet.Aliyun/      # 阿里云 OSS 存储
└── BaGet.Gcp/         # Google Cloud Storage
```

**核心层接口**（`BaGet.Core`）：
- `IPackageDatabase` — 包元数据持久化
- `IPackageService` — 包业务逻辑（上传/删除/查询）
- `IStorageService` — 文件存储抽象
- `ISearchService` / `ISearchIndexer` — 搜索后端抽象
- `IAuthenticationService` — API Key 认证

**提供者选择**：运行时通过 `appsettings.json` 中的 `Type` 字段选择活跃实现（数据库、存储、搜索），由 DI 容器注入。

## 构建与测试

```bash
# 构建
dotnet build src/BaGet/BaGet.csproj

# 本地运行（SQLite + 文件系统）
dotnet run --project src/BaGet/

# 运行全部测试
dotnet test

# 运行指定测试项目
dotnet test tests/BaGet.Core.Tests/
dotnet test tests/BaGet.Web.Tests/
```

访问 `http://localhost:5000/` 查看 Web UI，`http://localhost:5000/v3/index.json` 查看 NuGet API。

## 配置

主配置文件：`src/BaGet/appsettings.json`，完整配置说明见 [docs/configuration.md](../docs/configuration.md)。

关键字段：

| 字段 | 可选值 | 说明 |
|------|--------|------|
| `Database.Type` | `Sqlite` / `MySql` / `PostgreSql` / `SqlServer` | 默认 SQLite |
| `Storage.Type` | `FileSystem` / `AwsS3` / `AzureBlobStorage` / `AliyunOss` / `GoogleCloud` | 默认文件系统 |
| `Search.Type` | `Database` / `AzureSearch` | 默认数据库搜索 |
| `ApiKey` | 任意字符串 | 空则禁用认证 |
| `Mirror.Enabled` | `true` / `false` | 是否开启透传缓存 |

## 前端

- **框架**：Bootstrap 3.4.1 + Alpine.js（响应式）
- **图标**：Office UI Fabric Core 11（`ms-Icon ms-Icon--*` 类）
- **样式**：`src/BaGet.Web/wwwroot/css/site.css`（CSS 变量设计系统，主色 `#1a56db`）
- **中文字体栈**：`PingFang SC`, `Microsoft YaHei`, `Segoe UI`
- **语言**：`<html lang="zh-CN">`，所有 UI 文本为中文

Razor Pages 位于 `src/BaGet.Web/Pages/`：`Index.cshtml`（包列表）、`Package.cshtml`（包详情）、`Upload.cshtml`（发布说明）。

## 约定

- **数据库迁移**：每个 `BaGet.Database.*` 项目下有独立的 `Migrations/` 目录，新增实体字段须为每个数据库提供者分别添加迁移
- **添加存储/搜索后端**：实现对应接口，在 `BaGet.Core/BaGetApplication.cs` 中注册，在 `src/BaGet/Startup.cs` 中配置提供者
- **测试框架**：xUnit；集成测试使用 `Microsoft.AspNetCore.Mvc.Testing` 中的 `WebApplicationFactory`
- **注释与 UI 文本**：代码注释和所有 UI 可见文本均使用**中文**
- **文件组织**：按功能拆分文件，避免单文件过长

## 文档

- 部署指南：`docs/installation/`（local, docker, azure, aws, aliyun, gcp）
- NuGet 协议 SDK：`docs/advanced/sdk.md`
- 迁移指南：`docs/import/`
- 与其他方案对比：`docs/vs/`
