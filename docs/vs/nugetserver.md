# NuGet.Server

!!! warning
    本页内容正在建设中！

[NuGet.Server](https://github.com/NuGet/NuGet.Server) 是一个轻量级的独立 NuGet 服务器。如果您目前使用 NuGet.Server，建议升级到 BaGet。如需迁移帮助，请在 [GitHub](https://github.com/WinM-Tech/BaGet/issues) 提交 Issue。

* NuGet.Server
    * 仅支持 Windows
    * 支持 NuGet v2 API（缺少已验证包、签名包等功能）
    * 不支持 NuGet v3 API
    * 扩展性差
    * 文档不完善
    * 维护不活跃
* BaGet
    * 跨平台
    * 支持 NuGet v3 API

## 迁移指南

您可以使用 [NuGet.Server 迁移指南](../import/nugetserver.md) 将包导入到 BaGet。
