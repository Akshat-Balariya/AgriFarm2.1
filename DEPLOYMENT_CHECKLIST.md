# Render Deployment Checklist

Complete this checklist to successfully deploy your AgriGo server to Render.

## ✅ Pre-Deployment (Local)

- [ ] All changes committed to Git
  ```bash
  git status  # Should show clean working directory
  ```
- [ ] Code pushed to GitHub
  ```bash
  git push origin main
  ```
- [ ] Verified these files exist in `/server`:
  - [ ] `Procfile`
  - [ ] `render.yaml`
  - [ ] `.env.example`
  - [ ] `requirements.txt` (with gunicorn)
- [ ] Verified `app.py` reads PORT from environment:
  ```python
  port = int(os.getenv('PORT', 5000))
  ```
- [ ] Verified `server.js` reads PORT from environment:
  ```javascript
  const PORT = process.env.PORT || 3000;
  ```

## ✅ Render Account Setup

- [ ] Render account created at https://render.com
- [ ] GitHub connected to Render account
- [ ] GitHub repository authorized for Render access
- [ ] Email verified

## ✅ Blueprint Deployment

- [ ] Go to https://dashboard.render.com
- [ ] Click "New" → "Blueprint"
- [ ] Select your AgriGo repository
- [ ] Select correct branch (main)
- [ ] Review services (Flask, Node.js, PostgreSQL)
- [ ] Click "Create New Services"
- [ ] Wait for deployment to complete (~5-10 minutes)

## ✅ Environment Variables Configuration

### Flask API Service (agrigo-api-server)

Essential:
- [ ] `GEMINI_API_KEY` = [your Google Generative AI key]
- [ ] `SECRET_KEY` = [generate random string: `openssl rand -hex 32`]
- [ ] `JWT_SECRET_KEY` = [generate random string: `openssl rand -hex 32`]
- [ ] `FLASK_ENV` = `production`

Database:
- [ ] `DATABASE_URL` = [auto-populated by Render PostgreSQL]
- [ ] `SQLALCHEMY_DATABASE_URI` = [same as DATABASE_URL]

Firebase (if using):
- [ ] `FIREBASE_API_KEY` = [your value]
- [ ] `FIREBASE_AUTH_DOMAIN` = [your value]
- [ ] `FIREBASE_PROJECT_ID` = [your value]
- [ ] `FIREBASE_PRIVATE_KEY` = [your value]
- [ ] `FIREBASE_CLIENT_EMAIL` = [your value]

Email:
- [ ] `MAIL_SERVER` = `smtp.gmail.com`
- [ ] `MAIL_PORT` = `587`
- [ ] `MAIL_USE_TLS` = `true`
- [ ] `MAIL_USERNAME` = [your email]
- [ ] `MAIL_PASSWORD` = [your app password]

### Node.js Chatbot Service (agrigo-chatbot)

Essential:
- [ ] `GEMINI_API_KEY` = [same as above]
- [ ] `NODE_ENV` = `production`

### PostgreSQL Database

- [ ] Database created and URL available
- [ ] Connection credentials verified
- [ ] Automatic backups enabled

## ✅ Database Initialization

After services are running:

- [ ] Go to `agrigo-api-server` dashboard
- [ ] Click "Shell" tab
- [ ] Run initialization commands:
  ```bash
  FLASK_APP=app.py flask db init
  FLASK_APP=app.py flask db migrate
  FLASK_APP=app.py flask db upgrade
  ```
- [ ] No errors in output

## ✅ Verify Deployment

### Check Service Status
- [ ] Go to Render Dashboard
- [ ] `agrigo-api-server` shows "Live" status
- [ ] `agrigo-chatbot` shows "Live" status
- [ ] PostgreSQL shows "Available" status

### Test API Endpoints
- [ ] Visit `https://agrigo-api-server.onrender.com` (should load)
- [ ] Visit `https://agrigo-chatbot.onrender.com` (should load)
- [ ] Check Render logs for no 500 errors
- [ ] Test an API endpoint with curl:
  ```bash
  curl https://agrigo-api-server.onrender.com/api/firebase-config
  ```

## ✅ Get Service URLs

- [ ] Copy Flask API URL: `https://agrigo-api-server.onrender.com`
- [ ] Copy Chatbot URL: `https://agrigo-chatbot.onrender.com`
- [ ] Copy Database URL: (from Render dashboard)
- [ ] Save URLs somewhere secure

## ✅ Update Client Configuration

### Android App
- [ ] Open `client/gradle.properties`
- [ ] Update `API_BASE_URL` to Render URL
- [ ] Update `CHAT_API_BASE_URL` to Render URL
- [ ] Commit and push changes:
  ```bash
  git add client/gradle.properties
  git commit -m "Update API URLs to Render deployment"
  git push origin main
  ```
- [ ] Rebuild Android app:
  ```bash
  cd client && ./gradlew clean assembleDebug
  ```

### Website (agrinova-ui-main)
- [ ] Open `agrinova-ui-main/.env`
- [ ] Update `VITE_API_BASE_URL` to Render URL
- [ ] Update `VITE_CHAT_API_BASE_URL` to Render URL
- [ ] Test locally: `npm run dev`
- [ ] Deploy website to hosting (Vercel, Netlify, etc.)

## ✅ Testing

### Basic Connectivity
- [ ] Android app connects to API successfully
- [ ] Website connects to API successfully
- [ ] No CORS errors in browser console
- [ ] API responds with correct data

### Database Operations
- [ ] Can read from database
- [ ] Can write to database (test with new user)
- [ ] Queries execute properly
- [ ] No connection timeout errors

### Real-Time Features (if applicable)
- [ ] WebSocket connections working
- [ ] Real-time updates received
- [ ] No connection drop-offs

## ✅ Monitoring Setup

- [ ] Enable Render notifications for your services
- [ ] Set up email alerts for deployment failures
- [ ] Set up alerts for high error rates
- [ ] Check logs regularly for issues
- [ ] Monitor resource usage

## ✅ Documentation

- [ ] Update `README.md` with Render deployment info
- [ ] Share deployment URLs with team
- [ ] Document how to add new environment variables
- [ ] Document how to run database migrations
- [ ] Create runbook for common issues

## ✅ Cleanup (Optional)

Once everything works on Render:

- [ ] Verify you don't need local server for development
- [ ] Remove `server/` from `.gitignore` (keep it synced)
- [ ] Or move to separate branch if keeping local dev
- [ ] Delete local database (Render is source of truth)
- [ ] Update project structure documentation

## ✅ Post-Deployment

### Security
- [ ] All environment variables use Render secrets (not in code)
- [ ] Database credentials not exposed
- [ ] API keys stored securely
- [ ] HTTPS enabled (automatic with Render)
- [ ] CORS origins configured properly

### Backups
- [ ] Database backups enabled in Render
- [ ] Backup schedule verified
- [ ] Test restore procedure
- [ ] Store critical data backups elsewhere

### Performance
- [ ] Monitor response times in logs
- [ ] Check error rates
- [ ] Monitor database query performance
- [ ] Consider upgrading plan if needed

### Updates
- [ ] Keep dependencies updated
- [ ] Monitor security advisories
- [ ] Plan regular testing
- [ ] Document change log

## ✅ Troubleshooting

If something goes wrong:

- [ ] Check Render dashboard logs first
- [ ] Verify all environment variables are set
- [ ] Check database connection string format
- [ ] Verify API keys are correct
- [ ] Test with curl/Postman before blaming app
- [ ] Check CORS configuration
- [ ] Review recent code changes
- [ ] Check Render service status page

## 🎉 Success Indicators

Your deployment is successful when:

- ✅ Both services show "Live" in Render dashboard
- ✅ Database is "Available"
- ✅ No errors in service logs
- ✅ API endpoints respond correctly
- ✅ Android app connects successfully
- ✅ Website loads and communicates with API
- ✅ Database operations work (create/read/update/delete)
- ✅ Real-time features work (if applicable)

## 📝 Notes

Use this space for your own notes:

```
Render URLs:
- API: ___________________________________
- Chat: __________________________________
- Database: ______________________________

Time deployed: ____________________________

Issues encountered: ________________________
_________________________________________

Resolution: _______________________________
_________________________________________
```

---

**Need Help?**
- Render Support: https://render.com/support
- Check logs in Render dashboard
- Review RENDER_DEPLOYMENT.md for detailed guide

