# btcXpizza DApp Development

## 1. 项目概述

本项目是 btcXpizza DApp 的前后端协作仓库。

- **前端**: 请基于此仓库中的 Mock API 进行开发。
- **后端**: 负责提供接口实现和智能合约。

本文档旨在帮助前端同事快速搭建本地开发环境，并理解API数据结构。

## 2. 本地开发环境启动指南

请按照以下步骤启动一个本地的 Mock API 服务器，该服务器会模拟所有后端接口，供前端开发和调试使用。

### 前提条件

- 确保你的电脑已安装 [Node.js](https://nodejs.org/) (自带 npm)。

### 步骤

**1. 克隆本项目仓库**
```bash
git clone [你的项目仓库HTTPS或SSH链接]
cd btcxpizza-dapp
```

**2. 安装 json-server**
如果你是第一次使用，需要全局安装 `json-server`。打开终端并运行：
```bash
npm install -g json-server
```
*(此命令只需在你的电脑上执行一次)*

**3. 启动 Mock Server**
在项目**根目录**下，运行以下命令：
```bash
json-server --watch mock-api/db.json --routes mock-api/routes.json
```

**4. 验证**
如果看到类似下面的输出，说明服务器已成功启动：
```
  \{^_^}/ hi!

  Loading mock-api/db.json
  Loading mock-api/routes.json
  Done

  Resources
  http://localhost:3000/status
  http://localhost:3000/getReferralInfo
  http://localhost:3000/getPurchaseHistory
  http://localhost:3000/getUserDashboard
  http://localhost:3000/getReferrerAddress

  Home
  http://localhost:3000
```
现在，你可以在你的前端应用中请求 `http://localhost:3000/api/user/...` 的地址来获取模拟数据了。

## 3. API 接口文档

所有API接口的详细说明，包括每个字段的定义、数据类型和业务逻辑，都已在以下文档中详细列出：

**[点击查看详细API文档](./docs/API.md)**

## 4. 可用的 Mock Endpoints

Mock Server 启动后，以下API端点将可用：

- **获取用户状态**:
  `GET` `http://localhost:3000/api/user/status`

- **获取推荐信息**:
  `GET` `http://localhost:3000/api/user/getReferralInfo`

- **获取购买记录**:
  `GET` `http://localhost:3000/api/user/getPurchaseHistory`

- **获取用户中心数据**:
  `GET` `http://localhost:3000/api/user/getUserDashboard`

- **通过推荐码获取推荐人地址**:
  `GET` `http://localhost:3000/api/user/getReferrerAddress`

