# Render Deployment Guide for AgriGo Server

This guide explains how to deploy the AgriGo server to Render, eliminating the need to maintain a separate local server folder.

## Prerequisites

1. **Render Account**: Sign up at [render.com](https://render.com)
2. **Git Repository**: Your code should be in a Git repository
3. **Environment Variables**: Prepare the required secrets

## Required Environment Variables

Add these to your Render dashboard for each service:

### Flask API Service (agrigo-api-server)
- `GEMINI_API_KEY`: Your Google Generative AI API key
- `FLASK_ENV`: Set to `production`
- `SECRET_KEY`: Generate a secure random string
- `JWT_SECRET_KEY`: Generate a secure random string for JWT tokens
- `DATABASE_URL`: PostgreSQL connection string (auto-provided by Render)
- `REDIS_URL`: Redis connection string (if using Celery)
- `FIREBASE_API_KEY`: Firebase configuration
- `MAIL_USERNAME`: Email service username
- `MAIL_PASSWORD`: Email service password

### Node.js Chatbot Service (agrigo-chatbot)
- `GEMINI_API_KEY`: Your Google Generative AI API key
- `NODE_ENV`: Set to `production`

## Deployment Methods

### Option 1: Using render.yaml (Blueprint Deployment) - RECOMMENDED

1. Ensure `render.yaml` exists in your repository root
2. Push changes to GitHub/GitLab
3. Go to [Render Dashboard](https://dashboard.render.com)
4. Click "New" → "Blueprint"
5. Connect your repository
6. Select the branch containing the updated code
7. Configure environment variables in the dashboard
8. Click "Create New Services"

### Option 2: Manual Deployment

#### Flask API Service
1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click "New" → "Web Service"
3. Select your Git repository
4. Configure:
   - **Name**: `agrigo-api-server`
   - **Runtime**: Python 3.10
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn -w 4 -b 0.0.0.0:$PORT app:app`
5. Add environment variables (see above)
6. Click "Create Web Service"

#### Node.js Chatbot Service
1. Click "New" → "Web Service"
2. Select your Git repository
3. Configure:
   - **Name**: `agrigo-chatbot`
   - **Runtime**: Node 18
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
4. Add environment variables (see above)
5. Click "Create Web Service"

#### PostgreSQL Database
1. Click "New" → "PostgreSQL"
2. Configure database settings
3. Render will automatically provide `DATABASE_URL` environment variable

## Database Setup

After deployment, initialize your database:

```bash
# SSH into your Render service or use the dashboard's shell
FLASK_APP=app.py flask db init
FLASK_APP=app.py flask db migrate
FLASK_APP=app.py flask db upgrade
```

Or run migrations as a one-time job in Render.

## Updating Your Local Code

Once deployed to Render, you **no longer need the `/server` folder** for production. Your workflow becomes:

1. **Development**: Test locally
2. **Push to Git**: Commit and push changes
3. **Render Auto-Deploy**: Your services automatically redeploy when you push to the selected branch
4. **No Manual Server Management**: Render handles scaling and uptime

## Environment-Specific URLs

Update your client apps to use:

```env
# Production (Render)
API_BASE_URL=https://agrigo-api-server.onrender.com
CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com

# Development (Local)
API_BASE_URL=http://localhost:5000
CHAT_API_BASE_URL=http://localhost:3000
```

## Monitoring & Logs

1. Go to your service's dashboard
2. Click on "Logs" to view real-time logs
3. Set up alerts for errors
4. Monitor resource usage and uptime

## Troubleshooting

### Service Won't Start
- Check logs in Render dashboard
- Verify all environment variables are set
- Ensure `Procfile` or start command is correct

### Database Connection Issues
- Verify `DATABASE_URL` is set
- Run migrations
- Check database credentials

### High Memory Usage
- Reduce number of Gunicorn workers: `gunicorn -w 2`
- Check for memory leaks in code
- Consider upgrading Render plan

## Cost Considerations

- **Starter Plan**: Free tier with limitations (good for development)
- **Standard Plan**: ~$7-12/month per service (recommended for production)
- **Database**: Starter PostgreSQL is free; upgrades available

## Cleanup

If you decide not to use the local server folder anymore:

1. Update documentation to point to Render endpoints
2. Keep a `.env.example` file in root for reference
3. Remove old server configuration from client apps

## Next Steps

1. ✅ Configure Render services
2. ✅ Set up database backups in Render
3. ✅ Configure custom domain (if applicable)
4. ✅ Set up monitoring alerts
5. ✅ Document API endpoints for client developers

---

For more information, visit [Render Documentation](https://render.com/docs)

