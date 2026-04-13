---
title: 🐳 Portainer CE
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 15
---

# 🐳 Portainer Community Edition

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/portainer.png" width="80" />

**Portainer CE** is a lightweight service delivery platform for containerized applications. It provides an intuitive web interface to manage Docker containers, images, networks, volumes, and more - all from a single dashboard.

Perfect for managing your MOS Hub Docker environment visually without command line.

🏷️ **Category:** Productivity

🐳 **Image:** `portainer/portainer-ce:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [portainer.io](https://www.portainer.io/) |
| 📖 **Docs** | [docs.portainer.io](https://docs.portainer.io/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/portainer/portainer/issues) |
| 💼 **Business** | [portainer.io/pricing](https://www.portainer.io/pricing) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9443` | TCP | Portainer web interface (HTTPS) - **Primary** |
| `8000` | TCP | Edge Agent - for remote Docker environment management |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/portainer` | `/data` | RW | Portainer data, settings, and configuration |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RW | Docker socket for local container management |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Portainer**
2. Click **Install**
3. Access Portainer at `https://YOUR_SERVER_IP:9443`
   ⚠️ Note: Uses HTTPS, you may see a certificate warning initially

### Step 2: Initial Setup

1. Create an admin account (username + strong password)
2. Select your environment:
   - **Local** - Manage the Docker host Portainer runs on (recommended for MOS Hub)
   - **Remote** - Connect to other Docker hosts
   - **Agent** - Use with Portainer Agent on remote hosts
3. Click **Connect**

### Step 3: Start Managing Docker

Once logged in, you can:
- View all containers, images, networks, volumes
- Start/stop/restart containers
- View container logs and stats
- Access container consoles
- Deploy new containers from templates or images

---

## ✨ Key Features

### Container Management
- **Dashboard** - Overview of all containers with status
- **Live Stats** - CPU, memory, network I/O in real-time
- **Logs** - View and search container logs
- **Console** - Web-based terminal access to containers
- **Recreate** - Pull new image and recreate container

### Image Management
- **Pull Images** - Download from Docker Hub or private registries
- **Build Images** - Build from Dockerfile
- **Image List** - View all images, remove unused
- **Registry Management** - Connect to private registries

### Volume & Network
- **Volumes** - Create, inspect, remove volumes
- **Networks** - Manage Docker networks
- **Stack** - Deploy Docker Compose stacks

### App Templates
- Pre-built templates for popular applications
- Custom template repositories
- Deploy with one click

---

## 🔗 Connecting to MOS Hub

Portainer can manage your MOS Hub Docker environment:

### Local Environment (Recommended)
The default setup with Docker socket mounted at `/var/run/docker.sock` allows Portainer to manage the local Docker instance where MOS Hub runs.

### View MOS Hub Containers
1. Go to **Containers** in Portainer
2. You'll see all MOS Hub containers
3. You can start/stop/restart them (⚠️ careful not to break dependencies)

---

## 🔐 Security Best Practices

1. **Strong Admin Password** - Set during first setup
2. **HTTPS Only** - Portainer uses self-signed HTTPS by default
3. **Internal Access** - Keep Portainer on internal network if possible
4. **Authentication** - Enable external auth (LDAP, OAuth) in settings for teams
5. **Role-Based Access** - Create users with appropriate permissions
6. **Edge Agent** - Use for remote hosts instead of exposing Docker socket

## ⚠️ MOS Hub Integration Warning

> **Read-Only Usage Recommended:** When using Portainer alongside MOS Hub, treat Portainer as a **monitoring and debugging tool only**.
>
> **DO NOT:**
> - Create new containers in Portainer
> - Edit existing MOS Hub containers in Portainer
> - Delete containers through Portainer
>
> **WHY:** Containers created or modified in Portainer won't have entries in MOS Hub's `docker.json` configuration, causing them to be invisible to the MOS Webinterface. Always use MOS Hub for container lifecycle management.
>
> **OK to do in Portainer:**
> - View container status, logs, and stats
> - Access container consoles for debugging
> - Inspect container configuration
> - View resource usage graphs

---

## 🆚 Portainer CE vs BE (Business Edition)

| Feature | CE (Free) | BE (Paid) |
|---------|-----------|-----------|
| **Price** | Free | $199/5 nodes/year |
| **Container Management** | ✅ | ✅ |
| **Kubernetes** | ✅ | ✅ |
| **GitOps** | ❌ | ✅ |
| **RBAC** | Basic | Advanced |
| **SSO** | ❌ | ✅ |
| **Registry Scanning** | ❌ | ✅ |
| **Support** | Community | Official |

For most home/homelab users, **CE is sufficient**.

---

## 📱 Mobile App

Portainer has an official mobile app:
- **iOS** - [App Store](https://apps.apple.com/us/app/portainer/id1543334694)
- **Android** - [Play Store](https://play.google.com/store/apps/details?id=io.portainer.app)

Connect to your Portainer instance and manage containers on the go!

---

## 🔧 Advanced Configuration

### Reverse Proxy (Nginx Proxy Manager)

```nginx
location / {
    proxy_pass https://portainer:9443;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_ssl_verify off;
}
```

⚠️ Note: `proxy_ssl_verify off` is needed for Portainer's self-signed cert.

### Backup Portainer Data

Backup the data folder:
```bash
tar -czf portainer-backup.tar.gz /mnt/cache/appdata/portainer/
```

---

## 📝 Troubleshooting

### Certificate Warning
Portainer uses a self-signed certificate by default. You can:
- Accept the warning in your browser
- Or put Portainer behind a reverse proxy with valid SSL

### Cannot Access Docker Socket
Ensure the Docker socket is mounted correctly and PUID/PGID have permissions:
```bash
ls -la /var/run/docker.sock
```

### Forgot Admin Password
Reset via command line:
```bash
docker exec -it portainer /bin/sh
portainer-reset-password
```

---

> ⚠️ **Warning:** Be careful when managing MOS Hub containers through Portainer. Stopping essential containers (like MOS Hub itself) can break your setup!

> � **IMPORTANT - Read Only Usage:** Do **NOT** create or edit containers through Portainer when using MOS Hub! Containers created in Portainer will not appear in the MOS Webinterface because they lack the corresponding entry in the `docker.json` configuration file. Use Portainer only for **viewing** container status, logs, and stats - manage all container lifecycle operations (create, edit, delete) through the MOS Hub interface instead.

> �� **Tip:** Use Portainer's **Stacks** feature to deploy Docker Compose configurations alongside MOS Hub templates.

> 💡 **Tip:** Enable **Dark Mode** in Portainer settings for a more comfortable viewing experience.

> 📚 **For more information:** Visit the [Portainer Documentation](https://docs.portainer.io/) for detailed guides on Kubernetes, Swarm, and advanced features.
