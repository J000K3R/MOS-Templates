---
title: 🎵 Lidarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 11
---

# 🎵 Lidarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/lidarr.png" width="80" />

**Lidarr** is a music collection manager for Usenet and BitTorrent users. It can monitor multiple RSS feeds for new albums from your favorite artists and will grab, sort and rename them.

It is part of the *arr stack and integrates with **Prowlarr** for indexer management and download clients like **qBittorrent** or **SABnzbd**.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/lidarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Lidarr/Lidarr](https://github.com/Lidarr/Lidarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Lidarr/Lidarr/issues) |
| 💛 **Donate** | [Open Collective](https://opencollective.com/lidarr) |
| 📖 **Wiki** | [wiki.servarr.com/lidarr](https://wiki.servarr.com/lidarr) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8686` | TCP | Lidarr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/lidarr` | `/config` | RW | Configuration files and database for Lidarr. |
| `/mnt/cache/media/music` | `/music` | RW | Location of your music library. |
| `/mnt/cache/downloads` | `/downloads` | RW | Download folder for completed downloads from your download client. |

---

## ⚙️ Environment Variables

### 👤 User/Group Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions. |
| `PGID` | `500` | ❌ | Group ID for file permissions. |
| `UMASK` | `022` | ❌ | File creation mask for new files. |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Lidarr**
2. Adjust the volume paths to match your library structure
3. Ensure `PUID`/`PGID` match your media download client
4. Click **Install**
5. Open the WebUI at `http://YOUR_SERVER_IP:8686`
6. Complete the initial setup:
   - Add your download client (qBittorrent, SABnzbd, etc.)
   - Configure Prowlarr for indexer management (Settings → Indexers)
   - Add your music library path (`/music`)
   - Add artists and albums to monitor

---

## 🔗 Integration with Prowlarr

Lidarr uses **Prowlarr** for indexer management. To connect:

1. In Prowlarr: Go to **Settings → Apps → Add** → Select **Lidarr**
2. Enter your Lidarr URL: `http://YOUR_SERVER_IP:8686`
3. Enter your Lidarr API key (found in Lidarr: Settings → General)
4. Test and Save

---

## 🎵 Supported Audio Formats

Lidarr supports importing and managing:
- **Lossy:** MP3, AAC, OGG, WMA
- **Lossless:** FLAC, ALAC, WAV, AIFF

---

> ⚠️ **Note:** Ensure your download client and Lidarr use the same `PUID`/`PGID` values to avoid permission issues when moving completed downloads to your music library.

> 💡 **Tip:** Use the **Artist Folders** option in Settings → Media Management to keep your library organized with artist/album folder structure.

> 💡 **Tip:** For automatic album monitoring, use the **Monitor** dropdown when adding artists: `None`, `New`, `Existing`, or `All Albums`.

> 📚 **For more information:** Visit the [Servarr Wiki](https://wiki.servarr.com/lidarr) for detailed configuration guides, quality profiles, and troubleshooting.
