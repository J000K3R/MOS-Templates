---
title: 🔗 Matter Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 33
---

# 🔗 Matter Server

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/matter.png" width="80" />

**Matter Server** (matter.js) is the Matter controller that **Home Assistant** uses to talk to Matter devices (Wi-Fi, Ethernet and Thread). It is the official successor of the archived *Python Matter Server* and uses the same WebSocket API, so Home Assistant connects to it without any changes.

On Home Assistant OS this runs as an add-on. With Home Assistant in Docker (e.g. on MOS), you run it as its own container next to Home Assistant.

🏷️ **Category:** Network, Home Automation

🐳 **Image:** `ghcr.io/matter-js/matterjs-server:stable`

⚠️ **Requires:** Host networking · IPv6 enabled on the host interface · Same (V)LAN as your Matter devices, Thread border routers and phone

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/matter-js/matterjs-server](https://github.com/matter-js/matterjs-server) |
| 🐳 **Docker Guide** | [docs/docker.md](https://github.com/matter-js/matterjs-server/blob/main/docs/docker.md) |
| 🖥️ **OS Requirements** | [docs/os_requirements.md](https://github.com/matter-js/matterjs-server/blob/main/docs/os_requirements.md) |
| 🏠 **Home Assistant** | [home-assistant.io/integrations/matter](https://www.home-assistant.io/integrations/matter/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/matter-js/matterjs-server/issues) |

---

## 🌐 Network

| Setting | Value |
|---|---|
| Network mode | **host** (required – Matter uses IPv6 link-local multicast / mDNS) |
| Port | `5580` TCP – WebSocket API (`/ws`) and web dashboard |

There is no port mapping because the container uses the host network stack.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/matterjs-server` | `/data` | RW | Matter fabric data (node credentials, certificates). **Back this up** – losing it means re-pairing every device. |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone |
| `PRIMARY_INTERFACE` | `br0` | ❌ | Host interface for mDNS/Matter. Must have IPv6 enabled |
| `LISTEN_ADDRESS` | *(empty = all)* | ❌ | Bind address for port 5580. `127.0.0.1` = only local (recommended if HA runs on the same host with host networking) |
| `PORT` | `5580` | ❌ | WebSocket API / dashboard port |
| `LOG_LEVEL` | `info` | ❌ | `critical`, `error`, `warning`, `info`, `debug`, `verbose` |
| `ENABLE_TIME_SYNC` | `true` | ❌ | Sync time to Matter devices that support it |

More options (OTA, Bluetooth, DCL …): see the [upstream Docker guide](https://github.com/matter-js/matterjs-server/blob/main/docs/docker.md#environment-variables).

---

## ✅ Host Requirements (read before installing!)

Matter is built on **IPv6**. Most problems come from the host, not from the container.

### 1. IPv6 on the host interface

IPv6 must be **enabled** on the interface you set in `PRIMARY_INTERFACE` (e.g. `br0`):

```bash
# Must print 0 (= IPv6 enabled)
sysctl net.ipv6.conf.br0.disable_ipv6
# Should show at least an fe80:: link-local address
ip -6 addr show br0
```

If it prints `1`, enable IPv6 for this interface in the MOS network settings.

### 2. Thread routing (only needed for Thread devices)

For Thread devices behind a border router (HomePod, Apple TV, SLZB-MR5U …) the host must accept IPv6 route information from Router Advertisements:

| sysctl | Required value |
|---|---|
| `net.ipv6.conf.br0.accept_ra` | `1` |
| `net.ipv6.conf.br0.accept_ra_rt_info_max_plen` | `64` |
| `net.ipv6.conf.all.forwarding` | `0` |

```bash
sysctl net.ipv6.conf.br0.accept_ra net.ipv6.conf.br0.accept_ra_rt_info_max_plen net.ipv6.conf.all.forwarding
```

> These values must be **persistent** (survive reboot). Set them via the MOS network configuration or a boot script – `sysctl -w` alone is reset when the network service restarts.

### 3. Network equipment (e.g. UniFi)

- Matter devices, border routers, the phone used for pairing and the server must be in the **same (V)LAN**
- **Disable** "Multicast Enhancement" / multicast-to-unicast on the Wi-Fi networks used by Matter devices
- **Do not** use an mDNS reflector/forwarder ("mDNS" / "Multicast DNS" option in UniFi) for this network – it can corrupt Matter traffic
- If devices drop out, try toggling **IGMP/MLD snooping**

---

## 🚀 Quick Start

### Step 1: Check host requirements
Run the checks from the section above. IPv6 on `br0` is mandatory.

### Step 2: Prepare the data folder
The container runs **unprivileged as UID/GID 1000**:

```bash
mkdir -p /mnt/cache/appdata/matterjs-server
chown -R 1000:1000 /mnt/cache/appdata/matterjs-server
```

### Step 3: Install via MOS Hub
1. Open the **MOS Hub** and search for **Matter-Server**
2. Adjust the data path if your appdata lives elsewhere
3. Set `PRIMARY_INTERFACE` to your LAN interface (usually `br0`)
4. Optional: set `LISTEN_ADDRESS=127.0.0.1` if Home Assistant runs on the same host with host networking
5. Click **Install**

### Step 4: Verify
```bash
docker logs -f Matter-Server
docker inspect --format='{{.State.Health.Status}}' Matter-Server   # → healthy
```
Dashboard: `http://YOUR_SERVER_IP:5580` (only if `LISTEN_ADDRESS` is not `127.0.0.1`)

---

## 📊 Home Assistant Integration

1. **Settings → Devices & Services → Add Integration → Matter**
2. When asked for the server URL, enter:
   - same host, HA with host networking: `ws://localhost:5580/ws`
   - otherwise: `ws://YOUR_SERVER_IP:5580/ws`
3. For Thread devices: in the **Home Assistant iOS/Android app** go to *Settings → Companion App → Troubleshooting → Sync Thread credentials* so HA uses the same Thread network as your Apple/Google border routers.

### Adding devices (recommended: multi-admin)
1. Pair the device in its primary ecosystem first (e.g. **Apple Home**)
2. In Apple Home: device → *Settings* → **Turn On Pairing Mode** → copy the setup code
3. In Home Assistant: **Add Matter device** → *"It's already in use by another controller"* → enter the code

The device is then controlled natively by Apple Home **and** available in Home Assistant for automations.

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| HA can't connect to the server | Check `docker logs`, URL must end with `/ws`, firewall on port 5580 |
| Pairing fails / "device not found" | Phone, server and device in the same VLAN? IPv6 enabled on `br0`? mDNS reflector off? |
| Thread devices unavailable after a while | `accept_ra_rt_info_max_plen=64` set persistently? `all.forwarding=0`? |
| Permission denied on `/data` | `chown -R 1000:1000` on the data folder |
| Migrating from python-matter-server | Use the same data folder – data is migrated automatically on first start ([migration guide](https://github.com/matter-js/matterjs-server/blob/main/docs/migration.md)) |

---

## 💡 Best Practices

- **Back up** the data folder – it holds the fabric credentials
- Keep **one Thread network** (share credentials between Apple/Google and HA instead of creating a second one)
- Don't expose port 5580 to the internet – the API has **no authentication**