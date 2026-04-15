# Docker 部署指南

本文介绍如何使用 Docker 部署 WinM Nuget 私有包服务器。

## 前置条件

- 已安装 [Docker](https://docs.docker.com/get-docker/)（20.10+）
- 已安装 [Docker Compose](https://docs.docker.com/compose/install/)（2.0+，`docker compose` 命令）

## 镜像说明

镜像托管在 GitHub Container Registry（GHCR），地址为：

```
ghcr.io/winm-tech/baget
```

| 标签 | 说明 | 触发方式 |
|------|------|----------|
| `latest` | 最新开发版，每次推送到 `main` 分支自动构建 | 自动（CI/CD） |
| `1.0.0`、`1.1.0` 等 | 正式发布版本，稳定可用 | 手动触发 Release workflow |

> **生产环境建议**：使用具体版本标签（如 `ghcr.io/winm-tech/baget:1.0.0`）而非 `latest`，避免自动更新引入非预期变更。可在 [GitHub Releases](https://github.com/WinM-Tech/BaGet/releases) 查看所有可用版本。

---

## 方式一：Docker Compose 部署（推荐）

这是最简单的部署方式，适合生产环境。

### 1. 获取配置文件

克隆仓库或直接下载 `docker-compose.yml`：

```bash
git clone https://github.com/WinM-Tech/BaGet.git
cd BaGet
```

### 2. 修改 API 密钥

编辑 `docker-compose.yml`，将 `CHANGE-ME-SECRET-KEY` 替换为安全的密钥：

```yaml
environment:
  ApiKey: "your-strong-secret-key-here"
```

### 3. 启动服务

```bash
docker compose up -d
```

### 4. 验证服务

```bash
# 查看容器状态
docker compose ps

# 查看日志
docker compose logs -f winm-nuget
```

浏览器打开 `http://localhost:5000/` 可看到包列表页面。

### 常用命令

```bash
# 停止服务
docker compose down

# 更新到最新镜像并重启
docker compose pull && docker compose up -d

# 查看数据卷位置
docker volume inspect baget_baget-data
```

### 升级到指定版本

编辑 `docker-compose.yml`，将 `image` 改为目标版本标签：

```yaml
image: ghcr.io/winm-tech/baget:1.1.0
```

然后重新拉取并启动：

```bash
docker compose pull && docker compose up -d
```

> 数据卷 `baget-data` 不会因升级而丢失，升级前建议备份数据卷目录。

---

## 方式二：docker run 命令

如不使用 Compose，可以直接运行：

### 1. 创建数据目录

```bash
mkdir -p baget-data
```

### 2. 创建环境变量配置文件

创建 `baget.env`：

```ini
# API 密钥（请修改为安全的密钥）
ApiKey=YOUR-SECRET-API-KEY

# 存储配置
Storage__Type=FileSystem
Storage__Path=/var/baget/packages

# 数据库配置
Database__Type=Sqlite
Database__ConnectionString=Data Source=/var/baget/baget.db

# 搜索配置
Search__Type=Database
```

### 3. 启动容器

```bash
docker run -d \
  --name winm-nuget \
  -p 5000:80 \
  --env-file baget.env \
  -v "$(pwd)/baget-data:/var/baget" \
  --restart unless-stopped \
  ghcr.io/winm-tech/baget:latest
```

Windows PowerShell 用户：

```powershell
docker run -d `
  --name winm-nuget `
  -p 5000:80 `
  --env-file baget.env `
  -v "${PWD}/baget-data:/var/baget" `
  --restart unless-stopped `
  ghcr.io/winm-tech/baget:latest
```

---

## 方式三：本地构建镜像

如需自定义镜像，可从源码构建：

```bash
# 构建镜像
docker build -t winm-nuget:local .

# 使用本地镜像启动
docker run -d \
  --name winm-nuget \
  -p 5000:80 \
  --env-file baget.env \
  -v "$(pwd)/baget-data:/var/baget" \
  winm-nuget:local
```

---

## 发布 NuGet 包

```bash
dotnet nuget push \
  -s http://localhost:5000/v3/index.json \
  -k YOUR-SECRET-API-KEY \
  your-package.1.0.0.nupkg
```

发布符号包（`.snupkg`）：

```bash
dotnet nuget push \
  -s http://localhost:5000/v3/index.json \
  -k YOUR-SECRET-API-KEY \
  your-package.1.0.0.snupkg
```

---

## 配置 NuGet 客户端

### 方法一：Visual Studio

**工具 → NuGet 包管理器 → 包管理器设置 → 包源**，添加：

- 名称：`WinM Nuget`
- 地址：`http://localhost:5000/v3/index.json`

### 方法二：nuget.config 文件

在项目根目录创建或修改 `nuget.config`：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="WinM Nuget" value="http://localhost:5000/v3/index.json" />
  </packageSources>
</configuration>
```

### 方法三：命令行

```bash
dotnet nuget add source http://localhost:5000/v3/index.json \
  --name "WinM Nuget"
```

---

## 还原包

使用以下包源地址还原包：

```
http://localhost:5000/v3/index.json
```

---

## Symbol 调试服务器

符号服务器地址：

```
http://localhost:5000/api/download/symbols
```

在 Visual Studio 中配置：**调试 → 选项 → 符号**，添加上述地址。

---

## 数据持久化

| 路径 | 说明 |
|------|------|
| `/var/baget/baget.db` | SQLite 数据库（包元数据） |
| `/var/baget/packages/` | NuGet 包文件（`.nupkg` / `.snupkg`） |

**备份建议**：

```bash
# 备份数据卷
docker run --rm \
  -v baget_baget-data:/data \
  -v $(pwd):/backup \
  alpine tar czf /backup/baget-backup-$(date +%Y%m%d).tar.gz -C /data .
```

---

## 常见问题

**Q：容器内无法访问？**
检查端口映射和防火墙规则，确保宿主机的 5000 端口已开放。

**Q：推包时返回 401？**
确认 `ApiKey` 配置与 `-k` 参数的值一致。

**Q：如何更换端口？**
修改 `docker-compose.yml` 中的 `ports` 配置，例如改为 `"8080:80"`。

**Q：如何使用 MySQL/PostgreSQL 替代 SQLite？**
修改环境变量：
```ini
Database__Type=MySql
Database__ConnectionString=Server=db;Database=baget;User=root;Password=pass;
```

