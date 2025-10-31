# ExamPilot Setup Guide

## Prerequisites

- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn

## Installation Steps

### 1. Install MongoDB

**Windows:**
```bash
# Download and install from https://www.mongodb.com/try/download/community
# Or use chocolatey:
choco install mongodb
```

**macOS:**
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

**Linux:**
```bash
sudo apt update
sudo apt install mongodb
sudo systemctl start mongod
```

### 2. Clone and Install Dependencies

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

### 3. Configure Environment

Create a `.env` file in the root directory:

```bash
cp .env.example .env
```

Edit `.env` and update the following:
- `JWT_SECRET`: Generate a strong random string
- `MONGODB_URI`: Your MongoDB connection string (if different from default)

### 4. Start the Application

**Development Mode:**
```bash
# This runs both backend and frontend concurrently
npm run dev

# Or run separately:
# Terminal 1 - Backend
npm run server

# Terminal 2 - Frontend
cd client && npm start
```

**Production Mode:**
```bash
# Build frontend
npm run build

# Start server
npm run server
```

### 5. Access the Application

- **Frontend:** http://localhost:3000
- **Backend API:** http://localhost:5000
- **MongoDB:** mongodb://localhost:27017

## Default Credentials

### Admin Account
- **Email:** admin@exampilot.com
- **Password:** admin123

### Student Account
- **Email:** student@exampilot.com
- **Password:** student123

**⚠️ IMPORTANT:** Change these passwords in production!

## Testing the Application

1. **Login as Student:**
   - Go to http://localhost:3000/login
   - Use student credentials
   - Navigate to "Available Exams"
   - Start the "JavaScript Fundamentals" exam
   - Experience security features:
     - Try right-clicking (disabled)
     - Switch tabs (warning appears)
     - Use Ctrl+C (prevented)

2. **Login as Admin:**
   - Logout from student account
   - Login with admin credentials
   - Navigate to Admin Dashboard
   - Create a new exam
   - View analytics

3. **View Results:**
   - Complete an exam
   - Navigate to "My Results"
   - See detailed score breakdown

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user
- `POST /api/auth/logout` - Logout user

### Exams
- `GET /api/exams` - Get all active exams
- `GET /api/exams/:id` - Get exam details
- `POST /api/exams/:id/submit` - Submit exam

### Results
- `GET /api/results/me` - Get user results
- `GET /api/results/:id` - Get specific result

### Admin
- `GET /api/admin/users` - Get all users
- `GET /api/admin/exams` - Get all exams (admin view)
- `POST /api/admin/exams` - Create exam
- `PUT /api/admin/exams/:id` - Update exam
- `DELETE /api/admin/exams/:id` - Delete exam
- `GET /api/admin/analytics` - Get analytics

## Security Features Implemented

### Background Detection
- Tab visibility monitoring
- Window focus detection
- Auto-logging of violations

### Anti-Copy Protection
- Right-click disabled
- Keyboard shortcuts disabled (Ctrl+C, Ctrl+V, etc.)
- Print screen prevention (demo)

### Session Management
- JWT-based authentication
- Token expiration
- Automatic session timeout
- Activity monitoring

### Additional Security
- bcrypt password hashing
- Rate limiting
- Helmet.js security headers
- Input validation
- CORS protection

## Troubleshooting

### MongoDB Connection Issues
```bash
# Check if MongoDB is running
mongosh

# If not running, start it
# Windows:
net start MongoDB

# macOS/Linux:
sudo systemctl start mongod
# or
brew services start mongodb-community
```

### Port Already in Use
```bash
# Kill process on port 5000
# Windows:
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# macOS/Linux:
lsof -ti:5000 | xargs kill -9
```

### Dependencies Issues
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Same for client
cd client
rm -rf node_modules package-lock.json
npm install
```

## Development Tips

### Hot Reload
Both backend and frontend support hot reload in development mode.

### Database Reset
To reset the database:
```bash
# Connect to MongoDB
mongosh

# Use exampilot database
use exampilot

# Drop all collections
db.users.drop()
db.exams.drop()
db.results.drop()

# Restart the server to reseed
```

### Viewing Database
Use MongoDB Compass or any MongoDB GUI to view database contents.

## Next Steps

1. **Add More Security:**
   - Implement webcam monitoring (Proctorio-style)
   - Add machine learning for behavior detection
   - Implement browser lock-in features

2. **Enhanced Features:**
   - Question banks and randomization
   - Image/PDF upload for questions
   - Advanced analytics and reporting
   - Email notifications

3. **Production Deployment:**
   - Deploy backend to Heroku/Railway/DigitalOcean
   - Deploy frontend to Vercel/Netlify
   - Set up MongoDB Atlas
   - Configure SSL certificates
   - Set up monitoring and logging

## Support

For issues or questions, please refer to the main README.md or open an issue on GitHub.

