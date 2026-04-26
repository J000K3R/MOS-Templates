---
title: 🔌 OpenThread Border Router
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 20
---

# 🔌 OpenThread Border Router

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/openthread.png" width="80" />

**OpenThread Border Router (OTBR)** connects a Thread network to your LAN. It enables Matter-over-Thread smart home devices and provides the bridge between your WiFi/Ethernet network and the low-power Thread mesh network.

Essential for modern smart home setups using Matter/Thread devices like Eve, Nanoleaf, Aqara, and more.

🏷️ **Category:** Network

🐳 **Image:** `bnutzer/otbr-tcp:latest`

⚠️ **Requires:** Thread Radio Device (SMLIGHT SLZB, local USB stick, etc.)

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [openthread.io](https://openthread.io/) |
| 📖 **Documentation** | [openthread.io/guides](https://openthread.io/guides) |
| 🐛 **Support** | [GitHub Issues](https://github.com/bnutzer/docker-otbr-tcp/issues) |
| 📦 **Thread** | [threadgroup.org](https://www.threadgroup.org/) |
| 🏠 **Matter** | [csa-iot.org/all-solutions/matter](https://csa-iot.org/all-solutions/matter/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8081` | TCP | **REST API** - Home Assistant integration uses this |
| `8080` | TCP | **Web UI** - Optional OTBR management interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/otbr` | `/var/lib/thread` | RW | Thread network dataset and state (critical!) |

---

## ⚙️ Environment Variables

### 🧭 Core Configuration

| Variable | Default | Masked | Description |
|---|---|---|---|
| `RCP_HOST` | `` | ❌ | Thread radio IP/hostname (e.g., `192.168.1.15` for SLZB) |
| `RCP_PORT` | `7638` | ❌ | Radio port: 7638 (SLZB MR-series), 6638 (SLZB-06) |
| `RCP_BAUDRATE` | `460800` | ❌ | Communication speed: 460800, 230400, or 115200 |
| `RCP_USE_TCP` | `1` | ❌ | `1` for network radio (TCP), `0` for USB stick |

### 🖥️ Interface Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `OTBR_THREAD_IF` | `wpan0` | ❌ | Thread TUN device name |
| `OTBR_BACKBONE_IF` | `eth0` | ❌ | Host LAN interface: eth0, end0, wlan0 |
| `OTBR_REST_LISTEN_PORT` | `8081` | ❌ | REST API port for Home Assistant |
| `OTBR_WEB_ENABLE` | `1` | ❌ | Enable web UI: `1`=yes, `0`=no |
| `OTBR_WEB_PORT` | `8080` | ❌ | Web UI port |

### 🔧 Advanced Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `OTBR_RCP_ADDITIONAL_ARGS` | `&uart-flow-control` | ❌ | Flow control - remove if radio doesn't support it |
| `OTBR_LOG_LEVEL_INT` | `6` | ❌ | 0=emerg, 3=err, 4=warn, 6=info, 7=debug |
| `TZ` | `Europe/Vienna` | ❌ | Timezone |

---

## 🚀 Quick Start

### Prerequisites

You need a **Thread Radio Device**:

**Network-attached (TCP):**
- **SMLIGHT SLZB-06** (PoE, best value)
- **SMLIGHT SLZB-MR1** (mini, USB powered)
- Any device running `silabs-multiprotocol` or similar

**USB-attached:**
- **SkyConnect** (Home Assistant Yellow/SkyConnect)
- **SMLIGHT SLZB-07** (USB stick)
- **EFR32MG21** based sticks

### Step 1: Configure Your Thread Radio

**For SMLIGHT SLZB devices:**
1. Connect SLZB to your network
2. Find its IP address (check router or use `arp-scan`)
3. Enable the Thread radio in the SLZB web interface
4. Note the port (usually 7638)

### Step 2: Install OTBR via MOS Hub

1. Open **MOS Hub** and search for **OpenThread-Border-Router**
2. Configure radio connection:
   - `RCP_HOST`: Your SLZB IP (e.g., `192.168.1.15`)
   - `RCP_PORT`: `7638` (or `6638` for older SLZB-06)
   - `RCP_USE_TCP`: `1` for network radio
3. Verify `OTBR_BACKBONE_IF` matches your host interface:
   - Ethernet: `eth0` or `end0`
   - WiFi: `wlan0`
4. Click **Install**

### Step 3: Access OTBR Web UI

1. Open `http://YOUR_SERVER_IP:8080`
2. Verify radio connection status
3. Check Thread network formation

### Step 4: Connect to Home Assistant

1. In HA, go to **Settings** → **Devices & Services**
2. Click **Add Integration** → Search **"OpenThread Border Router"**
3. Enter: `http://YOUR_SERVER_IP:8081`
4. OTBR will be discovered and Thread network added

### Step 5: Pair Thread Devices

1. In Home Assistant, go to **Settings** → **Devices & Services**
2. Click **Add Device** → **Add Matter device**
3. Select **"Add device using the mobile app"** or **"Commission with code"**
4. Follow device pairing instructions

---

## 🔧 Configuration Guide

### SMLIGHT SLZB Setup

**SLZB-06 (PoE version):**
```
RCP_HOST=192.168.1.15
RCP_PORT=6638
RCP_USE_TCP=1
```

**SLZB-MR1/MR2 (USB/mini):**
```
RCP_HOST=192.168.1.15
RCP_PORT=7638
RCP_USE_TCP=1
```

### USB Thread Stick Setup

For local USB-attached radios (SkyConnect, etc.):

1. Find USB device:
   ```bash
   ls /dev/serial/by-id/
   ls /dev/ttyACM*
   ls /dev/ttyUSB*
   ```

2. Update container `extra_parameters`:
   ```
   --device=/dev/ttyUSB1
   ```

3. Set environment variables:
   ```
   RCP_USE_TCP=0
   RCP_HOST=
   RCP_PORT=
   ```

### Network Interface Detection

Find your backbone interface:
```bash
ip addr show
# Look for your main LAN interface
```

Common values:
- `eth0` - Standard Ethernet
- `end0` - Some newer systems (Debian 12+)
- `wlan0` - WiFi interface

### Baud Rate Adjustment

If experiencing instability:
```
RCP_BAUDRATE=230400
# or
RCP_BAUDRATE=115200
```

### Disable Flow Control

If your radio doesn't support flow control:
```
OTBR_RCP_ADDITIONAL_ARGS=
```

---

## 📊 Thread + Matter Concepts

### Thread Network
- Low-power mesh network protocol
- Self-healing, no single point of failure
- Devices route through each other
- **Range:** ~30-100m per hop, unlimited with mesh

### Border Router Role
- Bridges Thread network to IP network (WiFi/Ethernet)
- Multiple OTBRs can exist (redundancy)
- Required for Matter-over-Thread devices

### Matter Protocol
- Universal smart home standard (Apple, Google, Amazon, Samsung)
- Runs over Thread (wireless) or WiFi/Ethernet
- Local control, works without internet
- **Requires OTBR** for Thread devices

---

## 🏠 Compatible Devices

**Lights:**
- Eve Energy (outlet with power monitoring)
- Eve MotionBlinds
- Nanoleaf Essentials
- Aqara Smart Bulbs

**Sensors:**
- Eve Door & Window
- Eve Motion
- Eve Room (temp/humidity/air)
- Aqara sensors (various)

**Other:**
- Eve Aqua (water controller)
- Eve Flare (portable lamp)
- Schlage Encode Plus (lock)
- Any Matter-certified Thread device

---

## 🛠️ Troubleshooting

### "No Radio Connection"

1. **Verify radio IP:** Can you ping it?
   ```bash
   ping 192.168.1.15
   ```
2. **Check radio port:** Test with telnet
   ```bash
   telnet 192.168.1.15 7638
   ```
3. **Verify SLZB settings:** Is Thread radio enabled in web UI?
4. **Check baud rate:** Try lowering to 230400 or 115200

### "Home Assistant Can't Connect"

1. Verify REST API is accessible:
   ```bash
   curl http://YOUR_SERVER_IP:8081/node
   ```
2. Check `OTBR_REST_LISTEN_PORT` is 8081
3. Ensure network mode is `host` (not bridge)
4. Try using container IP instead of host IP

### "Thread Network Not Forming"

1. Check radio connection in OTBR web UI
2. Verify backbone interface is correct
3. Check logs: `docker logs openthread-border-router`
4. Ensure only one Thread network exists (or use different PAN IDs)

### "Matter Pairing Fails"

1. Ensure device is in pairing mode
2. Check Bluetooth is enabled on phone (for commissioning)
3. Verify Thread network is operational
4. Try moving device closer to Thread router
5. Check Home Assistant Matter integration logs

### "Container Won't Start"

1. Check privileged mode is enabled
2. Verify `/dev/net/tun` device exists:
   ```bash
   ls -la /dev/net/tun
   ```
3. Check logs for missing capabilities
4. Ensure volume path exists: `mkdir -p /mnt/cache/appdata/otbr`

---

## 🔗 Related Templates

| Template | Purpose | Integration |
|---|---|---|
| **Home Assistant** | Home automation | Matter/Thread integration |
| **Mosquitto** | MQTT broker | For other smart home protocols |
| **Zigbee2MQTT** | Zigbee network | Alternative mesh network |

---

## 💡 Best Practices

### Network Redundancy
- Multiple OTBRs can coexist for redundancy
- Each OTBR shares the same Thread dataset
- If one fails, Thread network continues via others

### Positioning
- Place Thread routers (powered devices) strategically
- Battery devices don't route traffic
- Aim for multiple paths between any two points

### Maintenance
- **Backup the data volume** - contains Thread dataset
- Keep OTBR updated for security and compatibility
- Monitor radio connection status

### Security
- Thread uses AES-CCM encryption
- Commissioning requires physical access or QR code
- Network key is stored in the data volume

---

> 💡 **Tip:** The Thread dataset in `/mnt/cache/appdata/otbr` is critical - backup this directory! It contains the network key and all mesh configuration.

> 💡 **Tip:** For best performance, use a wired backbone connection (eth0) rather than WiFi (wlan0).

> 💡 **Tip:** Multiple OTBRs increase reliability. If your SLZB loses power, devices can route through other Thread routers.

> 📚 **For more information:** Visit the [OpenThread Documentation](https://openthread.io/guides) and [Matter Specification](https://csa-iot.org/all-solutions/matter/) for detailed technical information.
