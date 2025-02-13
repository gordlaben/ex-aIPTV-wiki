# 🐳 Docker Installation with Gluetun VPN

## 📖 Table of Contents

1. [🐳 Docker Installation with Gluetun VPN](#-docker-installation-with-gluetun-vpn)
    - [📌 Prerequisites](#-prerequisites)
        - [🐳 Docker & Docker Compose](#-docker--docker-compose)
        - [📂 File & Folder Setup](#-file--folder-setup)
        - [🔧 System Requirements](#-system-requirements)
        - [🔌 Network & Ports](#-network--ports)
        - [🔀 VPN Provider compatible with Gluetun](#-vpn-provider-compatible-with-gluetun)
    - [🐳 Install using Docker Compose](#-install-using-docker-compose)
    - [Compose Stack Explained](#compose-stack-explained)
        - [Gluetun](#gluetun)
        - [aIPTV](#aiptv)
        - [unhealthy_container_restart](#unhealthy_container_restart)
2. [🌍 Environment Variables](#-environment-variables)

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

### 🔀 VPN Provider compatible with Gluetun
Check out the [Gluetun Github](https://github.com/qdm12/gluetun) for supported VPN providers.

<br>

## 🐳 Install using Docker Compose

```yaml
services:
    gluetun:
        image: qmcgaw/gluetun
        devices:
            - /dev/net/tun:/dev/net/tun
        cap_add:
            - NET_ADMIN
        ports:
            - 8000:8000 # gluetun api
            - 5002:5002 # aiptv
        volumes:
            - ./gluetun/data:/gluetun
        environment: # Visit Gluetun Wiki for your specific VPN provider
            - VPN_SERVICE_PROVIDER= # Should be filled
            - VPN_TYPE= # openvpn or wireguard
            - OPENVPN_USER= # Should be filled
            - OPENVPN_PASSWORD= # Should be filled
            - SERVER_COUNTRIES= # Optional
        restart: unless-stopped

    aiptv:
        image: <aiptv-image-if-you-have-it>:latest
        container_name: aiptv
        restart: unless-stopped
        environment:
            - PORT=5002
            - PUID=1000
            - PGID=1000
        volumes:
            - ./data/aiptv:/aiptv/data
        devices: # optional
            - /dev/dri:/dev/dri
        depends_on:
            gluetun:
                condition: service_healthy
                restart: true
        network_mode: service:gluetun

    # OPTIONAL but Recommended
    unhealthy_container_restarter:
        image: docker:cli
        network_mode: none
        cap_drop:
            - ALL
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        command: >
            sh -c "while true; do 
                sleep 60; 
                docker ps -q -f health=unhealthy | xargs --no-run-if-empty docker restart; 
            done"
        restart: unless-stopped
```

## Compose Stack Explained:
### **Gluetun**
A container that acts as a network proxy, routing traffic securely through your VPN. Check if you are connected successfully by opening this URL: `http://serverip:8000/v1/publicip/ip`

### **aIPTV**
The aIPTV container instance that handles IPTV streaming. Remember the PORT variable is moved to Gluetuns container and should not be used here!s

### **unhealthy_container_restart**
A small script contributed by @SergeantPanda ❤️ that automatically restarts aIPTV if Gluetun restarts. This ensures seamless operation without manual intervention.

<br>

## 🌍 Environment Variables

| Variable         | Default Value | Required | Description |
|-----------------|--------------|----------|-------------|
| `PORT`         | 5002         | ✅ Yes      | The port on which the aIPTV service runs. Change this if another service is using the same port. |
| `PUID`         | 1000         | ✅ Yes      | The user ID for the container, ensuring correct file permissions. |
| `PGID`         | 1000         | ✅ Yes      | The group ID for the container, maintaining permission consistency. |
| `DB_PORT`      | 5433         | ❌ No       | The PostgreSQL database connection port. Modify this if your database uses a different port. |
| `USE_EXTERNAL_DB` | false     | ❌ No       | Set to `true` if using an external database, otherwise `false` for internal management. |
| `DB_HOST`      | -            | ❌ No       | The hostname or IP of the external database. Required if `USE_EXTERNAL_DB` is `true`. |
| `DB_USER`      | -            | ❌ No       | The username for connecting to the external database. |
| `DB_PASSWORD`  | -            | ❌ No       | The password for connecting to the external database. |
| `DB_NAME`      | -            | ❌ No       | The database name used by aIPTV. Required when using an external database. |


<br>

## 🔗 Need Help? Join the Community!
If you run into any issues or have questions, join our **[Discord Channel](https://discord.gg/tP3JcygCA8)** for support and discussion!