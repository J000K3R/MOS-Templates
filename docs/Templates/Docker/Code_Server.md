---
title: 💻 Code Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 7
---

# 💻 Code Server

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/visual-studio-code.png" width="80" />

**Code Server** runs **Visual Studio Code** in the browser — giving you a full
development environment accessible from any device. It combines the simplicity
of a code editor with comprehensive editing, navigation, debugging and extensibility
features.

🏷️ **Category:** Productivity

🐳 **Image:** `lscr.io/linuxserver/code-server`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/microsoft/vscode](https://github.com/microsoft/vscode) |
| 🐛 **Support** | [GitHub Issues](https://github.com/microsoft/vscode/issues) |
| 🌐 **Website** | [code.visualstudio.com](https://code.visualstudio.com/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8443` | TCP | VS Code Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/code-server/config` | `/config` | RW | Config & workspace files |

---

## ⚙️ Environment Variables

### 🔑 Authentication

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PASSWORD` | `` | ✅ | Password for web interface login |
| `HASHED_PASSWORD` | `` | ✅ | Pre-hashed password (alternative to PASSWORD) |
| `SUDO_PASSWORD` | `` | ✅ | Password for sudo commands inside container |
| `SUDO_PASSWORD_HASH` | `` | ✅ | Pre-hashed sudo password (alternative to SUDO_PASSWORD) |

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PROXY_DOMAIN` | `code-server.my.domain` | ❌ | Domain for reverse proxy |
| `DEFAULT_WORKSPACE` | `/config/workspace` | ❌ | Default workspace folder on startup |
| `PWA_APPNAME` | `code-server` | ❌ | Progressive Web App name in browser |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `UMASK` | `022` | ❌ | File creation mask |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Code Server**
2. Set a secure `PASSWORD`
3. Set `SUDO_PASSWORD` if you need terminal access inside the container
4. Adjust `PUID` and `PGID` to match your server user (run `id` in terminal to check)
5. Click **Install**
6. Access VS Code at `http://your-server-ip:8443`

---

> 🔒 **Security Tip:** Always set a strong `PASSWORD` — Code Server gives full
> terminal access to your server from the browser!

> 💡 **Tip:** Use `HASHED_PASSWORD` instead of `PASSWORD` for better security.
> Generate a hash with:
> ```bash
> echo -n "yourpassword" | npx argon2-cli -e
> ```
