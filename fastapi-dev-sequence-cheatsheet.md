# FastAPI Development Cheatsheet

## 🚀 INITIAL PROJECT SETUP (One-time)

```
1.  Create project structure
2.  config/settings.py          # ENV vars, DB URL
3.  database.py                 # SQLAlchemy engine, SessionLocal, Base
4.  models/__init__.py          # Import all models
5.  alembic init alembic        # Initialize migrations
6.  alembic/env.py              # Configure (import Base, models)
7.  dependencies/database.py    # get_db() dependency
8.  exceptions/handlers.py      # Custom exceptions
9.  middleware/                 # CORS, auth, logging
10. main.py                     # FastAPI app, middleware, exception handlers
11. .env, requirements.txt
```

## 📝 CREATING NEW ENDPOINT

### With NEW table:
```
1. models/xyz.py           # SQLAlchemy model
2. alembic revision        # Generate migration
3. alembic upgrade head    # Apply migration
4. schemas/xyz.py          # Request + Response models
5. repositories/xyz.py     # DB operations (CRUD)
6. services/xyz.py         # Business logic
7. routers/xyz.py          # Endpoints
8. main.py                 # app.include_router(xyz_router)
9. tests/test_xyz.py       # Tests
```

### With EXISTING table:
```
1. schemas/xyz.py          # New schemas (if needed)
2. repositories/xyz.py     # New DB method (if needed)
3. services/xyz.py         # Business logic
4. routers/xyz.py          # Endpoint
5. tests/test_xyz.py       # Tests
```

### Simple endpoint (no new DB logic):
```
1. schemas/ (update)       # New schema if needed
2. services/ (update)      # Call existing repo methods
3. routers/ (update)       # Add endpoint
4. test
```

## 📂 PROJECT STRUCTURE

```
project/
├── alembic/
├── app/
│   ├── config/
│   │   └── settings.py
│   ├── database.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── schemas/
│   │   └── user.py
│   ├── repositories/
│   │   └── user.py
│   ├── services/
│   │   └── user.py
│   ├── routers/
│   │   └── user.py
│   ├── dependencies/
│   │   ├── database.py
│   │   └── auth.py
│   ├── exceptions/
│   │   └── handlers.py
│   ├── middleware/
│   └── main.py
├── tests/
├── .env
└── requirements.txt
```

## 🔥 QUICK DECISION TREE

**New endpoint needs:**
- **New table?** → Model → Migration → Schema → Repo → Service → Router
- **Existing table?** → Schema (maybe) → Service → Router
- **Just logic change?** → Service → Router
- **Only validation change?** → Schema

## 💡 PRO TIPS

- **Always start simple**: Model → Schema → Router (skip repo/service if trivial)
- **Add layers when needed**: Refactor simple router to use service later
- **Test after each layer**: Don't build everything then test
- **Feature folders** (for large projects): `features/users/{model,schema,repo,service,router}.py`

**Rule of thumb**: Model first (if DB), Schema second, Router last.
