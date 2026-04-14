# 使用阿里云对象存储服务（OSS）

您可以将包存储到[阿里云 OSS](https://www.aliyun.com/product/oss)。

## 配置 BaGet

通过编辑 `appsettings.json` 文件修改 BaGet 配置。完整配置项请参阅 [BaGet 配置指南](../configuration.md)。

### 阿里云 OSS 存储

修改 `appsettings.json` 文件：

```json
{
    ...

    "Storage": {
        "Type": "AliyunOss",
        "Endpoint": "oss-cn-hangzhou.aliyuncs.com",
        "Bucket": "my-bucket",
        "AccessKey": "",
        "AccessKeySecret": "",
        "Prefix": "lib/baget" // 可选
    },

    ...
}
```
