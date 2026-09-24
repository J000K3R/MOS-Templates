---
title: 🔐 Gluetun
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 23.5
---

# 🔐 Gluetun

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/gluetun.png" width="80" />

**Gluetun** is a VPN client in a single, lightweight Docker container supporting **many providers** (Mullvad, ProtonVPN, NordVPN, ExpressVPN, …) over **WireGuard** or **OpenVPN**. It ships with a built-in **kill switch and firewall** — containers routed through Gluetun are automatically blocked the moment the tunnel drops, so no traffic ever leaks outside the VPN.

🏷️ **Category:** Network / VPN

🐳 **Image:** `qmcgaw/gluetun:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/qmcgaw/gluetun](https://github.com/qmcgaw/gluetun) |
| 🐛 **Support** | [GitHub Issues](https://github.com/qmcgaw/gluetun/issues) |
| 📖 **Docs** | [github.com/qmcgaw/gluetun-wiki](https://github.com/qmcgaw/gluetun-wiki) |

---

## 🌐 Ports

No ports of their own — Gluetun is a **gateway**, not a service. Instead, **route other containers through it** (see *Usage* below). If your media apps need a port reachable from the LAN while going through the VPN (e.g. a torrent WebUI), expose those ports on Gluetun instead.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/gluetun` | `/gluetun` | RW | Gluetun configuration and helper scripts. |

---

## ⚙️ Environment Variables

### 🌍 Connection

| Variable | Default | Masked | Description |
|---|---|---|---|
| `VPN_SERVICE_PROVIDER` | `mullvad` | ❌ | VPN provider. Supported: `mullvad`, `protonvpn`, `nordvpn`, `expressvpn`, `private internet access`, `surfshark` and many more. |
| `VPN_TYPE` | `wireguard` | ❌ | Protocol: `wireguard` or `openvpn`. |
| `WIREGUARD_PRIVATE_KEY` | *(empty)* | ✅ | Your WireGuard **PrivateKey** (base64). Pull it from a generated Mullvad config file. Identical for all Mullvad servers. |
| `WIREGUARD_ADDRESSES` | *(empty)* | ❌ | Your WireGuard IP in CIDR (e.g. `10.64.222.21/32`). From the generated Mullvad config. Identical for all Mullvad servers. |
| `SERVER_COUNTRIES` | `Austria,Germany,Switzerland` | ❌ | Comma-separated countries to connect to (picked randomly). |
| `SERVER_CITIES` | `Vienna,Graz,Frankfurt,Munich,Zurich` | ❌ | Comma-separated cities to connect to. |
| `TZ` | `Europe/Vienna` | ❌ | Container timezone. |

> 💡 **Mullvad tip:** every Mullvad server config shares the **same** `PrivateKey` and **IPv4 Address`. So you only set those two values once — the `SERVER_*` variables decide *where* it connects.

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Gluetun**
2. Enter your **WireGuard Private Key** and **WireGuard Addresses** (from a generated Mullvad config)
3. Optionally adjust `SERVER_COUNTRIES` / `SERVER_CITIES`
4. Ensure `/dev/net/tun` exists on the host (`ls /dev/net/tun`)
5. Click **Install**
6. Verify the tunnel: open the container logs for a line like **`VPN service ready`**, or check the assigned public IP.

---

## 🔗 Usage — routing other containers through the VPN

Gluetun is a **gateway**, so the actual benefit comes from sending other apps through it:

1. Note Gluetun's container name, e.g. `gluetun`
2. Deploy the app that should use the VPN (e.g. a torrent client, Sportarr, …)
3. Run it with `network_mode: "container:gluetun"` (Compose) or `--network=container:gluetun` (docker run)
4. Configure the app's **proxy / DNS** via the `[IP]:[PORT]` endpoints Gluetun exposes:

| Endpoint | Purpose |
|---|---|
| `8888` | HTTP proxy |
| `1080` | SOCKS5 proxy |
| `8388` | Shadowsocks (if enabled) |
| `53` | DNS-over-TLS to your VPN provider |

5. **Kill switch = automatic:** if the tunnel drops, the firewall inside Gluetun blocks all traffic of connected containers until the VPN reconnects.

---

## 🧭 Provider Config Examples

### **Mullvad (WireGuard)** — recommended

| Variable | Value |
|---|---|
| `VPN_SERVICE_PROVIDER` | `mullvad` |
| `VPN_TYPE` | `wireguard` |
| `WIREGUARD_PRIVATE_KEY` | `*your private key*` |
| `WIREGUARD_ADDRESSES` | `10.64.222.21/32` |
| `SERVER_COUNTRIES` | `Austria` |

Mullvad lets multiple devices share one config (same key + address).

### **ProtonVPN (WireGuard)**

| Variable | Value |
|---|---|
| `VPN_SERVICE_PROVIDER` | `protonvpn` |
| `VPN_TYPE` | `wireguard` |
| `WIREGUARD_PRIVATE_KEY` | `*your private key*` |
| `SERVER_COUNTRIES` | `Switzerland` |

### **NordVPN (OpenVPN)**

| Variable | Value |
|---|---|
| `VPN_SERVICE_PROVIDER` | `nordvpn` |
| `VPN_TYPE` | `openvpn` |
| `OPENVPN_USER` | `*service username*` |
| `OPENVPN_PASSWORD` | `*service password*` (masked) |
| `SERVER_COUNTRIES` | `Austria` |

---

## 🧪 Image Tags

| Tag | Purpose |
|---|---|
| `latest` | Latest stable build (recommended) |
| `vX.Y.Z` | Pinned release |
| `sha-…` | Development / nightly builds |

---

> ⚠️ **NET_ADMIN + TUN required:** Gluetun needs the `NET_ADMIN` capability (already added via `--cap-add=NET_ADMIN` in this template) and a working `/dev/net/tun`. If `/dev/net/tun` is missing on the host it can usually be created with `mkdir -p /dev/net && mknod /dev/net/tun c 10 200 && chmod 600 /dev/net/tun`.

> 💡 **Tip:** After a server/container restart Gluetun randomly re-picks a server from your `SERVER_COUNTRIES`/`SERVER_CITIES` — perfect for rotating to a healthy node if the "no public IP after restart" issue appears.

> 🔍 **Verify it works:** `docker exec gluetun sh -c "wget -qO- https://api.ipify.org"` should return the VPN's **public IP**, not your home IP.

> 📚 **For more information:** See the [Gluetun Wiki](https://github.com/qmcgaw/gluetun-wiki) for the full list of providers, OpenVPN credentials and advanced firewall settings.
