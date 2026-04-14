# nuget.org

!!! warning
    本页内容正在建设中！

[nuget.org](https://www.nuget.org/)，也被称为“Gallery”，是托管开源包的事实标准 Feed。如果您开发开源项目，建议在此发布 NuGet 包。

Gallery 经过实战验证，具备良好的扩展性。您可以在 [Gallery 的 Wiki](https://github.com/NuGet/NuGetGallery/wiki/Hosting-nuget.org's-v3-services) 上找到自行托管实例的指南。
Gallery 的相关仓库：

* [NuGet/NuGetGallery](https://github.com/NuGet/NuGetGallery) - [nuget.org](https://nuget.org) 网站及 v2 API
* [NuGet/NuGet.Jobs](https://github.com/NuGet/NuGet.Jobs/) - 验证和包统计等后台作业
* [NuGet/NuGet.Services.Metadata](https://github.com/NuGet/NuGet.Services.Metadata/) - NuGet v3 实现
* [NuGet/ServerCommon](https://github.com/NuGet/ServerCommon) - NuGet 服务公用库

nuget.org 系统十分复杂，自行托管并不简单。

# BaGet vs nuget.org

待完善。详见 [相关 Issue](https://github.com/loic-sharma/BaGet/issues/71)。

* BaGet 仅与 [NuGet/NuGet.Services.Metadata](https://github.com/NuGet/NuGet.Services.Metadata/) 等价
* nuget.org v3 实现为静态架构
    * 仅支持 Windows
    * 高度依赖 Azure
    * 读取可扩展到近乎无限
    * 写入扩展比较差
    * JSON 静态文件存储于 Azure Blob Storage
    * 通过 CDN 提供服务
* BaGet v3 实现为动态架构
    * 跨平台实现
    * 请求由服务查询数据库响应
    * 架构简单，一键部署小型 Feed
    * 写入扩展性更好
    * 读取扩展相对较弱
    * 更易增加新功能
    * 可靠性相对较难保证
