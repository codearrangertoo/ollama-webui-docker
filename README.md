# Ollama Docker Compose Setup with WebUI and Remote Access via Cloudflare

This Docker Compose configuration outlines a complete setup for running local AI models using Ollama with a web interface. It's designed to be accessible remotely, with an optional integration of Cloudflare for enhanced security and accessibility. The setup includes three main services: `webui`, `ollama`, and an optional `tunnel` for those opting for Cloudflare access.

## Prerequisites

- Supported NVIDIA GPU
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Services Overview

### webui
- **Image**: `ghcr.io/open-webui/open-webui:main`
- **Function**: Serves as the web interface for interacting with the Ollama AI models.
- **Customization**: Adjust `OLLAMA_API_BASE_URL` to match the internal network URL of the `ollama` service. If running `ollama` on the docker host, comment out the existing `OLLAMA_API_BASE_URL` and use the provided alternative.

### ollama (Optional if you are running ollama on the docker host)
- **Image**: `ollama/ollama`
- **Function**: Acts as the AI model server, with the capability to utilize NVIDIA GPUs for model inference.
- **GPU Utilization**: Configured to use NVIDIA GPUs to ensure efficient model inference. Verify your system's compatibility.

### tunnel
- **Image**: `cloudflare/cloudflared:latest`
- **Function**: Provides a secure tunnel to the web UI via Cloudflare, enhancing remote access security.
- **Note**: We are using the demo mode by default, so the URL will change each time you restart unless you create an account with cloudflare

## Configuration and Deployment

1. **Volumes**: Two volumes, `ollama` and `open-webui`, are defined for data persistence across container restarts.

2. **Environment Variables**: Ensure `OLLAMA_API_BASE_URL` is correctly set. Utilize the `host.docker.internal` address if `ollama` runs on the Docker host.

3. **Deployment**:
    - Run `docker compose up -d` to start the services in detached mode.

4. **Accessing the Web UI**:
    - Directly via `http://localhost:8080` if local access is sufficient.
    - Through the Cloudflare Tunnel URL printed in the docker logs. Run `docker compose logs tunnel` to find the URL for remote access
