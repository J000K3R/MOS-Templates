---
title: 📡 Prowlarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 2
---

# 📡 Prowlarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/prowlarr.png" width="80" />

**Prowlarr** is an indexer manager/proxy built on the popular arr .net/reactjs base stack to integrate with your various PVR apps. Prowlarr supports management of both Torrent Trackers and Usenet Indexers.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/prowlarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Prowlarr/Prowlarr](https://github.com/Prowlarr/Prowlarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Prowlarr/Prowlarr/issues) |
| 💛 **Donate** | [Open Collective](https://opencollective.com/prowlarr) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9696` | TCP | Prowlarr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/prowlarr` | `/config` | RW | Configuration files and database for Prowlarr. |

---

## ⚙️ Environment Variables

### 👤 User/Group Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions. |
| `PGID` | `500` | ❌ | Group ID for file permissions. |
| `UMASK` | `022` | ❌ | File creation mask for new files. |

---

> 📚 **For more information:** Visit the [Servarr Wiki](https://wiki.servarr.com/prowlarr) for indexer setup guides and *arr app integration details.
