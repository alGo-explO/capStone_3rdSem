# ReadingHub – Personal Digital Library Manager

A full-stack web application for managing your personal reading life. Track books, log reading sessions, organize collections, and get AI-powered insights and recommendations — all in one place.

## Live Demo

**Frontend:** [https://capstone-sem3-kappa.vercel.app](https://capstone-sem3-kappa.vercel.app)  
**Backend API:** [https://capstone-sem3-12k8.onrender.com](https://capstone-sem3-12k8.onrender.com)

---

## Tech Stack

| Layer          | Technology                                      |
| -------------- | ----------------------------------------------- |
| Frontend       | React 18, React Router v6, Tailwind CSS, Vite   |
| Backend        | Node.js, Express.js (ES Modules)                |
| Database       | Neon (PostgreSQL), Prisma ORM                   |
| AI             | Google Gemini API (`@google/generative-ai`)     |
| Authentication | JWT (JSON Web Tokens)                           |
| Deployment     | Frontend: Vercel, Backend: Render               |

---

## Features

### Book Management (CRUD)
- Add books with title, author, genre, pages, ISBN, publisher, published year, language, format, and priority
- Five reading statuses: `To Read`, `In Progress`, `Completed`, `Did Not Finish`, `On Hold`
- Four book formats: `Physical`, `eBook`, `Audiobook`, `PDF`
- Edit all book details including reading progress (current page)
- Delete books with cascading cleanup of related data
- Personal notes, reviews, and 1–5 star ratings per book
- Automatic date tracking: date added, date started, date finished

### Search, Filter & Sort
- Search by title or author (case-insensitive)
- Filter by reading status and genre
- Sort by date added, title, author, or date finished
- Pagination with configurable page size

### Reading Sessions
- Log individual reading sessions with start page, end page, and duration
- Optional mood and location tracking per session
- Automatically updates the book's current page when a session is logged
- Auto-transitions a book from `To Read` to `In Progress` on first session

### Collections
- Create named collections with custom colors and descriptions
- Organize books into multiple collections
- Optional public/private visibility

### Analytics Dashboard
- Reading stats by timeframe: month, quarter, or year
- Pages read, reading time (hours), books completed
- Reading streak and longest streak tracking
- Average pages per day and average session duration
- Yearly reading goal progress
- Genre distribution breakdown
- Monthly reading progress chart

### AI Insights (Google Gemini)
- Personalized book recommendations based on your library
- Reading pattern analysis and habit insights
- Book summaries with context from your personal notes
- Reading goal suggestions tailored to your history
- Conversational AI chat about books — ask anything about a specific book or your library in general
- Graceful fallbacks when AI is unavailable

### Authentication & Profiles
- JWT-based signup and login
- User profile with bio, location, website, and yearly reading goal
- All data is scoped per user — isolated and private

---

## API Reference

### Auth

| Endpoint              | Method | Auth | Description         |
| --------------------- | ------ | ---- | ------------------- |
| `/api/auth/signup`    | POST   | —    | Register new user   |
| `/api/auth/login`     | POST   | —    | Login, get JWT      |
| `/api/auth/me`        | GET    | ✓    | Get current user    |
| `/api/auth/profile`   | GET    | ✓    | Get user profile    |
| `/api/auth/profile`   | PUT    | ✓    | Update profile      |

### Books

| Endpoint               | Method | Auth | Description                            |
| ---------------------- | ------ | ---- | -------------------------------------- |
| `/api/books`           | GET    | ✓    | List books (search, filter, sort, page)|
| `/api/books/:id`       | GET    | ✓    | Get single book                        |
| `/api/books`           | POST   | ✓    | Add new book                           |
| `/api/books/:id`       | PUT    | ✓    | Update book                            |
| `/api/books/:id`       | DELETE | ✓    | Delete book                            |
| `/api/books/stats/overview` | GET | ✓  | Quick count stats                      |

### Reading Sessions

| Endpoint                      | Method | Auth | Description                  |
| ----------------------------- | ------ | ---- | ---------------------------- |
| `/api/reading-sessions`       | GET    | ✓    | List sessions (paginated)    |
| `/api/reading-sessions`       | POST   | ✓    | Log new session              |
| `/api/reading-sessions/stats` | GET    | ✓    | Aggregate session stats      |
| `/api/reading-sessions/:id`   | DELETE | ✓    | Delete session               |

### Collections

| Endpoint                | Method | Auth | Description         |
| ----------------------- | ------ | ---- | ------------------- |
| `/api/collections`      | GET    | ✓    | List collections    |
| `/api/collections`      | POST   | ✓    | Create collection   |
| `/api/collections/:id`  | DELETE | ✓    | Delete collection   |

### Analytics

| Endpoint                          | Method | Auth | Description                |
| --------------------------------- | ------ | ---- | -------------------------- |
| `/api/analytics/stats`            | GET    | ✓    | Reading stats by timeframe |
| `/api/analytics/reading-progress` | GET    | ✓    | Progress chart data        |
| `/api/analytics/genres`           | GET    | ✓    | Genre distribution         |

### AI

| Endpoint                    | Method | Auth | Description                      |
| --------------------------- | ------ | ---- | -------------------------------- |
| `/api/ai/recommendations`   | GET    | ✓    | Personalized book recommendations|
| `/api/ai/insights`          | GET    | ✓    | Reading pattern insights         |
| `/api/ai/summary/:bookId`   | GET    | ✓    | AI summary for a specific book   |
| `/api/ai/goals`             | GET    | ✓    | Suggested reading goals          |
| `/api/ai/chat`              | POST   | ✓    | Conversational AI chat           |

---

## Project Structure

```
capStone_3rdSem/
├── frontend/
│   ├── src/
│   │   ├── components/       # Navbar, BookCard, BookSearch
│   │   ├── pages/            # Auth, Dashboard, Library, AddBook, EditBook,
│   │   │                     # BookDetails, Collections, ReadingSessions,
│   │   │                     # Analytics, AIInsights, Profile, SearchBooks
│   │   ├── config.js         # API URL config (dev/prod)
│   │   ├── App.jsx           # Routes and auth state
│   │   └── main.jsx
│   ├── vite.config.js
│   └── tailwind.config.js
│
└── backend/
    ├── routes/
    │   ├── auth.js
    │   ├── books.js
    │   ├── collections.js
    │   ├── readingSessions.js
    │   ├── analytics.js
    │   └── ai.js
    ├── services/
    │   └── geminiService.js  # Google Gemini AI integration
    ├── prisma/
    │   ├── schema.prisma     # Full data model
    │   └── migrations/       # 4 migration history files
    └── server.js
```

---

## Local Setup

### Prerequisites
- Node.js 18+
- A [Neon](https://neon.tech) PostgreSQL database
- A [Google AI Studio](https://aistudio.google.com) API key for Gemini

### 1. Clone the repo

```bash
git clone https://github.com/your-username/capStone_3rdSem.git
cd capStone_3rdSem
```

### 2. Backend

```bash
cd backend
npm install
```

Create a `.env` file:

```env
DATABASE_URL="postgresql://user:password@host/dbname?sslmode=require"
JWT_SECRET="your-secure-random-secret"
PORT=5001
NODE_ENV=development
GEMINI_API_KEY="your-gemini-api-key"
```

Run migrations and start:

```bash
npx prisma migrate deploy
node server.js
```

Backend runs on `http://localhost:5001`.

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend runs on `http://localhost:3000` (or the next available port).

---

## Data Model

The Prisma schema defines these models:

- **User** – accounts, profile, reading goal, follow relationships
- **Book** – full book metadata, reading status, progress, notes, reviews, ratings
- **ReadingSession** – individual reading logs with pages, duration, mood, location
- **Collection** – named book groupings with color and visibility
- **BookCollection** – many-to-many join between books and collections
- **Bookmark** – page bookmarks with optional notes
- **Quote** – saved quotes from books
- **Review** – structured reviews with rating, title, content
- **Follow** – user follow relationships

---

## Deployment

| Component | Platform  | Notes                              |
| --------- | --------- | ---------------------------------- |
| Frontend  | Vercel    | Auto-deploys from main branch      |
| Backend   | Render    | Web service, port set automatically|
| Database  | Neon      | Serverless PostgreSQL              |

Set these environment variables on Render:
- `DATABASE_URL` — Neon connection string
- `JWT_SECRET` — secure random string (32+ chars)
- `GEMINI_API_KEY` — Google AI key
- `NODE_ENV=production`

---

## Credits

Developed by **Sambhav Kumar**  
Stack: React, Node.js, Express, Prisma, Neon PostgreSQL, Google Gemini AI

## License

Open for educational and personal use.
