# ⚡ Production-Style Task Management System

A polished, full-stack Task Management System engineered with **React 19**, **TypeScript**, **Node.js**, **Express**, **Prisma ORM**, and **PostgreSQL**. Built with robust security practices, data isolation, real-time filters, metrics dashboard, and responsive SaaS UI.

---

## 🚀 Key Features

### 🔐 Authentication & Authorization
- **User Registration & Login**: Secure password hashing with `bcryptjs` and JWT token issue.
- **HTTP-Only Cookies**: Secure session token storage protecting against XSS attacks.
- **Strict User Isolation**: All backend database queries strictly filter by authenticated `userId` context.

### 📋 Comprehensive Task Management
- **Task CRUD**: Create, edit, delete, and view task details.
- **Task Lifecycle Status**: Track tasks through `TODO`, `IN_PROGRESS`, and `COMPLETED` states.
- **Priority Matrix**: Color-coded `LOW`, `MEDIUM`, and `HIGH` priority indicators.
- **Categorization**: Assign tags/categories (e.g., *Work*, *Personal*, *Urgent*, *Feature*).
- **Due Date & Overdue Engine**: Automatic calculation of *Due Today*, *Upcoming*, and *Overdue* states with visual alerts.
- **Task Detail Modal**: In-depth inspector modal for notes, timestamps, category, priority, and inline edits.

### 🔍 Advanced Search & Real-Time Filtering
- **Full-Text Search**: Instant search across titles, descriptions, and category tags.
- **Multi-Criteria Filtering**: Combine status tabs, priority dropdowns, category selectors, and due date filters.
- **Multi-Attribute Sorting**: Order tasks by priority (High → Low, Low → High), due date, title (A → Z), or creation timestamp.
- **Reset Toolbar**: One-click reset for all applied filters.

### 📊 SaaS Dashboard & Metrics
- **Live Visual Cards**: Real-time counts for Total Tasks, Completed, In-Progress, High Priority, and Overdue tasks.
- **Completion Celebration**: Dynamic micro-animations on completing tasks.

---

## 🛠️ Tech Stack

| Domain | Technology |
| :--- | :--- |
| **Frontend Framework** | React 19, TypeScript, Vite |
| **Styling & Components** | Tailwind CSS (v4), Glassmorphism UI, Responsive Layouts |
| **Backend Framework** | Node.js, Express 5, TypeScript |
| **ORM & Database** | Prisma ORM 7, PostgreSQL |
| **Authentication** | JSON Web Tokens (`jsonwebtoken`), `bcryptjs`, Cookie Parser |
| **Validation** | Zod (Types), Express-Validator (Route middleware) |
| **Documentation & API** | Swagger UI (`swagger-jsdoc`, `swagger-ui-express`) |
| **Testing** | Jest, ts-jest |

---

## 🏗️ System Architecture & Data Flow

```
[ React 19 Frontend ] 
       │ (Axios HTTP / REST API with Credentials)
       ▼
[ Express API Gateway ] ────► [ Auth Middleware (JWT Cookie Validation) ]
       │                                     │
       ▼                                     ▼
[ Zod & Express Validators ] ──► [ Tasks / Auth Controller ]
                                             │
                                             ▼
                                  [ Prisma ORM Client ]
                                             │
                                             ▼
                                  [ PostgreSQL Database ]
```

---

## 📐 Database Schema & Entity Relationships

```
┌───────────────────────────┐         ┌────────────────────────────────┐
│           User            │         │              Task              │
├───────────────────────────┤         ├────────────────────────────────┤
│ id        Int (PK)        │ 1     * │ id          Int (PK)           │
│ email     String (Unique) ├─────────┤ title       String             │
│ name      String          │         │ description String?            │
│ password  String (Hashed) │         │ status      Enum(Status)       │
│ createdAt DateTime        │         │ priority    Enum(Priority)     │
│ updatedAt DateTime        │         │ dueDate     DateTime?          │
└───────────────────────────┘         │ category    String?            │
                                      │ done        Boolean            │
                                      │ userId      Int (FK -> User)   │
                                      │ createdAt   DateTime           │
                                      │ updatedAt   DateTime           │
                                      └────────────────────────────────┘
```

### Database Performance Indexes
- `@@index([userId, status])` – Fast lookup for task status filters.
- `@@index([userId, priority])` – Optimized priority sorting & filtering.
- `@@index([userId, dueDate])` – Efficient query processing for overdue and due today calculations.

---

## 🔌 API Documentation

### Authentication Endpoints
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register a new user | ❌ |
| `POST` | `/api/auth/login` | Login user & issue JWT cookie | ❌ |
| `POST` | `/api/auth/logout` | Clear auth cookie | 🔒 |
| `GET` | `/api/auth/profile` | Get current logged-in user profile | 🔒 |

### Task Management Endpoints
| Method | Endpoint | Description | Query Parameters |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/tasks` | Get paginated user tasks | `q`, `status`, `priority`, `category`, `dueFilter`, `sortBy`, `order`, `page`, `limit` |
| `GET` | `/api/tasks/stats` | Get dashboard summary metrics | None |
| `GET` | `/api/tasks/:id` | Get single task by ID | None |
| `POST` | `/api/tasks` | Create a new task | Payload: `title`, `description`, `status`, `priority`, `dueDate`, `category` |
| `PUT` | `/api/tasks/:id` | Update an existing task | Payload: Partial task fields |
| `PATCH`| `/api/tasks/:id/toggle`| Toggle task status | None |
| `DELETE`| `/api/tasks/:id`| Delete task | None |

*Interactive Swagger documentation available at `/api/docs` when running the server.*

---

## ⚙️ Environment Configuration

Create a `.env` file inside the `backend` directory based on `.env.example`:

```env
PORT=5001
NODE_ENV=development
DATABASE_URL="postgresql://username:password@localhost:5432/taskmanager?schema=public"
JWT_SECRET="your_secure_jwt_secret_key_here"
FRONTEND_APP_URL="http://localhost:5173"
```

Create a `.env` file inside the `frontend` directory:

```env
VITE_API_URL="http://localhost:5001/api"
```

---

## 💻 Local Setup & Installation

### Prerequisites
- **Node.js**: `>= 20.x`
- **PostgreSQL**: Running locally or via cloud (e.g. Supabase, Neon, Render)

### 1. Clone Repository
```bash
git clone https://github.com/SAYALI8106/Task-Manager.git
cd Task-Manager
```

### 2. Setup Backend
```bash
cd backend
npm install
npx prisma generate
npx prisma db push
npm run dev
```

### 3. Setup Frontend
```bash
cd ../frontend
npm install
npm run dev
```

---

## 🧪 Testing

The backend includes automated unit & controller test coverage with Jest:

```bash
# Run backend tests
npm --prefix backend run test

# Run TypeScript compilation check
npm --prefix backend run build
npm --prefix frontend run build
```

---

## 💡 Placement & Technical Interview Preparation

Here are key architectural and technical concepts implemented in this repository to confidently discuss during technical interviews:

1. **State Management & Custom Hooks**: `useTasks` encapsulates query state, optimistic UI updates, and backend API interaction cleanly separated from presentation components.
2. **Relational Database Indexing**: Multi-column indexes on `(userId, status)`, `(userId, priority)`, and `(userId, dueDate)` ensure $O(\log N)$ query speed as task volume grows.
3. **Defense-in-Depth Security**: Password hashing (`bcrypt`), HttpOnly cookie tokens to prevent XSS credential theft, strict server-side Zod payload validation, and SQL injection prevention via Prisma prepared statements.
4. **Data Isolation in Multi-Tenant REST APIs**: Enforcing user authorization in controller logic so users can never view, update, or mutate another user's tasks ($404/403$ responses).
5. **Robust Date Handling**: Timezone-safe UTC storage with local timezone rendering for overdue and due-today calculations.


