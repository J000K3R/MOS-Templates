---
title: 📊 Dashdot
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 8
---

# 📊 Dashdot

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/dashdot.png" width="80" />

**Dashdot** is a modern, beautiful server dashboard that displays system information like CPU, RAM, storage, network, and GPU usage in real-time. It features a sleek, customizable interface with support for multiple widgets and themes.

Perfect for monitoring your MOS Hub server at a glance, whether on a large monitor or mobile device.

🏷️ **Category:** Monitoring

🐳 **Image:** `mauricen/dashdot:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [getdashdot.com](https://getdashdot.com/) |
| 📖 **Docs** | [getdashdot.com/docs](https://getdashdot.com/docs) |
| 🐛 **Support** | [GitHub Issues](https://github.com/MauriceNino/dashdot/issues) |
| 💛 **Sponsor** | [GitHub Sponsors](https://github.com/sponsors/MauriceNino) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3001` | TCP | Dashdot dashboard web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/` | `/mnt/host` | RO | Host root filesystem for gathering system statistics |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🎨 Display & Layout

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DASHDOT_WIDGET_LAYOUT` | `grid` | ❌ | Widget layout: `grid`, `list`, or custom order |

### 📊 Widgets (Enable/Disable)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DASHDOT_OS_WIDGET_ENABLE` | `true` | ❌ | OS information (distro, kernel, uptime) |
| `DASHDOT_CPU_WIDGET_ENABLE` | `true` | ❌ | CPU usage and temperature |
| `DASHDOT_STORAGE_WIDGET_ENABLE` | `true` | ❌ | Storage usage and health |
| `DASHDOT_RAM_WIDGET_ENABLE` | `true` | ❌ | RAM usage |
| `DASHDOT_NETWORK_WIDGET_ENABLE` | `true` | ❌ | Network speed and usage |
| `DASHDOT_GPU_WIDGET_ENABLE` | `false` | ❌ | GPU monitoring (enable only if you have GPU) |
| `DASHDOT_ACCEPT_IGD` | `true` | ❌ | Allow internal GPU device access |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Dashdot**
2. Click **Install**
3. Access Dashdot at `http://YOUR_SERVER_IP:3001`

That's it! Dashdot will automatically detect and display your system information.

---

## 🎨 Customization

### Widget Layout Options

Change `DASHDOT_WIDGET_LAYOUT` to customize the display:

| Layout | Description |
|---|---|
| `grid` | Default 2x3 grid layout |
| `list` | Vertical list layout |
| Custom | Comma-separated order: `os,cpu,storage,ram,network,gpu` |

### Example Custom Layout

```
os,cpu,ram,storage,network,gpu
```

This shows widgets in the specified order from left to right, top to bottom.

---

## 📊 Widget Overview

### OS Widget
- **Distribution** - Linux distribution name and version
- **Kernel** - Kernel version
- **Architecture** - CPU architecture (x86_64, ARM, etc.)
- **Uptime** - System uptime
- **Hostname** - Server hostname

### CPU Widget
- **Usage Graph** - Real-time CPU usage percentage
- **Core Count** - Number of CPU cores/threads
- **Temperature** - CPU temperature (if sensors available)
- **Model** - CPU model name
- **Frequency** - Current clock speed

### Storage Widget
- **Usage Graph** - Storage space visualization
- **Drive Health** - SMART status (if available)
- **Mount Points** - Disk usage per mount
- **RAID Info** - RAID array status

### RAM Widget
- **Usage Graph** - Memory usage visualization
- **Total/Used/Free** - Detailed memory statistics
- **Swap Usage** - Swap space monitoring

### Network Widget
- **Speed Graph** - Real-time up/down speeds
- **Interface Info** - Network interface details
- **Total Traffic** - Cumulative data usage
- **Public IP** - External IP address

### GPU Widget (Optional)
- **Usage Graph** - GPU utilization
- **VRAM Usage** - Video memory statistics
- **Temperature** - GPU temperature
- **Model** - GPU model name

---

## 🎨 Themes & Appearance

Dashdot automatically adapts to your system theme preference (dark/light mode) via CSS media queries.

### Browser/OS Theme Support
- **Dark Mode** - Automatically switches to dark theme
- **Light Mode** - Automatically switches to light theme
- **Manual Override** - Use browser dev tools or extensions to force a theme

---

## 🔒 Security Considerations

### Privileged Mode Required
⚠️ **Warning**: Dashdot requires `--privileged` mode and access to host root (`/`) to gather system information:
- Read CPU, RAM, storage statistics
- Access hardware sensors
- Monitor network interfaces

### Read-Only Access
The container only has **read-only** access to the host filesystem (`ro` mode), minimizing security risks.

### Network Access
- Keep Dashdot on internal network if possible
- Use reverse proxy with authentication for external access
- Consider pairing with Authelia/Authentik for public exposure

---

## 🛠️ Advanced Configuration

### Storage Filtering
To show only specific storage devices, use additional environment variables (add via MOS Hub):

```
DASHDOT_STORAGE_FILTER=mnt/host/cache,mnt/host/array
```

### Network Interface Selection
To monitor specific network interfaces:

```
DASHDOT_NETWORK_INTERFACE=eth0
```

### Custom Refresh Rate
Change how often widgets update (in milliseconds):

```
DASHDOT_REFRESH_RATE=1000
```

---

## 🌐 Reverse Proxy Setup

### Nginx Proxy Manager

```nginx
location / {
    proxy_pass http://dashdot:3001;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

### Authelia/Authentik Protection
Since Dashdot has no built-in authentication, protect it with:
- **Authelia** - Add to your protected endpoints
- **Authentik** - Configure as a managed application
- **NPM Access Lists** - Basic auth via Nginx Proxy Manager

---

## 📱 Mobile Access

Dashdot is fully responsive and works great on mobile browsers:
- **Smartphone** - Scrollable dashboard
- **Tablet** - Optimized grid layout
- **Desktop** - Full dashboard experience

---

## 📝 Troubleshooting

### Widget Shows "No Data"
1. Check if privileged mode is enabled
2. Verify host root mount is accessible: `/mnt/host`
3. Check container logs: `docker logs dashdot`

### Temperature Not Showing
- Install `lm-sensors` on the host system
- Some CPUs don't expose temperature sensors to containers

### Storage Shows Wrong Values
- Verify the host root mount: `-v /:/mnt/host:ro`
- Check if ZFS/BTRFS special filesystems need additional config

### High CPU Usage
- Reduce refresh rate: `DASHDOT_REFRESH_RATE=5000` (5 seconds)
- Disable unused widgets

---

> ⚠️ **Note:** Dashdot requires privileged access to read hardware information. Only deploy on hosts you control and trust.

> 💡 **Tip:** Use Dashdot alongside Beszel for a complete monitoring solution - Dashdot for quick overview, Beszel for historical trends and alerts.

> 💡 **Tip:** Place your most important widgets first using custom layout for quick mobile access.

> 💡 **Tip:** Disable the GPU widget if you don't have a dedicated GPU to reduce resource usage.

> 📚 **For more information:** Visit the [Dashdot Documentation](https://getdashdot.com/docs) for advanced configuration, API access, and customization options.
