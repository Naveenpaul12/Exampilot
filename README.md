# ExamPilot - AI-Powered Online Examination Platform

## 🎯 Overview
ExamPilot is a comprehensive online examination platform with advanced security features including background detection, anti-copy protection, and session monitoring.

## ✨ Features

### Student Features
- 🔐 Secure Login System
- 📝 Online Mock Tests
- 📊 Detailed Results & Analytics
- 📱 Responsive Design

### Admin Features
- 📤 Material Upload Interface
- ✏️ Exam Creation & Management
- 📈 Dashboard Analytics
- 👥 User Management

### Security Features
- 🎥 Background Detection
- 🚫 Anti-Copy Protection
- 🔒 Session Lock Mechanisms
- ⏱️ Time-based Monitoring

## 🚀 Quick Start

### Installation
```bash
# Install all dependencies
npm run install-all

# Run development server
npm run dev
```

### Environment Setup
Create a `.env` file in the root directory:
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/exampilot
JWT_SECRET=your_jwt_secret_key_here
NODE_ENV=development
```

## 📁 Project Structure

```
exampilot/
├── client/              # React frontend
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── App.js
│   └── package.json
├── server/              # Node.js backend
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   └── index.js
├── package.json
└── README.md
```

## 🛠️ Tech Stack

### Frontend
- React.js
- React Router
- Axios
- Material-UI / TailwindCSS

### Backend
- Node.js
- Express.js
- MongoDB with Mongoose
- JWT Authentication
- bcryptjs for password hashing

### Security
- Helmet.js
- Rate Limiting
- Session Management
- Background Activity Detection

## 📝 API Endpoints

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/register` - User registration
- `POST /api/auth/logout` - User logout

### Exams
- `GET /api/exams` - Get all exams
- `GET /api/exams/:id` - Get exam details
- `POST /api/exams/:id/submit` - Submit exam

### Admin
- `POST /api/admin/exams` - Create exam
- `GET /api/admin/users` - Get all users
- `POST /api/admin/upload` - Upload materials

## 🔒 Security Features

### Background Detection
- Monitors tab visibility
- Detects app switching
- Records suspicious activity

### Anti-Copy Protection
- Disables right-click
- Disables keyboard shortcuts
- Screen recording prevention

### Session Management
- Time-based session locking
- Activity monitoring
- Auto-submit on inactivity

## 📊 Dashboard Features

- User analytics
- Exam performance metrics
- Content management
- System monitoring

## 🤝 Contributing
1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📄 License
MIT License

## 🙏 Acknowledgments
Built with modern web technologies for secure online examinations.

