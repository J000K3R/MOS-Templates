---
title: 📡 Mosquitto (MQTT Broker)
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 17
---

# 📡 Mosquitto (MQTT Broker)

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/mosquitto.png" width="80" />

**Eclipse Mosquitto** is an open source message broker that implements the MQTT protocol. It is lightweight and suitable for IoT devices, home automation, and messaging between applications.

Perfect for Home Assistant, IoT projects, and any application needing reliable publish/subscribe messaging.

🏷️ **Category:** Network

🐳 **Image:** `eclipse-mosquitto:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [mosquitto.org](https://mosquitto.org/) |
| 📖 **Docs** | [mosquitto.org/documentation](https://mosquitto.org/documentation/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/eclipse/mosquitto/issues) |
| 📦 **Docker Hub** | [hub.docker.com/_/eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `1883` | TCP | Standard MQTT port (unencrypted) |
| `9001` | TCP | MQTT over WebSocket (for browser clients) |
| `8883` | TCP | MQTT over TLS (encrypted, requires certificates) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/mosquitto/config` | `/mosquitto/config` | RW | Configuration files (mosquitto.conf, passwords, ACLs) |
| `/mnt/cache/appdata/mosquitto/data` | `/mosquitto/data` | RW | Persistent data (retained messages, database) |
| `/mnt/cache/appdata/mosquitto/log` | `/mosquitto/log` | RW | Log files |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Step 1: Prepare Directories

Create the required directories:
```bash
mkdir -p /mnt/cache/appdata/mosquitto/config
mkdir -p /mnt/cache/appdata/mosquitto/data
mkdir -p /mnt/cache/appdata/mosquitto/log
```

### Step 2: Create Initial Configuration

Create a basic `mosquitto.conf` file:
```bash
cat > /mnt/cache/appdata/mosquitto/config/mosquitto.conf << 'EOF'
# Basic MQTT configuration
listener 1883
allow_anonymous true

# WebSocket support
listener 9001
protocol websockets

# Persistence
persistence true
persistence_location /mosquitto/data/

# Logging
log_dest file /mosquitto/log/mosquitto.log
log_dest stdout

# Include additional configs
include_dir /mosquitto/config/conf.d
EOF
```

### Step 3: Install via MOS Hub

1. Open the **MOS Hub** and search for **Mosquitto**
2. Click **Install**
3. The broker will start on ports 1883 and 9001

### Step 4: Test Connection

Test with mosquitto client or MQTT Explorer:
```bash
# Subscribe to a topic
mosquitto_sub -h YOUR_SERVER_IP -p 1883 -t "test/topic"

# Publish a message (in another terminal)
mosquitto_pub -h YOUR_SERVER_IP -p 1883 -t "test/topic" -m "Hello MQTT"
```

---

## 🔐 Authentication Setup

### Step 1: Create Password File

```bash
docker exec -it mosquitto mosquitto_passwd -c /mosquitto/config/passwd username
```

### Step 2: Update Configuration

Edit `/mnt/cache/appdata/mosquitto/config/mosquitto.conf`:
```
allow_anonymous false
password_file /mosquitto/config/passwd
```

### Step 3: Restart Container

Restart Mosquitto to apply changes:
```bash
docker restart mosquitto
```

---

## 📊 Common Use Cases

### Home Assistant Integration
Home Assistant uses MQTT for many integrations:
```yaml
# configuration.yaml
mqtt:
  broker: YOUR_SERVER_IP
  port: 1883
  username: homeassistant
  password: YOUR_PASSWORD
```

### Tasmota Devices
Flash Tasmota on ESP devices and configure:
```
Host: YOUR_SERVER_IP
Port: 1883
Topic: tasmota_device_01
```

### Zigbee2MQTT
Connect Zigbee devices to MQTT:
```yaml
# zigbee2mqtt configuration
mqtt:
  server: mqtt://YOUR_SERVER_IP:1883
  user: zigbee2mqtt
  password: YOUR_PASSWORD
```

### ESPHome
ESPHome devices can publish sensor data:
```yaml
mqtt:
  broker: YOUR_SERVER_IP
  username: esphome
  password: YOUR_PASSWORD
  topic_prefix: esphome/livingroom
```

---

## 🌐 Bridge Configuration

Connect multiple MQTT brokers:
```
# mosquitto.conf
connection bridge-name
address remote.broker.com:1883
topic # both 0 local/prefix remote/prefix
```

---

## 📱 MQTT Client Recommendations

| Client | Platform | Use Case |
|---|---|---|
| **MQTT Explorer** | Windows/Mac/Linux | Desktop debugging |
| **MQTTX** | All platforms | Modern desktop client |
| **MQTT Dash** | Android | Mobile dashboard |
| **IoT MQTT Panel** | iOS | Mobile dashboard |
| **mosquitto_pub/sub** | Linux | Command line tools |

---

## 🔍 Troubleshooting

### Connection Refused
1. Check if port 1883 is open: `netstat -tlnp | grep 1883`
2. Verify Mosquitto is running: `docker ps | grep mosquitto`
3. Check firewall rules

### Permission Denied
1. Check file ownership: `ls -la /mnt/cache/appdata/mosquitto/`
2. Ensure PUID/PGID can access the directories
3. Fix permissions: `chown -R 500:500 /mnt/cache/appdata/mosquitto/`

### High Memory Usage
1. Disable persistence if not needed: `persistence false`
2. Limit queued messages: `max_queued_messages 100`
3. Check for message loops in clients

### Log Files Growing
Configure log rotation in `mosquitto.conf`:
```
log_dest file /mosquitto/log/mosquitto.log
log_type error
log_type warning
log_type information
connection_messages true
```

---

> 💡 **Tip:** For home automation with under 100 devices, Mosquitto is perfect. For larger deployments, consider EMQX or HiveMQ.

> 💡 **Tip:** Use WebSocket port (9001) for browser-based dashboards and mobile apps.

> 💡 **Tip:** Enable persistence if you use retained messages or want to survive restarts without losing state.

> 💡 **Tip:** Use `$SYS/#` topics to monitor broker health, connection count, and message statistics.

> 📚 **For more information:** Visit the [Mosquitto Documentation](https://mosquitto.org/documentation/) for detailed configuration options, authentication methods, and plugin development.
