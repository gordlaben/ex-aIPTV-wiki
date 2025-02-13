# 🐳 Docker Setup with Caddy Reverse Proxy

This repository provides a **Docker Compose** setup for running **aIPTV** alongside **Caddy** as a reverse proxy to manage access control to the service. This configuration restricts certain endpoints to public access while securing other endpoints to be accessible only from local networks.

> **⚠️ Warning:**  
> This setup is currently in **testing** and may require adjustments. We encourage everyone to join our **[Discord Server](https://discord.gg/tP3JcygCA8)** and become part of our **community**.  Your **feedback** is invaluable to us, so please report any **issues**, **bugs**, or suggestions you may have to help improve the service.

## 📖 Table of Contents

1. [🔮 Overview](#-overview)
2. [🎨 Features](#-features)
    - [📌 Prerequisites](#-prerequisites)
        - [🐳 Docker & Docker Compose](#-docker--docker-compose)
3. [🛠️ Configuration](#-configuration)
    - [🐳 Docker Compose File](#-docker-compose-file)
    - [📜 Caddy Configuration](#-caddy-configuration)
    - [🗝️ Key Points](#-key-points)
4. [🛗 Access Control](#-access-control)
    - [🕺 Public Access](#-public-access)
    - [💳 Internal Access](#-internal-access)
5. [🔗 Need Help? Join the Community!](#-need-help-join-the-community)

<br>

## 🔮 Overview

This setup includes two primary services:

1. **aIPTV** – A containerized IPTV service that runs on port `5002`.
2. **Caddy** – A reverse proxy that forwards traffic to the aIPTV service, with specific access rules.

The **Caddy** container restricts access to certain endpoints to specific local IP ranges while allowing public access to a few key API endpoints.

## 🎨 Features

- **aIPTV** service running on the latest version.
- **Caddy** reverse proxy to manage access control.
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

    caddy:
        image: caddy:latest
        container_name: caddy
        restart: unless-stopped
        networks:
            - bridge
        ports:
            - "80:80"
            - "443:443"
        volumes:
            - ./Caddyfile:/etc/caddy/Caddyfile:ro

networks:
    bridge:
        driver: bridge
```

<br>

### 📜 Caddy Configuration

Create a `Caddyfile` to manage routing and access control. Here's the full configuration:

```Caddyfile
# Reverse Proxy Setup for aIPTV with Caddy

http:// {
    # Allow public access to specific endpoints
    @publicPaths {
        path /api/proxy/* /api/m3u/* /api/epg/* /hdhr/*
    }
    reverse_proxy @publicPaths aiptv:5002

    # Restrict access to internal users for all other paths
    @internalPaths {
        not path /api/proxy/* /api/m3u/* /api/epg/* /hdhr/*
    }
    @internalPaths {
        remote_ip 192.0.0.0/8 172.0.0.0/8 10.0.0.0/8 127.0.0.0/8
    }
    reverse_proxy @internalPaths aiptv:5002
}
```

### 🗝️ Key Points:
- **Public access** to `/api/proxy/`, `/api/m3u/`, `/api/epg/`, and `/hdhr/` using Caddy's reverse proxy.
- **Internal access restrictions** are enforced for all other paths based on IP range.
  
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