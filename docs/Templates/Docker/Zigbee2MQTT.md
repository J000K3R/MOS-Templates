---
title: 🔌 Zigbee2MQTT
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 19
---

# 🔌 Zigbee2MQTT

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/zigbee2mqtt.png" width="80" />

**Zigbee2MQTT** allows you to use your Zigbee devices without the vendor's bridge or gateway. It bridges events and allows you to control your Zigbee devices via MQTT.

Works great with Home Assistant, OpenHAB, and any MQTT-compatible home automation platform.

🏷️ **Category:** Network, Smarthome

🐳 **Image:** `koenkk/zigbee2mqtt:latest`

⚠️ **Requires:** Zigbee USB coordinator (CC2531, ConBee II, Sonoff ZBDongle, etc.)

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [zigbee2mqtt.io](https://www.zigbee2mqtt.io/) |
| 📖 **Documentation** | [zigbee2mqtt.io/guide](https://www.zigbee2mqtt.io/guide/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Koenkk/zigbee2mqtt/issues) |
| 📦 **Supported Devices** | [zigbee2mqtt.io/supported-devices](https://www.zigbee2mqtt.io/supported-devices/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Zigbee2MQTT web frontend |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/zigbee2mqtt` | `/app/data` | RW | Configuration, device database, and state |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🔗 MQTT Connection

| Variable | Default | Masked | Description |
|---|---|---|---|
| `ZIGBEE2MQTT_CONFIG_MQTT_SERVER` | `mqtt://mosquitto:1883` | ❌ | MQTT broker URL. Use container name or IP. |
| `ZIGBEE2MQTT_CONFIG_MQTT_USER` | `` | ❌ | MQTT username (if authentication enabled) |
| `ZIGBEE2MQTT_CONFIG_MQTT_PASSWORD` | `` | ✅ | MQTT password (if authentication enabled) |

### 🔌 Zigbee Adapter

| Variable | Default | Masked | Description |
|---|---|---|---|
| `ZIGBEE2MQTT_CONFIG_SERIAL_PORT` | `/dev/ttyUSB0` | ❌ | USB port of your Zigbee coordinator |
| `ZIGBEE2MQTT_CONFIG_SERIAL_ADAPTER` | `auto` | ❌ | Adapter type: zstack, deconz, zigate, ezsp |
| `ZIGBEE2MQTT_CONFIG_PERMIT_JOIN` | `false` | ❌ | Allow new devices to join (`true`/`false`) |

---

## 🚀 Quick Start

### Prerequisites

You need a **Zigbee USB coordinator**:
- **CC2531** (cheapest, needs flashing)
- **ConBee II / RaspBee** (popular, good range)
- **Sonoff ZBDongle-P/E** (popular, pre-flashed)
- **Electrolama zig-a-zig-ah! (zzh)**
- **Slaesh's CC2652RB stick**

And an **MQTT broker** (like the Mosquitto template).

### Step 1: Find Your USB Device

Before installing, identify your Zigbee adapter:

```bash
# List USB devices
ls -la /dev/serial/by-id/
ls -la /dev/ttyUSB*
ls -la /dev/ttyACM*
```

Note the device path (e.g., `/dev/ttyUSB0` or `/dev/ttyACM0`).

### Step 2: Update Container Device

Edit the container and update the **Device** in `extra_parameters`:
```
--device=/dev/ttyUSB0
```

Use the path you identified in Step 1.

### Step 3: Install via MOS Hub

1. Open the **MOS Hub** and search for **Zigbee2MQTT**
2. Configure MQTT:
   - If using Mosquitto template: keep default `mqtt://mosquitto:1883`
   - Otherwise set your MQTT broker IP: `mqtt://192.168.1.10:1883`
3. Set MQTT credentials if your broker requires authentication
4. Verify `ZIGBEE2MQTT_CONFIG_SERIAL_PORT` matches your USB device
5. Click **Install**

### Step 4: Access Frontend

1. Open Zigbee2MQTT at `http://YOUR_SERVER_IP:8080`
2. The frontend shows network map, devices, and logs

### Step 5: Pair Your First Device

1. In the web frontend, click **Permit join (All)**
2. Put your Zigbee device in pairing mode (usually hold button for 5-10 seconds)
3. Device should appear in the device list
4. Click on device to configure friendly name

---

## 🔧 Configuration

### MQTT Broker Setup

If using the **Mosquitto** template:
```
ZIGBEE2MQTT_CONFIG_MQTT_SERVER=mqtt://mosquitto:1883
```

If Mosquitto requires auth, set the same credentials you configured in Mosquitto.

For external MQTT broker:
```
ZIGBEE2MQTT_CONFIG_MQTT_SERVER=mqtt://192.168.1.10:1883
ZIGBEE2MQTT_CONFIG_MQTT_USER=zigbee2mqtt
ZIGBEE2MQTT_CONFIG_MQTT_PASSWORD=yourpassword
```

### Serial Port Troubleshooting

Common device paths:
- `/dev/ttyUSB0` - Most USB sticks (CC2531, Sonoff)
- `/dev/ttyACM0` - ConBee II, some other adapters
- `/dev/ttyAMA0` - RaspBee on Raspberry Pi GPIO

**Persistent naming** (recommended):
```bash
ls -la /dev/serial/by-id/
# Use the full path like:
# /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_12345-if00-port0
```

Update the device in container settings with the persistent path.

### Adapter Types

Auto-detect works for most adapters, but you can specify:
- `zstack` - Texas Instruments CC2531, CC2652P (most common)
- `deconz` - Dresden ConBee/RaspBee
- `zigate` - ZiGate USB-TTL
- `ezsp` - Silicon Labs EZSP (HubZ, HUSBZB-1)
- `zboss` - ZBOSS NCP

### Pairing Devices

Enable pairing:
```
ZIGBEE2MQTT_CONFIG_PERMIT_JOIN=true
```

**Security best practice:** Only enable when adding devices, then disable:
```
ZIGBEE2MQTT_CONFIG_PERMIT_JOIN=false
```

---

## 📊 Home Assistant Integration

Zigbee2MQTT integrates seamlessly with Home Assistant via MQTT Discovery.

### MQTT Discovery (Automatic)

1. Ensure Home Assistant MQTT integration is configured
2. Zigbee2MQTT auto-discovers devices - they appear automatically in HA
3. Devices show as entities you can control and monitor

### Manual MQTT Topics

Base topic: `zigbee2mqtt`

| Topic | Purpose |
|---|---|
| `zigbee2mqtt/FRIENDLY_NAME` | Device state |
| `zigbee2mqtt/FRIENDLY_NAME/set` | Send commands |
| `zigbee2mqtt/FRIENDLY_NAME/get` | Request state |

### Example: Control a Light

```yaml
# Home Assistant configuration.yaml
light:
  - platform: mqtt
    name: "Bedroom Light"
    state_topic: "zigbee2mqtt/bedroom_light"
    command_topic: "zigbee2mqtt/bedroom_light/set"
    brightness_state_topic: "zigbee2mqtt/bedroom_light"
    brightness_command_topic: "zigbee2mqtt/bedroom_light/set"
    brightness_scale: 254
```

---

## 🛠️ Troubleshooting

### "Error: Failed to connect to MQTT"

1. Verify MQTT broker is running: `docker ps | grep mosquitto`
2. Check MQTT URL is correct
3. Verify credentials if auth is enabled
4. Test with MQTT client: `mosquitto_pub -h IP -t test -m "hello"`

### "Error: SRSP - AF data request"

USB device not found or permissions issue:
1. Check device path is correct: `ls -la /dev/ttyUSB*`
2. Verify PUID/PGID can access the device
3. Fix permissions: `sudo usermod -aG dialout $USER`

### Devices Not Pairing

1. Enable permit join: `ZIGBEE2MQTT_CONFIG_PERMIT_JOIN=true`
2. Check device is Zigbee (not Z-Wave or WiFi)
3. Ensure device is in pairing mode (check device manual)
4. Move device closer to coordinator
5. Check supported devices list: [zigbee2mqtt.io/supported-devices](https://www.zigbee2mqtt.io/supported-devices/)

### Weak Signal / Dropped Connections

1. Use USB extension cable (2-3m) to move coordinator away from USB3/WiFi
2. Add Zigbee routers (smart plugs, bulbs) to extend mesh
3. Check channel conflicts (WiFi vs Zigbee)
4. Consider coordinator with external antenna

### High CPU Usage

1. Reduce log level in configuration
2. Disable device reporting if not needed
3. Check for devices flooding the network

---

## 💡 Best Practices

### Zigbee Network Range

- Zigbee creates a **mesh network** - powered devices (routers) extend range
- Battery devices (end devices) don't route traffic
- Add smart plugs or powered bulbs to extend network

### Security

- **Always disable** `PERMIT_JOIN` after pairing devices
- Change default MQTT passwords
- Use separate MQTT user for Zigbee2MQTT
- Keep coordinator firmware updated

### USB Port Selection

- Avoid USB 3.0 ports (causes interference)
- Use USB 2.0 port or extension cable
- Keep away from WiFi router/Bluetooth devices

### Backup

Backup the data volume:
```bash
cp -r /mnt/cache/appdata/zigbee2mqtt /backup/zigbee2mqtt-$(date +%Y%m%d)
```

This preserves your device network (pairing info).

---

## 🔗 Related Templates

| Template | Purpose | Integration |
|---|---|---|
| **Mosquitto** | MQTT broker | Required for Zigbee2MQTT |
| **Home Assistant** | Home automation | Consumes MQTT data |
| **Node-RED** | Automation flows | Can process MQTT events |

---

> 💡 **Tip:** Keep your coordinator firmware updated for bug fixes and new device support.

> 💡 **Tip:** A USB 2.0 extension cable (2-3 meters) significantly improves Zigbee range by moving the coordinator away from interference sources.

> 💡 **Tip:** After initial setup, disable PERMIT_JOIN to prevent unauthorized devices from joining your network.

> 📚 **For more information:** Visit the [Zigbee2MQTT Documentation](https://www.zigbee2mqtt.io/guide/) for device-specific instructions, network optimization, and troubleshooting.
