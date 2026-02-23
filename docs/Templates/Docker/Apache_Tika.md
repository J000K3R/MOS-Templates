---
title: 🪶 Apache Tika
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 1
---

# 🪶 Apache Tika

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/apache.png" width="80" />

**Apache Tika™** is a toolkit for detecting and extracting metadata and structured
text content from various documents using existing parser libraries.

🏷️ **Category:** Productivity

🐳 **Image:** `docker.io/apache/tika:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/apache/tika](https://github.com/apache/tika) |
| 🐛 **Support** | [Apache Jira](https://issues.apache.org/jira/projects/TIKA/issues) |
| 💛 **Donate** | [donate.apache.org](https://donate.apache.org/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9998` | TCP | Tika Web Interface & API |

---

## 💾 Volumes

No volumes required for this container.

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 🚀 Quick Start

1. Open the **MOS Hub**
2. Search for **Apache Tika**
3. Click **Install**
4. Access the Tika API at `http://your-server-ip:9998`

---

## 📡 API Usage

Once running, you can extract text from documents via the REST API:
```bash
# Extract text from a file
curl -X PUT --data-binary @document.pdf \
  -H "Content-Type: application/pdf" \
  http://your-server-ip:9998/tika

# Detect file type
curl -X PUT --data-binary @document.pdf \
  http://your-server-ip:9998/detect/stream
```

---

> 💡 **Tip:** Apache Tika is often used together with **Paperless-NGX** or
> **Nextcloud** for automatic document processing.
