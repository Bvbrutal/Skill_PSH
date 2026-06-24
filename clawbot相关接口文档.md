# Clawbot 接口文档

> 微信二维码登录与 IM 通道管理相关 API 接口说明。

---

## 目录

- [1. 微信二维码登录](#1-微信二维码登录)
  - [1.1 获取登录二维码](#11-获取登录二维码)
  - [1.2 轮询二维码扫描状态](#12-轮询二维码扫描状态)
  - [1.3 登录流程说明](#13-登录流程说明)
- [2. IM 通道管理](#2-im-通道管理)
  - [2.1 重置 IM 通道](#21-重置-im-通道)

---

## 1. 微信二维码登录

### 1.1 获取登录二维码

```
POST /api/v1/wechat/qrcode
```

**功能：** 获取微信登录二维码。

**请求参数：** 无

**响应示例：**

```json
{
  "success": true,
  "data": {
    "qrcode_url": "https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=https://ilinkai.weixin.qq.com/ilink/bot/get_qrcode_status?qrcode=xxx",
    "qrcode": "qrcode_token_xxx"
  }
}
```

**响应字段说明：**

| 字段名 | 类型 | 说明 |
| --- | --- | --- |
| `success` | boolean | 请求是否成功 |
| `data` | object | 二维码信息 |
| `data.qrcode_url` | string | 二维码图片 URL，可直接用于前端显示 |
| `data.qrcode` | string | 二维码 token，用于后续状态轮询 |

---

### 1.2 轮询二维码扫描状态

```
POST /api/v1/wechat/qrcode/status
```

**功能：** 轮询微信二维码扫描状态（长轮询接口）。

**请求参数：**

```json
{
  "qrcode": "qrcode_token_xxx"
}
```

**请求参数说明：**

| 参数名 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `qrcode` | string | 是 | 从获取二维码接口返回的二维码 token |

**响应示例：**

```json
{
  "success": true,
  "data": {
    "status": "wait",
    "credentials": {
      "bot_token": "bearer_token_xxx",
      "ilink_bot_id": "bot_id_xxx",
      "ilink_user_id": "user_id_xxx"
    },
    "baseurl": "https://ilinkai.weixin.qq.com"
  }
}
```

**响应字段说明：**

| 字段名 | 类型 | 说明 |
| --- | --- | --- |
| `success` | boolean | 请求是否成功 |
| `data` | object | 状态信息 |
| `data.status` | string | 二维码状态（见下表） |
| `data.credentials` | object \| null | 认证凭据，仅在 `status` 为 `confirmed` 时返回 |
| `data.credentials.bot_token` | string | Bearer token，用于后续 API 调用 |
| `data.credentials.ilink_bot_id` | string | Bot 标识符 |
| `data.credentials.ilink_user_id` | string | 用户标识符 |
| `data.baseurl` | string | API 基础 URL，可能覆盖默认值 |

**状态值说明：**

| 状态值 | 含义 |
| --- | --- |
| `wait` | 等待扫描 |
| `scaned` | 已扫描（等待确认） |
| `confirmed` | 已确认（登录成功） |
| `expired` | 二维码已过期 |

---

### 1.3 登录流程说明

```
1. 调用 POST /api/v1/wechat/qrcode  → 获取二维码 URL 和 token
2. 在前端显示二维码图片（使用 qrcode_url）
3. 使用 token 调用 POST /api/v1/wechat/qrcode/status  → 轮询扫描状态
4. 当状态变为 confirmed 时，获取认证凭据用于后续 IM 通道通信
```

---

## 2. IM 通道管理

### 2.1 重置 IM 通道

```
POST /api/v1/wechat/channel_reset
```

**功能：** 重置微信 IM 通道，删除数据库中用户的 IM 通道记录并停止对应的长轮询任务。

**请求参数：**

```json
{
  "channel_id": "channel_token_xxx"
}
```

**请求参数说明：**

| 参数名 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `channel_id` | string | 是 | 需要重置的通道 ID |

**响应示例：**

```json
{
  "data": {
    "message": "IM通道重置任务已提交",
    "channel_id": "channel_token_xxx"
  }
}
```

**响应字段说明：**

| 字段名 | 类型 | 说明 |
| --- | --- | --- |
| `data` | object | 重置结果信息 |
| `data.message` | string | 重置操作的结果消息 |
| `data.channel_id` | string | 被重置的通道 ID |

**注意事项：**

- 此操作会**删除**数据库中的 IM 通道记录
- 会**停止**对应通道的长轮询任务
- 需要有效的临时令牌认证
