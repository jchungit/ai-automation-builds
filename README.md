# 🤖 AI & Automation Builds

A collection of the AI-assisted tools and automations I've been building — some for my home lab, some for my day-to-day work. Most of these were built hands-on with **Claude Code** as a pair-programmer, running on my always-on home server.

> 🔒 Bot tokens, API keys, internal company data, and personal information are intentionally left out. Work-related tools are described at a high level only.

---

## 📋 Projects

| Project | What it does | Stack | Status |
|---|---|---|---|
| 🗓️ **Crew Hub** | Scheduling & info app for a field data-collection team | Google Apps Script, Google Sheets, Slack | ✅ Built |
| 📟 **Kai OS Telegram Bot** | Control and check my home server from my phone | Telegram Bot API, Claude Code | ✅ Running |
| 🧠 **Agentic OS** | Personal AI "operating system" — notes, skills, scheduled automations | Claude Code, Obsidian, cron-style schedules | 🚧 Building |
| 🎙️ **Jarvis Voice Assistant** | Voice assistant with Mac + phone clients synced through the home server | Claude API (pay-as-you-go), Claude Code | 🚧 Building |

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
