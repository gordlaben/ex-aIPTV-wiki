# ⚠️ **DEPRECATION WARNING** ⚠️

> **🚨 Important Notice:**
> This project is **deprecated** and is no longer actively maintained or supported. Use at your own risk.
> This repository is a **fork of the WIKI** previously used for the project.
> The owner of this page, **gordlaben**, has **no affiliation** with the original project and bears no responsibility for its content or maintenance.



<br>

# 🌐 aIPTV – M3U Proxy Server

**aIPTV** is a modern M3U proxy server designed to streamline m3u playlist management. It enables users to process, optimize, and serve M3U playlists with ease. Featuring seamless **EPG integration**, a **React-based UI**, and **hardware-accelerated streaming**, aIPTV offers a fast and efficient way to organize and enhance m3u playlist content. Built with **Node.js** and **PostgreSQL**, it ensures high performance and scalability while providing **Docker support** for effortless deployment.  

<br>

## 🌟 Key Features
- **🧠 Smart M3U Processing** – Efficient playlist parsing and management. 
- **🔄 Backup Streams** – Intelligent failover system that dynamically switches to backup sources when a stream becomes unavailable.
- **📺 EPG Integration** – Supports electronic program guides, including **[Schedules Direct](https://schedulesdirect.org/) API** integration.
- **🎨 Modern UI** – Built with React and Material-UI for a seamless experience.  
- **🚀 High Performance** – Optimized with Node.js and PostgreSQL.  
- **🎯 Hardware Acceleration** – Enhanced streaming with *GPU support*.  
- **🐳 Docker Ready** – Simple deployment using Docker and Docker Compose.  

<br>

## 🔹 Use Cases  
**aIPTV** is a versatile solution that can be used for various m3u playlist and streaming scenarios, including:  

1. **M3U Server Proxy** – Act as an intermediary to optimize and serve m3u playlists.  
2. **VPN Proxy for Streams** – Route all streams through a VPN to bypass restrictions and ensure privacy.  
3. **Multi-Source Management** – Merge multiple m3u playlist sources into a single, well-organized playlist.  
4. **Fallback & Backup Streams** – Automatically switch to backup streams if a primary stream becomes unavailable.  
5. **EPG Data Integration** – Add and manage Electronic Program Guide (EPG) sources directly.  
6. **Custom Stream Profiles** – Optimize streams for platforms like **Plex, Emby, and Jellyfin** with tailored profiles.   

<br>

## 📚 Getting Started

> **⚠️ Warning:**  
> This setup is currently in **testing** and may require adjustments. We encourage everyone to join our **[Discord Server](https://discord.gg/tP3JcygCA8)** and become part of our **community**.  Your **feedback** is invaluable to us, so please report any **issues**, **bugs**, or suggestions you may have to help improve the service.

## 📥 **Installation Guides** (Download/Setup related)
- 🐳 **[Docker & Docker Compose](Docs/Installation/docker.md)**
    - ➡️ [with **Gluetun** as VPN](Docs/Installation/gluetun.md)
    - ➡️ [with **NGINX** as proxy](Docs/Installation/nginx.md)
    - ➡️ [with **Traefik** as proxy](Docs/Installation/traefik.md)
    - ➡️ [with **Caddy** as proxy](Docs/Installation/caddy.md)
- 💄 **[Unraid](Docs/Installation/unraid.md)**

## 🖥️ **User Interface / Pages** (UI and navigation-related)
- 🖥️ **[Playlist Manager aka "Homepage"](Docs/UserInterface/Pages/playlist.md)**
- ✏️ ~~Schedules Direct~~ (Work In Progress)
- ✏️ ~~Stats~~ (Work In Progress)
- ✏️ ~~Logs~~ (Work In Progress)

## 📖 **How-To** (Guides and tutorials)
### 📺 **[Visual Guides](Docs/HowTo/howto.md)**
- [Add a Playlist](Docs/HowTo/howto.md#-add-a-playlist)  
- [Update Playlist Groups](Docs/HowTo/howto.md#-update-playlist-groups)  
- [Add Playlist Profiles](Docs/HowTo/howto.md#-add-playlist-profiles)  
- [Add Custom Channel Manually](Docs/HowTo/howto.md#-add-custom-channel-manually)  
- [Create Channels from Streams](Docs/HowTo/howto.md#-create-channels-from-streams)  
- [Add Streams to Channels](Docs/HowTo/howto.md#-add-streams-to-channels)  
- [Edit Global Streaming Settings](Docs/HowTo/howto.md#-edit-global-streaming-settings)  
- [Add Profiles](Docs/HowTo/howto.md#-add-profiles)  
- [Change Streaming Profiles](Docs/HowTo/howto.md#-change-streaming-profiles)  
- [Download Profile Playlist File](Docs/HowTo/howto.md#-download-profile-playlist-file)  
- [Change Profile Playlist Export Options](Docs/HowTo/howto.md#-change-profile-playlist-export-options)
- [Renumber Channels](Docs/HowTo/howto.md#-renumber-channels)

## 🧪 **Test Files for Validation** (Sample files to verify your setup and functionality)
These sample files are available for testing and validating your setup. You may download them directly or use their RAW URLs for import.

| M3U             | EPG          |
|-----------------|--------------|
| [fake_playlist_1.m3u](Testers/fake_playlist_1.m3u) | [fake_epg_1.xml](Testers/fake_epg_1.xml) |
| [fake_playlist_2.m3u](Testers/fake_playlist_2.m3u) | [fake_epg_2.xml](Testers/fake_epg_2.xml) |
| [fake_playlist_3.m3u](Testers/fake_playlist_3.m3u) | [fake_epg_3.xml](Testers/fake_epg_3.xml) |

## ⚠️ **Troubleshooting and Issues** (Errors and problem-solving)
- ⚠️ **[Generic Error Codes & Troubleshooting Guide](Docs/Generic/error_codes.md)**

<br>

## ⚠️ Disclaimer
aIPTV is a tool designed **only** for managing and organizing user-supplied playlists. It does **not** provide, distribute, or facilitate access to any content. Users must ensure compliance with applicable laws when using aIPTV.

<br>

## 🔗 Community & Support
Join the discussion and get support on our **[Discord Channel](https://discord.gg/tP3JcygCA8)**.

<br>

This repository serves as an introduction and guide for setting up aIPTV. More detailed technical documentation and troubleshooting guides will be added as needed.