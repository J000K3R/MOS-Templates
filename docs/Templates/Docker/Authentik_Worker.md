---
title: ⚙️ Authentik Worker
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 3
---

# ⚙️ Authentik Worker

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/authentik.png" width="80" />

**authentik Worker** is the background task processor for authentik. It handles
certificate management, backups, outpost deployments, and all asynchronous tasks
required by the authentik platform.

🏷️ **Category:** Web

🐳 **Image:** `ghcr.io/goauthentik/server:2025.12.4`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/goauthentik/authentik](https://github.com/goauthentik/authentik) |
| 🐛 **Support** | [GitHub Issues](https://github.com/goauthentik/authentik/issues) |
| 💛 **Pricing / Donate** | [goauthentik.io/pricing](https://goauthentik.io/pricing/) |

---

## 🌐 Ports

No ports exposed — the Worker runs as a background service only.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/authentik/backups` | `/backups` | RW | Backup Storage |
| `/mnt/cache/appdata/authentik/media` | `/media` | RW | Media Files |
| `/mnt/cache/appdata/authentik/certs` | `/certs` | RW | Certificates |
| `/mnt/cache/appdata/authentik/templates` | `/templates` | RW | Custom Templates |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RW | Docker Socket |

---

## ⚙️ Environment Variables

### 🗄️ PostgreSQL

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AUTHENTIK_POSTGRESQL__HOST` | `postgresql` | ❌ | PostgreSQL Host |
| `AUTHENTIK_POSTGRESQL__USER` | `authentik` | ❌ | PostgreSQL User |
| `AUTHENTIK_POSTGRESQL__NAME` | `authentik` | ❌ | PostgreSQL DB Name |
| `AUTHENTIK_POSTGRESQL__PASSWORD` | `` | ✅ | PostgreSQL Password |

### 🔑 Application

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AUTHENTIK_SECRET_KEY` | `` | ✅ | App Secret Key |
| `AUTHENTIK_ERROR_REPORTING__ENABLED` | `true` | ❌ | Enable Error Reporting |

### 📧 Email

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AUTHENTIK_EMAIL__HOST` | `mail.domain.com` | ❌ | SMTP Host |
| `AUTHENTIK_EMAIL__PORT` | `587` | ❌ | SMTP Port |
| `AUTHENTIK_EMAIL__USERNAME` | `mail@domain.com` | ❌ | SMTP Username |
| `AUTHENTIK_EMAIL__PASSWORD` | `` | ✅ | SMTP Password |
| `AUTHENTIK_EMAIL__USE_TLS` | `true` | ❌ | Use TLS |
| `AUTHENTIK_EMAIL__FROM` | `mail@domain.com` | ❌ | Sender Address |

---

## 🚀 Quick Start

1. Open the **MOS Hub**
2. Search for **Authentik Worker**
3. Make sure **Authentik Server** is already running
4. Use the **exact same** environment variable values as the Server!
5. Click **Install**

---

> ⚠️ **Note:** The Worker **must use identical** PostgreSQL and Secret Key values
> as the Authentik Server — otherwise the two containers won't be able to communicate.

> 💡 **Tip:** The Worker runs as `root` internally to manage certificates and the
> Docker socket. This is expected behavior for authentik.
