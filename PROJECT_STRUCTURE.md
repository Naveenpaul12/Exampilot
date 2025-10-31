# ExamPilot Project Structure

## Complete File Structure

```
exampilot/
│
├── 📄 package.json                 # Root package.json with scripts
├── 📄 .gitignore                   # Git ignore rules
├── 📄 .env.example                 # Environment variables template
├── 📄 README.md                    # Main project documentation
├── 📄 SETUP.md                     # Installation & setup guide
├── 📄 FEATURES.md                  # Features documentation
├── 📄 ARCHITECTURE.md              # System architecture
├── 📄 PROJECT_STRUCTURE.md         # This file
│
├── 📁 server/                      # Backend application
│   ├── 📄 index.js                 # Server entry point
│   │
│   ├── 📁 models/                  # Database models
│   │   ├── 📄 User.js              # User schema & methods
│   │   ├── 📄 Exam.js              # Exam schema
│   │   └── 📄 Result.js            # Result schema
│   │
│   ├── 📁 routes/                  # API routes
│   │   ├── 📄 auth.js              # Authentication endpoints
│   │   ├── 📄 exams.js             # Exam endpoints
│   │   ├── 📄 admin.js             # Admin endpoints
│   │   └── 📄 results.js           # Result endpoints
│   │
│   ├── 📁 middleware/              # Middleware functions
│   │   └── 📄 auth.js              # JWT verification & admin check
│   │
│   └── 📁 utils/                   # Utility functions
│       └── 📄 seedData.js          # Database seeding script
│
├── 📁 client/                      # Frontend React application
│   ├── 📄 package.json             # Frontend dependencies
│   │
│   ├── 📁 public/                  # Public static files
│   │   ├── 📄 index.html           # HTML template
│   │   └── 📄 manifest.json        # PWA manifest
│   │
│   └── 📁 src/                     # Source code
│       ├── 📄 index.js             # React entry point
│       ├── 📄 index.css            # Global styles
│       ├── 📄 App.js               # Main app component
│       │
│       ├── 📁 pages/               # Page components
│       │   ├── 📄 Login.js         # Login/Register page
│       │   ├── 📄 Dashboard.js     # Student dashboard
│       │   ├── 📄 Exam.js          # Exam interface
│       │   ├── 📄 Results.js       # Results page
│       │   └── 📄 AdminDashboard.js # Admin panel
│       │
│       ├── 📁 components/          # Reusable components
│       │   └── 📄 PrivateRoute.js  # Protected route wrapper
│       │
│       ├── 📁 context/             # React Context providers
│       │   └── 📄 AuthContext.js   # Authentication context
│       │
│       └── 📁 services/            # API services
│           └── 📄 api.js           # Axios configuration
│
├── 📁 node_modules/                # Dependencies (generated)
├── 📁 client/node_modules/         # Frontend dependencies (generated)
│
└── 📁 .env                         # Environment variables (create from .env.example)
```

---

## File Descriptions

### Root Level Files

| File | Description |
|------|-------------|
| `package.json` | Main package.json with scripts for running both backend and frontend |
| `.gitignore` | Files and directories to ignore in version control |
| `.env.example` | Template for environment variables |
| `README.md` | Main project documentation with overview |
| `SETUP.md` | Detailed installation and setup instructions |
| `FEATURES.md` | Comprehensive feature documentation |
| `ARCHITECTURE.md` | System architecture and design documentation |
| `PROJECT_STRUCTURE.md` | This file - project structure overview |

---

### Server Files

#### Main Application
- **`server/index.js`**
  - Express application setup
  - Middleware configuration
  - MongoDB connection
  - Route mounting
  - Server initialization

#### Models (`server/models/`)
- **`User.js`**
  - User schema with email, password, role
  - Password hashing middleware
  - Password comparison method
  - Exam history tracking

- **`Exam.js`**
  - Exam schema with questions
  - Question schema (nested)
  - Duration, marks, passing criteria
  - Active/inactive status

- **`Result.js`**
  - Result schema with answers
  - Score calculation
  - Security violations tracking
  - Duration and timestamps

#### Routes (`server/routes/`)
- **`auth.js`**
  - POST `/register` - User registration
  - POST `/login` - User authentication
  - GET `/me` - Get current user
  - POST `/logout` - User logout

- **`exams.js`**
  - GET `/` - List all exams
  - GET `/:id` - Get exam details
  - POST `/:id/submit` - Submit exam

- **`admin.js`**
  - GET `/users` - List all users
  - GET `/exams` - List all exams
  - POST `/exams` - Create exam
  - PUT `/exams/:id` - Update exam
  - DELETE `/exams/:id` - Delete exam
  - GET `/analytics` - Get analytics

- **`results.js`**
  - GET `/me` - Get user results
  - GET `/:id` - Get specific result

#### Middleware (`server/middleware/`)
- **`auth.js`**
  - JWT token verification
  - User authentication middleware
  - Admin authorization middleware

#### Utils (`server/utils/`)
- **`seedData.js`**
  - Creates default admin and student users
  - Creates sample exam
  - Runs on server startup (dev mode)

---

### Client Files

#### Main Application
- **`client/src/index.js`**
  - React DOM rendering
  - App component mounting

- **`client/src/index.css`**
  - Global styles
  - Animations
  - Custom scrollbar

- **`client/src/App.js`**
  - Routing configuration
  - Theme provider
  - Auth provider wrapper

#### Pages (`client/src/pages/`)
- **`Login.js`**
  - Login/Register form
  - Tabs for switching modes
  - Form validation
  - Error handling

- **`Dashboard.js`**
  - Exam listing
  - Navigation bar
  - User menu
  - Exam cards

- **`Exam.js`**
  - Exam interface
  - Question display
  - Timer countdown
  - Security features
  - Navigation controls
  - Submit dialog

- **`Results.js`**
  - Results listing
  - Score cards
  - Pass/fail indicators
  - Security warnings

- **`AdminDashboard.js`**
  - Analytics dashboard
  - Exam management table
  - User management
  - Create exam dialog

#### Components (`client/src/components/`)
- **`PrivateRoute.js`**
  - Protected route wrapper
  - Authentication check
  - Loading state
  - Redirect to login

#### Context (`client/src/context/`)
- **`AuthContext.js`**
  - User state management
  - Login/register methods
  - Logout method
  - Token management

#### Services (`client/src/services/`)
- **`api.js`**
  - Axios instance configuration
  - Base URL setup
  - Request interceptor (JWT)
  - Response interceptor (errors)

#### Public (`client/public/`)
- **`index.html`**
  - HTML template
  - Meta tags
  - Root div

- **`manifest.json`**
  - PWA configuration
  - App metadata

---

## Key Technologies Used

### Backend
- **Express.js** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - ODM (Object Document Mapper)
- **JWT** - Token-based authentication
- **Bcrypt** - Password hashing
- **Helmet** - Security headers
- **CORS** - Cross-origin resource sharing
- **Rate Limit** - API rate limiting
- **dotenv** - Environment variables

### Frontend
- **React** - UI library
- **React Router** - Client-side routing
- **Material-UI** - Component library
- **Axios** - HTTP client
- **Context API** - State management
- **React Scripts** - Build tooling

---

## Environment Variables

Create `.env` file from `.env.example`:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/exampilot
JWT_SECRET=your_super_secret_jwt_key_here
NODE_ENV=development
CLIENT_URL=http://localhost:3000
```

---

## Development Workflow

### 1. Initial Setup
```bash
npm install              # Install backend dependencies
cd client && npm install # Install frontend dependencies
```

### 2. Environment Configuration
```bash
cp .env.example .env     # Create .env file
# Edit .env with your configuration
```

### 3. Start Development Servers
```bash
npm run dev              # Runs both backend and frontend

# Or separately:
npm run server           # Backend only (port 5000)
cd client && npm start   # Frontend only (port 3000)
```

### 4. Database Setup
- MongoDB should be running
- Seed data will be created automatically on first run
- Default users:
  - Admin: admin@exampilot.com / admin123
  - Student: student@exampilot.com / student123

---

## Build & Deployment

### Frontend Build
```bash
cd client
npm run build           # Creates optimized build in client/build/
```

### Production Deployment
1. Set `NODE_ENV=production` in `.env`
2. Build frontend: `cd client && npm run build`
3. Configure MongoDB connection
4. Set secure `JWT_SECRET`
5. Deploy to hosting platforms

---

## File Size Overview

Approximate file counts:
- **Backend Files:** 10 core files
- **Frontend Files:** 15 core files
- **Documentation:** 6 files
- **Configuration:** 5 files
- **Total:** ~36 files

---

## Dependencies

### Backend (server/package.json)
- express ^4.18.2
- mongoose ^7.6.3
- bcryptjs ^2.4.3
- jsonwebtoken ^9.0.2
- cors ^2.8.5
- dotenv ^16.3.1
- helmet ^7.1.0
- express-rate-limit ^6.10.0
- express-validator ^7.0.1

### Frontend (client/package.json)
- react ^18.2.0
- react-dom ^18.2.0
- react-router-dom ^6.20.0
- axios ^1.6.2
- @mui/material ^5.14.20
- @mui/icons-material ^5.14.19

---

## Code Organization Principles

1. **Separation of Concerns**
   - Frontend handles UI/UX
   - Backend handles business logic
   - Models handle data structure

2. **Modular Design**
   - Each route in separate file
   - Reusable components
   - Shared utilities

3. **Security First**
   - Authentication on all protected routes
   - Input validation
   - Secure headers
   - Password hashing

4. **Scalability**
   - Environment-based configuration
   - Clear API structure
   - Database indexing
   - Error handling

5. **Maintainability**
   - Consistent naming
   - Code comments
   - Documentation
   - Type safety considerations

---

## Next Steps

After understanding the structure:

1. Read `SETUP.md` for installation
2. Read `FEATURES.md` for features
3. Read `ARCHITECTURE.md` for design
4. Start with `README.md` for overview
5. Explore code starting from `server/index.js` and `client/src/App.js`

---

**Built with modularity, security, and scalability in mind.**

