---
title: 📡 Uptime Kuma
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 19
---

# 📡 Uptime Kuma

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/uptime-kuma.png" width="80" />

**Uptime Kuma** is an easy-to-use, self-hosted monitoring tool. Keep track of
the uptime and response times of all your services, websites and servers —
with beautiful status pages and instant notifications when something goes down.

🏷️ **Category:** Productivity

🐳 **Image:** `louislam/uptime-kuma:2`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/louislam/uptime-kuma](https://github.com/louislam/uptime-kuma) |
| 🐛 **Support** | [GitHub Issues](https://github.com/louislam/uptime-kuma/issues) |
| 💛 **Donate** | [github.com/sponsors/louislam](https://github.com/sponsors/louislam) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3001` | TCP | Uptime Kuma Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/uptime-kuma` | `/app/data` | RW | Database & config |

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Uptime Kuma**
2. Click **Install**
3. Access Uptime Kuma at `http://your-server-ip:3001`
4. Create your admin account on first launch
5. Add your first monitor and start tracking!

---

## 📊 Monitor Types

Uptime Kuma supports a wide range of monitor types:

| Type | Description |
|---|---|
| **HTTP/HTTPS** | Monitor websites and web services |
| **TCP Port** | Check if a port is open and responding |
| **Ping** | Simple ICMP ping monitoring |
| **DNS** | Monitor DNS record resolution |
| **Docker Container** | Monitor container status directly |
| **Push** | Receive heartbeats from your services |
| **Steam Game Server** | Monitor Steam game servers |

---

## 🔔 Notification Channels

Uptime Kuma supports 90+ notification providers including:

- 📧 **Email** (SMTP)
- 💬 **Telegram**, **Discord**, **Slack**
- 📱 **Pushover**, **Gotify**, **ntfy**
- 🔔 **PagerDuty**, **OpsGenie**
- And many more!

---

> 💡 **Tip:** Use the built-in **Status Page** feature to create a public-facing
> status page for your services — great for sharing uptime info with your users!

> ⚠️ **Note:** Uptime Kuma v2 has a new database format — do not downgrade to v1
> after upgrading, as this may cause data loss!
