---
layout: default
title: BambuStudio
parent: Docker Templates
nav_order: 5
---

# 🖨️ BambuStudio

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/bambustudio.png" width="80" />

Bambu Studio is an open-source, cutting-edge, feature-rich slicing software for Bambu Lab 3D printers. It contains project-based workflows, systematically optimized slicing algorithms, and an easy-to-use graphical interface, bringing users an incredibly smooth printing experience.

## 📋 Quick Info

| | |
|---|---|
| **Image** | `lscr.io/linuxserver/bambustudio:latest` |
| **Web UI** | `https://[IP]:3001` |
| **Category** | Media |
| **Project** | [linuxserver/docker-bambustudio](https://github.com/linuxserver/docker-bambustudio) |
| **Support** | [GitHub Issues](https://github.com/linuxserver/docker-bambustudio/issues) |
| **Docs** | [LinuxServer Docs](https://docs.linuxserver.io/images/docker-bambustudio/) |

## 🐳 Docker Template

### Ports

| Port | Protocol | Description | Required |
|------|----------|-------------|----------|
| `3000` | TCP | Bambu Studio desktop GUI (HTTP) | ✅ |
| `3001` | TCP | Bambu Studio desktop GUI (HTTPS) | ✅ |

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PUID` | `500` | UserID — use `id your_user` to find yours |
| `PGID` | `500` | GroupID — use `id your_user` to find yours |
| `TZ` | `Etc/UTC` | Timezone (e.g. `Europe/Vienna`) |
| `DARK_MODE` | `true` | Enable dark mode UI (optional) |
| `CUSTOM_USER` | `abc` | HTTP Basic auth username (optional) |
| `PASSWORD` | `abc` | HTTP Basic auth password (optional, if unset = no auth) |
| `LC_ALL` | — | Language locale (e.g. `de_DE.UTF-8`) |

### Volumes

| Container Path | Host Path | Description | Required |
|----------------|-----------|-------------|----------|
| `/config` | `/mnt/cache/appdata/bambustudio` | Users home directory, stores settings and files | ✅ |

### Extra Parameters

| Parameter | Description |
|-----------|-------------|
| `--shm-size=1gb` | ⚠️ **Required** for all desktop GUI containers (Kasm/Selkies VNC) |
| `--restart=unless-stopped` | Auto-restart container |

## 🔧 Setup Instructions

1. Deploy the container via MOS Hub
2. Wait ~30 seconds for the desktop to initialize
3. Access the Web UI at `https://[MOS-IP]:3001`
4. Accept the self-signed certificate warning in your browser
5. Bambu Studio launches automatically

## 🔐 Security Notes

{: .warning }
This container provides **privileged access** to the host system. Do not expose it to the Internet without proper authentication.

- By default, there is **no authentication** — anyone with network access can use it
- Set `CUSTOM_USER` and `PASSWORD` for basic HTTP auth on a trusted local network
- For internet exposure, use a **reverse proxy** (e.g. SWAG/Caddy) with proper authentication
- The web interface includes a **terminal with passwordless sudo** — anyone with GUI access has root in the container

## 🖥️ Hardware Acceleration (Optional)

### Intel / AMD GPU

Add to extra parameters:
```
--device /dev/dri:/dev/dri
```

Environment variables:
- `PIXELFLUX_WAYLAND=true` (enables Wayland + Zero Copy encoding)
- `DRINODE=/dev/dri/renderD128` (Rendering GPU)
- `DRI_NODE=/dev/dri/renderD128` (Encoding GPU)

If both point to the same device, Zero Copy encoding is enabled automatically.

### Nvidia GPU

Requirements:
- Proprietary driver 580+ 
- `nvidia-drm.modeset=1 nvidia_drm.fbdev=1` in host bootloader
- Nvidia Docker runtime configured

Add to extra parameters:
```
--gpus all --runtime nvidia
```

## 🌐 Language Support

Set `LC_ALL` to change the desktop language:

| Language | Value |
|----------|-------|
| German | `de_DE.UTF-8` |
| French | `fr_FR.UTF-8` |
| Italian | `it_IT.UTF-8` |
| Spanish | `es_MX.UTF-8` |
| Chinese | `zh_CN.UTF-8` |
| Japanese | `ja_JP.UTF-8` |
| Russian | `ru_RU.UTF-8` |

## 📦 PRoot Apps (Persistent)

Natively installed packages (e.g. `apt-get install`) will **not persist** if the container is recreated. Use `proot-apps` for persistent installations:

```bash
proot-apps install filezilla
```

[Supported PRoot Apps list](https://github.com/linuxserver/proot-apps)

## 🔄 Updating

### Via MOS Hub
Simply update the image and recreate the container. Your `/config` volume preserves all settings.

### Via Docker CLI
```bash
docker pull lscr.io/linuxserver/bambustudio:latest
docker stop bambustudio
docker rm bambustudio
# Recreate with same parameters
```

## 📝 Docker Compose Reference

```yaml
---
services:
  bambustudio:
    image: lscr.io/linuxserver/bambustudio:latest
    container_name: bambustudio
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Vienna
      - DARK_MODE=true
    volumes:
      - /mnt/cache/appdata/bambustudio:/config
    ports:
      - 3000:3000
      - 3001:3001
    shm_size: "1gb"
    restart: unless-stopped
```

## ⚠️ Known Limitations

- **No ARM64 support** — x86-64 only
- **Self-signed certificate** — browser will show a security warning (expected)
- **HTTPS required** — HTTP (port 3000) has limited functionality (no WebCodecs for video/audio)
- **No Bambu Lab printer connection** — this is a slicing tool only, not a printer host
- **Wayland is default** since 11.04.26 — falls back to X11 on CPUs without AVX2
