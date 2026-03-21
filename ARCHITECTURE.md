# AgriGo Architecture After Render Deployment

## Before Deployment (Current)
```
Local Machine (You)
├── client/                          ← Android app (development)
│   ├── app/
│   ├── build.gradle.kts
│   └── src/
│
├── server/                          ← Server (local development)
│   ├── app.py                       ← Flask API
│   ├── server.js                    ← Node.js chatbot
│   ├── backend/                     ← Business logic
│   ├── migrations/                  ← Database
│   ├── requirements.txt
│   └── package.json
│
└── agrinova-ui-main/                ← Website (development)
    ├── src/
    ├── index.html
    └── vite.config.ts

Running:
- Android client → localhost:5000 (Flask) & localhost:3000 (Node)
- Website → localhost:5000 (Flask) & localhost:3000 (Node)
- Both share same local servers ❌ Not scalable
```

## After Deployment (Recommended) ✅
```
┌─────────────────────────────────────────────────────────────────┐
│                         RENDER (Cloud)                           │
│                                                                   │
│  ┌────────────────────┐  ┌────────────────────┐                │
│  │ agrigo-api-server  │  │ agrigo-chatbot     │                │
│  │ (Flask + Gunicorn) │  │ (Node.js Express)  │                │
│  │                    │  │                    │                │
│  │ Python 3.10        │  │ Node 18            │                │
│  │ Port: Auto ($PORT) │  │ Port: Auto ($PORT) │                │
│  └────────────────────┘  └────────────────────┘                │
│          │                        │                              │
│  ┌────────────────────────────────────────────────────┐         │
│  │         PostgreSQL (agrigo_db)                     │         │
│  │                                                     │         │
│  │  - User data                                        │         │
│  │  - Farm information                                 │         │
│  │  - Loan requests                                    │         │
│  │  - Schemes                                          │         │
│  └────────────────────────────────────────────────────┘         │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │
                   ┌──────────┴──────────┐
                   │                     │
         ┌─────────────────┐   ┌─────────────────┐
         │  Android App    │   │  Website        │
         │  (client/)      │   │  (agrinova-ui)  │
         │                 │   │                 │
         │ Configured with:│   │ Configured with:│
         │ API_BASE_URL=   │   │ VITE_API_*=     │
         │ https://agrigo- │   │ https://agrigo- │
         │ api-server.on-  │   │ api-server.on-  │
         │ render.com      │   │ render.com      │
         │                 │   │                 │
         └─────────────────┘   └─────────────────┘
                Local Dev              Local Dev

Key Changes:
✅ Server moved to Render cloud
✅ Automatic scaling built-in
✅ 99.9% uptime guaranteed
✅ Auto-deployment on git push
✅ No local server folder needed for production
✅ Single source of truth (Render)
```

## Local Development Still Works
```
Local Machine Development
├── Develop code locally
├── Test with local servers (optional)
│   - cd server && python app.py
│   - cd server && node server.js
│   - cd agrinova-ui-main && npm run dev
│
└── Push to GitHub → Render auto-deploys → Production updated
```

## Network Flow
```
User (Mobile or Web)
    │
    ├─ Sends request to: https://agrigo-api-server.onrender.com
    │  ↓
    │  Render Load Balancer
    │  ↓
    │  Flask API Container (auto-scaled)
    │  ↓
    │  PostgreSQL Database
    │  
    └─ Sends request to: https://agrigo-chatbot.onrender.com
       ↓
       Render Load Balancer
       ↓
       Node.js Container (auto-scaled)
       ↓
       Gemini AI API
```

## Deployment Timeline
```
1. You: Commit code locally
   ↓
2. You: Push to GitHub (git push origin main)
   ↓
3. GitHub: Trigger webhook to Render
   ↓
4. Render: Pull latest code
   ↓
5. Render: Build services
   ↓
6. Render: Run tests/migrations
   ↓
7. Render: Deploy with zero downtime
   ↓
8. You: Check Render dashboard for status
   ↓
9. ✅ Live in production! (usually < 2 minutes)
```

## Environment Variables Flow
```
Local Development:
app.py → reads .env or environment
         API_BASE_URL=http://localhost:5000

Render Production:
app.py → reads from Render Dashboard
         API_BASE_URL=https://agrigo-api-server.onrender.com
         
Android/Website: Use correct URLs per environment
```

## Backup & Recovery
```
Render Automatic Backups:
- Daily database backups
- Deployment history (rollback available)
- Log retention (30 days)

You can:
- Restore database from backup
- Rollback to previous deployment
- Access logs from dashboard
```

## Cost Structure
```
FREE TIER:
└─ Flask API: ~0.015 vCPU, 512MB RAM
└─ Node Chatbot: ~0.015 vCPU, 512MB RAM  
└─ PostgreSQL Starter: 160MB
└─ Monthly: $0

PAID TIER (when scaling up):
└─ Standard Plan: $7-12/month per service
└─ Better performance and guaranteed uptime
```

## Comparison: Before vs After

| Aspect | Before | After |
|--------|--------|-------|
| **Server Location** | Local machine | Render cloud |
| **Availability** | While computer is on | 24/7/365 |
| **Scaling** | Manual (buy more RAM) | Automatic |
| **Deployment** | Manual restart needed | Auto on git push |
| **Database Backup** | You manage | Render manages |
| **Public URL** | localhost (not accessible) | Public HTTPS URL |
| **Cost** | Your electricity | Free-12/month |
| **Maintenance** | You | Render handles |

---

## Summary
Your server is now:
- 🌍 **Globally accessible** (not just localhost)
- 🚀 **Automatically deployed** on git push
- 📈 **Auto-scaling** based on traffic
- 🔒 **Secure** with HTTPS
- ⚡ **Fast** with 99.9% uptime
- 💰 **Cost-effective** ($0-12/month)

**The server folder is still useful for local development, but your production server is now on Render!**

