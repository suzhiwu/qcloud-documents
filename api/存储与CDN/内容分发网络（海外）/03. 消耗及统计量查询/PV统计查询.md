## 1. 接口描述

本接口（GetCdnOverseaPv）用于查询指定域名、指定日期的请求数统计信息。

+ 每五分钟一个统计点，一天共有 288 个统计点；
+ 指定日期可选区间为 30 天内；
+ 一次可查询一个域名，域名需要已经开通海外 CDN。


接口请求域名：<font style="color:red">cdn.api.qcloud.com</font>

[调用Demo](https://cloud.tencent.com/document/product/228/1734)

## 2. 输入参数
以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见[公共请求参数](https://cloud.tencent.com/doc/api/231/4473)页面。其中，此接口的 Action 字段为 GetCdnOverseaPv。

| 参数名称 | 是否必选 | 类型     | 描述                    |
| ---- | ---- | ------ | --------------------- |
| date | 是    | String | 指定查询日期，格式为：2016-09-22 |
| host | 是    | String | 指定要查询的域名              |

**注意事项：**
+ date：查询日期，暂时仅支持 30 天内数据查询，格式需要为 xxxx-xx-xx。


## 3. 输出参数

| 参数名称     | 类型     | 描述                                       |
| -------- | ------ | ---------------------------------------- |
| code     | Int    | 公共错误码，0 表示成功，其他值表示失败。详见错误码页面的[公共错误码](https://cloud.tencent.com/doc/api/231/5078#1.-.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81)。 |
| message  | String | 模块错误信息描述，与接口相关。                          |
| codeDesc | String | 英文错误信息，或业务侧错误码。                          |
| data     | Object | 返回结果数据，详细说明如下                            |

#### data字段说明
| 参数名称           | 类型     | 描述                                       |
| -------------- | ------ | ---------------------------------------- |
| period         | Int    | 统计数据时间区间，默认为 5 分钟                          |
| app_id         | String | 腾讯云 [服务账号](http://console.cloud.tencent.com/cloudAccount)，与 UIN 对应 |
| host           | String | 指定查询的域名                                  |
| start_datetime | String | 查询起始时间点，若填入日期为 2016-09-22，则为 2016-09-22 00:00:00 |
| end_datetime   | String | 查询结束时间点，若填入日期为 2016-09-22，则为 2016-09-22 23:55:00 |
| time_data      | Object | 请求数明细数据，详细说明如下                           |

#### time_data字段说明
| 参数名称     | 类型    | 描述       |
| -------- | ----- | -------- |
| requests | Array | 各个区间的请求数 |
| cache    | Array | 各个区间的缓存命中率 |

**注意事项：**
+ requests 和 cache 分别为 Int 和 Float 类型数组，从指定查询日期 0 点开始，每 5 分钟一个数据；
+ 00:00:00 点的统计量代表了 00:00:00 - 00:04:59 期间的统计量。

## 4. 示例

### 4.1 输入示例

> date: 2016-09-22
> host: www.test.com


### 4.2 GET 请求

GET 请求需要将所有参数都加在 URL 后：

```
https://cdn.api.qcloud.com/v2/index.php?
Action=GetCdnOverseaPv
&SecretId=XXXXXXXXXXXXXXXXXXXXXXXXXXX
&Timestamp=1462422547
&Nonce=12345678
&Signature=XXXXXXXXXXXXXXXXXXXXXXXXX
&date=2016-09-22
&host=www.test.com
```

### 4.3 POST 请求

POST请求时，参数填充在 HTTP Request-body 中，请求地址：

```
https://cdn.api.qcloud.com/v2/index.php
```

参数支持 form-data、x-www-form-urlencoded 等格式，参数数组如下：

```
array (
  'Action' => 'GetCdnOverseaPv',
  'SecretId' => 'XXXXXXXXXXXXXXXXXXXXXXXXXXXX',
  'Timestamp' => 1462782282,
  'Nonce' => 123456789,
  'Signature' => 'XXXXXXXXXXXXXXXXXXXXXXXX',
  'date' => '2016-09-22',
  "host" => 'www.test.com'
)
```

### 4.4 返回结果示例

```json
{
    "code": 0,
    "message": "",
    "codeDesc": "Success",
    "data": {
        "period": 5,
        "app_id": 1234567
        "host": "www.test.com",
        "start_datetime": "2016-09-22 00:00:00",
        "end_datetime": "2016-09-22 23:55:00",
        "time_data": {
            "requests": [
            	1,
                2,
                ...
            ]
            "cache": [
                100,
                99.12,
                ...
            ]
        }
    }
}
```
