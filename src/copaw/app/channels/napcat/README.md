# NapCat Channel for CoPaw

此 Channel 使用 NapCat (OneBot 11 协议) 连接 QQ，实现消息收发功能。

## 使用前提

1. **安装并运行 NapCat**
   - 参考 [NapCat 官方文档](https://napcatnapcat.github.io/NapCatDocs/) 安装
   - 需要配置 WebSocket 连接用于接收消息

2. **配置 config.json**
   
   在 `~/.copaw/config.json` 的 `channels` 中添加：

```json
{
  "napcat": {
    "enabled": true,
    "host": "127.0.0.1",
    "port": 3000,
    "ws_port": 3001,
    "access_token": "",
    "bot_prefix": "",
    "filter_tool_messages": false,
    "filter_thinking": false,
    "dm_policy": "open",
    "group_policy": "open",
    "allow_from": [],
    "deny_message": ""
  }
}
```

## 配置项说明

| 配置项 | 必填 | 默认值 | 说明 |
|--------|------|--------|------|
| enabled | 是 | false | 是否启用此 Channel |
| host | 是 | 127.0.0.1 | NapCat 服务器地址 |
| port | 是 | 3000 | NapCat HTTP API 端口 |
| ws_port | 是 | 3001 | NapCat WebSocket 端口 |
| access_token | 否 | - | 访问令牌（如果 NapCat 配置了认证） |
| bot_prefix | 否 | - | 消息前缀，只有带此前缀的消息才会被处理 |
| filter_tool_messages | 否 | false | 是否过滤工具消息 |
| filter_thinking | 否 | false | 是否过滤思考过程 |
| dm_policy | 否 | open | 私聊策略：open/deny |
| group_policy | 否 | open | 群消息策略：open/deny |
| allow_from | 否 | [] | 允许的发送者列表 |
| deny_message | 否 | - | 拒绝时的提示消息 |

## 注意事项

- 确保 NapCat 已经启动并正常运行
- WebSocket 用于接收消息，HTTP API 用于发送消息
- 如果 NapCat 配置了 access_token，需要在配置中填写
