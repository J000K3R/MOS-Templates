---
title: 🐘 TimescaleDB
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 21
---

# 🐘 TimescaleDB

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/tigerdata.png" width="80" />

**TimescaleDB** is an open-source time-series database built on top of **PostgreSQL**.
It adds powerful time-series capabilities like continuous aggregates, automatic partitioning
and compression — while remaining fully compatible with standard PostgreSQL.

This container is used as the dedicated database backend for **[Tracearr](./Tracearr.html)**,
which requires TimescaleDB for its Quality tracking and historical data features.

🏷️ **Category:** Database

🐳 **Image:** `timescale/timescaledb:latest-pg15`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/timescale/timescaledb](https://github.com/timescale/timescaledb) |
| 🐛 **Support** | [GitHub Issues](https://github.com/timescale/timescaledb/issues) |
| 📖 **Docs** | [docs.timescale.com](https://docs.timescale.com/self-hosted/latest/install/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `5432` | TCP | PostgreSQL / TimescaleDB default port. Change the host port if you already have another PostgreSQL instance running on `5432` |

---

## 💾 Volumes

| Host Path | Container Path | Description |
|---|---|---|
| `/mnt/cache/appdata/timescaledb/data` | `/var/lib/postgresql/data` | Persistent database storage |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `POSTGRES_USER` | `tracearr` | ❌ | Database user that will be created on first startup |
| `POSTGRES_PASSWORD` | `` | ✅ | Secure password for the database user. Must be set before starting the container |
| `POSTGRES_DB` | `tracearr` | ❌ | Name of the database that will be automatically created on first startup |
| `TZ` | `Europe/Vienna` | ❌ | Timezone of the container, e.g. `Europe/Vienna` or `Europe/Berlin` |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **TimescaleDB**
2. Set a secure `POSTGRES_PASSWORD`
3. Adjust `POSTGRES_USER` and `POSTGRES_DB` if needed
4. Click **Install**
5. After the container is running, enable the TimescaleDB extension:

```bash
docker exec -it timescaledb psql -U tracearr -d tracearr -c "CREATE EXTENSION IF NOT EXISTS timescaledb;"
```

6. Point your **[Tracearr](./Tracearr.html)** `DATABASE_URL` to this container

---

## 🔗 DATABASE_URL for Tracearr

Use this format in your Tracearr configuration:

```
postgres://DB_USER:DB_PASSWORD@HOST_IP:5433/DB_NAME
```

Example:
```
postgres://tracearr:mysecretpassword@192.168.11.253:5433/tracearr
```

---

> ⚠️ **Note:** The host port is `5433` (not `5432`) to avoid conflicts if you already
> have a standard PostgreSQL container running on your server.

> 💡 **Tip:** After migrating Tracearr to this container, drop any manually created
> `library_stats_daily` table and restart Tracearr — it will recreate it correctly
> as a TimescaleDB continuous aggregate.