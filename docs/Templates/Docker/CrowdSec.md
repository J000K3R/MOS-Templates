---
title: 🛡️ CrowdSec
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 3
---

# 🛡️ CrowdSec

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/crowdsec.png" width="80" />

**CrowdSec** is an open-source, lightweight, and collaborative intrusion detection/prevention system. It analyzes logs, detects malicious behavior, shares threat intelligence with the community, and can block IPs through integration with bouncers.

Unlike traditional firewalls, CrowdSec works by analyzing application logs and using a crowd-sourced threat intelligence network to protect your infrastructure from attacks.

🏷️ **Category:** Security

🐳 **Image:** `crowdsecurity/crowdsec:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/crowdsecurity/crowdsec](https://github.com/crowdsecurity/crowdsec) |
| 🐛 **Support** | [GitHub Issues](https://github.com/crowdsecurity/crowdsec/issues) |
| 📖 **Docs** | [docs.crowdsec.net](https://docs.crowdsec.net/) |
| 🌐 **Hub** | [app.crowdsec.net](https://app.crowdsec.net/hub) |
| 📊 **Console** | [app.crowdsec.net](https://app.crowdsec.net/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | CrowdSec Local API (LAPI) for bouncers to query decisions |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/crowdsec/config` | `/etc/crowdsec` | RW | Configuration, parsers, scenarios, collections |
| `/mnt/cache/appdata/crowdsec/data` | `/var/lib/crowdsec/data` | RW | Database, local API decisions, CLI data |
| `/var/log` | `/var/log` | RO | Host system logs for analysis |

---

## ⚙️ Environment Variables

### 📦 Collections & Hub

| Variable | Default | Masked | Description |
|---|---|---|---|
| `COLLECTIONS` | `crowdsecurity/linux crowdsecurity/nginx crowdsecurity/sshd` | ❌ | Collections to auto-install (space-separated) |
| `ENROLL_KEY` | `` | ✅ | Enrollment key from CrowdSec Console |
| `ENROLL_INSTANCE_NAME` | `mos-server` | ❌ | Instance name shown in Console |

### 🔧 Agent/LAPI Mode

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DISABLE_AGENT` | `false` | ❌ | Disable log parser (only LAPI mode) |
| `DISABLE_API` | `false` | ❌ | Disable LAPI (only agent mode, connects to remote) |
| `LEVEL_INFO` | `false` | ❌ | Enable verbose logging |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **CrowdSec**
2. Keep default collections or customize for your services
3. Click **Install**
4. CrowdSec will auto-install the configured collections

### Step 2: Register with CrowdSec Console (Optional but Recommended)

Get your enrollment key:
1. Create account at [app.crowdsec.net](https://app.crowdsec.net/)
2. Go to **Instances** → **Add Instance**
3. Copy the enrollment key
4. Set it as `ENROLL_KEY` in the container environment
5. Restart the container

### Step 3: Install Bouncers

CrowdSec detects threats but doesn't block them alone. You need a **bouncer**:

| Bouncer | Use Case | Install |
|---|---|---|
| **Firewall Bouncer** | Block at iptables/nftables level | Run on host |
| **Nginx Bouncer** | Block in Nginx reverse proxy | Install in nginx container |
| **Cloudflare Bouncer** | Block at CDN level | Run as separate container |
| **Docker Bouncer** | Block Docker containers | Run as separate container |

Example: Install Nginx Bouncer for Nginx Proxy Manager:
```bash
docker exec crowdsec cscli bouncers add nginx-bouncer
```

---

## 🔧 Common Commands

### Check CrowdSec Status

```bash
docker exec -it crowdsec cscli metrics
```

### List Installed Collections

```bash
docker exec -it crowdsec cscli collections list
```

### Install Additional Collections

```bash
# For Docker log parsing
docker exec -it crowdsec cscli collections install crowdsecurity/docker

# For specific services
docker exec -it crowdsec cscli collections install crowdsecurity/nextcloud
docker exec -it crowdsec cscli collections install crowdsecurity/pgsql
```

### View Alerts & Decisions

```bash
# Recent alerts
docker exec -it crowdsec cscli alerts list

# Current ban decisions
docker exec -it crowdsec cscli decisions list

# Ban an IP manually
docker exec -it crowdsec cscli decisions add --ip 192.168.1.100 --duration 4h --reason "Manual ban"

# Unban an IP
docker exec -it crowdsec cscli decisions delete --ip 192.168.1.100
```

### Monitor Logs

```bash
docker logs -f crowdsec
```

---

## 📊 Popular Collections

| Collection | Description |
|---|---|
| `crowdsecurity/linux` | System log parsing (syslog, auth) |
| `crowdsecurity/nginx` | Nginx access/error log parsing |
| `crowdsecurity/apache2` | Apache log parsing |
| `crowdsecurity/sshd` | SSH brute force detection |
| `crowdsecurity/nextcloud` | Nextcloud security events |
| `crowdsecurity/wordpress` | WordPress attack detection |
| `crowdsecurity/docker` | Docker daemon log parsing |

---

## 🔗 Integration Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Services      │────▶│   CrowdSec      │────▶│   Bouncers      │
│   (nginx, ssh)  │     │   (detects)     │     │   (blocks)      │
│                 │     │                 │     │                 │
│   Logs ────────▶│     │   LAPI ────────▶│     │   Firewall/Nginx│
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │  CrowdSec       │
                       │  Console/Hub    │
                       │  (threat intel) │
                       └─────────────────┘
```

---

## 🌐 CrowdSec Console Features

When enrolled, you get:
- **Centralized Dashboard** - View alerts from all instances
- **Threat Intelligence** - See IPs blocked globally
- **Blocklists** - Subscribe to community blocklists
- **Metrics** - Visualize attacks and mitigations

---

## 🔒 Security Best Practices

1. **Use multiple detection sources** - Install collections for all your services
2. **Configure appropriate bouncers** - Block at the right level (firewall, nginx, etc.)
3. **Monitor false positives** - Check alerts regularly with `cscli alerts list`
4. **Whitelist your IPs** - Add your own IPs to prevent accidental lockout:
   ```bash
   docker exec crowdsec cscli decisions add --ip YOUR_IP --type whitelist
   ```
5. **Enable enrollment** - Join the community for better threat intelligence

---

> ⚠️ **Note:** CrowdSec alone doesn't block anything. You **must** install at least one bouncer to enforce decisions!

> 💡 **Tip:** Start with the Nginx Bouncer if using Nginx Proxy Manager - it's the easiest integration for web applications.

> 💡 **Tip:** Use `docker logs -f crowdsec` to watch detections in real-time during initial setup.

> 📚 **For more information:** Visit the [CrowdSec documentation](https://docs.crowdsec.net/) for bouncer setup, custom scenarios, and API reference.
