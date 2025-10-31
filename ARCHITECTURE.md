# ExamPilot Architecture Documentation

## System Overview

ExamPilot is a full-stack web application built with React (frontend) and Node.js (backend), designed for secure online examinations with advanced proctoring features.

```
┌─────────────────────────────────────────────────────────────┐
│                      Client (Browser)                        │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  React SPA                                              │  │
│  │  - Material-UI Components                               │  │
│  │  - Security Detection Layer                             │  │
│  │  - Real-time Validation                                 │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTPS
┌─────────────────────────────────────────────────────────────┐
│                    Backend Server                            │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Express.js API                                        │  │
│  │  - Authentication Middleware                           │  │
│  │  - Rate Limiting                                       │  │
│  │  - Security Headers                                    │  │
│  │  - Validation                                          │  │
│  └───────────────────────────────────────────────────────┘  │
│                    ↕                                          │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Business Logic Layer                                  │  │
│  │  - Auth Service                                        │  │
│  │  - Exam Service                                        │  │
│  │  - Result Service                                      │  │
│  │  - Admin Service                                       │  │
│  └───────────────────────────────────────────────────────┘  │
│                    ↕                                          │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Data Access Layer (Mongoose ODM)                     │  │
│  │  - User Model                                          │  │
│  │  - Exam Model                                          │  │
│  │  - Result Model                                        │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                   MongoDB Database                           │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Collections                                           │  │
│  │  - users                                               │  │
│  │  - exams                                               │  │
│  │  - results                                             │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Frontend Architecture

### Technology Stack
- **React 18:** Component-based UI library
- **React Router:** Client-side routing
- **Material-UI:** Component library
- **Axios:** HTTP client
- **Context API:** State management

### Component Structure
```
src/
├── App.js                    # Main application component
├── index.js                  # Entry point
├── index.css                 # Global styles
├── components/               # Reusable components
│   └── PrivateRoute.js      # Route protection
├── context/                  # Context providers
│   └── AuthContext.js       # Authentication state
├── pages/                    # Page components
│   ├── Login.js             # Login/Register page
│   ├── Dashboard.js         # Student dashboard
│   ├── Exam.js              # Exam interface
│   ├── Results.js           # Results page
│   └── AdminDashboard.js    # Admin panel
└── services/                 # API services
    └── api.js               # Axios configuration
```

### State Management Flow
```
┌──────────────┐
│  User Action │
└──────┬───────┘
       │
       ▼
┌─────────────────────────────────┐
│  Component                      │
│  (uses useAuth, useNavigate)   │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│  Context/AuthContext            │
│  - Stores user state            │
│  - Provides auth methods        │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│  API Service (api.js)           │
│  - Sends HTTP request           │
│  - Attaches JWT token           │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│  Backend API                    │
│  /api/*                         │
└─────────────────────────────────┘
```

### Security Features on Frontend
1. **Tab Visibility Detection**
   ```javascript
   document.addEventListener('visibilitychange', handleVisibilityChange);
   ```

2. **Right-Click Prevention**
   ```javascript
   document.addEventListener('contextmenu', e => e.preventDefault());
   ```

3. **Keyboard Shortcut Blocking**
   ```javascript
   document.addEventListener('keydown', handleKeyDown);
   ```

4. **Timer Enforcement**
   ```javascript
   setInterval(() => {
     setTimeRemaining(prev => prev - 1);
   }, 1000);
   ```

---

## Backend Architecture

### Technology Stack
- **Node.js:** Runtime environment
- **Express.js:** Web framework
- **MongoDB:** NoSQL database
- **Mongoose:** ODM (Object Document Mapper)
- **JWT:** Token-based authentication
- **Bcrypt:** Password hashing

### Directory Structure
```
server/
├── index.js                  # Application entry point
├── models/                   # Database models
│   ├── User.js              # User schema
│   ├── Exam.js              # Exam schema
│   └── Result.js            # Result schema
├── routes/                   # API routes
│   ├── auth.js              # Authentication routes
│   ├── exams.js             # Exam routes
│   ├── admin.js             # Admin routes
│   └── results.js           # Result routes
├── middleware/               # Middleware functions
│   └── auth.js              # JWT verification
├── utils/                    # Utility functions
│   └── seedData.js          # Database seeding
└── .env                      # Environment variables
```

### Request Flow
```
┌─────────────────────────────────────────┐
│  Client Request                         │
│  HTTP GET/POST/PUT/DELETE               │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Middleware Stack                       │
│  1. Helmet (Security headers)          │
│  2. CORS                                │
│  3. Rate Limiting                       │
│  4. Body Parser                         │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Route Handler                          │
│  - Authentication check (if protected)  │
│  - Admin check (if admin route)         │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Business Logic                         │
│  - Data validation                      │
│  - Database operations                  │
│  - Score calculation                    │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Response                               │
│  - JSON data                            │
│  - Status code                          │
│  - Error messages                       │
└─────────────────────────────────────────┘
```

---

## Database Schema

### User Collection
```javascript
{
  _id: ObjectId,
  name: String,
  email: String (unique),
  password: String (hashed),
  role: String ("student" | "admin"),
  profilePicture: String,
  createdAt: Date,
  examHistory: [{
    examId: ObjectId,
    score: Number,
    attemptedAt: Date
  }]
}
```

### Exam Collection
```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  duration: Number (minutes),
  totalQuestions: Number,
  totalMarks: Number,
  passingMarks: Number,
  questions: [{
    question: String,
    type: String ("mcq" | "true-false" | "short-answer"),
    options: [String],
    correctAnswer: String,
    points: Number,
    explanation: String
  }],
  instructions: [String],
  isActive: Boolean,
  allowReview: Boolean,
  showResult: Boolean,
  createdBy: ObjectId,
  createdAt: Date,
  updatedAt: Date
}
```

### Result Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  examId: ObjectId,
  answers: [{
    questionId: String,
    selectedAnswer: String,
    isCorrect: Boolean,
    pointsEarned: Number,
    timeSpent: Number
  }],
  totalScore: Number,
  percentage: Number,
  passed: Boolean,
  startTime: Date,
  endTime: Date,
  duration: Number (seconds),
  securityViolations: [{
    type: String,
    description: String,
    timestamp: Date
  }],
  ipAddress: String,
  userAgent: String,
  isProctored: Boolean
}
```

---

## Security Architecture

### Authentication Flow
```
1. User submits credentials
        ↓
2. Backend validates credentials
        ↓
3. Backend generates JWT token
        ↓
4. Client stores token (localStorage)
        ↓
5. Client includes token in headers
        ↓
6. Backend verifies token
        ↓
7. Request authorized
```

### Security Layers
```
┌─────────────────────────────────────────┐
│  Layer 1: Client-Side Security          │
│  - Form validation                       │
│  - XSS prevention                        │
│  - CSRF tokens (future)                  │
└─────────────────────────────────────────┘
              ↕
┌─────────────────────────────────────────┐
│  Layer 2: Network Security              │
│  - HTTPS (production)                    │
│  - CORS configuration                    │
│  - Rate limiting                         │
└─────────────────────────────────────────┘
              ↕
┌─────────────────────────────────────────┐
│  Layer 3: Application Security          │
│  - Helmet.js headers                     │
│  - Input sanitization                    │
│  - SQL injection prevention              │
└─────────────────────────────────────────┘
              ↕
┌─────────────────────────────────────────┐
│  Layer 4: Authentication                │
│  - JWT verification                      │
│  - Password hashing (bcrypt)             │
│  - Role-based access                     │
└─────────────────────────────────────────┘
              ↕
┌─────────────────────────────────────────┐
│  Layer 5: Database Security             │
│  - MongoDB security                      │
│  - Index optimization                    │
│  - Data encryption (future)              │
└─────────────────────────────────────────┘
```

---

## API Design

### RESTful Endpoints

**Authentication**
- `POST /api/auth/register` - Create account
- `POST /api/auth/login` - Authenticate
- `GET /api/auth/me` - Get current user
- `POST /api/auth/logout` - Logout

**Exams (Student)**
- `GET /api/exams` - List all exams
- `GET /api/exams/:id` - Get exam details
- `POST /api/exams/:id/submit` - Submit exam

**Results**
- `GET /api/results/me` - Get user results
- `GET /api/results/:id` - Get specific result

**Admin**
- `GET /api/admin/users` - List all users
- `GET /api/admin/exams` - List all exams
- `POST /api/admin/exams` - Create exam
- `PUT /api/admin/exams/:id` - Update exam
- `DELETE /api/admin/exams/:id` - Delete exam
- `GET /api/admin/analytics` - Get analytics

### Response Format
```json
{
  "success": true,
  "data": {...},
  "message": "Optional message"
}
```

### Error Format
```json
{
  "success": false,
  "error": "Error message",
  "details": {...}
}
```

---

## Deployment Architecture

### Development
```
┌──────────────┐
│  React Dev   │  localhost:3000
│  Server      │
└──────┬───────┘
       │
       ↓
┌──────────────┐
│  Node.js     │  localhost:5000
│  Express     │
└──────┬───────┘
       │
       ↓
┌──────────────┐
│  MongoDB     │  localhost:27017
│  Local       │
└──────────────┘
```

### Production (Recommended)
```
┌─────────────────────────────────────┐
│  CDN (Cloudflare)                   │
│  Static assets caching              │
└──────────┬──────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│  Frontend Hosting                   │
│  - Vercel / Netlify                 │
│  - Static React build               │
└──────────┬──────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│  Backend Hosting                    │
│  - Railway / Heroku / DigitalOcean  │
│  - Node.js + Express                │
└──────────┬──────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│  Database Hosting                   │
│  - MongoDB Atlas                    │
│  - Managed MongoDB                  │
└─────────────────────────────────────┘
```

---

## Scalability Considerations

### Current Architecture (Small-Medium Scale)
- Single server instance
- Direct MongoDB connection
- No caching layer
- Stateless authentication

### Recommended for Scale

**Load Balancing**
```
         ┌─────────┐
         │ Load    │
         │ Balancer│
         └────┬────┘
              │
      ┌───────┴───────┐
      │               │
   ┌──▼──┐        ┌──▼──┐
   │App 1│        │App 2│
   │Server│        │Server│
   └──┬──┘        └──┬──┘
      └───────┬───────┘
              │
         ┌────▼────┐
         │ MongoDB │
         │ Cluster │
         └─────────┘
```

**Caching Strategy**
- Redis for session management
- CDN for static assets
- MongoDB indexes for queries
- API response caching

**Database Scaling**
- MongoDB sharding
- Read replicas
- Connection pooling
- Query optimization

---

## Monitoring & Logging

### Current Implementation
- Console logging
- Error tracking in try-catch blocks
- Basic validation messages

### Recommended Additions
- Winston or Morgan for logging
- Sentry for error tracking
- PM2 for process management
- New Relic or DataDog for APM
- MongoDB monitoring tools
- API analytics

---

## Testing Strategy

### Current State
- Manual testing
- Basic error handling

### Recommended
**Unit Tests**
- Jest for frontend
- Mocha/Chai for backend

**Integration Tests**
- API endpoint testing
- Database interaction testing

**E2E Tests**
- Cypress or Playwright
- User flow testing

**Security Tests**
- Penetration testing
- Vulnerability scanning
- Security audits

---

## Performance Optimization

### Frontend
- Code splitting
- Lazy loading
- Image optimization
- Bundle size reduction

### Backend
- Database indexing
- Query optimization
- Response compression
- Connection pooling

### Database
- Proper indexing
- Aggregation pipelines
- Read/write separation
- Regular maintenance

---

This architecture provides a solid foundation for a secure, scalable online examination platform. The modular design allows for easy expansion and maintenance.

