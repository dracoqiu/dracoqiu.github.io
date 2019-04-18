---
title: 腾讯云SDK-PHP
date: 2019-04-18 17:40:48
tags:
	- SDK
---
## PHP-SDK
### 简介
欢迎使用腾讯云开发者工具套件（SDK）3.0，SDK3.0 是云 API3.0 平台的配套工具。目前已经支持 cvm、vpc、cbs 等产品，后续所有的云服务产品都会接入进来。新版 SDK 实现了统一化，具有各个语言版本的 SDK 使用方法相同，接口调用方式相同，统一的错误码和返回包格式这些优点。
为方便 PHP 开发者调试和接入腾讯云产品 API，这里向您介绍适用于 PHP 的腾讯云开发工具包，并提供首次使用开发工具包的简单示例。让您快速获取腾讯云 PHP SDK 并开始调用。
<!-- more -->
### 支持 3.0 版本的产品列表
SDK3.0支持全部 API3.0下的产品，本列表可能滞后于实际代码，如有疑问请咨询具体的产品。
- [云服务器](https://cloud.tencent.com/document/api/213/15689)
- [黑石物理服务器](https://cloud.tencent.com/document/api/386/18637)
- [云硬盘](https://cloud.tencent.com/document/api/362/15634)
- [容器服务](https://cloud.tencent.com/document/api/457/31853)
- [容器实例服务](https://cloud.tencent.com/document/api/858/17761)
- [弹性伸缩](https://cloud.tencent.com/document/api/377/20423)
- [无服务器云函数](https://cloud.tencent.com/document/api/583/17235)
- [批量计算](https://cloud.tencent.com/document/api/599/15880)
- [负载均衡](https://cloud.tencent.com/document/api/214/30667)
- [私有网络](https://cloud.tencent.com/document/api/215/15755)
- [专线接入](https://cloud.tencent.com/document/api/216/18404)
- [云数据库 MySQL](https://cloud.tencent.com/document/api/236/15830)
- [云数据库 Redis](https://cloud.tencent.com/document/api/239/20002)
- [云数据库 MongoDB](https://cloud.tencent.com/document/api/240/31797)
- [数据传输服务 DTS](https://cloud.tencent.com/document/api/571/18122)
- [云数据库 MariaDB](https://cloud.tencent.com/document/api/237/16144)
- [分布式数据库 DCDB](https://cloud.tencent.com/document/api/557/16124)
- [云数据库 SQL Server](https://cloud.tencent.com/document/api/238/19927)
- [云数据库 PostgreSQL](https://cloud.tencent.com/document/api/409/16761)
- [内容分发网络](https://cloud.tencent.com/document/api/228/30974)
- [主机安全](https://cloud.tencent.com/document/api/296/19825)
- [Web 漏洞扫描](https://cloud.tencent.com/document/api/692/16733)
- [应用安全](https://cloud.tencent.com/document/api/283/17742)
- [云点播](https://cloud.tencent.com/document/api/266/31753)
- [云直播](https://cloud.tencent.com/document/api/267/20456)
- [智能语音服务](https://cloud.tencent.com/document/api/441/17362)
- [机器翻译](https://cloud.tencent.com/document/api/551/15612)
- [智能钛机器学习](https://cloud.tencent.com/document/api/851/18295)
- [催收机器人](https://cloud.tencent.com/document/api/656/18281)
- [智聆口语评测](https://cloud.tencent.com/document/api/884/19310)
- [腾讯优评](https://cloud.tencent.com/document/api/853/18384)
- [Elasticsearch Service](https://cloud.tencent.com/document/api/845/30620)
- [物联网通信](https://cloud.tencent.com/document/api/634/19469)
- [TBaaS](https://cloud.tencent.com/document/api/663/19455)
- [云监控](https://cloud.tencent.com/document/api/248/30343)
- [迁移服务平台](https://cloud.tencent.com/document/api/659/18591)
- [电子合同服务](https://cloud.tencent.com/document/api/869/17778)
- [计费相关](https://cloud.tencent.com/document/api/555/19170)
- [渠道合作伙伴](https://cloud.tencent.com/document/api/563/16034)
- [人脸核身-云智慧眼](https://cloud.tencent.com/document/api/1007/31320)
- [威胁情报云查](https://cloud.tencent.com/document/api/1013/31737)
- [样本智能分析平台](https://cloud.tencent.com/document/api/1012/31720)
- [数学作业批改](https://cloud.tencent.com/document/api/1004/30607)
- [人脸融合](https://cloud.tencent.com/document/api/670/31052)
- [人脸识别](https://cloud.tencent.com/document/api/867/32770)
- [数字版权管理](https://cloud.tencent.com/document/api/1000/30698)

### Composer安装
```
composer require tencentcloud/tencentcloud-sdk-php
```
### 示例一
以查询可用区接口为例:
```php
<?php
require_once '../../../TCloudAutoLoader.php';
// 导入对应产品模块的client
use TencentCloud\Cvm\V20170312\CvmClient;
// 导入要请求接口对应的Request类
use TencentCloud\Cvm\V20170312\Models\DescribeZonesRequest;
use TencentCloud\Common\Exception\TencentCloudSDKException;
use TencentCloud\Common\Credential;
try {
    // 实例化一个证书对象，入参需要传入腾讯云账户secretId，secretKey
    $cred = new Credential("secretId", "secretKey");

    // # 实例化要请求产品(以cvm为例)的client对象
    $client = new CvmClient($cred, "ap-guangzhou");

    // 实例化一个请求对象
    $req = new DescribeZonesRequest();

    // 通过client对象调用想要访问的接口，需要传入请求对象
    $resp = $client->DescribeZones($req);

    print_r($resp->toJsonString());
}
catch(TencentCloudSDKException $e) {
    echo $e;
}
```
### 示例二
查询用户该地域下符合条件的所有实例
```php
<?php
require_once '../../../TCloudAutoLoader.php';
// 导入对应产品模块的client
use TencentCloud\Cvm\V20170312\CvmClient;
// 导入要请求接口对应的Request类
use TencentCloud\Cvm\V20170312\Models\DescribeInstancesRequest;
use TencentCloud\Cvm\V20170312\Models\Filter;
use TencentCloud\Common\Exception\TencentCloudSDKException;
use TencentCloud\Common\Credential;
// 导入可选配置类
use TencentCloud\Common\Profile\ClientProfile;
use TencentCloud\Common\Profile\HttpProfile;

try {
    // 实例化一个证书对象，入参需要传入腾讯云账户secretId，secretKey
    //$cred = new Credential("secretId", "secretKey");
    $cred = new Credential(getenv("TENCENTCLOUD_SECRET_ID"), getenv("TENCENTCLOUD_SECRET_KEY"));

    // 实例化一个http选项，可选的，没有特殊需求可以跳过
    $httpProfile = new HttpProfile();
    $httpProfile->setReqMethod("GET");  // post请求(默认为post请求)
    $httpProfile->setReqTimeout(30);    // 请求超时时间，单位为秒(默认60秒)
    $httpProfile->setEndpoint("cvm.ap-shanghai.tencentcloudapi.com");  // 指定接入地域域名(默认就近接入)

    // 实例化一个client选项，可选的，没有特殊需求可以跳过
    $clientProfile = new ClientProfile();
    $clientProfile->setSignMethod("HmacSHA256");  // 指定签名算法(默认为HmacSHA256)
    $clientProfile->setHttpProfile($httpProfile);

    // 实例化要请求产品(以cvm为例)的client对象,clientProfile是可选的
    $client = new CvmClient($cred, "ap-shanghai", $clientProfile);

    // 实例化一个cvm实例信息查询请求对象,每个接口都会对应一个request对象。
    $req = new DescribeInstancesRequest();

    // 填充请求参数,这里request对象的成员变量即对应接口的入参
    // 你可以通过官网接口文档或跳转到request对象的定义处查看请求参数的定义
    $respFilter = new Filter();  // 创建Filter对象, 以zone的维度来查询cvm实例
    $respFilter->Name = "zone";
    $respFilter->Values = ["ap-shanghai-1", "ap-shanghai-2"];
    $req->Filters = [$respFilter];  // Filters 是成员为Filter对象的列表

    // 这里还支持以标准json格式的string来赋值请求参数的方式。下面的代码跟上面的参数赋值是等效的
    $params = [
        "Filters" => [
            [
                "Name" => "zone",
                "Values" => ["ap-shanghai-1", "ap-shanghai-2"]
            ]
        ]
    ];
    $req->fromJsonString(json_encode($params));

    // 通过client对象调用DescribeInstances方法发起请求。注意请求方法名与请求对象是对应的
    // 返回的resp是一个DescribeInstancesResponse类的实例，与请求对象对应
    $resp = $client->DescribeInstances($req);

    // 输出json格式的字符串回包
    print_r($resp->toJsonString());

    // 也可以取出单个值。
    // 你可以通过官网接口文档或跳转到response对象的定义处查看返回字段的定义
    print_r($resp->TotalCount);
}
catch(TencentCloudSDKException $e) {
    echo $e;
}
```
### 分析
从上面的2个示例可以看出，使用sdk分为4步
1. 实例化一个证书对象
2. 实例化要请求产品的client对象
3. 实例化一个请求对象
4. 通过client对象调用想要访问的接口

### 例子 - 查询视频的详情
```php
<?php
require_once '../../../TCloudAutoLoader.php';
use TencentCloud\Common\Credential;
// 导入对应产品模块的client
use TencentCloud\Vod\V20180717\VodClient;
use TencentCloud\Vod\V20180717\Models\DescribeMediaInfosRequest;
use TencentCloud\Common\Exception\TencentCloudSDKException;

try {
    // 实例化一个证书对象，入参需要传入腾讯云账户secretId，secretKey
    $secretId = '123456';
    $secretKey = '123456';
    $cred = new Credential($secretId, $secretKey);

    // 实例化要请求产品的client对象
    /**
    * 这里是获取媒体详细信息，接口名称为【DescribeMediaInfos】
    * 在sdk里面搜索这个关键字，会找到包含这个名字的文件，结构如下
    * Vod
    * -- Models
    * ---- DescribeMediaInfosRequest.php 构建请求参数
    * ---- DescribeMediaInfosResponse.php 构建返回参数
    * -- VodClient.php
    */
    $client = new VodClient($cred, "ap-guangzhou");

    // 实例化一个请求对象
    $req = new DescribeMediaInfosRequest();
    // 传递参数
    $req->FileIds = ['5285890788094598255'];

    // 通过client对象调用想要访问的接口，需要传入请求对象
    $resp = $client->DescribeMediaInfos($req);
    /**
    * 如果提示错误【cURL error 60: SSL certificate problem: self signed certificate in certificate chain (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)】
    * 则修改文件【\vendor\tencentcloud\tencentcloud-sdk-php\src\TencentCloud\Common\Http\HttpConnection.php】的方法【__construct】
    * 由 $this->client = new Client(["base_uri" => $url]);
    * 改为 $this->client = new Client(["base_uri" => $url,'verify'=>false]);
    */
    $result = $resp->toJsonString();
    $res = json_decode($result, true);
    dd($res);
} catch (TencentCloudSDKException $e) {
    dd($e);
}
```
