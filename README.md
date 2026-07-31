<!--
  ┌──────────────────────────────────────────────────────┐
  │  apex-mcp-bridge — AI-Native Edge MCP Controller    │
  │  Production documentation (2026-07-31).              │
  └──────────────────────────────────────────────────────┘
-->
<p align="center">
  <img src="https://img.shields.io/badge/core-apex--mcp--bridge-6c5ce7?style=flat-square" alt="Core">
  <img src="https://img.shields.io/badge/version-latest-6c5ce7?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/rust--core-✓-DEA584?logo=rust&style=flat-square" alt="Rust">
  <img src="https://img.shields.io/badge/docker-✓-2496ED?logo=docker&style=flat-square" alt="Docker">
  <img src="https://img.shields.io/badge/MCP-2026.07-6c5ce7?style=flat-square" alt="MCP Protocol">
</p>

# Apex MCP Bridge

> **AI-Native Edge MCP Controller** — The core control layer that bridges AI agents to the physical world.

<p align="center">
  <kbd>
    <!-- 🖼️ SCREENSHOT PLACEHOLDER: Admin Dashboard -->
    <!-- Replace with: <img src="docs/screenshots/dashboard.png" alt="Apex MCP Bridge Admin Dashboard"> -->
    [Screenshot: Admin Dashboard — to be added]
  </kbd>
</p>

> � **中文文档 (Chinese)**: 完整的中文项目介绍请见 [README_ZH.md](./README_ZH.md) · 详细项目文档 [项目介绍.md](./项目介绍.md)

**Apex MCP Bridge** is an AI-native edge hub that serves as the **core control layer** for AI agents to access the physical world. It connects all MCP-compatible agent clients (TRAE, WorkBuddy, Codex, Claude Code, Cursor, etc.) on one side, and all physical devices and network services (lights, speakers, robot dogs, NAS, printers, databases, ERPs, etc.) on the other — acting as a **unified MCP smart controller**.

Deployed via Docker, it runs on any Docker-capable device (NAS, servers, Raspberry Pi, PCs) with zero hardware dependency.

---

## 💎 Key Highlights

| 🔐 5-Layer Security | 🎯 1 Unified MCP Entry | 🚀 0 Hardware Dependency |
|:---:|:---:|:---:|
| Token + Permission + TTL + Anti-Bypass + Rate Limit | All devices & services converge, agents see only **1 tool** | Docker on any device — NAS, Pi, Server, PC |
| What AI can't see, it can never touch | Constant token cost, zero hallucination inflation | Deploy in 3 minutes, no hardware required |

> **One-line pitch**: A safety gate + tool convergence layer that lets AI agents securely control any device and any service — all through a single MCP entry.

---

## ✨ Why Apex MCP Bridge

> AI agents learned to act, but no one installed a **safety gate** for them. Apex MCP Bridge is that gate.

The "Lobster Incident" in early 2026 saw 270,000 AI agent instances online simultaneously — deleting emails, leaking privacy, and getting scanned across the public internet. If this happens in an enterprise scenario: doors opened by any AI, robotic arms controlled by unauthorized commands, marketing agents accessing R&D data — the consequences are unbearable.

Apex MCP Bridge provides:

| Problem | Solution |
|---------|----------|
| No access control for AI agents | **Five-layer security** — token + permission + TTL + anti-bypass + rate limiting |
| N devices + M services = N+M MCP tools flooding the context | **Tool convergence** — all services converge to **1 MCP entry**, token-based permission filtering |
| High barrier to connect devices & services | **Dual open-source frameworks** — hardware (ESP32-S3/C3) + plugin engine, generate code via AI agents |
| Data privacy & compliance concerns | **Pure local architecture** — data never leaves the LAN, GDPR-ready |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        AI Agent Clients                           │
│   (TRAE · WorkBuddy · Claude Code · Cursor · Codex · ...)      │
└─────────────────────────────┬───────────────────────────────────┘
                              │ MCP Protocol
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Apex MCP Bridge (Rust)                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │ 5-Layer     │  │  Tool       │  │  MCP ↔ MQTT / Plugin    │ │
│  │ Security    │  │  Convergence│  │  Protocol Translation    │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │ Token       │  │  Audit      │  │  Plugin Engine          │ │
│  │ Management  │  │  Center     │  │  (auto-discovery)       │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
└───────────┬───────────────────────────────┬─────────────────────┘
            │ MQTT                          │ Plugin (stdin/stdout)
            ▼                               ▼
┌──────────────────────┐      ┌──────────────────────────────────┐
│  IoT Devices         │      │  Network Services                │
│  (ESP32-S3/C3 ·      │      │  (NAS · DB · Printer · ERP ·    │
│   Lights · Speakers · │      │   CRM · WebDAV · FTP · ...)    │
│   Robot Dogs · ...)  │      │                                   │
└──────────────────────┘      └──────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- Any Docker-capable device (NAS, server, Raspberry Pi, PC)
- Docker & Docker Compose installed
- Ports `8018` (Web UI + MCP) and `1883` (MQTT) available

### One-Command Deploy

```bash
# 1. Create project directory
mkdir -p apex-mcp-bridge && cd apex-mcp-bridge

# 2. Create docker-compose.yml (copy from repository)
#    See docker-compose.yml in this repo

# 3. Start services
docker-compose up -d
```

Wait 1-3 minutes for image pull and container startup. Verify with:

```bash
docker-compose ps
# Both apex-mcp-bridge and apex-mcp-bridge-mariadb should be "Up"
```

### Post-Deploy Configuration

1. **Set HOST_HOSTNAME**: Edit `docker-compose.yml`, set `HOST_HOSTNAME` to your LAN IP (e.g., `192.168.1.100`) — required for agent callbacks and device registration routing. Restart after change: `docker-compose up -d`

2. **Firewall**: Open ports `8018/tcp` and `1883/tcp`

3. **Login**: Open `http://<YOUR_IP>:8018` in your browser. Default admin credentials are provided — **change the password immediately after first login**.

   🖼️ **SCREENSHOT PLACEHOLDER**: Login page and admin dashboard after successful login.
   <!-- Replace with: <img src="docs/screenshots/login-and-dashboard.png" alt="Login and Dashboard"> -->

### 5-Step Setup Flow

```
1. Create Users        → User Management
2. Add Devices/Plugins → Plugin Management + Device Management
3. Issue Tokens        → MCP Token Management + MCP Permission Management
4. Connect Agents      → MCP Endpoint (http://<IP>:8018/mcp)
5. Audit Everything    → Audit Center
```

---

## 🔌 Plugin Installation

Plugins translate any network service into MCP tools. Example: SMB file plugin.

1. Download a plugin folder from the plugin repository: <https://github.com/apex-freen/apex-mcp-service-plugins>

2. Copy the plugin folder into `./data/service-plugins/`

3. The bridge auto-detects and loads it dynamically — **no restart needed**

> **Philosophy**: Copy a plugin folder into `service_plugins/`, and the bridge dynamically detects and loads it — no restart, no wiring, no registration step.

> **The real power**: You don't even need to write a single line of code. This project provides a complete, standardized plugin framework. Simply hand these templates as constraints to an AI agent, describe the service you want in natural language (a printer plugin? a data report?), and the agent generates complete, ready-to-run plugin code within the framework.

---

## 🔧 Core Features

### Five-Layer Security System

| Layer | Mechanism | Defense Against |
|-------|-----------|-----------------|
| ① Token Auth | Every request must carry a valid token | Unauthorized agents can't even see the device list |
| ② Permission Check | Token-bound fine-grained permissions (device/function level) | Only authorized devices/functions can be controlled |
| ③ TTL Expiry | Token-level TTL, second-level revocation | Compromised tokens can be invalidated instantly |
| ④ Anti-Bypass | Unified validation channel for all MCP requests | Direct call construction with device ID is blocked |
| ⑤ Rate Limiting | Token/device/function 3-dimensional throttling | Prevents AI hallucination loops and brute-force probing |

### Tool Convergence (Unique Innovation)

| Dimension | Traditional Direct MCP | Apex MCP Bridge |
|-----------|------------------------|-----------------|
| Tools visible to agent | N + M (all exposed) | **1** (unified entry) |
| Token cost per conversation | Linear growth with device count | **Constant** (device-independent) |
| No-permission devices | Exposed in tool list, relies on model "discipline" | **Completely invisible** |
| Hallucination risk | Higher with more tools | **Dramatically reduced** |

### System Modules

| Module | Functions |
|--------|-----------|
| **Audit Center** | Audit stats dashboard, operation audit log, authorization audit, token audit |
| **User Management** | Create/disable/delete system users |
| **Device Management** | Registered IoT devices, online status, capability lists |
| **Plugin Management** | Install/uninstall/enable/disable plugins, each = a set of MCP tools |
| **MCP Permission Management** | Bind tools/devices to tokens, fine-grained "who can do what" |
| **MCP Token Management** | Create/renew/revoke agent access tokens with TTL |
| **System Settings** | Port, log level, HOST_HOSTNAME, etc. |

---

## 🧩 Related Projects

| Project | Description | Link |
|---------|-------------|------|
| **apex-mcp-bridge** | Core project (this repository) | [GitHub](https://github.com/apex-freen/apex-mcp-bridge) |
| **apex-mcp-service-plugins** | Plugin ecosystem — extend the bridge with service capabilities | [GitHub](https://github.com/apex-freen/apex-mcp-service-plugins) |
| **apex-mcp-esp32-s3-v6** | Hardware framework (ESP32-S3) — open-source chip template | [GitHub](https://github.com/apex-freen/apex-mcp-esp32-s3-v6) |
| **apex-mcp-esp32-c3-v6** | Hardware framework (ESP32-C3) — open-source chip template | [GitHub](https://github.com/apex-freen/apex-mcp-esp32-c3-v6) |

---

## ⚠️ Security Notice

- **Default password**: The default database password `Apex1234` is for evaluation only. For production or public-facing deployments, **always change** `MARIADB_ROOT_PASSWORD` and `MARIADB_PASSWORD` to strong passwords.

- **Plugin safety**: Plugins execute arbitrary Python code on your host. Only install plugins from the official repository or sources you fully trust. If you obtained a plugin from an unofficial channel and don't understand its code, **do not use it** — it may contain malicious logic.

- **Network exposure**: The bridge is designed for LAN deployment. If you need remote access, use a VPN or secure tunnel rather than exposing ports directly to the internet.

---

## 🤝 Contributing

Contributions are welcome and greatly appreciated! Every little bit helps, and credit will always be given.

### How Can I Contribute?

| Contribution Type | How |
|---|---|
| **Report Bugs** | Open an issue with clear reproduction steps, environment info, and expected vs actual behavior |
| **Suggest Features** | Open an issue describing the feature, use case, and why it would be useful |
| **Write Plugins** | Build a new service plugin using the plugin framework and submit it to the [plugin repository](https://github.com/apex-freen/apex-mcp-service-plugins) |
| **Hardware Support** | Add support for new chips/devices using the hardware framework templates |
| **Documentation** | Fix typos, clarify sections, add examples, translate to other languages |
| **Code** | Fix bugs, implement new features, improve performance — see below for the workflow |

### Development Workflow

1. **Fork** the repository
2. **Clone** your fork: `git clone https://github.com/<your-username>/apex-mcp-bridge.git`
3. **Create** a feature branch: `git checkout -b feature/amazing-feature`
4. **Make** your changes
5. **Commit** with clear messages: `git commit -m 'feat: add amazing feature'`
6. **Push** to the branch: `git push origin feature/amazing-feature`
7. **Open** a Pull Request

### Plugin Development

The easiest way to contribute is to build a plugin. You don't need deep knowledge of the core codebase — just follow the plugin framework:

1. Copy the reference plugin structure from [apex-mcp-service-plugins](https://github.com/apex-freen/apex-mcp-service-plugins)
2. Describe your target service to an AI agent (e.g., "a printer plugin that supports CUPS")
3. The agent generates the plugin code within the framework constraints
4. Test it locally, then submit a PR to the plugin repository

### Community

- Star the repository if you find it useful
- Share your use cases and plugins in discussions
- Help other users by answering issues

---

## ❓ FAQ

<details>
<summary><b>Q: What MCP clients are supported?</b></summary>

All MCP-compatible clients: TRAE, WorkBuddy, Codex, Claude Code, Cursor, and any client implementing the Model Context Protocol.

</details>

<details>
<summary><b>Q: Can it run without Docker?</b></summary>

Docker is the officially supported and recommended deployment method. Direct binary deployment is possible for advanced users but not officially documented.

</details>

<details>
<summary><b>Q: How do I add a new service as an MCP tool?</b></summary>

Two ways:
1. **Plugin**: If there's an existing plugin, just copy it into `service_plugins/`
2. **Custom plugin**: Use the standard plugin framework, describe your service to an AI agent, and it generates the plugin code for you

</details>

<details>
<summary><b>Q: What hardware devices are supported?</b></summary>

Officially: ESP32-S3 and ESP32-C3 with the open-source hardware framework templates. Any MQTT-capable device can be integrated with custom firmware.

</details>

<details>
<summary><b>Q: Does my data leave the LAN?</b></summary>

No. Apex MCP Bridge uses a pure local architecture — all data stays on your network. The bridge never calls home or uploads any data.

</details>

<details>
<summary><b>Q: What skills do I need for plugin development?</b></summary>

**Minimal: basic Python + ability to describe what you want.**

The plugin framework handles all the boilerplate — input schemas, communication protocol, error handling, and response formats. You can:
- **Zero-code approach**: Describe the service to an AI agent ("I need a WebDAV plugin") and it generates the code
- **Basic Python**: Tweak existing plugins or write simple handlers if the AI output needs adjustment
- **Advanced**: Full custom plugin development for complex services

The key skill is knowing *what* service you want to expose — the framework handles *how*.

</details>

<details>
<summary><b>Q: What Python version is required for plugins?</b></summary>

**Python ≥ 3.10** is required for plugin handlers. The Docker image ships with a compatible Python runtime, so if you deploy via Docker you don't need to worry about it. If developing locally, ensure your Python version meets this requirement.

</details>

<details>
<summary><b>Q: How do I upgrade to a newer version?</b></summary>

```bash
# 1. Pull the latest image
docker-compose pull

# 2. Restart containers (preserves all data in volumes)
docker-compose up -d
```

Your configuration, plugins, devices, tokens, and audit logs are all stored in Docker volumes (`./data/`) and are **persisted across upgrades**. No migration step is needed for minor version updates. For major version upgrades, check the release notes for any specific instructions.

</details>

<details>
<summary><b>Q: How many devices can it handle? What's the performance?</b></summary>

A single Apex MCP Bridge instance comfortably handles:
- **100+ connected IoT devices** via MQTT
- **50+ active plugins** (network services)
- **Concurrent MCP requests** from multiple AI agents simultaneously

The Rust core is highly optimized and lightweight — it uses roughly 30-50 MB of RAM under normal load. On a Raspberry Pi 4B, it can process hundreds of MCP tool calls per second. For larger deployments (thousands of devices), you can run multiple Bridge instances with a shared configuration.

</details>

---

<p align="center">
  <sub>Built with ❤️ by the Apex MCP Bridge Team</sub>
</p>
