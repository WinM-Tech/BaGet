# WinM Nuget 📦

> 本项目基于 https://github.com/loic-sharma/BaGet 修改而来

[![构建状态](https://img.shields.io/github/actions/workflow/status/WinM-Tech/BaGet/.github/workflows/main.yml)](https://github.com/WinM-Tech/BaGet/actions)
[![许可证](https://img.shields.io/github/license/WinM-Tech/BaGet)](LICENSE)

**WinM Nuget** 是一个基于 [BaGet](https://github.com/loic-sharma/BaGet) 的轻量级、跨平台私有 NuGet 包服务器，支持 [NuGet V3 协议](https://learn.microsoft.com/nuget/what-is-nuget)和 [Symbol 符号包](https://docs.microsoft.com/zh-cn/windows/desktop/debug/symbol-servers-and-symbol-stores)，使用 .NET 8 构建。

<p align="center">
  <img width="100%" src="https://user-images.githubusercontent.com/737941/50140219-d8409700-0258-11e9-94c9-dad24d2b48bb.png">
</p>

## 快速开始

### 方式一：直接运行

1. 安装 [.NET 8 SDK](https://dotnet.microsoft.com/download)
2. 克隆本仓库
3. 在项目根目录运行：

```bash
dotnet run --project src/BaGet/
```

4. 浏览器打开 `http://localhost:5000/`

### 方式二：Docker 部署（推荐）

```bash
# 创建数据目录
mkdir baget-data

# 创建配置文件
cat > baget.env << EOF
ApiKey=YOUR-SECRET-API-KEY
Storage__Type=FileSystem
Storage__Path=/var/baget/packages
Database__Type=Sqlite
Database__ConnectionString=Data Source=/var/baget/baget.db
Search__Type=Database
EOF

# 启动容器
docker run -d --name winm-nuget \
  -p 5000:80 \
  --env-file baget.env \
  -v "$(pwd)/baget-data:/var/baget" \
  --restart unless-stopped \
  ghcr.io/winm-tech/baget:latest
```

详细部署说明见 [Docker 部署文档](docs/installation/docker.md)。

## 功能特性

- **跨平台**：支持 Windows、macOS 和 Linux
- **容器化**：原生支持 [Docker](docs/installation/docker.md) 部署
- **多种存储后端**：文件系统、[Azure Blob](docs/installation/azure.md)、[AWS S3](docs/installation/aws.md)、[阿里云 OSS](docs/installation/aliyun.md)、[Google Cloud](docs/installation/gcp.md)
- **多种数据库**：SQLite（默认）、MySQL、PostgreSQL、SQL Server
- **搜索支持**：内置数据库搜索，可选 Azure Search
- **透明缓存**：可作为 nuget.org 的镜像加速内网下载
- **Symbol 服务器**：支持 `.snupkg` 符号包，便于调试

## 发布包

```bash
dotnet nuget push -s http://localhost:5000/v3/index.json \
  -k YOUR-SECRET-API-KEY \
  your-package.1.0.0.nupkg
```

## 配置 NuGet 源

在 `nuget.config` 或 Visual Studio 中添加包源：

```
http://localhost:5000/v3/index.json
```

## 配置说明

主要配置项（`appsettings.json` 或环境变量）：

| 配置项 | 默认值 | 说明 |
|--------|--------|------|
| `ApiKey` | （空） | 发布包所需的 API 密钥，空则禁用认证 |
| `Database__Type` | `Sqlite` | 数据库类型：`Sqlite` / `MySql` / `PostgreSql` / `SqlServer` |
| `Storage__Type` | `FileSystem` | 存储类型：`FileSystem` / `AwsS3` / `AzureBlobStorage` / `AliyunOss` / `GoogleCloud` |
| `Search__Type` | `Database` | 搜索类型：`Database` / `AzureSearch` |
| `Mirror__Enabled` | `false` | 是否开启 nuget.org 透明缓存 |

完整配置说明见 [配置文档](docs/configuration.md)。

## 构建与测试

```bash
# 构建
dotnet build src/BaGet/BaGet.csproj

# 运行所有测试
dotnet test

# 本地开发运行
dotnet run --project src/BaGet/
```

## 相关链接

- [完整文档](docs/index.md)
- [Docker 部署](docs/installation/docker.md)
- [配置说明](docs/configuration.md)
- [GitHub 仓库](https://github.com/WinM-Tech/BaGet)
- [问题反馈](https://github.com/WinM-Tech/BaGet/issues)

## 许可证

本项目基于 [MIT License](LICENSE) 开源。

