---
title: 📊 Tracearr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 20
---

# 📊 Tracearr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/tracearr.png" width="80" />

**Tracearr** is a monitoring platform for **Plex**, **Jellyfin** and **Emby**.
Track streams in real-time, dig into playback analytics and spot account sharing
before it gets out of hand.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/connorgallopo/tracearr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/connorgallopo/tracearr](https://github.com/connorgallopo/tracearr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/connorgallopo/Tracearr/issues) |
| 💛 **Donate** | [github.com/sponsors/connorgallopo](https://github.com/sponsors/connorgallopo) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3000` | TCP | Tracearr Web Interface |

---

## 💾 Volumes

No volumes required for this container.

---

## ⚙️ Environment Variables

### 🔑 Security

| Variable | Default | Masked | Description |
|---|---|---|---|
| `JWT_SECRET` | `` | ✅ | JWT secret key (generate with `openssl rand -hex 32`) |
| `COOKIE_SECRET` | `` | ✅ | Cookie secret key (generate with `openssl rand -hex 32`) |

### 🗄️ Database & Cache

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DATABASE_URL` | `postgres://tracearr:tracearr@postgres:5432/tracearr` | ❌ | PostgreSQL connection URL |
| `REDIS_URL` | `redis://redis:6379` | ❌ | Redis connection URL |

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone |
| `NODE_ENV` | `production` | ❌ | Node environment |
| `HOST` | `0.0.0.0` | ❌ | Bind address |
| `CORS_ORIGIN` | `*` | ❌ | CORS origin (`*` for all, or specific domain) |
| `LOG_LEVEL` | `info` | ❌ | Log verbosity (`debug`, `info`, `warn`, `error`) |

---

## 🔑 Generating Secrets

Both `JWT_SECRET` and `COOKIE_SECRET` must be generated before starting.
Run the following command twice to generate two separate keys:
```bash
openssl rand -hex 32
```

---

## 🚀 Quick Start

1. Make sure **PostgreSQL** and **Redis** are running first (required!)
2. Generate your `JWT_SECRET` and `COOKIE_SECRET`
3. Open the **MOS Hub** and search for **Tracearr**
4. Fill in `DATABASE_URL` with your PostgreSQL connection details
5. Fill in `REDIS_URL` with your Redis connection details
6. Paste both generated secrets
7. Click **Install**
8. Access Tracearr at `http://your-server-ip:3000`

---

## 🔗 DATABASE_URL Format
```
postgres://DB_USER:DB_PASSWORD@POSTGRES_HOST:5432/DB_NAME
```

Example:
```
postgres://tracearr:mysecretpassword@postgres:5432/tracearr
```

---

> ⚠️ **Note:** Tracearr requires a running **PostgreSQL** and **Redis** instance.
> Make sure both are up and accessible before starting Tracearr!

> 💡 **Tip:** Set `CORS_ORIGIN` to your specific domain instead of `*` in
> production for better security — e.g. `https://tracearr.yourdomain.com`
