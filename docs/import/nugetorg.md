# 导入 nuget.org 包

!!! warning
    本页内容正在建设中！

## 镜像模式

您可以配置 BaGet 妆成 nuget.org 的镜像。例如，当您安装 BaGet 并启用镜像后，如果尝试安装
[`Newtonsoft.Json`](https://www.nuget.org/packages/Newtonsoft.Json/) 包，而 BaGet 本地尚没有该包，它会自动从 nuget.org 索引这个包。这也直接被称为“透传缓存”。

更多信息请参阅[启用透传缓存](../configuration#启用透传缓存)。

## 从 nuget.org 导入包下载量

1. 进入 `.\BaGet\src\BaGet` 目录
2. 执行：

```
dotnet run -- import-downloads
```

## 导入全部 nuget.org 包

* TODO: 提交代码
* 说明扩展方案
* 最后重建索引
* 导入 nuget.org 下载量
