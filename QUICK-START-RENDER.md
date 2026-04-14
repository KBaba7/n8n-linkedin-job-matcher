# Quick Start with Render.com (5 Minutes)

The easiest way to get started with zero configuration!

## Step-by-Step Guide

### 1. Sign Up for Render (30 seconds)

1. Go to [render.com](https://render.com)
2. Click "Get Started"
3. Sign up with GitHub (recommended) or email
4. Verify email if needed

### 2. Deploy N8N (2 minutes)

**Option A: One-Click Deploy (Easiest)**

1. Click this button: [![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/n8n-io/n8n)

2. Or manually:
   - Click "New +" → "Web Service"
   - Select "Deploy an existing image from a registry"
   - Docker Image: `n8nio/n8n:latest`
   - Name: `n8n-job-matcher`
   - Region: Choose closest to you
   - Instance Type: **Free**

### 3. Configure Environment Variables (2 minutes)

In the Render dashboard, add these environment variables:

```bash
# N8N Authentication
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=YourSecurePassword123

# N8N Configuration
N8N_PORT=5678
N8N_PROTOCOL=https
WEBHOOK_URL=https://YOUR-APP-NAME.onrender.com

# Your API Keys
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
APIFY_API_TOKEN=your_apify_token
GROQ_API_KEY=your_groq_api_key

# Optional: Timezone
GENERIC_TIMEZONE=America/New_York
```

**Where to get API keys:**
- **Telegram**: Talk to [@BotFather](https://t.me/botfather) on Telegram
- **Apify**: Sign up at [apify.com](https://apify.com) → Settings → API Token
- **Groq**: Sign up at [console.groq.com](https://console.groq.com) → API Keys

### 4. Deploy (30 seconds)

1. Click "Create Web Service"
2. Wait 5-10 minutes for deployment
3. You'll get a URL like: `https://n8n-job-matcher.onrender.com`

### 5. Access N8N (30 seconds)

1. Open your Render URL
2. Login with:
   - Username: `admin`
   - Password: (what you set above)

### 6. Import Workflow (1 minute)

1. In N8N, click "Workflows" → "Import from File"
2. Upload `workflow.json` from this repo
3. Click "Import"

### 7. Configure Credentials (2 minutes)

#### Telegram Bot
1. Click on any Telegram node
2. Click "Create New Credential"
3. Paste your bot token
4. Save

#### Apify API
1. Click on "Scrape LinkedIn via Apify" node
2. Click "Create New Credential" → "HTTP Header Auth"
3. Name: `Apify API`
4. Header Name: `Authorization`
5. Header Value: `Bearer YOUR_APIFY_TOKEN`
6. Save

#### Groq API
1. Click on any AI node (Parse Resume, Parse Job Criteria, or Match Jobs)
2. Click "Create New Credential" → "HTTP Header Auth"
3. Name: `Groq API`
4. Header Name: `Authorization`
5. Header Value: `Bearer YOUR_GROQ_KEY`
6. Save

### 8. Activate Workflow (10 seconds)

1. Toggle the workflow to "Active" (switch in top right)
2. Done! 🎉

### 9. Test Your Bot (1 minute)

1. Open Telegram
2. Search for your bot
3. Send `/start`
4. Upload a resume
5. Provide job criteria
6. Get matched jobs!

---

## Troubleshooting

### "Service Unavailable" after deployment
- Wait 2-3 more minutes, Render is still starting
- Check "Logs" tab in Render dashboard

### Telegram webhook not working
- Make sure `WEBHOOK_URL` matches your Render URL
- Ensure workflow is "Active"
- Check Telegram node has correct credentials

### "Rate limit exceeded" from Apify
- Free tier: 100 runs/month
- Wait or upgrade Apify plan

### Workflow not triggering
- Check workflow is Active (green toggle)
- Verify Telegram bot token is correct
- Look at "Executions" tab for errors

---

## Free Tier Limitations

### Render Free Tier
- ✅ 750 hours/month (enough for 24/7)
- ✅ HTTPS included
- ✅ Auto-deploy from Git
- ⚠️ Spins down after 15 min inactivity
- ⚠️ First request after sleep: ~30 sec wake-up

### How to Handle Sleep
The bot will sleep after 15 minutes of no activity. When a user sends a message:
1. First message wakes up the service (~30 seconds)
2. Bot responds: "Waking up, please wait..."
3. User sends message again
4. Bot processes normally

**To avoid sleep:**
- Upgrade to Render paid plan ($7/month for always-on)
- Or use Railway.app (no sleep on free tier)

---

## Keeping It Running 24/7 (Optional)

### Option 1: UptimeRobot (Free)
1. Sign up at [uptimerobot.com](https://uptimerobot.com)
2. Add monitor:
   - Type: HTTP(s)
   - URL: `https://your-app.onrender.com`
   - Interval: 5 minutes
3. This pings your app every 5 min, preventing sleep

### Option 2: Cron-job.org (Free)
1. Sign up at [cron-job.org](https://cron-job.org)
2. Create job:
   - URL: `https://your-app.onrender.com/healthz`
   - Interval: Every 10 minutes
3. Keeps service awake

---

## Updating Your Workflow

### Method 1: Via N8N UI
1. Make changes in N8N
2. Save workflow
3. Changes persist automatically

### Method 2: Via GitHub (Auto-deploy)
1. Connect Render to your GitHub repo
2. Push changes to `workflow.json`
3. Render auto-deploys

---

## Monitoring

### View Logs
1. Go to Render dashboard
2. Click your service
3. Click "Logs" tab
4. See real-time logs

### Check Executions
1. In N8N, click "Executions"
2. See all workflow runs
3. Debug failed executions

---

## Cost Breakdown

| Item | Cost |
|------|------|
| Render hosting | **FREE** (750h/month) |
| Telegram Bot | **FREE** |
| Groq API | **FREE** (14,400 req/day) |
| Apify | **FREE** (100 runs/month) |
| **Total** | **$0/month** 🎉 |

---

## Next Steps

1. ✅ Deploy to Render
2. ✅ Import workflow
3. ✅ Configure credentials
4. ✅ Test with Telegram
5. 🚀 Start finding jobs!

---

## Need Help?

- **Render Issues**: [render.com/docs](https://render.com/docs)
- **N8N Issues**: [docs.n8n.io](https://docs.n8n.io)
- **Telegram Bot**: [core.telegram.org/bots](https://core.telegram.org/bots)

Happy job hunting! 🎯
