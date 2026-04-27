---
title: 🏠 Matter Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 21
---

# 🏠 Matter Server

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/matter.png" width="80" />

**Matter Server** is the Python Matter Server for Home Assistant. It's required for commissioning and managing Matter devices (both WiFi and Thread). It connects to Home Assistant via WebSocket and handles all Matter protocol operations.

Essential for any modern smart home using Matter devices like Eve, Nanoleaf, Aqara, Philips Hue, and many more.

🏷️ **Category:** Smart Home

🐳 **Image:** `ghcr.io/home-assistant-libs/python-matter-server:stable`

⚠️ **Requires:** Bluetooth adapter for commissioning (or Thread Border Router for Thread devices)

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/home-assistant-libs/python-matter-server](https://github.com/home-assistant-libs/python-matter-server) |
| 📖 **Home Assistant Matter** | [home-assistant.io/integrations/matter](https://www.home-assistant.io/integrations/matter/) |
| 🏠 **Matter Standard** | [csa-iot.org/all-solutions/matter](https://csa-iot.org/all-solutions/matter/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/home-assistant-libs/python-matter-server/issues) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `5580` | WebSocket | Matter Server WebSocket API. Home Assistant connects here. |

**Note:** Uses host networking, so port is on the host directly.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/matter-server` | `/data` | RW | Matter fabric data, credentials, PAA certificates |
| `/run/dbus` | `/run/dbus` | RO | D-Bus socket for Bluetooth access |

---

## ⚙️ Environment Variables

### 🔧 Core Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `MATTER_STORAGE_PATH` | `/data` | ❌ | Where Matter stores fabric data. Must match volume mount. |
| `MATTER_WS_PORT` | `5580` | ❌ | WebSocket port. HA connects to `ws://host:5580/ws` |
| `PAA_ROOT_CERT_DIR` | `/data/credentials` | ❌ | PAA root certificates for device attestation |

### 📡 Bluetooth Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `BLUETOOTH_ADAPTER` | `0` | ❌ | Bluetooth adapter index (0=hci0). Set to -1 to disable. |

### 🌍 System

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Prerequisites

**Required for commissioning new devices:**
- **Bluetooth adapter** (USB dongle or built-in) on the host
- **D-Bus socket** available at `/run/dbus`

**For Thread devices (Eve, Nanoleaf, etc.):**
- **OpenThread Border Router** (see OTBR template)
- Thread radio device (SMLIGHT SLZB, etc.)

### Step 1: Verify Bluetooth (for commissioning)

Check Bluetooth is available:
```bash
hciconfig
# or
bluetoothctl list
```

If no adapter, you can still use Matter with already-paired devices, but can't commission new ones via Bluetooth.

### Step 2: Install via MOS Hub

1. Open **MOS Hub** and search for **Matter-Server**
2. Review settings:
   - `MATTER_WS_PORT`: Keep default 5580
   - `BLUETOOTH_ADAPTER`: 0 for first adapter, -1 if no Bluetooth
3. Click **Install**
4. Wait for container to start (downloads PAA certificates on first run)

### Step 3: Connect to Home Assistant

1. In Home Assistant, go to **Settings** → **Devices & Services**
2. Click **Add Integration**
3. Search for **"Matter"**
4. Select **"Add Matter device"**
5. Choose **"My own Matter server"**
6. Enter WebSocket URL:
   ```
   ws://YOUR_SERVER_IP:5580/ws
   ```
7. Matter integration will connect to the server

### Step 4: Commission a Matter Device

**Via Bluetooth (WiFi/Ethernet devices):**
1. Put device in pairing mode
2. In HA: **Settings** → **Devices & Services** → **Add Device**
3. Select **"Add Matter device"** → **"Commission using Bluetooth"**
4. Follow on-screen instructions

**Via Thread Border Router (Thread devices):**
1. Ensure OTBR is running and connected
2. Put Thread device in pairing mode
3. In HA: **Settings** → **Devices & Services** → **Add Device**
4. Select **"Add Matter device"** → **"Commission using phone"** or **"Commission with code"**
5. Follow pairing process

---

## 🔧 Configuration Guide

### Bluetooth Adapter Selection

If you have multiple Bluetooth adapters:
```bash
# List adapters
hciconfig

# Result example:
# hci0: Type: Primary  Bus: USB
# hci1: Type: Primary  Bus: USB
```

Set `BLUETOOTH_ADAPTER` to the index (0 for hci0, 1 for hci1).

### No Bluetooth? Commission via Phone

If your server has no Bluetooth:
1. Set `BLUETOOTH_ADAPTER=-1`
2. Use **Home Assistant Companion App** on your phone
3. Pairing happens via phone's Bluetooth
4. Phone transfers credentials to Matter Server

### Thread + Matter Together

For Thread devices, you need BOTH containers:
1. **Matter Server** (this template) - handles Matter protocol
2. **OpenThread Border Router** - handles Thread network

**Setup order:**
1. Install OTBR first, verify Thread network
2. Install Matter Server
3. In HA, add Matter integration (connects to port 5580)
4. HA automatically discovers OTBR as Thread border router

### Custom WebSocket Port

If port 5580 is in use:
```
MATTER_WS_PORT=5581
```

Then in HA, use:
```
ws://YOUR_SERVER_IP:5581/ws
```

---

## 📊 How It Works

### Matter Architecture

```
┌─────────────────┐     WebSocket      ┌─────────────────┐
│   Home Assistant│◄───────────────────►│  Matter Server  │
│   (Frontend)    │     Port 5580      │   (This Docker) │
└─────────────────┘                      └────────┬────────┘
                                                │
                      ┌─────────────────────────┼──────────┐
                      │                         │          │
                      ▼                         ▼          ▼
              ┌───────────┐            ┌──────────┐  ┌─────────┐
              │ Bluetooth │            │   WiFi   │  │ Thread  │
              │Commission │            │ Devices  │  │(via OTBR│
              └─────┬─────┘            └────┬─────┘  └───┬─────┘
                    │                         │            │
                    ▼                         ▼            ▼
              ┌───────────┐            ┌──────────┐  ┌─────────┐
              │ Eve Motion│            │ Nanoleaf │  │ Eve Door│
              │  (sensor) │            │  Bulb    │  │ Window  │
              └───────────┘            └──────────┘  └─────────┘
```

### Commissioning Methods

| Method | Works Without | Use Case |
|---|---|---|
| **Bluetooth** | Thread Border Router | WiFi/Ethernet Matter devices |
| **Thread** | Bluetooth | Already Thread-enabled devices |
| **Phone App** | Server Bluetooth | Delegate pairing to phone |
| **Manual Code** | Nothing | Enter pairing code directly |

---

## 🛠️ Troubleshooting

### "Failed to connect to Matter server"

1. Verify Matter Server is running:
   ```bash
   docker ps | grep matter
   ```

2. Check port 5580 is listening:
   ```bash
   netstat -tlnp | grep 5580
   ```

3. Test WebSocket connection:
   ```bash
   curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" \
     -H "Host: localhost" -H "Origin: http://localhost" \
     http://YOUR_SERVER_IP:5580/ws
   ```

4. Check Matter Server logs:
   ```bash
   docker logs matter-server
   ```

### "Bluetooth not available"

1. Verify Bluetooth on host:
   ```bash
   hciconfig
   systemctl status bluetooth
   ```

2. Check D-Bus socket exists:
   ```bash
   ls -la /run/dbus/system_bus_socket
   ```

3. Fix permissions:
   ```bash
   sudo usermod -aG bluetooth $USER
   # Logout and login again
   ```

4. If no Bluetooth needed, set `BLUETOOTH_ADAPTER=-1`

### "Commissioning failed"

1. **Device not in pairing mode** - Check device manual for pairing procedure
2. **Wrong Bluetooth adapter** - Try `BLUETOOTH_ADAPTER=0` or `=1`
3. **Bluetooth interference** - Move closer to server
4. **Phone method** - Use HA Companion App instead of server Bluetooth

### "Thread devices not working"

1. Verify OTBR is running and connected
2. Check HA discovered OTBR as border router
3. Verify Thread network is operational
4. Matter Server and OTBR should be on same host or network

### "PAA certificate errors"

On first run, Matter Server downloads PAA certificates:
1. Wait 2-3 minutes after first start
2. Check `/mnt/cache/appdata/matter-server/credentials/`
3. Restart container if stuck

### Container won't start

1. Check privileged mode is enabled
2. Verify D-Bus path exists: `ls /run/dbus`
3. Check volume permissions:
   ```bash
   chown -R 500:500 /mnt/cache/appdata/matter-server
   ```

---

## 🔗 Related Templates

| Template | Purpose | Integration |
|---|---|---|
| **OpenThread Border Router** | Thread network | Required for Thread devices |
| **Home Assistant** | Home automation | Consumes Matter Server API |
| **Mosquitto** | MQTT broker | For other protocols |
| **Zigbee2MQTT** | Zigbee network | Alternative to Matter |

---

## 💡 Best Practices

### Backup Strategy

**Critical:** Backup the data directory:
```bash
# Matter fabric and device credentials
tar -czf matter-backup-$(date +%Y%m%d).tar.gz /mnt/cache/appdata/matter-server/
```

Without this backup, you'll need to re-pair all devices if the server fails!

### Security

- Matter uses **certificate-based authentication**
- Each device has unique credentials
- Fabric data in `/data` contains keys - protect it!
- Commissioning requires physical access or QR code

### Network Requirements

- **mDNS** must work on your network (for device discovery)
- **IPv6** recommended (Thread uses IPv6)
- Host networking mode is required (can't use bridge)

### Commissioning Tips

1. **Use Bluetooth 5.0+** for best range
2. **USB extension cable** moves dongle away from interference
3. **Phone app method** works even without server Bluetooth
4. **Write down pairing codes** before factory reset

---

> 💡 **Tip:** The first start downloads PAA certificates (~50MB). Initial startup may take 2-3 minutes. Check logs with `docker logs matter-server`.

> 💡 **Tip:** If you have both Matter and Thread devices, you need this Matter Server + OpenThread Border Router. They work together!

> 💡 **Tip:** You can run Matter Server without Bluetooth if you use the Home Assistant mobile app for commissioning.

> 📚 **For more information:** Visit the [Home Assistant Matter Integration](https://www.home-assistant.io/integrations/matter/) documentation.
