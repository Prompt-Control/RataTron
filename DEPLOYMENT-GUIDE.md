# 🚀 Ratatron Live - Production Deployment Guide

Complete guide for deploying Ratatron Live to production for your support team.

---

## 📋 Quick Links

- **DigitalOcean:** $5/month, easiest setup
- **AWS EC2:** Scalable, enterprise-grade  
- **Heroku:** Simplest deployment
- **Docker:** Most flexible

---

## ✅ Pre-Deployment Checklist

### Google Cloud Setup
- [ ] OAuth 2.0 credentials obtained
- [ ] Google Drive API enabled
- [ ] Redirect URIs configured
- [ ] Test authentication locally

### Infrastructure
- [ ] Domain name (optional)
- [ ] Server/hosting provider selected
- [ ] SSL certificate plan (Let's Encrypt = free)

### Configuration
- [ ] .env file created with all credentials
- [ ] Logo customized (logo.png)
- [ ] Quality presets reviewed

---

## 💎 DigitalOcean (Recommended - $5/month)

### Step 1: Create Droplet
```bash
# On DigitalOcean:
# 1. Click "Create" → "Droplets"
# 2. Image: Ubuntu 22.04 LTS
# 3. Size: $5/month (1GB RAM)
# 4. Region: Closest to users
# 5. Authentication: SSH key
# 6. Click "Create Droplet"
```

### Step 2: SSH & Update
```bash
ssh root@YOUR_DROPLET_IP
apt-get update && apt-get upgrade -y
apt-get install -y curl git nano
```

### Step 3: Install Dependencies
```bash
# Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
apt-get install -y nodejs npm

# FFmpeg
apt-get install -y ffmpeg

# Verify
node --version
npm --version
ffmpeg -version
```

### Step 4: Deploy Application
```bash
mkdir -p /opt/ratatron
cd /opt/ratatron

# Upload or clone your files
git clone https://github.com/yourusername/ratatron.git .

# Install & setup
npm install
mkdir -p uploads logs temp-chunks public
cp index.html public/
cp logo.png public/

# Configure
cp .env .env.example
nano .env
# Add all Google credentials and settings
```

### Step 5: Setup PM2
```bash
npm install -g pm2
pm2 start server.js --name "ratatron"
pm2 startup
pm2 save
pm2 status
```

### Step 6: Setup Nginx & SSL
```bash
apt-get install -y nginx certbot python3-certbot-nginx

# Get SSL certificate
certbot certonly --nginx -d your-domain.com

# Configure Nginx (use nginx-production.conf template)
cp nginx-production.conf /etc/nginx/sites-available/ratatron
ln -s /etc/nginx/sites-available/ratatron /etc/nginx/sites-enabled/

nginx -t
systemctl restart nginx
```

### Step 7: Configure Firewall
```bash
ufw --force enable
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw status
```

### Step 8: Verify
```bash
curl https://your-domain.com/api/health
# Should return: {"status":"ok","driveAuthenticated":true}
```

**Done!** Your server is live at `https://your-domain.com` ✅

---

## 🔧 AWS EC2

### Step 1: Launch Instance
```bash
# 1. AWS Console → EC2
# 2. Launch Instance
# 3. Select: Ubuntu Server 22.04 LTS
# 4. Type: t3.micro (free tier)
# 5. Security Group: Allow 22, 80, 443, 3000
# 6. Create key pair (.pem file)
```

### Step 2: Connect
```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@your-instance-ip
```

### Step 3-8: Follow DigitalOcean steps above

Everything is identical after SSH connection.

---

## 🚀 Heroku (Easiest)

### Step 1-2: Setup
```bash
brew install heroku
heroku login
cd /path/to/ratatron
heroku create your-app-name
```

### Step 3: Add Buildpacks
```bash
heroku buildpacks:add heroku/nodejs
heroku buildpacks:add https://github.com/jonathanong/heroku-buildpack-ffmpeg.git
```

### Step 4: Configure
```bash
heroku config:set GOOGLE_CLIENT_ID=xxx
heroku config:set GOOGLE_CLIENT_SECRET=xxx
heroku config:set GOOGLE_REDIRECT_URI=https://your-app.herokuapp.com/api/auth/google-callback
heroku config:set GOOGLE_DRIVE_FOLDER_ID=root
heroku config:set NODE_ENV=production
```

### Step 5: Deploy
```bash
git push heroku main
heroku open
```

**Live at:** `https://your-app.herokuapp.com` ✅

**Cost Note:** Free tier limited; Hobby ($7/mo) recommended

---

## 🐳 Docker

### Local Testing
```bash
docker build -f Dockerfile -t ratatron:latest .
docker run -d -p 3000:3000 --env-file .env -v ratatron-uploads:/app/uploads ratatron:latest
docker logs -f <container-id>
```

### With Docker Compose
```bash
docker-compose -f docker-compose.yml up -d
docker-compose ps
docker-compose logs -f ratatron-backend
```

### Deploy Anywhere
```bash
# Push to Docker Hub
docker login
docker tag ratatron:latest yourusername/ratatron:latest
docker push yourusername/ratatron:latest

# Deploy on any cloud:
docker run -d -p 3000:3000 --env-file .env yourusername/ratatron:latest
```

---

## 📊 Monitoring

### Health Check
```bash
curl https://your-domain.com/api/health
```

### View Logs
```bash
# PM2
pm2 logs ratatron

# Systemd
journalctl -u ratatron -f

# Files
tail -f logs/server.log
```

### Server Status
```bash
pm2 status
pm2 restart ratatron
```

---

## 🔐 Security

✅ Enable HTTPS (Let's Encrypt = free)
✅ Setup firewall (UFW)
✅ Regular updates: `apt-get upgrade`
✅ Backup tokens: `.auth-token.json`
✅ Monitor logs for errors

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| Server won't start | `pm2 logs ratatron` - check errors |
| Google auth fails | Verify GOOGLE_REDIRECT_URI in Console |
| Videos not uploading | Check Google Drive auth, disk space |
| High memory usage | `pm2 restart ratatron` or increase heap |
| 502 Bad Gateway | Check if Node is running: `pm2 status` |

---

## 📈 Performance Tips

- Enable Gzip compression in Nginx
- Use CDN for static files (logo.png)
- Monitor disk space regularly
- Cleanup old temp files (6+ hour retention)
- Set appropriate client_max_body_size

---

**Your Ratatron server is production-ready!** 🎉

For detailed instructions, see README.md and QUICKSTART.md
