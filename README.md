# Paperless-NGX-AI Docker Setup

AI-powered document processing service for Paperless-NGX. This service automatically analyzes documents using AI to extract metadata, including titles, correspondents, tags, document types, and dates.

## Features

- **AI-Powered Analysis**: Automatically extracts metadata from documents using AI
- **Multiple AI Providers**: Support for OpenAI, Ollama, Azure OpenAI, and custom API endpoints
- **Flexible Scheduling**: Configure document scanning intervals using cron expressions
- **Customizable Prompts**: Tailor the AI's behavior with custom system prompts
- **Feature Toggles**: Enable/disable specific processing features (tagging, correspondents, titles, etc.)
- **Custom Fields**: Support for custom field extraction
- **Automatic Tagging**: Optionally add an "ai-processed" tag to analyzed documents

## Prerequisites

- Docker and Docker Compose installed
- A running Paperless-NGX instance
- API token from your Paperless-NGX instance
- Access to an AI provider (OpenAI, Ollama, Azure, or custom API)

## Quick Start

1. **Clone or download this repository**

2. **Create your environment file**
   ```bash
   cp .env.example .env
   ```

3. **Configure your environment variables** (see Configuration section below)

4. **Update the volume path** in `compose.yaml` to match your system:
   ```yaml
   volumes:
     - /your/path/to/data:/app/data:rw
   ```

5. **Start the service**
   ```bash
   docker-compose up -d
   ```

6. **Check the logs**
   ```bash
   docker-compose logs -f
   ```

## Configuration

### Required Settings

Edit the `.env` file and configure the following required settings:

#### Paperless-NGX Connection
```env
PAPERLESS_API_URL=http://your-paperless-host:port/api
PAPERLESS_API_TOKEN=your_paperless_api_token_here
PAPERLESS_USERNAME=your_email@example.com
```

To get your Paperless API token:
1. Log into your Paperless-NGX instance
2. Go to Settings ’ API Tokens
3. Create a new token or copy an existing one

#### AI Provider

Choose one of the following AI providers by setting `AI_PROVIDER`:

**OpenAI**
```env
AI_PROVIDER=openai
OPENAI_API_KEY=sk-your-openai-api-key-here
OPENAI_MODEL=gpt-4o-mini
```

**Ollama (Local)**
```env
AI_PROVIDER=ollama
OLLAMA_API_URL=http://your-ollama-host:11434
OLLAMA_MODEL=mistral
```

**Custom API**
```env
AI_PROVIDER=custom
CUSTOM_API_KEY=your-custom-api-key
CUSTOM_BASE_URL=http://your-custom-api-host:port
CUSTOM_MODEL=openai/gpt-4o-mini
```

**Azure OpenAI**
```env
AI_PROVIDER=azure
AZURE_ENDPOINT=https://your-resource.openai.azure.com
AZURE_API_KEY=your-azure-api-key
AZURE_DEPLOYMENT_NAME=your-deployment-name
AZURE_API_VERSION=2024-02-01
```

### Optional Settings

#### Scanning Schedule
```env
# Cron format: minute hour day month day-of-week
# Default: every 30 minutes
SCAN_INTERVAL=*/30 * * * *
```

#### Feature Toggles
```env
ACTIVATE_TAGGING=yes              # Extract and apply tags
ACTIVATE_CORRESPONDENTS=yes       # Extract correspondent information
ACTIVATE_DOCUMENT_TYPE=yes        # Determine document type
ACTIVATE_TITLE=yes                # Generate document title
ACTIVATE_CUSTOM_FIELDS=yes        # Process custom fields
```

#### Processing Options
```env
PROCESS_PREDEFINED_DOCUMENTS=no   # Process documents that already have metadata
USE_EXISTING_DATA=no              # Use existing metadata as context for AI
ADD_AI_PROCESSED_TAG=yes          # Add a tag to processed documents
AI_PROCESSED_TAG_NAME=ai-processed # Name of the processing tag
DISABLE_AUTOMATIC_PROCESSING=no   # Disable scheduled automatic processing
```

## Port Configuration

The service is accessible on port `3747` by default. To change this, edit the port mapping in `compose.yaml`:

```yaml
ports:
  - 3747:3000  # Change 3747 to your desired port
```

## Volume Configuration

By default, the data is stored at `/volume1/docker/paperlessngxai`. Update this path in `compose.yaml`:

```yaml
volumes:
  - /your/path/to/data:/app/data:rw
```

## Customizing the AI Prompt

The `SYSTEM_PROMPT` environment variable controls how the AI analyzes documents. You can customize it to:
- Change the language of extracted metadata
- Adjust tagging preferences
- Modify title formatting rules
- Add custom extraction rules

## Healthcheck

The service includes a healthcheck that verifies the application is responding on port 3000. The healthcheck:
- Runs every 10 seconds
- Times out after 5 seconds
- Retries 3 times before marking as unhealthy
- Waits 90 seconds on startup before first check

## Troubleshooting

### Service won't start
- Check that all required environment variables are set
- Verify your Paperless-NGX API URL is accessible
- Ensure your API token is valid

### Documents aren't being processed
- Check the logs: `docker-compose logs -f`
- Verify your AI provider credentials are correct
- Ensure the SCAN_INTERVAL is set correctly
- Check that automatic processing isn't disabled

### AI provider errors
- Verify your API keys are valid
- Check that the API endpoints are accessible from the container
- Review rate limits for your AI provider

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

- Never commit your `.env` file to version control
- Keep your API tokens secure
- Use strong API keys for the service
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
