# 🚂 Railway Deployment Guide - CampusConnect Backend

Quick guide to deploy this Spring Boot backend on Railway.

## 🚀 Quick Deploy (5 minutes)

### Step 1: Create Railway Account

1. Go to [Railway.app](https://railway.app/)
2. Sign up with GitHub (free, no credit card required)

### Step 2: Deploy Backend

#### Option A: Deploy from GitHub (Recommended)

```bash
1. In Railway dashboard, click "New Project"
2. Select "Deploy from GitHub repo"
3. Choose: Campus-connect-backend
4. Railway will auto-detect Java/Maven and start building
5. Wait 3-5 minutes for first build
```

#### Option B: Deploy with Railway CLI

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize project
railway init

# Deploy
railway up
```

### Step 3: Add PostgreSQL Database

```bash
1. In your Railway project, click "New"
2. Select "Database" → "Add PostgreSQL"
3. Wait for provisioning (1-2 minutes)
4. Railway will automatically link DATABASE_URL
```

### Step 4: Configure Environment Variables

```bash
In Railway dashboard, go to your backend service:
1. Click "Variables" tab
2. Add these variables:

Required:
- DATABASE_URL: (auto-set by Railway PostgreSQL)
- PORT: 8080

Optional:
- CORS_ORIGINS: https://your-frontend-url.vercel.app
- SPRING_PROFILES_ACTIVE: prod
```

### Step 5: Generate Domain

```bash
1. Go to "Settings" tab
2. Click "Generate Domain"
3. Your backend URL: https://xxx.up.railway.app
4. Save this URL for frontend configuration
```

## ✅ Verify Deployment

### Check Build Logs

```bash
1. Click on your service
2. Go to "Deployments" tab
3. Click latest deployment
4. View build logs
```

### Test Endpoints

```bash
# Test health (if you have actuator)
curl https://your-app.up.railway.app/actuator/health

# Test feed endpoint
curl https://your-app.up.railway.app/feed

# Test users endpoint
curl https://your-app.up.railway.app/users
```

## 📋 Configuration Files Explained

### railway.json

```json
{
  "build": {
    "builder": "NIXPACKS" // Railway's build system
  },
  "deploy": {
    "startCommand": "java -jar target/socialmedia-web-0.0.1-SNAPSHOT.jar",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### nixpacks.toml

```toml
[phases.setup]
nixPkgs = ['maven', 'jdk17']  // Install Maven and Java 17

[phases.build]
cmds = ['mvn clean package -DskipTests']  // Build JAR

[start]
cmd = 'java -Dserver.port=$PORT -jar target/socialmedia-web-0.0.1-SNAPSHOT.jar'
```

## 🔧 Environment Variables

### Required Variables

```properties
DATABASE_URL=postgresql://postgres:xxx@containers-us-west-xxx.railway.app:5432/railway
PORT=8080
```

### Optional Variables

```properties
CORS_ORIGINS=https://your-frontend.vercel.app
SPRING_PROFILES_ACTIVE=prod
JAVA_OPTS=-Xmx512m
```

## 🗄️ Database Setup

### Automatic Setup

Railway automatically:

- Creates PostgreSQL database
- Sets DATABASE_URL environment variable
- Runs Flyway migrations on startup
- Creates all tables from migration files

### Manual Database Check

```bash
1. Click on PostgreSQL service
2. Go to "Data" tab
3. View tables: users, posts, follows, chats, messages
```

### Connection String Format

```
postgresql://postgres:[password]@[host]:[port]/railway
```

## 🔍 Troubleshooting

### Build Fails

```bash
Problem: Maven build errors
Solution:
1. Check pom.xml for errors
2. Verify Java version (should be 17)
3. Check Railway build logs
4. Try: mvn clean package locally first
```

### Database Connection Failed

```bash
Problem: Can't connect to database
Solution:
1. Verify DATABASE_URL is set
2. Check PostgreSQL service is running
3. View PostgreSQL logs
4. Ensure Flyway migrations completed
```

### Application Won't Start

```bash
Problem: JAR file not starting
Solution:
1. Check if JAR was built successfully
2. Verify PORT variable is set
3. Check application logs
4. Ensure no port conflicts
```

### CORS Errors

```bash
Problem: Frontend can't access backend
Solution:
1. Add frontend URL to CORS_ORIGINS
2. Format: https://your-app.vercel.app
3. Restart backend service
4. Clear browser cache
```

## 📊 Monitoring

### View Logs

```bash
1. Click on backend service
2. Go to "Deployments" tab
3. Click "View Logs"
4. Monitor real-time logs
```

### Check Metrics

```bash
1. Go to "Metrics" tab
2. View CPU usage
3. View memory usage
4. View request count
```

### Keep Service Awake

```bash
Use UptimeRobot (free):
1. Go to uptimerobot.com
2. Add monitor for your backend URL
3. Set interval: 5 minutes
4. Prevents cold starts
```

## 💰 Free Tier Limits

### What You Get FREE:

- ✅ 500 hours/month execution time
- ✅ $5 credit/month
- ✅ 512MB RAM
- ✅ Shared CPU
- ✅ 500MB PostgreSQL storage
- ⚠️ Sleeps after 15 min inactivity

### Usage Tips:

- Monitor hours in dashboard
- Use UptimeRobot to keep awake
- Optimize database queries
- Check usage weekly

## 🔄 Continuous Deployment

### Auto-Deploy on Push

```bash
1. Push code to GitHub main branch
2. Railway detects changes
3. Automatically rebuilds
4. Deploys new version
5. Zero downtime deployment
```

### Manual Redeploy

```bash
1. Go to "Deployments" tab
2. Click "Deploy" button
3. Select "Redeploy"
```

## 🔐 Security Best Practices

### Environment Variables

- ✅ Never commit DATABASE_URL
- ✅ Use Railway's variable system
- ✅ Rotate database passwords regularly
- ✅ Use HTTPS only

### CORS Configuration

- ✅ Specify exact frontend URLs
- ❌ Don't use wildcard (\*) in production
- ✅ Include both http and https for testing

## 📝 Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] Railway account created
- [ ] Backend deployed from GitHub
- [ ] PostgreSQL database added
- [ ] DATABASE_URL automatically set
- [ ] CORS_ORIGINS configured
- [ ] Domain generated
- [ ] Backend URL tested
- [ ] Flyway migrations ran
- [ ] Tables created in database
- [ ] Endpoints responding
- [ ] Logs checked for errors

## 🎉 Success!

Your backend is now live at:

```
https://your-app.up.railway.app
```

### Next Steps:

1. Save your backend URL
2. Configure frontend with this URL
3. Test all API endpoints
4. Set up monitoring with UptimeRobot
5. Share your app!

## 🆘 Need Help?

- Railway Docs: https://docs.railway.app/
- Railway Discord: https://discord.gg/railway
- GitHub Issues: Create issue in your repo
- Stack Overflow: Tag `railway`, `spring-boot`

---

**Happy Deploying! 🚂**
