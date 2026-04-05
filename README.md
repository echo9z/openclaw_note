openclaw官方文档：https://docs.openclaw.ai/
ClawHub Skills 市场：https://clawhub.ai/
Vercel Skills：https://github.com/vercel-labs/skills
openclaw各类集合：https://github.com/hesamsheikh/awesome-openclaw-usecases
Node.js：https://nodejs.org/zh-cn/download/
skills市场：https://github.com/obra/superpowers
OpenClaw 中文教程：https://www.openclaw101.club/
github社区分享的案例：https://github.com/hesamsheikh/awesome-openclaw-usecases

不错教程中文教程：
[OpenClaw - OpenClaw 中文网](https://www.clawfather.cn/docs-home)
[OpenClaw101 教程 - OpenClaw101 - OpenClaw101](https://www.openclaw101.club/docs/)
[OpenClaw 中文站 - 文档、安装指南、Skills 与新手帮助](https://openclaw.cc/)
[xianyu110/awesome-openclaw-tutorial: 从零开始玩转OpenClaw：最全面的中文教程，涵盖安装、配置、实战案例和避坑指南](https://github.com/xianyu110/awesome-openclaw-tutorial)
[OpenClaw完全指南](https://my.feishu.cn/wiki/QzGAwOH4LiZOYXktGyhcHoLUnRe)
## 介绍
OpenClaw 是一个**自托管网关** ，可将您喜爱的聊天应用（例如 WhatsApp、Telegram、Discord、iMessage 等）连接到像 Pi 这样的 AI 编码代理。您只需在自己的机器（或服务器）上运行一个网关进程，它就会成为您的消息应用和始终可用的 AI 助手之间的桥梁。

OpenClaw的核心架构：
![](./assets/img/oc1.png)

### Gateway（网关）

Gateway 是 OpenClaw 的核心守护进程，它是整个系统的「大脑」和「中枢」。Gateway 负责：
- **消息路由**：接收来自各个渠道（Channel）的消息，转发给 AI 模型处理，再将回复发送回对应的渠道
- **会话管理**：维护每个对话的上下文、历史记录和状态
- **技能调度**：根据 AI 的判断，调用合适的技能（Skill）来完成任务
- **设备协调**：管理和协调所有连接的设备节点（Node）
Gateway 通常运行在你的服务器（VPS）上，作为一个后台服务持续运行。你可以通过 `openclaw gateway start` 命令启动它，通过 `openclaw gateway status` 查看其运行状态。

### Session（会话）

Session 是一次对话的上下文容器。每当你在某个渠道上与 AI 开始对话时，OpenClaw 会创建或恢复一个 Session。Session 中包含：
- 对话历史（你说了什么，AI 回复了什么）
- 上下文状态（当前正在做什么任务）
- 记忆数据（AI 记住了哪些重要信息）
- 配置参数（使用哪个模型、什么人格等）
Session 是 OpenClaw 实现「连续对话」和「记忆」的基础。即使你关闭了聊天窗口，下次回来时 AI 依然记得之前的对话内容。

### Channel（渠道）

Channel 是 OpenClaw 与外部世界的连接点。每个 Channel 代表一个通讯平台的接入，例如：
- **WhatsApp Channel**：通过 WhatsApp Business API 接收和发送消息
- **Telegram Channel**：通过 Telegram Bot API 与用户交互
- **Discord Channel**：作为 Discord Bot 参与服务器对话
- **飞书 Channel**：通过飞书开放平台接入
- **钉钉 Channel**：通过钉钉机器人接口接入
- **QQ Channel**：通过 QQ 机器人接口接入
- **微信 Channel**：通过微信接口接入
每个 Channel 都是独立的插件，你可以根据需要启用或禁用。一个 Gateway 可以同时连接多个 Channel，实现「一个 AI，多个入口」的效果。

### Agent（智能体）

Agent 是 OpenClaw 中的 AI 实体。一个 Agent 包含了：

- **模型配置**：使用哪个 LLM（如 Claude、GPT-4、Gemini）
- **人格设定**：系统提示词、行为规则、个性特征
- **技能列表**：Agent 可以使用哪些技能
- **权限范围**：Agent 可以做什么、不能做什么

可以配置多个不同的 Agent，每个 Agent 有不同的人格和能力。比如一个专门处理工作消息的「工作助手」，一个负责日常聊天的「生活伙伴」，还有一个擅长编程的「代码助手」。

### Skill（技能）

Skill 是 OpenClaw 的能力扩展机制。每个 Skill 定义了 Agent 可以使用的一种工具或能力：
- **内置技能**：文件读写、网页搜索、浏览器控制、代码执行、图片处理等
- **通讯技能**：发送消息、管理群组、处理通知等
- **设备技能**：控制手机摄像头、获取位置、执行远程命令等
- **自定义技能**：你可以编写自己的技能，实现任何你需要的功能
Skill 系统让 OpenClaw 不仅仅是一个「聊天机器人」，而是一个真正能「做事」的 AI 助手。AI 可以根据对话内容自主判断何时需要使用哪些技能，无需人工干预。

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
# 安装系统服务并添加服务守护进程(首次运行)
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
![](./assets/img/ins1.png)

3.选择`QuickStart`快速开始配置：
![](./assets/img/ins2.png)
QuickStart - 快速开始模式 
- 自动用默认配置启动，比如运行端口号，ip地址
- 细节以后可以用 openclaw configure修改
Manual - 手动配置模式  
- 自定义设置端口、权限、安全等参数
具体可以参考：[新手引导向导（CLI）- OpenClaw](https://docs.openclaw.ai/start/wizard#quickstart-defaults)

4.选中模型，以国内智普模型厂商为例，选择的`Z.AI`
![](./assets/img/ins3.png)

5.选择所购买的厂商套餐，我购买的是智普国内的Coding-Plan-CN套餐，选择`Coding-Plan-CN `
![](./assets/img/ins4.png)

6.选择`Paste API key now`，密钥将存在openclaw配置中。
![](./assets/img/ins5.png)
或者`Use external secret provider`外部提要提供。

7.将api key复制到`Enter Z.AI API key`中；（提供购买套餐的api key即可，智普创建key：[api key](https://bigmodel.cn/usercenter/proj-mgmt/apikeys)）；配置Model configured模型为`zai/glm-4.7`
![](./assets/img/ins6.png)

8.Channel 选择`Skip for now`暂时跳过，后面单独配置社交媒体机器人账号介绍，比如接入飞书和Discord。
![](./assets/img/ins7.png)

9.Web search网页搜索功能可以暂时跳过，后面可以配置智普的mcp功能
选择`Skip for now`暂时跳过 (稍后可以使用 openclaw configure --section web进行配置)
![](./assets/img/ins8.png)

10.Skills技能配置也先选择`no`暂时跳过，skills技能是什么后续可以介绍，或者阅读 Agent Skills官方文档：[Overview - Agent Skills](https://agentskills.io/home)
![](./assets/img/ins9.png)

11.进入 Hooks 配置：新手选择`Skip for now`暂时跳过
![](./assets/img/ins10.png)
hooks用来“在某些命令触发时自动执行动作”（例如 `/new` 时做会话记忆整理）。

这是我自己添加的hooks配置，供参考：
![](./assets/img/ins11.png)
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
![](./assets/img/ins12.png)

选了 `Open the Web UI`，一般会直接给出带 token 的 Dashboard 链接并尝试自动打开浏览器，会在浏览器自动打开 http://127.0.0.1:18789/
浏览器打开页面，可以输入内容从进行测试openclaw是否正常。
![](./assets/img/ins13.png)

检查网关
```bash
# 确认Gateway在运行
openclaw status
```
![](./assets/img/ins14.png)

```bash
# 会在浏览器将下图URL自动打开
openclaw dashboard
```
![](./assets/img/ins15.png)

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

# 使用守护进程（后台运行）|停止|重启
openclaw gateway start|stop|restart
```

## 接入feishu

官方说明文档：[Chat Channels - OpenClaw](https://docs.openclaw.ai/channels/feishu)

### 0.快速创建飞书机器人应用（推荐）

打开飞书官方快速生成机器人应用页面，登录飞书账户。 https://open.feishu.cn/page/openclaw?form=multiAgent
飞书官方已经会把权限和回调自动配置好。你只需要输入名称和选个头像即可。将生产的app_id和app_secret复制到记事本中备用。直接浏览操作[3.OpenClaw添加飞书channel](### 3.OpenClaw添加飞书channel)。4到6步骤已经用飞书官方快速生成机器人应用页面完成了，不需再进行繁琐的配置
![[ag0.png]]

### 1.打开平台并创建企业应用
1. 浏览器中打开飞书开发平台：https://open.feishu.cn/app。
2. 注册并登录飞书账号（需要有企业管理员权限）
3. 点击"创建企业自建应用"
![](./assets/img/fei1.png)

填写应用信息：
- 应用名称：建议用"OpenClaw"或"ClawBot"
- 应用描述：内部使用的AI助手
- 图标：可以上传一个喜欢的图标
![](./assets/img/fei2.png)

### 2.获取App ID与App Secret
创建完成应用，点击ClawBot应用进入详情页面：
1. 点击左侧"凭证与基础信息"
2. 记录以下信息：
	- App ID（形如cli_xxxxxxxxxxxxxxxx）
	- App Secret（点击"查看"按钮显示）
![](./assets/img/fei3.png)
>**注意**：App Secret务必保密，不要截图外传，不要发到群里。泄露了别人就能控制你的机器人。

### 3.OpenClaw添加飞书channel

1.终端运行添加channels命令：
```bash
openclaw channels add
```

2.添加聊天通道配置，选择`yes`
![](./assets/img/fei4.png)

3.选择channel类型为`Feishu/Lark`
![](./assets/img/fei5.png)

4.我选择`Use local plugin path`，本地在安装openclaw已经安装好了`@openclaw/feishu`插件，没有或可以选择`Download from npm`，进行npm安装飞书插件。
```bash
│  ○ Download from npm (@openclaw/feishu)  # 通过npm下载feishu插件
│  ● Use local plugin path
│  (C:\Users\xxx\AppData\Roaming\fnm\node-versions\v22.20.0\installation\node_modules\openclaw\extensions\feishu)
│  ○ Skip for now
```
![](./assets/img/fei6.png)

5.添加`Feishu`应用程序密钥，选择`Enter App Secret`
```bash
◆  How do you want to provide this App Secret?
│  ● Enter App Secret (Stores the credential directly in OpenClaw config) # 直接将凭证存储在 OpenClaw 配置中
│  ○ Use external secret provider
```

将前面获取的app id和secret，复制到对话框中即可。
![](./assets/img/fei7.png)

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
![](./assets/img/fei8.png)
最后，选择 `Finished`，然后按提示完成`Finshed`配置即可。
![](./assets/img/fei9.png)

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
![](./assets/img/fei10.png)
可以通过 `C:\Users\11312\.openclaw\openclaw.json` 查看对应的 channel 配置

配置完成之后，重启openclaw
```bash
openclaw gateway restart
```

```bash
# openclaw添加channel插件命令
openclaw plugins enable/disable feishu # 安装以后可以启用/禁用

# 查看openclaw下的安装了那些plugins插件
openclaw plugins list
```

### 4.添加机器人应用能力
点击左侧“添加应用能力”，按能力添加，选择机器人框，点击添加。
![](./assets/img/fei11.png)
设置机器人名称
![](./assets/img/fei12.png)

### 5.添加权限管理
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
![](./assets/img/fei13.png)

机器人应用权限，具体对应说明可以点击开通权限进行查找
- 应用身份权限tenant_access_token
- 用户身份权限user_access_token
![](./assets/img/fei14.png)

### 6.开启事件订阅
在飞书平台开启事件订阅
1. 回到飞书开放平台
2. 点击左侧"事件与回调"
3. 在"事件订阅方式"中，选择"使用 **长连接** 接收事件"，点击保存
![](./assets/img/fei15.png)

添加事件订阅在"订阅事件"区域，点击"添加事件"：
1. 搜索im.message.receive_v1
2. 勾选并确认添加这个事件表示"收到消息时通知我"。
![](./assets/img/fei16.png)

处理上面接收消息事件，还可以添加下面几项订阅事件，比如将机器人加入群聊等。

| 事件                            | 说明                 |
| ----------------------------- | ------------------ |
| im.message.receive_v1         | 接收消息(必需)，用于单独聊天机器人 |
| im.message.message_read_v1    | 消息已读回执             |
| im.chat.member.bot.added_v1   | 机器人进群              |
| im.chat.member.bot.deleted_v1 | 机器人被移出群            |
| im.message.recalled_v1        | 消息撤回               |
### 7.接入应用测试
以上步骤全部完成后，即可与机器人对话。但在此之前需要先创建一个版本
![](./assets/img/fei17.png)

创建一个版本号，填写更新说明，页面滑倒最底部点击`保存`。
![](./assets/img/fei18.png)

发布完成后，回到飞书客户端，可以看到应用已上线，点击打开应用
![](./assets/img/fei19.png)

向机器人发送`你好`，或者问`你是什么模型`即可完成飞书的接入。
![](./assets/img/fei20.png)

创建一个测试群@机器人：
![](./assets/img/fei21.png)

点击群右上角三个点，点击群机器人，再点击`添加机器人`，搜索框输入机器人名称`ClawBot`，将机器人添加群里即可。
![](./assets/img/fei22.png)

群里面@ClawBot机器，OpenClaw可以正常回复，说明飞书接入成功了。
![](./assets/img/fei23.png)

## 接入discord
对于有外网条件的同学，可以考虑接入Discord，该应用多频道(channel)的设计天生就适合 OpenClaw 的多Agent场景使用。参考：[Discord - OpenClaw](https://docs.openclaw.ai/channels/discord)

### 1.新建 discord 应用

浏览器打开：https://discord.com/developers/applications
1. 点击 **New Application**，输入应用名称，创建一个新app
![](./assets/img/dis1.png)

2. 在左侧菜单中点击 **Bot** 页面，修改用户名`ClawBot`机器人名称。点击 **Reset Token** 重制令牌获取 Bot Token（只显示一次，请妥善保存），生成的Token复制保存备用，共后续配置channel中。
![](./assets/img/dis3.png)
 开启以下 Intents：下滑页面开启三个意图配置，点击保存更改。
![](./assets/img/dis2.png)
- ✅ Presence Intent（可选，接收在线状态更新事件）
- ✅ Message Content Intent（必须，否则无法读取消息内容）
- ✅ Server Members Intent（推荐，角色允许列表 allowlis 和名称与ID匹配时需要使用）

配置Bot权限：点击页面左侧的**OAuth2**，在接下来的页面中选择**OAuth2 URL Generator**，先配置**Scopes**，建议先按照最小化原则勾选 bot 、 applications.commands 。
![](./assets/img/dis4.png)
下滑继续配置**Bot Permissions**，勾选 View Channels、Send Messages、Read Message History、Embed Links、Attach Files、Add Reactions (可选)。
![](./assets/img/dis5.png)

复制页面底部生成的链接（Generated URL）
![](./assets/img/dis6.png)

将复制链接在浏览器中打开，在弹窗中选择您希望添加Bot的Discord服务器（需要您具有服务器的管理员权限）。点击继续（Continue）后进行授权，即可邀请bot加入对应的Discord服务器。
![](./assets/img/dis7.png)
![](./assets/img/dis8.png)
在页面或app点击自己channel，点击`综合`频道，看到ClawBot添加完成的效果，如下图所示：
![](./assets/img/dis9.png)

### 2.配置openclaw channel

1.在终端运行添加channels命令：
```bash
openclaw channels add
```
添加channel配置，选择`yes`
```bash
*  Configure chat channels now?
|  > Yes /   No
```

2.选择 Discord（Bot API）：
![](./assets/img/dis10.png)

3.选择Discord账号，选择`default (primary)`
```bash
*  Discord account
|  > default (primary)
|    Add a new account
```

4.提供Discord bot token令牌，选择`Enter Discord bot token`
```bash
*  How do you want to provide this Discord bot token?
|  > Enter Discord bot token (Stores the credential directly in OpenClaw config)
|    Use external secret provider
```

5.将Discord bot的令牌，复制到终端中
```bash
o  Enter Discord bot token
|  复制Discord bot的令牌
```
![](./assets/img/dis11.png)

6.配置访问权限，选择`yes`
```bash
*  Configure Discord channels access?
|  > Yes /   No
```
![](./assets/img/dis12.png)

7.访问权限模式，选择 Open (allow all channels) 。
![](./assets/img/dis13.png)

8.最后，选择 `Finished`，然后按提示完成`Finshed`配置即可。
![](./assets/img/dis14.png)

9.后续的访问策略`yes`
```bash
o  Configure DM access policies now? (default: pairing)
|  Yes

o  Discord DM policy
|  Open (public inbound DMs)

o  Add display names for these accounts? (optional)
|  No

o  Bind configured channel accounts to agents now?
|  Yes

o  Route discord account "default" to agent
|  main (default)

o  Routing bindings
|  Added: discord accountId=default
```

如果出现`discord: failed to deploy native commands: fetch failed`问题？可以在`.openclaw/openclaw.json`，添加配置`channels.discord.proxy`，设置路由 Discord 网关 WebSocket 流量和启动 REST 查询（应用 ID + 允许列表解析），具体阅读[Discord - OpenClaw](https://docs.openclaw.ai/channels/discord#gateway-proxy)。
```json
 "channels": {
    "discord": {
      "proxy": "http://127.0.0.1:7897", # 代理网络端口
      "accounts": {
        "primary": {
          "proxy": "http://127.0.0.1:7897",
        },
      },
     ... ...
    },
 }
```

## 接入qq

### 注册QQ开放平台

1.前往QQ开放平台：https://q.qq.com/qqbot/openclaw/login.html，可以用手机QQ扫描图中二维码进行注册/登录。或者注册QQ开放账户[QQ开放平台](https://q.qq.com/#/)。

>没有未注册QQ开放平台，执行扫码操作后，系统将自动完成QQ开放平台注册流程，将扫码所用的QQ账号与该平台账号进行绑定。
![](./assets/img/qq1.png)

2.在手机QQ扫码后选择同意绑定后，即完成注册，进入QQ机器人配置页。
### 创建一个QQBot机器人
3.点击创建机器人，手机端APP上会出现机器人被创建成功消息。
![](./assets/img/qq2.png)

4.机器人创建完成后，将 “AppID” 和 “AppSecret” 两个参数，复制保存到个人记事本或备忘录中（请妥善保存勿泄露，注意数据安全），后续步骤中需要使用。
![](./assets/img/qq3.png)

### 安装openclaw-qqbot插件
1.安装qqbot的openclaw的插件，打开终端输入：
```bash
openclaw plugins install @tencent-connect/openclaw-qqbot@latest

# 确认是否将qqbot插件安装到openclaw中
openclaw plugins list
```
2.配置绑定当前QQ机器人
```bash
openclaw channels add --channel qqbot --token "xxxxx:xxxxxxxxxxx"
```
3.重启本地openclaw服务
```bash
openclaw gateway restart
```
在qq应用对聊天机器人发送消息，正常回复接入完成
![](./assets/img/qq4.png)
接入参考：https://cloud.tencent.com/developer/article/2626045


## 接入个人微信

首先将手机微信升级到最新版本。进入设置 -> 点击插件里面能看到
![](./assets/wx1.png)
说明版本更新到最新了，支持接入微信。

然后再你的服务器上安装微信的 OpenClaw 插件：

```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```
![](./assets/wx2.png)
等待微信插件生成二维码，通过手机微信进行扫码，即可openclaw与个人微信连接完成。
![](./assets/wx3.png)
如何你不想使用该插件，openclaw plugins disable禁用该微信插件
```bash
openclaw plugins disable openclaw-weixin
```

## 终端聊天界面（TUI)
TUI界面即openclaw cli命令行窗口对话。
1. 打开 TUI。
```
openclaw tui
```

2. 输入消息并按 Enter。远程 Gateway 网关：
```
openclaw tui --url ws://<host>:<port> --token <gateway-token>
```
如果 Gateway 网关使用密码认证，请使用 `--password`。

### /new
`/new`新建一个session对话
![](./assets/tui1.png)
### /session
终端对话框中输入`/session`，切换某一个agent的对话记录，类似claude中`resume`恢复会话功能。
![](./assets/tui2.png)
### /model
配置过多个模型，可以使用 `/model 模型标识` 快捷切换模型
![](./assets/tui3.png)

### /verbose
`/verbose` 开启详细推理模式，会显示我的思考过程（thinking）。
运行时可以随时输入：
```bash
/verbose on   # 开启
/verbose off  # 关闭
```
开启后我会把内部推理步骤展示出来。

### /status
查看当前会话使用状态，使用的模型、上下文使用多少、使用时常等待。
![](./assets/tui4.png)

## 打开 Web 控制面板

可以通过浏览器和 Bot 对话：

```bash
openclaw dashboard
```

浏览器会自动打开 `http://localhost:18789`，一个web聊天界面。

> 如果 onboard 时显示了带 token 的 URL（如 `http://127.0.0.1:18789/#token=xxxxx`），可以直接用该 URL 访问，无需手动输入 token。


## 配置其他模型

### 配置MiniMax
1.执行以下命令，进入 OpenClaw 模型配置界面
```bash
openclaw config
```

2.选择网关运行位置，选择本地`Local gateway`
```bash
◆  Where will the Gateway run?
│  ● Local (this machine) (No gateway detected (ws://127.0.0.1:18789))
│  ○ Remote (info-only)
```

3.选择配置`Model`模块
```bash
◆  Select sections to configure
│  ○ Workspace
│  ● Model (Pick provider + credentials)
│  ○ Web tools
│  ○ Gateway
│  ○ Daemon
│  ○ Channels
│  ○ Skills
│  ○ Health check
│  ○ Continue
```

4.模型选择`MiniMax`
```bash
◆  Model/auth provider
│  ○ OpenAI
│  ○ Anthropic
│  ○ Chutes
│  ● MiniMax
```

5.MiniMax认证方法，选择`MiniMax CN — API Key`
```bash
◆  MiniMax auth method
│  ○ MiniMax Global — OAuth (minimax.io)
│  ○ MiniMax Global — API Key (minimax.io)
│  ○ MiniMax CN — OAuth (minimaxi.com)
│  ● MiniMax CN — API Key (minimaxi.com) (sk-api- or sk-cp- keys supported)
│  ○ Back
```

6.选择`Paste API key now`，直接回车使用默认选项
```bash
◆  How do you want to provide this API key?
│  ● Paste API key now (Stores the key directly in OpenClaw config)
│  ○ Use external secret provider
```
将minmax api key复制到终端中进行回车
![](./assets/img/min1.png)

7.模型选择`minimax/MiniMax-M2.5`
```bash
◆  Models in /model picker (multi-select)
│  ... ...
│  ● minimax/MiniMax-M2.5
```

8.完成模型配置，将glm-4.7修改为minmax-m2.5，完成configure配置，选择`Continue`
```bash
◇  Select sections to configure
│  Continue
```

9.在Clawbot中询问，测试openclaw是否正常回答。
![](./assets/img/min2.png)

### 配置Openrouter

```shell
curl https://openrouter.ai/api/v1/chat/completions /
  -H "Content-Type: application/json" /
  -H "Authorization: Bearer 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" /
  -d '{
  "model": "stepfun/step-3.5-flash:free",
  "messages": [
    {
      "role": "user",
      "content": "hello"
    }
  ],
  "reasoning": {
    "enabled": true
  }
}'
```
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

1.执行以下命令，进入 OpenClaw 模型配置界面
```bash
openclaw config
```

2.选择网关运行位置，选择本地`Local gateway`
```bash
◆  Where will the Gateway run?
│  ● Local (this machine) (No gateway detected (ws://127.0.0.1:18789))
│  ○ Remote (info-only)
```
3.选择配置`Model`模块
```bash
◆  Select sections to configure
│  ○ Workspace
│  ● Model (Pick provider + credentials)
```
4.模型选择`OpenRouter`
```bash
◆  Model/auth provider
│  ○ OpenAI
│  ○ Anthropic
│  ... ...
│  ● OpenRouter (API key)
```

5.选择`Paste API key now`，直接回车使用默认选项
```bash
◆  How do you want to provide this API key?
│  ● Paste API key now (Stores the key directly in OpenClaw config)
│  ○ Use external secret provider
```
复制OpenRouter[API 密钥 | OpenRouter](https://openrouter.ai/settings/keys)到终端中
![](./assets/img/opr1.png)

在终端中按小键盘`—>`右键，跳转到Search：输入`free`，模型为国内阶跃星辰厂商Step 3.5 Flash，目前在[OpenClaw | OpenRouter](https://openrouter.ai/apps?url=https%3A%2F%2Fopenclaw.ai%2F)使用量排名前几免费模型。
![](./assets/img/opr2.png)
![](./assets/img/opr3.png)

8.完成模型配置，将minmax-m2.5修改为step-3.5-flash，完成configure配置，选择`Continue`
```bash
◇  Select sections to configure
│  Continue
```

## Workspace 工作空间
Workspace 是 OpenClaw 智能体的**工作目录**，也是它读取上下文、保存记忆、执行工具操作的唯一位置。
```bash
tree -L 2 ~/.openclaw/
├── openclaw.json # 主配置文件
├── workspace/ # 默认工作空间（多agent时，会创建多个workspace）
│ ├── AGENTS.md # 操作指令和记忆
│ ├── SOUL.md # 人格、边界、语气
│ ├── TOOLS.md # 工具使用笔记
│ ├── IDENTITY.md # 助手名称/头像/表情
│ ├── USER.md # 用户信息
│ ├── MEMORY.md # 长期记忆（仅主会话加载）
│ └── skills/ # 工作空间级技能
├── agents/ # 多智能体会话存储
├── skills/ # 全局技能
└── credentials/ # 凭证存储
```

- Gateway：统一的后台服务进程
- Agent：Gateway 启动后可注册多个 Agent（智能体），每个 Agent 拥有独立的 Workspace（工作区）。
    - 实现：基于 Pi 极简 Agent 框架，核心采用 ReACT（System Prompt + 工具使用）范式。
    - SubAgent：子 Agent，允许在会话中启动子 Agent（子 Agent 仅共享工作区，不共享上下文），任务完成后向主 Agent 发送摘要。
- Workspace：每个 Agent 独立的工作区，核心文件如下（均支持对话式隐式修改，例如用户说"从现在起你叫 Jack"，会自动更新 IDENTITY.md）
    - `AGENTS.md`：相当于 `CLAUDE.md`，附加到 system prompt 中，定义 Agent 核心行为。
    - `IDENTITY.md`：Agent 身份定义（名称、头像等）
    - `SOUL.md`：Agent 核心人设或性格特征
    - `USER.md`：用户偏好设置（称呼、习惯、地点等）
    - `MEMORY.md`：每次会话前必读的核心记忆，例如默认语言等。
    - `memory/`：记忆目录，除 Agent 自主生成外，按时间组织（2026-02-03-2134.md 或 2026-02-05.md），保存 Session 摘要。
    - `HEARTBEAT.md`：定义心跳任务，每 30 分钟执行一次。
    - `TOOLS.md`：工具使用指引，定义工具调用场景和条件。
- Session：会话，每个 Agent 支持多个会话（如不同 IM 账号、群组、主题等），会话间上下文独立，但共享同一 Workspace。
    - 会话上下文默认累积，除非达到上限触发压缩，或手动使用 `/new` 清空。
    - 同一 Session 内，Agent 交互串行处理，新消息进入队列等待。
- Channel：频道（通常为 IM），Agent 与用户交互的主要载体，支持常见聊天应用。
    - 以 Telegram 为例：可配置多个 Bot，每个 Bot 可加入不同群组，每个群组可配置不同主题。
    - 用户向 Bot 发起聊天后，会创建或复用已有 Session。
- Cron and Heartbeat：定时任务和心跳机制。
    - Heartbeat：每 30 分钟（可配置）在指定 Session 中（默认为主 Session）执行 HEARTBEAT.md 中的任务并发送消息。无需发送时返回 HEARTBEAT_OK。
    - Cron：定时任务，指定特定 Agent，可选择新建 Session 或在特定 Session 中执行。
- Tools & Skills：工具和技能体系
    - 内置工具：Bash、文件编辑、Web Reader/Searcher、浏览器等。
    - Skills：OpenClaw 通过 Skills 扩展能力，支持官方 Skill 和外部 Skill 目录。
    - MCP vs Skill：
        - **OpenClaw 默认不支持 MCP**。如确需调用已有 MCP Server，可以用 mcporter 命令行工具调用（封装为单独的 skill）
        - Skill 实现上优先使用 CLI 调用外部工具，而非 API 调用。
            - 当前 LLM 对 CLI 工具调用有较好理解，例如 file-search（文件搜索）、gogs（Gmail 控制）、agent-browser（浏览器控制）。

## OpenClaw配置MCP
mcporter 是一个专门用于管理 MCP 服务器的命令行工具，可以快速配置、测试、管理 MCP 服务器。https://github.com/steipete/mcporter

执行下面命令进行安装：
```bash
npm install -g mcporter

# 验证安装是否正常
mcporter -v
```
或者直接对着接入openclaw助手说“帮我安装mcporter”，它也会帮你安装和配置好mcporter。

### 方法一：自动发现
`mcporter` 默认就会去查找被本地用户下面所安装的Cli。.比如Claude Code、OpenCode 的配置文件，读取配置mcp信息。
- **Linux/macOS:** `~/.claude.json`或 `~/.config/opencode/opencode.json`
- **Windows:** `\Users\{username}\.claude.json`
```bash
$ mcporter list
mcporter 0.7.3 — Listing 7 server(s) (per-server timeout: 30s)
- mcp-server-time (offline — unable to reach server, 0.1s) [source: ~/.config/Code/User/mcp.json]
- web-reader (1 tool, 0.4s) [source: ~/.claude.json]
- web-search-prime (1 tool, 0.5s) [source: ~/.claude.json]
- zreadgit (3 tools, 0.7s) [source: ~/.claude.json]
- MiniMax (offline — unable to reach server, 1.1s) [source: ~/.config/opencode/opencode.json]
- ref-context (2 tools, 1.9s) [source: ~/.claude.json]
- zai-mcp-server (8 tools, 2.3s) [source: ~/.claude.json]
✔ Listed 7 servers (5 healthy; 2 offline).
```
直接对openclaw助手直接告诉他，在遇到网络搜索是直接调用MiniMax Mcp服务器中，MiniMax MCP 支持web_search网络搜索和understand_image图像识别功能。
![](./assets/mcp1.png)
让openclaw助手列出当前所有的mcp服务器
![](./assets/mcp2.png)

### 方法二：手动添加到mcporter本地配置
如果你希望将 OpenCode 中的某个特定服务器永久固定到 `mcporter` 的配置中，而不依赖于 OpenCode 的文件，可以手动注册。
比如opencode中Mcp配置文件为例：
```bash
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "MiniMax": {
      "type": "local",
      "command": ["uvx", "minimax-coding-plan-mcp", "-y"],
      "environment": {
        "MINIMAX_API_KEY": "sk-cp-xxx",
        "MINIMAX_API_HOST": "https://api.minimaxi.com"
      },
      "enabled": true
    }
  }
```
转化为命令行会自动自动生成`~/.mcporter/mcporter.json`文件
```bash
$ mcporter config add MiniMax \
  --command "uvx" \
  --arg "minimax-coding-plan-mcp" \
  --arg "-y" \
  --env MINIMAX_API_KEY="sk-cp-xxx" \
  --env MINIMAX_API_HOST="https://api.minimaxi.com" \
  --scope home

# Added 'MiniMax' to ~/.mcporter/mcporter.json
```
参数说明：
- command / --stdio：指定运行命令（这里用 uvx）
- arg：传递给命令的参数
- env：环境变量（可多个）
- scope home：写入用户级配置（~/.config/mcporter.json）

例如`ref` mcp文档搜索功能
`.claude.json`配置文件中env中配置
```bash
"mcpServers": {
  "ref-context": {
    "type": "http",
    "url": "https://api.ref.tools/mcp",
    "headers": {
      "x-ref-api-key": "ref-xxx"
    }
  }
}
```
转化为`mcporter config`命令
```bash
$ mcporter config add ref-context \
  --url "https://api.ref.tools/mcp" \
  --header "x-ref-api-key: ref-xxx" \
  --description "Ref Context - 文档搜索与URL内容读取" \
  --scope home
```

下面是`~/.mcporter/mcporter.json`文件
```bash
{
  "mcpServers": {
    "MiniMax": {
      "command": "uvx",
      "args": [
        "minimax-coding-plan-mcp",
        "-y"
      ],
      "env": {
        "MINIMAX_API_KEY": "sk-cp-xxx",
        "MINIMAX_API_HOST": "https://api.minimaxi.com"
      }
    },
    "ref-context": {
      "baseUrl": "https://api.ref.tools/mcp",
      "description": "Ref Context - 文档搜索与URL内容读取",
      "headers": {
        "x-ref-api-key": "ref-xxx"
      }
    }
  },
  "imports": []
}
```

`mcporter list`读取的mcporter配置文件，即配置2个的mcp服务器
```bash
$ mcporter list
mcporter 0.7.3 — Listing 2 server(s) (per-server timeout: 30s)
- ref-context — Ref Context - 文档搜索与URL内容读取 (2 tools, 3.1s)
- MiniMax (2 tools, 4.4s)
✔ Listed 2 servers (2 healthy).
```
![](./assets/mcp3.png)

## OpenClaw 配置Skills

skills是由Anthropic提出，核心渐进式及加载，按需加载上下文内容（理解nodejs按需加载），而不是像mcp服务器将一堆指令和文档全部加载，会消耗跟多token；skills使用其中1-2个工具，加载token相对少于前者。
Agent Skills官方文档：[Overview - Agent Skills](https://agentskills.io/home)
介绍skills：[Agent Skills 完全指南](https://mp.weixin.qq.com/s/VQSRPTf5bOyA1bjS2JH5Kw?poc_token=HKIlxmmjtD3uw3nmJ6fJs4tM-ouP3sNZoDodr7JS)
### 查看openclaw skills
```bash
openclaw skills list  # 列出可用技能
openclaw skills info <名称> # 查看技能详情
openclaw skills check # 检查技能依赖状态

# 列出技能
openclaw skills list

# 只显示就绪的技能
openclaw skills list --eligible

# 查看详情
openclaw skills info weather

# 检查依赖
openclaw skills check --verbose
```

### 第一种：openclaw内置skills
openclaw已经内置了多种skills，通过OpenClaw WebUI界面找到代理->选择skills，在skill过滤想要的技能，比如搜索weather天气skills，默认是启用enable这个skills。
![](./assets/sk1.png)
可以直接在机器人助手里，直接问他今天的天气如何。
![](./assets/sk2.png)

### 第二种：使用第三方skills市场
openclaw市场：https://clawhub.ai/
openclaw中文市场：https://cn.clawhub-mirror.com/
腾讯skills市场：https://skillhub.tencent.com/

以官方clawhub市场为例，需要先安装clawhub
```bash
npm i -g clawhub
```
或者直接指向npx，他会直接帮助你安装clawhub
```bash
npx clawhub@latest install <所安装skills名称>
```

先通过gituhb账户注册clawhub账户，通过clawhub login进行cli终端登录
```bash
$ clawhub login 
Opening browser: https://clawhub.ai/cli/auth?redirect_uri=http%3A%2F%2F127.0.0.1%3A41593%2Fcallback&label_b64=Q0xJIHRva2Vu&state=cfdf0ea17d5f166c0c3eaf1f61013287
✔ OK. Logged in as @echo9z.
```

在 ClawHub 上搜索你需要的技能：
```bash
# 比如安装#File Search  文件搜索 kills
$ clawhub search "File Search"
file-search  File Search  (3.746)
filesystem  Filesystem  (1.115)
bailian-web-search  Bailian Web Search  (1.051)
apple-mail-search  Apple Mail Search  (1.028)
local-file-rag-basic  local-file-rag-basic  (1.013)
visual-file-sorter  视觉系文件分类大师 (Visual File Sorter)  (0.995)
npm-search  NPM Search  (0.992)
memory-search  Memory Search  (0.986)
local-image-search  Local Image Search  (0.955)
file-browser  file-browser  (0.915)
```

比如我需安装`File Search` skills技能，用 `fd` 和 `rg` (ripgrep) 快速搜索文件名和内容。会安装当前`workspace/skills`中。
```bash
# 根据上面搜索到名称为file-search 进行安装
 clawhub install file-Search
✔ OK. Installed file-Search -> /home/echo9z/.openclaw/workspace/skills/file-Search

# 安装指定版本 
clawhub install file-Search --version 1.x.x
```
![](./assets/sk3.png)

openclaw webui的代理界面可以管理这个skills技能。
![](./assets/sk4.png)

这里安装是一个简单skills，只有一个简单skills.md文件，复杂一些的包含scripts、references、assets等等，具体可以阅读前面Agent Skills 完全指南文章。
```md
myskill-name/
├── SKILL.md (必需)
│   ├── YAML 前置元数据 (必需)
│   │   ├── name: (必需)
│   │   └── description: (必需)
│   └── Markdown 指令 (必需)
└── 打包资源 (可选)
    ├── scripts/     - 可执行代码
	│   └── exec.py
    ├── references/  - 上下文文档
	│   └── doc.md
    └── assets/      - 输出文件（模板、图片等）
	    └── pic.png
```

>注意：在安装其他skills技能，前请仔细阅读ClawHub 上的SKILL.md文件，以及其他附件脚本或其他文件内容，查看是否有恶意注入代码。

#### ClawHub CLI 常用命令

|命令|说明|示例|
|---|---|---|
|`clawhub list`|列出已安装的技能|查看当前工作空间的所有技能|
|`clawhub update <skill>`|更新指定技能到最新版本|`clawhub update baoyu-image-gen`|
|`clawhub update --all`|更新所有技能|批量更新所有已安装技能|
|`clawhub update --force`|强制更新（忽略版本检查）|解决版本冲突时使用|

### 第三种：通过vercel的skills cli进行安装

Vercel Labs 出品的 skills —— 一个开源的 AI Agent 技能管理工具，支持 OpenCode、Claude Code、Codex、Cursor 等 40+ 主流 Agent。
```bash
npm install -g skills-cli

# 或直接用 npx
npx skills add vercel-labs/agent-skills
```

```bash
# 通过GitHub的仓库名进行安装
npx skills add vercel-labs/agent-skills

# 安装指导github url进行安装
npx skills add https://github.com/vercel-labs/agent-skills

# 指定技能
npx skills add vercel-labs/agent-skills --skill frontend-design --skill skill-creator

# 安装到特定 Agent
npx skills add vercel-labs/agent-skills -a claude-code -a opencode

# 全局安装（所有项目可用）
npx skills add vercel-labs/agent-skills -g
```

比如我们安装前端ui/ux的skills
```bash
npx skills add https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
```
进入skills安装界面，通过空格选择你需要安装skills，这里选择ui/ux的前端设计skills进行回车。
![](./assets/sk5.png)
选择你所需要安装cli工具
![](./assets/sk6.png)

选择所安装级别是项目目录，还是全局用户目录，我们选择全局安装
![](./assets/sk7.png)
安装的方式是每个cli工具，是软链接方式，还是每一个cli都安装
- Symlink (符号链接/软链接)：插件文件只在电脑里存一个源文件。对于其他 AI 软件，工具会创建一个“快捷方式”（软链接）指向这个源文件。
- Copy to all agents (复制到所有代理)：工具会把插件文件完整地 **克隆** 到每一个 AI 软件的配置目录下。
![](./assets/sk8.png)
最后，进行确认安装即可Proceed with installation? Yes。

全局安装的 skills 通过 软链接 方式挂载到 OpenClaw，结构如下：
```bash
~/.openclaw/skills/
├── find-skills    → ~/.agents/skills/find-skills
└── ui-ux-pro-max  → ~/.agents/skills/ui-ux-pro-max
```
![](./assets/sk9.png)

当你安装过多的skills，推荐可以安装一个find-skills。当你对助手说“帮我查一下飞书文档”时，如果安装了这个，AI 会先调用 `find-skills` 来查看你本地是否已经安装了 `feishu` 扩展。

## 配置多智能体（Multi-Agent）

1.输入下面终端命令，进入创建agent向导：
```bash
openclaw agents add <agent_name>
```

2.设置agent所存放的目录，默认回车
```bash
◆  Workspace directory
│  /home/echo9z/.openclaw/workspace-codingbot█
└
```

3.接下来，是否需要需要种main主agent中复制身份配置文件，比如`USER`，`SOUL`，`IDENTITY`。我要配置一个全新的agent，选择`no`
```bash
◆  Copy auth profiles from "main"?
│  ○ Yes / ● No
```

4.问你是否需要配置新的模型，按具体需求你可以选择新的模型。这里选择`no`
```bash
Configure model/auth for this agent now?
│  ○ Yes / ● No
```

5.配置agent channel通道，选择yes
```bash
◆  Configure chat channels now?
│  ● Yes / ○ No
```
以飞书为例，选择`feishu` channel，同时进入飞书开发平台，新建一个机器人应用，具体操作参考前面的接入飞书章节。
或者https://open.feishu.cn/page/openclaw?form=multiAgent打开这连接能快速的创建一个机器人应用。
![](./assets/ag0.png)

将生存appId和appSecret，复制至终端向导中。
![](./assets/ag1.png)
>注意：这个填写id和secret会覆盖原有的飞书配置的id和secret，需要手动的添加飞书.openclaw.json的channel配置。具体看7步骤。

6.**DM (Desktop Manager)** 访问策略，主要是针对你这种使用什么桌面环境（KDE Plasma）用户，决定 AI 代理（Agent）如何与你的桌面系统进行交互和权限绑定。默认选择`no`。
![](./assets/ag2.png)
新增workspace工作空间`.openclaw/workspace-codingbot`
```bash
└── workspace-codingbot
    ├── AGENTS.md
    ├── BOOTSTRAP.md
    ├── HEARTBEAT.md
    ├── IDENTITY.md
    ├── SOUL.md
    ├── TOOLS.md
    └── USER.md
```

openclaw.json文件中的list就会多出`"id": "codingbot"`的agent配置
```bash
"list": [
  {
    "id": "main",
    "model": "minimax/MiniMax-M2.7",
    "skills": [
	  ... ...
      "weather",
      "find-skills",
      "ui-ux-pro-max",
      "file-search",
      "obsidian"
    ]
  },
  {
    "id": "codingbot",
    "name": "codingbot",
    "workspace": "/home/echo9z/.openclaw/workspace-codingbot",
    "agentDir": "/home/echo9z/.openclaw/agents/codingbot/agent",
    "model": "glm-5-turbo"
  }
]
```

7.完成飞书codingbot agent的创建，还需要配置`.openclaw/openclaw.json`文件中的channels.feishu.accounts中添加一个`codingbot:{}`对象
```bash
"channels": {
  "feishu": {
    "enabled": true,
    "appId": "cli_a939xxx", // 默认主main飞书id
    "appSecret": "p7wSxxxx",
    "connectionMode": "websocket",
    "domain": "feishu",
    "groupPolicy": "open",
    "dmPolicy": "open",
    "allowFrom": ["*"],
    "groups": {
      "oc_45efad14b3505839d7af16f018524bd1": {
        "requireMention": true
      }
    },
    "accounts": {
      // 默认机器人
      "default": {
        "appId": "cli_a939xxx",
        "appSecret": "p7wSxxxx"
      },
      // agent1 新增的
      "codingbotAcc": { 
        // 这里配额制对应的飞书机器人应用id和secret
        "appId": "cli_a942xxx",
        "appSecret": "xNnFoPxxx"
      }
    }
  },
}
```

多账号会话隔离（dmScope）

|值|隔离粒度|行为|
|---|---|---|
|main|最粗粒度|所有私聊共用一个会话|
|per-peer|按用户粒度|同一用户跨不同 channel 共享会话|
|per-channel-peer|按 channel + 用户 粒度|不同 channel 的同一用户各自独立，但同 channel 下多账号仍共享|
|per-account-channel-peer|按账号 + channel + 用户 粒度|多账号推荐，每个机器人与每个用户的会话完全独立。|

按账号 + channel + 用户 粒度设置隔离的快捷指令：
```json
openclaw config set session.dmScope "per-account-channel-peer"
```

>【注意】：仅设置 session.dmScope 不够——还需通过 agents + bindings 将不同账号绑定到不同 agent，才能实现记忆完全隔离

将agentId绑定到飞书的channel通道中：
```bash
  // 会话绑定
  "bindings": [
    // 飞书机器人绑定 AgentId对应上面的子AgentId
    // channel选择飞书
    // accountId对应下面的飞书机器人的accountId
    {
      "type": "route",
      "agentId": "codingbot",
      "match": {
        "channel": "feishu",
        "accountId": "codingbotAcc" // 对应的accounts.codingbotAcc
      }
    }
  ],
```
具体参考：https://bytedance.larkoffice.com/docx/WNNXdhKxmo8KDJxMM9dc0GD5nFf

8.飞书应用就可以看到新增独立的agent。
![](./assets/ag3.png)


## opencalw中subagent的使用
比如当前使用到批量任务、并发任务，可以用到opencalw中subagent子代理机制，一个主agent下面创建多个子subagent。子subagent不要自己配置，主agent会根据任务类型、任务难度，自动帮你派生子agent来完成任务。
![](./assets/sub1.png)

/subagents命令
使用 `/subagents` 来检查或控制**当前会话**的子代理运行：
- `/subagents list`
- `/subagents stop <id|#|all>`
- `/subagents log <id|#> [limit] [tools]`
- `/subagents info <id|#>`
- `/subagents send <id|#> <message>`
`/subagents info` 显示运行元数据（状态、时间戳、会话 ID、记录路径、清理）。

主要目标：
- 并行化"研究/长任务/慢工具"工作，不阻塞主运行。
- 默认保持子代理隔离（会话分离 + 可选沙箱）。
- 保持工具表面难以误用：子代理默认**不**获取会话工具。
- 避免嵌套：子代理不能生成子代理。

`/subagent list`：查看最近子agent活动状态
![](./assets/sub2.png)

## 创建定时任务

| 命令                             | 说明       |
| ------------------------------ | -------- |
| `openclaw cron status`         | 查看调度器状态  |
| `openclaw cron list`           | 列出所有定时任务 |
| `openclaw cron add`            | 添加定时任务   |
| `openclaw cron edit <ID>`      | 编辑任务     |
| `openclaw cron rm <ID>`        | 删除任务     |
| `openclaw cron enable <ID>`    | 启用任务     |
| `openclaw cron disable <ID>`   | 禁用任务     |
| `openclaw cron run <ID>`       | 立即执行任务   |
| `openclaw cron runs --id <ID>` | 查看执行历史   |

```bash
# 每天早上9点提醒
openclaw cron add --name "早间提醒" --cron "0 9 * * *" --system-event "早安！新的一天开始了"

# 工作日下班提醒
openclaw cron add --name "下班提醒" --cron "0 18 * * 1-5" --system-event "该下班了，记得写日报"

# 查看任务
openclaw cron list
```

命令创建了一个一次性任务，在 2026-02-01 16:00 UTC 向主会话推送一条提醒，然后自动清理掉
```bash
openclaw cron add \
  --name "Reminder" \   # 定时任务名称
  --at "2026-02-01T16:00:00Z" \ # 设置运行时间（ISO 格式，Z = UTC）
  --session main \ # 在哪个 session 执行（main = 主会话）
  --system-event "Reminder: check the cron docs draft" \ # 触发一个系统事件，推送给主会话
  --wake now \ # 唤醒模式：now = 立即唤醒处理，next-heartbeat = 等下次心跳
  --delete-after-run  # 一次性任务，运行成功后自动删除
```

每天定时执行任务：每天18点生成一个整理一份今日必看（包含知乎、百度、今日头条、b站、微博、抖音）
```bash
openclaw cron add \
  --name "每日必看" \
  --cron "0 0 18 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "每天18点生成今日必看新闻摘要，来源：知乎、百度、今日头条、B站、微博、抖音" \
  --channel feishu \
  --to "oc_45efad14b3505839d7axxxx
```
或者直接自然语言，通过openclaw助手进行创建。
![](./assets/cro1.png)

## 使用ACP操作Claudecode
ACP（Agent Client Protocol）是 OpenClaw 用于连接外部编程工具（如 Claude Code、Codex、OpenCode、Gemini CLI）的协议。通过 ACP，你可以在聊天中直接调用这些强大的代码工具来完成编程任务。[openclaw/acpx](https://github.com/openclaw/acpx)
![](./assets/acp1.png)

### 安装acp
```bash
openclaw plugins install acpx
```
### 添加acp配置
在openclaw.json中添加以下配置
核心配置 `acp.*`
```json
  "acp": {
    "enabled": true,
    "dispatch": { "enabled": true }, // ACP 消息分发动态开关（不影响 /acp 控制命令）_
    "backend": "acpx",
    "defaultAgent": "claude", // 默认 harness，不指定 agentId 时用_
    "allowedAgents": ["claude", "codex", "opencode", "pi", "gemini"], // 允许调用的 agent 白名单_
    "maxConcurrentSessions": 8, // 最大并发 ACP 会话数
    "stream": { // 控制 ACP 输出流如何切块传递给 OpenClaw
      "coalesceIdleMs": 300,      // 空闲 300ms 合并输出批次
      "maxChunkChars": 1200       // 单个流块最大字符数
    },
    "runtime": {
      "ttlMinutes": 120            // ACP 会话最大存活时间（分钟） 0=永不过期
    }
  },
```

acpx插件配置，`plugins.entries.acpx.config`
```json
  "plugins": {
    "entries": {
      "acpx": {
        "enabled": true,
        "config": {
          "command": "acpx",                    // acpx 二进制路径或命令名
          "expectedVersion": "any",             // 版本校验策略
          "permissionMode": "approve-all",      // 权限模式
          "nonInteractivePermissions": "deny"   // 无 TTY 时的行为
        }
      }
    }
  }
```

会话绑定配置 `session.threadBindings`
```json
  "session": {
    "threadBindings": {
      "enabled": true,      // 全局开关
      "idleHours": 24,       // 空闲 N 小时自动解绑（0=禁用）
      "maxAgeHours": 0       // 硬性最大存活（0=永不过期）
    }
  }
```

agents 配置
```json
  "agents": {
    "list": [
      {
        "id": "codingbot",
        "name": "codingbot",
        "workspace": "/home/echo9z/.openclaw/workspace-codingbot",
        "agentDir": "/home/echo9z/.openclaw/agents/codingbot/agent",
        "model": "zai/glm-5.1",
        "runtime": {
          "type": "acp",
          "acp": {
            "agent": "claude", // 指定用哪个 harness（如 claude / codex）
            "backend": "acpx",
            "mode": "persistent", // persistent 持久会话，oneshot 单次执行
            "cwd": "/home/echo9z/.openclaw/workspace-codingbot" // ACP 会话的工作目录
          }
        }
      },
    ]
  }
```

持久化绑定 bindings
把某个频道/话题直接绑死到某个 ACP session，开机即用：
```json
 "bindings": [
    {
      "type": "acp",
      "agentId": "codingbot",
      "match": {
        "channel": "feishu",
        "accountId": "coding-bot",
        "peer": { "kind": "dm", "id": "ou_82e1de991d071xxxxxx" }
      },
      "acp": { 
        "cwd": "/home/echo9z/.openclaw/workspace-codingbot",
        "label": "codingbot"
      }
    }
 ]
```
这里的bindings.peer.id直接完openclaw助手
![](./assets/acp2.png)

首次使用
在飞书使用，可以直接使用这个命令/acp 去完成一个 ACP 的场景
创建会话
```bash
/acp spawn claude --bind here  # 启动并绑定当前对话（推荐）

/acp spawn claude --bind off  # 启动但不绑定，消息不发到这个 session

/acp spawn claude --mode persistent --thread auto
/acp spawn claude --cwd /path/to/project
```

在飞书聊天框，通过`/acp spawn claude --bind here`创建acp:claude会话。接下来在对话框输入的内容，和在终端中运行claude code对话一样。比如让他写一个脚本。
- --bind here把当前飞书对话绑定到这个 session
![](./assets/acp3.png)

openclaw会在后台运行一个claude code进程
![](./assets/acp4.png)

创建会话`/acp spawn`
```Plain
/acp steer [--session <session-key|session-id|session-label>] <instruction>

/acp steer --session reacPjt 帮我写一个py的hello脚本
```

**常用选项：**
- --session：可选。指定目标会话，支持三种标识：session key、session ID（UUID格式）、session label
- `<instruction>`：要发送的引导指令文本
- `--mode persistent|oneshot` - 持久模式（默认）或一次性模式
- `--thread auto|here|off` - 线程绑定方式
- `--cwd <路径>` - 设置工作目录
- `--label <名称>` - 给会话起个短名字

创建claude持久会话，同时指定某个路径为工作目录
```bash
/acp spawn claude --mode persistent --cwd /home/xxx/projects --label reacPjt
```
- mode persistent：persistent 持久会话
- cwd /home/xxx/projects：会话的工作目

查看状态
```bash
/acp status
/acp sessions
```

查看历史
```bash
/acp sessions history --limit 10
```

关闭会话
```bash
/acp close/acp close --session <会话>

/acp close # 关闭当前绑定会话
```

其他acp详细命令参考：[ACP详细命令指南](https://leochens.feishu.cn/wiki/ESYOwkF4hi0BqDkmg95cGSCMnjc)

## OpenClaw 常用命令速查

openclaw所有相关配置：`.openclaw/openclaw.json`
具体详解：[openclaw配置详解](https://leochens.feishu.cn/wiki/OxgqwBcoeiVK1lkGDo7c2iCUnJc)
我们使用vscode打开openclaw.json配置文件，默认的配置很多，我们把一级 Key 收缩一下，整体上就是这几个分类：
![](./assets/cfg1.png)
- **meta**：元数据模块，记录配置文件的基础信息
- **wizard**：向导配置模块，记录交互式配置向导的运行记录
- **auth**：认证配置模块，管理模型提供商的认证信息
- **models**：模型提供商与模型元信息模块
- **agents**：智能体默认行为配置模块
- **commands**：命令执行配置模块，控制 OpenClaw 的命令运行规则
- **tools**：配置工具模块（搜索、浏览器等）
- **session**：会话管理配置模块，定义会话的作用域等规则
- **hooks**：钩子配置模块，管理内置的事件钩子 / 插件
- **gateway**：网关配置模块，控制 OpenClaw 的网络服务（端口、认证、访问控制等）
- **plugins**：配置插件模块

下面是models模型模块主要配置信息，比如模型请求地址，模型类型，default模型是什么模型。
```json
  "models": {
    "mode": "merge",
    "providers": {
      "minimax": {
        "baseUrl": "https://api.minimaxi.com/anthropic",
        "api": "anthropic-messages",
        "authHeader": true,
        "models": [
          {
            "id": "MiniMax-M2.7",
            "name": "MiniMax M2.7",
            "reasoning": true,
            "input": [
              "text"
            ],
            "cost": {
              "input": 0.3,
              "output": 1.2,
              "cacheRead": 0.03,
              "cacheWrite": 0.12
            },
            "contextWindow": 200000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "minimax/MiniMax-M2.7"
      },
      "models": {
        "minimax/MiniMax-M2.7": {
          "alias": "Minimax"
        }
      },
      "workspace": "/home/echo9z/.openclaw/workspace"
    }
  },
```
- `models`：模型提供商与模型元信息
	OpenClaw 识别自定义模型提供商的核心配置，告诉系统去哪里调用模型、模型的参数/计费规则是什么。
- `agents`：智能体默认行为配置
	这部分定义 OpenClaw 智能体的默认使用规则，比如默认用哪个模型、哪些模型可以用（白名单）、模型的快捷别名。

具体看openclaw.json配置文件
## OpenClaw常用命令
安装完成后，以下是日常使用中最常用的 [OpenClaw 命令](https://linux.do/t/topic/1748901)：

| 命令                                               | 功能               |
| ------------------------------------------------ | ---------------- |
| `openclaw status`                                | 查看 OpenClaw 运行状态 |
| `openclaw onboard`                               | 重新进入配置向导         |
| `openclaw gateway start\|stop\|restart`          | 启动、停止、重启服务       |
| `openclaw gateway stop`                          | 停止服务             |
| `openclaw gateway restart`                       | 重启服务             |
| `openclaw update`                                | 更新到最新版本          |
| `openclaw health`                                | 健康检查             |
| `openclaw doctor`                                | 诊断问题             |
| `openclaw dashboard`                             | 获取 Web UI 访问链接   |
| `openclaw security audit --deep`                 | 安全审计             |
| `openclaw uninstall`                             | 卸载 OpenClaw      |
| `openclaw plugins list`                          | 查看插件列表           |
| `openclaw plugins enable\|disable <plgins name>` | 启用、禁用插件          |
| `openclaw agents list`                           | 查看所有agents       |
| `openclaw agents bindings`                       | 查看agents绑定关系     |
| `openclaw hooks list`                            | 查看所有 Hooks       |
| `openclaw hooks enable\|disable <name>`          | 启用、禁用指定 Hook     |

### 初始化与配置
```bash
openclaw onboard --reset              # 重置后运行向导
openclaw onboard --flow quickstart    # 快速开始模式
openclaw onboard --flow advanced      # 高级模式
openclaw onboard --gateway-port 18789 # 指定网关端口
openclaw onboard --install-daemon     # 安装为系统服务
```

### Gateway 网关服务
```bash
openclaw gateway  # 启动 WebSocket 网关（前台运行）
openclaw gateway status # 查看网关状态（默认探测 RPC）
openclaw gateway start  # 启动网关服务（后台）
openclaw gateway stop 停止网关服务
openclaw gateway restart  重启网关服务
openclaw gateway install  安装为系统服务
openclaw gateway uninstall  卸载系统服务
```
### RPC 调用
```bash
openclaw gateway call <方法> [--params '<JSON>']   # 调用 RPC 方法
openclaw gateway health                           # 获取健康状态
openclaw gateway probe                            # 探测网关
openclaw gateway discover                         # 发现网关
```
### 插件管理（plugins）

```bash
# 列出插件
openclaw plugins list

# 查看插件详情
openclaw plugins info <plugins_name>

# 安装插件
openclaw plugins install <plugins_name>

# 启用插件（需重启Gateway）
openclaw plugins enable <plugins_name>

# 禁用插件
openclaw plugins disable <plugins_name>

# 插件诊断
openclaw plugins doctor
```

### 修改配置
```bash
# 添加或更新配置项
openclaw config set agents[0].model "anthropic/claude-opus-4-20250514"
 
# 查看当前配置
openclaw config get agents
 
# 验证配置文件
openclaw doctor
```
### 配置管理
```bash
openclaw config 启动配置向导（无子命令时）
openclaw config get <路径>  获取配置值
openclaw config set <路径> <值>  设置配置值
openclaw config unset <路径>  删除配置值
openclaw config file  显示配置文件路径
openclaw config validate  验证配置
```

```bash
# 获取配置
openclaw config get agents.defaults.model.primary
openclaw config get gateway.port
openclaw config get channels.telegram.accounts.default.token

# 添加或更新配置项
openclaw config set agents[0].model "anthropic/claude-opus-4-20250514"
openclaw config set gateway.port 18789

# 删除配置
openclaw config unset agents.defaults.thinking

agents.defaults.model.primary       # 主模型
agents.defaults.thinking            # 思考模式
agents.defaults.timeoutSeconds      # 超时时间
gateway.port                        # 网关端口
gateway.auth.token                  # 网关令牌
channels.feishu.enabled             # feishu 启用状态
```
 
### 查看日志

```bash
openclaw logs                  # 查看网关日志 
openclaw logs --follow         # 实时跟踪日志 
openclaw logs --limit 500      # 限制行数 
openclaw logs --json           # JSON 格式输出 
openclaw logs --plain          # 纯文本输`
```

| 类型       | 路径                                         | 说明                       |
| -------- | ------------------------------------------ | ------------------------ |
| **会话日志** | `~/.openclaw/agents/main/sessions/*.jsonl` | 对话历史，JSONL 格式            |
| **运行日志** | `/tmp/openclaw/openclaw-YYYY-MM-DD.log`    | Gateway + OpenClaw 主进程日志 |
| **审计日志** | `~/.openclaw/logs/config-audit.jsonl`      | 配置变更记录                   |
其中 **运行日志** 是大头，包含了飞书插件加载、Gateway 启停、消息收发等所有运行时信息。

### 安全与诊断

安全审计
```bash
openclaw security audit              # 基本审计
openclaw security audit --deep       # 深度审计（包括网关探测）
openclaw security audit --fix        # 自动修复安全问题
```

健康检查
```bash
openclaw health                      # 获取网关健康状态
openclaw health --json               # JSON 输出
openclaw doctor                      # 快速诊断
openclaw doctor --deep               # 深度诊断
openclaw doctor --yes                # 自动接受默认修复
```

密钥管理
```bash
openclaw secrets reload              # 重新加载密钥引用
openclaw secrets audit               # 审计密钥安全
```

### 更新与维护
更新
```bash
openclaw update                      # 更新 OpenClaw
openclaw update --check              # 检查更新
```
备份
```bash
openclaw backup create               # 创建备份
openclaw backup verify <文件>        # 验证备份
```
重置
```bash
openclaw reset                       # 重置配置（保留 CLI）
openclaw reset --scope config        # 只重置配置
openclaw reset --scope config+creds+sessions  # 重置配置+凭证+会话
openclaw reset --scope full          # 完全重置
```
卸载
```bash
openclaw uninstall                   # 卸载网关服务和数据
openclaw uninstall --all             # 完全卸载
```

学习教程：
[【OpenClaw】全网最详细安装与使用基础教程](https://www.bilibili.com/video/BV1DtNczDEyR?spm_id_from=333.788.videopod.sections&vd_source=c1080d219d1f81326f0064a8647d7355)
[【OpenClaw】 多Agent架构详解，为什么你必须用“AI团队”？](https://www.bilibili.com/video/BV1f6AAzYEMU/?spm_id_from=333.1387.homepage.video_card.click&vd_source=c1080d219d1f81326f0064a8647d7355)
[【OpenClaw】手把手叫你打造属于你的AI团队!](https://www.bilibili.com/video/BV1yXXQBpEaE/?spm_id_from=333.1387.homepage.video_card.click&vd_source=c1080d219d1f81326f0064a8647d7355)

参考：https://mp.weixin.qq.com/s/0Xq9XOfTjnQYwqXOVe9rZg
https://linux.do/t/topic/1748901
https://cloud.tencent.com/developer/article/2624973
[Windows快速部署OpenClaw + Qwen3.5 Plus完整教程 – 大绵羊博客](https://dmyblog.cn/3009.html)
[OpenClaw 全流程部署指南 - 晚安的静谧小屋](https://blog.goodnightan.com/posts/openclaw-install/)
[OpenClaw 最新保姆级飞书对接指南教程 搭建属于你的 AI 助手-阿里云开发者社区](https://developer.aliyun.com/article/1711222)