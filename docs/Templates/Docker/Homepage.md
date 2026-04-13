---
title: 🏠 Homepage
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 12
---

# 🏠 Homepage

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/homepage.png" width="80" />

**Homepage** is a modern, fully static, fast, and secure dashboard for your server. It integrates with various services like Docker, Plex, Sonarr, Radarr, and many more to provide a comprehensive overview of your infrastructure.

Features include service status monitoring, real-time widgets, bookmarks, and a beautiful, customizable interface that puts all your important information in one place.

🏷️ **Category:** Productivity

🐳 **Image:** `ghcr.io/gethomepage/homepage:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [gethomepage.dev](https://gethomepage.dev/) |
| 📖 **Docs** | [gethomepage.dev](https://gethomepage.dev/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/gethomepage/homepage/issues) |
| 💛 **Sponsor** | [GitHub Sponsors](https://github.com/sponsors/gethomepage) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3000` | TCP | Homepage dashboard web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/homepage/config` | `/app/config` | RW | Configuration files (bookmarks, services, settings, widgets) |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RO | Docker socket for container monitoring |
| `/mnt/cache/appdata/homepage/icons` | `/app/public/icons` | RO | Optional custom icons folder |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🔒 Security

| Variable | Default | Masked | Description |
|---|---|---|---|
| `HOMEPAGE_ALLOWED_HOSTS` | `localhost:3000,127.0.0.1:3000` | ❌ | Allowed hosts for CORS/security. Add your domain when using reverse proxy. |

---

## 🚀 Quick Start

### Step 1: Prepare Directories

Create the configuration directories:
```bash
mkdir -p /mnt/cache/appdata/homepage/config
mkdir -p /mnt/cache/appdata/homepage/icons
```

### Step 2: Install via MOS Hub

1. Open the **MOS Hub** and search for **Homepage**
2. Ensure `PUID`/`PGID` can read your Docker socket
3. Click **Install**
4. Access Homepage at `http://YOUR_SERVER_IP:3000`

### Step 3: Configure Allowed Hosts (Important!)

If using a reverse proxy (Nginx Proxy Manager, Traefik, etc.), update the `HOMEPAGE_ALLOWED_HOSTS` environment variable:

```
localhost:3000,127.0.0.1:3000,dashboard.yourdomain.com
```

Without this, Homepage will show "Invalid Host" errors when accessing via domain.

### Step 4: Initial Configuration

On first start, Homepage creates default config files. Edit them to customize:
```bash
cd /mnt/cache/appdata/homepage/config
```

---

## 📁 Configuration Files

Homepage uses YAML configuration files in `/app/config`:

### books.yaml
```yaml
- Developer:
    - Github:
        - abbr: GH
          href: https://github.com/

- Social:
    - Reddit:
        - abbr: RE
          href: https://reddit.com/
```

### services.yaml
```yaml
- Media:
    - Plex:
        href: http://plex:32400
        description: Media Server
        widget:
          type: plex
          url: http://plex:32400
          key: YOUR_PLEX_TOKEN

- Download:
    - Sonarr:
        href: http://sonarr:8989
        widget:
          type: sonarr
          url: http://sonarr:8989
          key: YOUR_SONARR_API_KEY
```

### widgets.yaml
```yaml
- resources:
    cpu: true
    memory: true
    disk: /mnt/cache

- search:
    provider: duckduckgo
    target: _blank

- greeting:
    text_size: xl
    text: "Welcome Home!"
```

### settings.yaml
```yaml
title: My Dashboard
theme: dark
color: slate
language: de
```

---

## 🔌 Supported Service Widgets

Homepage supports 100+ service integrations:

### Media
| Service | Widget Features |
|---|---|
| **Plex** | Now playing, library stats |
| **Jellyfin** | Now playing, library stats |
| **Sonarr** | Queued, missing episodes |
| **Radarr** | Queued, missing movies |
| **Lidarr** | Queued, missing albums |
| **Bazarr** | Missing subtitles |
| **Tautulli** | Stream stats |
| **Overseerr** | Requests, issues |
| **Prowlarr** | Indexer status |

### Download
| Service | Widget Features |
|---|---|
| **qBittorrent** | Download speed, active torrents |
| **Transmission** | Download speed, active torrents |
| **SABnzbd** | Queue status, speed |
| **NZBGet** | Queue status, speed |
| **Deluge** | Download speed, active torrents |

### Monitoring
| Service | Widget Features |
|---|---|
| **Portainer** | Container count, status |
| **Docker** | Container stats (via socket) |
| **Uptime Kuma** | Monitor status |
| **Grafana** | Dashboard links |
| **Prometheus** | Metric queries |
| **Pi-hole** | Query stats, blocked queries |

### Network & Security
| Service | Widget Features |
|---|---|
| **AdGuard Home** | Query stats |
| **WireGuard** | Connected peers |
| **Nginx Proxy Manager** | Proxy host count |
| **Cloudflare** | Zone stats |
| **Traefik** | Router count |

---

## 🎨 Themes & Customization

### Built-in Themes
- `light` - Light mode
- `dark` - Dark mode (default)

### Color Schemes
- `slate`, `gray`, `zinc`, `neutral`, `stone`
- `red`, `orange`, `amber`, `yellow`
- `green`, `emerald`, `teal`, `cyan`
- `blue`, `indigo`, `violet`, `purple`, `fuchsia`, `pink`, `rose`

### Custom CSS
Add custom styles in `custom.css` in the config directory.

---

## 🔐 Security Best Practices

1. **Authentication** - Put Homepage behind Authelia/Authentik if exposing externally
2. **API Keys** - Store in widgets.yaml (not committed to Git)
3. **Docker Socket** - Only read-only access, consider Docker Proxy for production
4. **HTTPS** - Use reverse proxy with SSL for external access
5. **Internal Network** - Keep Homepage on internal network if possible

---

## 🔗 Comparison: Homepage vs Homarr vs Dashy

| Feature | Homepage | Homarr | Dashy |
|---------|----------|--------|-------|
| **Speed** | ⭐⭐⭐⭐⭐ (Static) | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Widgets** | 100+ | Many | Fewer |
| **Customization** | YAML Config | UI-based | YAML + UI |
| **Icons** | Built-in + Custom | Built-in | Custom |
| **Docker** | Native integration | Via integration | Manual |
| **Mobile** | Responsive | Responsive | Responsive |
| **Best For** | Power users | Beginners | Heavy customization |

---

## 🛠️ Docker Integration

Homepage automatically detects and monitors containers:

```yaml
# In services.yaml, reference by container name
- Media:
    - Plex:
        container: plex
        widget:
          type: plex
          url: http://plex:32400
          key: YOUR_TOKEN
```

### Labels for Auto-Discovery
Add labels to your Docker containers for automatic service discovery:
```yaml
labels:
  - homepage.group=Media
  - homepage.name=Plex
  - homepage.icon=plex.png
  - homepage.href=http://plex:32400
```

---

> ⚠️ **Note:** Homepage is a static site - it cannot write to your services, only read from them via APIs.

> 💡 **Tip:** Start with the default configuration and gradually add services. Widgets will show "API Error" until properly configured.

> 💡 **Tip:** Use **Docker labels** for automatic service discovery - no need to manually edit services.yaml!

> 💡 **Tip:** The **search widget** supports multiple providers: DuckDuckGo, Google, Bing, Baidu, Brave, and custom.

> 📚 **For more information:** Visit the [Homepage Documentation](https://gethomepage.dev/) for detailed widget configuration, API setup, and advanced features.
