---
title: 🖨️ Bambuddy
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 50
---

# 🖨️ Bambuddy

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/bambuddy.png" width="80" />

**Bambuddy** is a self-hosted command center for Bambu Lab 3D printers — from one A1 to an entire print farm. Manage print archives, monitor live camera feeds, schedule prints, slice STL/3MF files server-side, manage filament inventory with SpoolBuddy, and control virtual printers via proxy mode.

No cloud dependency. All your print data stays local on your server.

🏷️ **Category:** Utilities

🐳 **Image:** `ghcr.io/maziggy/bambuddy:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [github.com/maziggy/bambuddy](https://github.com/maziggy/bambuddy) |
| 📖 **Documentation** | [Bambuddy Docs](https://bambuddy.cool) |
| 🐛 **Support** | [GitHub Issues](https://github.com/maziggy/bambuddy/issues) |
| 💬 **Discord** | [Bambuddy Discord](https://discord.gg/aFS3ZfScHM) |
| 💛 **Donate** | [GitHub Sponsors](https://github.com/sponsors/maziggy) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8000` | TCP | Bambuddy web interface |

> ⚠️ **Note:** Bambuddy uses **bridge mode** by default in this template. For full functionality (printer auto-discovery via SSDP, virtual printers, proxy mode, FTP/RTSP camera streaming), consider switching to **host network mode**. In bridge mode, printers must be added manually by IP address.

> ℹ️ If you enable **Virtual Printers** or **Proxy Mode**, additional ports are required: `3000`, `3002`, `8883`, `990`, `6000`, `322`, `2024-2026`, and `50000-50029` (FTP passive). See the [Bambuddy docker-compose.yml](https://github.com/maziggy/bambuddy/blob/main/docker-compose.yml) for details.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/bambuddy` | `/app/data` | RW | Configuration, database, print archives, virtual printer certificates, backups |
| `/mnt/cache/appdata/bambuddy/logs` | `/app/logs` | RW | Application log files |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions (run `id -u` on host) |
| `PGID` | `500` | ❌ | Group ID for file permissions (run `id -g` on host) |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 📝 Application

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PORT` | `8000` | ❌ | Port Bambuddy runs on inside the container |
| `LOG_LEVEL` | `INFO` | ❌ | Logging level (DEBUG, INFO, WARNING, ERROR) |
| `LOG_TO_FILE` | `true` | ❌ | Enable file logging to `/app/logs/bambuddy.log` |

---

## 🔐 Capabilities

| Capability | Description |
|---|---|
| `NET_BIND_SERVICE` | Required for binding to privileged ports (322, 990) for FTPS and RTSPS camera proxy in virtual printer / proxy mode |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Bambuddy**
2. Review and optionally customize:
   - Set **PUID/PGID** to match your host user
   - Set **TZ** to your timezone
3. Click **Install**
4. Wait for the container to start

### Step 2: First Access

1. Access Bambuddy at `http://YOUR_SERVER_IP:8000`
2. Create your admin account on first launch

### Step 3: Add Your First Printer

1. Go to **Printers** → **Add Printer**
2. Enter your Bambu Lab printer's **IP address** and **Access Code**
   - The Access Code is found in your printer's settings: **Settings → Network → LAN Access Code**
3. Click **Add**

> 💡 In bridge mode, printer auto-discovery (SSDP) is not available. You must add printers manually by IP.

### Step 4: Enable Developer Mode (Required)

Bambuddy requires **Developer Mode** on your Bambu Lab printer for direct local control:

1. On your printer: **Settings → Network → LAN Mode → Enable**
2. Alternatively: **Settings → General → Device → Developer Mode → Enable**

---

## 📊 Key Features

### 📦 Print Archive
- Automatic 3MF archiving with metadata extraction
- 3D model preview (Three.js) directly in browser
- Duplicate detection & full-text search
- Photo attachments & failure analysis
- Timelapse editor (trim, speed, music) with automatic AVI-to-MP4 conversion
- Re-print to any connected printer with AMS mapping (auto-match or manual slot selection)
- Multi-plate support with plate thumbnail browsing
- Archive comparison (side-by-side diff)
- Per-archive print history with full print log

### 📊 Monitoring & Control
- Real-time printer status via WebSocket
- Live camera streaming (MJPEG) with multi-viewer support
- Cam Wall view — grid of camera tiles for at-a-glance monitoring
- Print progress in browser tab title
- Full automation — schedule prints, auto power-off, notifications

### 🏠 Multi-Printer / Print Farm
- Manage your entire print farm from one interface
- Multi-printer dispatch with AMS routing
- Nozzle-aware matching for dual-nozzle printers (H2D/H2D Pro)
- Filament Track Switch (FTS) support

### 🌐 Remote Printing with Proxy Mode
- Print from anywhere via secure proxy relay
- End-to-end TLS encryption
- Optional Tailscale integration
- No cloud dependency — direct connection through your Bambuddy server

### 🍰 Integrated Slicing
- Slice STL/3MF files server-side — no desktop slicer required
- One-click slicing from any browser (including mobile via PWA)
- Bring your own profiles — import Printer Preset Bundles (.bbscfg)
- Re-slice for a different printer in one click
- Multi-plate projects — "Slice all N plates" in one operation

### 🧩 Slicer Pipelines
- Save a recipe (printer + process + filament + bed type) and reuse in one click
- Multi-copy fanout across matching printers
- Runs dashboard with status badges and retry failed copies

### 🧶 SpoolBuddy (Filament Management)
- Track filament inventory with SpoolBuddy integration
- Monitor spool weights, materials, and colors
- AMS slot mapping

### 🔒 Security
- Multi-factor authentication (MFA) support
- OIDC / SSO integration (Keycloak, Authentik, etc.)
- Permission-gated slicer pipelines
- Admin token permissions with granular access control
- All data stored locally — no cloud dependency

---

## 💡 Tips

- **Host network mode** is recommended for full functionality (printer discovery, virtual printers, proxy mode). Switch from bridge to host in MOS container settings if needed.
- **Enable Developer Mode** on your Bambu Lab printer — Bambuddy uses local LAN access, not the Bambu cloud.
- **Back up regularly** — Bambuddy supports scheduled backups to external storage. Mount a NAS share to `/app/data/backups` for automatic off-site backups.
- **Tailscale integration** — mount the host's Tailscale socket (`/var/run/tailscale/tailscaled.sock`) to enable Let's Encrypt certs for virtual printers over your tailnet.
- **External PostgreSQL** — Bambuddy uses SQLite by default. For larger print farms, configure an external PostgreSQL database via the `DATABASE_URL` environment variable.
- **Slicer API sidecar** — deploy the OrcaSlicer API sidecar container alongside Bambuddy to enable the integrated slicing feature.

> 💡 **Tip:** After initial setup, immediately configure MFA and change the default admin password to secure your print farm data.

> 💡 **Tip:** If you have multiple Bambu Lab printers, use the **Cam Wall** view for at-a-glance monitoring of all cameras simultaneously.

> 📚 **For more information:** Visit the [Bambuddy Documentation](https://bambuddy.cool) and the [GitHub README](https://github.com/maziggy/bambuddy) for detailed guides on features, proxy mode, slicer integration, and configuration.
