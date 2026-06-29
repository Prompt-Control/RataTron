# 🐀 Ratatron Live - Video Support Platform

**Ratatron Live** is a modern, mobile-first video capture and streaming platform designed for **remote support teams** to collect high-quality video evidence from end users in real-time. Built with adaptive quality streaming, automatic compression, and direct cloud storage integration.

---

## 📋 Table of Contents
- [What is Ratatron Live?](#what-is-ratatron-live)
- [Key Features](#key-features)
- [Use Cases](#use-cases)
- [How It Works](#how-it-works)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [Configuration](#configuration)
- [API Reference](#api-reference)
- [Support](#support)

---

## 🎯 What is Ratatron Live?

Ratatron Live is a **browser-based video capture solution** that enables support teams to remotely request video evidence from users. The platform intelligently adapts to network conditions, ensuring reliable video capture regardless of connection quality.

### Perfect For:
- **Technical Support Teams** - Collect video evidence of issues for faster troubleshooting
- **QA & Testing** - Remote test case execution and verification
- **Bug Reporting** - Users capture and submit video evidence of bugs
- **Insurance & Claims** - Damage assessment and documentation
- **Remote Diagnostics** - Medical, automotive, and equipment diagnostics
- **Training & Documentation** - Screen recording and demos

---

## ✨ Key Features

### 📱 **Adaptive Quality Streaming**
- **Automatic network detection** (2G, 3G, 4G, 5G)
- **Real-time quality adjustment** based on connection speed
- **5 quality presets:**
  - 🎬 5G/WiFi: 1280×720 @ 60fps (2.25 GB/hour)
  - ⚡ 4G: 1280×720 @ 30fps (1.1 GB/hour) 
  - 📊 3G: 854×480 @ 24fps (450 MB/hour)
  - 🔋 2G: 640×360 @ 15fps (225 MB/hour)
  - 🪫 Battery Saver: 854×480 @ 24fps (360 MB/hour)

### 🎥 **Real-Time Video Capture**
- Browser-based camera access (no app installation)
- Works on mobile, tablet, and desktop
- Automatic audio & video sync
- Live preview with quality indicator

### 📤 **Smart Uploading**
- **Chunked uploads** - Resume-able if interrupted
- **Real-time progress tracking** - Watch each chunk upload
- **Prevents accidental close** - Warning on page close during upload
- Shows upload status and network diagnostics to user

### 🗂️ **Cloud Storage Integration**
- **Direct Google Drive upload** - No intermediate storage
- **Automatic compression** - H.264 video, AAC audio
- **Instant sharing** - Get Google Drive link immediately
- **Organized storage** - Auto-creates "Ratatron Videos" folder

### 🔍 **Developer Analytics**
- **Detailed logging** - Network stats, session info, errors
- **Network diagnostics** - Connection type, speed, latency
- **Session tracking** - Unique IDs for support team reference
- **Browser console logs** - Full debug info for developers

### 🎨 **User-Friendly Interface**
- Clean, intuitive design
- Status indicators (connection, quality, progress)
- Real-time chunk visualization
- Mobile-optimized responsive layout
- Support team branding options

---

## 💡 Use Cases

### **Technical Support Workflow**

```
1. Support Agent
   ↓
   "Can you record your screen showing the issue?"
   ↓
2. User (End Customer)
   ↓
   Opens Ratatron Live link
   ↓
   Grants camera permission
   ↓
   Records video (app auto-adapts to network speed)
   ↓
3. Video Uploads
   ↓
   Real-time progress shown to user
   ↓
   "Don't close until upload completes"
   ↓
4. Support Agent
   ↓
   Receives Google Drive link
   ↓
   Reviews video with full context
   ↓
   Faster problem resolution! ✅
```

### **Insurance Damage Assessment**

```
Adjuster → Request video documentation
User → Record property damage/incident
Ratatron → Auto-optimize for mobile network
Video → Streams to company Google Drive
Review → Access anywhere, analyze quality evidence
Faster Claims Processing ✅
```

### **Medical Remote Diagnostics**

```
Healthcare Provider → Request patient video
Patient → Record symptoms/area of concern
Ratatron → Ensures video quality regardless of connection
Storage → HIPAA-compliant Google Drive
Analysis → Better diagnosis from visual evidence ✅
```

---

## 🔄 How It Works

### **User Flow**

1. **Support agent sends link** to user via email/chat
2. **User opens in browser** (no app needed)
3. **Camera permission granted**
4. **Quality auto-selected** based on network speed
5. **Video recorded** in real-time
6. **Chunks uploaded** automatically (every 5 seconds)
7. **Compression happens** on server (H.264)
8. **Google Drive upload** begins immediately after chunks received
9. **Shareable link provided** when complete

### **Technical Flow**

```
Client Browser
    ↓ (Records video in chunks)
    ↓
Server (Node.js + Express)
    ↓ (Receives chunks via /api/upload-chunk)
    ↓ (Stores temporarily in temp-chunks/)
    ↓ (When all chunks received:)
    ├→ Stitch chunks into single file
    ├→ Compress with FFmpeg (H.264)
    └→ Upload to Google Drive
    ↓
Google Drive
    ↓ (Stores video)
    ↓ (Generates shareable link)
    ↓
Support Agent
    (Access video, download, share with team)
```

---

## 🛠️ Technology Stack

### **Frontend**
- HTML5 Canvas & MediaRecorder API
- Vanilla JavaScript (no dependencies)
- Responsive CSS3 design
- WebSocket for real-time updates
- Network Information API for connection detection

### **Backend**
- **Node.js** (v18+)
- **Express.js** - HTTP server
- **FFmpeg** - Video compression
- **Google APIs** - Drive storage
- **WebSocket** - Real-time communication
- **Multer** - File upload handling

### **Storage**
- **Google Drive** - Primary storage
- **Local temp-chunks** - Intermediate processing
- Optional: Redis for caching, PostgreSQL for history

### **Deployment**
- **Docker** - Containerization
- **Nginx** - Reverse proxy
- Supports: DigitalOcean, AWS, Heroku, any Linux server

---

## 🚀 Getting Started

### **Prerequisites**
- Node.js 18+
- FFmpeg installed
- Google Cloud Project with OAuth credentials
- Modern web browser (Chrome, Firefox, Safari, Edge)

### **Quick Start (Development)**

```bash
# Clone repository
git clone https://github.com/yourusername/ratatron.git
cd ratatron

# Install dependencies
npm install

# Configure environment
cp .env .env.local
nano .env  # Add Google credentials

# Start server
npm run dev

# Access at http://localhost:3000
```

### **Quick Start (Local Network Mobile Testing)**

```bash
# Start server
npm run dev

# Get your Mac/PC IP
ifconfig | grep "inet " | grep -v 127.0.0.1

# On mobile (same WiFi)
# Open: http://192.168.1.100:3000

# Or use ngrok for HTTPS
npm install -g localtunnel
lt --port 3000
# Open the https:// URL on mobile
```

---

## 📦 Deployment

### **Option 1: DigitalOcean (Recommended - $5/month)**

```bash
# 1. Create Ubuntu 22.04 droplet ($5/month)
# 2. SSH into server
ssh root@YOUR_IP

# 3. Run setup script
bash deployment-setup.sh

# 4. Configure
nano .env

# 5. Start
npm start
```

**See DEPLOYMENT-GUIDE.md for detailed instructions.**

### **Option 2: Docker**

```bash
# Build image
docker build -t ratatron:latest .

# Run with compose
docker-compose -f docker-compose.yml up -d

# Access at http://localhost:3000
```

### **Option 3: Heroku**

```bash
# Create app
heroku create your-app-name

# Add buildpacks
heroku buildpacks:add heroku/nodejs
heroku buildpacks:add https://github.com/jonathanong/heroku-buildpack-ffmpeg.git

# Deploy
git push heroku main
```

---

## ⚙️ Configuration

### **Environment Variables**

```env
# Server
PORT=3000
NODE_ENV=production

# Google OAuth (required)
GOOGLE_CLIENT_ID=xxx.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=xxx
GOOGLE_REDIRECT_URI=https://your-domain.com/api/auth/google-callback
GOOGLE_DRIVE_FOLDER_ID=root

# Quality Settings
MAX_FILE_SIZE=500000000
CHUNK_RETENTION_HOURS=6
SESSION_RETENTION_HOURS=24

# Logging
LOG_LEVEL=info
LOG_FILE=./logs/server.log
```

### **Quality Presets**

Modify `server.js` to customize:
```javascript
const QUALITY_PRESETS = {
  '5g': { bitrate: '5000k', resolution: '1280x720', fps: 60 },
  '4g': { bitrate: '2500k', resolution: '1280x720', fps: 30 },
  // ... etc
};
```

---

## 📡 API Reference

### **Endpoints**

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/upload-chunk` | Upload video chunk |
| GET | `/api/session/:id` | Get session status |
| GET | `/api/health` | Server health check |
| GET | `/api/quality-presets` | Available quality options |
| GET | `/api/auth/google-url` | Google OAuth URL |
| GET | `/api/auth/google-callback` | OAuth callback (Google redirect) |
| GET | `/api/debug/sessions` | List active sessions (dev) |
| GET | `/api/debug/files` | List temp files (dev) |

### **WebSocket Messages**

```javascript
// Client → Server
{ type: 'init', clientId: 'xxx', preset: '4g' }
{ type: 'network-stats', downlink: 5.5, rtt: 20 }
{ type: 'heartbeat' }

// Server → Client
{ type: 'quality-recommendation', preset: '4g' }
{ type: 'compression-progress', currentSize: 150 }
{ type: 'drive-upload-progress', progress: 50 }
{ type: 'session-complete', driveLink: 'https://...' }
```

---

## 🔐 Security Considerations

✅ **CORS Protection** - Restricted to authorized domains
✅ **OAuth 2.0** - Secure Google Drive authentication
✅ **HTTPS Required** - TLS encryption for production
✅ **Rate Limiting** - Prevents abuse
✅ **File Size Limits** - Configurable max upload size
✅ **Temporary Storage** - Auto-cleanup of temp files
✅ **No Local Archives** - Videos stream directly to Google Drive

---

## 📊 Monitoring & Analytics

### **What Support Teams See**
- Video upload progress
- Session ID for tracking
- Google Drive link with video
- Upload completion time

### **What Developers See (Console Logs)**
- Network connection type & speed
- FFmpeg compression logs
- Google Drive API calls
- Session processing timeline
- Error details & stack traces
- Device information
- Upload performance metrics

---

## 🤝 Support Team Integration

### **For Your Support Platform**

1. **Generate unique link** for each user request
2. **Send link** via email/chat: `https://yourserver.com?user=123`
3. **User records video** on their device
4. **You receive notification** with Google Drive link
5. **Download & review** or share with team

### **Customize for Your Team**

Update `index.html` to add:
- Company logo (already supported - see logo.png)
- Custom colors and branding
- Support ticket integration
- User identification
- Custom instructions

---

## 📈 Performance Metrics

### **Typical Workflow Times**
- **Video capture:** User-dependent (1-10 minutes)
- **Chunk upload:** 5 seconds per chunk (live)
- **Compression:** 10-20 seconds for 2 minutes video
- **Google Drive upload:** 30 seconds - 2 minutes (depends on file size & network)
- **Total time:** ~5 minutes for typical support video

### **Network Efficiency**
| Network | Quality | Data/Minute | Upload Time (1 min video) |
|---------|---------|------------|--------------------------|
| 5G | 1280×720@60 | 37.5 MB | 10 seconds |
| 4G | 1280×720@30 | 18.3 MB | 20 seconds |
| 3G | 854×480@24 | 7.5 MB | 1 minute |
| 2G | 640×360@15 | 3.75 MB | 2 minutes |

---

## 🐛 Troubleshooting

### **Common Issues**

**Camera not working on mobile**
- Use HTTPS (LocalTunnel or ngrok)
- Check browser permissions
- Try different browser

**Videos not uploading**
- Check Google Drive authentication
- Verify GOOGLE_REDIRECT_URI matches exactly
- Check network connection

**Poor video quality**
- Normal - adjusts to your network speed
- Check connection type in settings
- Try on stronger WiFi

**Server won't start**
- Ensure FFmpeg installed: `ffmpeg -version`
- Check port 3000 available: `lsof -i :3000`
- Review .env configuration

---

## 📝 License

MIT License - Free for commercial and personal use

---

## 🎯 Roadmap

- [ ] Multi-camera support
- [ ] Screen recording (desktop)
- [ ] Video annotation tools
- [ ] Custom watermarks
- [ ] AWS S3 backend support
- [ ] Analytics dashboard
- [ ] Team collaboration features
- [ ] Webhook notifications

---

## 💬 Support & Community

- **Documentation:** See DEPLOYMENT-GUIDE.md and QUICKSTART.md
- **Issues:** GitHub Issues
- **Email:** support@example.com
- **Community:** [Discord/Forum]

---

**Made with ❤️ for support teams worldwide**

*Ratatron Live - Making remote support effortless* 🐀
