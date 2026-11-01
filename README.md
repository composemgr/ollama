## 👋 Welcome to ollama 🚀

Get up and running with large language models locally

## 📋 Description

Ollama runs large language models locally and exposes them over an API. This
stack pairs the Ollama server with Open WebUI as the browser chat front end
and ComfyUI for image generation, sharing one model directory, behind the
host nginx proxy.

## 🚀 Services

- **app**: ghcr.io/open-webui/open-webui:main (published web UI)
- **ollama-llm**: ollama/ollama:latest (model API)
- **ollama-comfyui**: ghcr.io/ai-dock/comfyui:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/ollama/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/ollama" ~/.local/srv/docker/ollama
cd ~/.local/srv/docker/ollama
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install ollama
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
OLLAMA_KEEP_ALIVE=5m
OLLAMA_MAX_LOADED_MODELS=1
OLLAMA_NUM_PARALLEL=1
COMFYUI_ARGS=--cpu
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64278` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service, Open WebUI, carries the proxy labels and joins the
external network on port `8080`. The Ollama model API and ComfyUI stay on the
internal project network, so the proxy variants do not publish port `11434`.
If you need the raw model API through a proxy, add a second router for the
`ollama-llm` service.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64278
- **Ollama API**: http://172.17.0.1:11434

Port `64278` serves the Open WebUI chat interface, the one users log into.
Port `11434` is the Ollama model API, used by `ollama` CLI clients and any
local SDK. Both are published on the dockerhost bind address `172.17.0.1` so
they stay off the public interfaces. The model API has no authentication, treat
anything on port `11434` as trusted-only.

## 📂 Volumes

- `./volumes/data/open-webui` - Open WebUI database, config, and uploads
- `./volumes/data/models` - Ollama model files, shared with ComfyUI
- `./volumes/data/ollama` - Ollama state under `/root/.ollama`
- `./volumes/data/comfyui` - ComfyUI workspace

## 🔐 Security

- Set `APP_SECRET_KEY` to a random 32 character or longer value, it signs
  Open WebUI sessions
- The Ollama API on port `11434` has no authentication at all, anyone who can
  reach it can run models, restrict it to trusted networks
- Use HTTPS via the reverse proxy in production, prompts and responses are sent
  in clear over plain HTTP
- The stack requests `/dev/dri` for GPU passthrough, remove both `devices`
  entries to run on CPU only
- `COMFYUI_ARGS=--cpu` forces CPU inference, drop the flag to use the GPU
- Telemetry and analytics are disabled
- Models are large, keep an eye on disk under `./volumes/data/models`
- Regularly update Docker images for security patches
- Backup all volumes regularly, re-pulling a model is faster than losing one

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- A `/dev/dri` device for GPU acceleration, optional
- Enough RAM and disk for the models you intend to pull

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
