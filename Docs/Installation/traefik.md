# 🐳 Docker Setup with Traefik Reverse Proxy

This repository provides a **Docker Compose** setup for running **aIPTV** alongside **Traefik** as a reverse proxy to manage access control to the service. This configuration restricts certain endpoints to public access while securing other endpoints to be accessible only from local networks.

> **⚠️ Warning:**  
> This setup is currently in **testing** and may require adjustments. We encourage everyone to join our **[Discord Server](https://discord.gg/tP3JcygCA8)** and become part of our **community**.  Your **feedback** is invaluable to us, so please report any **issues**, **bugs**, or suggestions you may have to help improve the service.


## 📖 Table of Contents

1. [🔮 Overview](#-overview)
2. [🎨 Features](#-features)
    - [📌 Prerequisites](#-prerequisites)
        - [🐳 Docker & Docker Compose](#-docker--docker-compose)
3. [🛠️ Configuration](#-configuration)
    - [🐳 Docker Compose File](#-docker-compose-file)
    - [📜 Traefik Configuration](#-traefik-configuration)
    - [🗝️ Key Points](#-key-points)
4. [🛗 Access Control](#-access-control)
    - [🕺 Public Access](#-public-access)
    - [💳 Internal Access](#-internal-access)
5. [🔗 Need Help? Join the Community!](#-need-help-join-the-community)

<br>

## 🔮 Overview

This setup includes two primary services:

1. **aIPTV** – A containerized IPTV service that runs on port `5002`.
2. **Traefik** – A reverse proxy that forwards traffic to the aIPTV service, with specific access rules.

The **Traefik** container restricts access to certain endpoints to specific local IP ranges while allowing public access to a few key API endpoints.

## 🎨 Features

- **aIPTV** service running on the latest version.
- **Traefik** reverse proxy to manage access control.
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

    traefik:
        image: traefik:v2.5
        container_name: traefik
        restart: unless-stopped
        networks:
            - bridge
        ports:
            - "80:80"
            - "443:443"
        command:
            - "--api.dashboard=true"
            - "--api.insecure=false"
            - "--providers.docker=true"
            - "--entryPoints.web.address=:80"
            - "--entryPoints.websecure.address=:443"
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.aiptv.rule=(PathPrefix(`/api/proxy/`) || PathPrefix(`/api/m3u/`) || PathPrefix(`/api/epg/`) || PathPrefix(`/hdhr/`))"
            - "traefik.http.services.aiptv.loadbalancer.server.port=5002"

networks:
    bridge:
        driver: bridge
```

<br>

### 📜 Traefik Configuration

Traefik uses labels in the `docker-compose.yml` file to manage routing and access control. Here’s the basic configuration:

- Traefik uses `PathPrefix` routing to allow public access to specific endpoints like `/api/proxy/`, `/api/m3u/`, `/api/epg/`, and `/hdhr/`.
- For internal access, Traefik restricts all other paths based on IP addresses.

You can adjust Traefik settings in the labels or through additional Traefik configurations if needed.

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