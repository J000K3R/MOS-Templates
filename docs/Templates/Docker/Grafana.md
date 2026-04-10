---
title: 📊 Grafana
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 12
---

# 📊 Grafana

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/grafana.png" width="80" />

**Grafana** is the open source analytics and monitoring solution for every database. Create, explore, and share beautiful dashboards with your team. Visualize metrics, logs, and traces from multiple data sources in one unified interface.

Grafana is often paired with **Prometheus**, **InfluxDB**, or **Loki** for a complete monitoring stack.

🏷️ **Category:** Monitoring

🐳 **Image:** `grafana/grafana:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/grafana/grafana](https://github.com/grafana/grafana) |
| 🐛 **Support** | [GitHub Issues](https://github.com/grafana/grafana/issues) |
| 📖 **Docs** | [grafana.com/docs](https://grafana.com/docs/) |
| 🏪 **Plugins** | [grafana.com/plugins](https://grafana.com/grafana/plugins/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3000` | TCP | Grafana Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/grafana` | `/var/lib/grafana` | RW | Grafana configuration, dashboards, and data storage. |

---

## ⚙️ Environment Variables

### 🔑 Security

| Variable | Default | Masked | Description |
|---|---|---|---|
| `GF_SECURITY_ADMIN_USER` | `admin` | ❌ | Username for the Grafana admin account. |
| `GF_SECURITY_ADMIN_PASSWORD` | `admin` | ✅ | Password for the Grafana admin account (change this!). |
| `GF_USERS_ALLOW_SIGN_UP` | `false` | ❌ | Disable user self-registration. |

### 🌍 Server

| Variable | Default | Masked | Description |
|---|---|---|---|
| `GF_SERVER_ROOT_URL` | `http://localhost:3000` | ❌ | Public URL for Grafana (change if using reverse proxy). |
| `GF_INSTALL_PLUGINS` | `` | ❌ | Comma-separated list of plugins to install on startup (optional). |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions. |
| `PGID` | `500` | ❌ | Group ID for file permissions. |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Grafana**
2. Change the default `GF_SECURITY_ADMIN_PASSWORD` to a secure password
3. Adjust `GF_SERVER_ROOT_URL` if using a reverse proxy
4. Click **Install**
5. Access Grafana at `http://YOUR_SERVER_IP:3000`
6. Login with your admin credentials
7. Add your first data source (Prometheus, InfluxDB, etc.)

---

## 🔗 Popular Data Sources

Grafana supports 100+ data sources:

| Data Source | Type | Description |
|---|---|---|
| **Prometheus** | Metrics | Time-series monitoring for Kubernetes, Docker, etc. |
| **InfluxDB** | Metrics/Logs | High-performance time-series database |
| **Loki** | Logs | Log aggregation system (Grafana Labs) |
| **MySQL/PostgreSQL** | SQL | Query SQL databases directly |
| **Elasticsearch** | Logs/Metrics | Full-text search and analytics |

---

## 📊 Pre-built Dashboards

Grafana has a large library of pre-built dashboards:

1. In Grafana, go to **Dashboards → Import**
2. Enter a dashboard ID from [grafana.com/dashboards](https://grafana.com/dashboards)
3. Select your data source
4. Enjoy instant visualizations!

**Popular Dashboards:**
- Node Exporter Full (ID: 1860) - System metrics
- Docker Monitoring (ID: 893) - Container stats
- Unraid System (ID: 7233) - Unraid-specific metrics

---

## 🔌 Useful Plugins

Install plugins via the environment variable `GF_INSTALL_PLUGINS`:

| Plugin | ID | Description |
|---|---|---|
| Clock | `grafana-clock-panel` | Time display for dashboards |
| Pie Chart | `grafana-piechart-panel` | Pie charts and donut charts |
| Worldmap | `grafana-worldmap-panel` | Geographic data visualization |

Example: `GF_INSTALL_PLUGINS=grafana-clock-panel,grafana-piechart-panel`

---

> ⚠️ **Note:** Change the default admin password immediately after first login for security!

> 💡 **Tip:** Use the **Explore** feature to test queries against your data sources before building dashboards.

> 💡 **Tip:** Enable **Alerting** to get notifications when metrics cross thresholds — supports Email, Slack, Discord, PagerDuty, and more!

> 📚 **For more information:** Visit the [Grafana documentation](https://grafana.com/docs/) for detailed setup guides, alerting configuration, and plugin development.
