# PocketBase Docker Compose

This repository contains a Docker Compose configuration for running [PocketBase](https://pocketbase.io/), an open source backend for your next SaaS and Mobile app in seconds.

## Prerequisites

- Docker
- Docker Compose

## Quick Start

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/pocketbase-docker.git
   cd pocketbase-docker
   ```

2. Create the necessary directories:
   ```
   mkdir -p data public hooks
   ```

3. Start PocketBase:
   ```
   docker-compose up -d
   ```

PocketBase will now be running and accessible at `http://localhost:8090`.

## Configuration

The `docker-compose.yml` file includes the following configuration:

```yaml
version: "3.7"
services:
  pocketbase:
    image: ghcr.io/muchobien/pocketbase:latest
    container_name: pocketbase
    restart: always
    volumes:
      - /home/wback/pocketbase-docker/data:/pb_data
      - /home/wback/pocketbase-docker/public:/pb_public #optional
      - /home/wback/pocketbase-docker/hooks:/pb_hooks #optional
    healthcheck: #optional (recommended) since v0.10.0
      test: wget --no-verbose --tries=1 --spider http://localhost:8090/api/health || exit 1
      interval: 5s
      timeout: 5s
      retries: 5
    network_mode: host
```

### Volumes

- `/pb_data`: Contains PocketBase data and configurations.
- `/pb_public`: (Optional) For serving static files.
- `/pb_hooks`: (Optional) For custom server-side hooks.

Make sure to adjust the host paths in the `volumes` section to match your directory structure.

### Network

This configuration uses `network_mode: host`, which means PocketBase will use the host's network stack. This allows PocketBase to be accessed directly on port 8090 of your host machine.

### Health Check

A health check is configured to ensure the container is running properly. It checks the `/api/health` endpoint every 5 seconds.

## Customization

You can customize this setup by modifying the `docker-compose.yml` file. For example, you might want to:

- Change the container name
- Adjust volume mappings
- Modify health check parameters
- Add environment variables

## Updating PocketBase

To update to the latest version of PocketBase, pull the latest image and recreate the container:

```
docker-compose pull
docker-compose up -d
```

## Backup

The `/pb_data` directory contains all of your PocketBase data. Make sure to back up this directory regularly.

## Support

For issues related to PocketBase itself, please refer to the [official PocketBase documentation](https://pocketbase.io/docs/) or [GitHub repository](https://github.com/pocketbase/pocketbase).

For issues specific to this Docker setup, please open an issue in this repository.

## License

This Docker Compose configuration is provided under the MIT License. PocketBase itself has its own license, which can be found in its GitHub repository.
