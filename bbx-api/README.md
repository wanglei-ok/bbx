# BBX REST API  
- **Public API**  
	- **Login**
		- [1.注册](#注册)
		- [2.密码登录](#密码登录)
		- [3.短信验证码登录](#短信验证码登录)
		- [4.重置密码](#重置密码)
	- **Bet** 
		- [5.当前投注期数](#当前投注期数)
		- [6.平台已产生游戏场次](#平台已产生游戏场次)
		- [7.已开奖场次信息](#已开奖场次信息)
		- [8.排行榜](#排行榜)
		- [9.上次预测结果](#上次预测结果)
		- [10.当前投注期数信息](#当前投注期数信息)
	- **Public**
		- [11.矿工费获取](#矿工费获取)
		- [12.获取SMS验证码](#获取sms验证码)  
	    - [13.多语言支持](#多语言支持)
		- [14.通知、公告、活动](#通知公告活动)
- **AccessToken API**  
	- **Login**
		- [15.注销](#注销)
	- **Bet** 
		- [16.一维空间快捷投注](#一维空间快捷投注)
		- [17.三维空间快捷投注](#三维空间快捷投注)
		- [18.投注](#投注)
	- **Assets**
		- [19.个人资产](#个人资产)
		- [20.个人充币地址获取](#个人充币地址获取)
		- [21.充币记录列表获取](#充币记录列表获取)
		- [22.可用余额获取](#可用余额获取)
		- [23.提币记录列表获取](#提币记录列表获取)
		- [24.提币](#提币)
		- [25.指定币种充提币记录列表获取](#指定币种充提币记录列表获取)
	- **Personal**
		- [26.会员信息展示](#会员信息展示)
		- [27.会员信息编辑](#会员信息编辑)
		- [28.阅读完成奖励](#阅读完成奖励)
		- [29.登录密码设置接口](#登录密码设置接口)
		- [30.资金密码设置接口](#资金密码设置接口)
		- [31.谷歌验证设置接口](#谷歌验证设置接口)
		- [32.手势密码设置接口](#手势密码设置接口)
		- [33.登录验证设置接口](#登录验证设置接口)
		- [34.提现验证设置接口](#提现验证设置接口)
		- [35.交易验证设置接口](#交易验证设置接口)
		- [36.用户参与游戏列表](#用户参与游戏列表)
		- [37.用户参与游戏详情](#用户参与游戏详情)
		- [38.合伙人邀请码获取](#合伙人邀请码获取)
		- [39.签到状态获取](#签到状态获取)
		- [40.签到](#签到)
		- [41.任务列表](#任务列表)
		- [42.用户游戏数据统计](#用户游戏数据统计)
- **待解决问题**
	- [通过什么方式发送token？](#通过什么方式发送token)
	- [采用什么方式处理多语言问题？](#采用什么方式处理多语言问题)
	

## 术语
- **Public API**        不需要登录，可直接访问的API
- **AccessToken API**   需要登录后才能使用的API，否则收到"No access"错误消息
- **Login**             登录/注册 API
- **Bet**               投注模块 API 
- **Assets**            资产模块 API
- **Personal**          个人模块 API
- **Public**            公共模块 API

## 多语言支持
由于APP采用多语言设计，因此接口也必须考虑对多语言的支持，  
以下是我们建议的方案，最终方案协商确定。

#### 采用什么方式处理多语言问题
1. API接口统一使用英文，由客户端语言包负责语言的本地化显示处理。
	- 优点：标准化方法，低耦合，不易出现问题。
	- 缺点：客户端需要做语言包的在线升级，否则就需要客户端升级来配合新增字符串的多语言处理。
	
2. API接口返回的数据直接用于显示，服务端支持语言切换，返回数据中直接做好语言处理。
	- 优点：简单粗暴
	- 缺点：接口业务复杂，性能较低

建议采用方案一
- API接口统一使用英文
- 服务端提供翻译接口，对新增消息正确显示提供支持
- 客户端使用本地缓存数据，先查缓存，后后接口，接口查询结果加入本地缓存，从而提高效率。
- 客户端安装程序包含最新的语言包，减少翻译接口查询压力

### GET /v1/public/tr
对于英文字符串进行翻译处理
  
请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| locale | true | string | 要翻译的语言标识，符合i18n标准，目前只支持英文到中文的翻译 | zh | zh |
| text | true | string | 待翻译的文本 |  |  |

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| data | true     | string   | 译文 | |

```json
/* GET /v1/public/tr?locale=zh&text=deposit */
{
	"status": "ok",
	"data": "提币"
}

/* GET /v1/public/tr?locale=zh&text=deposit */
{
  "status": "error",
  "err-msg": "error message"
}
```

## AccessToken    
所有需要认证访问的API接口需要增加token参数，之后在接口描述中不再累赘

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| token | false | string | 认证成功后返回的token字符串，所有需要登录后使用的API需要在请求中包含此参数 | 0 | |

#### 通过什么方式发送token
1. 在请求头中，新定义一个标记
2. 在请求参数中，GET跟在URL里，POST跟在请求对象中的一个属性

## ResultObject
所有请求结果由结果对象形式返回，之后在接口描述中不再累赘

请求参数:  

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| status         | true     | string   | 应答状态                                   | "ok","error" |
| data         | false     | json   | 当status == "ok"，data中包含应答数据对象，具请求不同对象不同   | |
| err-msg         | false     | string   | 当status == "error"，data 中包含错误信息字符串   | |

```json
/* GET /v1/assets/address?currency=ETH */
{
	"status": "ok",
	"data": {
		"address": "0xE36a16E59B834BF834A3B1eEB86E911490050737",
		"tag": "101264005",
	}
}

/* GET /v1/assets/address?currency=ETH */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 分页器
获取大量数据列表的接口支持分页请求

### 请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| offset         | false     | number   | 分页查询起始   | 0 | |
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |

### 响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| total         | true     | number   | 总记录数                                   | |
| count         | true     | number   | data中的对象数量  | |
| list         | true     | array   | json数组，包含实际数据对象   | |

```json
/* GET /v1/assets/deposit?currency=ETH&offset=20&limit=10 */
{
	"status": "ok",
	"data": {
		"total": 22,
		"count": 2,
		"list": [{
			"txhash": "0x5a795770f8af9690eb38c45ea3d089ea7e74544063029f18b10a7b7951ae789b",
			"amount": "0.014",
			"symbol": "ETH",
			"ts": 1543819612,
			"state": "2/6"
		}, {
			"txhash": "0xe703437c16c71f9072d75762d42df4c9a9fc147d2fa7873aecba79e697cb7cb4",
			"amount": "0.016",
			"symbol": "ETH",
			"ts": 1541227612,
			"state": "succ"
		}]
	}
}

```

## 注册  
### POST /v1/login/register 

请求参数：

| 参数名称   | 是否必须 | 类型   | 描述                               | 取值范围 |
| ---------- | -------- | ------ | ---------------------------------- | -------- |
| username   | true     | string | 用户名（现阶段仅支持手机号码注册） |          |
| password   | true     | string | 输入的密码                         |          |
| rePassword | true     | string | 重复输入密码                       |          |
| sms        | true     | string | 短信验证码                         |          |
| capCode    | true     | string | 图形验证码                         |          |
| inviteCode | false    | string | 邀请码                             |          |

响应数据：

（无）

请求响应示例：

```json
/* POST /v1/login/register
{
	"username": "1339991111",
	"password": "123456",
	"rePassword": "123456",
	"sms": "1234",
	"capCode": "eef2",
	"inviteCode": "2afees"
}
*/
{
    "status": "ok"
}

/* POST /v1/login/register */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 密码登录  
### POST /v1/login/userLogin 

请求参数:

| 参数名称 | 是否必须 | 类型   | 描述       | 取值范围 |
| -------- | -------- | ------ | ---------- | -------- |
| mobile   | true     | string | 手机号码   |          |
| sms      | true     | string | 短信验证码 |          |
| capCode  | true     | string | 图形验证码 |          |

响应数据:

| 参数名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
| -------- | -------- | -------- | ---- | -------- |
| token    | true     | string   |      |          |

请求响应示例:

```json
/* POST /v1/login/userlogin*/
{
    "status": "ok",
    "data": {
    	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"    
    }
}

/* POST /v1/login/userlogin */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 短信验证码登录  
### POST /v1/login/mobileLogin 

请求参数:

| 参数名称 | 是否必须 | 类型   | 描述       | 取值范围 |
| -------- | -------- | ------ | ---------- | -------- |
| mobile   | true     | string | 手机号码   |          |
| sms      | true     | string | 短信验证码 |          |
| capCode  | true     | string | 图形验证码 |          |

响应数据:

| 参数名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
| -------- | -------- | -------- | ---- | -------- |
| token    | true     | string   |      |          |

请求响应示例:

```json
/* POST /v1/login/mobileLogin
{
	"mobile": "13399991111",
	"sms": "3344",
	"capCode": "af3d"
}
*/
{
    "status": "ok",
    "data": {
     	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
    }
}

/* POST /v1/login/mobileLogin */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 重置密码  
### POST /v1/login/resetPassword 

请求参数：

| 参数名称   | 是否必须 | 类型   | 描述         | 取值范围 |
| ---------- | -------- | ------ | ------------ | -------- |
| mobile     | true     | string | 手机号码     |          |
| password   | true     | string | 设置新密码   |          |
| rePassword | true     | string | 重复输入密码 |          |
| sms        | true     | string | 短信验证码   |          |

响应参数：

（无）

请求响应示例：

```json
/* POST /v1/login/resetPassword
{
	"mobile": "1339992222",
	"password": "123456",
	"rePassword": "123456",
	"sms": "1111"
}
*/
{
    "status": "ok"
}

/* POST /v1/login/resetPassword */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 当前投注期数
### GET /v1/bet/currentPhrase 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 | 1,2,3,4  |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述                                        | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------- | -------- |
| phrase   | true     | number   | 当前期数。为0时，表示该维空间游戏已停机维护 |          |

请求响应示例：

```json
/* GET /v1/bet/getCurPhrase?type=1 */
{
    "status": "ok",
    "data": {
        "phrase": 333
    }
}

/* GET /v1/bet/getCurPhrase?type=1 */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 平台已产生游戏场次
### GET /v1/bet/producedList 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述     | 默认值 | 取值范围 |
| -------- | -------- | -------- | -------- | ------ | -------- |
| offset   | false    | number   | 分页起始 | 0      |          |
| limit    | false    | number   | 分页长度 | 10     |          |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述     | 取值范围 |
| -------- | -------- | -------- | -------- | -------- |
| list     | true     | array    | 场次列表 |          |

请求响应示例：

```json
/* GET /v1/bet/producedList?offset=3&limit=10 */
{
    "status": "ok",    
    "data": {
        "total": 7,
		"count": 4,
     	"list": [29, 30, 35, 100]
    }
}

/* GET /v1/bet/producedList?offset=3&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 已开奖场次信息  
### GET  /v1/bet/openedList 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 默认值 | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | ------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 |        | 1,2,3,4  |
| offset   | false    | number   | 分页起始                                                     | 0      |          |
| limit    | false    | number   | 分页长度                                                     | 10     |          |

响应数据：

| 参数名称  | 是否必须 | 数据类型 | 描述                                    | 取值范围 |
| --------- | -------- | -------- | --------------------------------------- | -------- |
| phrase    | true     | array    | 场次数（期数）                          |          |
| amount    | true     | number   | 奖池金额                                |          |
| price     | true     | number   | 单注金额                                |          |
| time      | true     | number   | 开奖时间，unix时间, 精确到秒            |          |
| betStatus | true     | number   | 0表示未开奖；１表示待确认；２表示已开奖 | 0,1,2    |

请求响应数据：

```json
/* GET  /v1/bet/getOpenedList?offset=20&limit=10&type=1  */
{
    "status": "ok",    
    "data": {
        "total": 22,
		"count": 2,
        "list": [{        	
            "phrase": 29,
    		"amount": 111.33,
    		"price": 2.2,
    		"time": 1543822057,
    		"betStatus": 0
        },{
            "phrase": 28,
    		"amount": 111.33,
    		"price": 2.2,
    		"time": 1543822057,
    		"betStatus": 0
        }        
    ]
    }
}

/* GET  /v1/bet/getOpenedList?offset=20&limit=10&type=1  */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 排行榜  
### GET /v1/bet/rank　

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 默认值 | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | ------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 |        | 0,1,2,3  |
| offset   | false    | number   | 分页起始                                                     | 0      |          |
| limit    | false    | number   | 分页长度                                                     | 10     |          |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述               | 取值范围 |
| -------- | -------- | -------- | ------------------ | -------- |
| rank     | true     | number   | 排名               |          |
| nickname | true     | string   | 昵称               |          |
| time     | true     | number   | unix时间, 精确到秒 |          |
| prize    | true     | number   | 累计赢得的金额     |          |

请求响应数据：

```json
/* GET  /v1/bet/rank?type=1&offset=30&limit=10 */
{
    "status": "ok",        
    "data": {
        "total": 32,
		"count": 2,
        "list": [{    		
    		"rank":1,
            "nickname": "LiMing",
            "time": 1543822057,
            "prize": 9999.99
        }, {
        	"rank":2,
            "nickname": "Liu de",
            "time": 1543822057,
            "prize": 999.99
        }
    ]
    }
}

/* GET  /v1/bet/rank?type=1&offset=30&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 上次预测结果  
### GET /v1/bet/lastPhraseDetail 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 | 1,2,3,4  |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述                     | 取值范围 |
| -------- | -------- | -------- | ------------------------ | -------- |
| phrase   | true     | number   | 最近一期出结果的期数     |          |
| opened   | true     | string   | 最近一期出结果的开奖结果 |          |
| pool     | true     | number   | 最近一期出结果的奖池金额 |          |

请求响应示例：

```json
/* GET /v1/bet/lastPhraseDetail?type=1  */
{
    "status": "ok",
    "data": {
        "phrase": 29,
    	"opend": "3",
    	"pool": 3333.221
    }    
}

/* GET /v1/bet/lastPhraseDetail?type=1  */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 当前投注期数信息
### GET /v1/bet/currentPhraseDetail 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 | 1,2,3,4  |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| phrase   | true     | number   | 当前期数                                                     |          |
| pool     | true     | number   | 当前奖池金额                                                 |          |
| price    | true     | number   | 每注单价                                                     |          |
| item     | true     | object    | 当前下注统计。当请求参数type=1时，依次表示数字、字母的下注金额；当type=2时，依次表示下注0-f的下注金额；type=3，依次表示下注0、1、2、3、4、5的下注金额，number类型 |          |

请求响应数据：

```json
/* GET /v1/bet/currentPhraseDetail?type=1 */
{
	"status": "ok",
	"data": {
		"phrase": 29,
		"pool": 3333.221,
		"price": 11,
		"item": {
			"0": 33,
			"1": 44.33,
			"2": 55,
			"3": 66,
			"4": 77,
			"5": 88
		}
	}
}

/* GET /v1/bet/currentPhraseDetail?type=1 */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 矿工费获取
### GET /v1/public/fee	
获取指定币种当前矿工费  
请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     || BTC, ETH, USDT, BBX |
| amount         | true     | number   | 提币数量   |||


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| fee         | true     | number   | 矿工费   ||
| symbol         | true     | string   | 货币符号   | BTC, ETH, USDT, BBX |


请求响应例子:

```json
/* GET /v1/assets/fee?currency=ETH&amount=43.12 */
{
	"status": "ok",
	"data": {
		"amount": "0.6468",
		"symbol": "ETH",
	}
}

/* GET /v1/assets/fee?currency=ETH&amount=43.12 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 获取SMS验证码
### GET /v1/public/sms/:mobile	
获取短信验证码  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| mobile         | true     | string   | 手机号码   |  |  |

响应数据:

（无）


请求响应例子:

```json
/* GET /v1/public/sms/13300040005 */
{
	"status": "ok",
}

/* GET /v1/public/sms/13300040005 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 通知、公告、活动  
### GET /v1/public/announce 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                     | 默认值 | 取值范围 |
| -------- | -------- | -------- | -------------------------------------------------------- | ------ | -------- |
| type     | true     | number   | 类别：0表示所有类型；1表示系统通知；2表示公告；3表示活动 |        | 0,1,2,3  |
| offset   | false    | number   | 分页起始                                                 | 0      |          |
| limit    | false    | number   | 分页长度                                                 | 10     |          |

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述                         | 取值范围 |
| -------- | -------- | -------- | ---------------------------- | -------- |
| title    | true     | string   | 主题                         |          |
| time     | true     | number   | 发布时间，UNIX时间，精确到秒 |          |
| url      | true     | string   | 详情页面                     |          |

请求响应示例：

```json
/* GET  /v1/public/announce?type=2&offset=0&limit=2 */
{
    "status": "ok",    
    "data":  {
        "total": 10,
		"count": 2,
        "list": [
    		{
    			"title": "好消息，注册即送100USDT!!!",
    			"time": 1543822067,
    			"url": "https://www.baidu.com"
    		},
    		{
    			"title": "2018年12月5日系统维护通知",
    			"time": 1543822057,
    			"url": "https://www.baidu.com"
    		}    		
    	]  
    }  
}

/* GET /v1/public/announce?type=1&offset=0&limit=2 */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 注销  
### POST /v1/login/logout 

请求参数：

（无）

响应数据：

（无）

请求响应示例：

```json
/* POST /v1/login/logout */
{
    "status": "ok"
}

/* POST /v1/login/logout */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 平台已产生的游戏期数详情  
### GET  /v1/bet/producedDetail 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 默认值 | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | ------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 |        | 1,2,3,4  |
| phrase   | true     | number   | 期数                                                         |        |          |

响应数据：

| 参数名称  | 是否必须 | 数据类型 | 描述                                    | 取值范围 |
| --------- | -------- | -------- | --------------------------------------- | -------- |
| amount    | true     | number   | 奖池金额                                |          |
| price     | true     | number   | 单注金额                                |          |
| time      | true     | number   | 产生时间，unix时间，精确到秒            |          |
| betStatus | true     | number   | 0表示未开奖；１表示待确认；２表示已开奖 | 0,1,2    |

请求响应示例：

```json
/* GET  /v1/bet/producedDetail?type=1&phrase=29  */
{
    "status": "ok",
    "data": {
    	"amount": 111.33,
    	"price": 2.2,
    	"time": 1543822057,
    	"betStatus": 0
    }
}

/* GET  /v1/bet/producedDetail?type=1&phrase=29  */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 一维空间快捷投注
### POST /v1/bet/firstdimension 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                  | 取值范围 |
| -------- | -------- | -------- | --------------------- | -------- |
| choice   | true     | number   | 0表示字母；１表示数字 | 0,1      |

响应数据：

（无）

请求响应示例：

```json
/* POST /v1/bet/firstdimension
{
	"choice": 1
}
*/
{
    "status": "ok",
}

/* POST /v1/bet/firstdimension */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 三维空间快捷投注
### POST /v1/bet/thirddimension 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述 | 取值范围    |
| -------- | -------- | -------- | ---- | ----------- |
| choice   | true     | number   | 选中 | 0,1,2,3,4,5 |

响应数据：

（无）

请求响应示例：

```json
/* POST /v1/bet/thirddimension
{
	"choice": 5
}
*/
{
    "status": "ok"
}

/* POST /v1/bet/thirddimension */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 投注  
### POST /v1/bet/bet 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| type     | true     | number   | 游戏类别：1表示一维空间游戏；２表示二维空间游戏；3表示三维空间游戏；４表示四维空间游戏 | 1,2,3,4  |
| choice   | true     | string   | 用户投注数据                                                 |          |
| amount   | true     | number   | 注数                                                         |          |

响应数据：

（无）

请求响应示例：

```json
/* POST /v1/bet/bet  
{
	"type": 1,
	"choice": "1",
	"amount": 3
}
*/
{
    "status": "ok"
}

/* POST /v1/bet/bet  */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 个人资产  
### GET /v1/assets/personal 

请求参数：

（无）

响应参数：

| 参数名称     | 是否必须 | 数据类型 | 描述       | 取值范围 |
| ------------ | -------- | -------- | ---------- | -------- |
| total        | true     | number   | 总量       |          |
| assetName    | true     | string   | 资产名称   |          |
| assetBalance | true     | number   | 余额       |          |
| assetFrozen  | true     | number   | 已冻结数量 |          |

请求响应示例：

```json
/* GET /v1/assets/personal */
{
    "status": "ok",
    "data": {
        "total": 10,
        "list": [
        {
    		"assetName": "BTC",
    		"assetBalance": 100.02,
    		"assetFrozen": 5.001        
        },
        {
    		"assetName": "LTC",
    		"assetBalance": 100.02,
    		"assetFrozen": 5.001        
        }        
    ]
    }
}

/* GET /v1/assets/personal */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 个人充币地址获取 
### GET /v1/assets/address  
指定币种获取个人充币地址

请求参数:   

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     |  | BTC, ETH, USDT, BBX |


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| address         | true     | string   | 充币地址                                   | |
| tag         | false     | string   | 标记备注（有则显示，如果有标记备注，代表这种类型的数字货币，需要填写充币地址和标注备注信息才能定位用户账户来完成充币）   | |


请求响应例子:

```json
/* GET /v1/assets/address?currency=ETH */
{
	"status": "ok",
	"data": {
		"address": "0xE36a16E59B834BF834A3B1eEB86E911490050737",
		"tag": "101264005",
	}
}

/* GET /v1/assets/address?currency=ETH */
{
  "status": "error",
  "err-msg": "error message"
}
```



## 充币记录列表获取
### GET /v1/assets/deposit	
获取个人充币记录列表

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     |   | BTC, ETH, USDT, BBX |
| offset         | false     | number   | 分页查询起始   | 0 | |
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| txhash         | true     | string   | 交易HASH                                   | |
| amount         | true     | number   | 充币数量   | |
| symbol         | true     | string   | 货币符号   | BTC, ETH, USDT, BBX |
| ts         | true     | number   | 充币时间。UNIX时间戳，单位：秒   | |
| state         | true     | string   | 状态字符串   | '1/30';'succ';'fail' |


请求响应例子:

```json
/* GET /v1/assets/deposit?currency=ETH&offset=20&limit=10 */
{
	"status": "ok",
	"data": {
		"total": 22,
		"count": 2,
		"list": [{
			"txhash": "0x5a795770f8af9690eb38c45ea3d089ea7e74544063029f18b10a7b7951ae789b",
			"amount": "0.014",
			"symbol": "ETH",
			"ts": 1543819612,
			"state": "2/6"
		}, {
			"txhash": "0xe703437c16c71f9072d75762d42df4c9a9fc147d2fa7873aecba79e697cb7cb4",
			"amount": "0.016",
			"symbol": "ETH",
			"ts": 1541227612,
			"state": "succ"
		}]
	}
}

/* GET /v1/assets/deposit?currency=ETH&offset=20&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 可用余额获取
### GET /v1/assets/balance	
获取个人可以使用的USDT余额

请求参数: 

（无）

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| | true     | number | 资产余额,单位USDT | |


请求响应例子:

```json
/* GET /v1/assets/balance */
{
	"status": "ok",
	"data": 186.321
}


/* GET /v1/assets/balance */
{
  "status": "error",
  "err-msg": "error message"
}
```


  
## 提币记录列表获取  
### GET /v1/assets/withdraw	 
获取个人提币记录列表  
请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     | | BTC, ETH, USDT, BBX |
| offset         | false     | number   | 分页查询起始   | 0 | |
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| txhash         | true     | string   | 交易HASH                                   | |
| amount         | true     | number   | 提币数量   | |
| symbol         | true     | string   | 货币符号   | BTC, ETH, USDT, BBX |
| ts         | true     | number   | 提币时间。UNIX时间戳，单位：秒   | |
| state         | true     | string   | 状态字符串   | 'pending';'rejected';'processing';'completed' （分别对应：待审核；已拒绝;处理中；已完成） |

请求响应例子:

```json
/* GET /v1/assets/withdraw?currency=ETH&offset=5&limit=5 */
{
	"status": "ok",
	"data": {
		"total": 7,
		"count": 2,
		"list": [{
			"txhash": "0xcc925614438fba54bbbefa68bae9b1e31ee02a8b0069d37d7e6cbcb2a67f88ef",
			"amount": "0.06",
			"symbol": "ETH",
			"ts": 1543819612,
			"state": "completed"
		}, {
			"txhash": "0x853e041101f7315035bb8bb5fa38b6e5008c3ea6aa106a647196d45eb65c12e2",
			"amount": "0.0001",
			"symbol": "ETH",
			"ts": 1541227612,
			"state": "pending"
		}]
	}
}

/* GET /v1/assets/withdraw?currency=ETH&offset=5&limit=5 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 提币
### POST /v1/assets/withdraw	
发送提币请求到后台处理  
请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     |      | BTC, ETH, USDT, BBX |
| address    | true     | string | 提币到这个地址     |       | |
| amount    | true     | string | 提币数量     |       | |
| fundPassword    | true     | string | 资金密码     |       | |


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| data | true     | string   | 提币请求id,可用于查询请求处理状态 | |


请求响应例子:

```json
/* POST /v1/assets/withdraw
{
	"currency": "BTC",
	"address": "168o1kqNquEJeR9vosUB5fw4eAwcVAgh8P",
	"amount": "1323.334",
	"fundPassword": "888888"
}
*/
{
	"status": "ok",
	"data": "99831"
}


/* POST /v1/assets/withdraw
{
	"currency": "BTC",
	"address": "168o1kqNquEJeR9vosUB5fw4eAwcVAgh8P",
	"amount": "1323.334",
	"fundPassword": "888888"
}
*/
{
  "status": "error",
  "err-msg": "error message"
}
```


## 指定币种充提币记录列表获取
### GET /v1/assets/receipt	
指定币种获取个人账单  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| currency    | true     | string | 币种     | | BTC, ETH, USDT, BBX |
| offset         | false     | number   | 分页查询起始   | 0 | |
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |



响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| type         | true     | string   | 类型                                   | deposit:充币<br>withdraw:提币<br>reward:奖励<br> |
| txhash         | false     | string   | 链上转账交易哈希，内部转账无交易哈希  | |
| amount         | true     | number   | 数量   | |
| symbol         | true     | string   | 货币符号   | BTC, ETH, USDT, BBX |
| ts         | true     | number   | 记录时间。UNIX时间戳，单位：秒   | |


请求响应例子:

```json
/* GET /v1/assets/receipt?currency=ETH&offset=20&limit=10 */
{
	"status": "ok",
	"data": {
		"total": 24,
		"count": 4,
		"list": [{
			"type": "deposit",
			"txhash": "0x5a795770f8af9690eb38c45ea3d089ea7e74544063029f18b10a7b7951ae789b",
			"amount": "0.014",
			"symbol": "ETH",
			"ts": 1543819612
		}, {
			"type": "deposit",
			"txhash": "0xe703437c16c71f9072d75762d42df4c9a9fc147d2fa7873aecba79e697cb7cb4",
			"amount": "0.016",
			"symbol": "ETH",
			"ts": 1541227612
		}, {
			"type": "withdraw",
			"txhash": "0xcc925614438fba54bbbefa68bae9b1e31ee02a8b0069d37d7e6cbcb2a67f88ef",
			"amount": "0.06",
			"symbol": "ETH",
			"ts": 1543819612
		}, {
			"type": "reward",
			"txhash": "0x853e041101f7315035bb8bb5fa38b6e5008c3ea6aa106a647196d45eb65c12e2",
			"amount": "0.0001",
			"symbol": "ETH",
			"ts": 1541227612
		}]
	}
}

/* GET /v1/assets/receipt?currency=ETH&offset=20&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```



## 会员信息展示  
### GET /v1/personal/info 

请求参数：

（无）

响应数据：

| 参数名称 | 是否必须 | 数据类型 | 描述     | 取值范围 |
| -------- | -------- | -------- | -------- | -------- |
| avatar   | false    | string   | 头像     |          |
| nickname | false    | string   | 昵称     |          |
| level    | false    | string   | 会员等级 |          |
| mobile   | false    | string   | 手机     |          |
| email    | false    | string   | 邮箱     |          |

请求响应示例：

```json
/* GET /v1/personal/info */
{
    "status": "ok",
    "data" : {
        "avatar": "http://image.baidu.com/search/detail?ct=503316480&z=0&ipn=d&word=%E4%B9%A0%E8%BF%91%E5%B9%B3&step_word=&hs=0&pn=4&spn=0&di=101192120040&pi=0&rn=1&tn=baiduimagedetail&is=0%2C0&istype=2&ie=utf-8&oe=utf-8&in=&cl=2&lm=-1&st=-1&cs=178161544%2C2228296541&os=2215135238%2C1015760909&simid=0%2C0&adpicid=0&lpn=0&ln=1932&fr=&fmq=1543819496839_R&fm=result&ic=0&s=undefined&hd=0&latest=0&copyright=0&se=&sme=&tab=0&width=&height=&face=undefined&ist=&jit=&cg=&bdtype=0&oriquery=&objurl=http%3A%2F%2Fmedia.workercn.cn%2Fsites%2Fmedia%2Fgrrb%2F2018_03%2F21%2F1%2Ftpzz183705_b.jpg&fromurl=ippr_z2C%24qAzdH3FAzdH3F4j1tw_z%26e3Bo56hj6vg_z%26e3BvgAzdH3FftpjfAzdH3F4j1twAzdH3F266kAzdH3Fda8b_anAzdH3Fd8AzdH3FGRa8a8_z%26e3Bip4&gsm=0&rpstart=0&rpnum=0&islist=&querylist=&selected_tags=",
        "nickname": "LiMing",
        "level":"3",
        "mobile":"1339992222"
    }
}    

/* GET /v1/personal/info */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 会员信息编辑  
### PUT /v1/personal/info 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述     | 取值范围               |
| -------- | -------- | -------- | -------- | ---------------------- |
| avatar   | false    | string   | 头像     | 以下四项最少有一项编辑 |
| nickname | false    | string   | 昵称     |                        |
| mobile   | false    | string   | 手机号码 |                        |
| email    | false    | string   | 邮箱     |                        |

响应数据：

（无）

请求响应示例：

```json
/* PUT  /v1/personal/info
{
	"nickname": "LiMing",
	"email": "110@qq.com"
}
*/
{
    "status": "ok"
}

/* PUT  /v1/personal/info
{
  "status": "error",
  "err-msg": "error message"
}
```


## 阅读完成奖励  
### POST /v1/task/mission 

请求参数：

| 参数名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
| -------- | -------- | -------- | ---- | -------- |
| id       | true     | number   | ID   |          |

响应数据：

| 参数名称   | 是否必须 | 数据类型 | 描述                       | 取值范围 |
| ---------- | -------- | -------- | -------------------------- | -------- |
| thisBonus  | true     | number   | 该篇文章带来的奖励金币数量 |          |
| totalBonus | true     | number   | 累积奖励金币数量           |          |

请求响应示例：

```json
/* POST /v1/task/mission
{
	"id": 101
}
*/
{
    "status": "ok",
    "data": {        
    	"thisBonus": 20,
    	"totalBonus": 520
    }
}

/* POST /v1/task/mission */
{
  "status": "error",
  "err-msg": "error message"
}
```


## 登录密码设置接口
### PUT /v1/personal/password	
修改登录密码  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| oldPassword    | true     | string | 旧密码     |   |  |
| newPassword    | true     | string | 新密码     |    |  |
| confirmPassword    | true     | string | 确认新密码     |  | |
| smsVerifyCode    | true     | string | 短信验证码     |  | |


响应数据: 
（无）

请求响应例子:

```json
/* PUT /v1/personal/password 
{
	"oldPassword": "123456",
	"newPassword": "654321",
	"confirmPassword": "654321",
	"smsVerifyCode": "001122"
}
*/

{
	"status": "ok"
}

/* PUT /v1/personal/password */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 资金密码设置接口
### PUT /v1/personal/fundPassword	
修改资金密码  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| newPassword    | true     | string | 新密码     | | |
| confirmPassword    | true     | string | 确认新密码     | | |
| smsVerifyCode    | true     | string | 短信验证码     | | |


响应数据: 
（无）

请求响应例子:

```json
/* PUT /v1/personal/fundPassword 
{
	"newPassword": "654321",
	"confirmPassword": "654321",
	"smsVerifyCode": "001122"
}
*/

{
	"status": "ok"
}

/* PUT /v1/personal/fundPassword */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 谷歌验证设置接口
### GET /v1/personal/googleAuthenticator	
获取谷歌验证秘钥  

请求参数: 
(无)


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| key         | true     | string   | 谷歌验证秘钥                                   | |


请求响应例子:

```json
/* GET /v1/personal/googleAuthenticator */
{
	"status": "ok",
	"data": {
		"key": "TKCYH2DMB6MMFKYQ"
	}
}

/* GET /v1/personal/googleAuthenticator */
{
  "status": "error",
  "err-msg": "error message"
}
```

### PUT /v1/personal/googleAuthenticator	
设置谷歌验证器  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| code    | true     | string | 谷歌验证码     | | |
| smsVerifyCode    | true     | string | 短信验证码,服务端检查成功才允许设置     |   | |


响应数据: 
（无）

请求响应例子:

```json
/* PUT /v1/personal/googleAuthenticator 
{
	"code": "123456",
	"smsVerifyCode": "001122"
}
*/

{
	"status": "ok"
}

/* PUT /v1/personal/googleAuthenticator */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 手势密码设置接口
暂不实现

## 登录验证设置接口
暂不实现，允许密码登录，允许短信登录，允许账号登录，允许手机号登录

## 提现验证设置接口
暂不实现，开启资金密码验证，开启谷歌验证

## 交易验证设置接口
暂不实现，开启资金密码验证，开启谷歌验证

## 用户参与游戏列表
### GET /v1/personal/bet	
获取个人竞猜投票列表  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| offset         | false     | number   | 分页查询起始   | 0 | |
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| id         | true     | number   | 记录id,用于查询详细信息 | |
| num         | true     | number   | 期数                                   | |
| name         | false     | string   | 玩法名称  | 'One-dimensional';'Two-dimensional';'Three-dimensional','Four-dimensional' ('一维空间','二维空间','三维空间','四维空间') |
| quantity         | true     | number   | 投注数量   | |
| winning         | true     | bool   | 是否中奖   | true;false |
| state         | true     | number   | 开奖状态   | =0 表示待开奖，[1 - 5] 等待确认， >= 6 已开奖 |


请求响应例子:

```json
/* GET /v1/personal/bet?offset=0&limit=10 */
{
	"status": "ok",
	"data": {
		"total": 102,
		"count": 10,
		"list": [{
			"id": 10,
			"num": 552379,
			"name": "One-dimensional",
			"quantity": 24,
			"winning": false,
			"state": 0
		}, {
			"id": 9,
			"num": 552378,
			"name": "One-dimensional",
			"quantity": 2,
			"winning": false,
			"state": 1
		}, {
			"id": 8,
			"num": 552377,
			"name": "One-dimensional",
			"quantity": 5,
			"winning": false,
			"state": 2
		}, {
			"id": 7,
			"num": 552376,
			"name": "One-dimensional",
			"quantity": 9,
			"winning": false,
			"state": 3
		}, {
			"id": 6,
			"num": 552375,
			"name": "One-dimensional",
			"quantity": 100,
			"winning": false,
			"state": 4
		}, {
			"id": 5,
			"num": 552374,
			"name": "Four-dimensional",
			"quantity": 50,
			"winning": false,
			"state": 5
		}, {
			"id": 4,
			"num": 552373,
			"name": "One-dimensional",
			"quantity": 12,
			"winning": false,
			"state": 6
		}, {
			"id": 3,
			"num": 552372,
			"name": "Three-dimensional",
			"quantity": 5,
			"winning": false,
			"state": 7
		}, {
			"id": 2,
			"num": 552371,
			"name": "One-dimensional",
			"quantity": 5,
			"winning": true,
			"state": 8
		}, {
			"id": 1,
			"num": 552370,
			"name": "Two-dimensional",
			"quantity": 20,
			"winning": false,
			"state": 9
		}]
	}
}

/* GET /v1/personal/bet?offset=0&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 用户参与游戏详情
### GET /v1/personal/bet/:id	
获取个人竞猜投票详情  

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| id         | true     | number   | 指定id查询投注记录   | - | - |

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| id         | true     | number   | 记录id,用于查询详细信息 | |
| num         | true     | number   | 期数                                   | |
| name         | false     | string   | 玩法名称  | 'One-dimensional';'Two-dimensional';'Three-dimensional','Four-dimensional' ('一维空间','二维空间','三维空间','四维空间') |
| quantity         | true     | number   | 投注数量   | |
| winning         | true     | bool   | 是否中奖   | true;false |
| state         | true     | number   | 开奖状态   | =0 表示待开奖，[1 - 5] 等待确认， >= 6 已开奖 |
| reward         | true     | number   | 奖励金额   | 单位:USDT |
| result         | true     | string   | 开奖结果   | blockhash |
| price         | true     | number   | 单注金额   | |
| ts         | true     | number   | 开奖时间   | UNIX时间戳，单位：秒 |


请求响应例子:

```json
/* GET /v1/personal/bet/552379 */
{
	"status": "ok",
	"data": {
		"id": 10,
		"num": 552379,
		"name": "One-dimensional",
		"quantity": 24,
		"winning": false,
		"state": 0,
		"reward": "5234.32",
		"result": "0000000000000000001d759519a42e6b9b83152d65022fd0942328f4e0aa1a64",
		"price": 1,
		"ts": 1543834050
	}
}

/* GET /v1/personal/bet/552379 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 合伙人邀请码获取
### GET /v1/personal/invite	
获取个人邀请码  

请求参数: 

(无)

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| code         | true     | string   | 邀请码                                   | |
| url         | true     | string   | 邀请码url(含邀请码，直接用于生成二维码)  | |



请求响应例子:

```json
/* GET /v1/personal/invite */
{
	"status": "ok",
	"data": {
		"code": "bmvr2v",
		"url": "http://bbx.com/v1/personal/register/bmvr2v"
	}
}

/* GET /v1/personal/invite */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 签到状态获取
### GET /v1/personal/signin	
获取个人签到状态  

请求参数: 

(无)

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| count         | true     | number   | 连续签到次数 | |
| reward         | true     | number   | 明日签到奖金  | |


请求响应例子:

```json
/* GET /v1/personal/signin */
{
	"status": "ok",
	"data": {
		"count": 4,
		"reward": "16"
	}
}

/* GET /v1/personal/signin */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 签到
### POST /v1/personal/signin 
每日签到  

请求参数: 

(无)

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| count         | true     | number   | 连续签到次数 ||
| reward         | true     | number   | 明日签到奖金  ||


请求响应例子:

```json
/* POST /v1/personal/signin */
{
	"status": "ok",
	"data": {
		"count": 5,
		"reward": "32"
	}
}

/* GET /v1/personal/signin */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 任务列表
### GET /v1/personal/task	
获取任务列表  

请求参数:   

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| offset         | false     | number   | 分页查询起始   | 0 ||
| limit         | false     | number   | 分页查询数量   | 10 | 0 - 1000 |

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| action         | true     | string   | 根据任务名称做相关处理<br>completion:完成资料<br>invite:邀请好友<br>bet:参与竞猜<br>exposure:晒收入<br><^http[s]?://>:跳转到http://或https://开头的url，展示其页面内容  | completion;invite;bet;exposure;^http[s]?://.* |
| catalog         | true     | number   | 任务分类，0-新手任务；1-日常任务；2-专项任务 | [0,1,2]  |
| countdown         | true     | number   | 有效期倒计时，单位:天，-1表示无限期  | -1 or 自然数  |
| title         | true     | string   | 任务标题  |  |
| reward         | true     | number   | 任务奖励  |  |
| description         | true     | number   | 任务描述  |  |
| todo         | true     | string   | 按钮上显示的文字，比如"去完成"  |  |


请求响应例子:

```json
/* GET /v1/personal/task?offset=20&limit=10 */
{
	"status": "ok",
	"data": {
		"total": 25,
		"count": 5,
		"list": [{
			"action": "completion",
			"catalog": 0,
			"countdown": 29,
			"title": "data completion",
			"reward": 140,
			"description": "Improve user data to get rewards",
			"todo": "To do"
		}, {
			"action": "invite",
			"catalog": 0,
			"countdown": 29,
			"title": "invite friends",
			"reward": 140,
			"description": "Friend registration to get rewards",
			"todo": "To do"
		}, {
			"action": "bet",
			"catalog": 0,
			"countdown": 29,
			"title": "Play a game",
			"reward": 140,
			"description": "Participate in the game to get rewards",
			"todo": "To do"
		}, {
			"action": "exposure",
			"catalog": 1,
			"countdown": -1,
			"title": "wages online exposure",
			"reward": 140,
			"description": "Wages online exposure to get rewards",
			"todo": "To do"
		}, {
			"action": "https://task.for.christmas",
			"catalog": 2,
			"countdown": 4,
			"title": "Happy christmas",
			"reward": 140,
			"description": "Jingle Bells Jingle Bells Dashing through the snow In a one horse open sleigh O'er the fields we go Laughing all the way Bells",
			"todo": "To do"
		}]
	}
}

/* GET /v1/personal/task?offset=20&limit=10 */
{
  "status": "error",
  "err-msg": "error message"
}
```

## 用户游戏数据统计
### GET /v1/personal/stat	
获取个人游戏数据统计  

请求参数: 

(无)

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| count         | true     | number   | 参与竞猜次数   ||
| reward         | true     | number   | 奖金总数，单位:USDT ||


请求响应例子:

```json
/* GET /v1/personal/stat */
{
	"status": "ok",
	"data": {
		"count": 123,
		"reward": "5234.32"
	}
}

/* GET /v1/personal/stat */
{
  "status": "error",
  "err-msg": "error message"
}
```

