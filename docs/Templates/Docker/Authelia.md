---
title: 🔐 Authelia
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 2
---

# 🔐 Authelia

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/authelia.png" width="80" />

**Authelia** is an open-source authentication and authorization server providing two-factor authentication and single sign-on (SSO) for your applications via a web portal. It acts as a companion for reverse proxies like **Nginx Proxy Manager**, **Traefik**, or **SWAG** to protect your applications.

Authelia supports TOTP (Time-based One-Time Password), WebAuthn (security keys), and Push Notifications for 2FA.

🏷️ **Category:** Security

🐳 **Image:** `authelia/authelia:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/authelia/authelia](https://github.com/authelia/authelia) |
| 🐛 **Support** | [GitHub Issues](https://github.com/authelia/authelia/issues) |
| 📖 **Docs** | [www.authelia.com](https://www.authelia.com/) |
| 💛 **Donate** | [GitHub Sponsors](https://github.com/sponsors/authelia) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9091` | TCP | Authelia web portal and API |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/authelia/config` | `/config` | RW | Authelia configuration files (configuration.yml) |
| `/mnt/cache/appdata/authelia/data` | `/data` | RW | Persistent data for users, sessions, and TOTP secrets |

---

## ⚙️ Environment Variables

### 🔑 Secrets (Required)

Generate all secrets with: `openssl rand -hex 32`

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AUTHELIA_JWT_SECRET` | `` | ✅ | Secret key for JWT token generation |
| `AUTHELIA_SESSION_SECRET` | `` | ✅ | Secret key for session encryption |
| `AUTHELIA_STORAGE_ENCRYPTION_KEY` | `` | ✅ | Encryption key for database storage |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Step 1: Generate Secrets

Run these commands to generate required secrets:

```bash
openssl rand -hex 32  # JWT Secret
openssl rand -hex 32  # Session Secret
openssl rand -hex 32  # Storage Encryption Key
```

### Step 2: Create Configuration

Create `/mnt/cache/appdata/authelia/config/configuration.yml` with minimal config:

```yaml
---
theme: light
default_2fa_method: totp
server:
  host: 0.0.0.0
  port: 9091
log:
  level: info
  format: text
totp:
  issuer: authelia.com
  period: 30
  skew: 1
authentication_backend:
  file:
    path: /data/users_database.yml
    password:
      algorithm: argon2id
      iterations: 1
      salt_length: 16
      parallelism: 8
      memory: 64
access_control:
  default_policy: two_factor
  rules:
    - domain: app1.yourdomain.com
      policy: two_factor
    - domain: app2.yourdomain.com
      policy: one_factor
session:
  name: authelia_session
  expiration: 1h
  inactivity: 5m
  remember_me_duration: 1M
  domain: yourdomain.com
regulation:
  max_retries: 3
  find_time: 2m
  ban_time: 5m
storage:
  local:
    path: /data/db.sqlite3
notifier:
  filesystem:
    filename: /data/notification.txt
```

### Step 3: Create Users Database

Create `/mnt/cache/appdata/authelia/data/users_database.yml`:

```yaml
---
users:
  admin:
    disabled: false
    displayname: "Admin User"
    password: "$argon2id$v=19$m=65536,t=3,p=4$..."  # Use authelia hash-password
    email: admin@yourdomain.com
    groups:
      - admins
      - dev
```

**Generate password hash:**
```bash
docker run authelia/authelia:latest authelia hash-password 'yourpassword'
```

### Step 4: Install via MOS Hub

1. Open the **MOS Hub** and search for **Authelia**
2. Fill in all three secrets generated in Step 1
3. Click **Install**
4. Access Authelia at `http://YOUR_SERVER_IP:9091`

---

## 🔗 Reverse Proxy Integration

### Nginx Proxy Manager Example:

Add to **Advanced** tab of the proxy host you want to protect:

```nginx
location / {
    include /snippets/authelia-location.conf;
    proxy_pass http://backend:port;
}
```

### Traefik Example:

Use the **authelia** middleware in your Traefik labels:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.myapp.rule=Host(`app.yourdomain.com`)"
  - "traefik.http.routers.myapp.middlewares=authelia@docker"
```

---

## 🛡️ Security Features

| Feature | Description |
|---|---|
| **Two-Factor Authentication** | TOTP (Google Authenticator, Authy), WebAuthn (YubiKey), Push Notifications |
| **Single Sign-On (SSO)** | Login once, access all protected apps |
| **Access Control** | Per-domain access rules (bypass, one_factor, two_factor, deny) |
| **Session Management** | Configurable expiration, inactivity timeout, remember me |
| **Brute Force Protection** | Rate limiting and temporary bans |
| **Audit Logging** | Track all authentication attempts |

---

## 📧 Email Notifications (Optional)

For password resets and 2FA setup, configure SMTP in `configuration.yml`:

```yaml
notifier:
  smtp:
    host: mail.yourdomain.com
    port: 587
    username: authelia@yourdomain.com
    password: yourpassword
    sender: authelia@yourdomain.com
    subject: "[Authelia] {title}"
    startup_check_address: test@authelia.com
```

---

> ⚠️ **Important:** Keep your secrets (`AUTHELIA_JWT_SECRET`, `AUTHELIA_SESSION_SECRET`, `AUTHELIA_STORAGE_ENCRYPTION_KEY`) secure and backed up. Losing them will invalidate all sessions and encrypted data!

> 💡 **Tip:** Start with `default_policy: one_factor` to test your setup, then switch to `two_factor` once everything works.

> 💡 **Tip:** Use WebAuthn/FIDO2 security keys (YubiKey, etc.) for the strongest 2FA protection.

> 📚 **For more information:** Visit the [Authelia documentation](https://www.authelia.com/) for LDAP integration, OAuth2, and advanced access control rules.
