# RetroArch-WebDAV-Sync

This repository contains a Docker Compose configuration for a secure WebDAV server using rclone and Tailscale. The WebDAV server is accessible exclusively through Tailscale, ensuring private, encrypted networking without exposing services to the public internet. This server was designed to provide a simple server for RetroArch configuration and save state sync, but it is a generic file server and can be used for anything.

## Features

- **WebDAV Server**: Powered by rclone, enabling a lightweight WebDAV server.
- **Secure Networking**: Access the WebDAV server securely via Tailscale's zero-config VPN.
- **Dockerized Setup**: Fully containerized environment using Docker Compose for easy setup, deployment, and portability. Tailscale is set up in userspace mode to reduce host configuration.

## Prerequisites

- Docker and Docker Compose installed on a server.
- Tailscale and RetroArch installed on your gaming systems.
- A Tailscale account
- Basic knowledge of Docker, Tailscale, and RetroArch

## Getting Started

Follow these instructions to set up and run the WebDAV server:

### 1. Clone the Repository

```bash
git clone https://github.com/0x000C/RetroArch-WebDAV-Sync.git
cd RetroArch-WebDAV-Sync
```

### 2. Get your Tailscale auth key

Go to your [Tailscale Admin Console Settings](https://login.tailscale.com/admin/settings/keys) and generate a one-time auth key for the next step. The reusable option can be generated to reduce effort if you need to do troubleshooting.

### 3. Set Up Your Environment

Create an `.env` file in the root of the project to store your environment variables:

```bash
TS_AUTHKEY=<your-tailscale-authkey>
CONTAINER_NAME=<your-container-name>
RESTART_POLICY=unless-stopped
```

The container name will act as a label on your host for this container and also act as the hostname of your WebDAV server.

### 4. Build and Run the Docker Containers

Run the following commands to start the WebDAV server and Tailscale services and follow the logs to make sure everything starts up correctly:

```bash
sudo docker compose up -d
sudo docker compose logs -f
```
### 5. Open WebDAV Server

Confirm that the WebDAV server is running and allow Tailscale to generate HTTPS certificates by navigating to your WebDAV server URL (e.g. `https://CONTAINER_NAME.auto-generated.ts.net/`) or Tailscale IP address (e.g. `100.xxx.xxx.xxx`). You can find these addresses on your [Tailscale Admin Console](https://login.tailscale.com/admin/machines).

### 6. Update RetroArch Configuration

In RetroArch, go to Settings->Saving->Cloud Sync set:

* Enable Cloud Sync to `ON`
* Cloud Sync Backend to `webdav`
* Cloud Storage URL to your WebDAV server URL.
* Leave Username and Password blank

Restart RetroArch and it will set up the necessary directories on the server.

Repeat these steps on any other devices you want to sync with.

## Data Directories

The WebDAV server saves files on the server in the `data/rclone` directory.

## Future Features

The following features are ideas for future development:

- **HTTP to HTTPS Redirect**: Ensure automatic redirection from HTTP to HTTPS within the Tailscale container setup.
- **Authentication**: Switch from rclone to a heavier server like nginx to add login credentials for securing the WebDAV server.

Feel free to contribute or suggest features by opening issues or submitting pull requests!

## Useful Links

- [Tailscale Auth Keys Documentation](https://tailscale.com/kb/1085/auth-keys)
- [Tailscale Docker Integration Documentation](https://tailscale.com/kb/1282/docker)
- [Guide: Running Tailscale Inside Docker](https://tailscale.com/blog/docker-tailscale-guide)
- [Tailscale Docker Hub Image](https://hub.docker.com/r/tailscale/tailscale)
- [rclone Docker Hub Image](https://hub.docker.com/r/rclone/rclone)
- [rclone WebDAV Serve Command Documentation](https://rclone.org/commands/rclone_serve_webdav/)
- [RetroArch Cloud Sync Documentation](https://docs.libretro.com/guides/retroarch-cloud-sync/)

## License

This project is licensed under the Apache 2.0 License. See the [LICENSE](LICENSE) file for details.
