# ExamPilot UI/UX Design Documentation

## Design System

### Color Palette
- **Primary:** #1976d2 (Blue)
- **Secondary:** #dc004e (Pink/Red)
- **Success:** #4caf50 (Green)
- **Error:** #f44336 (Red)
- **Warning:** #ff9800 (Orange)
- **Background:** #f5f5f5 (Light Gray)
- **Text:** #212121 (Dark Gray)

### Typography
- **Font Family:** Roboto, Helvetica, Arial, sans-serif
- **Headings:** 600 weight
- **Body:** 400 weight
- **Responsive:** Scales with screen size

### Components Library
- Material-UI Components
- Consistent spacing (8px grid)
- Card-based layouts
- Modern shadows and hover effects

---

## Screen Layouts

### 1. Login Screen

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  [Gradient Background: Purple to Blue]             │
│                                                     │
│           ┌───────────────────────────┐            │
│           │      ExamPilot            │            │
│           │                           │            │
│           │   [Login] [Register]      │  ← Tabs    │
│           │                           │            │
│           │   [Error Alert]           │            │
│           │                           │            │
│           │   Email Address           │            │
│           │   [________________]      │            │
│           │                           │            │
│           │   Password                │            │
│           │   [________________]      │            │
│           │                           │            │
│           │   [   Sign In Button   ]  │            │
│           │                           │            │
│           │   Don't have account?     │            │
│           │   [Register]              │            │
│           └───────────────────────────┘            │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Clean gradient background
- Tab switching (Login/Register)
- Form validation
- Error messages
- Responsive design

---

### 2. Student Dashboard

```
┌─────────────────────────────────────────────────────┐
│ [Logo] ExamPilot  [Results] [Admin] [Avatar ▼]     │ ← Navigation Bar
├─────────────────────────────────────────────────────┤
│                                                     │
│  Available Exams                                    │
│                                                     │
│  ┌────────────────┐  ┌────────────────┐          │
│  │ Exam Card 1    │  │ Exam Card 2    │          │
│  │                │  │                │          │
│  │ Title          │  │ Title          │          │
│  │ Description    │  │ Description    │          │
│  │                │  │                │          │
│  │ [5 Q] [30min]  │  │ [10 Q] [60min] │          │
│  │                │  │                │          │
│  │ [Start Exam]   │  │ [Start Exam]   │          │
│  └────────────────┘  └────────────────┘          │
│                                                     │
│  ┌────────────────┐  ┌────────────────┐          │
│  │ Exam Card 3    │  │ Exam Card 4    │          │
│  │                │  │                │          │
│  │ Title          │  │ Title          │          │
│  │ Description    │  │ Description    │          │
│  │                │  │                │          │
│  │ [15 Q] [90min] │  │ [20 Q] [120min]│          │
│  │                │  │                │          │
│  │ [Start Exam]   │  │ [Start Exam]   │          │
│  └────────────────┘  └────────────────┘          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Grid layout of exams
- Card-based design
- Hover effects
- Quick start buttons
- User avatar menu
- Navigation links

---

### 3. Exam Interface

```
┌─────────────────────────────────────────────────────┐
│ JavaScript Fundamentals            [00:25:30] ⏱️     │ ← Timer
├─────────────────────────────────────────────────────┤
│                                                     │
│  [Progress Bar: ████████░░░░░░░░░░ 40%]            │
│  Question 3 of 5 • 2 answered                       │
│                                                     │
│  ┌───────────────────────────────────────────────┐ │
│  │                                               │ │
│  │ What is the correct way to declare a          │ │
│  │ variable in ES6?                    [2pts]    │ │
│  │                                               │ │
│  │ ○ var                                         │ │
│  │ ○ let                                         │ │
│  │ ● const (selected)                            │ │
│  │ ○ Both let and const                         │ │
│  │                                               │ │
│  └───────────────────────────────────────────────┘ │
│                                                     │
│  [Previous]                    [Next]               │
│                                                     │
│  ⚠️ Security violations detected: 1                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Question display area
- Radio button options
- Timer countdown (top right)
- Progress bar
- Navigation buttons
- Security alert
- Fullscreen mode

---

### 4. Security Warning (Overlay)

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│                                                     │
│  ┌───────────────────────────────────────────────┐ │
│  │ ⚠️ [SHAKING ANIMATION]                        │ │
│  │                                               │ │
│  │ Security violation detected!                  │ │
│  │ Please stay on this page.                     │ │
│  │                                               │ │
│  └───────────────────────────────────────────────┘ │
│                                                     │
│                                                     │
│  [Rest of exam interface behind overlay]           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Animated shake effect
- Warning icon
- Fixed position overlay
- Auto-dismiss after 3 seconds

---

### 5. Submit Confirmation Dialog

```
┌────────────────────────────────────────┐
│ ┌────────────────────────────────────┐ │
│ │ Submit Exam?                       │ │
│ │                                    │ │
│ │ Are you sure you want to submit   │ │
│ │ your exam? You won't be able to   │ │
│ │ make changes after submission.    │ │
│ │                                    │ │
│ │ ⚠️ You have 1 security violation   │ │
│ │    recorded.                       │ │
│ │                                    │ │
│ │         [Cancel]  [Submit]         │ │
│ └────────────────────────────────────┘ │
└────────────────────────────────────────┘
```

**Features:**
- Modal dialog
- Clear warning message
- Security violations listed
- Cancel and Submit buttons

---

### 6. Results Screen

```
┌─────────────────────────────────────────────────────┐
│ [Logo] ExamPilot  [Dashboard] [Avatar ▼] [Logout]   │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Exam Results                                       │
│                                                     │
│  ┌────────────────────┐  ┌────────────────────┐   │
│  │ JS Fundamentals    │  │ Python Basics      │   │
│  │                    │  │                    │   │
│  │ ✓ Passed           │  │ ✗ Failed           │   │
│  │                    │  │                    │   │
│  │ Score: 8/10        │  │ Score: 4/10        │   │
│  │ 80%                │  │ 40%                │   │
│  │                    │  │                    │   │
│  │ 25m 30s            │  │ 18m 15s            │   │
│  │ Dec 15, 2023       │  │ Dec 10, 2023       │   │
│  │                    │  │                    │   │
│  │ ⚠️ 2 violations     │  │ ✓ Clean            │   │
│  └────────────────────┘  └────────────────────┘   │
│                                                     │
│  ┌────────────────────┐  ┌────────────────────┐   │
│  │ React Essentials   │  │ Node.js Advanced   │   │
│  │                    │  │                    │   │
│  │ ✓ Passed           │  │ ✓ Passed           │   │
│  │                    │  │                    │   │
│  │ Score: 15/20       │  │ Score: 18/20       │   │
│  │ 75%                │  │ 90%                │   │
│  │                    │  │                    │   │
│  │ 45m 00s            │  │ 55m 00s            │   │
│  │ Nov 28, 2023       │  │ Nov 20, 2023       │   │
│  │                    │  │                    │   │
│  │ ✓ Clean            │  │ ✓ Clean            │   │
│  └────────────────────┘  └────────────────────┘   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Grid layout of result cards
- Pass/Fail indicators
- Score breakdown
- Duration display
- Date stamps
- Security violation alerts
- Color-coded status

---

### 7. Admin Dashboard

```
┌─────────────────────────────────────────────────────┐
│ [Admin] ExamPilot  [Student View] [Avatar ▼] [Logout]│
├─────────────────────────────────────────────────────┤
│                                                     │
│  [25 Users] [8 Exams] [120 Results] [75% Avg]      │
│                                                     │
│  Exams Management                    [+ Create Exam]│
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │ Title    │Duration│Q│Marks│Status│Actions   │  │
│  ├──────────────────────────────────────────────┤  │
│  │ JS Fund. │30min   │5│10  │Active│✏️ 🗑️      │  │
│  │ Python   │60min   │10│20 │Active│✏️ 🗑️      │  │
│  │ React    │90min   │15│30 │Active│✏️ 🗑️      │  │
│  │ Node.js  │45min   │8│15 │Inact.│✏️ 🗑️      │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
│  Recent Submissions                                 │
│  ┌──────────────────────────────────────────────┐  │
│  │ Student Name │Exam│Score│Date    │Status    │  │
│  ├──────────────────────────────────────────────┤  │
│  │ John Doe     │JS  │8/10 │Today   │✓ Pass    │  │
│  │ Jane Smith   │Py  │6/10 │Today   │✗ Fail    │  │
│  │ Bob Wilson   │JS  │9/10 │Yesterday│✓ Pass  │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Features:**
- Analytics cards at top
- Data tables
- Action buttons (Edit, Delete)
- Status indicators
- Quick stats
- Recent activity feed

---

### 8. Create Exam Dialog

```
┌────────────────────────────────────────┐
│ ┌────────────────────────────────────┐ │
│ │ Create New Exam                    │ │
│ │                                    │ │
│ │ Exam Title                         │ │
│ │ [________________________]         │ │
│ │                                    │ │
│ │ Description                        │ │
│ │ [________________________]         │ │
│ │ [________________________]         │ │
│ │                                    │ │
│ │ Duration: [60] min                │ │
│ │ Questions: [10]                   │ │
│ │                                    │ │
│ │ Total Marks: [20]                 │ │
│ │ Passing Marks: [12]               │ │
│ │                                    │ │
│ │                                    │ │
│ │      [Cancel]    [Create Exam]     │ │
│ └────────────────────────────────────┘ │
└────────────────────────────────────────┘
```

**Features:**
- Modal form
- All exam fields
- Validation
- Create button

---

### 9. Empty States

**No Exams:**
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│                                                     │
│              📚 (icon)                              │
│                                                     │
│       No exams available at the moment             │
│                                                     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**No Results:**
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│                                                     │
│              📊 (icon)                              │
│                                                     │
│         No exam results yet                        │
│                                                     │
│              [Take an Exam]                         │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## User Flow Diagrams

### Student Journey
```
Login → Dashboard → Select Exam → Start Exam
                                    ↓
                         Answer Questions (Timer Running)
                                    ↓
                              Submit Exam
                                    ↓
                            View Results
                                    ↓
                            Back to Dashboard
```

### Admin Journey
```
Login → Admin Dashboard → View Analytics
                        ↓
                   Create/Edit Exam
                        ↓
                   View Submissions
                        ↓
                   User Management
                        ↓
                    System Monitor
```

---

## Responsive Design

### Mobile (< 768px)
- Single column layout
- Stack cards vertically
- Bottom navigation
- Hamburger menu
- Touch-friendly buttons

### Tablet (768px - 1024px)
- 2-column grid
- Side navigation
- Larger touch targets

### Desktop (> 1024px)
- Full navigation bar
- 3-4 column grid
- Hover effects
- Full feature set

---

## Accessibility Features

- Keyboard navigation
- Screen reader support
- High contrast mode
- Focus indicators
- ARIA labels
- Semantic HTML
- Alt text for icons

---

## UI Components Library

### Buttons
- Primary (Blue)
- Secondary (Pink)
- Outlined
- Text
- Disabled state

### Cards
- Default shadow
- Hover elevation
- Rounded corners

### Alerts
- Success (Green)
- Error (Red)
- Warning (Orange)
- Info (Blue)

### Inputs
- Text fields
- Radio buttons
- Checkboxes
- Dropdowns

### Navigation
- App Bar
- Drawer
- Bottom Navigation
- Tabs

---

This design system provides a modern, professional interface for secure online examinations while maintaining usability and accessibility.

