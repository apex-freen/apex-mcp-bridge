# Apex MCP Bridge — Complete Project Guide

## Related Projects

### Core Project

| Region | Platform | Link |
|---|---|---|
| Global | GitHub | <https://github.com/apex-freen/apex-mcp-bridge> |
| China | Gitee | <https://gitee.com/freen/apex-mcp-bridge> |

### Service Plugins Repository

| Region | Platform | Link |
|---|---|---|
| Global | GitHub | <https://github.com/apex-freen/apex-mcp-service-plugins> |
| China | Gitee | <https://gitee.com/freen/apex-mcp-service-plugins> |

### Hardware Firmware Repositories

Both hardware frameworks are **open source under the MIT License**.

| Chip Model | Global (GitHub) | China (Gitee) |
|---|---|---|
| ESP32-S3 | <https://github.com/apex-freen/apex-mcp-esp32-s3-v6> | <https://gitee.com/freen/apex-mcp-esp32-s3-v6> |
| ESP32-C3 | <https://github.com/apex-freen/apex-mcp-esp32-c3-v6> | <https://gitee.com/freen/apex-mcp-esp32-c3-v6> |

> **Note**: The hardware framework and plugin framework are open source (MIT). The core Apex MCP Bridge project itself is distributed as a Docker image.

---

## 1. Project Introduction

**Apex MCP Bridge** is an **AI-native edge hub** that serves as the **core control layer** for AI agents to access the physical world. It connects all MCP-compatible agent clients (including TRAE, WorkBuddy, Codex, Claude Code, Cursor, etc.) on one side, and all physical devices and network services (lights, speakers, robot dogs, NAS, printers, databases, ERPs, etc.) on the other — acting as a **unified MCP smart controller**.

The project is deployed via Docker containers and can run on any Docker-capable device (NAS, servers, Raspberry Pi, PCs), significantly reducing deployment barriers and hardware costs.

Three key characteristics:
1. **Authorizable and manageable** — tokens, permissions, and audit trails for full security control
2. **Controls devices, accesses service resources** — turn lights on/off, play music, read intranet files, all executable through it
3. **Links services and devices together** — one sentence triggers speaker playback + robot dog dance + display power-on simultaneously

---

## 2. Project Overview

### 2.1 Project Positioning

Apex MCP Bridge plays four key roles simultaneously:

| Role | Description |
|---|---|
| **Translator** | Agent clients speak MCP, IoT hardware speaks MQTT, intranet services use the plugin engine — three languages that don't understand each other; Bridge translates in the middle |
| **Gatekeeper** | Agents without tokens can't even see the device list; permissions expire instantly; five-layer security blocks unauthorized commands |
| **Executor** | Turns abstract MCP commands into concrete light switches, speaker playback, network service and device linkage — giving agents the ability to "reach out and touch the physical world" |
| **Pathfinder** | Provides **open-source** IoT chip hardware framework + plugin service framework (MIT License), so anyone can quickly develop their own agent-enabled devices and services using AI agents + open-source frameworks |

### 2.2 Three Core Capabilities

| # | Capability | Description |
|---|---|---|
| ① | **Five-Layer Security** | Token auth → Permission check → TTL expiry → Anti-bypass → Rate limiting. No token = invisible; no permission = uncontrollable; what AI can't see it can never touch |
| ② | **Dual Open-Source Framework Support (MIT)** | Hardware line: ESP32-S3/C3 chip full engineering framework open-source; Service line: Plugin development framework open-source, supporting plugin-based MCP enablement for any network service (databases, printers, ERPs, CRMs, WebDAV, FTP, etc.). Strictly aligned with MCP standard protocol, directly usable by all MCP-compatible agent clients |
| ③ | **Device × Service Dual-Channel Access** | Hardware devices via MQTT, network services via plugins — two channels unified under security validation and MCP interface — agents get complete, freely combinable capabilities rather than scattered tools |

### 2.3 Core Innovation: Tool Convergence Mechanism

This is Apex MCP Bridge's unique capability that sets it apart from all existing solutions:

**Problem**: N devices + M services = N+M independent MCP tools exposed to agents → massive Token consumption per conversation → more tools = higher hallucination rate

**Bridge's solution**: All devices and services converge to **1 MCP entry point** → agents see only 1 tool → Bridge automatically converges the visible tool list based on token permissions

| Dimension | Traditional Direct MCP | Apex MCP Bridge |
|---|---|---|
| Tools visible to agent | N + M (all exposed) | **1** (unified entry) |
| Token cost per conversation | Linear growth with device/service count | **Constant** (device-independent) |
| No-permission devices | Exposed in tool list, relies on model "self-discipline" | **Completely invisible**, not in the list |
| Hallucination risk | Higher with more tools | **Dramatically reduced** |

### 2.4 Five-Layer Security System

| Layer | Mechanism | Defends Against |
|---|---|---|
| ① Token Auth | Every request must carry a valid token | Uncredentialed AI can't even see the device list |
| ② Permission Check | Token-bound fine-grained permissions (device/function level) | Only authorized devices/functions can be controlled |
| ③ TTL Expiry | Tokens support TTL, second-level revocation | Compromised tokens can be invalidated instantly |
| ④ Anti-Bypass | Unified validation channel for all MCP requests | Direct call construction with device ID is blocked |
| ⑤ Rate Limiting | Token/device/function 3-dimensional throttling | Prevents high-frequency loops caused by AI hallucination |

### 2.5 Technical Architecture

- **Core Controller**: High-performance MCP Server framework implemented in Rust, protocol translation between MCP ↔ MQTT / Plugin engine
- **Hardware Access**: **Open-source** IoT chip framework templates (ESP32-S3/C3), AI agents can generate MQTT firmware code on demand
- **Plugin Engine**: Plugin-based configuration architecture, supporting rapid MCP enablement for any network service (NAS, databases, printers, ERPs, CRMs, WebDAV, FTP, reporting servers, etc.), with each new plugin automatically inheriting the full security validation chain
- **Smart Cache**: Multi-level queries (region → device → function), full-chain validation on first access, local cache reads thereafter
- **Pure Local Architecture**: Data never leaves the intranet, meeting GDPR and other privacy compliance requirements

---

## 3. System Features and Usage

> 📖 **Deployment & Operations**: This section focuses on system features and usage. For Docker deployment, configuration, firewall setup, and upgrade procedures, please refer to [README.md](../README.md) in the repository root.

### 3.1 System Features Overview

Apex MCP Bridge's core goal is: **Let authorized agents securely call devices and network services on your LAN via the MCP protocol.**

Around this goal, the system configuration logic is divided into five steps:

1. **Configure who**: Create users → User Management
2. **Configure what**: Install plugins (network services → MCP tools) + Register devices (IoT hardware → MCP tools) → Plugin Management / Device Management
3. **Configure authorization**: Issue tokens to agents, bind permissions → MCP Token Management / MCP Permission Management
4. **Provide MCP**: Agent access → MCP Endpoint
5. **Audit**: Who called what, when, and did what — all traceable → Audit Center

The system includes the following functional modules:

#### Audit Center

| Feature | Description |
|---|---|
| Audit Statistics | Visualized statistics of operation counts, device call frequencies, and token usage |
| Operation Audit | Complete record of every agent tool call, parameters used, and results returned |
| Authorization Audit | Complete record of token creation/modification/revocation |
| Token Audit | Usage frequency, source IP, and anomaly behavior flags for each token |

<p align="center">
<img src="dashboard.png" width="400" alt="Data Overview"><br>
<em>Data Overview Dashboard</em><br><br>
<img src="operation-audit.png" width="400" alt="Operation Audit"><br>
<em>Operation Audit</em><br><br>
<img src="authorization-audit.png" width="400" alt="Authorization Audit"><br>
<em>Authorization Audit</em>
</p>

#### Admin Panel

| Feature | Description |
|---|---|
| User Management | Create/disable/delete user accounts with system access |
| Device Management | View registered IoT devices, device status (online/offline), device capability lists |
| Plugin Management | Install/uninstall/enable/disable plugins, each plugin corresponds to a set of MCP tools |
| MCP Permission Management | Bind specific tools/devices to tokens, fine-grained control over "who can do what" |
| MCP Token Management | Create/renew/revoke agent access tokens, supports TTL expiry |
| System Settings | Basic configuration (ports, log levels, HOST_HOSTNAME, etc.) |

<p align="center"><img src="token-qrcode.png" width="400" alt="Token QR Code"><br><em>Token QR Code — Scan to Connect Agent</em></p>

<p align="center"><img src="device-list.png" width="400" alt="Device Management"><br><em>Device Management — IoT Device List & Capability Config</em></p>

<p align="center"><img src="plugin-list.png" width="400" alt="Plugin Management"><br><em>Plugin Management — Network Service MCP Enablement</em></p>

<p align="center"><img src="token-qrcode.png" width="400" alt="Token Management"><br><em>MCP Token Management — Creation & Permission Binding</em></p>

> **System logic in one sentence**: First create users, then add devices and plugins (tools), then bind permissions to tokens, agents use tokens to access the MCP endpoint, with full audit trail throughout.

Apex MCP Bridge does five things:

| Capability | Description |
|---|---|
| **Protocol Translation** | Translates NAS SMB/NFS dialects, device MQTT dialects → into MCP "standard language" |
| **Tool Convergence** | No matter how many services and devices you have, agents see only 1 MCP entry point |
| **Five-Layer Security** | Token auth + Permission check + TTL expiry + Anti-bypass detection + Rate limiting |
| **Plugin Extension** | SMB file management, MQTT device access… plugin-based, install on demand. Rapidly extend any network service based on seed plugins |
| **Hardware Framework** | Provides **open-source** template framework (MIT) for consumer-grade chips (ESP32-S3, ESP32-C3), leveraging framework rules and constraints so AI agents can rapidly generate IoT device firmware based on your requirements, also enables retrofitting compatibility for traditional IoT chip devices |

### 3.2 Plugin Installation

Plugins are modules that translate network services into MCP tools. Using the SMB file plugin as an example:

1. Download the plugin folder (e.g., `smb-file-plugin`) from the plugin repository:
   - Global: <https://github.com/apex-freen/apex-mcp-service-plugins>
   - China: <https://gitee.com/freen/apex-mcp-service-plugins>
2. Copy the plugin folder into `./data/service-plugins/` directory

   🖼️ **SCREENSHOT PLACEHOLDER**: Plugin directory structure (showing smb-file-plugin and other plugin folders under service-plugins)

3. System auto-detects and loads it — no restart needed

   🖼️ **SCREENSHOT PLACEHOLDER**: Plugin list in admin panel after successful loading (showing plugin status as "Enabled", corresponding MCP tool list)

### 3.3 Hardware Device Access

1. Use the **open-source** ESP32-S3/C3 hardware framework templates, generate device firmware on demand through AI agents (such as TRAE):
   - ESP32-S3 firmware repo: <https://github.com/apex-freen/apex-mcp-esp32-s3-v6>
   - ESP32-C3 firmware repo: <https://github.com/apex-freen/apex-mcp-esp32-c3-v6>

   🖼️ **SCREENSHOT PLACEHOLDER**: Firmware flashing tool interface (showing ESP32 firmware flashing process)

2. Flash firmware to ESP32 chip

   🖼️ **SCREENSHOT PLACEHOLDER**: Hardware wiring photo (showing ESP32-S3 + PM5102 audio module and peripherals wired up)

3. Power on the device, configure WiFi, auto-register to Bridge

   🖼️ **SCREENSHOT PLACEHOLDER**: Device WiFi configuration + auto-registration notification

4. View device status and capability list in admin panel

   🖼️ **SCREENSHOT PLACEHOLDER**: Device details in admin panel (showing online status, capability list, supported MCP operations)

### 3.4 Connect Agents

In any MCP-compatible agent client (such as TRAE, WorkBuddy, Codex, Claude Code, Cursor, etc.), add an MCP Server with the address:

```
http://<YOUR_IP>:8018/mcp
```

Use with your token.

---

## 4. Project Value

### 4.1 Security Value

**AI gained hands-on capabilities, but nobody installed a "safety gate" for it.** Apex MCP Bridge is that gate.

- The "Lobster Incident" in early 2026: 270,000 AI agent instances online simultaneously, leading to incidents including email deletion, privacy leaks, and public internet scanning
- In enterprise scenarios: door access opened by any AI, robotic arms controlled by unauthorized commands, marketing agents accessing R&D data without permission — consequences are unbearable
- Bridge's five-layer security validation ensures **"It's not that you can't control anything — it's that only authorized agents can control"**

### 4.2 Efficiency Value: Lowering Deployment Barriers

As a smart hardware developer, building the full chain from scratch requires:
- Mastery of embedded development + network protocol stack + MQTT Broker + authentication logic + permission system
- Renting servers, setting up environments, full go-live → **at least two weeks minimum**

Using Apex MCP Bridge:
- **Hardware development**: Set up ESP-IDF environment once, then for any smart light or smart motor project, give the template to an agent and say "GPIO4 to GPIO5, add PWM dimming" — firmware generated
- **Plugin development**: Copy the `smb-file-plugin` folder, give it to an agent and say "change to WebDAV protocol" — new plugin born in seconds
- **Everyone is an embedded engineer. Everyone is a backend developer. Framework constraints + agent generation = you only need to describe the requirement.**

### 4.3 Commercial Value: Cost Comparison

Taking a single feature "provide song-request service for a café" as an example:

| Solution | Cost | Timeline | Operations |
|---|---|---|---|
| Traditional (outsourcing + cloud) | Hundreds of thousands RMB+ | 6+ months | Requires ongoing operations team |
| Apex MCP Bridge | A few thousand RMB hardware cost | 1 day deployment | Plug and play, zero technical team |

Smart services that were previously only affordable for large malls are now within reach for a small café, a community service station, or a rural homestay.

### 4.4 Social Value: Technology Democratization

In July 2026, the Ministry of Commerce and eight other departments jointly issued the "Opinions on Accelerating the Innovation and Development of the Retail Industry", explicitly requiring offline entities to "advance digital transformation, scenario-based renovation, and digital-intelligence empowerment."

**Policy has shifted from "bonus" to "mandatory"**, yet small merchants, community service points, and elderly care stations — the groups most in need of digital assistance — are blocked by costs.

Apex MCP Bridge enables them to meet policy requirements for digital service capabilities with a few thousand RMB in hardware costs, a one-day deployment cycle, and zero technical team.

---

## 5. Project Goals

### 5.1 Short-Term Goals

- Build a complete MCP Bridge ecosystem, making Docker deployment the mainstream method
- Enrich the plugin ecosystem: cover NAS, printers, databases, WebDAV, smart home, and other common scenarios
- Expand hardware support: beyond ESP32-S3/C3, support more mainstream IoT chips
- Improve developer toolchain: make the process of agents generating firmware and plugins even smoother

### 5.2 Mid-Term Goals

- Become the **de facto standard for MCP device and service access**
- Build a developer community: get more people involved in plugin and hardware framework development
- Enterprise features: multi-tenancy, audit reporting, compliance certification
- Commercial validation: explore monetization paths through hardware, services, and technical support

### 5.3 Long-Term Goals

- **Let AI agents safely and controllably reach the physical world**
- Become the infrastructure for offline entity digital transformation
- Drive "agent controllers" to become as ubiquitous as routers in homes and offices
- Build an open-source ecosystem so the entire industry benefits from standardized MCP access solutions

---

## 6. Target Audience

### Three Layers of Target Audience, Three Layers of Value

| Layer | Audience Profile | Pain Points | Change Brought by Bridge |
|---|---|---|---|
| **End Users** | Mall customers, community residents, elderly care station residents, café patrons | Installing dozens of Apps, searching for various remote controls, asking staff | **One Token → Agent handles all services** |
| **Small Merchants / Community Services / Elderly Care** | Café owners, community workers, elderly care station operators, rural homestay hosts | Technical team + cloud + hundreds of thousands budget + 6-month cycle | **One device plugged in → Three steps to open** |
| **Developers / Hardware Vendors** | Embedded developers, smart hardware vendors, AI application developers, system integrators | Learn embedded + learn protocols + set up environments + write auth | **Open-source framework + AI agent generation → Working by afternoon** |

### 6.1 End Users

Scan a code when entering a mall to get a Token, tell the agent directly:
- "What's good to eat on the third floor? Get me a queue number"
- "What movies are playing at the cinema and at what times?"
- "Order me a latte at the café, I'll be there in five minutes"

**One Token = Full capabilities of the entire scene, no App installs, no remote controls to find.**

### 6.2 Small Merchants / Community Services / Elderly Care

This is the most socially valuable layer:
- A community elderly care station buys one Bridge, plugs it in, connects speakers, lights, and call systems
- Elderly residents hold a Token and say one sentence to control — no App installs, no learning operations
- Smart services previously only affordable for large malls are now within reach of a community station, a small café, or a rural homestay

### 6.3 Developers / Hardware Vendors

- Use the **open-source** hardware framework templates, tell the AI agent what functionality you want, and it generates firmware code directly
- Flash it in — device connected, from idea to working in one afternoon
- Going global with GDPR compliance? Pure local architecture as safety net, all data stays on customer premises

---

## 7. What Users Gain

### 7.1 For End Users

| Gain | Description |
|---|---|
| **Ultimate Convenience** | One Token for everything, no App installs, no passwords to remember, no remote controls to find |
| **Natural Interaction** | Talk to agents in natural language, say what you want directly |
| **Scenario-Based Services** | Automatic device and service coordination, one sentence triggers multi-device synergy (e.g., "home mode" auto lights + music + curtains) |
| **Privacy & Security** | Data never leaves the LAN, no worry about personal data being uploaded to the cloud |

### 7.2 For Merchants and Operators

| Gain | Description |
|---|---|
| **Extremely Low Cost** | A few thousand RMB in hardware + one-day deployment cycle, vs. hundreds of thousands invested in traditional solutions |
| **Zero Technical Barrier** | No technical team needed, plug-and-play, visual admin panel |
| **Digital Compliance** | Meets nine-ministry policy requirements for offline entity digital transformation |
| **Differentiated Competitiveness** | Competitors are still building Apps, you're already using AI agents — one generation ahead in user experience |
| **Data-Driven Operations** | Audit Center provides complete user behavior data to support business decisions |

### 7.3 For Developers

| Gain | Description |
|---|---|
| **Efficiency Boost** | Hardware development compressed from two weeks to an afternoon, plugin development from days to minutes |
| **Focus on Core Value** | No more reinventing the wheel (MQTT, auth, permission systems), focus on business logic |
| **Ecosystem Support** | Both hardware framework and plugin framework are MIT open-source, no commercial restrictions, freely usable in commercial projects |
| **AI Empowerment** | Deep integration with all-platform MCP agents, let AI write code for you, constrained generation ensures quality |
| **Hardware Capability Monetization** | Your embedded skills + AI agents = entirely new product forms and business models |

### 7.4 For Hardware Vendors

| Gain | Description |
|---|---|
| **AI Capability Enhancement** | Your hardware immediately gains AI-agent controllability, dramatically increasing product value-add |
| **Standardized Access** | Follows MCP standard protocol, one development compatible with all MCP-enabled agent platforms |
| **Rapid Iteration** | Based on **open-source** framework templates, new product cycle from prototype to market significantly shortened |
| **Global Compliance** | Pure local architecture + data never leaves the intranet, meeting GDPR and other international privacy regulations |

---

## 8. Monetization Paths

### 8.1 Hardware Sales

| Monetization Method | Description | Target Customers |
|---|---|---|
| **Ready-to-Use Hardware Sales** | Launch pre-configured Raspberry Pi/industrial PC with Bridge pre-installed, plug-and-play | Small merchants, community services, elderly care stations |
| **Custom Hardware Solutions** | Custom scenario-specific hardware solutions for enterprise clients (e.g., mall smart controller, hotel guest control system) | Chain enterprises, hotels, property management companies |
| **Dev Board/Kit Sales** | Sell ESP32 dev boards + sensor kits to developers, bundled with tutorials as starter packs | Developers, hardware enthusiasts, students |

### 8.2 Software and Services

| Monetization Method | Description | Target Customers |
|---|---|---|
| **Commercial Licensing** | Provide commercial licenses for customers requiring enterprise features (multi-tenancy, SLA, technical support) | Mid-to-large enterprises, government agencies |
| **SaaS Management Platform** | Provide cloud management platform for centralized management of multiple Bridge devices, batch configuration deployment, global data viewing | Chain enterprises, system integrators |
| **Plugin Marketplace** | Build official plugin marketplace, developers can upload paid plugins, platform takes commission | All users |
| **Custom Plugin Development** | Custom service plugins for clients (e.g., integrating specific ERPs, specific IoT platforms) | Enterprises with special integration needs |

### 8.3 Technical Support and Consulting

| Monetization Method | Description | Target Customers |
|---|---|---|
| **Deployment Services** | On-site/remote deployment services, including device access, plugin configuration, agent integration | Merchants lacking technical capability |
| **Technical Training** | Training courses for enterprises and developers, teaching how to develop smart scenarios using Bridge | Enterprise technical teams, developers |
| **Scenario Solution Consulting** | Scenario-based solution consulting for clients, such as smart mall, smart elderly care, smart homestay solutions | Industry clients |
| **Annual Technical Support** | Annual technical support services, including troubleshooting, version upgrades, security hardening | Enterprise customers |

### 8.4 Ecosystem and Platform

| Monetization Method | Description | Target Customers |
|---|---|---|
| **Certification Program** | Launch hardware/plugin certification program, certified products receive official recommendation and traffic support | Hardware vendors, plugin developers |
| **Revenue Sharing Model** | Partner with offline scenarios (malls, cafés, elderly care stations), share revenue based on Token usage/call volume | Scenario operators |
| **Data Services** | Under compliance premises, provide anonymized industry data insight reports | Industry researchers, investment institutions |

### 8.5 Specific Business Scenario Examples

**Scenario 1: Smart Café Solution**
- Sell a hardware bundle (Bridge + smart speaker + smart light controller): ¥2980
- On-site deployment service: ¥500
- Annual technical support: ¥365/year
- Follow-up custom plugins (e.g., POS system integration): ¥2000+

**Scenario 2: Community Elderly Care Station Solution**
- Batch hardware (10 stations minimum): ¥1980/unit
- Overall solution consulting: ¥20000
- Custom call system plugin: ¥5000
- Staff training: ¥3000/session

**Scenario 3: Developer Starter Kit**
- ESP32-S3 dev board + PM5102 audio module + sensor kit + tutorials: ¥398
- Online course (from 0 to building first AI-controlled device): ¥199
- Advanced course (plugin development + scenario coordination): ¥499

---

## Summary

Apex MCP Bridge is more than just a technical product — it's the **security foundation for AI agents to reach the physical world**, the **democratization tool for offline entity digital transformation**, and the **empowerment platform for developers to unleash creativity**.

The project's core goal remains constant:

> **Let AI agents safely, controllably, and cost-effectively touch the physical world; let technology benefit every small merchant, every community service point, and every person in need.**

And the business opportunities within belong to everyone willing to participate — whether as users, developers, hardware vendors, or partners.
