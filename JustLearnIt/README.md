# JustLearnIt

An LMS (Learning Management System) web application where students earn XP and gold by solving questions, teachers track assignments, and school administrators manage the entire platform. FastAPI backend + vanilla HTML/JS frontend.

## Features

**Student**

* Topic-based question solving (practice matches)
* Earning XP and gold
* Daily streak system
* Badge and achievement system
* Store: purchase score boosts, streak freezes, cosmetic badges, and loot chests with earned gold
* Assignment tracking and notifications
* Success statistics by topic

**Teacher**

* Class-based student list and statistics
* Assigning and updating homework
* Viewing courses and topics
* School-wide filtered analytics reports

**Admin**

* School, user, student, and teacher CRUD
* Course, topic, and question management
* Platform-wide statistics
* Audit log (all meaningful user actions are recorded in the `system_logs` table)

## Tech Stack

* **Backend:** Python 3.10+ · FastAPI 0.109 · Uvicorn
* **Database:** PostgreSQL (`psycopg2`)
* **Authentication:** JWT (`python-jose`) · SHA-256 password hashing
* **Frontend:** Vanilla HTML/CSS/JS (single file: `frontend/index.html`)

## Installation

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/JustLearnIt.git
cd JustLearnIt

```

2. **Create a virtual environment and install dependencies**

```bash
python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt

```

3. **Create the `.env` file**

```env
DB_URL=postgresql://user:password@localhost:5432/justlearnit
JWT_SECRET=change-for-production
PASSWORD_SALT=change-this-salt-too

```

4. **Start the application**

```bash
uvicorn main:app --reload

```

On first launch, the application automatically creates all tables and initial seed data (courses, store items, admin user).

No separate server is required for the frontend. Open `http://localhost:8000` — the backend serves `frontend/index.html` directly.

## Project Structure

```text
JustLearnIt/
├── main.py               # Application entry point
├── requirements.txt
├── core/
│   ├── db.py             # PostgreSQL connection, table creation, seed
│   ├── security.py       # JWT, password hashing, role-based dependencies
│   ├── achievements.py   # Badge awarding, score checking, statistics updates
│   └── logs.py           # Audit log (log_action)
├── modules/
│   ├── auth.py           # Login and JWT generation
│   ├── students.py       # Student profile, badges, assignments, notifications
│   ├── teacher.py        # Teacher dashboard and assignment management
│   ├── questions.py      # Course, topic, and question listing
│   ├── match.py          # Practice match start and answer submission
│   ├── market.py         # Store items and purchasing
│   ├── analytics.py      # Class and school-based statistics
│   └── admin.py          # Admin CRUD and reporting
└── frontend/
    └── index.html        # Single-page frontend

```

## API

Swagger UI: `http://localhost:8000/docs`

| Prefix | Scope |
| --- | --- |
| `POST /auth/login` | Login, obtain JWT |
| `GET /students/...` | Student profile, badges, assignments |
| `GET /questions/subjects` | Course and topic list |
| `POST /match/start` | Start a new practice match |
| `POST /match/submit` | Submit an answer |
| `GET /market/items` | Store item list |
| `POST /market/buy` | Purchase an item |
| `GET /teacher/...` | Teacher dashboard |
| `GET /analytics/...` | Analytics reports |
| `GET /admin/...` | Admin management |
| `GET /health` | Health check |

## License

MIT
