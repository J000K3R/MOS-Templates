---
title: 🌐 Unifi Toolkit
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 18
---

# 🌐 Unifi Toolkit

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/unifi-toolkit.png" width="80" />

**Unifi Toolkit** is a **third-party, unofficial web-based toolkit** for managing Ubiquiti Unifi networks. It is **not affiliated with or endorsed by Ubiquiti Inc.** It provides an alternative interface for configuring Unifi devices via the Unifi Controller API.

⚠️ **This is NOT official Ubiquiti software.** It is a community tool created by Crosstalk Solutions that connects to your existing Unifi Controller via API.

Perfect for Unifi network administrators who want a simplified management interface alongside or instead of the standard Unifi Controller.

🏷️ **Category:** Network

🐳 **Image:** `crosstalksolutions/unifi-toolkit:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Crosstalk-Solutions/unifi-toolkit](https://github.com/Crosstalk-Solutions/unifi-toolkit) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Crosstalk-Solutions/unifi-toolkit/issues) |
| 💛 **Crosstalk Solutions** | [crosstalksolutions.com](https://www.crosstalksolutions.com/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8000` | TCP | Unifi Toolkit web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/unifi-toolkit` | `/app/config` | RW | Configuration and settings |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### � Required Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `ENCRYPTION_KEY` | `` | ✅ | **Required!** Encryption key for stored credentials. Generate with: `openssl rand -base64 32` |

### � Deployment Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DEPLOYMENT_TYPE` | `local` | ❌ | Deployment mode: `local` for homelab, `production` for public access |

### 🔗 Unifi Controller Connection

| Variable | Default | Masked | Description |
|---|---|---|---|
| `UNIFI_CONTROLLER_URL` | `https://192.168.1.1` | ❌ | **Required.** Local controller IP/hostname (e.g., `https://192.168.1.1`). **Note:** Cloud access via `unifi.ui.com` is NOT supported. |
| `UNIFI_API_KEY` | `` | ✅ | **Recommended.** API key from Unifi OS Settings → Admins. Generate in your Unifi Controller. |
| `UNIFI_USERNAME` | `` | ❌ | Fallback username if not using API key (optional) |
| `UNIFI_PASSWORD` | `` | ✅ | Fallback password if not using API key (optional) |
| `UNIFI_SITE_ID` | `default` | ❌ | Site ID from URL, not friendly name. Use ID from `/manage/site/{id}/...` |
| `UNIFI_VERIFY_SSL` | `false` | ❌ | SSL verification. Set to `true` only with valid SSL certificates |
| `STALKER_REFRESH_INTERVAL` | `60` | ❌ | Device refresh interval in seconds |

---

## 🚀 Quick Start

### Prerequisites

You need an existing **Unifi Controller** (self-hosted or cloud key). This tool connects to it via API.

### Step 1: Generate Encryption Key

**Required:** Generate a strong encryption key:
```bash
openssl rand -base64 32
```
Copy the output and save it for the `ENCRYPTION_KEY` variable.

### Step 2: Generate API Key (Recommended)

1. Login to your **Unifi Controller** web interface
2. Go to **Settings** (gear icon) → **Admins**
3. Click on your admin user
4. Click **Create API Key**
5. Copy the API key (you won't see it again!)

### Step 3: Install via MOS Hub

1. Open the **MOS Hub** and search for **Unifi-Toolkit**
2. **Set the Encryption Key** - Paste the key from Step 1 into `ENCRYPTION_KEY`
3. Configure the Unifi Controller connection:
   - Set `UNIFI_CONTROLLER_URL` to your controller (e.g., `https://192.168.1.1`)
   - Set `UNIFI_API_KEY` with the key from Step 2 (recommended)
   - **OR** set `UNIFI_USERNAME` and `UNIFI_PASSWORD` as fallback
4. Click **Install**
5. Access Unifi Toolkit at `http://YOUR_SERVER_IP:8000`

### Step 4: Verify Connection

1. Open the web interface
2. The toolkit should automatically connect to your Unifi Controller
3. You should see your network devices and configuration

---

## 🔧 Configuration

### Unifi Controller URL Examples

| Setup | URL Example | Notes |
|---|---|---|
| **UDM/UDM-Pro** | `https://192.168.1.1` | Use IP address, port 443 is default |
| **Cloud Key Gen2+** | `https://192.168.1.20` | Local IP, not unifi.ui.com |
| **Self-hosted Controller** | `https://192.168.1.10:8443` | Default port 8443 |
| **Docker Container** | `https://unifi:8443` | If on same Docker network |

⚠️ **Important:** Use your controller's **local IP address**. Cloud access via `unifi.ui.com` is **NOT supported** by this toolkit.

### SSL Certificate Verification

If your Unifi Controller uses a self-signed certificate (default), keep:
```
UNIFI_VERIFY_SSL=false
```

If you have a valid SSL certificate installed:
```
UNIFI_VERIFY_SSL=true
```

### Multi-Site Setup

If you have multiple sites in your Unifi Controller, specify the site ID (not the friendly name):
```
UNIFI_SITE_ID=default    # Default site
UNIFI_SITE_ID=5f3a2b1c4d8e9f0a1b2c3d4e   # Site ID from URL
```

**Finding your Site ID:**
1. In Unifi Controller, switch to the site you want
2. Look at the URL: `/manage/site/5f3a2b1c4d8e9f0a1b2c3d4e/dashboard`
3. The Site ID is the long string after `/site/`

---

## 📊 Features

### Device Management
- View all Unifi devices on your network
- See device status and connectivity
- Manage device configurations
- Restart or adopt devices

### Network Configuration
- Configure WiFi networks
- Manage VLANs and subnets
- Set up port forwarding
- Configure firewall rules

### Client Monitoring
- View connected clients
- See bandwidth usage
- Manage client access
- Block/unblock devices

### Site Management
- Switch between multiple sites
- Site-specific configurations
- Site statistics and health

---

## 🔒 Security Considerations

### API Access
- The toolkit requires admin access to your Unifi Controller
- Credentials are stored in the container environment
- Use a dedicated service account if possible

### Network Access
- Keep Unifi Toolkit on internal network
- Use reverse proxy with authentication for external access
- Consider VPN access instead of public exposure

### SSL/TLS
- Unifi Controllers typically use self-signed certificates
- Set `UNIFI_VERIFY_SSL=false` for default setups
- Install valid SSL certificates on your controller for production

---

## 🌐 Unifi Controller Setup

### Generating an API Key (Recommended Method)

The API Key is the most secure way to connect:

1. Login to your **Unifi Controller** web interface
2. Go to **Settings** (gear icon in bottom left) → **Admins**
3. Click on your admin user account
4. Scroll down to **API Keys** section
5. Click **Create API Key**
6. Give it a name (e.g., "MOS-Hub-Toolkit")
7. **Copy the key immediately** - you won't see it again!
8. Paste the key into `UNIFI_API_KEY` environment variable

### Controller Requirements

- Controller must be running and reachable via local IP
- Network connectivity between MOS Hub and Controller
- For UDM/UDM-Pro: Use `https://192.168.1.1` (no port, uses 443)
- For self-hosted: Use `https://IP:8443` (or 443 if configured)

---

## 🛠️ Troubleshooting

### "ERROR: No .env file found and ENCRYPTION_KEY not set!"

**This is the most common error!** You must set the `ENCRYPTION_KEY` environment variable.

**Solution:**
1. Generate a key: `openssl rand -base64 32`
2. Copy the output (it looks like: `aBcD1234...`)
3. Paste it into the `ENCRYPTION_KEY` field in MOS Hub
4. Redeploy the container

### Connection Failed

1. **Check URL** - Verify `UNIFI_CONTROLLER_URL` is correct with `https://`
   - UDM/UDM-Pro: Use `https://192.168.1.1` (no port needed, uses 443)
   - Self-hosted: Use `https://IP:8443` if not using standard 443
   - **DO NOT use `unifi.ui.com`** - only local IPs work
2. **Network Access** - Ensure MOS Hub can reach the Unifi Controller
   ```bash
   curl -k https://UNIFI_IP:8443
   ```

### Authentication Failed

1. **API Key (Recommended)** - Verify you generated the API key correctly in Unifi OS Settings → Admins
2. **Username/Password Fallback** - Only used if API key is empty
3. **Local Account** - Ensure you're using a local admin account, not a UI.com SSO account

### SSL Certificate Errors

If you see SSL errors:
1. Set `UNIFI_VERIFY_SSL=false` (default, works with self-signed certs)
2. Or install valid certificates on your Unifi Controller and set to `true`

### Site Not Found

1. Check `UNIFI_SITE_ID` matches the ID from URL (not friendly name)
2. Site ID is the long string in URL: `/manage/site/{ID}/dashboard`
3. Example: `5f3a2b1c4d8e9f0a1b2c3d4e` not `Home Office`

---

> 💡 **Tip:** Keep your Unifi Controller updated for the best API compatibility and security.

> 💡 **Tip:** Use Unifi Toolkit alongside the official Unifi Controller - they complement each other.

> 💡 **Tip:** For UDM/UDM-Pro users, the controller runs on port 443, not 8443.

> 📚 **For more information:** Visit the [Unifi Toolkit GitHub](https://github.com/Crosstalk-Solutions/unifi-toolkit) for detailed documentation and feature updates.
