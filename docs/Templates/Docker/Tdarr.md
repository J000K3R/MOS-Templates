---
title: 🎬 Tdarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 28
---

# 🎬 Tdarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/tdarr.png" width="80" />

**Tdarr** is a distributed transcoding system for automating media library management.
It processes your media files in parallel across multiple nodes, supporting both CPU
and GPU (NVENC/VAAPI/QSV) transcoding with FFmpeg and HandBrake.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/haveagitgat/tdarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/haveagitgat/tdarr](https://github.com/haveagitgat/tdarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/haveagitgat/tdarr/issues) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8265` | TCP | Tdarr Web Interface |
| `8266` | TCP | Tdarr Server communication port (used by nodes) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/tdarr/server` | `/app/server` | RW | Tdarr Server database, plugins, and samples |
| `/mnt/cache/appdata/tdarr/configs` | `/app/configs` | RW | Server and Node configuration files |
| `/mnt/cache/appdata/tdarr/logs` | `/app/logs` | RW | Tdarr Server and Node log files |
| `/mnt/media` | `/media` | RW | Your media library (movies, TV shows) |
| `/mnt/cache/tdarr/transcode_cache` | `/temp` | RW | Temporary transcode cache (SSD recommended) |

---

## ⚙️ Environment Variables

| Name | Key | Default | Description |
|---|---|---|---|
| Server IP | `serverIP` | `0.0.0.0` | Tdarr Server bind address |
| Server Port | `serverPort` | `8266` | Tdarr Server communication port |
| WebUI Port | `webUIPort` | `8265` | Web interface port |
| Internal Node | `internalNode` | `true` | Enable Tdarr Node inside Server container |
| In Container | `inContainer` | `true` | Set to true when running inside Docker |
| FFmpeg Version | `ffmpegVersion` | `7` | FFmpeg version to use (6 or 7) |
| Node Name | `nodeName` | `MainNode` | Name for the internal Tdarr Node |
| Authentication | `auth` | `false` | Enable password protection for web UI |
| NVIDIA Driver Capabilities | `NVIDIA_DRIVER_CAPABILITIES` | `all` | NVIDIA GPU capabilities for NVENC |
| NVIDIA Visible Devices | `NVIDIA_VISIBLE_DEVICES` | `all` | Which NVIDIA GPUs to expose |
| PUID | `PUID` | `500` | User ID for file permissions |
| PGID | `PGID` | `500` | Group ID for file permissions |
| TZ | `TZ` | `Europe/Vienna` | Timezone for the container |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Tdarr**
2. Click **Install**
3. Access Tdarr at `http://your-server-ip:8265`
4. Configure your library paths in the web UI
5. Set up transcode plugins and workers

---

## 🎮 Hardware Transcoding

### NVIDIA NVENC
- Requires NVIDIA Container Toolkit on host
- Set `NVIDIA_DRIVER_CAPABILITIES=all` and `NVIDIA_VISIBLE_DEVICES=all`
- Remove the `/dev/dri` device if not using Intel/AMD

### Intel QSV / AMD VAAPI
- Uses the `/dev/dri` device mapping
- Remove NVIDIA environment variables if not using NVIDIA GPU
- For Intel, ensure iGPU is enabled in BIOS

---

## 🔧 Multiple Nodes Setup

Tdarr supports distributed transcoding across multiple machines:

1. **Server**: Runs the main Tdarr container (this template)
2. **Nodes**: Deploy additional Tdarr Node containers on other machines
3. Configure nodes to connect to the Server IP on port 8266

---

> 💡 **Tip:** Place your transcode cache on fast storage (SSD/NVMe) for better performance —
> Tdarr writes heavily to this directory during transcoding.

> ⚠️ **Note:** Ensure your `/mnt/media` path matches your Radarr/Sonarr paths for proper
> library integration and health checking.

> 📚 **For more information:** Visit the [Tdarr documentation](https://docs.tdarr.io/)
> for plugin configuration, node setup, and automation workflows.
