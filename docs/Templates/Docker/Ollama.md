---
title: 🦙 Ollama
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 16
---

# 🦙 Ollama

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/ollama.png" width="80" />

**Ollama** allows you to run large language models (LLMs) locally on your own hardware.
It supports popular models like Llama, Mistral, Gemma, and many more — with a simple
command-line interface and REST API for easy integration.

This container provides a self-hosted AI inference server that can be used by
applications like Open WebUI, LibreChat, or any other OpenAI-compatible client.

🏷️ **Category:** AI / LLM

🐳 **Image:** `ollama/ollama`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/ollama/ollama](https://github.com/ollama/ollama) |
| 🐛 **Support** | [GitHub Issues](https://github.com/ollama/ollama/issues) |
| 📖 **Docs** | [ollama.com](https://ollama.com) |
| 📚 **Library** | [ollama.com/library](https://ollama.com/library) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `11434` | TCP | Ollama API port — used for model inference and management |

---

## 💾 Volumes

| Host Path | Container Path | Description |
|---|---|---|
| `/mnt/cache/appdata/ollama` | `/root/.ollama` | Storage for downloaded models and configuration |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `OLLAMA_HOST` | `0.0.0.0:11434` | ❌ | Bind address for the Ollama API. Use `0.0.0.0` to accept connections from any IP |
| `OLLAMA_MODELS` | `/root/.ollama/models` | ❌ | Path where models are stored inside the container |
| `OLLAMA_ORIGINS` | `` | ❌ | Allowed CORS origins (comma-separated). Set to `*` to allow all origins |
| `OLLAMA_KEEP_ALIVE` | `5m` | ❌ | Duration to keep models loaded in memory after last request (e.g., `5m`, `1h`, `24h`) |
| `OLLAMA_NUM_PARALLEL` | `1` | ❌ | Number of parallel model requests to handle simultaneously |
| `OLLAMA_MAX_LOADED_MODELS` | `1` | ❌ | Maximum number of models to keep loaded in memory at once |
| `OLLAMA_FLASH_ATTENTION` | `false` | ❌ | Enable Flash Attention for faster inference on supported GPUs |
| `OLLAMA_KV_CACHE_TYPE` | `` | ❌ | KV cache quantization type: `f16`, `q8_0`, `q4_0` (reduces VRAM usage) |
| `OLLAMA_GPU_OVERHEAD` | `` | ❌ | Reserve VRAM overhead (in bytes) for other GPU applications |
| `OLLAMA_DEBUG` | `false` | ❌ | Enable debug logging for troubleshooting |
| `TZ` | `Europe/Vienna` | ❌ | Timezone of the container, e.g. `Europe/Vienna` or `Europe/Berlin` |
| `PUID` | `500` | ❌ | User ID for file permissions (MOS default: 500) |
| `PGID` | `500` | ❌ | Group ID for file permissions (MOS default: 500) |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Ollama**
2. Adjust the `OLLAMA_ORIGINS` variable if you need CORS access from specific domains
3. For GPU support, ensure your host has NVIDIA Docker runtime configured
4. Click **Install**
5. Pull your first model:

```bash
docker exec -it ollama ollama pull llama3.2
```

6. Test the API:

```bash
curl http://YOUR_HOST_IP:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "Hello, how are you?"
}'
```

---

## 🔧 GPU Support (NVIDIA)

For NVIDIA GPU acceleration, ensure you have:

1. **NVIDIA Driver** installed on your host
2. **NVIDIA Container Toolkit** installed:
   ```bash
   # Ubuntu/Debian
   sudo apt install nvidia-container-toolkit
   sudo nvidia-ctk runtime configure --runtime=docker
   sudo systemctl restart docker
   ```

3. The container will automatically use available GPUs when properly configured.

---

## 📊 Recommended Models

| Model | Size | Use Case |
|---|---|---|
| `llama3.2` | 2-4 GB | General purpose, fast responses |
| `mistral` | 4-7 GB | Good reasoning and instruction following |
| `gemma2` | 3-9 GB | Google's efficient models |
| `codellama` | 4-17 GB | Code generation and completion |
| `llava` | 4-7 GB | Vision — image understanding |

---

## 🔗 OpenAI-Compatible API

Ollama provides an OpenAI-compatible API endpoint at:

```
http://YOUR_HOST_IP:11434/v1/chat/completions
```

Use this URL in applications that expect an OpenAI API endpoint. No API key required!

---

> ⚠️ **Note:** Downloaded models can be large (2-50+ GB). Ensure you have sufficient
> storage in your volume mount (`/mnt/cache/appdata/ollama`).

> 💡 **Tip:** Use `OLLAMA_KEEP_ALIVE=24h` to keep models loaded and avoid reload delays
> during active usage periods.

> 💡 **Tip:** For multiple concurrent users, increase `OLLAMA_NUM_PARALLEL` and
> `OLLAMA_MAX_LOADED_MODELS` accordingly.
