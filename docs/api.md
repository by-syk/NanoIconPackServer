# API 一览


> 该文档未经严格校对，如有疑问或发现问题请[提交 issue](https://github.com/by-syk/NanoIconPackServer/issues) 或[给我发送邮件](mailto:By_syk@163.com)。


#### 接口清单

+ [申请适配图标](#申请适配图标)

  POST `/req/:iconpack`

+ [查询对目标 APP 的请求适配次数](#查询对目标%20APP%20的请求适配次数)

  GET `/req/:iconpack/:pkg`

+ [查询请求数 TOP 的 APP](#查询请求数%20TOP%20的%20APP)

  GET `/reqtop2/:iconpack/:user`

+ [在已标记的 APP 中查询请求数 TOP 的 APP](#在已标记的%20APP%20中查询请求数%20TOP%20的%20APP)

  GET `/reqtopfiltered2/:iconpack/:user`

+ [对申请适配的 APP 标记已处理](#对申请适配的%20APP%20标记已处理)

  POST `/reqfilter/:iconpack/:user`

+ [对申请适配的 APP 标记未处理](#对申请适配的%20APP%20标记未处理)

  DELETE `/reqfilter/:iconpack/:user`

+ [根据包名、APP 中英文名查询 APP 代码](#根据包名、APP%20中英文名查询%20APP%20代码)

  GET `/code/:keyword`

+ [根据包名+启动项查询 APP 代码](#根据包名+启动项查询%20APP%20代码)
  
  GET `/code/:pkg/:launcher`

+ [查询接入的图标包总数](#查询接入的图标包总数)

  GET `/sum/iconpack`

+ [查询申请的 APP 总数](#查询申请的%20APP%20总数)

  GET `/sum/app`

+ [查询申请总次数](#查询申请总次数)

  GET `/sum/req`

+ [统计各图标包最近一月申请用户数和申请次数](#统计各图标包最近一月申请用户数和申请次数)

  GET `/stats/month`

+ [统计目标图标包周申请趋势](#统计目标图标包周申请趋势)

  GET `/trend/week/:iconpack`

+ [获取所有图标包](#获取所有图标包)

  GET `/iconpacks`

+ [获取各系统常用 APP 代码（如电话、信息、相机等）](#获取各系统常用%20APP%20代码（如电话、信息、相机等）)

  GET `/base`

+ [添加赞助记录](#添加赞助记录)

  POST `/donate/:iconpack/:user`

+ [删除赞助记录](#删除赞助记录)

  DELETE `/donate`

+ [获取目标图标包的赞助记录](#获取目标图标包的赞助记录)

  GET `/donate/:iconpack/:user`


#### 申请适配图标
POST `/req/:iconpack`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 来源图标包包名 | `com.atony.iconpack.originalwish` |

| BODY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `label` | string | 否 | | 目标 APP 名 | `快图浏览` |
| `labelEn` | string | 否 | | 目标 APP 英文名 | `QuickPic` |
| `pkg` | string | 是 | | 目标 APP 包名 | `com.alensw.PicFolder` |
| `launcher` | string | 是 | | 目标 APP 启动项 | `com.alensw.PicFolder.GalleryActivity` |
| `sysApp` | boolean | 否 | `false` | 是否为系统 APP（无为假） | `true` |
| `deviceId` | string | 是 | | 申请设备 ID | `caaf8f51e0be8a719128c4b6` |
| `deviceBrand` | string | 否 | | 申请设备品牌 | `google` |
| `deviceModel` | string | 否 | | 申请设备型号 | `Pixel` |
| `deviceSdk` | number | 否 | | 申请设备系统版本 | `25` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": 15 // 在该图标包中该 APP 累积申请15次
}
```


#### 查询对目标 APP 的请求适配次数
GET `/req/:iconpack/:pkg`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 所属图标包包名 | `com.atony.iconpack.originalwish` |
| `pkg` | string | 是 | | 目标 APP 包名 | `com.alensw.PicFolder` |

| QUERY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `deviceid` | string | 否 | | 目标设备 ID（有则同时判断该设备是否申请过该 APP） | `caaf8f51e0be8a719128c4b6` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": {
        "num": 13,
        "reqed": 1 // 该设备已申请过该 APP
    }
}
```


#### 查询请求数 TOP 的 APP
GET `/reqtop2/:iconpack/:user`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名 | `com.atony.iconpack.originalwish` |
| `user` | string | 是 | | 用户口令 | `peter` |

| QUERY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `limit` | number | 否 | `32` | 返回记录数（最大 128）| `2` |
| `filter` | boolean| 否 | `false` | 过滤掉已标记的 APP | `false` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "录音录屏",
            "pkg": "org.lineageos.recorder",
            "launcher": "org.lineageos.recorder.RecorderActivity",
            "sum": 308,
            "filter": 0 // 该 APP 未被标记
        },
        {
            "label": "Dir",
            "pkg": "kh.android.dir",
            "launcher": "kh.android.dir.ui.activities.MDMainActivity",
            "sum": 237,
            "filter": 1 // 该 APP 已被标记
        }
    ]
}
```


#### 在已标记的 APP 中查询请求数 TOP 的 APP
GET `/reqtopfiltered2/:iconpack/:user`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名 | `com.atony.iconpack.originalwish` |
| `user` | string | 是 | | 用户口令 | `peter` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "Dir",
            "pkg": "kh.android.dir",
            "launcher": "kh.android.dir.ui.activities.MDMainActivity",
            "sum": 237,
            "filter": 1
        },
        {
            "label": "便签",
            "pkg": "com.oneplus.note",
            "launcher": "com.oneplus.note.ui.MainActivity",
            "sum": 122,
            "filter": 1
        }
    ]
}
```


#### 对申请适配的 APP 标记已处理
POST `/reqfilter/:iconpack/:user`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名 | `com.atony.iconpack.originalwish` |
| `user` | string | 是 | | 用户口令 | `peter` |

| BODY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `pkg` | string | 是 | | 目标 APP 包名 | `com.alensw.PicFolder` |
| `launcher` | string | 否 | | 目标 APP 启动项 | `com.alensw.PicFolder.GalleryActivity` |

返回结果例：
```
{
    "status": 0,
    "msg": "success"
}
```


#### 对申请适配的 APP 标记未处理
DELETE `/reqfilter/:iconpack/:user`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名 | `com.atony.iconpack.originalwish` |
| `user` | string | 是 | | 用户口令 | `peter` |

| QUERY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `pkg` | string | 是 | | 目标 APP 包名 | `com.alensw.PicFolder` |
| `launcher` | string | 否 | | 目标 APP 启动项 | `com.alensw.PicFolder.GalleryActivity` |

返回结果例：
```
{
    "status": 0,
    "msg": "success"
}
```


#### 根据包名、APP 中英文名查询 APP 代码
GET `/code/:keyword`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `keyword` | string | 是 | | 关键字（包名或 APP 名） | `快图浏览` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "快图浏览",
            "labelEn": "QuickPic",
            "pkg": "com.alensw.PicFolder",
            "launcher": "com.alensw.PicFolder.GalleryActivity",
            "sum": 175 // 在所有图标包中该 APP 累积申请175次
        }
    ]
}
```


#### 根据包名+启动项查询 APP 代码
GET `/code/:pkg/:launcher`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `pkg` | string | 是 | | 目标 APP 包名 | `com.alensw.PicFolder` |
| `launcher` | string | 是 | | 目标 APP 启动项 | `com.alensw.PicFolder.GalleryActivity` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "快图浏览",
            "labelEn": "QuickPic",
            "pkg": "com.alensw.PicFolder",
            "launcher": "com.alensw.PicFolder.GalleryActivity"
        },
        {
            "label": "快圖瀏覽",
            "labelEn": "QuickPic",
            "pkg": "com.alensw.PicFolder",
            "launcher": "com.alensw.PicFolder.GalleryActivity"
        }
    ]
}
```


#### 查询接入的图标包总数
GET `/sum/iconpack`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": 63
}
```


#### 查询申请的 APP 总数
GET `/sum/app`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": 69907
}
```


#### 查询申请总次数
GET `/sum/req`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": 2117134
}
```


#### 统计各图标包最近一月申请用户数和申请次数
GET `/stats/month`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "Pure",
            "pkg": "me.morirain.dev.iconpack.pure",
            "reqs": 9764, // 最近一个月该图标包收到9764次申请
            "users": 1493 // 最近一个月该图标包收到1493个用户的申请
        },
        {
            "label": "Nokia Evolve+",
            "pkg": "com.wanmonstar.nokiaevolveplus",
            "reqs": 5792,
            "users": 411
        },
        {
            "label": "line",
            "pkg": "com.crazypig321.lineicons2",
            "reqs": 3226,
            "users": 325
        }
    ]
}
```


#### 统计目标图标包周申请趋势
GET `/trend/week/:iconpack`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名或 APP 名 | `com.atony.iconpack.originalwish` |

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": {
        "label": "OriginalWish",
        "pkg": "com.atony.iconpack.originalwish",
        "weeks": [
            {
                "year": "2018",
                "week": "04", // 2018年第4周
                "reqs": 103, // 本周该图标包收到9764次申请
                "users": 12 // 本周该图标包收到12个用户的申请
            },
            {
                "year": "2018",
                "week": "05",
                "reqs": 77,
                "users": 13
            }
        ]
    }
}
```


#### 获取所有图标包
GET `/iconpacks`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "label": "Pure",
            "pkg": "me.morirain.dev.iconpack.pure"
        },
        {
            "label": "OriginalWish",
            "pkg": "com.atony.iconpack.originalwish"
        },
        {
            "label": "PDC",
            "pkg": "com.pandecheng.iconpack"
        }
    ]
}
```


#### 获取各系统常用 APP 代码（如电话、信息、相机等）
GET `/base`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "icon": "browser",
            "label": "浏览器",
            "labelEn": "Browser",
            "more": [
                {
                    "pkg": "cn.nubia.browser",
                    "launcher": "com.android.browser.BrowserLauncher",
                    "brand": "nubia"
                },
                {
                    "pkg": "com.android.browser",
                    "launcher": "com.android.browser.BrowserActivity",
                    "brand": "Xiaomi"
                }
            ]
        },
        {
            "icon": "calculator",
            "label": "计算器",
            "labelEn": "Calculator",
            "more": [
                {
                    "pkg": "com.android.calculator2",
                    "launcher": "com.android.calculator2.Calculator",
                    "brand": "google"
                }
            ]
        }
    ]
}
```


#### 添加赞助记录
POST `/donate/:iconpack/:user`

| PATH 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `iconpack` | string | 是 | | 目标图标包包名 | `com.atony.iconpack.originalwish` |
| `user` | string | 是 | | 用户口令 | `peter` |

| BODY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `money` | number | 是 | | 金额（低于￥10000） | `10` |
| `donator` | string | 否 | | 赞助者 | `coolapk@By_syk` |
| `comment` | string | 否 | | 备注 | `老用户` |
| `date` | string | 否 | | 日期 | `2018-02-01` |

返回结果例：
```
{
    "status": 0,
    "msg": "success"
}
```


#### 删除赞助记录
DELETE `/donate`

| QUERY 参数 | 类型 | 必须 | 默认值 | 描述 | 例 |
| :---- | :---- | :---- | :---- | :---- | :--- |
| `id` | string | 是 | | 赞助记录 ID | `1e2c2e1b-19dc-4a56-ac75-f9c8858d` |

返回结果例：
```
{
    "status": 0,
    "msg": "success"
}
```


#### 获取目标图标包的赞助记录
GET `/donate/:iconpack/:user`

返回结果例：
```
{
    "status": 0,
    "msg": "success",
    "result": [
        {
            "id": "1e2c2e1b-19dc-4a56-ac75-f9c8858d",
            "money": 10,
            "donator": "coolapk@By_syk",
            "comment": "老用户",
            "date": "20180201"
        }
    ]
}
```
