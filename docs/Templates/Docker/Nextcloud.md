---
title: ☁️ Nextcloud
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 16
---

# ☁️ Nextcloud

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/nextcloud.png" width="80" />

**Nextcloud** gives you access to all your files wherever you are. Pick your own
server, protect your data and access it from desktop or mobile devices. Sync and
share files from FTP drives, Dropbox, NAS and more — your data stays yours.

🏷️ **Category:** Cloud

🐳 **Image:** `lscr.io/linuxserver/nextcloud:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/linuxserver/docker-nextcloud](https://github.com/linuxserver/docker-nextcloud) |
| 🐛 **Support** | [GitHub Issues](https://github.com/linuxserver/docker-nextcloud/issues) |
| 💛 **Donate** | [opencollective.com/linuxserver](https://opencollective.com/linuxserver#category-CONTRIBUTE) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `443` | TCP | Nextcloud Web Interface (HTTPS) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/nextcloud` | `/config` | RW | Config, database & web files |
| `/mnt/cache/appdata/nextcloud_data` | `/data` | RW | User files & uploaded data |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `1000` | ❌ | User ID for file permissions |
| `PGID` | `1000` | ❌ | Group ID for file permissions |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Nextcloud**
2. Adjust `PUID` and `PGID` to match your server user (run `id` in terminal to check)
3. Click **Install**
4. Access Nextcloud at `https://your-server-ip:443`
5. Complete the setup wizard — choose your admin credentials and database

---

## 🧩 Recommended Apps

After setup, install these apps from the Nextcloud App Store for the best experience:

| App | Description |
|---|---|
| **Collabora Online** | Edit documents, spreadsheets & presentations in browser |
| **Talk** | Video calls & chat |
| **Calendar** | Sync calendars via CalDAV |
| **Contacts** | Sync contacts via CardDAV |
| **Notes** | Simple markdown notes |
| **Memories** | Google Photos-like photo management |

---

> 💡 **Tip:** Pair Nextcloud with **Collabora CODE** for a complete self-hosted
> Google Workspace alternative — edit all your documents directly in the browser!

> ⚠️ **Note:** Nextcloud runs on port `443` (HTTPS) by default. Make sure no other
> service is using this port, or change it to something like `8443` if needed.
