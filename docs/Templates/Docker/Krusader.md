---
title: 🗂️ Krusader
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 14
---

# 🗂️ Krusader

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/krusader.png" width="80" />

**Krusader** is an advanced twin-panel (commander style) file manager for KDE Plasma
and other desktops — similar to Midnight Commander or Total Commander. It provides
all the file management features you could possibly want, accessible directly in
your browser via a web interface.

🏷️ **Category:** Productivity

🐳 **Image:** `ich777/krusader`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [krusader.org](https://krusader.org/) |
| 🐛 **Support** | [KDE Invent Issues](https://invent.kde.org/utilities/krusader/-/issues) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Krusader Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/krusader` | `/krusader` | RW | App config & data |
| `/mnt/cache` | `/mnt/cache` | RW | MOS filesystem access |

---

## ⚙️ Environment Variables

### 🖥️ Display

| Variable | Default | Masked | Description |
|---|---|---|---|
| `CUSTOM_RES_W` | `1280` | ❌ | Screen resolution width (px) |
| `CUSTOM_RES_H` | `1024` | ❌ | Screen resolution height (px) |
| `USER_LOCALES` | `en_US.UTF-8 UTF8` | ❌ | System locale & encoding |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `UID` | `500` | ❌ | User ID for file ownership |
| `GID` | `500` | ❌ | Group ID for file ownership |
| `UMASK` | `000` | ❌ | File creation mask |
| `RUNASROOT` | `false` | ❌ | Run as root (not recommended!) |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Krusader**
2. Adjust `UID` and `GID` to match your server user (run `id` in terminal to check)
3. Set your preferred resolution with `CUSTOM_RES_W` and `CUSTOM_RES_H`
4. Click **Install**
5. Access Krusader at `http://your-server-ip:8080`

---

> ⚠️ **Note:** Keep `RUNASROOT` set to `false` for security — running as root
> gives the container full write access to your entire filesystem!

> 💡 **Tip:** The `/mnt/cache` volume gives Krusader direct access to your MOS
> filesystem, so you can manage all your server files directly from the browser.
