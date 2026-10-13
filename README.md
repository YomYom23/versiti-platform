# Versiti

A university/student matching platform. Students describe their academics, budget, intended major, location and campus preferences. Universities describe their programs, tuition and admission profile. Each side sees a 0–100 match score against the other, with a breakdown of why.

This repository is the umbrella for two projects, included as git submodules:

| Project | Stack | What it is |
| --- | --- | --- |
| [`university-project-frontend`](./university-project-frontend) | React 19, TypeScript, Vite, React Router, Framer Motion | Landing page, sign-up/sign-in, a multi-step student onboarding flow, and student and university dashboards |
| [`University-project-api`](./University-project-api) | Bun, Express, TypeScript, PostgreSQL, Prisma Next | REST API: cookie/JWT auth and onboarding persistence |

## What works

- Registration and login for `STUDENT`, `UNIVERSITY_REP` and `ADMIN` roles. Passwords are hashed with bcrypt, the session is an httpOnly JWT cookie, and the password hash is never returned in a response.
- Student onboarding that auto-saves one step at a time to the API and resumes where the user left off.
- A match-scoring model, run client-side, that scores a student against a university out of 100 across five weighted factors: academics (25), budget (25), major (20), campus vibe (15), location (15).
- Student dashboard: universities ranked by match score, filters, a saved list, and a profile view.
- University dashboard: candidate list ranked by match score, a shortlist, and summary stats.
- Protected routes on the frontend, and an auth middleware on the API.

## Current scope

This is a working prototype, not a finished product.

- The dashboards rank **sample data** (`src/data/mockUniversities.ts`, `src/data/mockStudents.ts`), not database records. Only account creation and student onboarding are stored in PostgreSQL.
- The university onboarding form exists, but it isn't saved to the API yet; `UniversityProfile` exists in the schema. Messages is a placeholder.
- Match scoring runs in the browser. Moving it to the API is the next step, once universities are stored.
- There is no automated test suite.

## Architecture

```
React SPA (Vite, :5173)
   │  fetch, credentials: "include"
   ▼
Express API (Bun, :3000)
   ├─ /auth   registration · login · logout · status
   └─ /users  me (GET) · onboarding save (PATCH)
   │
   ▼
PostgreSQL  ←  User · StudentProfile · UniversityProfile
```

## Running it locally

See each project's README. You need Bun, a PostgreSQL 15+ database, and both servers running: the API on port 3000, the frontend on port 5173.

```bash
git clone --recurse-submodules <this repo>
```
