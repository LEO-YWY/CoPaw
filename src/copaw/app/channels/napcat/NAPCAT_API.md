# NapCat Channel for CoPaw - 接口说明文档

## 概述

NapCat 是一个基于 OneBot 11 标准的 QQ 机器人框架，运行在本地，提供 HTTP API 接口供外部调用。

**与官方 QQ Bot 的区别：**
- 官方 QQ Bot：需要申请 App ID，使用腾讯官方 API，需要审核
- NapCat：自托管运行，使用 OneBot 11 协议，无需审核，直接登录 QQ

---

## 核心 API 接口

### 1. 发送群消息
- **接口：** `POST /send_group_msg`
- **说明：** 发送消息到指定群
- **请求参数：**
```json
{
  "group_id": "123456",      // 群号 (必需)
  "message": "hello"         // 消息内容 (必需)
}
```
- **响应示例：**
```json
{
  "status": "ok",
  "retcode": 0,
  "data": {
    "message_id": 123456
  }
}
```

### 2. 获取登录号信息
- **接口：** `POST /get_login_info`
- **说明：** 获取当前登录的 QQ 号信息
- **请求参数：** 无
- **响应示例：**
```json
{
  "status": "ok",
  "retcode": 0,
  "data": {
    "user_id": 123456789,
    "nickname": "用户名"
  }
}
```

### 3. 获取群列表
- **接口：** `POST /get_group_list`
- **说明：** 获取机器人加入的所有群列表
- **请求参数：** 无
- **响应示例：**
```json
{
  "status": "ok",
  "retcode": 0,
  "data": [
    {
      "group_id": 123456,
      "group_name": "测试群",
      "member_count": 100,
      "max_member_count": 200
    }
  ]
}
```

### 4. 获取群成员列表
- **接口：** `POST /get_group_member_list`
- **说明：** 获取指定群的成员列表
- **请求参数：**
```json
{
  "group_id": "123456"  // 群号
}
```

### 5. 发送私聊消息
- **接口：** `POST /send_private_msg`
- **说明：** 发送私聊消息
- **请求参数：**
```json
{
  "user_id": "123456",   // 对方 QQ 号
  "message": "hello"     // 消息内容
}
```

### 6. 获取群消息历史
- **接口：** `POST /get_group_msg_history`
- **说明：** 获取群消息历史记录
- **请求参数：**
```json
{
  "group_id": "123456",   // 群号
  "message_seq": 0         // 起始消息序号，0 表示从最新消息开始
}
```

### 7. 撤回消息
- **接口：** `POST /delete_msg`
- **说明：** 撤回指定消息
- **请求参数：**
```json
{
  "message_id": 123456   // 消息 ID
}
```

### 8. 获取好友列表
- **接口：** `POST /get_friend_list`
- **说明：** 获取好友列表
- **请求参数：** 无

### 9. 获取用户信息
- **接口：** `POST /get_stranger_info`
- **说明：** 获取指定用户信息
- **请求参数：**
```json
{
  "user_id": "123456"  // 用户 QQ 号
}
```

---

## 消息段类型 (Message Segments)

NapCat (OneBot 11) 支持多种消息段类型：

### 纯文本
```json
{"type": "text", "data": {"text": "Hello"}}
```

### @某人
```json
{"type": "at", "data": {"qq": "123456"}}
```

### 图片
```json
{"type": "image", "data": {"file": "xxx.jpg"}}
// 或使用 URL
{"type": "image", "data": {"url": "https://xxx.com/img.jpg"}}
```

### 表情
```json
{"type": "face", "data": {"id": 123}}
```

### 语音
```json
{"type": "record", "data": {"file": "xxx.mp3"}}
```

### 视频
```json
{"type": "video", "data": {"file": "xxx.mp4"}}
```

### 位置
```json
{"type": "location", "data": {"lat": 39.904, "lon": 116.407, "title": "北京"}}
```

### JSON
```json
{"type": "json", "data": {"json": "{\"key\": \"value\"}"}}
```

### 合并转发
```json
{"type": "node", "data": {"id": 123}}
```

---

## 消息接收方式

### 方式 1: WebSocket (推荐)

NapCat 支持 WebSocket 连接接收事件消息。需要配置 NapCat 启用 WebSocket 服务。

**连接地址：** `ws://your-napcat-host:port/ws`

**事件示例：**
```json
{
  "post_type": "message",
  "message_type": "group",
  "group_id": 123456,
  "user_id": 789,
  "message": "Hello",
  "message_id": 111111
}
```

### 方式 2: HTTP Webhook (回调)

配置 NapCat 将事件消息 POST 到指定 URL。

---

## 配置项说明

| 配置项 | 说明 | 示例 |
|--------|------|------|
| `host` | NapCat 服务器地址 | `127.0.0.1` |
| `port` | NapCat HTTP API 端口 | `3000` |
| `ws_port` | NapCat WebSocket 端口 | `3001` |
| `access_token` | 访问令牌（可选） | `your-token` |
| `bot_prefix` | 消息前缀过滤 | `[BOT]` |
| `group_policy` | 群消息策略 | `open` / `deny` |
| `dm_policy` | 私聊消息策略 | `open` / `deny` |

---

## 响应状态码

| retcode | 说明 |
|---------|------|
| 0 | 成功 |
| 1 | 错误 |
| 100 | 权限不足 |
| 101 | 账号不存在 |
| 102 | 群不存在 |
| 103 | 群成员不存在 |
| 104 | 消息不存在 |
| 105 | 未知消息类型 |
| 106 | 错误的 JSON |
| 107 | 参数错误 |
| 108 | 请求超时 |
| 109 | 速率限制 |
| 110 | 目标不存在 |

---

## 完整请求示例

### cURL
```bash
curl -X POST http://127.0.0.1:3000/send_group_msg \
  -H "Content-Type: application/json" \
  -d '{"group_id": "123456", "message": "Hello"}'
```

### Python
```python
import requests

url = "http://127.0.0.1:3000/send_group_msg"
data = {"group_id": "123456", "message": "Hello"}
response = requests.post(url, json=data)
print(response.json())
```
