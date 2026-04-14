# BaGet SDK

您可以使用 BaGet 的 [`BaGet.Protocol`](https://www.nuget.org/packages/BaGet.Protocol) 包与 NuGet 服务器进行交互。

## 快速开始

安装 [`BaGet.Protocol`](https://www.nuget.org/packages/BaGet.Protocol) 包：

```
dotnet add package BaGet.Protocol
```

## 列出包的所有版本

查找 `Newtonsoft.Json` 包的所有版本：

```csharp
NuGetClient client = new NuGetClient("https://api.nuget.org/v3/index.json");

IReadOnlyList<NuGetVersion>> versions = await client.ListPackageVersionsAsync("Newtonsoft.Json");

foreach (NuGetVersion version in versions)
{
    Console.WriteLine($"找到版本：{version}");
}
```

## 下载包

```csharp
NuGetClient client = new NuGetClient("https://api.nuget.org/v3/index.json");

string packageId = "Newtonsoft.Json";
NuGetVersion packageVersion = new NuGetVersion("12.0.1");

using (Stream packageStream = await client.DownloadPackageAsync(packageId, packageVersion))
{
    Console.WriteLine($"已下载包 {packageId} {packageVersion}");
}
```

## 获取包的元数据

```csharp
NuGetClient client = new NuGetClient("https://api.nuget.org/v3/index.json");

// 获取某个包所有版本的元数据
IReadOnlyList<PackageMetadata> items = await client.GetPackageMetadataAsync("Newtonsoft.Json");
if (!items.Any())
{
    Console.WriteLine("包 'Newtonsoft.Json' 不存在");
    return;
}

foreach (var metadata in items)
{
    Console.WriteLine($"版本：{metadata.Version}");
    Console.WriteLine($"已上架：{metadata.Listed}");
    Console.WriteLine($"标签：{metadata.Tags}");
    Console.WriteLine($"描述：{metadata.Description}");
}

// 或者获取某个特定版本的元数据
string packageId = "Newtonsoft.Json"
NuGetVersion packageVersion = new NuGetVersion("12.0.1");

PackageMetadata metadata = await client.GetPackageMetadataAsync(packageId, packageVersion);

Console.WriteLine($"已上架：{metadata.Listed}");
Console.WriteLine($"标签：{metadata.Tags}");
Console.WriteLine($"描述：{metadata.Description}");
```

## 搜索包

搜索包含 "json" 关键字的包：

```csharp
NuGetClient client = new NuGetClient("https://api.nuget.org/v3/index.json");
IReadOnlyList<SearchResult> results = await client.SearchAsync("json");

foreach (SearchResult result in results)
{
    Console.WriteLine($"找到包 {result.PackageId} {searchResult.Version}");
}
```
