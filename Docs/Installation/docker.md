# 🐳 Docker Installation

## 📖 Table of Contents

1. [🐳 Docker Installation](#-docker-installation)
    - [📌 Prerequisites](#-prerequisites)
        - [🐳 Docker & Docker Compose](#-docker--docker-compose)
        - [📂 File & Folder Setup](#-file--folder-setup)
        - [🔧 System Requirements](#-system-requirements)
        - [🔌 Network & Ports](#-network--ports)
    - [🐳 Install using Docker Run](#-install-using-docker-run)
    - [🐳 Install using Docker Compose](#-install-using-docker-compose)
2. [🌍 Environment Variables](#-environment-variables)
3. [🏷️ Docker Tags](#-docker-tags)

<br>

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

### 📂 File & Folder Setup
Ensure you have a designated directory for aIPTV’s data storage:
```sh
mkdir -p ./data/aiptv
```

### 🔧 System Requirements
- **OS**: Linux (Recommended), macOS, or Windows with WSL2  
- **CPU**: x86_64 / ARM64 with virtualization support  
- **Memory**: At least **2GB RAM** (4GB+ recommended for larger playlists)  
- **Disk Space**: At least **1GB free space**  

### 🔌 Network & Ports
- Ensure **port 5002** is available or modify the `PORT` variable in the `docker-compose.yml` file.
- Allow outbound connections for fetching EPG data and playlist updates.

Once everything is set up, proceed with installation.

<br>

## 🐳 Install using Docker Run
```sh
docker run --net bridge --name aiptv --restart unless-stopped \
    -p 5002:5002 \
    -e DB_PORT=5433 \
    -e PORT=5002 \
    -e PUID=1000 \
    -e PGID=1000 \
    -v ./data/aiptv:/aiptv/data \
    --device /dev/dri:/dev/dri \
    <aiptv-image-if-you-have-it>:latest
```

<br>

## 🐳 Install using Docker Compose

```yaml
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
        devices: # optional
            - /dev/dri:/dev/dri
```

<br>

## 🌍 Environment Variables

| Variable         | Default Value | Required | Description |
|-----------------|--------------|----------|-------------|
| `PORT`         | 5002         | ✅ Yes      | The port on which the aIPTV service runs. Change this if another service is using the same port. |
| `PUID`         | 1000         | ✅ Yes      | The user ID for the container, ensuring correct file permissions. |
| `PGID`         | 1000         | ✅ Yes      | The group ID for the container, maintaining permission consistency. |
| `DB_PORT`      | 5433         | ❌ No       | The PostgreSQL database connection port. Modify this if your database uses a different port. |
| `BASE_PATH`      | -         | ❌ No       | Configure aIPTV to serve the UI from a specified subfolder, allowing access via `/<base_url>`.|
| `LOG_LEVEL`      | -         | ❌ No       | Set to debug for more extensive logging. |
| `USE_EXTERNAL_DB` | false     | ❌ No       | Set to `true` if using an external database, otherwise `false` for internal management. |
| `DB_HOST`      | -            | ❌ No       | The hostname or IP of the external database. Required if `USE_EXTERNAL_DB` is `true`. |
| `DB_USER`      | -            | ❌ No       | The username for connecting to the external database. |
| `DB_PASSWORD`  | -            | ❌ No       | The password for connecting to the external database. |
| `DB_NAME`      | -            | ❌ No       | The database name used by aIPTV. Required when using an external database. |

<br>

## 🏷️ Docker Tags

- `latest` → Pulls the most recent stable version  
- `development-latest` → Pulls the latest development branch version  

### Example usage:
```yaml
services:
    aiptv:
        image: <aiptv-image-if-you-have-it>:development-latest
...
```

<br>

## 🔗 Need Help? Join the Community!
If you run into any issues or have questions, join our **[Discord Channel](https://discord.gg/tP3JcygCA8)** for support and discussion!