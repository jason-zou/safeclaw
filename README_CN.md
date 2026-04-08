<p align="center">
  <img src="https://jason-zou.github.io/safeclaw/icon.png" alt="SafeClaw" width="80" />
</p>

<h1 align="center">SafeClaw</h1>

<p align="center">
  <strong>安全优先的 AI 助理网关</strong><br/>
  Security-First AI Assistant Gateway
</p>

<p align="center">
  <a href="https://jason-zou.github.io/safeclaw/">官方网站</a> ·
  <a href="https://github.com/jason-zou/safeclaw/releases">下载</a> ·
  <a href="./README.md">English</a>
</p>

<p align="center">
  <a href="https://github.com/jason-zou/safeclaw/releases"><img src="https://img.shields.io/github/v/release/jason-zou/safeclaw?style=flat-square" alt="Release" /></a>
  <img src="https://img.shields.io/badge/平台-macOS%20(ARM64)-blue?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/语言-Rust-orange?style=flat-square" alt="Rust" />
  <img src="https://img.shields.io/badge/二进制大小-~15MB-green?style=flat-square" alt="Size" />
</p>

---

SafeClaw 是一款安全优先的 AI 助理网关，使用 Rust 构建。单一 ~15MB 二进制文件，零外部依赖，提供多层运行时安全架构、多模型支持、丰富工具生态和内置 Web 管理控制台。

## 核心特性

### 运行时安全架构

SafeClaw 在运行时层面强制执行安全策略，而不仅仅依赖 prompt。

- **入站/出站安全管线** — 每条消息在 AI 处理前后都经过 prompt 注入检测、PII 扫描和凭证泄露检测
- **工具审查管线** — 每次工具调用都经过风险分类，高风险操作需要人工审批
- **数据流污点追踪** — 标记数据来源，自动阻断密钥泄露、PII 外泄和命令注入
- **SSRF 防护** — 三层防御：协议白名单 → 主机黑名单 → DNS 解析后 IP 检查
- **权限能力模型** — 子 Agent 继承收窄的权限集，从结构上防止权限提升
- **沙箱执行** — Shell 命令在 Docker 或 Firejail 沙箱中运行，限制资源、隔离网络、降低权限
- **紧急停止 (E-Stop)** — 随时可触发的紧急停止，支持多级别
- **密钥加密存储** — API Key 使用 ChaCha20 加密存储
- **全链路审计** — 每次交互、工具调用和安全决策全程可追溯

### 多模型支持

接入主流大模型服务商，支持自动降级切换。

| 服务商 | 协议 | 说明 |
|--------|------|------|
| OpenAI | OpenAI API | GPT-4o、o1、o3 等 |
| Anthropic | Anthropic API | Claude 4、Sonnet、Haiku |
| Google Gemini | Gemini API | Gemini 2.5 Pro、Flash |
| DeepSeek | OpenAI 兼容 | DeepSeek V3、R1 |
| Kimi / Moonshot | OpenAI 兼容 | Kimi K2、Moonshot |
| Kimi Coding | Anthropic 兼容 | 编程优化模型 |
| Ollama | 本地 | 运行任意本地模型 |
| Claude Code | 本地 CLI | CLI 流式输出 |
| Qwen Code | 本地 CLI | CLI 流式输出 |
| 自定义 | 可配置 | 任何 OpenAI / Anthropic / Gemini 兼容端点 |

- **Provider 降级链** — API 失败时自动切换到备选模型
- **按助手配置模型** — 每个助手可以使用不同的模型
- **流式输出** — 实时 token 流式传输，支持思维链展示

### 多通道接入

一个网关，多个入口。

- **CLI** — 交互式终端，全屏 TUI 界面
- **Web 控制台** — 内置聊天、管理和监控
- **HTTP / SSE / WebSocket** — 标准 API 接入
- **飞书** — 机器人，支持审批卡片
- **Telegram** — 机器人，支持媒体处理
- **Discord** — 机器人，支持服务器/用户访问控制
- **WhatsApp** — 通过 Cloud API 接入
- **Slack** — Socket Mode，App/Bot Token 方式
- **Signal** — 通过 Signal CLI REST API
- **LINE** — Webhook，签名验证
- **企业微信** — Webhook，消息加解密
- **微信** — 桥接方式接入
- **Webhooks** — 通用 HTTP 集成

### MCP (Model Context Protocol) 集成

一等公民级别的 MCP 生态支持。

- **自动发现** — 连接 MCP 服务器，自动发现并注册工具
- **Stdio 和 HTTP/SSE 传输** — 子进程或远程服务器两种方式
- **健康监测** — 后台健康检查，指数退避自动重连
- **模板市场** — 内置 Filesystem、GitHub、PostgreSQL、SQLite、Brave Search、Slack、Puppeteer、Google Drive 等常用模板
- **统一配置编辑器** — 可视化表单 + JSON 两种模式
- **全端管理** — CLI / TUI / Web 三端均可管理 MCP 服务器

### 丰富工具生态

28+ 内置工具，全部经过安全管线管控。

| 类别 | 工具 |
|------|------|
| **执行** | Shell、Docker 沙箱、文件读写/编辑/搜索 |
| **开发** | Git 操作、代码搜索、Glob 搜索 |
| **网络** | 网页搜索、网页抓取、HTTP 请求、浏览器自动化 |
| **记忆** | 保存、搜索、获取、遗忘 — 支持语义向量搜索 |
| **知识** | 知识库搜索、知识图谱（实体、关系、查询） |
| **调度** | Cron 定时任务、提醒、周期任务 |
| **通信** | 发送消息、会话管理、助手间通信 |
| **委派** | 子 Agent 派生、助手间协作 |
| **媒体** | 图像生成、图像理解、视觉记忆 |
| **Canvas** | 创建和展示交互式 HTML 画布 |
| **MCP** | 所有已连接 MCP 服务器的工具 |
| **插件** | SafeClaw 插件注册的自定义工具 |

### 记忆与知识

持久化、可搜索的记忆，随着对话不断增长。

- **三层记忆架构** — 工作记忆、长期记忆和整合摘要
- **语义搜索** — 本地向量嵌入，关键词 + 向量混合检索
- **自动召回** — 相关记忆自动注入上下文
- **知识图谱** — SQLite 支撑的实体-关系图，AI 可自主构建和查询
- **文档导入** — 支持 PDF、电子表格、EPUB 等格式导入知识库
- **视觉记忆** — 图像、截图和媒体的感知缓冲区
- **记忆整合** — 后台记忆整合处理，丰富长期记忆

### 多助手系统

运行多个 AI 助手，各自拥有独立身份和工作区。

- **独立人格** — 每个助手有自己的性格、系统提示词和行为规则
- **隔离记忆** — 每个助手独立的 `memory.db`
- **助手间通信** — 助手之间可以发送消息和协作
- **子 Agent 派生** — 委派聚焦的子任务，权限自动收窄
- **按助手配置模型** — 每个助手可使用不同的大模型

### 主动智能

SafeClaw 不只是被动回答，还能主动执行任务。

- **Cron 调度器** — 支持完整工具访问的周期任务
- **心跳巡检** — 定期健康检查和主动通知
- **定时提醒** — 一次性延迟任务

### 技能与插件

通过社区技能和自定义插件扩展 SafeClaw。

- **SKILL.md 标准** — 兼容 OpenClaw 技能格式
- **28+ 内置技能** — 开箱即用
- **ClawHub** — 社区技能仓库，支持搜索和安装
- **插件系统** — Node.js 插件，JSON-RPC 协议，可注册工具、钩子、Prompt 片段和技能

### Web 控制台

内置 Web 管理界面，地址 `localhost:18790`。

- **仪表盘** — 系统状态、审计摘要、健康告警
- **智能对话** — 流式聊天，附件支持，审批流程
- **助手管理** — 配置人格、模型、工具和渠道
- **自动化** — Cron 任务管理
- **记忆管理** — 浏览、搜索和管理记忆条目
- **知识库 & Wiki** — 知识库和 Wiki 管理
- **MCP** — 服务器管理和模板市场
- **技能 & 插件** — 安装和管理扩展
- **安全中心** — 审计事件、E-Stop、用户管理
- **渠道管理** — 配置消息平台集成
- **设置** — Provider 配置、嵌入配置、系统设置
- **Setup Wizard** — 引导式首次配置

### 部署方式

| 模式 | 命令 | 说明 |
|------|------|------|
| **桌面应用** | 双击 `.dmg` | macOS 系统托盘应用 |
| **守护进程** | `safeclaw daemon` | 网关 + 渠道 + 调度器 |
| **网关** | `safeclaw gateway` | HTTP/WebSocket API 服务 |
| **CLI 交互** | `safeclaw agent` | 本地交互式终端 |
| **系统服务** | `safeclaw service install` | launchd / systemd 自启动 |

## 安装

### macOS (Apple Silicon)

1. 从 [Releases](https://github.com/jason-zou/safeclaw/releases) 下载最新的 `.dmg` 文件
2. 打开 DMG，将 SafeClaw.app 拖到 Applications
3. 从 Launchpad 启动 SafeClaw

应用已通过 Apple 代码签名和公证，可直接运行。

### 首次运行

首次启动时，SafeClaw 会打开 Setup Wizard 引导配置：

1. **大模型服务商** — 选择模型并输入 API Key
2. **消息渠道**（可选）— 连接 Telegram、Discord、Slack 等
3. **安全设置** — 检查默认安全配置

配置完成后，访问 `http://localhost:18790` 打开 Web 控制台。

## 技术规格

| 规格 | 数值 |
|------|------|
| 语言 | Rust |
| 二进制大小 | ~15 MB |
| 启动时间 | 毫秒级 |
| GC 停顿 | 无（零垃圾回收） |
| 记忆后端 | SQLite |
| 向量存储 | int8 量化，节省 75% 空间 |
| 密钥加密 | ChaCha20-Poly1305 |
| 支持平台 | macOS (ARM64)，Windows 和 Linux 即将推出 |

## 许可证

SafeClaw 为商业软件。前往 [Releases](https://github.com/jason-zou/safeclaw/releases) 下载。

---

<p align="center">
  <a href="https://jason-zou.github.io/safeclaw/">官方网站</a> ·
  <a href="https://github.com/jason-zou/safeclaw/releases">下载</a> ·
  <a href="./README.md">English</a>
</p>
