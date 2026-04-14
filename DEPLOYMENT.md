# Deployment Guide

## Quick Start (Docker)

### 1. Clone and Setup

```bash
cd n8n-linkedin-job-matcher
cp .env.example .env
# Edit .env with your credentials
```

### 2. Start N8N

```bash
docker-compose up -d
```

### 3. Access N8N

Open http://localhost:5678 in your browser

### 4. Import Workflow

1. Login to N8N (admin/changeme)
2. Click "Import from File"
3. Select `workflow.json`
4. Configure credentials

## Credential Setup

### Telegram Bot

1. Create bot via @BotFather
2. Copy token
3. In N8N: Credentials → Add → Telegram
4. Paste token

### Apify API

1. Sign up at apify.com
2. Get API token from Settings
3. In N8N: Credentials → Add → HTTP Header Auth
4. Name: `Authorization`
5. Value: `Bearer YOUR_TOKEN`

### Groq API

1. Sign up at console.groq.com (FREE)
2. Create API key from dashboard
3. In N8N: Credentials → Add → HTTP Header Auth
4. Name: `Groq API`
5. Header: `Authorization`
6. Value: `Bearer gsk_your_key_here`
7. Save

See GROQ-SETUP.md for detailed instructions

## MCP Server Setup

### Option 1: Local Installation

```bash
npm install -g @apify/mcp-server
```

### Option 2: Docker

Add to docker-compose.yml:

```yaml
mcp-server:
  image: node:18-alpine
  command: npx -y @apify/mcp-server
  environment:
    - APIFY_API_TOKEN=${APIFY_API_TOKEN}
  networks:
    - n8n-network
```

## Production Deployment

### Using Railway

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Deploy
railway up
```

### Using Render

1. Connect GitHub repo
2. Select Docker deployment
3. Add environment variables
4. Deploy

### Using DigitalOcean

```bash
# Create droplet
doctl compute droplet create n8n-job-matcher \
  --image docker-20-04 \
  --size s-2vcpu-4gb \
  --region nyc1

# SSH and deploy
ssh root@your-droplet-ip
git clone your-repo
cd n8n-linkedin-job-matcher
docker-compose up -d
```

## Environment Variables

Required:
- `TELEGRAM_BOT_TOKEN`
- `APIFY_API_TOKEN`
- `GROQ_API_KEY`

Optional:
- `N8N_HOST`
- `N8N_PROTOCOL`
- `DATABASE_URL`
- `REDIS_URL`

## Monitoring

### Check Logs

```bash
docker-compose logs -f n8n
```

### Health Check

```bash
curl http://localhost:5678/healthz
```

## Troubleshooting

### N8N won't start
```bash
docker-compose down
docker-compose up -d
```

### Workflow fails
- Check credentials
- Verify API tokens
- Check Apify quota

### Telegram not responding
- Verify bot token
- Check webhook URL
- Restart workflow
