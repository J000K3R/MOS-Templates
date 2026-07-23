---
title: 🎬 Seerr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 39
---

# 🎬 Seerr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/jellyseerr.png" width="80" />

**Seerr** is a free and open source application for managing media requests for
your library. It integrates with **Jellyfin**, **Plex** and **Emby**, as well as
your existing services like **Sonarr** and **Radarr** — making it easy for users
to request movies and TV shows.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/seerr-team/seerr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/seerr-team/seerr](https://github.com/seerr-team/seerr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/seerr-team/seerr/issues) |
| 💛 **Donate** | [opencollective.com/seerr](https://opencollective.com/seerr) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `5055` | TCP | Seerr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/seerr` | `/app/config/` | RW | Config & database files |

---

## ⚙️ Environment Variables

### 🎞️ Media Server

| Variable | Default | Masked | Description |
|---|---|---|---|
| `LOG_LEVEL` | `debug` | ❌ | Log verbosity (`debug`, `info`, `warn`, `error`) |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Seerr**
2. Set `SEERR_TYPE` to your media server (`jellyfin`, `plex` or `emby`)
3. Adjust `PUID` and `PGID` to match your server user (run `id` in terminal)
4. Click **Install**
5. Access Seerr at `http://your-server-ip:5055`
6. Complete the setup wizard and connect to your media server

---

## 🔗 Integrations

Seerr works seamlessly with the following services:

| Service | Type | Description |
|---|---|---|
| **Jellyfin** | Media Server | Open source media server |
| **Plex** | Media Server | Popular media server |
| **Emby** | Media Server | Personal media server |
| **Sonarr** | TV Shows | Automatic TV show downloader |
| **Radarr** | Movies | Automatic movie downloader |

---

> 💡 **Tip:** Set `SEERR_TYPE` correctly before first launch — changing it later
> requires a fresh setup!

> ⚠️ **Note:** Make sure your media server (Jellyfin/Plex/Emby) is running and
> accessible before completing the Seerr setup wizard.

> 📚 **For more information:** Visit the [Seerr documentation](https://github.com/seerr-team/seerr/blob/main/README.md)
> for additional integration options and notification settings.
