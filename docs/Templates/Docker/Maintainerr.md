---
title: 📝 Maintainerr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 30
---

# 📝 Maintainerr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/maintainerr.png" width="80" />

**Maintainerr** is a smart media library management tool that keeps your **Plex**, **Jellyfin** or **Emby** library organized automatically. It removes duplicates, prunes old content and manages collections based on custom rules you define — so your server stays tidy without manual work.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/maintainerr/maintainerr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Maintainerr/Maintainerr](https://github.com/Maintainerr/Maintainerr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Maintainerr/Maintainerr/issues) |
| 📖 **Docs** | [docs.maintainerr.info](https://docs.maintainerr.info/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `6246` | TCP | Maintainerr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/maintainerr` | `/opt/data` | RW | Maintainerr configuration, database and logs. Must be writable by UID 1000:1000. |

---

## ⚙️ Environment Variables

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |
| `UI_PORT` | `6246` | ❌ | Web UI listen port. |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Maintainerr**
2. Adjust the volume path to your appdata location (e.g. `/mnt/cache/appdata/maintainerr`)
3. Click **Install**
4. Open the WebUI at `http://YOUR_SERVER_IP:6246`
5. Complete the initial setup:
   - Configure your media server (Plex / Jellyfin / Emby) connection
   - Set up rules for content you want to remove or collect
   - Review the **Warnings** tab before anything is deleted

---

## 🔗 Integration with Plex / Jellyfin / Emby

Maintainerr needs to connect to your media library manager to know what content exists:

1. In Maintainerr: Go to the setup screen after first login
2. Select your media server type (**Plex**, **Jellyfin** or **Emby**)
3. Enter your server URL and API key / token
4. Test the connection
5. Choose which libraries / collections Maintainerr should manage

---

## 🧹 Cleaning & Collections

Maintainerr works with **rules** that can either **remove** or **collect** content:

- **Removal rules:** Delete media when it meets conditions (e.g. watch status, age, duplicates)
- **Collection rules:** Automatically group media into collections based on metadata

> ⚠️ **Safety:** Always check the **Warnings** tab before applying a removal rule. Deleted content cannot be easily recovered.

---

## 📋 Leftover-Folder Cleanup

To enable the **leftover-folder cleanup** feature (removes orphaned folders after media is deleted), add a bind mount for your media root folder:

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/media` | `/data/media` | RW | Media root folder (same root as Radarr/Sonarr). |

**Important:** Use the container path that Radarr/Sonarr report as their root folder, and make sure `PUID`/`PGID` have write access there.

---

> ⚠️ **Note:** Maintainerr runs as UID 1000:1000 inside the container. Ensure your host appdata folder has matching ownership, otherwise the container may not write its config.

> 💡 **Tip:** Start with **collection rules** before any removal rules to get familiar with the interface.

> 💡 **Tip:** Use the **Warnings** tab regularly — it shows what Maintainerr is about to change before it acts.

> 📚 **For more information:** Visit the [Maintainerr Docs](https://docs.maintainerr.info/) for detailed rule configuration and media server setup guides.