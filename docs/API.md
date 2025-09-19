# btcXpizza 前端 API 文档

本文档定义了 btcXpizza DApp 前端所需的所有后端接口。

## 1. 获取用户状态

获取当前连接钱包的用户的状态，用于判断其参与阶段。

- **Endpoint**: `GET /api/user/status`
- **说明**: 用于控制首页核心按钮（“立即参与”、“已参与等待空投”、“重新复投”）的显示逻辑，并预填用户上次使用的BTC地址。

**返回数据:**
```json
{
  "status": "completed", // String: 用户当前状态。可能的值: "not_participated", "pending_airdrop", "completed"
  "btcAddress": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh" // String: 用户上次参与时填写的BTC钱包地址，若首次参与则为空字符串 ""
}
```

---

## 2. 获取推荐信息

获取用户的直接上级和直接下级列表。

- **Endpoint**: `GET /api/user/getReferralInfo`
- **说明**: 用于在“参与空投”页面展示用户的邀请关系。

**返回数据:**
```json
{
  "upline": "0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B", // String: 用户的上级推荐人地址。如果没有上级，则为 null
  "downline": [ // Array: 用户的直接下级列表
    {
      "address": "0x1Db3439a222C519ab44bb1144FC28167b4Fa6EE6", // String: 下级用户的钱包地址
      "joinTimestamp": "2025-09-16T14:25:10Z" // String (ISO 8601): 该下级用户的加入时间
    },
    {
      "address": "0x95222290DD7278Aa3Ddd389Cc1E1d165CC4BAfe5", // String: 下级用户的钱包地址
      "joinTimestamp": "2025-09-17T08:40:02Z" // String (ISO 8601): 该下级用户的加入时间
    }
  ]
}
```

---

## 3. 获取购买记录

获取用户通过“购买btcXpizza”功能的所有历史订单记录。

- **Endpoint**: `GET /api/user/getPurchaseHistory`
- **说明**: 此接口数据仅针对单独购买代币功能，与300U参与空投的记录无关。

**返回数据:**
```json
[ // Array: 用户的历史购买订单列表
  {
    "purchaseTimestamp": "2025-08-20T10:15:30Z", // String (ISO 8601): 用户发起购买交易的时间
    "amountUSD": 100, // Number: 用户该笔订单支付的USDT金额
    "status": "completed" // String: 订单处理状态。可能的值: "completed" (已发放), "pending" (待发放)
  },
  {
    "purchaseTimestamp": "2025-09-18T11:20:11Z",
    "amountUSD": 150,
    "status": "pending"
  }
]
```

---

## 4. 获取用户中心数据看板

聚合接口，一次性返回“用户中心”页面所需的所有数据。

- **Endpoint**: `GET /api/user/getUserDashboard`
- **说明**: 这是数据最核心的接口，前端应在进入用户中心页面时调用此接口来渲染整个页面的数据。

**返回数据:**
```json
{
  "vipLevel": "V2", // String: 用户当前的管理奖等级 (例如 "V0","V1", "V2", ..., "V7")
  "rewards": { // Object: 包含所有奖励相关数据的对象
    "current": 850.50, // Number: 当前奖励。当前生命周期的累计收益，用于判断5倍出局进度
    "total": 2150.50, // Number: 历史总奖励。用户所有生命周期累计的总收益
    "referral": 312.40, // Number: 累计获得的推荐奖金额
    "downline": 378.00, // Number: 累计获得的见点奖金额
    "management": 160.10 // Number: 累计获得的管理奖金额
  },
  "team": { // Object: 包含团队相关数据的对象
    "directInvites": 18, // Number: 邀请人数。直接邀请的下级总数
    "totalMembers": 168 // Number: 团队人数。整个下级网络中的成员总数
  },
  "referralLink": "[https://btcx.pizza/join?ref=k8f7b2m9](https://btcx.pizza/join?ref=k8f7b2m9)", // String: 用户的专属推广链接，邀请码为8位随机字符串
  "inviteHistory": [ // Array: 用户的历史直推记录列表
    {
      "address": "0x1Db3439a222C519ab44bb1144FC28167b4Fa6EE6", // String: 直推下级的钱包地址
      "joinTimestamp": "2025-09-16T14:25:10Z" // String (ISO 8601): 该下级的加入时间
    },
    {
      "address": "0x95222290DD7278Aa3Ddd389Cc1E1d165CC4BAfe5", // String: 直推下级的钱包地址
      "joinTimestamp": "2025-09-17T08:40:02Z" // String (ISO 8601): 该下级的加入时间
    }
  ]
}
```
