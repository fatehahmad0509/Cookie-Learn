# 🍪 CookieLearn

**Modern, AI-powered, Duolingo-like language learning platform.**
React + TypeScript frontend, FastAPI + PostgreSQL backend, live lesson generation with Google Gemini 3.1 Flash Lite.

![CookieLearn](https://images.unsplash.com/photo-1772371272174-392cf9cfabae?w=800)

## ✨ Features

### 🎯 Learning System
- **10 Languages**: English, Turkish, Azerbaijani, German, French, Spanish, Italian, Japanese, Korean, Russian
- **CEFR level support** (A1 → C2)
- **7 Question types**: Multiple choice, fill in the blank, translation, word matching, word ordering, listening, sentence completion
- **AI-Dynamic Lessons**: Gemini 3.1 generates fresh questions every time
- Units → Sections → Lessons → Questions structure

### 🎮 Gamification
- XP + level system
- Streak (daily series) + streak flame animation
- Hearts system (auto-refills on loss)
- Daily goal + daily quests
- Diamond economy (refill hearts, etc.)
- Badges & achievements (12 unique)
- Weekly leagues (Bronze → Master, auto promotion)
- League rankings / leaderboard

### 🤖 AI Features (Gemini 3.1 Flash Lite)
- **AI Teacher (Cookie)** — answers questions within lesson context
- **Speaking Practice** — chat with AI in target language + error correction
- **Wrong answer explanation** — explains why it's wrong and what's correct
- **Word explanations** — meaning, pronunciation, examples, synonyms/antonyms
- **Dynamic question generation** — personalized quizzes based on level and weak topics
- **Translation + grammar note** — not just translates, explains usage
- Analyzes user mistakes, produces more questions on weak topics

### 🎨 Design
- Deep space violet dark mode + vibrant light mode
- Duolingo-style tactile "pushable" buttons
- Skill tree (zig-zag path)
- Framer Motion animations
- Fully responsive (mobile-first)
- Outfit + Nunito typography

### 👤 User
- JWT-based auth (bcrypt password hashing)
- Profile editing + avatar upload
- Native language / level / daily goal settings

### 🛠️ Admin Panel
- Add/delete languages
- Unit/Section/Lesson/Question CRUD (API)
- User management (role, XP)

---

## 🛠️ Technologies

### Backend
- **Python 3.11** + **FastAPI 0.110**
- **SQLAlchemy 2.0** (async-ready)
- **PostgreSQL** (production) / SQLite (dev)
- **bcrypt** + **PyJWT**
- **google-genai** (Gemini 3.1 Flash Lite)
- **Pydantic v2** validation
- Auto-generated OpenAPI/Swagger docs at `/docs`

### Frontend
- **React 19** + **TypeScript 5.4**
- **Tailwind CSS 3.4** + **shadcn/ui**
- **Framer Motion** animations
- **React Router 7**
- **Recharts** charts
- **Sonner** toast notifications
- **TanStack Query** data management
- PWA ready

---

## 📁 Project Structure

```
/app
├── backend/
│   ├── server.py              # FastAPI entry point
│   ├── database.py            # SQLAlchemy engine + session
│   ├── models.py              # ORM models (User, Language, Unit, Lesson, Question, ...)
│   ├── schemas.py             # Pydantic schemas
│   ├── security.py            # JWT + bcrypt
│   ├── gamification.py        # XP/streak/hearts/quests/achievements
│   ├── ai_service.py          # Gemini 3.1 integration
│   ├── seed.py                # Seed data (languages, curriculum, badges, demo accounts)
│   ├── routers/
│   │   ├── auth.py            # Register/Login/Change password
│   │   ├── users.py           # Profile, avatar, active language
│   │   ├── languages.py       # Curriculum, lesson, question
│   │   ├── progress.py        # Submit answer, complete lesson, daily quests
│   │   ├── ai.py              # Chat, generate quiz, explain word, translate
│   │   ├── stats.py           # Statistics, achievements
│   │   ├── leaderboard.py     # League rankings
│   │   └── admin.py           # Admin CRUD
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── index.tsx
│   │   ├── contexts/          # Auth, Theme
│   │   ├── components/        # Layout, ProtectedRoute, QuestionRenderer, ui/
│   │   ├── pages/             # Landing, Login, Register, Dashboard, LessonPlayer, AIChat, Practice, Profile, Stats, Leaderboard, Achievements, Shop, Admin, LanguageSelect
│   │   ├── lib/api.ts         # Axios client + JWT
│   │   └── types/index.ts
│   ├── Dockerfile
│   ├── nginx.conf
│   ├── tsconfig.json
│   └── package.json
└── README.md
```

---

## 🚀 Installation (Local)

### Prerequisites
- Python 3.11+
- Node 20+, Yarn
- PostgreSQL 15+ (or auto SQLite for dev)
- Google AI Studio API Key ([aistudio.google.com](https://aistudio.google.com/apikey))

### Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Create .env
cat > .env <<EOF
DATABASE_URL=postgresql+psycopg2://user:pass@localhost:5432/cookielearn
JWT_SECRET=change-me-in-production
JWT_ALGORITHM=HS256
JWT_EXPIRE_MINUTES=10080
GEMINI_API_KEY=your-google-ai-studio-key
GEMINI_MODEL=gemini-3.1-flash-lite
CORS_ORIGINS=*
EOF

uvicorn server:app --reload --port 8001
```

Tables and seed data (10 languages, 12 badges, demo accounts, English curriculum) are auto-created on startup.

### Frontend

```bash
cd frontend
yarn install

# Create .env
echo 'REACT_APP_BACKEND_URL=http://localhost:8001' > .env

yarn start
```

---

## 🔐 Demo Accounts

- **Student**: `demo@cookielearn.app` / `demo123`
- **Admin**: `admin@cookielearn.app` / `admin123`

---

## 🌐 API

Swagger UI: `http://localhost:8001/docs`
OpenAPI JSON: `http://localhost:8001/openapi.json`

### Highlighted Endpoints
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/me`
- `PATCH /api/users/me`
- `POST /api/users/me/avatar`
- `GET /api/languages`
- `GET /api/languages/{code}/curriculum`
- `GET /api/languages/lessons/{id}/questions`
- `POST /api/progress/answer`
- `POST /api/progress/complete-lesson`
- `GET /api/progress/daily-quests`
- `GET /api/stats/me`
- `GET /api/stats/achievements`
- `GET /api/leaderboard`
- `POST /api/ai/chat` — AI Teacher & Speaking Practice
- `POST /api/ai/explain-wrong` — Wrong answer explanation
- `POST /api/ai/word` — Word explanation
- `POST /api/ai/quiz/generate` — Dynamic quiz generation
- `POST /api/ai/translate` — Translation + grammar note
- `/api/admin/*` — Admin CRUD

---

## ☁️ Deploy

### Render + Neon PostgreSQL

1. Create a new Postgres on **Neon**, get the connection string.
2. Push this repo to GitHub.
3. Create a **Backend Service** on Render:
   - Runtime: Docker (`/backend/Dockerfile`)
   - Env vars: `DATABASE_URL` (Neon), `JWT_SECRET`, `GEMINI_API_KEY`, `GEMINI_MODEL=gemini-3.1-flash-lite`, `CORS_ORIGINS`
4. Create a **Frontend Static Site** on Render:
   - Build: `cd frontend && yarn install && yarn build`
   - Publish dir: `frontend/build`
   - Env: `REACT_APP_BACKEND_URL=<render-backend-url>`

Or deploy both as `Docker` services.

---

## 📸 Screenshots

Landing → Dashboard (skill tree) → Lesson player (7 question types) → AI Chat → Stats charts → Leaderboard → Admin.

---

## 📜 License

MIT — Open source for portfolio purposes.

---

Powered by [Google Gemini 3.1](https://ai.google.dev/)
