# EduNexus ERP — Education Resource Planning System

A modern, full-stack Education ERP built with **Next.js 15**, **Supabase**, **Drizzle ORM**, **Better Auth**, and **Tailwind CSS v4**.

---

## Tech Stack

| Layer         | Technology                       |
|---------------|----------------------------------|
| Framework     | Next.js 15 (App Router, Turbopack) |
| UI            | Tailwind CSS v4, Radix UI, Framer Motion |
| Auth          | Better Auth v1.5 (email/password) |
| Database      | Supabase (PostgreSQL)            |
| ORM           | Drizzle ORM                      |
| Validation    | Zod + React Hook Form            |
| Charts        | Recharts                         |
| Notifications | Sonner                           |
| Language      | TypeScript 5.7                   |

---

## Modules (10 Modules)

1. **Student & Admissions Management** — Admissions dashboard, application forms, student database, enrollment, courses, documents
2. **CRM & Lead Management** — Lead dashboard, inquiry pipeline, communication timeline, campaign tracking
3. **Academics & Course Management** — Courses, curriculum, timetable, attendance, exams, results
4. **Fees & Finance Management** — Fee structures, payment tracking, invoices, expenses, P&L reports
5. **Faculty & HR Management** — Faculty profiles, attendance/leave, payroll, permissions
6. **Campus & Operations** — Assets, library, hostel, transport, inventory
7. **Legal & Compliance** — Document repository, compliance dashboard, accreditation
8. **Analytics & Reporting** — Executive dashboard, admission/academic/financial reports
9. **Student & Parent Web Portal** — Student dashboard, attendance, fees, results, documents
10. **Admin & System Settings** — Roles, permissions, integrations, notifications, system config

---

## Getting Started

### 1. Clone and Install

```bash
git clone <your-repo>
cd education-erp
npm install
```

### 2. Set Up Supabase

1. Go to [supabase.com](https://supabase.com) and create a new project
2. Copy the **database connection string** from Settings → Database

### 3. Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env`:

```env
DATABASE_URL=postgresql://postgres:[YOUR-PASSWORD]@db.[YOUR-PROJECT-REF].supabase.co:5432/postgres
DIRECT_URL=postgresql://postgres:[YOUR-PASSWORD]@db.[YOUR-PROJECT-REF].supabase.co:5432/postgres

BETTER_AUTH_SECRET=generate-a-random-32-char-string-here
BETTER_AUTH_URL=http://localhost:3000

NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME="EduNexus ERP"
```

> Generate a secret: `openssl rand -base64 32`

### 4. Push Database Schema

```bash
npm run db:push
```

This creates all tables in your Supabase PostgreSQL database.

### 5. Run the Dev Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) → you'll be redirected to the login page.

### 6. Create Your Admin Account

Click "Create account" on the login page, fill in your details, and you're in!

---

## Project Structure

```
src/
├── app/
│   ├── api/                    # 20+ REST API routes
│   │   ├── auth/[...all]/      # Better Auth handler
│   │   ├── students/           # CRUD + search/filter/paginate
│   │   ├── courses/            # Course management
│   │   ├── departments/        # Department management
│   │   ├── faculty/            # Faculty CRUD
│   │   ├── payments/           # Fee payments
│   │   ├── fees/               # Fee structures
│   │   ├── invoices/           # Invoice generation
│   │   ├── expenses/           # Expense tracking
│   │   ├── attendance/         # Bulk attendance marking
│   │   ├── examinations/       # Exam scheduling
│   │   ├── results/            # Result management
│   │   ├── timetable/          # Timetable management
│   │   ├── batches/            # Batch management
│   │   ├── subjects/           # Subject management
│   │   ├── leads/              # CRM leads
│   │   ├── leaves/             # Faculty leaves
│   │   ├── payroll/            # Payroll processing
│   │   ├── compliance/         # Compliance documents
│   │   └── analytics/          # Dashboard analytics
│   ├── auth/
│   │   ├── login/              # Login page
│   │   └── register/           # Registration page
│   ├── (dashboard)/            # Protected dashboard layout
│   │   ├── admissions/         # Main dashboard
│   │   ├── students/           # Student database
│   │   ├── crm/                # CRM & leads
│   │   ├── academics/          # Courses, attendance, exams
│   │   ├── finance/            # Payments, fees, expenses
│   │   ├── faculty/            # Faculty & HR
│   │   ├── campus/             # Campus operations
│   │   ├── compliance/         # Legal & compliance
│   │   ├── analytics/          # Analytics & reporting
│   │   ├── reports/            # Web portals
│   │   └── settings/           # System settings
│   ├── globals.css             # Tailwind v4 theme
│   └── layout.tsx              # Root layout
├── components/
│   ├── ui/                     # Button, Input, Badge, Card, Modal, Select, Textarea
│   ├── layout/                 # Sidebar, TopBar
│   ├── dashboard/              # StatCard, PageHeader
│   └── tables/                 # DataTable with pagination
├── db/
│   ├── index.ts                # Drizzle + postgres connection
│   └── schema/
│       ├── auth.ts             # Users, sessions, accounts, verifications
│       ├── students.ts         # Students, courses, departments, batches, subjects
│       ├── faculty.ts          # Faculty, leaves, payroll
│       ├── finance.ts          # Fee structures, payments, expenses, invoices
│       ├── academics.ts        # Timetable, attendance, examinations, results
│       ├── campus.ts           # Assets, library, hostel, transport, compliance, leads
│       └── index.ts            # Re-exports all schemas
└── lib/
    ├── auth.ts                 # Better Auth server config
    ├── auth-client.ts          # Better Auth client
    ├── api-helpers.ts          # Auth middleware, response helpers
    └── utils.ts                # cn() utility
```

---

## API Reference

All endpoints require authentication (session cookie).

### Students
- `GET    /api/students?page=1&limit=20&search=&status=&courseId=&sortBy=createdAt&sortOrder=desc`
- `POST   /api/students` — Create student
- `GET    /api/students/[id]` — Get student with course & batch
- `PUT    /api/students/[id]` — Update student
- `DELETE /api/students/[id]` — Delete student

### Courses
- `GET    /api/courses?all=true` — All active courses
- `GET    /api/courses?page=1&limit=50&search=` — Paginated
- `POST   /api/courses` — Create course

### Departments
- `GET    /api/departments?all=true` — All departments
- `POST   /api/departments` — Create department

### Faculty
- `GET    /api/faculty?page=1&search=&departmentId=`
- `POST   /api/faculty` — Create faculty member

### Payments
- `GET    /api/payments?page=1&status=&studentId=`
- `POST   /api/payments` — Record payment

### Attendance
- `GET    /api/attendance?studentId=&subjectId=&date=`
- `POST   /api/attendance` — Mark attendance (supports bulk array)

### Other Endpoints
- `/api/batches`, `/api/subjects`, `/api/fees`, `/api/expenses`
- `/api/invoices`, `/api/examinations`, `/api/results`
- `/api/timetable`, `/api/leads`, `/api/leaves`, `/api/payroll`
- `/api/compliance`, `/api/analytics`

---

## Database Commands

```bash
npm run db:push      # Push schema to database (development)
npm run db:generate  # Generate migration files
npm run db:migrate   # Run migrations (production)
npm run db:studio    # Open Drizzle Studio GUI
```

---

## Design Philosophy

- **Tailwind CSS v4** with CSS-first `@theme` configuration — no `tailwind.config.js`
- **Outfit** display font + **Plus Jakarta Sans** body font + **JetBrains Mono** for code
- Emerald brand palette with refined surface colors
- Glass morphism, subtle gradients, and micro-animations
- Responsive sidebar with collapse support
- Dark mode ready (CSS variables configured)

---

## License

MIT
