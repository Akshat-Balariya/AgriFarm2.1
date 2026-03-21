# Render Deployment Quick Reference

## 📋 Checklist Before Deploying

- [ ] All code committed to GitHub
- [ ] `Procfile` exists in `/server`
- [ ] `render.yaml` exists in `/server`
- [ ] `requirements.txt` has `gunicorn`
- [ ] `app.py` uses environment PORT variable
- [ ] `.env.example` files created

## 🚀 Quick Deploy (5 minutes)

1. **Push to GitHub**
   ```bash
   git push origin main
   ```

2. **Go to Render Dashboard**
   - https://dashboard.render.com
   - Click "New" → "Blueprint"

3. **Select Your Repo**
   - Choose AgriGo repository
   - Select main branch
   - Click "Create New Services"

4. **Add Environment Variables** (in Render Dashboard)
   ```
   GEMINI_API_KEY=your-api-key
   SECRET_KEY=random-string-1234567890
   JWT_SECRET_KEY=random-string-0987654321
   DATABASE_URL=auto-populated
   ```

5. **Initialize Database** (via Shell)
   ```bash
   FLASK_APP=app.py flask db upgrade
   ```

6. **Done!** ✅
   - Flask API: `https://agrigo-api-server.onrender.com`
   - Chatbot: `https://agrigo-chatbot.onrender.com`

## 🔄 Update Workflow

1. **Make changes locally**
2. **Commit & push**
   ```bash
   git add .
   git commit -m "Your message"
   git push origin main
   ```
3. **Render auto-deploys** ✅

## 🔧 Update Client URLs

**Android (`client/gradle.properties`):**
```properties
API_BASE_URL=https://agrigo-api-server.onrender.com
CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com
```

**Website (`agrinova-ui-main/.env`):**
```
VITE_API_BASE_URL=https://agrigo-api-server.onrender.com
VITE_CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com
```

## 📊 Important Render URLs

- **Dashboard**: https://dashboard.render.com
- **Your Services**: Will appear after deployment
- **Logs**: Service Dashboard → "Logs" tab
- **Monitoring**: Built-in metrics and alerts

## ⚠️ Common Issues & Fixes

| Issue | Solution |
|-------|----------|
| "Port not found" | Check that app uses `int(os.getenv('PORT', 5000))` |
| "Module not found" | Add dependency to `requirements.txt` |
| "Database error" | Run migrations via Shell tab |
| "API returns 500" | Check environment variables in Render |
| "CORS errors" | Update `CORS_ORIGINS` in environment |

## 💰 Cost

- **Starter Plan**: Free (with limitations)
- **Standard Plan**: ~$7-12/month per service
- **Database**: Free starter PostgreSQL, upgrade available

## 🎯 No More Server Folder!

Once on Render:
- Local `/server` folder is optional (for development only)
- Production runs entirely on Render
- Just push code → auto-deploy → live in seconds

---

**Full Guide**: See `RENDER_DEPLOYMENT.md`
**Summary**: See `DEPLOYMENT_SUMMARY.md`

