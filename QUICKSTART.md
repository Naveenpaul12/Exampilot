# ExamPilot Quick Start Guide

## ⚡ Get Running in 5 Minutes

### Prerequisites Check
- [ ] Node.js installed (v14+)
- [ ] MongoDB running locally
- [ ] Terminal/Command Prompt access

### Step 1: Install Dependencies

```bash
# Navigate to project directory
cd exampilot

# Install backend dependencies
npm install

# Install frontend dependencies
cd client
npm install
cd ..
```

### Step 2: Create Environment File

Create a `.env` file in the root directory:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/exampilot
JWT_SECRET=exampilot_secret_key_dev_2024
NODE_ENV=development
CLIENT_URL=http://localhost:3000
```

### Step 3: Start MongoDB

**Windows:**
```bash
# If installed as service
net start MongoDB

# Or if running manually
mongod
```

**macOS/Linux:**
```bash
# Using Homebrew (macOS)
brew services start mongodb-community

# Using systemd (Linux)
sudo systemctl start mongod
```

### Step 4: Run the Application

**Option A: Run Both Servers Together**
```bash
npm run dev
```

**Option B: Run Separately**
```bash
# Terminal 1 - Backend
npm run server

# Terminal 2 - Frontend
cd client && npm start
```

### Step 5: Access the Application

- **Frontend:** http://localhost:3000
- **Backend API:** http://localhost:5000
- **Health Check:** http://localhost:5000/api/health

---

## 🧪 Test the Application

### 1. Login as Student

1. Go to http://localhost:3000
2. Click "Register" or use:
   - **Email:** student@exampilot.com
   - **Password:** student123

3. You'll see the "Available Exams" dashboard

### 2. Take an Exam

1. Click "Start Exam" on "JavaScript Fundamentals"
2. Watch the timer countdown
3. Answer the questions
4. Notice security features:
   - Try right-clicking (blocked)
   - Switch tabs (warning appears)
   - Use Ctrl+C (blocked)

5. Click "Submit Exam"
6. View your results

### 3. Login as Admin

1. Logout from student account
2. Login with:
   - **Email:** admin@exampilot.com
   - **Password:** admin123

3. Navigate to Admin Dashboard
4. View analytics and exams

---

## 📁 Default Accounts

These are created automatically on first run:

**Admin Account:**
```
Email: admin@exampilot.com
Password: admin123
Role: Administrator
```

**Student Account:**
```
Email: student@exampilot.com
Password: student123
Role: Student
```

**⚠️ Change passwords immediately in production!**

---

## 🎯 Key Features to Test

### Security Features
- [ ] Right-click is disabled
- [ ] Tab switching triggers warning
- [ ] Keyboard shortcuts blocked
- [ ] Timer auto-submits exam
- [ ] Violations logged in results

### Exam Features
- [ ] Multiple choice questions
- [ ] Progress bar updates
- [ ] Navigation works (Previous/Next)
- [ ] Timer countdown
- [ ] Submit confirmation

### Admin Features
- [ ] View all exams
- [ ] Create new exam
- [ ] View analytics
- [ ] Delete exam
- [ ] User management

---

## 🐛 Troubleshooting

### MongoDB Not Running
**Error:** `MongoDB Connection Error`

**Solution:**
```bash
# Windows
net start MongoDB

# macOS
brew services start mongodb-community

# Linux
sudo systemctl start mongod
```

### Port Already in Use
**Error:** `Port 5000 is already in use`

**Solution:**
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <process_id> /F

# macOS/Linux
lsof -ti:5000 | xargs kill -9
```

### Cannot Find Module
**Error:** `Cannot find module 'react-scripts'`

**Solution:**
```bash
cd client
npm install
```

### Database Seeding Failed
**Error:** Seed data not created

**Solution:**
```bash
# Restart the server
npm run server

# Or manually seed
mongosh
use exampilot
# Server will auto-seed on startup
```

---

## 🔍 Verify Installation

Run these checks:

```bash
# Check Node.js
node --version

# Check MongoDB
mongosh --version

# Check installation
npm list --depth=0

# Check client installation
cd client
npm list --depth=0
cd ..
```

---

## 📚 Next Steps

1. **Read Documentation:**
   - `README.md` - Overview
   - `SETUP.md` - Detailed setup
   - `FEATURES.md` - All features
   - `ARCHITECTURE.md` - System design

2. **Explore the Code:**
   - Start with `server/index.js`
   - Check `client/src/App.js`
   - Review routes and models

3. **Customize:**
   - Add your own exams
   - Modify UI colors/styles
   - Add new question types
   - Implement additional security

4. **Deploy:**
   - Deploy backend to Heroku/Railway
   - Deploy frontend to Vercel/Netlify
   - Set up MongoDB Atlas
   - Configure production environment

---

## 🆘 Getting Help

**Common Issues:**
1. MongoDB not running
2. Port conflicts
3. Missing dependencies
4. Environment variables not set

**Resources:**
- Check console logs
- Review error messages
- Read documentation files
- Check GitHub issues

---

## ✅ Installation Checklist

- [ ] Node.js installed
- [ ] MongoDB installed and running
- [ ] Dependencies installed (root)
- [ ] Dependencies installed (client)
- [ ] .env file created
- [ ] Server starts without errors
- [ ] Client starts without errors
- [ ] Can login as student
- [ ] Can login as admin
- [ ] Can take an exam
- [ ] Results appear after submission
- [ ] Admin dashboard accessible

---

**You're ready to go! 🚀**

Start building your secure online examination platform.

