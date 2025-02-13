# 🐳 Docker Setup with NGINX Reverse Proxy

This repository provides a **Docker Compose** setup for running **aIPTV** alongside **NGINX** as a reverse proxy to manage access control to the service. This configuration restricts certain endpoints to public access while securing other endpoints to be accessible only from local networks.

> **⚠️ Warning:**  
> This setup is currently in **testing** and may require adjustments. We encourage everyone to join our **[Discord Server](https://discord.gg/tP3JcygCA8)** and become part of our **community**.  Your **feedback** is invaluable to us, so please report any **issues**, **bugs**, or suggestions you may have to help improve the service.

## 📖 Table of Contents

1. [🔮 Overview](#-overview)
2. [🎨 Features](#-features)
    - [📌 Prerequisites](#-prerequisites)
        - [🐳 Docker & Docker Compose](#-docker--docker-compose)
3. [🛠️ Configuration](#-configuration)
    - [🐳 Docker Compose File](#-docker-compose-file)
    - [📜 NGINX Configuration](#-nginx-configuration)
    - [🗝️ Key Points](#-key-points)
4. [🛗 Access Control](#-access-control)
    - [🕺 Public Access](#-public-access)
    - [💳 Internal Access](#-internal-access)
5. [🔗 Need Help? Join the Community!](#-need-help-join-the-community)

<br>

## 🔮 Overview

This setup includes two primary services:

1. **aIPTV** – A containerized IPTV service that runs on port `5002`.
2. **NGINX** – A reverse proxy that forwards traffic to the aIPTV service, with specific access rules.

The **NGINX** container restricts access to certain endpoints to specific local IP ranges while allowing public access to a few key API endpoints.

## 🎨 Features

- **aIPTV** service running on the latest version.
- **NGINX** reverse proxy to manage access control.
- Public access to specific endpoints:
  - `/api/proxy/`
  - `/api/m3u/`
  - `/api/epg/`
  - `/hdhr/`
- Internal access restriction to certain IP ranges:
  - `192.0.0.0/8`
  - `172.0.0.0/8`
  - `10.0.0.0/8`
  - `127.0.0.0/8`

## 📌 Prerequisites

Before running aIPTV using Docker, ensure you have the following dependencies installed:

### 🐳 Docker & Docker Compose
- Install **Docker**: [Get Docker](https://docs.docker.com/get-docker/)
- Install **Docker Compose**: [Get Docker Compose](https://docs.docker.com/compose/install/)

Verify installation:
```sh
docker --version
docker-compose --version
```

## 🛠️ Configuration

### 🐳 Docker Compose File

The `docker-compose.yml` file defines two services:

```yaml
version: '3.8'

services:
    aiptv:
        image: <aiptv-image-if-you-have-it>:latest
        container_name: aiptv
        restart: unless-stopped
        networks:
            - bridge
        ports:
            - "5002:5002"
        environment:
            - PORT=5002
            - PUID=1000
            - PGID=1000
        volumes:
            - ./data/aiptv:/aiptv/data
        devices:
            - /dev/dri:/dev/dri

    nginx:
        image: nginx:latest
        container_name: nginx
        restart: unless-stopped
        networks:
            - bridge
        ports:
            - "80:80"
        volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf:ro

networks:
    bridge:
        driver: bridge
```

<br>

### 📜 NGINX Configuration

Create an `nginx.conf` file to manage routing and access control. Here's the full configuration:

```nginx
events {}

http {
    upstream aiptv_backend {
        server aiptv:5002;
    }

    server {
        listen 80;

        # Allow public access to specific endpoints
        location ~ ^/api/proxy/|^/api/m3u/|^/api/epg/|^/hdhr/ {
            proxy_pass http://aiptv_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        # Restrict access to internal users
        location / {
            allow 192.0.0.0/8;
            allow 172.0.0.0/8;
            allow 10.0.0.0/8;
            allow 127.0.0.0/8;
            deny all;
            proxy_pass http://aiptv_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

### 🗝️ Key Points:
- **Public access** to `/api/proxy/`, `/api/m3u/`, `/api/epg/`, and `/hdhr/`.
- **Internal access restrictions** are enforced for all other paths.
  
You can adjust the IP ranges or add more conditions as needed.

## 🛗 Access Control

### 🕺 Public Access
- Endpoints like `/api/proxy/`, `/api/m3u/`, `/api/epg/`, and `/hdhr/` are accessible to everyone.

### 💳 Internal Access
- All other routes are restricted and can only be accessed from the following IP ranges:
  - `192.0.0.0/8`
  - `172.0.0.0/8`
  - `10.0.0.0/8`
  - `127.0.0.0/8`

This allows you to protect your service while enabling external users to access specific resources.

<br>

## 🔗 Need Help? Join the Community!
If you run into any issues or have questions, join our **[Discord Channel](https://discord.gg/tP3JcygCA8)** for support and discussion!