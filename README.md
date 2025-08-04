# 📘 Paperless-ngx Docker Compose Stack

[![MIT License](https://img.shields.io/github/license/Vantasin/paperless-ngx?style=flat-square)](LICENSE)
[![Docker Compose](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)](https://www.docker.com/)
[![ZFS](https://img.shields.io/badge/ZFS-OpenZFS-blue?style=flat-square)](https://openzfs.org/)

[![Paperless-ngx](https://img.shields.io/badge/Paperless--ngx-Document%20Management-blue?logo=read-the-docs&style=flat-square)](https://docs.paperless-ngx.com)

Paperless-ngx is a powerful, self-hosted document management system that turns your physical documents into a searchable digital archive. It supports automated OCR, tagging, and organization, making document handling easy and efficient.

---

## 📁 Directory Structure

```bash
tank/
├── docker/
│   ├── compose/
│   │   └── paperless-ngx/              # Git repo lives here
│   │       ├── docker-compose.yml  # Main Docker Compose config
│   │       ├── .env                # Runtime environment variables and secrets (gitignored!)
│   │       ├── env.example         # Example .env file for reference
│   │       └── README.md           # This file
│   └── data/
│       └── paperless-ngx/              # Volume mounts and persistent data
```

---

## 🧰 Prerequisites

* Docker Engine
* Docker Compose V2
* Git
* (Optional) ZFS on Linux for dataset management

> ⚠️ **Note:** These instructions assume your ZFS pool is named `tank`. If your pool has a different name (e.g., `rpool`, `zdata`, etc.), replace `tank` in all paths and commands with your actual pool name.

---

## ⚙️ Setup Instructions

1. **Create the stack directory and clone the repository**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/compose/paperless-ngx
   cd /tank/docker/compose/paperless-ngx
   sudo git clone https://github.com/Vantasin/Paperless-ngx.git .
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/compose/paperless-ngx
   cd ~/docker/compose/paperless-ngx
   git clone https://github.com/Vantasin/Paperless-ngx.git .
   ```

2. **Create the runtime data directory** (optional)

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/data/paperless-ngx
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/data/paperless-ngx
   ```

3. **Configure environment variables**

   Copy and modify the `.env` file:

   ```bash
   sudo cp env.example .env
   sudo nano .env
   sudo chmod 600 .env
   ```

   > **Note:** Be sure to update the `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` and if necessary the `PAPERLESS_DATA_VOLUME`.
   
4. **Configure webserver environment variables**

   Copy and modify the `webserver.env` file:

   ```bash
   sudo cp webserver.env.example webserver.env
   sudo nano webserver.env
   sudo chmod 600 webserver.env
   ```

   > **Note:** Be sure to update the `PAPERLESS_URL`, `PAPERLESS_SECRET_KEY`, `PAPERLESS_TIME_ZONE` and if necessary the `USERMAP_UID`, `USERMAP_GID` & `PAPERLESS_OCR_LANGUAGE`.

   > **Note:** `PAPERLESS_URL` is the URL where the service will be running. Eg. `https://paperless.example.com`.
   
   > **Tip:** You must create the `PAPERLESS_URL` using [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy for HTTPS certificates via Let's Encrypt.
   >
   > **Proxy Host:**
   >  - **Domain Name:** `https://paperless.example.com`
   >  - **Scheme:** `http`
   >  - **Forward Hostname/IP:** `paperless`
   >  - **Forward Port:** `8000`

5. **Start paperless-ngx**

   ```bash
   sudo docker compose up -d
   ```

---

## 🌐 Accessing Paperless-ngx Web UI

Once deployed, access **Paperless-ngx** using:

- **Web Interface:** Enter the URL that was set for `PAPERLESS_URL`. Eg. `https://paperless.example.com`.

- **Initial Setup:** When you first access the web interface, you will be prompted to create a superuser account.

  > **Tip:** Setup Two Factor Authentication.

- **Optional:** Download the mobile app to easily scan documents with your phone.

---

## 🙏 Acknowledgments

- [ChatGPT](https://openai.com/chatgpt) — for assistance in generating setup scripts and templates.
- [Docker](https://www.docker.com/) — for container orchestration and runtime.
- [OpenZFS](https://openzfs.org/) — for advanced local filesystem features, dataset organization, and snapshotting.
- [Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) — the powerful self-hosted document management system with OCR and automated organization features.