---
title: 🏅 Sportarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 31
---

# 🏅 Sportarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/sportarr.png" width="80" />

**Sportarr** automatically downloads sports content for you. Follow your favorite **teams and athletes**, Sportarr matches new releases from your configured indexers and pushes them to your **download client** (qBittorrent, Deluge, Transmission, rTorrent, …) — including optional **IPTV DVR**.

🏷️ **Category:** Media / Automation

🐳 **Image:** `sportarr/sportarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Sportarr/Sportarr](https://github.com/Sportarr/Sportarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Sportarr/Sportarr/issues) |
| 📖 **Docs** | [wiki.sportarr.net](https://wiki.sportarr.net/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `1867` | TCP | Sportarr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/sportarr` | `/config` | RW | Sportarr configuration, database and settings. Host folder must be chowned to 99:100 to match the `--user 99:100` override. |
| `/mnt/cache/media/sportarr` | `/data` | RW | Media library root folder. Keep downloads under the same mount so imports can hardlink instead of copying. |

---

## ⚙️ Environment Variables

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |
| `PUID` | `99` | ❌ | User ID the container runs as. |
| `PGID` | `100` | ❌ | Group ID the container runs as. |
| `UMASK` | `022` | ❌ | File permissions mask for created files. |
| `UI_PORT` | `1867` | ❌ | Web UI listen port. |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Sportarr**
2. Adjust the volume paths to your appdata / media location
3. Ensure the folders are writable by UID 99:100 (`chown -R 99:100`)
4. Click **Install**
5. Open the WebUI at `http://YOUR_SERVER_IP:1867`
6. Complete the initial setup:
   - Add your indexer connection (**Prowlarr** / **autobrr**)
   - Add your download client (**qBittorrent**, Deluge, etc.)
   - Follow your favorite teams & athletes

---

## 🔌 Integrations

Sportarr connects to your **indexers** and **download clients**:

1. In Sportarr: go to **Settings → Integrations**
2. Add your indexer (**Prowlarr** recommended, or autobrr)
3. Add your download client (qBittorrent, Deluge, Transmission, rTorrent, SABnzbd, NZBGet, …)
4. Test the connection
5. Follow teams / athletes to start automatic grabs

Supported integrations: Aria2, autobrr, Bazarr, Decypharr, Deluge, Jellyfin, Kodi, Notifiarr, NZBGet, NZBdav, Plex, Prowlarr, qBittorrent, rTorrent, SABnzbd, Transmission, Unpackerr, Vuze and more.

---

## 📺 IPTV DVR (Alpha)

Sportarr includes an **experimental IPTV DVR** feature for recording TV channels:

- Configure an M3U playlist source in **Settings**
- Define recording schedules for your wanted events
- Recordings land in your media folder

> ⚠️ **Note:** IPTV DVR is in **alpha** status — expect rough edges and possible breaking changes.

---

## 🧪 Image Tags

| Tag | Purpose |
|---|---|
| `latest` | Stable releases (recommended) |
| `dev` | Rolling development builds |

---

> ⚠️ **Note:** Sportarr runs with `--user 99:100` inside the container. Ensure your host appdata / data folders have matching ownership, otherwise Sportarr may not write its config.

> 💡 **Tip:** Keep downloads and media on the **same mount** (`/data`) so Sportarr imports via **hardlink** instead of copying — saves double disk space.

> 📚 **For more information:** Visit the [Sportarr Wiki](https://wiki.sportarr.net/) for detailed setup, integrations and troubleshooting guides.

