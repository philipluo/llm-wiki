# 飞书 Bot 搭建指南：agents-to-im 连接 Claude Code

> 本文档记录如何将 Claude Code 编码代理桥接到飞书/Lark，实现私聊控制、群聊会话隔离的完整搭建过程。

## 前置条件

- Node.js 20+ 已安装
- 飞书开放平台账号
- 本地已安装并认证 Claude Code CLI

---

## 第一阶段：飞书应用创建与配置（飞书开放平台）

### 步骤 1：创建飞书自建应用

**📍 飞书开放平台**

1. 打开 https://open.feishu.cn/app
2. 点击「创建自建应用」
3. 填写应用名称和描述
4. 创建完成后，记录 `App ID` 和 `App Secret`

---

### 步骤 2：开启 Bot 能力

**📍 飞书开放平台 → 应用详情页**

1. 进入「添加应用能力」
2. 启用「机器人」能力
3. 设置 Bot 名称和描述

---

### 步骤 3：导入权限 scopes JSON

**📍 飞书开放平台 → 权限管理**

链接格式：`https://open.feishu.cn/app/{你的App_ID}/auth`

1. 找到「导入权限」按钮
2. 粘贴以下 JSON 并导入：

```json
{
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "bitable:app",
      "bitable:app:readonly",
      "cardkit:card:read",
      "cardkit:card:write",
      "contact:contact.base:readonly",
      "contact:user.base:readonly",
      "contact:user.basic_profile:readonly",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "docs:doc",
      "docs:doc:readonly",
      "docs:document.comment:create",
      "docs:document.comment:read",
      "docs:document.comment:update",
      "docs:document.comment:write_only",
      "docs:document.content:read",
      "docs:document.media:download",
      "docs:document.media:upload",
      "docs:document.subscription",
      "docs:document.subscription:read",
      "docs:document:copy",
      "docs:document:export",
      "docs:document:import",
      "docs:event.document_deleted:read",
      "docs:event.document_edited:read",
      "docs:event.document_opened:read",
      "docs:event:subscribe",
      "docx:document",
      "docx:document.block:convert",
      "docx:document:create",
      "docx:document:readonly",
      "drive:drive",
      "drive:drive:readonly",
      "event:ip_list",
      "im:app_feed_card:write",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:chat:read",
      "im:chat:update",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.group_msg",
      "im:message.p2p_msg:readonly",
      "im:message.pins:read",
      "im:message.pins:write_only",
      "im:message.reactions:read",
      "im:message.reactions:write_only",
      "im:message.urgent:phone",
      "im:message.urgent:sms",
      "im:message:readonly",
      "im:message:recall",
      "im:message:send_as_bot",
      "im:message:send_multi_users",
      "im:message:send_sys_msg",
      "im:message:update",
      "im:resource",
      "sheets:spreadsheet",
      "wiki:wiki",
      "wiki:wiki:readonly"
    ],
    "user": [
      "aily:file:read",
      "aily:file:write",
      "base:app:copy",
      "base:app:create",
      "base:app:read",
      "base:app:update",
      "base:field:create",
      "base:field:delete",
      "base:field:read",
      "base:field:update",
      "base:record:create",
      "base:record:delete",
      "base:record:retrieve",
      "base:record:update",
      "base:table:create",
      "base:table:delete",
      "base:table:read",
      "base:table:update",
      "base:view:read",
      "base:view:write_only",
      "board:whiteboard:node:create",
      "board:whiteboard:node:read",
      "calendar:calendar.event:create",
      "calendar:calendar.event:delete",
      "calendar:calendar.event:read",
      "calendar:calendar.event:reply",
      "calendar:calendar.event:update",
      "calendar:calendar.free_busy:read",
      "calendar:calendar:read",
      "contact:contact.base:readonly",
      "contact:user.base:readonly",
      "contact:user.basic_profile:readonly",
      "contact:user.employee_id:readonly",
      "contact:user:search",
      "docs:document.comment:create",
      "docs:document.comment:read",
      "docs:document.comment:update",
      "docs:document.media:download",
      "docs:document.media:upload",
      "docs:document:copy",
      "docs:document:export",
      "docx:document:create",
      "docx:document:readonly",
      "docx:document:write_only",
      "drive:drive.metadata:readonly",
      "drive:file:download",
      "drive:file:upload",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:read",
      "im:chat:read",
      "im:message",
      "im:message.group_msg:get_as_user",
      "im:message.p2p_msg:get_as_user",
      "im:message:readonly",
      "offline_access",
      "search:docs:read",
      "search:message",
      "sheets:spreadsheet.meta:read",
      "sheets:spreadsheet:create",
      "sheets:spreadsheet:read",
      "sheets:spreadsheet:write_only",
      "space:document:delete",
      "space:document:move",
      "space:document:retrieve",
      "task:comment:read",
      "task:comment:write",
      "task:task:read",
      "task:task:write",
      "task:task:writeonly",
      "task:tasklist:read",
      "task:tasklist:write",
      "wiki:node:copy",
      "wiki:node:create",
      "wiki:node:move",
      "wiki:node:read",
      "wiki:node:retrieve",
      "wiki:space:read",
      "wiki:space:retrieve",
      "wiki:space:write_only"
    ]
  }
}
```

---

### 步骤 4：第一次发布版本

**📍 飞书开放平台 → 版本管理与发布**

链接格式：`https://open.feishu.cn/app/{你的App_ID}/release`

1. 点击「创建版本」
2. 填写版本号（如 `1.0.0`）和版本说明
3. 提交审核
4. 等待管理员审批通过

⚠️ **重要**：没有发布前，Bot 和权限都不会生效

---

## 第二阶段：本地安装与配置

### 步骤 5：安装 agents-to-im

**📍 本地终端**

```bash
npm install -g agents-to-im
```

---

### 步骤 6：创建配置文件

**📍 本地终端**

```bash
mkdir -p ~/.agents-to-im
```

配置文件位置：`~/.agents-to-im/config.env`

最小配置内容：

```env
# Working directory
CTI_DEFAULT_WORKDIR=/你的/工作目录/路径

# Feishu / Lark bot
CTI_FEISHU_APP_ID=cli_xxx    # 替换为你的 App ID
CTI_FEISHU_APP_SECRET=xxx    # 替换为你的 App Secret

# Claude runtime
CTI_CLAUDE_CODE_EXECUTABLE=/path/to/claude  # 可选，默认自动检测
```

---

### 步骤 7：启动 Bridge

**📍 本地终端**

```bash
agents-to-im start
agents-to-im status  # 确认运行状态
```

启动成功后，可以看到：
- Bridge running (PID: xxx)
- Dashboard: http://127.0.0.1:13578

⚠️ **重要**：飞书在配置长连接时会校验应用连接状态，所以 bridge 必须先启动

---

## 第三阶段：事件订阅配置（飞书开放平台）

### 步骤 8：配置长连接事件

**📍 飞书开放平台 → 事件订阅**

链接格式：`https://open.feishu.cn/app/{你的App_ID}/event?tab=event`

1. 把「订阅方式」切换到 **「使用长连接接收事件」**（不是 Webhook）
2. 添加以下 4 个事件：
   - `im.message.receive_v1` — 接收消息
   - `im.message.message_read_v1` — 消息已读
   - `im.chat.updated_v1` — 群名修改同步
   - `im.chat.member.bot.added_v1` — Bot 加入群

---

### 步骤 9：添加卡片回调

**📍 飞书开放平台 → 回调配置**

链接格式：`https://open.feishu.cn/app/{你的App_ID}/event?tab=callback`

1. 添加回调：`card.action.trigger`

这个回调让 Bot 能接收卡片按钮点击（如权限审批的「允许/拒绝」）

---

### 步骤 10：第二次发布版本

**📍 飞书开放平台 → 版本管理与发布**

事件和回调配置后需要再次发布才能生效：
1. 创建新版本（如 `1.0.1`）
2. 提交审核并等待审批通过

---

## 第四阶段：测试与使用

### 步骤 11：测试 Bot

**📍 飞书 App**

1. 打开飞书
2. 搜索你的 Bot（应用名称）
3. 私聊 Bot，发送：`/new:claude`
4. Bot 会创建新群并弹出模式选择卡片

---

## 使用命令

### 私聊（控制面）

| 命令 | 说明 |
|------|------|
| `/new:claude` | 创建 Claude 会话群 |
| `/new:codex` | 创建 Codex 会话群 |
| `/resume:claude` | 恢复最近的 Claude 会话 |
| `/resume:codex` | 恢复最近的 Codex 会话 |

### 群聊内

| 命令 | 说明 |
|------|------|
| 普通消息 | 继续当前会话 |
| `/mode` | 切换 Claude 模式 |
| `/plan` | 进入交互式计划 |
| `/stop` | 中断当前输出 |
| `/reset` | 新会话，保留群 |

---

## 常用管理命令

**📍 本地终端**

```bash
agents-to-im start     # 启动
agents-to-im stop      # 停止
agents-to-im restart   # 重启（配置变更后）
agents-to-im status    # 查看状态
agents-to-im logs 100  # 查看最近日志
agents-to-im doctor    # 诊断问题
```

---

## 修改默认工作目录

**📍 本地终端**

1. 编辑配置文件：
   ```bash
   vim ~/.agents-to-im/config.env
   ```

2. 修改 `CTI_DEFAULT_WORKDIR` 的值

3. 重启 bridge：
   ```bash
   agents-to-im restart
   ```

---

## 故障排查

| 症状 | 检查项 |
|------|------|
| Bridge 启动失败 | `agents-to-im doctor` |
| 私聊没反应 | 应用是否已发布、Bot 是否启用、长连接是否配置 |
| 卡片按钮没反应 | `card.action.trigger` 回调是否已配置且版本已发布 |
| 权限报错 | 检查 scopes JSON 是否完整导入 |

---

## 参考链接

- 项目仓库：https://github.com/francize/agents-to-im
- 飞书开放平台：https://open.feishu.cn/app
- 本地 Dashboard：http://127.0.0.1:13578

---

#agents-to-im #飞书 #Claude-Code #配置指南