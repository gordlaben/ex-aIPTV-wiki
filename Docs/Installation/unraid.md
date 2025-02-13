# 🖥️ aIPTV Installation on Unraid

## 📖 Table of Contents

1. [🖥️ aIPTV Installation on Unraid](#-aiptv-installation-on-unraid)
    - [📌 Prerequisites](#-prerequisites)
        - [🛠️ Requirements](#-requirements)
        - [🔌 Network & Ports](#-network--ports)
2. [🚀 Manual Installation via Docker](#-manual-installation-via-docker)
    - [1️⃣ Add a New Container](#1️⃣-add-a-new-container)
    - [2️⃣ Configure Basic Settings](#2️⃣-configure-basic-settings)
    - [3️⃣ Add Additional Settings](#3️⃣-add-additional-settings)
    - [4️⃣ Apply and Start the Container](#4️⃣-apply-and-start-the-container)
3. [📜 Advanced Configuration](#-advanced-configuration)
    - [🔄 Customizing Environment Variables](#-customizing-environment-variables)
4. [🏷️ Docker Tags](#-docker-tags)

<br>

## 📌 Prerequisites

Before setting up aIPTV on **Unraid**, ensure you have the following:

### 🛠️ Requirements
- **Unraid OS** installed and running.
- **Community Applications Plugin** installed (required for easy app installation).
- **Docker enabled** in Unraid.

### 🔌 Network & Ports
- Ensure **port 5002** is available or adjust it in the container settings.
- Allow outbound connections for fetching **EPG data** and playlist updates.

<br>

## 🚀 Manual Installation via Docker

#### 1️⃣ Add a New Container  
1. Open **Unraid Web UI**.
2. Navigate to **Docker** → **Add Container**.

#### 2️⃣ Configure Basic Settings  
- **Name:** `aIPTV`  
- **Repository:** `<aiptv-image-if-you-have-it>:latest`  
- **WebUI:** `http://[IP]:[PORT:5002]/`  

#### 3️⃣ Add Additional Settings  
Click **"Add another Path, Port, Variable, Label or Device"** and configure the following:

##### 🔹 **Port Configuration**
| Name  | Container Port | Host Port |
|-------|---------------|-----------|
| Port  | 5002          | 5002      |

##### 🔹 **Environment Variables**
| Name  | Key   | Value |
|-------|------|-------|
| PUID  | `PUID` | `99`  |
| PGID  | `PGID` | `100`  |
| PORT  | `PORT` | `5002`  |

##### 🔹 **Volume Mapping**
| Name  | Container Path   | Host Path |
|-------|----------------|-----------|
| Data  | `/aiptv/data`  | `/mnt/user/appdata/aiptv` |

#### 4️⃣ Apply and Start the Container  
- Click **"Apply"** to save the settings and start the container.  
- aIPTV should now be accessible at **`http://[UNRAID-IP]:5002/`**.

<br>

## 📜 Advanced Configuration

### 🔄 Customizing Environment Variables

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

- `latest` → Pulls the most recent stable version.  
- `development-latest` → Pulls the latest development branch version.

**Example Usage in Unraid:**

**Repository:** `<aiptv-image-if-you-have-it>:development-latest`

<br>

## 🔗 Need Help? Join the Community!
If you run into any issues or have questions, join our **[Discord Channel](https://discord.gg/tP3JcygCA8)** for support and discussion!