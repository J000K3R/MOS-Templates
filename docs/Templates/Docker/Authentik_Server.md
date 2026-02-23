---
title: 🔐 Authentik Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 2
---

# 🔐 Authentik Server

![Authentik](https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/authentik.png)

**authentik** is an open-source Identity Provider (IdP) for modern SSO. It supports
SAML, OAuth2/OIDC, LDAP, RADIUS, and more, designed for self-hosting from small labs
to large production clusters.

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

| Port | Protocol | Description |
|---|---|---|
| `9000` | TCP | Authentik Web Interface (HTTP) |
| `9443` | TCP | Authentik Web Interface (HTTPS) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/authentik/templates/` | `/templates` | RW | Custom Templates |
| `/mnt/cache/appdata/authentik/media/` | `/media` | RW | Media Files |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RO | Docker Socket |

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
2. Search for **Authentik Server**
3. Set up a **PostgreSQL** database first (required!)
4. Fill in the required environment variables
5. Click **Install**
6. Access the Web UI at `http://your-server-ip:9000`

---

> ⚠️ **Note:** Authentik requires a running **PostgreSQL** instance and a **Redis**
> instance. Make sure to deploy those first before starting the Authentik Server.

> 💡 **Tip:** Authentik Server and **Authentik Worker** need to be deployed together
> and share the same configuration values.
