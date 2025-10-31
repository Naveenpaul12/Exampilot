# ExamPilot Deployment Guide

## 🚀 Production Deployment

### Overview
This guide covers deploying ExamPilot to production environments with best practices for security, scalability, and reliability.

---

## 📋 Pre-Deployment Checklist

### Security
- [ ] Change default JWT secret
- [ ] Update admin/student passwords
- [ ] Enable HTTPS
- [ ] Set secure environment variables
- [ ] Configure CORS properly
- [ ] Enable rate limiting
- [ ] Set up firewall rules
- [ ] Configure backup strategy

### Performance
- [ ] Enable MongoDB indexes
- [ ] Configure CDN for static assets
- [ ] Set up caching layer
- [ ] Enable compression
- [ ] Optimize images
- [ ] Minify JavaScript/CSS

### Monitoring
- [ ] Set up error tracking (Sentry)
- [ ] Configure logging (Winston)
- [ ] Enable APM (New Relic)
- [ ] Set up uptime monitoring
- [ ] Configure database backups

---

## 🌐 Deployment Options

### Option 1: Fully Managed (Recommended)

**Backend:** Railway / Heroku / Render  
**Frontend:** Vercel / Netlify  
**Database:** MongoDB Atlas  

#### Backend Deployment (Railway)

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize project
railway init

# Add MongoDB service
railway add mongodb

# Deploy
railway up
```

**Environment Variables:**
```
PORT=5000
MONGODB_URI=<railway-provided-uri>
JWT_SECRET=<strong-random-secret>
NODE_ENV=production
CLIENT_URL=https://your-domain.vercel.app
```

#### Frontend Deployment (Vercel)

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy
cd client
vercel --prod
```

**Environment Variables:**
```
REACT_APP_API_URL=https://your-backend.railway.app
```

#### Database (MongoDB Atlas)

1. Create account at https://www.mongodb.com/cloud/atlas
2. Create cluster
3. Add database user
4. Whitelist IP addresses
5. Get connection string

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/exampilot
```

---

### Option 2: VPS (DigitalOcean/Droplet)

#### Server Setup

```bash
# Connect to server
ssh root@your-server-ip

# Update system
apt update && apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
apt install -y nodejs

# Install MongoDB
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-7.0.list
apt update
apt install -y mongodb-org

# Install Nginx
apt install -y nginx

# Install PM2
npm install -g pm2
```

#### Application Deployment

```bash
# Clone repository
git clone https://github.com/yourusername/exampilot.git
cd exampilot

# Install dependencies
npm install
cd client && npm install && npm run build && cd ..

# Create .env file
nano .env
# Add production environment variables

# Start with PM2
pm2 start server/index.js --name exampilot-api

# Setup Nginx reverse proxy
sudo nano /etc/nginx/sites-available/exampilot
```

**Nginx Configuration:**
```nginx
server {
    listen 80;
    server_name your-domain.com;

    # Frontend
    location / {
        root /path/to/exampilot/client/build;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/exampilot /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

# Setup SSL with Let's Encrypt
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

#### MongoDB Configuration

```bash
# Secure MongoDB
sudo nano /etc/mongod.conf

# Enable authentication
security:
  authorization: enabled

# Create admin user
mongosh
use admin
db.createUser({
  user: "admin",
  pwd: "strong-password",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})

# Restart MongoDB
sudo systemctl restart mongod
```

---

### Option 3: Docker Deployment

#### Create Dockerfile

**Backend Dockerfile:**
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY server ./server

EXPOSE 5000

CMD ["node", "server/index.js"]
```

**Frontend Dockerfile:**
```dockerfile
FROM node:18-alpine as build

WORKDIR /app

COPY client/package*.json ./client/
RUN npm ci --prefix client

COPY client ./client
RUN npm run build --prefix client

FROM nginx:alpine
COPY --from=build /app/client/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Docker Compose

```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:7.0
    volumes:
      - mongodb_data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: password
    ports:
      - "27017:27017"

  backend:
    build: .
    depends_on:
      - mongodb
    environment:
      PORT: 5000
      MONGODB_URI: mongodb://admin:password@mongodb:27017/exampilot?authSource=admin
      JWT_SECRET: your-secret
      NODE_ENV: production
      CLIENT_URL: http://localhost
    ports:
      - "5000:5000"

  frontend:
    build:
      context: .
      dockerfile: Dockerfile.frontend
    depends_on:
      - backend
    ports:
      - "80:80"

volumes:
  mongodb_data:
```

**Deploy:**
```bash
docker-compose up -d
```

---

## 🔒 Security Hardening

### Environment Variables
```env
# Production environment
NODE_ENV=production

# Strong JWT secret (use openssl to generate)
JWT_SECRET=$(openssl rand -base64 32)

# Secure MongoDB connection
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/exampilot?ssl=true

# Production URLs
CLIENT_URL=https://exampilot.com
API_URL=https://api.exampilot.com

# Rate limiting (stricter in production)
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX=50

# Session security
SESSION_SECRET=<another-random-secret>
COOKIE_SECURE=true
COOKIE_HTTP_ONLY=true
```

### Backend Security

**server/index.js additions:**
```javascript
// Disable X-Powered-By header
app.disable('x-powered-by');

// Trust proxy (if behind reverse proxy)
app.set('trust proxy', 1);

// Stricter CORS
app.use(cors({
  origin: process.env.CLIENT_URL,
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));

// Enhanced rate limiting
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  standardHeaders: true,
  legacyHeaders: false,
});

const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  skipSuccessfulRequests: true
});

app.use('/api/', apiLimiter);
app.use('/api/auth/login', authLimiter);
app.use('/api/auth/register', authLimiter);
```

### MongoDB Security

```javascript
// Enable SSL in production
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  ssl: process.env.NODE_ENV === 'production',
  sslValidate: true,
  authSource: 'admin',
  serverSelectionTimeoutMS: 5000,
});
```

---

## 📊 Monitoring Setup

### Error Tracking (Sentry)

```bash
npm install @sentry/node @sentry/react
```

**Backend:**
```javascript
const Sentry = require("@sentry/node");

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1,
});

app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.errorHandler());
```

**Frontend:**
```javascript
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: process.env.REACT_APP_SENTRY_DSN,
  environment: process.env.NODE_ENV,
});
```

### Logging (Winston)

```bash
npm install winston winston-daily-rotate-file
```

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
  ],
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}
```

---

## 🔄 CI/CD Pipeline

### GitHub Actions

**.github/workflows/deploy.yml:**
```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm install
      - run: npm test
      - name: Deploy to Railway
        uses: bervProject/railway-deploy@latest
        with:
          railway_token: ${{ secrets.RAILWAY_TOKEN }}
          service: backend

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: cd client && npm install && npm run build
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

---

## 📈 Performance Optimization

### Database Indexing

```javascript
// In model files
examSchema.index({ isActive: 1, createdAt: -1 });
resultSchema.index({ userId: 1, examId: 1 });
userSchema.index({ email: 1 });
```

### Caching

```bash
npm install redis
```

```javascript
const redis = require('redis');
const client = redis.createClient(process.env.REDIS_URL);

// Cache exam list
const getExams = async (req, res) => {
  const cached = await client.get('exams:all');
  if (cached) {
    return res.json(JSON.parse(cached));
  }
  
  const exams = await Exam.find({ isActive: true });
  await client.setex('exams:all', 300, JSON.stringify(exams));
  res.json(exams);
};
```

### CDN Configuration

Upload static assets to CDN:
- S3 + CloudFront
- Cloudflare
- Vercel Edge Network

---

## 🧪 Testing in Production

### Health Checks

```javascript
app.get('/health', async (req, res) => {
  try {
    await mongoose.connection.db.admin().ping();
    res.json({
      status: 'healthy',
      database: 'connected',
      timestamp: new Date()
    });
  } catch (error) {
    res.status(500).json({
      status: 'unhealthy',
      database: 'disconnected',
      error: error.message
    });
  }
});
```

### Load Testing

```bash
npm install -g artillery

# Create test file
cat > load-test.yml << EOF
config:
  target: 'https://api.exampilot.com'
  phases:
    - duration: 60
      arrivalRate: 10
scenarios:
  - name: "Login flow"
    flow:
      - post:
          url: "/api/auth/login"
          json:
            email: "test@example.com"
            password: "test123"
      - get:
          url: "/api/exams"
      - get:
          url: "/api/results/me"
EOF

artillery run load-test.yml
```

---

## 📝 Post-Deployment

### Monitoring
- [ ] Check application logs
- [ ] Monitor error rates
- [ ] Track response times
- [ ] Watch database performance
- [ ] Verify backups

### Verification
- [ ] Login works
- [ ] Exams accessible
- [ ] Results display correctly
- [ ] Admin functions work
- [ ] Security features active
- [ ] SSL certificate valid
- [ ] Analytics collecting

---

## 🆘 Rollback Plan

### Quick Rollback
```bash
# PM2
pm2 restart exampilot-api

# Docker
docker-compose down
docker-compose up -d

# Railway
railway rollback

# Vercel
vercel rollback
```

### Database Rollback
```bash
# MongoDB restore
mongorestore --uri="mongodb://..." --drop backup/
```

---

**Ready for production! 🎉**

Choose the deployment option that best fits your needs and infrastructure.

