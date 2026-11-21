---
id: "twitch-tournament-maker"
title: "Twitch Tournament Maker"
tagline: "Interactive OBS broadcast overlay for live Twitch chat King of the Hill & single-elimination bracket battles with real-time charts and IndexedDB storage."
category: "tool"
status: "production"
featured: false
badge: "OBS Studio Tool"
problemStatement: "Running community head-to-head battles or knockout tournaments on Twitch usually requires external websites with ads, mismatched aspect ratios, and manual vote counting. Furthermore, storing dozens of custom contestant images in localStorage quickly hits the 5 MB browser quota limit."
technicalSolution: "A self-contained single-file HTML5 broadcast application designed directly for OBS Studio Browser Sources, connecting to Twitch IRC over WebSockets with live Chart.js vote visualization. Supports both King of the Hill and Single-Elimination Tournament Bracket modes, compresses uploaded images to WebP in IndexedDB, and provides bilingual UI, adjustable background opacity, and offline JSON backups."
techStack:
  - "HTML5"
  - "CSS3"
  - "JavaScript"
  - "Chart.js"
  - "IndexedDB"
  - "Twitch IRC WebSocket"
architectureHighlights:
  - "Zero-dependency single-file architecture tailored for OBS Studio Browser Source and Interact window workflows"
  - "Dual game modes: King of the Hill (waiting/eliminated queues) and Single-Elimination Tournament Bracket with automatic bye balancing"
  - "IndexedDB image store with synchronous in-memory cache and automatic localStorage migration"
  - "Twitch IRC WebSocket client with IRCv3 tags for broadcaster/moderator chat commands and silent exponential-backoff reconnect"
  - "Scoped 1400x1032 overlays keeping the 520x520 OBS chat area unobstructed during pause, result, and champion screens"
screenshots:
  - "/screenshots/twitch-tournament-maker/01-overview.png"
  - "/screenshots/twitch-tournament-maker/02-feature.png"
  - "/screenshots/twitch-tournament-maker/03-detail.png"
links:
  chromeWebStore: ""
  demoUrl: ""
  githubUrl: "https://github.com/Mello2110/twitch-tournament-maker"
sandbox:
  enabled: false
  type: "none"
  embedUrl: ""
  notes: "Designed as a local 1920x1080 OBS Studio Browser Source overlay with Twitch IRC WebSocket integration."
stats:
  version: "v2.0.0"
---

# Twitch Tournament Maker

[![Status](https://img.shields.io/badge/Status-Active-emerald?style=flat-square)](#)
[![Version](https://img.shields.io/badge/Version-v2.0.0-blue?style=flat-square)](#)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](#)

> Interactive OBS broadcast overlay for live Twitch chat King of the Hill & single-elimination bracket battles with real-time charts and IndexedDB storage.

<p align="left">
  <a href="https://github.com/Mello2110"><b>Mellow Solutions on GitHub</b></a> &nbsp;|&nbsp;
  <a href="https://buymeacoffee.com/mellowsolutions"><b>Buy Me a Coffee</b></a>
</p>

---

## Problem & Motivation
Running community head-to-head battles or knockout tournaments on Twitch usually requires screen-capturing third-party websites filled with ads, awkward aspect ratios, and no native chat voting. Streamers need a clean, reliable `1920×1080` OBS Browser Source where viewers can vote directly via `!1` and `!2` while keeping a dedicated `520×520` space unobstructed for an on-stream Twitch chat overlay.

## Technical Solution
**Twitch Tournament Maker** (`BattleProSystem.html`) is a zero-build, single-file broadcast tool built for OBS Studio:
- **Dual Battle Modes:**
  - **King of the Hill (`koth`):** Winner stays in the arena while challengers rotate from the waiting queue until the last contender stands.
  - **Tournament Bracket (`tournament`):** Bottom-to-top single-elimination bracket tree with automatic bye distribution (max 25% byes) and champion laurel frame.
- **Live Twitch Chat Voting:** Connects directly to `wss://irc-ws.chat.twitch.tv:443` with `CAP REQ :twitch.tv/tags`, supporting single-vote correction, toggleable **Multi-Vote** mode, optional trailing text after commands (`!1 easy win`), and broadcaster/moderator commands (`!battle start`, `!battle pause`, `!battle next`, `!battle skip`, `!battle reset`).
- **Unlimited Local Image Storage:** Uploaded images are center-cropped, WebP-encoded, and stored in **IndexedDB** (`BattleProImages`) with an in-memory render cache and automatic migration from legacy `localStorage` base64 entries.
- **Broadcast-Ready Overlays & Themes:** Features an Antique Gold / Vintage Crimson accent system with presets, adjustable background opacity (`0–100%`) for transparent OBS scenes, bilingual UI (`English` / `Deutsch`), and scoped pause/result/champion overlays that never cover the `520×520` chat area.

## Architecture & Tech Stack
- **Languages & Libraries:** Vanilla HTML5, CSS3 (custom properties & 1920×1080 broadcast grid), ES2022 JavaScript, Chart.js 4.4 (UMD).
- **Core Architecture:**
  - **Storage Layer:** `IndexedDB` for binary/WebP contestant thumbnails + `localStorage` (`battleConfig`, `battleState`, `battleCredentials`) with automatic schema migration.
  - **Network Layer:** Stateful Twitch IRC WebSocket client with `PING`/`PONG` heartbeat, silent exponential-backoff reconnect, and auth-error handling.
  - **UI & Controls:** Tabbed OBS-friendly settings drawer (*Twitch & Setup*, *Items*, *Mode & Voting*, *Design & OBS*, *Backup*), hotkeys (`1`, `2`, `N`, `Space`, `Esc`), and two-step inline confirmation for destructive actions.

## Installation & Usage
1. Download or clone the repository and locate `BattleProSystem.html`.
2. In **OBS Studio**, add a new **Browser** source, enable **Local file**, and select `BattleProSystem.html`.
3. Set the Browser Source resolution to **Width: `1920`**, **Height: `1080`**.
4. Right-click the source in OBS and open **Interact** → click the **Settings** gear icon:
   - **Twitch & Setup:** Enter your Twitch channel name and OAuth token (`chat:read` scope) and click **Save & Connect**.
   - **Items:** Drag & drop contestant images, rename them inline, and click **Shuffle & Start**.
   - **Mode & Voting / Design & OBS:** Switch between **King of the Hill** and **Tournament Bracket**, configure voting criteria, language (`EN`/`DE`), accent color, and background opacity.
5. Click **Start Voting** (or use hotkeys `1` Left, `2` Right, `N` Next, `Space` Pause, `Esc` Close Menu) and let your chat vote with `!1` and `!2`.

## License & Disclaimer
Part of the **Mellow Solutions** portfolio. Provided under the MIT License as-is for non-commercial and personal utility.
