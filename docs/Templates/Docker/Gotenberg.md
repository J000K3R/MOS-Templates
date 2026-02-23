---
title: 📄 Gotenberg
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 11
---

# 📄 Gotenberg

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/gotenberg.png" width="80" />

**Gotenberg** provides a developer-friendly API for converting numerous document
formats — HTML, Markdown, Word, Excel and more — into PDF files using powerful
tools like **Chromium** and **LibreOffice**.

🏷️ **Category:** Productivity

🐳 **Image:** `gotenberg/gotenberg`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/gotenberg/gotenberg](https://github.com/gotenberg/gotenberg) |
| 🐛 **Support** | [GitHub Issues](https://github.com/gotenberg/gotenberg/issues) |
| 💛 **Donate** | [github.com/sponsors/gulien](https://github.com/sponsors/gulien) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `3000` | TCP | Gotenberg HTTP API |

---

## 💾 Volumes

No volumes required for this container.

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 📡 API Usage

Once running, you can convert documents via the REST API:
```bash
# Convert an HTML file to PDF
curl --request POST \
  --url http://your-server-ip:3000/forms/chromium/convert/html \
  --header 'Content-Type: multipart/form-data' \
  --form files=@index.html \
  -o result.pdf

# Convert a Word document to PDF
curl --request POST \
  --url http://your-server-ip:3000/forms/libreoffice/convert \
  --header 'Content-Type: multipart/form-data' \
  --form files=@document.docx \
  -o result.pdf
```

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Gotenberg**
2. Click **Install**
3. The API is immediately available at `http://your-server-ip:3000`
4. Send conversion requests via HTTP POST

---

> 💡 **Tip:** Gotenberg works great together with **Paperless-NGX** for automatic
> document conversion, or with **Nextcloud** for on-the-fly PDF generation.

> 🔒 **Security Note:** JavaScript is disabled by default (`--chromium-disable-javascript=true`)
> and only local files are allowed (`--chromium-allow-list=file:///tmp/.*`).
> This is the recommended secure configuration.
