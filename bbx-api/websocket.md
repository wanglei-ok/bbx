

# BBX Websocket 
采用订阅广播模式

- [接口URL](#接口url)
- [订阅](#订阅)
- [取消订阅](#取消订阅) 
- [Topic格式](#topic格式) 
- [推送数据](#推送数据) 
	- [最新公告](#最新公告)  
	- [最新活动](#最新活动) 
	- [最新通知](#最新通知) 
	- [统计数据](#统计数据) 
	- [平台统计数据](#平台统计数据) 
	- [一维空间统计数据](#一维空间统计数据) 
	- [二维空间统计数据](#二维空间统计数据) 
	- [三维空间统计数据](#三维空间统计数据) 
	- [四维空间统计数据](#四维空间统计数据) 
	- [投注状态](#投注状态) 
	- [奖池状态](#奖池状态) 
	- [一维空间奖池](#一维空间奖池) 
	- [二维空间奖池](#二维空间奖池) 
	- [三维空间奖池](#三维空间奖池) 
	- [四维空间奖池](#四维空间奖池) 
	- [个人可用余额(**AccessToken API** )](#个人可用余额) 
- [在线客服](#在线客服) 	


## 接口URL

ws://api.bbx.com/wshandler (后期可能修改)

## 订阅
客户端建立连接后，向服务端发起订阅请求。  
之后每当订阅topic有更新时，客户端会收到数据。

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述       | 默认值 | 取值范围               |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| sub | true | string | 订阅消息，topic格式 | | |
| id | true | string | 客户端生成的id,用于匹配异步请求应答 |  |  |
| token | false | string | 登录后才能访问的topic,订阅时需要提交access token |  |  |

```json
{
	"sub": "topic to sub",
	"id": "id generate by client"
}
```


响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| status     | true     | string   | 订阅状态 | "ok";"error" |
| id         | true     | string   | 服务端回填id  | |
| ts         | true     | number   | 服务端填写UNIX时间戳，单位：秒   | |
| subbed     | false     | string   | 订阅成功时，服务端回填topic   | |
| err-code   | false     | string   | 订阅失败时，包含错误编码 | |
| err-msg    | false     | string   | 订阅失败时，包含错误信息字符串 | |
```json
# succ
{
	"status": "ok",
	"id": "id generate by client",
	"ts": 1543819612,
	"subed": "topic to sub"
}

# error
{
	"status": "error",
	"id": "id generate by client",
	"ts": 1543819612,
	"err-code": "bad-request",
	"err-msg": "invalid topic"
}
```

## 取消订阅

订阅数据之后，客户端可以取消订阅，取消订阅后服务器将不会再发送该topic的数据

请求参数: 

| 参数名称  | 是否必须 | 类型   | 描述 | 默认值 | 取值范围 |
| :--------- | :-------- | :------ | :---------- | :------ | :---------------------- |
| unsub | true | string | 订阅消息，topic格式 | | |
| id    | true | string | 客户端生成的id,用于匹配异步请求应答 |  |  |
```json
{
	"unsub": "topic to unsub",
	"id": "id generate by client"
}
```

响应数据:

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| status   | true      | string   | 请求状态 | "ok";"error" |
| id       | true      | string   | 服务端回填id  | |
| ts       | true      | number   | 服务端填写UNIX时间戳，单位：秒   | |
| err-code | false     | string   | 取消订阅失败时，包含错误编码 | |
| err-msg  | false     | string   | 取消订阅失败时，包含错误信息字符串 | |
| unsubbed | false     | string   | 取消订阅成功时，服务端回填topic   | |


```json
# succ
{
	"status": "ok",
	"id": "id generate by client",
	"ts": 1543819612,
	"unsubbed": "topic to unsub"
}

# error
{
	"status": "error",
	"id": "id generate by client",
	"ts": 1543819612,
	"err-code": "bad-request",
	"err-msg": "invalid topic"
}
```

## Topic格式

| topic类型  | topic语法 | 描述 |
| :--------- | :-------- | :-------- |
|最新公告|bbx.push.announce|推送最新公告信息|
|最新活动|bbx.push.event |推送最新活动|
|最新通知|bbx.push.notice|推送平台产生的通知|
|平台统计数据|bbx.stat.platform|推送平台统计，累计收集的USDT数据和累计游戏人次，投注次数|
|一维空间统计数据|bbx.stat.one|推送一维空间统计，累计收集的USDT数据和累计游戏人次，投注次数|
|二维空间统计数据|bbx.stat.two|推送二维空间统计，累计收集的USDT数据和累计游戏人次，投注次数|
|三维空间统计数据|bbx.stat.three|推送三维空间统计，累计收集的USDT数据和累计游戏人次，投注次数|
|四维空间统计数据|bbx.stat.four|推送四维空间统计，累计收集的USDT数据和累计游戏人次，投注次数|
|一维空间奖池|bbx.current.one|推送一维空间，当前奖池奖金，每个投注项的注数，当前参与人次|
|二维空间奖池|bbx.current.two|推送二维空间，当前奖池奖金，每个投注项的注数，当前参与人次|
|三维空间奖池|bbx.current.three|推送三维空间，当前奖池奖金，每个投注项的注数，当前参与人次|
|四维空间奖池|bbx.current.four|推送四维空间，当前奖池奖金，当前投注数，当前参与人次|
|个人可用余额|bbx.personal.balance|**AccessToken API**个人可用余额变动时，推动可用余额|
```json
# Request
{
	"sub": "bbx.push.announce",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.push.announce"
}

# Error Request
{
	"sub": "bbx.push.invalid",
	"id": "id2"
}


# Error Response
{
	"status": "error",
	"id": "id2",
	"ts": 1544676787,
	"err-code": "bad-request",
	"err-msg": "invalid topic"
}
```

## 推送数据
| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| channel | true     | string   | topic格式 |  |
| ts      | false    | number   | 数据时间，UNIX时间戳，单位：秒  | |
| data    | true     | json     | 推送数据对象 | |


```json
{
	"channel": "topic to sub",
	"ts": "unix timestamp",
	"data": {}
}
```


## 最新公告 
**bbx.push.announce**

推送最新公告信息

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| title   | true      | string   | 公告标题 | |
| content       | false      | string   | 文本格式的公告内容  | |
| url       | false      | string   | HTML格式的公告内容  | |
| ts       | true      | number   | 公告时间  | UNIX时间戳，单位：秒 |


```json
# Request
{
	"sub": "bbx.push.announce",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.push.announce"
}

# Push Data
{
	"channel": "bbx.push.announce",
	"ts": "1543819612",
	"data": {
		"title": "No. 578890, suspension of purchase",
		"content": "Avoid high transaction fees and stop buying. When we resume business, we will notify you separately.",
		"ts": "1543928391"
	}
}
```

## 最新活动
**bbx.push.event**  

推送最新活动  

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| thumbnail       | true      | string   | 活动缩略图  | |
| title   | true      | string   | 活动标题 | |
| content       | false      | string   | 文本格式的活动内容  | |
| url       | false      | string   | HTML格式的活动内容  | |
| ts-start       | true      | number   |  活动开始时间  | UNIX时间戳，单位：秒 |
| ts-end       | true      | number   | 活动结束时间  | UNIX时间戳，单位：秒 |


```json
# Request
{
	"sub": "bbx.push.event",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.push.event"
}

# Push Data
{
	"channel": "bbx.push.event",
	"ts": "1543819612",
	"data": {
		"thumbnail": "http://pic-file.bbx.com/612aad94480bd79dd3cb726fee8de4bd/320X40",
		"title": "OTC trading",
		"content": "Free handling fee",
		"ts-start": "1543928391",
		"ts-end": "1546520391"
	}
}
```

## 最新通知
**bbx.push.notice**

推送平台产生的通知  

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| title   | true      | string   | 通知标题 | |
| content       | false      | string   | 文本格式的通知内容  | |
| url       | false      | string   | HTML格式的通知内容  | |
| ts      | true      | number   |  通知时间  | UNIX时间戳，单位：秒 |



```json
# Request
{
	"sub": "bbx.push.notice",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.push.notice"
}

# Push Data
{
	"channel": "bbx.push.notice",
	"ts": "1543819612",
	"data": {
		"title": "Your account 18092283703 has already logged in to APP.",
		"url": "http://notice.bbx.com/c9311d18f3",
		"ts": "1543928391"
	}
}
```

## 统计数据

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| total-amount   | true      | number   | 累计金额 | |
| total-player       | true      | number   | 累计人次  | |
| total-betting       | true      | number   | 累计投注数   | |
```json
{
	"total-amount": 8765234567,
	"total-player": 45687,
	"total-betting": 768477
}
```

## 平台统计数据
**bbx.stat.platform**

推送平台累计收集的USDT数据和累计游戏人次，投注次数
```json
# Request
{
	"sub": "bbx.stat.platform",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.stat.platform"
}

# Push Data
{
	"channel": "bbx.stat.platform",
	"ts": "1543819612",
	"data": {
		"total-amount": 8765234567,
		"total-player": 45687,
		"total-betting": 768477
	}
}
```

## 一维空间统计数据
**bbx.stat.one**

推送一维空间统计，累计收集的USDT数据和累计游戏人次，投注次数
```json
# Request
{
	"sub": "bbx.stat.one",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.stat.one"
}

# Push Data
{
	"channel": "bbx.stat.one",
	"ts": "1543819612",
	"data": {
		"total-amount": 5234567,
		"total-player": 87,
		"total-betting": 477
	}
}
```

## 二维空间统计数据
**bbx.stat.two**


推送二维空间统计，累计收集的USDT数据和累计游戏人次，投注次数
```json
# Request
{
	"sub": "bbx.stat.two",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.stat.two"
}

# Push Data
{
	"channel": "bbx.stat.two",
	"ts": "1543819612",
	"data": {
		"total-amount": 5234567,
		"total-player": 87,
		"total-betting": 477
	}
}
```

## 三维空间统计数据
**bbx.stat.three**


推送三维空间统计，累计收集的USDT数据和累计游戏人次，投注次数
```json
# Request
{
	"sub": "bbx.stat.three",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.stat.three"
}

# Push Data
{
	"channel": "bbx.stat.three",
	"ts": "1543819612",
	"data": {
		"total-amount": 5234567,
		"total-player": 87,
		"total-betting": 477
	}
}
```

## 四维空间统计数据
**bbx.stat.four**

推送四维空间统计，累计收集的USDT数据和累计游戏人次，投注次数
```json
# Request
{
	"sub": "bbx.stat.four",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.stat.four"
}

# Push Data
{
	"channel": "bbx.stat.four",
	"ts": "1543819612",
	"data": {
		"total-amount": 5234567,
		"total-player": 87,
		"total-betting": 477
	}
}
```

## 投注状态

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| amount   | true      | number   | 投注金额 | |
| player       | true      | number   | 参与人次  | |
| betting       | true      | number   | 投注数量   | |

## 奖池状态

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| amount   | true      | number   | 投注金额 | |
| player       | true      | number   | 参与人次  | |
| betting       | true      | number   | 投注数量   | |
| items       | false      | array   | 每个投注项的投注状态，四维空间没有投注项，其他空间投注项的数量<br>一维空间：2<br>二维空间：16<br>三维空间：6<br>四维空间：0<br>   | |

## 一维空间奖池
**bbx.current.one**

推送一维空间，当前奖池奖金，每个投注项的注数，当前参与人次

```json
# Request
{
	"sub": "bbx.current.one",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.current.one"
}

# Push Data
{
	"channel": "bbx.current.one",
	"ts": "1543819612",
	"data": {
		"amount": 3584,
		"player": 16,
		"betting": 1792,
		"items": [{
			"amount": 2942,
			"player": 2,
			"betting": 1471
		}, {
			"amount": 642,
			"player": 14,
			"betting": 321
		}]
	}
}
```

## 二维空间奖池
**bbx.current.two**

推送二维空间，当前奖池奖金，每个投注项的注数，当前参与人次
```json
# Request
{
	"sub": "bbx.current.two",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.current.two"
}

# Push Data
{
	"channel": "bbx.current.two",
	"ts": "1543819612",
	"data": {
		"amount": 18072,
		"player": 42,
		"betting": 9036,
		"items": [{
			"amount": 0,
			"player": 0,
			"betting": 0
		}, {
			"amount": 0,
			"player": 0,
			"betting": 0
		}, {
			"amount": 64,
			"player": 33,
			"betting": 32
		}, {
			"amount": 1766,
			"player": 7,
			"betting": 883
		}, {
			"amount": 2942,
			"player": 2,
			"betting": 1471
		}, {
			"amount": 642,
			"player": 14,
			"betting": 321
		}, {
			"amount": 64,
			"player": 33,
			"betting": 32
		}, {
			"amount": 1766,
			"player": 7,
			"betting": 883
		}, {
			"amount": 2942,
			"player": 2,
			"betting": 1471
		}, {
			"amount": 642,
			"player": 14,
			"betting": 321
		}, {
			"amount": 64,
			"player": 33,
			"betting": 32
		}, {
			"amount": 1766,
			"player": 7,
			"betting": 883
		}, {
			"amount": 2942,
			"player": 2,
			"betting": 1471
		}, {
			"amount": 642,
			"player": 14,
			"betting": 321
		}, {
			"amount": 64,
			"player": 33,
			"betting": 32
		}, {
			"amount": 1766,
			"player": 7,
			"betting": 883
		}]
	}
}
```
## 三维空间奖池
**bbx.current.three**

推送三维空间，当前奖池奖金，每个投注项的注数，当前参与人次
```json
# Request
{
	"sub": "bbx.current.one",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.current.one"
}

# Push Data
{
	"channel": "bbx.current.one",
	"ts": "1543819612",
	"data": {
		"amount": 116228,
		"player": 87,
		"betting": 58114,
		"items": [{
			"amount": 2942,
			"player": 2,
			"betting": 1471
		}, {
			"amount": 642,
			"player": 14,
			"betting": 321
		}, {
			"amount": 64,
			"player": 33,
			"betting": 32
		}, {
			"amount": 90,
			"player": 25,
			"betting": 45
		}, {
			"amount": 110724,
			"player": 6,
			"betting": 55362
		}, {
			"amount": 1766,
			"player": 7,
			"betting": 883
		}]
	}
}
```
## 四维空间奖池
**bbx.current.four**

推送四维空间，当前奖池奖金，当前投注数，当前参与人次
```json
# Request
{
	"sub": "bbx.current.one",
	"id": "id1"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.current.one"
}


# Push Data
{
	"channel": "bbx.current.one",
	"ts": "1543819612",
	"data": {
		"amount": 116228,
		"player": 87,
		"betting": 58114
	}
}
```

## 个人可用余额
**AccessToken API**
**bbx.personal.balance**

个人可用余额变动时，推动可用余额，请求订阅时需要携带token参数

| 参数名称         | 是否必须 | 数据类型 | 描述                                           | 取值范围       |
| :--------------- | :------- | :------- | :--------------------------------------------- | :------------- |
| data   | true      | number   | 可用余额 | |

```json
# succ
# Request
{
	"sub": "bbx.personal.balance",
	"id": "id1",
	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}

# Response
{
	"status": "ok",
	"id": "id1",
	"ts": 1543819612,
	"subbed": "bbx.personal.balance"
}

# Push Data
{
	"channel": "bbx.personal.balance",
	"ts": "1543819612",
	"data": 2126
}

# error
# Request
{
	"sub": "bbx.personal.balance",
	"id": "id2"
}

# Error Response
{
	"status": "error",
	"id": "id2",
	"ts": 1543819612,
	"err-code": "bad-request",
	"err-msg": "No access."
}


```

## 在线客服	
暂不实现

