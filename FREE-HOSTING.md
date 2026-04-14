# Free Hosting Options (No AWS/GCP Required)

## Option 1: Render.com (Recommended - Easiest)

### ✅ Pros
- **FREE tier**: 750 hours/month
- Zero configuration
- Auto-deploy from GitHub
- Built-in PostgreSQL
- HTTPS included

### 📋 Setup Steps

1. **Create Account**
   - Go to [render.com](https://render.com)
   - Sign up with GitHub (free)

2. **Deploy N8N**
   - Click "New +" → "Web Service"
   - Connect your GitHub repo (or use Render's template)
   - Or use this direct link: [Deploy N8N Template](https://render.com/deploy?repo=https://github.com/n8n-io/n8n)

3. **Configure**
   ```yaml
   Name: n8n-job-matcher
   Environment: Docker
   Region: Oregon (US West)
   Branch: main
   Build Command: (leave empty)
   Start Command: n8n start
   ```

4. **Add Environment Variables**
   ```
   N8N_BASIC_AUTH_ACTIVE=true
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=your_secure_password
   TELEGRAM_BOT_TOKEN=your_telegram_token
   APIFY_API_TOKEN=your_apify_token
   GROQ_API_KEY=your_groq_key
   WEBHOOK_URL=https://your-app.onrender.com
   ```

5. **Deploy**
   - Click "Create Web Service"
   - Wait 5-10 minutes
   - Access at: `https://your-app.onrender.com`

### ⚠️ Free Tier Limitations
- Spins down after 15 min inactivity
- First request after sleep takes ~30 seconds
- 750 hours/month (enough for 24/7 if only one service)

---

## Option 2: Railway.app (Best Performance)

### ✅ Pros
- **$5 FREE credit/month** (enough for light usage)
- No sleep/spin-down
- Faster than Render
- Easy GitHub integration

### 📋 Setup Steps

1. **Create Account**
   - Go to [railway.app](https://railway.app)
   - Sign up with GitHub

2. **Deploy from Template**
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Or use: [Railway N8N Template](https://railway.app/template/n8n)

3. **Configure**
   - Railway auto-detects Docker
   - Add environment variables in dashboard

4. **Environment Variables**
   ```
   N8N_BASIC_AUTH_ACTIVE=true
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=your_password
   TELEGRAM_BOT_TOKEN=your_token
   APIFY_API_TOKEN=your_token
   GROQ_API_KEY=your_key
   ```

5. **Get URL**
   - Railway provides: `https://your-app.up.railway.app`

### 💰 Cost Estimate
- Free: $5 credit/month
- Typical usage: $2-3/month (well within free tier)

---

## Option 3: Fly.io (Most Generous Free Tier)

### ✅ Pros
- **FREE**: 3 shared VMs, 3GB storage
- No credit card required
- Global edge network
- No sleep/spin-down

### 📋 Setup Steps

1. **Install Fly CLI**
   ```bash
   # macOS
   brew install flyctl
   
   # Linux
   curl -L https://fly.io/install.sh | sh
   
   # Windows
   powershell -Command "iwr https://fly.io/install.ps1 -useb | iex"
   ```

2. **Sign Up**
   ```bash
   flyctl auth signup
   ```

3. **Create fly.toml**
   ```bash
   cd n8n-linkedin-job-matcher
   flyctl launch
   ```

4. **Configure fly.toml**
   ```toml
   app = "n8n-job-matcher"
   
   [build]
     image = "n8nio/n8n:latest"
   
   [env]
     N8N_PORT = "8080"
     N8N_PROTOCOL = "https"
   
   [[services]]
     internal_port = 5678
     protocol = "tcp"
   
     [[services.ports]]
       handlers = ["http"]
       port = 80
   
     [[services.ports]]
       handlers = ["tls", "http"]
       port = 443
   ```

5. **Set Secrets**
   ```bash
   flyctl secrets set N8N_BASIC_AUTH_ACTIVE=true
   flyctl secrets set N8N_BASIC_AUTH_USER=admin
   flyctl secrets set N8N_BASIC_AUTH_PASSWORD=your_password
   flyctl secrets set TELEGRAM_BOT_TOKEN=your_token
   flyctl secrets set APIFY_API_TOKEN=your_token
   flyctl secrets set GROQ_API_KEY=your_key
   ```

6. **Deploy**
   ```bash
   flyctl deploy
   ```

7. **Access**
   ```bash
   flyctl open
   # Opens: https://n8n-job-matcher.fly.dev
   ```

---

## Option 4: Local Machine (Completely Free)

### ✅ Pros
- 100% free
- Full control
- No limitations
- Works offline

### 📋 Setup Steps

1. **Install Docker Desktop**
   - Download from [docker.com](https://www.docker.com/products/docker-desktop)
   - Install and start

2. **Run N8N**
   ```bash
   cd n8n-linkedin-job-matcher
   docker-compose up -d
   ```

3. **Access Locally**
   - Open: http://localhost:5678

4. **Expose to Internet (for Telegram webhooks)**
   
   **Option A: ngrok (Easiest)**
   ```bash
   # Install ngrok
   brew install ngrok  # macOS
   # or download from ngrok.com
   
   # Create free account at ngrok.com
   ngrok config add-authtoken YOUR_TOKEN
   
   # Expose N8N
   ngrok http 5678
   
   # Use the https URL for Telegram webhook
   # Example: https://abc123.ngrok.io
   ```
   
   **Option B: Cloudflare Tunnel (Better)**
   ```bash
   # Install cloudflared
   brew install cloudflare/cloudflare/cloudflared
   
   # Login
   cloudflared tunnel login
   
   # Create tunnel
   cloudflared tunnel create n8n-tunnel
   
   # Run tunnel
   cloudflared tunnel --url http://localhost:5678
   
   # Get permanent URL
   ```

### ⚠️ Considerations
- Computer must stay on
- Need stable internet
- Use ngrok/cloudflare for webhooks

---

## Option 5: Replit (Quickest Start)

### ✅ Pros
- **FREE tier** available
- No installation needed
- Browser-based IDE
- Instant deployment

### 📋 Setup Steps

1. **Create Account**
   - Go to [replit.com](https://replit.com)
   - Sign up (free)

2. **Create Repl**
   - Click "Create Repl"
   - Select "Docker"
   - Name: "n8n-job-matcher"

3. **Add Dockerfile**
   ```dockerfile
   FROM n8nio/n8n:latest
   
   ENV N8N_PORT=5678
   ENV N8N_PROTOCOL=https
   
   EXPOSE 5678
   
   CMD ["n8n", "start"]
   ```

4. **Configure Secrets**
   - Click "Secrets" (lock icon)
   - Add:
     - `N8N_BASIC_AUTH_USER`
     - `N8N_BASIC_AUTH_PASSWORD`
     - `TELEGRAM_BOT_TOKEN`
     - `APIFY_API_TOKEN`
     - `GROQ_API_KEY`

5. **Run**
   - Click "Run"
   - Access via Replit URL

### ⚠️ Free Tier Limitations
- Sleeps after 1 hour inactivity
- Limited CPU/RAM
- Public by default

---

## Option 6: Koyeb (New & Generous)

### ✅ Pros
- **FREE**: 1 web service, 1 database
- No credit card
- Auto-scaling
- Global CDN

### 📋 Setup Steps

1. **Sign Up**
   - Go to [koyeb.com](https://www.koyeb.com)
   - Create free account

2. **Deploy**
   - Click "Create App"
   - Select "Docker"
   - Image: `n8nio/n8n:latest`

3. **Environment Variables**
   ```
   N8N_BASIC_AUTH_ACTIVE=true
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=your_password
   TELEGRAM_BOT_TOKEN=your_token
   APIFY_API_TOKEN=your_token
   GROQ_API_KEY=your_key
   ```

4. **Deploy**
   - Click "Deploy"
   - Get URL: `https://your-app.koyeb.app`

---

## Comparison Table

| Platform | Free Tier | Sleep? | Setup Time | Best For |
|----------|-----------|--------|------------|----------|
| **Render** | 750h/month | Yes (15min) | 5 min | Beginners |
| **Railway** | $5 credit | No | 3 min | Best performance |
| **Fly.io** | 3 VMs | No | 10 min | Advanced users |
| **Local + ngrok** | Unlimited | No | 5 min | Development |
| **Replit** | Limited | Yes (1hr) | 2 min | Quick test |
| **Koyeb** | 1 service | No | 5 min | Production |

---

## Recommended Setup for Your Use Case

### For Testing (1-2 weeks)
→ **Replit** or **Local + ngrok**

### For Personal Use (ongoing)
→ **Render** (free forever, just slower wake-up)

### For Production/Serious Use
→ **Railway** ($5/month credit is enough)

### For Maximum Free Tier
→ **Fly.io** (no sleep, generous limits)

---

## Next Steps

1. Choose a platform above
2. Follow the setup steps
3. Import your `workflow.json`
4. Configure Telegram webhook with your new URL
5. Start matching jobs!

Need help with any specific platform? Let me know!
