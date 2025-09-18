btcXpizza DApp API 文档
本文档定义了 btcXpizza DApp 前端所需的所有后端接口。前端开发应严格按照本文档定义的数据结构进行页面开发和数据绑定。

通用约定
所有接口的基础 URL 为 [你的API服务器地址]。

所有接口均为 GET 请求。

所有接口的响应数据格式均为 application/json。

时间戳格式统一采用 ISO 8601 字符串格式，例如："2025-09-18T11:55:43Z"。

1. 获取用户状态
此接口用于获取当前连接钱包的用户在项目中的核心状态，用于判断首页核心按钮的显示文本及功能。

Endpoint: GET /api/user/status

描述: 查询用户当前的参与状态及其关联的 BTC 钱包地址。

响应数据示例
JSON

{
  "status": "completed",
  "btcAddress": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh"
}
字段说明
字段	类型	描述
status	String	用户当前状态。有以下四种可能的值： - "not_participated": 新用户，从未参与过。 - "pending_airdrop": 已支付300U，等待项目方手动转币。 - "completed": 已正常参与，且项目方已确认空投。 - "max_profit_reached": 已达到5倍收益，需要复投。
btcAddress	String	用户关联的BTC钱包地址。如果用户之前参与过，此字段会返回用户上次填写的地址，用于前端输入框的自动填充。如果用户是新用户，此字段可能为 null。

Export to Sheets
2. 获取用户推荐关系
此接口用于获取用户的直接上级和直接下级列表。

Endpoint: GET /api/user/getReferralInfo

描述: 查询用户的直推上级和下级列表信息。

响应数据示例
JSON

{
  "upline": "0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B",
  "downline": [
    {
      "address": "0x1Db3439a222C519ab44bb1144FC28167b4Fa6EE6",
      "joinTimestamp": "2025-09-16T14:25:10Z"
    },
    {
      "address": "0x95222290DD7278Aa3Ddd389Cc1E1d165CC4BAfe5",
      "joinTimestamp": "2025-09-17T08:40:02Z"
    }
  ]
}
字段说明
字段	类型	描述
upline	String	null
downline	Array	直接下级列表。一个包含对象的数组，每个对象代表一个用户的直接下级。如果用户没有直推下级，返回空数组 []。
↳ address	String	下级用户的钱包地址。
↳ joinTimestamp	String	该下级用户的加入时间。

Export to Sheets
3. 获取购买记录
此接口用于获取用户通过“购买btcXpizza”功能（非300U参与空投）的所有历史订单。

Endpoint: GET /api/user/getPurchaseHistory

描述: 查询用户所有单独购买 btcXpizza 代币的历史记录。

响应数据示例
JSON

[
  {
    "purchaseTimestamp": "2025-08-20T10:15:30Z",
    "amountUSD": 100,
    "status": "completed"
  },
  {
    "purchaseTimestamp": "2025-09-18T11:20:11Z",
    "amountUSD": 150,
    "status": "pending"
  }
]
字段说明
字段	类型	描述
purchaseTimestamp	String	购买时间。用户发起这笔购买交易的时间戳。
amountUSD	Number	购买金额。用户该笔订单支付的USDT金额。
status	String	订单处理状态。有两个可能的值： - "completed": 项目方已完成手动的btcXpizza代币转账。 - "pending": 用户已付款，等待项目方处理。

Export to Sheets
4. 获取用户中心数据看板
这是最核心的聚合接口，一次性返回用户中心页面所需的所有数据。

Endpoint: GET /api/user/getUserDashboard

描述: 获取用户中心页面的所有聚合数据。

响应数据示例
JSON

{
  "vipLevel": "V2",
  "rewards": {
    "current": 850.50,
    "total": 2150.50,
    "referral": 312.40,
    "downline": 378.00,
    "management": 160.10
  },
  "team": {
    "directInvites": 18,
    "totalMembers": 168
  },
  "referralLink": "https://btcx.pizza/join?ref=k8f7b2m9",
  "inviteHistory": [
    {
      "address": "0x1Db3439a222C519ab44bb1144FC28167b4Fa6EE6",
      "joinTimestamp": "2025-09-16T14:25:10Z"
    }
  ]
}
字段说明
字段	类型	描述
vipLevel	String	VIP等级。用户当前的管理奖等级，例如 "V1", "V2", ..., "V7"。
rewards	Object	奖励数据对象。一个包含所有奖励相关数据的对象。
↳ current	Number	当前奖励。用户在当前生命周期（从上次入金/复投至今）累计的总收益。此数值直接关联5倍出局进度（上限1500U）。
↳ total	Number	历史总奖励。用户从第一次参与至今，所有生命周期累计的总收益。
↳ referral	Number	推荐奖。累计获得的直推奖励金额。
↳ downline	Number	见点奖。累计获得的见点奖励金额。
↳ management	Number	管理奖。累计获得的团队管理奖励金额。
team	Object	团队数据对象。一个包含团队相关数据的对象。
↳ directInvites	Number	邀请人数。用户直接邀请的下级总数。
↳ totalMembers	Number	团队人数。用户整个下级网络中的成员总数。
referralLink	String	推广链接。后端为用户生成的唯一邀请链接。ref参数为8位随机字母/数字组合的邀请码。前端需提供一键复制功能。
inviteHistory	Array	邀请记录。一个包含对象的数组，每个对象代表一个用户的直接下级，用于渲染邀请记录列表。如果为空，则返回 []。
↳ address	String	直推下级的钱包地址。
↳ joinTimestamp	String	该下级的加入
