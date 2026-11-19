# 🤖 AI & Automation Builds

A collection of the AI-assisted tools and automations I've been building — some for my home lab, some for my day-to-day work. Most of these were built hands-on with **Claude Code** as a pair-programmer, running on my always-on home server.

> 🔒 Bot tokens, API keys, internal company data, and personal information are intentionally left out. Work-related tools are described at a high level only.

---

## 📋 Projects

| Project | What it does | Stack | Status |
|---|---|---|---|
| 🗺️ **Home Lab Mission Control** | Live network map of every device in the house — status, activity, storage, remote access, threat view | Web app in Docker, UniFi API, Jellyfin API, Tailscale, Git + launchd sync | ✅ Running |
| 🌩️ **Threat Forecast** | Turns real firewall/IPS attack data into a TV-weather-style threat briefing | Web app in Docker, UniFi data | ✅ Running |
| 💻 **macOS Menu-Bar Mods** | "Lid FX" lid-close screen animation + "KAI Wall" multi-monitor wallpaper manager | macOS app bundles built via shell script, Claude | 🚧 Testing |
| 🗓️ **Crew Hub** | Scheduling & info app for a field data-collection team | Google Apps Script, Google Sheets, Slack | ✅ Built |
| 📟 **Kai OS Telegram Bot** | Control and check my home server from my phone | Telegram Bot API, Claude Code | ✅ Running |
| 🧠 **Agentic OS** | Personal AI "operating system" — notes, skills, scheduled automations | Claude Code, Obsidian, cron-style schedules | 🚧 Building |
| 🎙️ **Jarvis Voice Assistant** | Voice assistant with Mac + phone clients synced through the home server | Claude API (pay-as-you-go), Claude Code | 🚧 Building |

---

## 🗺️ Home Lab Mission Control — Live Network Dashboard
**Problem:** My home lab spans a 24/7 server, a gaming PC, several laptops, phones, a TV, and smart-home devices across multiple VLANs. Checking on them meant jumping between the UniFi app, Tailscale, Jellyfin, and Docker.

**What I built:** a single self-hosted dashboard (Docker container on the home server) that shows the whole network as a live map. I started it with Grok and now maintain it with Claude and Claude Code.

- **Live network map** with kind-specific device icons and animated traffic "light trails"
- **Device activity in plain language** — "Watching something," "Someone's using it," "Idle" — built from live per-client UniFi traffic plus keyboard/mouse idle time reported by each computer, with a **"Right Now"** summary strip
- **What's playing** — Jellyfin titles from Jellyfin's own API; TV app usage from UniFi's traffic-identification (DPI, Deep Packet Inspection) data; a 30-day **"Apps used"** history per device
- **Per-computer health** — small PowerShell and Bash reporter scripts send drive usage, CPU, and idle time every 10 minutes
- **Remote-access details** — Tailscale device status (direct vs. relayed connection, data transferred, key expiry)
- **Comings & goings** — arrive/leave notifications for our cars through a **read-only** bridge to a self-hosted vehicle logger, with Telegram alerts
- **Hardening** — locked down the API endpoint that lets the dashboard change device labels, so it can't be called without authorization
- **Multi-machine development** — the code lives in a private Git repo; a `launchd` job on the server auto-pulls, pushes, and restarts the container, so I can edit from any of my computers (Mac or Windows) and a `CLAUDE.md` handoff file gives AI coding agents full project context

**Problems solved:**

| Symptom | Root cause | Fix |
|---|---|---|
| Animations missing only on the Windows PC | Windows "reduced motion" accessibility setting | Added a dashboard setting that overrides it |
| Info panels jumped to the top every refresh | Re-render reset scroll position | Preserved scroll position across data refreshes |
| Couldn't show which websites each device visits | Pi-hole running in Docker sees container-network addresses, not real client IPs | Deferred per-website tracking; used per-app (DPI) tracking instead |

> 🔒 The dashboard repo is private — it contains real device names and addresses. This write-up describes the design only.

---

## 🌩️ Threat Forecast — Security Data as a Weather Report
A companion view to the dashboard that presents real firewall and IPS (Intrusion Prevention System) activity the way a TV weather segment would:
- **Radar "storm cells"** for top attack-origin countries
- **Auto-generated plain-language forecast** (e.g., how busy inbound scanning was and which ports were targeted)
- **Severity-coded ticker** of recent events

**Why:** a SOC analyst's job is partly *translating* security data for non-technical people. This was practice in turning raw counts into something anyone in the house can understand at a glance.

---

## 💻 macOS Menu-Bar Mods (Lid FX + KAI Wall)
Two small native macOS menu-bar apps, built with Claude instead of paying for existing apps:
- **Lid FX** — reads the MacBook's lid-angle sensor and animates the screen (fold + blur + fade) as the lid closes and opens
- **KAI Wall** — wallpaper manager for a multi-monitor desk

**Lesson learned:** the effect fell back to a plain black fade until the app had macOS **Screen Recording** permission — and every rebuild counts as a new app to macOS, so the permission has to be re-granted. A good reminder of how macOS privacy controls (TCC, Transparency Consent and Control) gate what apps can do.

---

## 🗓️ Crew Hub — Team Scheduling App
**Problem:** A rotating team of 5–10+ people per shift needed one place to see schedules, assignments, and shift info, and leads needed an easy way to confirm who had checked in.

**What I built:**
- A **Google Apps Script** web app backed by **Google Sheets** as the data source — no servers or extra licenses needed
- Schedule and info views tailored to the team
- **Slack check-in automation** — check-ins post to a channel so leads can see attendance at a glance

**Skills:** `Google Apps Script` · `JavaScript` · `Google Sheets` · `Slack integration` · `Requirements gathering from real users`

---

## 📟 Kai OS — Telegram Server Bot
**Problem:** I wanted to check on (and safely control) my home server without opening a laptop or exposing an admin panel to the internet.

**What I built:**
- A personal **Telegram bot** with commands such as `/status` (health check) and `/shutdown`
- Migrated from my Windows desktop to the 24/7 Mac mini so it's always reachable
- Telegram is also where my **Wazuh security alerts** and **Watchtower update reports** land, so my phone is one place for server status

**Skills:** `Telegram Bot API` · `Service migration` · `Notifications/alerting`

---

## 🧠 Agentic OS
**Goal:** A personal "operating system" where an AI agent has structured context about my day-to-day life — family, home lab, learning, and work — and can run repeatable tasks on a schedule.

**Design:**
- **Claude Code** as the agent, running on the home server
- An **Obsidian** vault as long-term, human-readable memory (Markdown notes)
- Reusable **skills** for recurring tasks and **scheduled automations** (e.g., daily briefs)
- Built in levels: give the agent context first, then skills, then scheduled automations

**Skills:** `Claude Code` · `Prompt / context engineering` · `Obsidian` · `Automation design`

---

## 🎙️ Jarvis Voice Assistant
**Goal:** A voice assistant on my Mac and my phone that stays in sync, using the home server as the central hub.

**Decisions so far:**
- Pay-as-you-go **API credits** instead of a subscription, to keep costs predictable and low
- Built my own version from scratch instead of buying a prompt pack — better for learning how the pieces fit
- Home server as the hub so both clients share the same state

**Skills:** `LLM APIs` · `Cost management` · `Client/server design`

---

## 💡 What I Learned
- **AI pair-programming works best when I own the design.** I decide the architecture and security boundaries; the AI speeds up the typing.
- **Treat bots like any other service:** least privilege, secrets out of code, logs and alerts on.
- **Automation is only worth it if people use it** — Crew Hub was designed around tools the team already used (Sheets + Slack).
