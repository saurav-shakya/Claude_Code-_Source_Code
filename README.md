# 🔓 Claude Code — Complete Decompiled Source Code

> **The full internal source code of Anthropic's Claude Code CLI (v1.0.x), reverse-engineered and decompiled from the official npm package.**

<p align="center">
  <img src="https://img.shields.io/badge/Language-TypeScript-blue?style=for-the-badge&logo=typescript"/>
  <img src="https://img.shields.io/badge/Runtime-Bun-black?style=for-the-badge&logo=bun"/>
  <img src="https://img.shields.io/badge/Files-1,884-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Lines_of_Code-512,664-red?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Leaked-critical?style=for-the-badge"/>
</p>

---

## ⚠️ Disclaimer

This repository contains decompiled source code from Anthropic's proprietary **Claude Code** product. It is published **for educational and research purposes only**. All intellectual property belongs to **Anthropic, PBC**. This code is **not licensed for redistribution, modification, or commercial use**. Use responsibly.

---

## 📌 What is Claude Code?

**Claude Code** is Anthropic's **agentic AI coding assistant** — a terminal-based CLI tool that integrates the Claude AI model directly into your development workflow. It can:

- Read, write, and edit files autonomously
- Execute shell commands with intelligent sandboxing
- Search codebases using grep, glob, and ripgrep
- Interact with MCP (Model Context Protocol) servers
- Manage multi-agent workflows ("swarms")
- Connect to IDEs (VS Code, JetBrains, etc.)
- Resume, branch, and fork conversations
- Operate in interactive (REPL) and headless (SDK) modes

This repository contains the **complete decompiled `src/` directory** — every internal module, every tool, every prompt, every hook.

---

## 🏗️ Architecture Overview

```
src/
├── main.tsx                 # CLI entrypoint — argument parsing, migrations, session bootstrap
├── query.ts                 # Core agentic query loop — the heart of Claude Code
├── QueryEngine.ts           # Headless query engine for SDK/non-interactive modes
├── Tool.ts                  # Tool type system — defines the interface for ALL tools
├── commands.ts              # Slash-command registry and loader
├── context.ts               # System & user context builders
├── cost-tracker.ts          # Token usage & cost tracking
├── history.ts               # Session history management
├── setup.ts                 # First-run setup and onboarding
├── tools.ts                 # Tool pool assembly and filtering
├── tasks.ts                 # Background task definitions
│
├── screens/                 # Full-screen TUI screens
│   ├── REPL.tsx             # The main REPL (895KB, 5,006 lines — the largest file)
│   ├── Doctor.tsx           # Diagnostic screen
│   └── ResumeConversation.tsx
│
├── tools/                   # All 40+ built-in tools
│   ├── BashTool/            # Shell command execution (with sandbox)
│   ├── FileEditTool/        # AI-powered file editing
│   ├── FileReadTool/        # File reading with range support
│   ├── FileWriteTool/       # Full file creation/overwrite
│   ├── GrepTool/            # Ripgrep-powered code search
│   ├── GlobTool/            # File pattern matching
│   ├── AgentTool/           # Sub-agent spawning (multi-agent)
│   ├── WebSearchTool/       # Web search capability
│   ├── WebFetchTool/        # URL content fetching
│   ├── MCPTool/             # MCP server tool proxying
│   ├── NotebookEditTool/    # Jupyter notebook editing
│   ├── REPLTool/            # REPL tool execution
│   ├── LSPTool/             # Language Server Protocol integration
│   ├── TaskCreateTool/      # Background task creation
│   ├── TeamCreateTool/      # Multi-agent team creation
│   ├── TodoWriteTool/       # TODO list management
│   ├── SkillTool/           # Skill/workflow invocation
│   ├── SleepTool/           # Timed delay tool
│   ├── ScheduleCronTool/    # Cron job scheduling
│   ├── SendMessageTool/     # Inter-agent messaging
│   └── ... (40+ tools total)
│
├── components/              # 140+ Ink (React-for-CLI) UI components
│   ├── Messages.tsx         # Conversation message renderer
│   ├── PromptInput/         # User input with autocomplete, vim mode
│   ├── StatusLine.tsx       # Bottom status bar
│   ├── Spinner.tsx          # Activity spinner with tips
│   ├── ModelPicker.tsx      # Model selection dialog
│   ├── ThemePicker.tsx      # Theme customization
│   ├── PermissionRequest.tsx # Tool permission dialogs
│   ├── StructuredDiff/      # Side-by-side diff viewer
│   ├── VirtualMessageList.tsx # Virtualized scrolling message list
│   ├── ScrollKeybindingHandler.tsx # Keyboard navigation
│   ├── ConsoleOAuthFlow.tsx # OAuth authentication flow
│   └── ... (140+ components)
│
├── hooks/                   # 85+ React hooks
│   ├── useCanUseTool.tsx    # Tool permission checking
│   ├── useReplBridge.tsx    # Remote control bridge (115KB!)
│   ├── useVoiceIntegration.tsx # Voice mode (99KB!)
│   ├── useTypeahead.tsx     # Autocomplete engine (212KB!)
│   ├── useGlobalKeybindings.tsx # Keyboard shortcut system
│   └── ... (85+ hooks)
│
├── utils/                   # 300+ utility modules
│   ├── auth.ts              # Authentication (OAuth, API keys, keychain)
│   ├── config.ts            # Configuration management
│   ├── hooks.ts             # Hook lifecycle system (159KB!)
│   ├── messages.ts          # Message construction & normalization (193KB!)
│   ├── sessionStorage.ts    # Session persistence (180KB!)
│   ├── teleport.tsx         # Session teleportation (cloud sync, 175KB!)
│   ├── attachments.ts       # File/memory attachments (127KB!)
│   ├── worktree.ts          # Git worktree operations
│   ├── permissions/         # Permission system
│   ├── model/               # Model selection & capabilities
│   ├── settings/            # Settings management
│   ├── swarm/               # Multi-agent swarm system
│   ├── mcp/                 # MCP client utilities
│   ├── telemetry/           # Analytics & tracing
│   ├── sandbox/             # Command sandboxing
│   └── ... (300+ files)
│
├── services/                # Backend service integrations
│   ├── mcp/                 # Model Context Protocol client
│   ├── oauth/               # OAuth 2.0 flow
│   ├── analytics/           # GrowthBook, Statsig, telemetry
│   ├── lsp/                 # Language Server Protocol client
│   ├── compact/             # Auto-compaction (context management)
│   ├── api/                 # Anthropic API client
│   ├── plugins/             # Plugin system
│   ├── tips/                # User tips & suggestions
│   └── voice.ts             # Voice input/STT
│
├── commands/                # 100+ slash commands (/command)
│   ├── init.ts              # /init — project initialization
│   ├── commit.ts            # /commit — git commit
│   ├── review.ts            # /review — code review
│   ├── compact/             # /compact — context compaction
│   ├── doctor/              # /doctor — diagnostics
│   ├── mcp/                 # /mcp — MCP server management
│   ├── model/               # /model — model switching
│   ├── teleport/            # /teleport — session sync
│   ├── ultraplan.tsx         # /ultraplan — advanced planning
│   ├── insights.ts          # /insights — usage analytics (113KB!)
│   └── ... (100+ commands)
│
├── bridge/                  # Remote Control Bridge (mobile/web)
│   ├── replBridge.ts        # Bridge core (100KB!)
│   ├── bridgeMain.ts        # Bridge lifecycle (115KB!)
│   ├── remoteBridgeCore.ts  # Remote session bridging
│   ├── jwtUtils.ts          # JWT authentication
│   └── trustedDevice.ts     # Device trust management
│
├── state/                   # Global state management
│   ├── AppState.tsx         # React context-based state
│   ├── AppStateStore.ts     # State store definition
│   └── store.ts             # Zustand-like store
│
├── types/                   # TypeScript type definitions
│   ├── message.ts           # Message types
│   ├── permissions.ts       # Permission types
│   ├── hooks.ts             # Hook event types
│   └── plugin.ts            # Plugin types
│
├── entrypoints/             # Multiple entry points
│   ├── cli.tsx              # Interactive CLI entrypoint
│   ├── mcp.ts               # MCP server mode
│   ├── sdk/                 # SDK entrypoint
│   └── init.ts              # Initialization
│
├── coordinator/             # Multi-agent coordinator mode
│   └── coordinatorMode.ts   # Orchestrates agent teams
│
├── plugins/                 # Plugin system
│   ├── builtinPlugins.ts    # Built-in plugin loader
│   └── bundled/             # Bundled plugins
│
├── skills/                  # Skill system
│   ├── bundledSkills.ts     # Built-in skills registry
│   ├── loadSkillsDir.ts     # Dynamic skill loader
│   └── bundled/             # Bundled skill definitions
│
├── migrations/              # Settings/config migrations
│   ├── migrateSonnet45ToSonnet46.ts
│   ├── migrateOpusToOpus1m.ts
│   └── ... (11 migration scripts)
│
├── vim/                     # Vim mode implementation
│   ├── motions.ts           # Cursor motions
│   ├── operators.ts         # Vim operators
│   ├── textObjects.ts       # Text objects
│   └── transitions.ts       # Mode transitions
│
├── buddy/                   # Companion sprite system (Easter egg)
│   ├── CompanionSprite.tsx  # Animated ASCII companion
│   ├── sprites.ts           # Sprite definitions
│   └── companion.ts         # Companion logic
│
├── voice/                   # Voice mode
│   └── voiceModeEnabled.ts  # Voice feature gate
│
├── ink/                     # Custom Ink (React terminal) extensions
├── keybindings/             # Keybinding system
├── outputStyles/            # Output formatting styles
├── schemas/                 # Zod schemas
├── constants/               # Constants and configuration
├── context/                 # React contexts
├── native-ts/               # Native TypeScript helpers
├── server/                  # Direct-connect server
├── remote/                  # Remote session management
├── upstreamproxy/           # Upstream proxy support
├── moreright/               # Layout system
└── memdir/                  # In-memory directory abstraction
```

---

## 🔑 Key Findings & Revelations

### 1. 🧠 The Query Loop (`query.ts`)
The **core of Claude Code** is an async generator function `query()` that:
- Manages the agentic loop (model call → tool execution → model call → ...)
- Handles auto-compaction when context gets too large
- Implements reactive compaction for `prompt_too_long` errors
- Supports `max_output_tokens` recovery with automatic continuation
- Manages microcompact (cache-aware context shrinking)
- Tracks token budgets (`task_budget` API parameter)
- Handles streaming tool execution (parallel tool calls)

### 2. 🔒 Anti-Debugging Protection (`main.tsx`)
Claude Code **actively detects and terminates** if running under a debugger:
```typescript
if ("external" !== 'ant' && isBeingDebugged()) {
  process.exit(1);
}
```
It checks for `--inspect`, `--debug`, Node.js inspector API, and `NODE_OPTIONS` environment variables. Internal Anthropic builds (`"ant"`) bypass this check.

### 3. 🏷️ Internal vs External Builds
The codebase uses `bun:bundle` feature flags extensively:
- `feature('COORDINATOR_MODE')` — Multi-agent orchestration
- `feature('KAIROS')` — Assistant mode (proactive agent)
- `feature('VOICE_MODE')` — Voice input
- `feature('BRIDGE_MODE')` — Mobile/web remote control
- `feature('SSH_REMOTE')` — SSH remote execution
- `feature('DIRECT_CONNECT')` — Direct server connections
- `feature('ULTRAPLAN')` — Advanced planning mode
- `feature('BUDDY')` — Companion sprite Easter egg
- `feature('FORK_SUBAGENT')` — Agent forking
- `feature('TRANSCRIPT_CLASSIFIER')` — Auto-mode safety classifier
- `feature('EXPERIMENTAL_SKILL_SEARCH')` — Skill discovery
- `feature('WEB_BROWSER_TOOL')` — Browser automation tool

Code marked `"external" === 'ant'` is **Anthropic-internal only** and dead-code-eliminated from public builds.

### 4. 🔐 Authentication System (`utils/auth.ts`)
Claude Code supports **multiple authentication methods**:
- **OAuth 2.0** via `claude.ai` subscription
- **API Keys** from `ANTHROPIC_API_KEY` environment variable
- **macOS Keychain** integration for secure key storage
- **API Key Helper** — custom scripts that provide API keys on demand
- **File Descriptor** — keys passed via fd (for subprocess isolation)
- **AWS STS** — Bedrock authentication with credential refresh
- **GCP** — Vertex AI authentication
- **Foundry** — Azure Foundry authentication

### 5. 🛡️ Permission System (`utils/permissions/`)
A sophisticated multi-level permission system:
- **Permission Modes**: `default`, `plan`, `auto`, `bypassPermissions`
- **Auto-Mode Classifier**: A transcript-based safety classifier that auto-approves safe actions
- **Hook-Based Permissions**: External scripts can approve/deny tool usage
- **Always Allow/Deny Rules**: Configurable per-tool permission patterns
- **Sandbox**: Command sandboxing using OS-level primitives
- **Trust Dialog**: First-run workspace trust confirmation

### 6. 📡 Remote Control Bridge (`bridge/`)
Claude Code can be **controlled remotely** from mobile/web:
- WebSocket-based real-time communication
- JWT authentication for device trust
- Capacity wake system for mobile push notifications
- Message bridging between local and remote sessions
- QR code generation for mobile connection

### 7. 🤖 Multi-Agent System (`utils/swarm/`, `tools/AgentTool/`)
Full multi-agent orchestration support:
- **Agent Types**: Custom agents via `.claude/agents/` directory
- **Team Creation**: `TeamCreateTool` spawns agent teams
- **Inter-Agent Communication**: `SendMessageTool` for mailbox messaging
- **Coordinator Mode**: Central orchestrator that delegates to workers
- **In-Process Teammates**: Agents run in the same process (lightweight)
- **tmux/iTerm2 Backends**: Agents in separate terminal panes

### 8. 💰 Cost & Token Tracking (`cost-tracker.ts`)
Detailed financial tracking:
- Per-model token usage (input, output, cache read, cache creation)
- USD cost calculation using internal pricing tables
- Web search request counting
- Session cost persistence and restoration on resume
- Cost threshold dialogs (warns at spending limits)

### 9. 🔄 Context Management (`services/compact/`)
Four-tier context management to stay within model limits:
1. **Snip Compact** — Removes old messages from the middle
2. **Microcompact** — Cache-aware content shrinking (preserves cache keys)
3. **Auto Compact** — Full summarization of conversation history
4. **Reactive Compact** — Emergency compaction on API `prompt_too_long` errors
5. **Context Collapse** — Staged collapsing of tool-use sequences

### 10. 📝 Session Storage (`utils/sessionStorage.ts`, 180KB!)
Comprehensive session persistence:
- JSONL-based transcript storage
- Session search by title, content, and metadata
- File history snapshots for undo/redo
- Attribution tracking for git commit decoration
- Content replacement records for resumed sessions
- Background session management (concurrent sessions)

### 11. 🎨 UI System (Ink/React for Terminal)
A full React-based terminal UI built on Ink:
- Virtual scrolling message list
- Structured diff viewer
- Model picker dialog
- Theme system with multiple themes
- Vim mode with full motion/operator/text-object support
- Fullscreen layout with alternate screen buffer
- Animated spinner with contextual tips

### 12. 🔌 Plugin System (`utils/plugins/`)
Extensible plugin architecture:
- **DXT format** — Declarative extension toolkit
- **MCP servers** — Model Context Protocol integration
- **Bundled plugins** — Built-in extensions (GitHub, Slack, etc.)
- **Marketplace** — Official plugin marketplace support
- **Version management** — Automatic updates and orphan cleanup

### 13. 🎙️ Voice Mode (`services/voice.ts`, `hooks/useVoiceIntegration.tsx`)
Full voice input support:
- Speech-to-Text streaming
- Voice keyterm detection
- Integration with the prompt input system
- 99KB of voice integration hooks

### 14. 🌐 Teleport System (`utils/teleport.tsx`, 175KB!)
Cloud-based session synchronization:
- Transfer sessions between machines
- Git-based session state serialization
- Branch checkout for teleported sessions
- Error recovery and retry logic

### 15. 📊 Analytics & Telemetry (`services/analytics/`)
Comprehensive analytics infrastructure:
- **GrowthBook** — Feature flag & A/B testing
- **Statsig** — Event logging
- **OpenTelemetry** — Session tracing with Perfetto export
- **BigQuery exporter** — Internal data pipeline
- **Diagnostic logging** — PII-free diagnostic events

### 16. 🏗️ Model Migrations (`migrations/`)
Automated model string migrations:
- `Sonnet 4.5 → Sonnet 4.6`
- `Opus → Opus 1M`
- `Fennec → Opus` (internal codename)
- `Legacy Opus → Current Opus`
- Model-specific config updates

---

## 📈 Codebase Statistics

| Metric | Value |
|--------|-------|
| **Total Files** | 1,902 |
| **TypeScript/TSX Files** | 1,884 |
| **Total Lines of Code** | 512,664 |
| **Largest File** | `screens/REPL.tsx` (895 KB, 5,006 lines) |
| **Main Entry Point** | `main.tsx` (803 KB, 4,684 lines) |
| **Built-in Tools** | 40+ |
| **Slash Commands** | 100+ |
| **UI Components** | 140+ |
| **React Hooks** | 85+ |
| **Utility Modules** | 300+ |

---

## 🛠️ Technology Stack

| Technology | Purpose |
|-----------|---------|
| **TypeScript** | Primary language |
| **Bun** | Runtime & bundler (via `bun:bundle` feature flags) |
| **React / Ink** | Terminal UI framework |
| **Anthropic SDK** | Claude API client |
| **Commander.js** | CLI argument parsing |
| **Zod** | Runtime type validation |
| **chalk** | Terminal coloring |
| **execa** | Child process execution |
| **lodash-es** | Utility functions |
| **MCP SDK** | Model Context Protocol |
| **GrowthBook** | Feature flags |

---

## 🔍 Internal Codenames Discovered

| Codename | Meaning |
|----------|---------|
| **Tengu** | Analytics event prefix (`tengu_*`) — Claude Code's internal name |
| **Kairos** | Assistant/proactive mode |
| **Fennec** | An earlier Claude model variant |
| **Lodestone** | Deep link / URL protocol handler |
| **Tungsten** | An experimental tool type |
| **Walrus** | Reactive compact feature |
| **Torch** | An experimental command |
| **Ultraplan** | Advanced multi-step planning |
| **Grove** | A UI component system |
| **Buddy** | Animated companion sprite |

---

## 🚀 How This Code Was Obtained

Claude Code is distributed as a **compiled JavaScript bundle** via npm (`@anthropic-ai/claude-code`). The source was obtained by:

1. Installing the official npm package
2. Extracting the bundled JavaScript
3. Decompiling/reformatting the minified source
4. Reconstructing the module structure from import paths
5. Recovering TypeScript types from the build artifacts

The code retains original comments, function names, and module structure — Anthropic did **not** obfuscate the source beyond standard bundling.

---

## ⚖️ Legal Notice

This code is the **intellectual property of Anthropic, PBC**. It is published here for:
- Security research and vulnerability analysis
- Understanding AI agent architectures
- Educational purposes

**Do NOT use this code to:**
- Build competing products
- Bypass security or licensing mechanisms
- Distribute modified versions commercially

If Anthropic requests removal, this repository will be taken down immediately.

---

## 🤝 Contributing

This is a **static snapshot** of the decompiled source. No contributions to the source code are accepted. Issues documenting findings, architecture insights, or security observations are welcome.

---

## 📜 License

This code is **NOT open source**. It is proprietary software owned by **Anthropic, PBC**. No license is granted for use, modification, or distribution.

---

<p align="center">
  <strong>⭐ Star this repo if you found it insightful</strong><br/>
  <em>Understanding how AI agents work internally is key to building better ones.</em>
</p>
