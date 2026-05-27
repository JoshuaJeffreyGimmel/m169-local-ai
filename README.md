# Local LLM Infrastructure Stack

A fully containerized local Large Language Model (LLM) development and monitoring environment. This stack deploys Open WebUI as the frontend, LiteLLM as an OpenAI-compatible proxy router, Ollama for local model execution, and a full Prometheus/Grafana monitoring suite to track GPU and hardware metrics.

---

## 📋 Prerequisites

Before running this stack, you must have the following installed on your host system:

* **Docker:** Version 20.10.0 or higher.
* **Docker Compose:** V2 syntax support (e.g., `docker compose up` rather than `docker-compose up`).

### ⚠️ Critical Hardware Requirement: Nvidia GPU
The `ollama` and `dcgm-exporter` services in this configuration are explicitly configured to utilize **Nvidia GPUs** via the Nvidia Container Toolkit. 

* **If you have an Nvidia GPU:** Ensure you have installed the [Nvidia Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) on your host machine so Docker can access your hardware.
* **If you DO NOT have an Nvidia GPU (AMD, Intel, or Apple Silicon):** The Ollama service will likely crash or fail to start because it cannot find the requested `nvidia` driver and capabilities. To run this on non-Nvidia hardware, you will need to modify the `docker-compose.yml` file to remove the `deploy.resources.reservations` blocks under `ollama` and `dcgm-exporter`.

---

## 🚀 Getting Started

### 1. Configuration & Security (Crucial Step)
Before spinning up the containers, you **must** change the default placeholder credentials to secure your stack. 

Open your `docker-compose.yml` file and update the following:
* **`WEBUI_SECRET_KEY`** (under `open-webui`): Change the default string to a long, random sequence to secure user sessions.
* **Database Passwords:** Change the password in the `DATABASE_URL` (under `litellm`) and the `POSTGRES_PASSWORD` (under `db`). They must match exactly.

### 2. Launching the Stack
Run the following command in the root directory of your project to download the images and start the services in the background:

```bash
docker compose up -d
