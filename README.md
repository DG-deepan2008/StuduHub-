# StudyHub • 3rd Semester Academic Dashboard

A modern, SaaS-style dark academic dashboard built for 3rd-semester students to manage **Notes**, **Question Papers**, and **Attendance** across **6 core subjects**.

![Theme](https://img.shields.io/badge/Theme-Dark%20Academic%20SaaS-6366f1)
![Stack](https://img.shields.io/badge/Stack-React%20%2B%20TypeScript%20%2B%20Node.js%20%2B%20SQLite-blue)
![Database](https://img.shields.io/badge/Database-SQLite%20(Built--in%20Node%2024)-emerald)

---

## 🚀 Key Features

1. **Central Subject Configuration**:
   - Exactly 6 selectable subjects configured in a single file: `subjects.config.json`.
   - Edit subject names and codes either by modifying `subjects.config.json` directly or clicking the **"Configure Subjects"** button in the UI.

2. **Dashboard**:
   - Total count of uploaded Notes files.
   - Total count of Question Paper files.
   - Overall semester attendance percentage with progress bar.
   - Critical Alert banner when any subject drops below 75% attendance.
   - 6 selectable Subject Cards: Clicking any card displays that subject's notes count, question papers count, and attendance rate with direct action shortcuts.

3. **Notes Repository**:
   - Dropdown filter with the 6 subjects (plus "All Subjects").
   - Real file upload supporting: **PDF, DOC, DOCX, PPT, PPTX, TXT, PNG, JPG, JPEG, WEBP** (up to 50 MB).
   - Real-time search by title or original file name.
   - Card layout with file type icons, size, and upload date.
   - Direct **Open / In-Browser View**, **Download**, and **Delete** actions.

4. **Question Papers Vault**:
   - Dropdown filter with the 6 subjects.
   - Exam Type filter (`End-Semester`, `Mid-Term`, `Internal Assessment`, `Model Exam`, `Supplementary`).
   - Year filter (`2026`, `2025`, `2024`, `2023`, `2022`, etc.).
   - Search by title or exam type.
   - Upload modal, card view, and download/open buttons.

5. **Attendance Tracker**:
   - Shows all 6 subjects with classes attended and total classes held.
   - Automatic percentage calculation with dynamic progress bar:
     - 🟢 Green: $\ge 80\%$
     - 🟡 Yellow: $75\% - 79\%$
     - 🔴 Red: $< 75\%$ (Eligibility Deficit)
   - **Smart Target Calculator**: Informs you how many consecutive classes to attend to reach 75%, or how many you can safely miss.
   - Quick **"+ Present"** and **"+ Absent"** buttons for one-click daily tracking.
   - Toggle between **Cards View** and **Clean Table View**.
   - Edit modal to update attended/total counts or reset to 0.

6. **Student Authentication (Optional/Private Accounts)**:
   - Built-in guest student mode out-of-the-box.
   - Optional sign-up / sign-in with salted bcrypt passwords and JWT tokens for private student data.

---

## 📁 Project Structure

```text
d:\study 3sem\
├── subjects.config.json       # Central configuration file for all 6 subjects
├── package.json               # Root scripts (start, server, client, build)
├── server/                    # Node.js + Express backend
│   ├── index.js               # Main Express entry point & static SPA server
│   ├── db.js                  # SQLite database setup (built-in node:sqlite)
│   ├── config.js              # Reads and persists subjects.config.json
│   ├── .env                   # Server environment variables
│   ├── .env.example           # Example server environment variables
│   ├── data/                  # SQLite storage
│   │   └── academic_dashboard.sqlite
│   ├── middleware/
│   │   ├── auth.js            # JWT auth middleware with seamless guest fallback
│   │   └── upload.js          # Multer file upload & extension validation
│   ├── routes/
│   │   ├── auth.js            # User registration & login
│   │   ├── subjects.js        # GET subjects & PUT update configuration
│   │   ├── attendance.js      # CRUD & quick increment for attendance
│   │   ├── notes.js           # CRUD & download for notes
│   │   ├── papers.js          # CRUD & download for question papers
│   │   └── stats.js           # Dashboard KPI counters and subject breakdown
│   └── uploads/               # Persistent file storage on disk
│       ├── notes/             # Uploaded lecture notes & slides
│       └── papers/            # Uploaded exam question papers
├── client/                    # Vite + React 18 + TypeScript + Tailwind CSS
│   ├── src/
│   │   ├── api/
│   │   │   └── client.ts      # Typed fetch client with JWT token management
│   │   ├── components/
│   │   │   ├── Layout.tsx     # Responsive layout
│   │   │   ├── Sidebar.tsx    # Sidebar with 6 subject shortcuts
│   │   │   ├── Topbar.tsx     # Sticky topbar with active subject selector
│   │   │   ├── StatCard.tsx   # Modern SaaS stat cards with glowing gradients
│   │   │   ├── ProgressBar.tsx# 75% cutoff attendance progress bar
│   │   │   ├── Modal.tsx      # Reusable backdrop modal dialog
│   │   │   ├── SubjectConfigModal.tsx # Edit subjects from UI
│   │   │   └── AuthModal.tsx  # Sign In / Sign Up modal
│   │   ├── context/
│   │   │   └── AuthContext.tsx# Reactive authentication state
│   │   ├── pages/
│   │   │   ├── Dashboard.tsx  # Dashboard overview with 6 subject selector cards
│   │   │   ├── NotesPage.tsx  # Notes page with filters, search, and upload
│   │   │   ├── QuestionPapersPage.tsx # Question papers page
│   │   │   └── AttendancePage.tsx     # Attendance tracker (cards & table)
│   │   ├── types/
│   │   │   └── index.ts       # Shared TypeScript data models
│   │   ├── App.tsx            # Main App root component
│   │   ├── main.tsx           # React DOM root
│   │   └── index.css          # Tailwind CSS styles and glassmorphism classes
│   ├── dist/                  # Production frontend build
│   ├── tailwind.config.js     # Tailwind configuration
│   ├── vite.config.ts         # Vite configuration with proxy to port 5000
│   └── package.json           # Client package dependencies
└── scratch/                   # Verification and sample data seed scripts
    ├── seed-sample-data.js    # Populates realistic demo notes & attendance
    └── test-full-app.js       # End-to-end automated API verification
```

---

## ⚙️ How to Change the 6 Subjects

The 6 subjects are controlled by `subjects.config.json` in the root folder:

```json
{
  "subjects": [
    { "id": "sub_1", "code": "SUB101", "name": "Subject 1", "description": "Foundational course - Subject 1", "color": "blue" },
    { "id": "sub_2", "code": "SUB102", "name": "Subject 2", "description": "Core concepts - Subject 2", "color": "indigo" },
    { "id": "sub_3", "code": "SUB103", "name": "Subject 3", "description": "Advanced topics - Subject 3", "color": "purple" },
    { "id": "sub_4", "code": "SUB104", "name": "Subject 4", "description": "Applied theory - Subject 4", "color": "cyan" },
    { "id": "sub_5", "code": "SUB105", "name": "Subject 5", "description": "Lab & Practice - Subject 5", "color": "emerald" },
    { "id": "sub_6", "code": "SUB106", "name": "Subject 6", "description": "Elective / Specialization - Subject 6", "color": "rose" }
  ]
}
```

You can change names and codes by:
1. Simply opening `subjects.config.json` in your editor and editing the `name` or `code` values.
2. OR clicking the **"Configure Subjects"** button directly in the Topbar or Sidebar in the application.

---

## 🏃‍♂️ How to Run the Application

### Prerequisites
- Node.js version 20+ (Node.js 24 is installed and verified).
- npm installed.

### Option 1: Run Fullstack Production (Fastest - Single Port)
```bash
# From root directory:
npm start
```
Open **[http://localhost:5000](http://localhost:5000)** in your browser!

### Option 2: Run in Development Mode with Hot Reload
In terminal 1 (start backend server):
```bash
node server/index.js
```

In terminal 2 (start client with Vite HMR):
```bash
cd client
npm run dev
```
Open **[http://localhost:5173](http://localhost:5173)** in your browser! Requests to `/api` and `/uploads` will automatically proxy to the backend.

---

## 🔐 Environment Variables

The server uses the `.env` file located in `server/.env`:

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `PORT` | `5000` | Port for Express backend |
| `JWT_SECRET` | `academic_study_hub_secret_key_change_in_production_2026` | Secret key for signing JWT tokens |
| `MAX_FILE_SIZE_MB` | `50` | Maximum allowed file upload size in MB |
| `NODE_ENV` | `development` | Runtime environment mode |

---

## 🗄️ Database Setup Steps

The database uses Node.js 24's zero-dependency synchronous SQLite engine (`node:sqlite`).

- **No manual installation or database server setup is required.**
- The database file is automatically created at `server/data/academic_dashboard.sqlite` upon server launch.
- Database tables (`users`, `attendance`, `notes`, `question_papers`) and default constraints are initialized automatically.
- To re-seed sample data anytime, run:
  ```bash
  node scratch/seed-sample-data.js
  ```
