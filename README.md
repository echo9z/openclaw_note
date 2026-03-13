openclaw官方文档：https://docs.openclaw.ai/
ClawHub Skills 市场：https://clawhub.ai/
Vercel Skills：https://github.com/vercel-labs/skills
openclaw各类集合：https://github.com/hesamsheikh/awesome-openclaw-usecases
Node.js：https://nodejs.org/zh-cn/download/
skills市场：https://github.com/obra/superpowers
OpenClaw 中文教程：https://www.openclaw101.club/
### 介绍
OpenClaw 是一个**自托管网关** ，可将您喜爱的聊天应用（例如 WhatsApp、Telegram、Discord、iMessage 等）连接到像 Pi 这样的 AI 编码代理。您只需在自己的机器（或服务器）上运行一个网关进程，它就会成为您的消息应用和始终可用的 AI 助手之间的桥梁。

OpenClaw的核心架构：
![[Pasted image 20260312203633.png]]

1、Gateway（网关）
Gateway 是 OpenClaw 与外部世界交互的桥梁：
消息路由：将 IM（微信、Telegram、Discord 等）消息转发给对应的 Agent 处理结果回复：将 Agent 的响应发送回原始 IM 渠道多渠道支持：同时接入多个 IM 平台，统一管理

2、Agent（智能体）
Agent 是处理任务的实体：
会话管理：维护对话上下文，支持多轮对话任务执行：调用 Skills 和工具完成用户请求记忆机制：通过记忆系统保持长期上下文

3、Skills（技能）
Skills 是 OpenClaw 的能力扩展，类似于"插件"或"应用"：
模块化设计：每个 Skill 定义一组特定任务生态丰富：可从 ClawHub 安装第三方 Skills自定义开发：支持编写自定义 Skills 扩展能力典型 Skills：GitHub、Weather、Coding Agent 等

4、Channel（频道）
Channel 定义了消息的输入/输出渠道：
多平台接入：支持 Telegram、Discord、WhatsApp、Feishu、Signal 等配置灵活：每个Channel 可独立配置机器人行为消息格式：处理不同平台的消息格式差异

## 安装openclaw
自动安装openclaw
```bash
iwr -useb https://openclaw.ai/install.ps1 | iex
```
上述ps1脚本会自动安装node和openclaw

手动安装openclaw
当前文档安装环境：openclaw基于Node.v22，powershell7，git.v2.48自行安装
```bash
# 使用 npm
npm install -g openclaw@latest
# 或使用 pnpm
# pnpm add -g openclaw@latest
```

检测openclaw是否安装完成
```bash
➜ openclaw --version
OpenClaw 2026.3.8 (3caab92)
```

1.在windows以**管理员身份运行powershell**，在powershell中执行下面命令，进入openclaw指引向导。
```bash
# 安装系统服务并添加服务守护进程
openclaw onboard --install-daemon

# 仅运行配置向导
openclaw onboard
```
该向导会配置身份验证、网关设置和可选通道。具体参阅[新手引导向导（CLI）OpenClaw](https://docs.openclaw.ai/start/wizard)

2.当前是“默认个人使用模式”如果以后要给多人用或对外开放，需要自己做安全加固，现在只是确认你知道这件事？选择`yes`
```bash
 I understand this is personal-by-default and shared/multi-user use requires lock-down. Continue?
● Yes / ○ No
```
![[Pasted image 20260312205250.png]]

3.选择`QuickStart`快速开始配置：
![[Pasted image 20260312205636.png]]
QuickStart - 快速开始模式 
- 自动用默认配置启动，比如运行端口号，ip地址
- 细节以后可以用 openclaw configure修改
Manual - 手动配置模式  
- 自定义设置端口、权限、安全等参数
具体可以参考：[新手引导向导（CLI）- OpenClaw](https://docs.openclaw.ai/start/wizard#quickstart-defaults)

4.选中模型，以国内智普模型厂商为例，选择的`Z.AI`
![[Pasted image 20260312210433.png]]

5.选择所购买的厂商套餐，我购买的是智普国内的Coding-Plan-CN套餐，选择`Coding-Plan-CN `
![[Pasted image 20260312210641.png]]

6.选择`Paste API key now`，密钥将存在openclaw配置中。
![[Pasted image 20260312211402.png]]
或者`Use external secret provider`外部提要提供。

7.将api key复制到`Enter Z.AI API key`中；（提供购买套餐的api key即可，智普创建key：[api key](https://bigmodel.cn/usercenter/proj-mgmt/apikeys)）；配置Model configured模型为`zai/glm-4.7`
![[Pasted image 20260312212146.png]]

8.Channel 选择`Skip for now`暂时跳过，后面单独配置社交媒体机器人账号介绍，比如接入飞书和Discord。
![[Pasted image 20260312212639.png]]

9.Web search网页搜索功能可以暂时跳过，后面可以配置智普的mcp功能
选择`Skip for now`暂时跳过 (稍后可以使用 openclaw configure --section web进行配置)
![[Pasted image 20260312213452.png]]

10.Skills技能配置也先选择`no`暂时跳过，skills技能是什么后续可以介绍，或者阅读 Agent Skills官方文档：[Overview - Agent Skills](https://agentskills.io/home)
![[Pasted image 20260312213949.png]]

11.进入 Hooks 配置：新手选择`Skip for now`暂时跳过
![[Pasted image 20260312214351.png]]
hooks用来“在某些命令触发时自动执行动作”（例如 `/new` 时做会话记忆整理）。

这是我自己添加的hooks配置，供参考：
![[Pasted image 20260312215900.png]]
- 🚀 boot-md  
    启动时自动加载一个 Markdown 文件  
    类似启动说明或预设提示  
    一般新手不需要
- 📎 bootstrap-extra-files  
    启动时自动加载额外文件  
    用于高级自定义  
    普通使用不需要
- 📝command-logger  
    记录执行过的命令  
    方便排查问题  
    会保存执行记录
- 💾session-memory  
    保存会话记忆  
    让它在不同会话之间保留上下文  
    适合长期项目使用

12.然后选择的Web UI打开 OpenClaw Web 端，选择如何首次启动你的 Bot：选择`Open the Web UI`
```text
◇  How do you want to hatch your bot?
│  ○ Hatch in TUI (recommended)
│  ● Open the Web UI
│  ○ Do this later
● Open the Web UI
○ Run in background (daemon mode)
○ Exit (configure later)
```
Hatch in TUI (recommended) ：直接在终端进入交互式 TUI 界面，与 Bot 对话并设定人设。推荐选这个。 
Open the Web UI：打开浏览器 Web 控制面板完成初始化
Do this later： 跳过，以后再做
![[Pasted image 20260313141518.png]]

选了 `Open the Web UI`，一般会直接给出带 token 的 Dashboard 链接并尝试自动打开浏览器，会在浏览器自动打开 http://127.0.0.1:18789/。
浏览器打开页面，可以输入内容从进行测试openclaw是否正常。
![[Pasted image 20260313141652.png]]

检查网关
```bash
# 确认Gateway在运行
openclaw status
```
![[Pasted image 20260313151149.png]]

```bash
# 会在浏览器将下图URL自动打开
openclaw dashboard
```
![[Pasted image 20260313151250.png]]

遇到小问题
Gateway启动失败症状：向导提示Gateway failed to start
可能原因：
1. 端口18789被占用
2. 权限不足，windows使用管理员权限运行powershell7
3. 配置文件错误
解决方法
```bash
# 查看端口占用
lsof -i :18789 # 这是Linux下面

# 或者换端口启动
openclaw gateway start --port 18790
```

发送消息无回复症状：消息发送后，一直显示"正在输入"但没有回复
可能原因：
1. API Key错误
2. 网络不通
3. 模型服务异常
解决方法：
```bash
# 检查配置
openclaw config get

# 检查模型连接
openclaw doctor
```

Web UI打不开症状：浏览器访问127.0.0.1:18789显示无法连接
可能原因：
1. Gateway没启动
2. 防火墙阻挡
3. 地址输错
解决方法
```bash
# 确认Gateway在运行
openclaw status

# 如果未运行，手动启动
openclaw gateway start
```

## 启动openclaw
powershell7中运行opencalw
```bash
# 前台运行（适合调试）
openclaw gateway --port 18789 --verbose

# 使用守护进程（后台运行）
openclaw gateway start
```


## channel接入飞书

官方说明文档：[Chat Channels - OpenClaw](https://docs.openclaw.ai/channels/feishu)
### 打开平台并创建企业应用
1. 浏览器中打开飞书开发平台：https://open.feishu.cn/app。
2. 注册并登录飞书账号（需要有企业管理员权限）
3. 点击"创建企业自建应用"
![[Pasted image 20260313154003.png]]

填写应用信息：
- 应用名称：建议用"OpenClaw"或"ClawBot"
- 应用描述：内部使用的AI助手
- 图标：可以上传一个喜欢的图标
![[Pasted image 20260313154429.png]]

### 获取App ID与App Secret
创建完成应用，点击ClawBot应用进入详情页面：
1. 点击左侧"凭证与基础信息"
2. 记录以下信息：
	- App ID（形如cli_xxxxxxxxxxxxxxxx）
	- App Secret（点击"查看"按钮显示）
![[Pasted image 20260313154704.png]]
>**注意**：App Secret务必保密，不要截图外传，不要发到群里。泄露了别人就能控制你的机器人。

### OpenClaw添加飞书channel

1.运行添加channels命令：
```bash
openclaw channels add
```

2.添加聊天通道配置，选择`yes`
![[Pasted image 20260313181147.png]]

3.选择channel类型为`Feishu/Lark`
![[Pasted image 20260313181218.png]]

4.我选择`Use local plugin path`，本地在安装openclaw已经安装好了`@openclaw/feishu`插件，没有或可以选择`Download from npm`，进行npm安装飞书插件。
```bash
│  ○ Download from npm (@openclaw/feishu)  # 通过npm下载feishu插件
│  ● Use local plugin path
│  (C:\Users\xxx\AppData\Roaming\fnm\node-versions\v22.20.0\installation\node_modules\openclaw\extensions\feishu)
│  ○ Skip for now
```
![[Pasted image 20260313184818.png]]

5.添加`Feishu`应用程序密钥，选择`Enter App Secret`
```bash
◆  How do you want to provide this App Secret?
│  ● Enter App Secret (Stores the credential directly in OpenClaw config) # 直接将凭证存储在 OpenClaw 配置中
│  ○ Use external secret provider
```

将前面获取的app id和secret，复制到对话框中即可。
![[c355716e-797e-49f8-b254-c0f48b6c8ba3.png]]

6.Feishu的连接选择`WebSocket`，使用长连接接收事件
```bash
◇  Feishu connection mode
│  ● WebSocket (default)
│  ○ Webhook
```

7.选择飞书域名，选择`feishu.cn`国内域名
```bash
◆  Which Feishu domain?
│  ● Feishu (feishu.cn) - China
│  ○ Lark (larksuite.com) - International
```

8.选择群聊响应策略，选择`Open - respond in all groups`
```bash
 Group chat policy
│  ○ Allowlist - only respond in specific groups
│  ● Open - respond in all groups (requires mention)
│  ○ Disabled - don't respond in groups
```
- ● Open - respond in all groups（在所有群组中回复）
- ○ Allowlist - only respond to specific users（仅在特定群组中响应）
- ○ Disabled - don't respond in groups（不响应任何群组）
![[Pasted image 20260313200740.png]]
最后，选择 `Finished`，然后按提示完成`Finshed`配置即可。
![[Pasted image 20260313201305.png]]

完成之后，会继续让你选择访问策略`yes`。（**DM (Direct Message)** 指的是用户在微信、Telegram 或 Discord 等平台上与你的机器人进行的一对一私聊。这个选项是在问你，是否要在当前设置阶段就自定义“谁可以私聊你的机器人”。）
```bash
 Configure DM access policies now? (default: pairing)
│  ● Yes / ○ No
```
1.Feishu DM policy 选择`yes`
2.Add display names for these accounts? (optional)选择`No`
3.Bind configured channel accounts to agents now?选择`yes`
4.Route feishu account "default" to agent feishu绑定到模型的

```bash
◇  Configure DM access policies now? (default: pairing)
│  Yes 
│
◇  Feishu DM access ─────────────────────────────────────────────────────────────────────────╮
│                                                                                            │
│  Default: pairing (unknown DMs get a pairing code).                             │
│  Approve: openclaw pairing approve feishu <code>                                │
│  Allowlist DMs: channels.feishu.dmPolicy="allowlist" + channels.feishu.allowFrom entries.  │
│  Public DMs: channels.feishu.dmPolicy="open" + channels.feishu.allowFrom includes "*".     │
│  Multi-user DMs: run: openclaw config set session.dmScope "per-channel-peer" (or           │
│  "per-account-channel-peer" for multi-account channels) to isolate sessions.               │
│  Docs: channels/pairing               │
│                                                                                            │
├────────────────────────────────────────────────────────────────────────────────────────────╯
│
◇  Feishu DM policy
│  Open (public inbound DMs)
│
◇  Add display names for these accounts? (optional)
│  No
│
◇  Bind configured channel accounts to agents now?
│  Yes
│
◇  Route feishu account "default" to agent
│  main (default)
│
◇  Routing bindings ────────────────╮
│                                   │
│  Added: feishu accountId=default  │
│                                   │
├───────────────────────────────────╯
Config overwrite: C:\Users\11312\.openclaw\openclaw.json (sha256 xxxxxxc4ae1ed33a4e399b071d7325a2f67aad19e54e44085709dc46dc -> xxxxd6ccdf38c55ef56d7f04247f908c0eb487613a07f7260009004b, backup=C:\Users\11312\.openclaw\openclaw.json.bak)
│
└  Channels updated.
```
![[Pasted image 20260313204745.png]]
可以通过 `C:\Users\11312\.openclaw\openclaw.json` 查看对应的 channel 配置

配置完成之后，重启openclaw
```bash
openclaw gateway restart
```

```bash
# openclaw添加channel插件命令
openclaw plugins enable/disabel feishu # 安装以后可以启用/禁用

# 查看openclaw下的安装了那些plugins插件
openclaw plugins list
```

### 添加机器人应用能力
点击左侧“添加应用能力”，按能力添加，选择机器人框，点击添加。
![[Pasted image 20260313165041.png]]
设置机器人名称
![[Pasted image 20260313173609.png]]

### 添加权限管理
点击权限管理，为机器人应用添加相关权限，我们这里以下面json为例，将json内容直接复制JSON文本框中
```json
 {
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "cardkit:card:write",
      "contact:contact.base:readonly",
      "contact:user.base:readonly",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "event:ip_list",
      "im:chat.members:bot_access",
      "im:chat:readonly",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.group_msg",
      "im:message.p2p_msg:readonly",
      "im:message.reactions:read",
      "im:message:readonly",
      "im:message:recall",
      "im:message:send_as_bot",
      "im:message:update",
      "im:resource"
    ],
    "user": [
      "aily:file:read",
      "aily:file:write",
      "contact:contact.base:readonly",
      "im:chat.access_event.bot_p2p_chat:read"
    ]
  }
}
```
![[Pasted image 20260313172433.png]]

机器人应用权限，具体对应说明可以点击开通权限进行查找
```txt
应用身份权限tenant_access_token
tenant
"aily:file:read",获取文件
"aily:file:write", 上传文件
"application:application.app_message_stats.overview:readonly", 获取机器人发送消息的概览数据
"application:application:self_manage", 管理应用自身资源
"application:bot.menu:write", 创建、更新、删除机器人菜单
"contact:user.employee_id:readonly", 获取用户 user ID
"corehr:file:download", 下载文件
"cardkit:card:write", 创建与更新卡片
"contact:contact.base:readonly", 获取通讯录基本信息
"contact:user.base:readonly", 获取用户基本信息
"event:ip_list", 获取事件的出口 IP
"im:chat:readonly", 获取群组信息
"im:chat.members:bot_access", 订阅机器人进、出群事件
"im:message", 获取与发送单聊、群组消息
"im:message.group_at_msg:readonly", 接收群聊中@机器人消息事件
"im:message.group_msg", 获取群组中所有消息（敏感权限）
"im:message.p2p_msg:readonly",   读取用户发给机器人的单聊消息
"im:message.reactions:read",   查看消息表情回复
"im:message:readonly",   获取单聊、群组消息
"im:message:recall",  撤回消息
"im:message:send_as_bot",   以应用的身份发消息
"im:message:update",   更新消息
"im:resource" 获取与上传图片或文件资源

用户身份权限user_access_token
user
"aily:file:read",获取文件
"aily:file:write", 上传文件
"im:chat.access_event.bot_p2p_chat:read" 订阅访问机器人会话相关事件
"contact:contact.base:readonly" 获取通讯录基本信息
```
![[Pasted image 20260313171943.png]]

### 开启事件订阅
在飞书平台开启事件订阅
1. 回到飞书开放平台
2. 点击左侧"事件与回调"
3. 在"事件订阅方式"中，选择"使用 **长连接** 接收事件"，点击保存
![[Pasted image 20260313205524.png]]

添加事件订阅在"订阅事件"区域，点击"添加事件"：
1. 搜索im.message.receive_v1
2. 勾选并确认添加这个事件表示"收到消息时通知我"。
![[Pasted image 20260313205857.png]]

处理上面接收消息事件，还可以添加下面几项订阅事件，比如将机器人加入群聊等。

| 事件                            | 说明                 |
| ----------------------------- | ------------------ |
| im.message.receive_v1         | 接收消息(必需)，用于单独聊天机器人 |
| im.message.message_read_v1    | 消息已读回执             |
| im.chat.member.bot.added_v1   | 机器人进群              |
| im.chat.member.bot.deleted_v1 | 机器人被移出群            |
### 接入应用测试
以上步骤全部完成后，即可与机器人对话。但在此之前需要先创建一个版本
![[Pasted image 20260313210219.png]]

创建一个版本号，填写更新说明，页面滑倒最底部点击`保存`。
![[Pasted image 20260313210308.png]]

发布完成后，回到飞书客户端，可以看到应用已上线，点击打开应用
![[Pasted image 20260313210515.png]]

向机器人发送`你好`，或者问`你是什么模型`即可完成飞书的接入。
![[Pasted image 20260313210927.png]]

创建一个测试群@机器人：
![[Pasted image 20260313214903.png]]

点击群右上角三个点，点击群机器人，再点击`添加机器人`，搜索框输入机器人名称`ClawBot`，将机器人添加群里即可。
![[Pasted image 20260313215136.png]]

群里面@ClawBot机器即可
![[Pasted image 20260313215804.png]]
OpenClaw可以正常回复，说明飞书接入成功了。

## OpenClaw 常用命令速查

安装完成后，以下是日常使用中最常用的 OpenClaw 命令：

| 命令                               | 功能               |
| -------------------------------- | ---------------- |
| `openclaw status`                | 查看 OpenClaw 运行状态 |
| `openclaw onboard`               | 重新进入配置向导         |
| `openclaw gateway start`         | 启动服务             |
| `openclaw gateway stop`          | 停止服务             |
| `openclaw gateway restart`       | 重启服务             |
| `openclaw update`                | 更新到最新版本          |
| `openclaw health`                | 健康检查             |
| `openclaw doctor`                | 诊断问题             |
| `openclaw dashboard`             | 获取 Web UI 访问链接   |
| `openclaw security audit --deep` | 安全审计             |
| `openclaw uninstall`             | 卸载 OpenClaw      |

参考：https://mp.weixin.qq.com/s/0Xq9XOfTjnQYwqXOVe9rZg
[Windows快速部署OpenClaw + Qwen3.5 Plus完整教程 – 大绵羊博客](https://dmyblog.cn/3009.html)
[OpenClaw 全流程部署指南 - 晚安的静谧小屋](https://blog.goodnightan.com/posts/openclaw-install/)
[OpenClaw 最新保姆级飞书对接指南教程 搭建属于你的 AI 助手-阿里云开发者社区](https://developer.aliyun.com/article/1711222)