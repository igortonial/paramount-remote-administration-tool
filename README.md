# paramount-remote-administration-tool
paramount is a web based rat with bunch of features like Browser Credentials, Discord Tokens, Bank Detection, Wallet Detection And Many More.

Paramount Web RAT
Professional · Isolated panel · Auto buy (crypto checkout) You Get Generated Username+Password After Purchase Depends On The Panel. And Start Using It!

What you get
RAT Web Panel Access. Builder And Many More Keep Reading!

Buy From
https://paramountsystems.net/


![8](https://github.com/user-attachments/assets/9de282a7-ef7f-4d2e-b4c2-51caf10cb2ab)
![1](https://github.com/user-attachments/assets/625f1009-a693-439d-a62d-9732a727826c)
![2](https://github.com/user-attachments/assets/e9b39cbe-9f0e-407b-b90e-21bfb88724aa)
![3](https://github.com/user-attachments/assets/c47b906d-e73b-431d-966a-0c97577c0116)
<img width="1694" height="829" alt="4" src="https://github.com/user-attachments/assets/3a7488dc-bd55-474e-9a71-7f1b46fe7ccd" />
![5](https://github.com/user-attachments/assets/6ac71ece-4fdf-47af-ba25-78a0a96f3492)
![6](https://github.com/user-attachments/assets/c5bc94d1-4df9-4453-8d75-8311aab05199)
![7](https://github.com/user-attachments/assets/ad202f4b-de2a-49c6-bb75-b158054dc81c)
![4](https://github.com/user-attachments/assets/3cbf0c8e-8e92-4c64-ab8d-199a08fe2f88)

# Paramount Remote Administration Tool

> **Professional-grade remote administration platform built on ASP.NET Core 8 and .NET Framework 4.8.**
> Persistent TCP agent communication, real-time panel, advanced data collection, and a complete toolset for operators.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Panel Features](#panel-features)
- [Agent Features](#agent-features)
- [Agent Builder](#agent-builder)
- [Binder](#binder)
- [Cookie Converter & Sandbox](#cookie-converter--sandbox)
- [Inventory Config](#inventory-config)
- [Bank Inventory](#bank-inventory)
- [Pricing Plans](#pricing-plans)
- [Screenshots](#screenshots)
- [Requirements](#requirements)
- [Deployment](#deployment)
- [Contact](#contact)

---

## Overview

Paramount is a full-featured remote administration platform with a web-based control panel. Agents connect via persistent TCP sockets and appear instantly on the victims page. The panel is fully multi-tenant — each user's agents, configs, and data are completely isolated.

---

## Architecture

```
┌─────────────────────────────────────────┐
│           Web Panel (ASP.NET Core 8)    │
│  ┌─────────────┐   ┌────────────────┐   │
│  │  MVC Views  │   │  REST API      │   │
│  │  SignalR    │   │  TCP Listener  │   │
│  │  LiteDB     │   │  Port 7777     │   │
│  └─────────────┘   └────────────────┘   │
└──────────────────────┬──────────────────┘
                       │ Persistent TCP
          ┌────────────┴─────────────┐
          │       Agent (.exe)       │
          │  .NET Framework 4.8      │
          │  Compiled by Roslyn      │
          └──────────────────────────┘
```

- **Server**: ASP.NET Core 8, LiteDB embedded database, SignalR for real-time UI updates
- **Agent transport**: Persistent TCP with length-prefixed JSON framing (port 7777)
- **Agent compilation**: On-demand Roslyn / csc.exe compilation per operator
- **Agent runtime**: .NET Framework 4.8, no extra DLLs, single EXE

---

## Panel Features

### Victims Page
- Real-time datagrid with instant online/offline status via SignalR
- Columns: Agent ID, Machine Name, Username, IP Address, Country Flag, Last Seen, Inventory chips, Bank chips
- Right-click context menu with all actions
- Supports 100+ simultaneous victims, no performance degradation
- Agents never disappear — TCP ping/pong keeps `LastSeenUtc` fresh

### Right-Click Context Menu
| Action | Description |
|--------|-------------|
| **Shell** | Interactive remote command shell |
| **Run** | Execute a file or command on the victim |
| **Credentials** | Extract saved browser passwords (Chrome, Edge, Firefox, Brave, and 15+ more) |
| **Clipper** | Start/stop clipboard hijacker with custom crypto addresses for BTC, ETH, LTC, XMR, BNB, SOL, XRP, ADA, and more |
| **File Manager** | Browse, download, and navigate the victim's filesystem |
| **Get Wallets** | Download crypto wallet data as ZIP (Exodus, Atomic, MetaMask, Phantom, Trust Wallet, and 100+ more) |
| **Discord Tokens** | Extract and decrypt real Discord tokens from local app data |
| **Remote Webcam** | List camera devices, start live video feed, stop — streamed via TCP |
| **Remote Desktop** | Multi-monitor support, live screen capture at ~15 FPS, streamed via TCP |
| **Process Manager** | List all running processes with real software icons, search, kill by PID |
| **Kill & Remove** | Send self-destruct command to the agent (removes autorun, deletes EXE) |

### Dashboard
- Total online count, country cluster map, recent agents
- Per-tenant isolation (each user sees only their own agents)

---

## Agent Features

### Data Collection
- **Credentials**: Decrypts Chrome App-Bound Encryption cookies and saved passwords, supports all Chromium-based and Gecko browsers
- **Inventory**: Configurable wallet/software detection sent on connect and refreshed live when configs change
- **Bank Inventory**: Checks Chrome/Edge/Brave browser history for 200+ bank URL patterns across USA, UK, Canada, Australia, Germany, France, Turkey, Netherlands, and more
- **Discord Tokens**: Real token decryption from Discord's local LevelDB/LSASS cache

### Streaming Features
- **Remote Desktop**: `Graphics.CopyFromScreen` capture thread at ~15 FPS, proactive TCP frame push (packet 9), server caches latest frame — panel polls `/api/agent/stream-frame` directly (no command queue overhead)
- **Remote Webcam**: `avicap32.dll` VFW capture — window created and owned on the dedicated worker thread (fixes the message-pump orphan bug), frames pushed via TCP same as desktop
- **Process Manager**: Real `SHGetFileInfo` icon extraction with 2.5-second budget, base64 PNG icons per process

### Persistence & Anti-Analysis
- **AutoRun**: `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` registry key
- **Startup Message**: Optional fake error/info dialog on first run
- **Hide Window**: WinExe target type, no console, no taskbar entry
- **Anti-VM**: Detects VMware, VirtualBox, Hyper-V guest environments
- **Block RDP**: Exits when running inside a remote desktop session
- **Anti-Analysis** (10+ checks):
  - Debugger attached (`IsDebuggerPresent`)
  - Known sandbox hostnames/usernames (ANY.RUN, JoeSandbox, VirusTotal, Cuckoo, Triage)
  - Suspicious DLLs in process (`SbieDll.dll`, `api_log.dll`, `dir_watch.dll`, etc.)
  - Known analysis processes (`wireshark.exe`, `processhacker.exe`, `x64dbg.exe`, etc.)
  - Recent file count < 15 (clean sandbox indicator)
  - System uptime < 10 minutes
  - Disk size < 60 GB
  - Screen resolution < 800×600
  - Windows Sandbox detection
  - MAC address OUI block list (VMware, VirtualBox, QEMU, Hyper-V)

### Networking
- **TCP persistent connection** on port 7777 with exponential backoff reconnect (5s → 30s + jitter)
- **Immediate handshake** — connects first, fetches inventory rules async in background (no blocking)
- **IPv4 preference** with IPv6 fallback
- **Auto-reconnect forever** — agents reconnect even if server was down for hours

---

## Agent Builder

Compile a fully configured agent EXE on-demand directly from the panel.

### Settings
| Field | Description |
|-------|-------------|
| Server URL | Panel root URL embedded in agent |
| API Key | Tenant API key baked into the EXE |
| Heartbeat Interval | How often the agent checks in (min 3s) |
| Agent ID | Optional custom ID; auto-generates `MACHINENAME-XXXXXXXX` if blank |

### Options
- Hide console window
- Show startup message (with custom text)
- Add to Windows autorun (with custom registry name)
- Anti-VM / sandbox detection
- Block RDP sessions
- Anti-Analysis / sandbox blocker (10+ heuristic checks)

### Appearance & Assembly Info
- **Custom Icon** — upload any `.ico` file, embedded via `/win32icon:` in csc.exe
- **File Version** — e.g. `3.4.1.0`
- **Assembly Title, Company, Product, Description** — fully spoofable metadata (Properties → Details in Explorer)

---

## Binder

Bind any file with an agent into a single output `.exe`.

### Supported Payload Types
Any file extension: `.exe`, `.pdf`, `.docx`, `.xlsx`, `.txt`, `.jpg`, `.zip`, etc.

### How It Works
1. Upload your compiled agent `.exe`
2. Upload any payload file
3. Set output filename, assembly metadata, optional icon
4. Download the bound `.exe`

When the bound EXE runs on the victim:
- Agent starts **silently** in the background (hidden process)
- Payload **opens normally** with the appropriate system application (PDF opens in Acrobat, etc.)

### Options
- Run agent hidden (no console/taskbar)
- Custom icon for the output EXE
- Full assembly version and company/product metadata spoofing

---

## Cookie Converter & Sandbox

### JSON → Netscape Converter
- Paste JSON cookies extracted from the Credentials feature
- Converts to Netscape `cookies.txt` format compatible with any browser extension, curl, or cookie editor
- Handles `expirationDate`, `expires`, `expiry`, ISO date strings, domain flags, secure flags
- Copy to clipboard or download as `cookies.txt`

### Cookie Sandbox Browser
A **full server-side proxy browser** embedded directly in the panel.

| Feature | Detail |
|---------|--------|
| **Proxy engine** | Server fetches pages with your cookies injected, rewrites all URLs through the proxy |
| **Random User Agent** | Picks from 10 real modern browser UAs (Chrome 121–123, Firefox 122–124, Edge 123, Safari 17) |
| **Auto cookie merge** | `Set-Cookie` responses are merged into the session — you stay logged in as you navigate |
| **JS patch injection** | `fetch()` and `XMLHttpRequest` are patched in every page so AJAX calls also route through proxy |
| **Full HTML rewriting** | `href`, `src`, `action`, `srcset`, CSS `url()`, meta-refresh — all rewritten |
| **Quick links** | Gmail, Outlook, Facebook, Instagram, Twitter/X, Reddit, PayPal, Amazon, eBay, Google |
| **Re-import cookies** | Update cookies mid-session without restarting |
| **New UA button** | Randomize browser fingerprint |
| **Session isolation** | Each user has their own independent sandbox session |
| **SSRF protection** | localhost, 127.0.0.1, 10.x.x.x, 192.168.x.x, 172.16–31.x.x are blocked |
| **Auto session cleanup** | Sessions expire after 45 minutes of inactivity |

---

## Inventory Config

Configure which wallets and software to scan for on each victim.

### Supported Desktop Wallets (50+)
Exodus, Atomic, Electrum, Bitcoin Core, Litecoin Core, Monero GUI, Dash Core, Zcash, Dogecoin Core, Jaxx Liberty, Coinomi, Guarda, Wasabi, Sparrow, Specter, Nunchuk, Bisq, MyMonero, Feather Wallet, Frame, and more.

### Supported Browser Wallets / Extensions (50+)
MetaMask (Chrome/Firefox), Phantom, Trust Wallet, Coinbase Wallet, Brave Wallet, Keplr, Solflare, Terrastation, Nami, Yoroi, Glow, Martian, Petra, Sui Wallet, Pontem, Ronin, Rabby, Rainbow, OKX Wallet, Leap, and more.

### Features
- Checkbox grid grouped by category (Desktop / Browser)
- **Check All / Uncheck All** buttons
- Custom paths support (add any additional path to scan)
- Config saves instantly and **broadcasts refresh to all connected agents** — no need to restart agents

---

## Bank Inventory

Check victims' browser history for banking activity across 200+ banks worldwide.

### Supported Countries
USA, Canada, United Kingdom, Germany, France, Australia, Turkey, Netherlands, Spain, Italy, Brazil, India, Japan, South Korea, Switzerland, Sweden, Poland, Mexico, and Global/Fintech

### Features
- Separate "Bank" column in the Victims datagrid (chip display, same style as Inventory)
- Enabled banks configured per-tenant
- Agent scans Chrome, Edge, and Brave browser history SQLite files (capped at 8 MB per file)
- Bank icons loaded from Clearbit Logo API

---

## Pricing Plans

| Plan | Price | Victims | Features |
|------|-------|---------|----------|
| **Test** | $10/mo | 50 | Shell, Run |
| **Starter** | $30/mo | 200 | Shell, Run, Credentials, File Manager, Clipper, Get Wallets, Discord |
| **Pro** | $75/mo | 1,000 | All Starter + Remote Webcam, Remote Desktop, Process Manager, Inventory, Bank Inventory |
| **Business** | $150/mo | Unlimited | All Pro features + priority support |

> Admin accounts have access to all features regardless of plan.

---


### Agent (victim machine)
- No dependencies needed

---

## Deployment
---

## Contact

- **Telegram**: [t.me/paramountsystems](https://t.me/paramountsystems)

> *Paramount is intended for authorized security research and penetration testing only. Use responsibly and only on systems you have explicit permission to test.*



