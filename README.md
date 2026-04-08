<h1 align="center">SafeClaw</h1>

<p align="center">
  <strong>Security-First AI Assistant Gateway</strong><br/>
  安全优先的 AI 助理网关
</p>

<p align="center">
  <a href="https://jason-zou.github.io/safeclaw/">Website</a> ·
  <a href="https://github.com/jason-zou/safeclaw/releases">Download</a> ·
  <a href="./README_CN.md">中文文档</a>
</p>

<p align="center">
  <a href="https://github.com/jason-zou/safeclaw/releases"><img src="https://img.shields.io/github/v/release/jason-zou/safeclaw?style=flat-square" alt="Release" /></a>
  <img src="https://img.shields.io/badge/platform-macOS%20(ARM64)-blue?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/language-Rust-orange?style=flat-square" alt="Rust" />
  <img src="https://img.shields.io/badge/binary%20size-~15MB-green?style=flat-square" alt="Size" />
</p>

---

SafeClaw is a security-first AI assistant gateway built in Rust. It ships as a single ~15MB binary with zero external dependencies, providing multi-layer runtime security, multi-model support, rich tool ecosystem, and a built-in web dashboard.

## Key Features

### Runtime Security Architecture

SafeClaw enforces security at the runtime level — not just through prompts.

- **Inbound/Outbound Security Pipeline** — every message passes through prompt injection detection, PII scanning, and credential leak detection before and after the AI processes it
- **Tool Review Pipeline** — each tool call is risk-classified and optionally requires human approval before execution
- **Taint Tracking** — data flow labels track where information comes from, automatically blocking exfiltration of secrets, PII, or command injection
- **SSRF Protection** — three-layer defense: scheme validation → hostname blocklist → post-DNS IP check
- **Capability Model** — sub-agents inherit narrowed permissions; privilege escalation is structurally prevented
- **Sandbox Execution** — shell commands run inside Docker or Firejail with resource limits, network isolation, and dropped capabilities
- **E-Stop** — emergency stop at any time, with graduated severity levels
- **Encrypted Secret Storage** — API keys are encrypted at rest with ChaCha20
- **Audit Logging** — full-chain tracing of every interaction, tool call, and security decision

### Multi-Model Support

Connect to any major LLM provider with automatic failover.

| Provider | Protocol | Notes |
|----------|----------|-------|
| OpenAI | OpenAI API | GPT-4o, o1, o3, etc. |
| Anthropic | Anthropic API | Claude 4, Sonnet, Haiku |
| Google Gemini | Gemini API | Gemini 2.5 Pro, Flash |
| DeepSeek | OpenAI-compatible | DeepSeek V3, R1 |
| Kimi / Moonshot | OpenAI-compatible | Kimi K2, Moonshot |
| Kimi Coding | Anthropic-compatible | Programming-optimized |
| Ollama | Local | Run any local model |
| Claude Code | Local CLI | Streaming via CLI |
| Qwen Code | Local CLI | Streaming via CLI |
| Custom | Configurable | Any OpenAI / Anthropic / Gemini-compatible endpoint |

- **Provider Fallback Chain** — automatic failover to backup models on API errors
- **Per-Assistant Model Override** — each assistant can use a different provider
- **Streaming** — real-time token streaming with thinking/reasoning traces

### Multi-Channel Messaging

One gateway, many entry points.

- **CLI** — interactive terminal with full-screen TUI (ratatui)
- **Web Dashboard** — built-in chat, management, and monitoring
- **HTTP / SSE / WebSocket** — standard API access
- **Lark (Feishu)** — bot with approval card support
- **Telegram** — bot with media handling
- **Discord** — bot with guild/user access control
- **WhatsApp** — via Cloud API
- **Slack** — Socket Mode with App/Bot tokens
- **Signal** — via Signal CLI REST API
- **LINE** — webhook with signature verification
- **WeCom (WeChat Work)** — webhook with message encryption
- **WeChat** — bot via bridge
- **Webhooks** — generic HTTP integration

### MCP (Model Context Protocol) Integration

First-class support for the MCP ecosystem.

- **Auto-Discovery** — connect to MCP servers, discover and register tools automatically
- **Stdio & HTTP/SSE Transports** — spawn subprocess or connect to remote servers
- **Health Monitoring** — background health checks with auto-reconnect and exponential backoff
- **Template Marketplace** — built-in templates for Filesystem, GitHub, PostgreSQL, SQLite, Brave Search, Slack, Puppeteer, Google Drive, and more
- **Unified Config Editor** — visual form + JSON mode for server configuration
- **CLI / TUI / Web** — manage MCP servers from any interface

### Rich Tool Ecosystem

28+ built-in tools the AI can use, all governed by the security pipeline.

| Category | Tools |
|----------|-------|
| **Execution** | Shell, Docker sandbox, file read/write/edit/search |
| **Development** | Git operations, code search, glob search |
| **Web** | Web search, web fetch, HTTP requests, browser automation |
| **Memory** | Save, search, get, forget — with semantic vector search |
| **Knowledge** | Knowledge base search, knowledge graph (entities, relations, queries) |
| **Scheduling** | Cron jobs, reminders, recurring tasks |
| **Communication** | Send messages, session management, inter-assistant messaging |
| **Delegation** | Sub-agent spawning, assistant-to-assistant collaboration |
| **Media** | Image generation, image understanding, visual memory |
| **Canvas** | Create and present interactive HTML canvases |
| **MCP** | All tools from connected MCP servers |
| **Plugins** | Custom tools from SafeClaw plugins |

### Memory & Knowledge

Persistent, searchable memory that grows with every conversation.

- **Three-Layer Memory** — working memory, long-term memory, and consolidated summaries
- **Semantic Search** — local vector embeddings with hybrid keyword + vector retrieval
- **Auto-Recall** — relevant memories are automatically injected into context
- **Knowledge Graph** — entity-relation graph backed by SQLite, with tools for the AI to build and query
- **Document Ingestion** — ingest PDFs, spreadsheets, EPUB, and more into searchable knowledge bases
- **Visual Memory** — perception buffer for images, screenshots, and media
- **Dreaming** — background memory consolidation for richer long-term recall

### Multi-Assistant System

Run multiple AI assistants with isolated identities and workspaces.

- **Independent Persona** — each assistant has its own personality, system prompt, and behavior rules
- **Isolated Memory** — separate `memory.db` per assistant
- **Agent-to-Agent** — assistants can message each other and collaborate
- **Sub-Agent Spawning** — delegate focused sub-tasks with narrowed permissions
- **Per-Assistant Model** — each assistant can use a different LLM provider

### Proactive Intelligence

SafeClaw doesn't just respond — it acts on its own when configured.

- **Cron Scheduler** — recurring jobs with full tool access
- **Heartbeat** — periodic health checks and proactive notifications
- **Reminders** — one-shot delayed tasks

### Skills & Plugins

Extend SafeClaw with community skills and custom plugins.

- **SKILL.md Standard** — compatible with the OpenClaw skill format
- **28+ Built-in Skills** — ready to use out of the box
- **ClawHub** — community skill repository with search and install
- **Plugin System** — Node.js plugins with JSON-RPC protocol, can register tools, hooks, prompt sections, and skills

### Web Dashboard

Built-in web console at `localhost:18790` for visual management.

- **Dashboard** — system status, audit summary, health alerts
- **Chat** — streaming conversation with attachment support and approval flows
- **Assistants** — manage personas, models, tools, and channels per assistant
- **Automations** — cron job management
- **Memory** — browse, search, and manage memory entries
- **Knowledge & Wiki** — knowledge base and wiki management
- **MCP** — server management and template marketplace
- **Skills & Plugins** — install and manage extensions
- **Security** — audit events, E-Stop, user management
- **Channels** — configure messaging platform integrations
- **Settings** — provider profiles, embedding config, system settings
- **Setup Wizard** — guided first-time configuration

### Deployment Options

| Mode | Command | Description |
|------|---------|-------------|
| **Desktop App** | Double-click `.dmg` | macOS system tray app with gateway |
| **Daemon** | `safeclaw daemon` | Gateway + channels + scheduler |
| **Gateway** | `safeclaw gateway` | HTTP/WebSocket API server |
| **CLI Agent** | `safeclaw agent` | Local interactive terminal |
| **System Service** | `safeclaw service install` | Auto-start via launchd / systemd |

## Installation

### macOS (Apple Silicon)

1. Download the latest `.dmg` from [Releases](https://github.com/jason-zou/safeclaw/releases)
2. Open the DMG and drag SafeClaw.app to Applications
3. Launch SafeClaw from Launchpad

The app is code-signed and notarized by Apple. No Gatekeeper warnings.

### First Run

On first launch, SafeClaw opens the Setup Wizard to configure:

1. **LLM Provider** — choose your model and enter API key
2. **Channel** (optional) — connect Telegram, Discord, Slack, etc.
3. **Security** — review default security settings

After setup, access the web dashboard at `http://localhost:18790`.

## Technical Specifications

| Spec | Value |
|------|-------|
| Language | Rust |
| Binary size | ~15 MB |
| Startup time | Milliseconds |
| GC pauses | Zero (no garbage collector) |
| Memory backend | SQLite |
| Vector storage | int8 quantized, 75% size reduction |
| Secret encryption | ChaCha20-Poly1305 |
| Platforms | macOS (ARM64). Windows and Linux coming soon |

## License

SafeClaw is proprietary software. See [Releases](https://github.com/jason-zou/safeclaw/releases) for downloads.

---

<p align="center">
  <a href="https://jason-zou.github.io/safeclaw/">Website</a> ·
  <a href="https://github.com/jason-zou/safeclaw/releases">Download</a> ·
  <a href="./README_CN.md">中文文档</a>
</p>
