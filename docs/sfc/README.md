

# 同程顺风车分销接口 V1.0.3

### 版本

| 版本号 | 更新时间   | 更新内容                             |
| ------ | ---------- | ------------------------------------ |
| 1.0.0  | 2021-04-01 | 编写顺风车文档                       |
| 1.0.1  | 2021-05-19 | 订单状态同步增加参数                 |
| 1.0.2  | 2021-05-26 | 订单状态同步枚举增加状态码 10        |
| 1.0.3  | 2021-09-01 | 新增订单详情展示虚拟号以及虚拟号推送 |

### 概述

本文档用于同程用车与第三方接入涉及到的技术内容; 本文档涉及到的经纬度信息全部以高德地图为准. 

### 接入方式

全部接口(文档中回调接口除外)以 HTTP 方式提交 POST 请求。

### 编码

请求以及返回都使用 UTF-8 字符集进行编码

### 消息格式

 所有接口都为==Json==格式

### 域名

线上环境：car.17usoft.net/internalCarOpenApi/

顺风车测试环境：m.ly.com/car-test/stage/internalCarOpenApi/

### 公共请求参数

接口(文档中回调接口除外)分为两部分参数：

##### body 部分：

使用DES 加密消息体

使用DES 加密，密钥为同程提供的 secretKey。加密体是{body 内容}，body 里的内容为接口参数的 json。

==DES 加密规格:==

CipherMode.ECB;

PaddingMode.PKCS7

 ==字节编码方式:==

16 进制大写公式为：

body = DESEncrypt(request body 内容, secretKey)

例如：

{"cellphone": "18512345678"}

##### header 部分：

| 字段名称        | 类型   | 是否必须 | 说明                                                     |
| --------------- | ------ | -------- | -------------------------------------------------------- |
| distributorCode | String | 是       | 第三方服务 ID，由同程艺龙方提供                          |
| timestamp       | String | 是       | 秒级时间戳                                               |
| sign            | String | 是       | MD5（distributorCode + timestamp + 加密后  body 的长度） |

### 公共返回参数

| 字段名称 | 类型    | 说明     |
| -------- | ------- | -------- |
| code     | Integer | 返回编码 |
| message  | String  | 返回信息 |
| data     | Object  | 返回结果 |

### 其他说明

使用接口前，需要与同程艺龙运营方咨询并提供测试 ip，同程艺龙方需要将其加入白名单，同程艺龙方也会提供测试地址、测试 key 和第三方服务 ID。

同程侧提供demo。



# 基础数据API 接口

基础Api 接口包括航班动态查询、附近司机查询等接口。

## 1. 航班信息

###  1.1 根据航班号查询航班信息

###### 请求地址

api/flight/flightNoV2

###### 请求参数

| **字段名称** | **类型** | **是否必须** | **说明**                  |
| ------------ | -------- | ------------ | ------------------------- |
| flightNo     | String   | 是           | 航班号 例如：CZ6180       |
| flightDate   | String   | 是           | 航班日期 例如：2018-12-04 |

###### 返回参数

| 字段名称           | 类型   | 说明                               |
| ------------------ | ------ | ---------------------------------- |
| data               | List   | 航班信息列表                       |
| departureTime      | String | 出发时间                           |
| departureTerminal  | String | 出发航站楼中文                     |
| depLongitude       | String | 出发航站楼经度                     |
| departureAirport   | String | 出发机场三字码                     |
| arrGlobalFlag      | String | 达机场标识（ 0：国内、 1：国际）   |
| arrivalterminal    | String | 到达航站楼中文                     |
| arrCityName        | String | 到达城市                           |
| arrivalAirportCN   | String | 到达机场中文                       |
| arrCityId          | String | 到达城市id                         |
| depCityId          | String | 出发城市id                         |
| departureAirportCN | String | 出发机场中文                       |
| depGlobalFlag      | String | 出发机场标识 （0：国内、 1：国际） |
| airlineCnName      | String | 航司中文                           |
| flightNo           | String | 航班号                             |
| arrLongitude       | String | 到达航站楼经度                     |
| arrivalTime        | String | 到达时间                           |
| arrLatitude        | String | 到达航站楼纬度                     |
| depCityName        | String | 出发城市                           |
| flightDuration     | String | 飞行延迟（类似：1，2）             |
| arrivalAirport     | String | 到达机场三字码                     |
| depLatitude        | String | 出发航站楼纬度                     |
| status             | String | 航班计划状态                       |

### 1.2 根据起降地查询航班信息

###### 请求地址

api/flight/flightCity

###### 请求参数

| **字段名称**  | **类型** | **是否必须** | **说明**                         |
| ------------- | -------- | ------------ | -------------------------------- |
| depCityId     | String   | 是           | 起飞城市Id（同程侧）             |
| ariCityId     | String   | 是           | 到达城市Id（同程侧）             |
| depGlobalFlag | String   | 是           | 到达地国内外（0-国内、1-国际）   |
| arrGlobalFlag | String   | 是           | 出发地国内外（0-国内、1-国际）   |
| date          | Date     | 是           | 起飞时间（时间格式：yyyy-MM-dd） |

###### 返回参数

| 字段名称           | 类型    | 说明                               |
| ------------------ | ------- | ---------------------------------- |
| code               | Integer | 200                                |
| message            | String  | 返回信息                           |
| data               | List    | 航班信息列表                       |
| departureTime      | String  | 出发时间                           |
| departureTerminal  | String  | 出发航站楼中文                     |
| depLongitude       | String  | 出发航站楼经度                     |
| departureAirport   | String  | 出发机场三字码                     |
| arrGlobalFlag      | String  | 达机场标识（ 0：国内、 1：国际）   |
| arrivalterminal    | String  | 到达航站楼中文                     |
| arrCityName        | String  | 到达城市                           |
| arrivalAirportCN   | String  | 到达机场中文                       |
| arrCityId          | String  | 到达城市id                         |
| depCityId          | String  | 出发城市id                         |
| departureAirportCN | String  | 出发机场中文                       |
| depGlobalFlag      | String  | 出发机场标识 （0：国内、 1：国际） |
| airlineCnName      | String  | 航司中文                           |
| flightNo           | String  | 航班号                             |
| arrLongitude       | String  | 到达航站楼经度                     |
| arrivalTime        | String  | 到达时间                           |
| arrLatitude        | String  | 到达航站楼纬度                     |
| depCityName        | String  | 出发城市                           |
| flightDuration     | String  | 飞行延迟（类似：1，2）             |
| arrivalAirport     | String  | 到达机场三字码                     |
| depLatitude        | String  | 出发航站楼纬度                     |
| status             | String  | 航班计划状态                       |



# 顺风车API接口列表

## 1. 运力

### 1.1. 查价

根据起始点地址、终点地址、用车时间等信息查询顺风车报价信息

###### 请求地址

api/sfc/price/all

###### 请求参数

| 字段名称           | 类型    | 是否必须 | 说明                                                         |
| :----------------- | ------- | -------- | ------------------------------------------------------------ |
| useTime            | String  | 否       | 用车时间，为空时默认是最晚出发时间（格式：yyyy-MM-dd hh:mm:ss） |
| earliestTime       | String  | 是       | 最早出发时间（格式：yyyy-MM-dd hh:mm:ss）                    |
| latestTime         | String  | 是       | 最晚出发时间（格式：yyyy-MM-dd hh:mm:ss）                    |
| tcServiceTypeId    | Integer | 否       | 产品 id，参考【产品类型ID代码】                              |
| startCityId        | String  | 是       | 出发城市 id（同程侧）                                        |
| startCityName      | String  | 是       | 出发城市                                                     |
| startAddress       | String  | 是       | 上车地址                                                     |
| startAddressDetail | String  | 是       | 上车详细地址（可以与startAddress传的一致）                   |
| startLatitude      | String  | 是       | 上车纬度                                                     |
| startLongitude     | String  | 是       | 上车经度                                                     |
| startAdCode        | String  | 否       | 上车行政区划代码                                             |
| endCityId          | String  | 是       | 到达城市 id                                                  |
| endCityName        | String  | 是       | 到达城市                                                     |
| endAddress         | String  | 是       | 下车地址                                                     |
| endAddressDetail   | String  | 是       | 下车详细地址（可以与endAddress传的一致）                     |
| endLatitude        | String  | 是       | 下车纬度                                                     |
| endLongitude       | String  | 是       | 下车经度                                                     |
| endAdCode          | String  | 否       | 下车行政区划代码                                             |
| paxNum             | Integer | 是       | 乘客数量                                                     |
| acceptPool         | Integer | 是       | 是否接受拼车 (0独享，1拼车)                                  |
| crossCity          | boolean | 是       | 是否跨城                                                     |
| supplierCode       | String  | 否       | 供应商 code                                                  |

###### 返回参数

| 字段名称     | 类型                  | 说明       |
| :----------- | :-------------------- | ---------- |
| carPriceList | List<SfcCarPriceInfo> | 供应商编码 |

**SfcCarPriceInfo**

| 字段         | 类型               | 说明         |
| ------------ | ------------------ | ------------ |
| carTips      | String             | 提醒文案     |
| supplierCode | String             | 供应商编码   |
| name         | String             | 供应商名称   |
| icon         | String             | 供应商图标   |
| priceMark    | String             | 询价唯一标识 |
| priceList    | List<SfcPriceInfo> | 报价         |

**SfcPriceInfo**

| 字段名称   | 类型       | 说明                           |
| ---------- | ---------- | ------------------------------ |
| type       | String     | 类型                           |
| amount     | BigDecimal | 价格                           |
| acceptPool | Integer    | 是否接受拼车（1-拼车，0-独享） |

###### 请求参数示例

```json
{
    "useTime":"2021-03-26 15:10:00",
    "earliestTime":"2021-03-26 15:00:00",
    "latestTime":"2021-03-26 15:30:00",
    "startCityId":321,
    "startCityName":"上海",
    "startAddress":"上海虹桥站",
    "startAddressDetail":"上海虹桥站",
    "startLatitude":"31.195165",
    "startLongitude":"121.320898",
    "startAdCode":"",
    "endCityId":321,
    "endCityName":"上海",
    "endAddress":"上海迪士尼乐园",
    "endAddressDetail":"上海迪士尼乐园",
    "endLatitude":"31.152963",
    "endLongitude":"121.675821",
    "endAdCode":"",
    "paxNum":1,
    "acceptPool":1,
    "tcServiceTypeId":14,
    "crossCity":false,
    "supplierCode":"HelloBike"
}
```

###### 返回结果示例

```json
{
    "code":200,
    "message":"请求成功",
    "data":{
        "carPriceList":[
            {
                "carTips":"最多乘坐4人",
                "supplierCode":"HelloBike",
                "name":"哈啰顺风车",
                "icon":"https://pic5.40017.cn/i/ori/VEraE1QH7O.jpg",
                "priceMark":"3c5b314a252f455196a6a1540d40cc32",
                "priceList":[
                    {
                        "type":"拼车",
                        "amount":164,
                        "acceptPool":1
                    },
                    {
                        "type":"独享",
                        "amount":229,
                        "acceptPool":0
                    }
                ]
            }
        ]
    }
}
```



### 1.2. 城市服务开通查询

###### 请求地址

sfc/price/getAllCityList

###### 请求参数

| 字段名称     | 类型   | 是否必须 | 说明       |
| ------------ | ------ | -------- | ---------- |
| supplierCode | String | 是       | 服务商code |

###### 返回参数

| 字段名称 | 类型           | 说明           |
| -------- | -------------- | -------------- |
| code     | Integer        |                |
| message  | String         |                |
| data     | List<CityInfo> | 开通的城市列表 |

**CityInfo**

| 字段名称     | 类型    | 说明       |
| ------------ | ------- | ---------- |
| cityId       | Integer | 城市id     |
| cityName     | String  | 城市名称   |
| supplierCode | String  | 供应商code |

###### 请求参数示例

```
{
	"supplierCode":"HelloBike"
}
```



###### 返回参数示例

```json
{
    "code":200,
    "message":"请求成功",
    "data":[
        {
            "cityId":226,
            "cityName":"苏州",
            "supplierCode":"HelloBike"
        },
        {
            "cityId":229,
            "cityName":"无锡",
            "supplierCode":"HelloBike"
        },
        {
            "cityId":321,
            "cityName":"上海",
            "supplierCode":"HelloBike"
        }
    ]
}
```



## 2. 订单

### 2.1. 创单

根据询价返回的priceMark创单

###### 请求地址

api/sfc/order/create

###### 请求参数

| 字段名称           | 类型       | 是否必须 | 说明                                          |
| ------------------ | ---------- | -------- | --------------------------------------------- |
| distributorOrderId | String     | 是       | 分销侧订单号                                  |
| priceMark          | String     | 是       | 询价的唯一标识（15分钟的时效性）              |
| passengerName      | String     | 是       | 乘客姓名                                      |
| passengerCellphone | String     | 是       | 乘客手机号码                                  |
| leaveMessage       | String     | 否       | 留言（参考【留言枚举值】）                    |
| supplierCode       | String     | 是       | 供应商编码                                    |
| acceptPool         | Integer    | 是       | 是否接受拼车 (0独享，1拼车)，默认：1-接受拼车 |
| thanksFee          | BigDecimal | 否       | 感谢费，单位：元                              |

###### 返回参数

| 字段名称      | 类型       | 说明                                                   |
| ------------- | ---------- | ------------------------------------------------------ |
| orderId       | String     | 订单id                                                 |
| totalAmount   | BigDecimal | 订单支付金额（可能与查价返回不一致）                   |
| thanksFeeFlag | Integer    | 感谢费调用状态标志位(0:未调用，1:调用成功，2:调用失败) |

###### 请求参数示例

```json
{
    "distributorOrderId":"20210329001",
    "priceMark":"4581ab4e40724532bac3d4e7715d08f4",
    "leaveMessage":"有一个大件行李箱",
    "passengerName":"周先生",
    "passengerCellphone":"15995433552",
    "acceptPool":1,
    "supplierCode":"HelloBike"
}
```

###### 返回结果示例

```
{
    "code":200,
    "message":"请求成功",
    "data":{
        "orderId":"SFC6065876A206A26BC83",
        "totalAmount":82
        "thanksFeeFlag":0
    }
}
```



### 2.2. 订单详情

根据订单号获取订单详情

###### 请求地址

/api/sfc/order/detail

###### 请求参数

| 字段名称 | 类型   | 是否必须 | 说明       |
| -------- | ------ | -------- | ---------- |
| orderId  | String | 是       | 同程订单号 |

###### 返回参数

| 字段名称            | 类型                | 说明                                |
| ------------------- | ------------------- | ----------------------------------- |
| orderId             | String              | 同程订单id                          |
| orderStatus         | Integer             | 订单状态                            |
| orderStatusName     | String              | 订单状态描述                        |
| carpoolStatus       | Integer             | 拼车状态，参考【拼车状态码】        |
| useTime             | String              | 用车时间（yyyy-MM-dd HH:mm:ss）     |
| earliestTime        | String              | 最早出发时间（yyyy-MM-dd HH:mm:ss） |
| latestTime          | String              | 最晚出发时间（yyyy-MM-dd HH:mm:ss） |
| productId           | Integer             | 产品id                              |
| productName         | String              | 产品名称                            |
| supplierCode        | String              | 服务商code                          |
| supplierName        | String              | 服务商名称                          |
| createTime          | String              | 创单时间                            |
| totalAmount         | BigDecimal          | 订单总费用，单位：元                |
| lostAmount          | BigDecimal          | 有损金额，单位：元                  |
| carpoolReturnAmount | BigDecimal          | 拼车成功，退差价。单位：元          |
| thanksFee           | BigDecimal          | 感谢费，单位：元                    |
| crossCity           | boolean             | 是否跨城                            |
| passengerNumber     | Integer             | 乘客人数                            |
| passengerName       | String              | 乘客姓名                            |
| passengerCellphone  | String              | 乘客联系手机号                      |
| distance            | Integer             | 预估距离，单位：米                  |
| duration            | Integer             | 预估时间，单位：分钟                |
| addressInfo         | SfcOrderAddressInfo | 用车地址信息                        |
| driverInfo          | SfcOrderDriverInfo  | 司机信息                            |
| cancelReason        | String              | 取消原因                            |

**SfcOrderAddressInfo**

| 字段名称           | 类型    | 说明           |
| ------------------ | ------- | -------------- |
| startCityId        | Integer | 出发地城市id   |
| startCityName      | String  | 出发地城市名称 |
| startAddress       | String  | 出发地地址     |
| startAddressDetail | String  | 出发地详细地址 |
| startLatitude      | String  | 出发地维度     |
| startLongitude     | String  | 出发地经度     |
| endCityId          | Integer | 目的地城市id   |
| endCityName        | String  | 目的地城市名称 |
| endAddress         | String  | 目的地地址     |
| endAddressDetail   | String  | 目的地详细地址 |
| endLatitude        | String  | 目的地维度     |
| endLongitude       | String  | 目的地经度     |

**SfcOrderDriverInfo**

| 字段名称        | 类型   | 说明       |
| --------------- | ------ | ---------- |
| plateNumber     | String | 司机车牌号 |
| carColor        | String | 车辆颜色   |
| driverName      | String | 司机名字   |
| driverCellphone | String | 司机电话   |
| carBrand        | String | 车辆品牌   |
| virtualPhone    | String | 司机虚拟号 |

###### 请求参数示例

```
{
    "orderId":"SFC606538171069FUA37C",
}
```

###### 返回参数示例

```json
{
    "code":200,
    "message":"请求成功",
    "data":{
        "orderId":"SFC60779F4B000A5UCU91",
        "orderStatus":1000,
        "orderStatusName":"取消",
        "useTime":"2021-04-16 11:00:00",
        "earliestTime":"2021-04-16 10:00:00",
        "latestTime":"2021-04-16 11:00:00",
        "productId":19,
        "productName":"即时专车",
        "supplierCode":"HelloBike",
        "supplierName":"哈啰顺风车",
        "createTime":"2021-04-15 10:04:59",
        "driverInfo":{
            "plateNumber":"",
            "carColor":"",
            "driverName":"",
            "driverCellphone":"",
            "virtualPhone":"",
            "carBrand":""
        },
        "totalAmount":8,
        "lostAmount":0,
        "carpoolReturnAmount":0,
        "crossCity":false,
        "passengerNumber":1,
        "passengerName":"先生女士",
        "passengerCellphone":"18662683818",
        "addressInfo":{
            "startCityId":226,
            "startCityName":"苏州",
            "startAddress":"苏州园区站",
            "startAddressDetail":"江苏省苏州市吴中区瑞新路与玲珑街交汇处",
            "startLatitude":"31.3410960",
            "startLongitude":"120.7105740",
            "endCityId":226,
            "endCityName":"苏州",
            "endAddress":"华池街北(公交站)",
            "endAddressDetail":"江苏省苏州市吴中区苏州工业园区直属镇苏虹中路华奕天合工业坊",
            "endLatitude":"31.3348440",
            "endLongitude":"120.7132420"
        },
        "distance":0,
        "duration":5,
        "cancelReason":"未知原因",
        "carpoolStatus":0
    }
}
```



### 2.3. 乘客确认

乘客确认上车、到达

###### 请求地址

api/sfc/order/passengerConfirm

###### 请求参数

| 字段名称  | 类型   | 是否必须 | 说明                                                 |
| --------- | ------ | -------- | ---------------------------------------------------- |
| orderId   | String | 是       | 同程订单号                                           |
| event     | String | 是       | 确认事件（ON_CAR：确认上车，ARRIVE：确认到达目的地） |
| longitude | String | 否       | 乘客当前的经度                                       |
| latitude  | String | 否       | 乘客当前的纬度                                       |

###### 返回参数

| 字段    | 类型    | 说明                      |
| ------- | ------- | ------------------------- |
| code    | Integer | 响应代码（200：请求成功） |
| message | String  | 响应消息                  |

###### 请求参数示例

```
{
    "orderId": "SFC5FB33C45303DDAAC11",
    "event": "ON_CAR"
}
```

###### 返回参数示例

```
{
    "code":200,
    "message":"请求成功"
}
```

### 2.4. 支付通知

###### 请求地址

api/sfc/order/payCallBack

###### 请求参数

| 字段名称  | 类型       | 是否必须 | 说明       |
| --------- | ---------- | -------- | ---------- |
| orderId   | String     | 是       | 同程订单号 |
| payAmount | BigDecimal | 是       | 支付金额   |

###### 返回参数

| 字段名称 | 类型    | 说明           |
| -------- | ------- | -------------- |
| data     | boolean | true(请求成功) |

###### 请求参数示例

```
{
    "orderId": "SFC5FB33C45303DDAAC11",
    "payAmount": 120.35
}
```

###### 返回参数示例

```
{
    "code":200,
    "message":"请求成功",
    "data":true
}
```

### 2.5. 取消订单

###### 请求地址

api/sfc/order/cancel

###### 请求参数

| 字段名称 | 类型   | 是否必须 | 说明       |
| -------- | ------ | -------- | ---------- |
| orderId  | String | 是       | 同程订单号 |
| reason   | String | 是       | 取消原因   |

###### 返回参数

| 字段名称 | 类型    | 说明                      |
| -------- | ------- | ------------------------- |
| code     | Integer | 响应代码（200：请求成功） |
| message  | String  | 响应消息                  |

###### 请求参数示例

```json
{
    "orderId": "SFC5FB33C45303DDAAC11",
    "reason": "用户取消，我改变了行程"
}
```

###### 返回参数示例

```json
{
    "code":200,
    "message":"请求成功"
}
```

### 2.6. 退款

###### 请求地址

api/sfc/order/refund



###### 请求参数

| 字段名称           | 类型       | 是否必须 | 说明                              |
| ------------------ | ---------- | -------- | --------------------------------- |
| orderId            | String     | 是       | 订单号（同程侧）                  |
| refundType         | Integer    | 是       | 退款类型（1-8），参考【退款类型】 |
| refundMoney        | BigDecimal | 是       | 退款金额（单位：元）              |
| remark             | string     | 是       | 退款原因                          |
| multipleRefund     | boolean    | 否       | 是否多次退款，默认false           |
| deductSameAsRefund | boolean    | 否       | 是否扣司机同样的钱，默认false     |

###### 返回参数

| 字段名称 | 类型    | 说明                      |
| -------- | ------- | ------------------------- |
| code     | Integer | 响应代码（200：请求成功） |
| message  | string  | 响应消息                  |

###### 请求参数示例

###### 返回参数示例

```
{
    "code":200,
    "message":"请求成功"
}
```



## 3 回调API接口列表

### 3.1 订单状态通知

回调各种状态给分销侧（HTTP POST 形式），接入方调取订单详情接口获取相关信息和订单状态值

###### 请求参数

| 字段名称    | 类型    | 说明                                                         |
| ----------- | ------- | ------------------------------------------------------------ |
| tcOrderId   | String  | 同程订单号                                                   |
| orderStatus | String  | 回调状态，参考【回调通知状态码】                             |
| cancelType  | Integer | 1:用户取消,2:平台取消,3:供应商取消,4:分销侧取消,5:车主取消     [注：orderStatus等于1000时才有值，默认为空] |
| sign        | String  | 签名 MD5((tcOrderId+secretKey).toUpperCase) md5 是小写       |

###### 请求参数示例

```json
{
    "tcOrderId":"YNC5B447U5130125B4UU2",
    "orderStatus":"1000",
    "cancelType":1,
    "sign":"234234233242234"
}
```

######  返回结果示例

```json
{
    "success":true,
    "errorCode":"",
    "errorMessage":"",
    "data":true
}
```

注意：

当合作方返回如上 Json，同程方认为回调成功，如果合作方返回失败，同程方重试 3 次停止

### 3.2 虚拟号推送

回调虚拟号给分销侧（HTTP POST 形式）

###### 请求参数

| 字段名称         | 类型   | 说明                                                   |
| ---------------- | ------ | ------------------------------------------------------ |
| tcOrderId        | String | 同程订单号                                             |
| virtualCellphone | String | 虚拟号                                                 |
| supplierCode     | String | 供应商code                                             |
| sign             | String | 签名 MD5((tcOrderId+secretKey).toUpperCase) md5 是小写 |

###### 请求参数示例

```json
{
    "tcOrderId":"SFC61FCUF50202D21A96U",
    "virtualCellphone":"17151130258",
    "supplierCode":"DidaPinChe",
    "sign":"234234233242234"
}
```

######  返回结果示例

```json
{
    "success":true,
    "errorCode":"",
    "errorMessage":"",
    "data":true
}
```

## 4. 枚举

### 4.1. 产品类型ID代码

| 产品id | 说明       |
| ------ | ---------- |
| 12     | 顺风车接机 |
| 13     | 顺风车送机 |
| 14     | 顺风车接站 |
| 15     | 顺风车送站 |
| 11     | 顺风车打车 |

### 4.2. 错误码

| 错误码 | 说明                   |
| ------ | ---------------------- |
| 200    | 请求成功               |
| 400    | 参数存在问题           |
| 401    | 验签失败               |
| 500    | 服务器内部异常         |
| 501    | 请求失败：业务错误信息 |

### 4.3 回调通知类型

| 事件编码 | 说明             |
| -------- | ---------------- |
| 10       | 感谢费调用失败   |
| 20       | 司机已接单       |
| 100      | 司机已到达出发地 |
| 1000     | 订单取消         |
| 400      | 已结算           |
| 350      | 司机到达目的地   |

### 4.4 订单状态码

| 状态码 |                          |
| ------ | ------------------------ |
| 0      | 订单创建成功             |
| 10     | 已派单                   |
| 20     | 司机已接单               |
| 100    | 司机已到达出发地         |
| 200    | 服务开始（乘客确认上车） |
| 300    | 服务结束（乘客确认下车） |
| 1000   | 订单已取消               |

### 4.5 留言枚举值

| 枚举值                         |
| ------------------------------ |
| 有孕妇/老人                    |
| 需要儿童座椅                   |
| 有宠物                         |
| 有大件行李                     |
| 我会佩戴口罩                   |
| 请走高速，费用我承担           |
| 不承担高速费、过桥费等额外费用 |

### 4.6 退款类型

| 类型编码 | 描述                       |
| -------- | -------------------------- |
| 1        | 半路下车协商一致，部分退   |
| 2        | 半路下车协商一致，全额退款 |
| 3        | 车主额外接人               |
| 4        | 乘客未坐车误点击上车       |
| 5        | 乘客未坐车系统自动完成     |
| 6        | 车主诈骗                   |
| 7        | 线下收费退还加价款         |
| 8        | 其他原因                   |

### 4.7 取消原因

| 描述                     | 类型       |
| ------------------------ | ---------- |
| 我临时行程有变，无法出行 | 乘客       |
| 我选择了其他交通方式     | 乘客       |
| 信息填写有误，重新叫车   | 乘客       |
| 其他                     | 乘客       |
| 没有车主接单             | 和平台有关 |
| 不了解，只是想看看       | 和平台有关 |
| 其他平台价格更低         | 和平台有关 |
| 想去别的平台再看看       | 和平台有关 |

### 4.8 拼车状态码

| 状态码 | 描述       |
| ------ | ---------- |
| 0      | 选择不拼车 |
| 1      | 选择拼车   |
| 2      | 拼车失败   |
| 3      | 拼车成功   |

