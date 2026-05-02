---
tags: [concept]
created: 2026-04-21
updated: 2026-04-21
sources: ""
related: ""
---

# agents-to-im 飞书 Bot 搭建指南

> 将 Claude Code 编码代理桥接到飞书/Lark，实现私聊控制、群聊会话隔离的完整搭建过程。

## 前置条件

- Node.js 20+ 已安装
- 飞书开放平台账号
- 本地已安装并认证 Claude Code CLI

---

## 四阶段搭建流程

| 阶段 | 内容 | 关键步骤 |
|------|------|---------|
| **1** | 飞书应用创建 | 创建应用 → 开启 Bot → 导入权限 scopes JSON → 第一次发布 |
| **2** | 本地安装配置 | npm install → 创建 config.env → 启动 bridge |
| **3** | 事件订阅配置 | 长连接（不是 Webhook）→ 4 个事件 → 卡片回调 → 第二次发布 |
| **4** | 测试使用 | 飞书搜 Bot → 私聊 /new:claude → 创建群聊会话 |

---

## 第一阶段：飞书应用创建与配置

### 步骤 1：创建飞书自建应用

1. 打开 https://open.feishu.cn/app
2. 点击「创建自建应用」
3. 记录 App ID 和 App Secret

### 步骤 2：开启 Bot 能力

进入「添加应用能力」→ 启用「机器人」

### 步骤 3：导入权限 scopes JSON

权限管理页面导入完整 JSON（见原文）

### 步骤 4：第一次发布版本

⚠️ 没有发布前，Bot 和权限都不会生效

---

## 第二阶段：本地安装与配置

```bash
npm install -g agents-to-im
mkdir -p ~/.agents-to-im
```

config.env 最小配置：

```env
CTI_DEFAULT_WORKDIR=/你的/工作目录/路径
CTI_FEISHU_APP_ID=cli_xxx
CTI_FEISHU_APP_SECRET=xxx
```

启动：

```bash
agents-to-im start
agents-to-im status
```

⚠️ bridge 必须先启动，飞书长连接会校验连接状态

---

## 第三阶段：事件订阅配置

### 步骤 8：配置长连接事件

切换到「使用长连接接收事件」（不是 Webhook），添加 4 个事件：

- im.message.receive_v1 — 接收消息
- im.message.message_read_v1 — 消息已读
- im.chat.updated_v1 — 群名修改同步
- im.chat.member.bot.added_v1 — Bot 加入群

### 步骤 9：添加卡片回调

添加回调：card.action.trigger

### 步骤 10：第二次发布版本

事件和回调配置后需要再次发布

---

## 使用命令

### 私聊（控制面）

| 命令 | 说明 |
|------|------|
| /new:claude | 创建 Claude 会话群 |
| /resume:claude | 恢复最近会话 |

### 群聊内

| 命令 | 说明 |
|------|------|
| /mode | 切换 Claude 模式 |
| /stop | 中断输出 |
| /reset | 新会话 |

---

## 故障排查

| 症状 | 检查项 |
|------|------|
| 私聊没反应 | 应用是否已发布、Bot 是否启用 |
| 卡片按钮没反应 | card.action.trigger 回调是否配置 |
| 权限报错 | scopes JSON 是否完整导入 |

---

## 已知问题

### 项目切换受限

**现象**：飞书 App（手机端/电脑端）的卡片项目列表中，无法直接选择切换项目，只能使用默认项目。

**临时解决办法**：
1. 更新 `~/.agents-to-im/config.env` 中的 `CTI_DEFAULT_WORKDIR` 为目标项目路径
2. 重启 bridge：`agents-to-im restart`
3. 在飞书 Bot 卡片下拉列表中会出现历史项目，此时可切换

**限制**：下拉列表最多显示最近 5 个项目，超过 5 个的历史项目可能无法访问

---

## 参考链接

- 项目仓库：https://github.com/francize/agents-to-im
- 本地 Dashboard：http://127.0.0.1:13578
