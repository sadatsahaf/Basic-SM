# Basic-SM

A **learning project** built to explore the [FastAPI](https://fastapi.tiangolo.com/) framework with Python. The project takes the shape of a basic social media backend, covering real-world concepts like user authentication, file uploads, async database access, and RESTful API design.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | FastAPI |
| ASGI Server | Uvicorn |
| Auth | fastapi-users (JWT) |
| ORM | SQLAlchemy (async) |
| Database | SQLite via aiosqlite |
| Image Storage | ImageKit.io |
| Config | python-dotenv |
| Package Manager | uv |

---

## What I Learned

- Setting up a **FastAPI** project from scratch with lifespan events
- Defining **REST API routes** — GET, POST, DELETE
- Integrating **JWT authentication** with `fastapi-users` (register, login, password reset, verification)
- Designing **SQLAlchemy async models** with relationships (User ↔ Post)
- Handling **file uploads** (images & videos) using `UploadFile`
- Storing and serving media via **ImageKit.io CDN**
- Managing environment variables securely with `.env` and `python-dotenv`
- Structuring a multi-file FastAPI project (routes, models, schemas, db, auth)

---

## Project Structure

```
Basic-SM/
├── app/
│   ├── app.py        # FastAPI app, routes (upload, feed, delete)
│   ├── db.py         # SQLAlchemy models (User, Post) & DB setup
│   ├── schemas.py    # Pydantic schemas for request/response
│   ├── users.py      # fastapi-users config (JWT, UserManager)
│   └── images.py     # ImageKit.io client setup
├── main.py           # Uvicorn entry point
├── pyproject.toml    # Project metadata & dependencies
├── ISSUES.md         # Known bugs and improvement notes
└── README.md
```

---

## API Endpoints

### Auth (via fastapi-users)

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/register` | Register a new user |
| `POST` | `/auth/jwt/login` | Login and receive JWT token |
| `POST` | `/auth/forgot-password` | Request password reset |
| `POST` | `/auth/reset-password` | Reset password with token |
| `POST` | `/auth/request-verify-token` | Request email verification |
| `POST` | `/auth/verify` | Verify account |

### Posts

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/upload` | Upload an image or video with a caption |
| `GET` | `/feed` | Get all posts ordered by newest first |
| `DELETE` | `/post/{post_id}` | Delete a post by ID |

> Interactive docs available at `http://localhost:8000/docs` after running the server.

---

## Data Models

### User
Managed by `fastapi-users` — includes email, hashed password, and active/verified status.

### Post
| Field | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `user_id` | UUID | Foreign key to User |
| `caption` | Text | Optional caption |
| `url` | String | ImageKit CDN URL |
| `file_type` | String | `"image"` or `"video"` |
| `file_name` | String | File name on ImageKit |
| `created_at` | DateTime | Upload timestamp |

---

## Getting Started

### Prerequisites

- Python >= 3.10
- [`uv`](https://docs.astral.sh/uv/) package manager
- An [ImageKit.io](https://imagekit.io/) account (free tier works)

### Installation

```bash
git clone https://github.com/sadatsahaf/Basic-SM.git
cd Basic-SM
uv sync
```

### Configuration

Create a `.env` file in the project root:

```env
IMAGEKIT_PUBLIC_KEY=your_public_key
IMAGEKIT_PRIVATE_KEY=your_private_key
IMAGEKIT_URL_ENDPOINT=https://ik.imagekit.io/your_imagekit_id
```

> ⚠️ Never commit your `.env` file. Add it to `.gitignore`.

### Run the Server

```bash
python main.py
```

Server runs at `http://localhost:8000`  
Swagger UI at `http://localhost:8000/docs`

---

## Known Issues

See [`ISSUES.md`](./ISSUES.md) for a full list of known bugs and improvement notes identified during development. This is intentional — part of the learning process was identifying and documenting what went wrong.

---

## Notes

This is a personal learning project and not intended for production. The codebase was built incrementally while exploring FastAPI concepts, and some parts may be incomplete or contain bugs documented in `ISSUES.md`.