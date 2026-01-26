## 👋 Welcome to ollama 🚀

Get up and running with large language models locally

## 📋 Description

Get up and running with large language models locally

## 🚀 Services

- **ui**: ghcr.io/open-webui/open-webui:main
- **app**: ollama/ollama:latest
- **comfy**: ghcr.io/ai-dock/comfyui:v2-cpu-22.04-v0.2.7

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/ollama/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/ollama" ~/.local/srv/docker/ollama
cd ~/.local/srv/docker/ollama
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install ollama
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:59042

## 📂 Volumes

- `./rootfs/data/ui` - Data storage
- `./rootfs/data/models` - Data storage
- `./rootfs/data/ollama` - Data storage
- `./rootfs/data/comfy` - Data storage

## 🔍 Logging

```shell
docker compose logs -f ui
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
