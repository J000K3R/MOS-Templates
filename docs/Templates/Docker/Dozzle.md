---
title: 🪵 Dozzle
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 8
---

# 🪵 Dozzle

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/dozzle.png" width="80" />

**Dozzle** is a small, lightweight application with a web-based interface for
monitoring Docker logs in real time. It doesn't store any log files — it's purely
for live monitoring of your container logs.

🏷️ **Category:** Productivity

🐳 **Image:** `amir20/dozzle:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/amir20/dozzle](https://github.com/amir20/dozzle) |
| 🐛 **Support** | [GitHub Issues](https://github.com/amir20/dozzle/issues) |
| 💛 **Donate** | [buymeacoffee.com/amirraminfar](https://buymeacoffee.com/amirraminfar) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Dozzle Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/var/run/docker.sock` | `/var/run/docker.sock` | RW | Docker Socket for log access |

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Dozzle**
2. Click **Install**
3. Access the Web UI at `http://your-server-ip:8080`
4. All running containers and their logs appear automatically

---

> 🔒 **Security Tip:** Dozzle has access to **all container logs** on your system
> via the Docker socket. Make sure it's not publicly accessible without authentication!

> 💡 **Tip:** Dozzle supports **multi-host monitoring** — you can connect multiple
> Docker hosts and monitor all their logs from a single Dozzle instance.
