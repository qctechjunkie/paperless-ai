# Paperless-NGX-AI Docker Setup

AI-powered document processing service for Paperless-NGX that automatically analyzes and processes documents.

## Prerequisites

- Docker and Docker Compose installed

## Quick Start

1. **Clone or download this repository**

2. **Create your environment file**
   ```bash
   cp .env.example .env
   ```

3. **Configure your environment variables**

   Edit the `.env` file to customize your setup (see Configuration section below)

4. **Start the service**
   ```bash
   docker-compose up -d
   ```

5. **Check the logs**
   ```bash
   docker-compose logs -f
   ```

## Configuration

All configuration is done through the `.env` file.

### Docker Settings

#### Container Configuration
```env
CONTAINER_NAME=paperless-ai        # Name of the Docker container
EXTERNAL_PORT=3000                 # Port to access the service on your host
PAPERLESS_AI_PORT=3000            # Internal container port (usually keep as 3000)
PUID=1000                          # User ID for file permissions
PGID=1000                          # Group ID for file permissions
```

**PUID/PGID**: Set these to match your user's UID/GID to ensure proper file permissions. Find yours with:
```bash
id -u  # Returns PUID
id -g  # Returns PGID
```

#### Data Storage
```env
DATA_PATH=./data
```

Choose one of these options:
- **Relative path**: `./data` (creates data folder in project directory)
- **Absolute path**: `/volume1/docker/paperlessngxai` (Linux/NAS)
- **Windows path**: `C:\Users\YourName\Docker\paperless-ai\data`

#### RAG Service (Optional)
```env
RAG_SERVICE_URL=http://localhost:8000
RAG_SERVICE_ENABLED=true
```

The RAG (Retrieval-Augmented Generation) service provides enhanced document processing capabilities. Set `RAG_SERVICE_ENABLED=false` if not using this feature.

## Security Features

The Docker container is configured with enhanced security:
- **Capability Drop**: Drops all Linux capabilities and only grants what's needed
- **No New Privileges**: Prevents privilege escalation attacks
- **User Permissions**: Runs with specified PUID/PGID instead of root

## Healthcheck

The service includes a healthcheck that verifies the application is responding. The healthcheck:
- Runs every 10 seconds
- Times out after 5 seconds
- Retries 3 times before marking as unhealthy
- Waits 90 seconds on startup before first check

## Troubleshooting

### Service won't start
- Check that all required environment variables are set
- Verify the DATA_PATH exists and is writable
- Check the logs: `docker-compose logs -f`

### Permission errors
- Ensure PUID/PGID match your user's IDs (use `id -u` and `id -g` commands)
- Check that the DATA_PATH directory has correct permissions
- On Linux/NAS: `sudo chown -R 1000:1000 /path/to/data`

### Port conflicts
- If EXTERNAL_PORT is already in use, change it to a different port in `.env`
- Example: `EXTERNAL_PORT=3001`

## Docker Compose Commands

```bash
# Start the service
docker-compose up -d

# Stop the service
docker-compose down

# View logs
docker-compose logs -f

# Restart the service
docker-compose restart

# Update to latest image
docker-compose pull
docker-compose up -d
```

## Security Notes

- Never commit your `.env` file to version control (it's already in `.gitignore`)
- Use strong values for sensitive configuration
- Consider using Docker secrets for sensitive data in production

## Links

- [Paperless-NGX](https://github.com/paperless-ngx/paperless-ngx)
- [Docker Documentation](https://docs.docker.com/)

## Support

For issues with:
- **Paperless-NGX**: Visit the [Paperless-NGX GitHub](https://github.com/paperless-ngx/paperless-ngx)
- **This Docker image**: Visit the [clusterzx/paperless-ai](https://hub.docker.com/r/clusterzx/paperless-ai) Docker Hub page

## License

This setup uses the [clusterzx/paperless-ai](https://hub.docker.com/r/clusterzx/paperless-ai) Docker image. Please refer to the image's documentation for licensing information.
