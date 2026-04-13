---
title: 🔌 Portainer Agent
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 16
---

# 🔌 Portainer Agent

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/portainer-agent.png" width="80" />

**Portainer Agent** is a lightweight component that runs on Docker hosts to allow a central Portainer instance to manage them securely. It creates a secure tunnel between your main Portainer server and remote Docker environments without requiring direct Docker socket exposure over the network.

This allows you to manage multiple MOS Hub/Docker hosts from a single Portainer dashboard.

🏷️ **Category:** Productivity

🐳 **Image:** `portainer/agent:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [portainer.io](https://www.portainer.io/) |
| 📖 **Docs** | [docs.portainer.io](https://docs.portainer.io/admin/environments/add/docker-edge-agent) |
| 🐛 **Support** | [GitHub Issues](https://github.com/portainer/portainer/issues) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9001` | TCP | Portainer Agent communication port. The central Portainer instance connects to this port. |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/var/run/docker.sock` | `/var/run/docker.sock` | RW | Docker socket for container management |
| `/var/lib/docker/volumes` | `/var/lib/docker/volumes` | RW | Docker volumes directory |
| `/` | `/host` | RO | Host root filesystem (for volume browsing) |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AGENT_SECRET` | `` | ✅ | Optional shared secret for authentication. Must match in Portainer when adding environment. |
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Architecture Overview

```
┌─────────────────┐         ┌─────────────────┐
│   Portainer     │────────▶│  Portainer      │
│   (Central)     │  :9001  │  Agent          │
│   Management    │         │  (This Host)    │
│   Interface     │         │                 │
└─────────────────┘         └─────────────────┘
                                   │
                                   ▼
                            ┌─────────────────┐
                            │  Local Docker   │
                            │  Environment    │
                            └─────────────────┘
```

### Step 1: Install Portainer Agent via MOS Hub

1. Open the **MOS Hub** and search for **Portainer-Agent**
2. Click **Install**
3. The agent will start on port `9001`
4. Make note of this host's IP address

### Step 2: Get Host IP Address

Find the IP of this MOS Hub server:
```bash
ip addr show
```

You'll need: `http://HOST_IP:9001` for the next step.

### Step 3: Add Environment in Central Portainer

On your main Portainer instance:

1. Go to **Environments** → **Add Environment**
2. Select **Docker Standalone**
3. Select **Agent** as the connection method
4. Enter details:
   - **Name**: MOS Hub Server (or any descriptive name)
   - **Environment URL**: `http://HOST_IP:9001` (replace with actual IP)
   - **Agent Secret**: (if you set `AGENT_SECRET`, enter it here)
5. Click **Connect**

### Step 4: Verify Connection

1. The environment should show as **Up**
2. Click on the new environment to view its containers
3. You can now manage this remote MOS Hub from your central Portainer!

---

## 🔐 Security Considerations

### Network Security
- **Firewall**: Only allow port 9001 access from your Portainer server IP
- **Internal Network**: Keep the agent on internal/VPN network if possible
- **AGENT_SECRET**: Always set a shared secret for production use

### Privileged Access
⚠️ **Warning**: The agent requires:
- Access to Docker socket (full container control)
- Access to host filesystem (for volume management)
- Privileged mode enabled

Only deploy the agent on hosts you trust and control completely.

---

## 🌐 Use Cases

### Centralized Management
Manage multiple MOS Hub servers from one dashboard:
- Home server
- VPS in cloud
- Raspberry Pi at another location
- Friend's server (with permission!)

### Remote Monitoring
Check container status, logs, and resource usage on remote hosts without SSH.

### Remote Deployment
Deploy containers to remote hosts from your central Portainer interface.

---

## 🔄 High Availability Setup

For production environments:

1. **Multiple Agents**: Deploy agents on all Docker hosts
2. **Load Balancing**: Use multiple Portainer replicas (BE feature)
3. **Backup**: Backup Portainer data to restore connections

---

## 📝 Troubleshooting

### Agent Not Connecting

1. **Check if agent is running**:
   ```bash
   docker ps | grep portainer-agent
   ```

2. **Verify port is accessible**:
   ```bash
   curl http://localhost:9001
   ```
   Should return agent info.

3. **Check firewall rules**:
   Ensure port 9001 is open between Portainer and Agent.

4. **Check logs**:
   ```bash
   docker logs portainer-agent
   ```

### Connection Timeout

- Verify IP address is correct
- Check network connectivity: `ping PORTAINER_IP` from agent host
- Ensure no NAT/firewall blocking port 9001

### Authentication Failed

If using `AGENT_SECRET`:
- Ensure secret matches exactly in Portainer and Agent
- Restart agent after changing secret

---

> ⚠️ **Warning**: The Portainer Agent has significant access to your Docker host. Only install it on hosts you control and trust. Never expose port 9001 to the public internet without firewall restrictions!

> 💡 **Tip**: Use a VPN or private network for agent communication instead of exposing port 9001 publicly.

> 💡 **Tip**: Set a strong `AGENT_SECRET` to prevent unauthorized connections even if port 9001 is somehow accessed.

> 💡 **Tip**: You can manage the Agent container itself through MOS Hub - but if you stop it, you'll lose remote management until you restart it locally!

> 📚 **For more information:** Visit the [Portainer Agent Documentation](https://docs.portainer.io/admin/environments/add/docker-edge-agent) for advanced configuration and Edge Agent setup.
