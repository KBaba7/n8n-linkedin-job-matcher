# N8N LinkedIn Job Matcher with Telegram

An N8N automation that uses Apify MCP server to scrape LinkedIn jobs matching your resume and preferences via Telegram chat interface.

## Architecture

```
Telegram Bot → N8N Workflow → Apify MCP Server → LinkedIn Scraper → Job Matching → Telegram Response
```

## Prerequisites

1. **N8N Instance** (self-hosted or cloud)
2. **Telegram Bot Token** (from @BotFather)
3. **Apify API Token** (from apify.com)
4. **MCP Server Setup** (Apify MCP server)
5. **Groq API Key** (for resume parsing and job matching - FREE tier available!)

## Quick Start (5 Minutes)

### 🚀 Easiest Way: Deploy to Render.com (FREE)

**See [QUICK-START-RENDER.md](QUICK-START-RENDER.md) for detailed guide**

1. Sign up at [render.com](https://render.com) (free)
2. Deploy N8N with one click
3. Import `workflow.json`
4. Add your API keys
5. Start using via Telegram!

### 🏠 Other Hosting Options

**See [FREE-HOSTING.md](FREE-HOSTING.md) for all options:**
- **Render.com** - Easiest, free forever (recommended)
- **Railway.app** - Best performance, $5 free credit
- **Fly.io** - Most generous free tier, no sleep
- **Local + ngrok** - Run on your computer
- **Replit** - Browser-based, instant start
- **Koyeb** - New platform, generous free tier

## Setup Instructions

### 1. Get API Keys (All FREE)

**Telegram Bot**
```bash
# Talk to @BotFather on Telegram
/newbot
# Follow prompts to get your bot token
```

**Apify API**
- Sign up at [apify.com](https://apify.com)
- Go to Settings → API Token
- Copy token

**Groq API**
- Sign up at [console.groq.com](https://console.groq.com)
- Go to API Keys
- Create new key

### 2. Deploy N8N

Choose your hosting platform from [FREE-HOSTING.md](FREE-HOSTING.md)

### 3. Import Workflow

1. Access your N8N instance
2. Click "Import from File"
3. Upload `workflow.json`
4. Configure credentials:
   - Telegram Bot Token
   - Apify API Token
   - Groq API Key

## Usage

### Complete User Flow

**Everything happens in Telegram! See [USER-FLOW.md](USER-FLOW.md) for detailed guide.**

**Quick Overview:**

1. **Start Bot**: Send `/start` to your Telegram bot
2. **Upload Resume**: Attach PDF/TXT/DOCX file in chat (📎 button)
3. **Provide Criteria**: Type job preferences naturally
4. **Get Matches**: Receive top 5 matched jobs with scores
5. **Apply**: Click links to apply on LinkedIn

**Visual Guide:** See [SCREENSHOTS.md](SCREENSHOTS.md) for what users see

### Start Conversation

Send `/start` to your Telegram bot

### Upload Resume

Send your resume as a PDF or text file

### Specify Job Criteria

```
Job Title: Software Engineer, Backend Developer
Location: San Francisco, Remote
Level: Mid-Level, Senior
```

### Receive Matched Jobs

Bot will return:
- Job title and company
- Location and work type
- Match score with reasoning
- Application link
- Key requirements comparison

## Features

- **Resume parsing** - Supports PDF, TXT, DOCX, and more
- **Skill extraction** - AI-powered analysis of experience and skills
- **LinkedIn job scraping** - Via Apify actors
- **Intelligent job matching** - AI compares resume to job requirements
- **Conversational interface** - Easy Telegram chat
- **Match scoring** - 0-100% match with explanations
- **Direct application links** - One-click apply

## File Structure

```
n8n-linkedin-job-matcher/
├── README.md
├── workflow.json           # N8N workflow
├── mcp-config.json        # MCP server configuration
├── telegram-commands.md   # Bot command reference
└── example-resume.txt     # Sample resume format
```
