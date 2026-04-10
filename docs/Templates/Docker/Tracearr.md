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
| `DATABASE_URL` | `postgres://tracearr:tracearr@timescaledb:5433/tracearr` | ❌ | PostgreSQL connection URL — point this to your **TimescaleDB** container |
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

1. Install and start **[TimescaleDB](./TimescaleDB.html)** first — Tracearr requires it for quality tracking features
2. Make sure **Redis** is running as well
3. Generate your `JWT_SECRET` and `COOKIE_SECRET`
4. Open the **MOS Hub** and search for **Tracearr**
5. Fill in `DATABASE_URL` pointing to your TimescaleDB instance
6. Fill in `REDIS_URL` with your Redis connection details
7. Paste both generated secrets
8. Click **Install**
9. Access Tracearr at `http://your-server-ip:3000`

---

## 🔗 DATABASE_URL Format
```
postgres://DB_USER:DB_PASSWORD@TIMESCALEDB_HOST:5432/DB_NAME
```

Example:
```
postgres://tracearr:mysecretpassword@192.168.11.253:5432/tracearr
```

---

> ⚠️ **Note:** Tracearr requires a running **[TimescaleDB](https://j000k3r.github.io/MOS-Templates/docs/Templates/Docker/TimescaleDB.html)** and **Redis** instance.
> Using a plain PostgreSQL instance is possible but will cause errors on the
> **Quality** page — specifically the `library_stats_daily` continuous aggregate
> will not work without the TimescaleDB extension.

> 💡 **Tip:** Set `CORS_ORIGIN` to your specific domain instead of `*` in
> production for better security — e.g. `https://tracearr.yourdomain.com`