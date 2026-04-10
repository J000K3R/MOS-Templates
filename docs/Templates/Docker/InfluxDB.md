---
title: 📈 InfluxDB
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 13
---

# 📈 InfluxDB

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/influxdb.png" width="80" />

**InfluxDB** is an open-source time series database designed to handle high write and query loads. It is the perfect choice for storing metrics, events, and real-time analytics data from your applications and infrastructure.

It is commonly used with **Grafana** for visualization, **Telegraf** for data collection, and various monitoring tools.

🏷️ **Category:** Database

🐳 **Image:** `influxdb:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/influxdata/influxdb](https://github.com/influxdata/influxdb) |
| 🐛 **Support** | [GitHub Issues](https://github.com/influxdata/influxdb/issues) |
| 📖 **Docs** | [docs.influxdata.com](https://docs.influxdata.com/influxdb/latest/) |
| 💛 **InfluxData** | [www.influxdata.com](https://www.influxdata.com/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8086` | TCP | InfluxDB HTTP API and Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/influxdb` | `/var/lib/influxdb2` | RW | Persistent storage for InfluxDB data, configuration, and WAL files |

---

## ⚙️ Environment Variables

### 🔑 Setup & Admin

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DOCKER_INFLUXDB_INIT_MODE` | `setup` | ❌ | Initialization mode: 'setup' for first-time, 'upgrade' for existing DB |
| `DOCKER_INFLUXDB_INIT_USERNAME` | `admin` | ❌ | Admin account username |
| `DOCKER_INFLUXDB_INIT_PASSWORD` | `` | ✅ | Admin password (min 8 chars, **required** for first start) |
| `DOCKER_INFLUXDB_INIT_ADMIN_TOKEN` | `` | ✅ | API token for admin operations (auto-generated if empty) |

### 🗄️ Organization & Bucket

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DOCKER_INFLUXDB_INIT_ORG` | `mos` | ❌ | Default organization name |
| `DOCKER_INFLUXDB_INIT_BUCKET` | `data` | ❌ | Default bucket for time-series data |
| `DOCKER_INFLUXDB_INIT_RETENTION` | `0` | ❌ | Retention period in hours (0 = infinite) |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **InfluxDB**
2. Set a secure `DOCKER_INFLUXDB_INIT_PASSWORD` (minimum 8 characters)
3. Optionally set `DOCKER_INFLUXDB_INIT_ADMIN_TOKEN` or leave empty to auto-generate
4. Click **Install**
5. Access the Web UI at `http://YOUR_SERVER_IP:8086`
6. Login with your admin credentials
7. Start writing data via the API or configure data sources like Telegraf

---

## 🔗 Connection Information

**API Endpoint:**
```
http://YOUR_SERVER_IP:8086
```

**InfluxDB URL for Grafana:**
```
http://YOUR_SERVER_IP:8086
```

**Organization:** `mos` (or your custom value)
**Bucket:** `data` (or your custom value)
**Token:** Use the admin token from setup

---

## 📝 Writing Data Examples

### Using curl:
```bash
curl -i -XPOST 'http://YOUR_SERVER_IP:8086/api/v2/write?org=mos&bucket=data' \
  --header 'Authorization: Token YOUR_TOKEN' \
  --data-raw 'cpu,host=server1 usage=45.2'
```

### Line Protocol Format:
```
measurement,tag=value field=value timestamp
```

Example:
```
temperature,sensor=living_room value=22.5 1704067200000000000
```

---

## 🔌 Data Collection Tools

| Tool | Purpose | Integration |
|---|---|---|
| **Telegraf** | System metrics collection | Native InfluxDB output |
| **Prometheus** | Metrics scraping | Remote write to InfluxDB |
| **Grafana** | Visualization | Native InfluxDB v2 data source |

---

## 📊 Common Use Cases

| Use Case | Description |
|---|---|
| **Server Monitoring** | CPU, memory, disk, network metrics |
| **Application Metrics** | Response times, error rates, throughput |
| **IoT Data** | Sensor readings, device telemetry |
| **Financial Data** | Stock prices, trading metrics |
| **Event Tracking** | User actions, system events |

---

## 🔒 Security Notes

- **Always set a strong password** for the admin account
- **Save your admin token** securely - it cannot be recovered if lost
- Use **bucket-specific tokens** for applications instead of admin token
- Enable **HTTPS** for production deployments (use reverse proxy)

---

> ⚠️ **Important:** The `DOCKER_INFLUXDB_INIT_PASSWORD` must be at least 8 characters long. The container will fail to start with a shorter password.

> 💡 **Tip:** Use **Telegraf** with the InfluxDB output plugin to automatically collect system metrics from your MOS server.

> 💡 **Tip:** Pair InfluxDB with **Grafana** for beautiful dashboards and alerting on your metrics.

> 📚 **For more information:** Visit the [InfluxDB documentation](https://docs.influxdata.com/influxdb/latest/) for query language (Flux), API reference, and best practices.
