# LiGet

!!! warning
    本页内容正在建设中！

[LiGet](https://github.com/ai-traders/liget) 是一个以 Linux 为首要平台的 NuGet 服务器。

* LiGet
    * 对 Paket 支持强
    * 仅支持 NuGet v2 API （缺少已验证包、签名包等）
    * 使用单一 JSON 文件存储包元数据
* BaGet
    * 支持 NuGet v3 API
    * 包元数据存储在数据库中
    * 可导入 nuget.org 上的所有包
    * 可在 Azure 上运行
