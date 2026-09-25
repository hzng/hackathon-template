# Hackathon Starter

A reusable frontend and API starter for hackathons: Vite + React + TypeScript, a FastAPI backend, and Supabase Postgres.

## Setup

### 1. Install prerequisites

Install the following tools before starting:

- Node.js `20.19+` or `22.12+`, with npm
- Python `3.11+`
- [uv](https://docs.astral.sh/uv/)
- A Supabase project

### 2. Configure the Supabase database

1. Open your Supabase project and select **Connect**.
2. Copy a Postgres connection string. For a persistent local FastAPI process, use the direct connection if your network supports IPv6; use the session pooler if it is IPv4-only. See [Supabase connection options](https://supabase.com/docs/guides/database/connecting-to-postgres).
3. From the repo root, copy the backend environment example:

   ```sh
   cp backend/.env.example backend/.env
   ```

4. Open `backend/.env` and set `DATABASE_URL` to the copied connection string. Change its scheme to `postgresql+asyncpg://` for the asyncpg driver. Keep the remaining host, port, username, and database from Supabase's string. Percent-encode special characters in the password.

Do not put the database password or a Supabase service-role key in frontend environment variables. Vite exposes variables prefixed with `VITE_` to browser code.

### 3. Start the FastAPI backend

Open a terminal at the repo root and run:

```sh
cd backend
uv sync
uv run fastapi dev
```

The API runs at `http://localhost:8000`. Open `http://localhost:8000/docs` for the interactive API documentation. The health endpoint at `http://localhost:8000/api/health` returns a successful status when the backend can reach Supabase. Without a configured database connection, it returns an error.

### 4. Start the frontend

Open a second terminal at the repo root and run:

```sh
cd frontend
npm install
npm run dev
```

Open the Vite URL shown in the terminal (usually `http://localhost:5173`). The Vite dev server forwards `/api` requests to FastAPI at `http://localhost:8000`.

### 5. Run the linters

From the repo root, run each linter in its project folder:

```sh
cd frontend
npm run lint
```

```sh
cd backend
uv run ruff check .
```

### 6. Open the UI component showcase

1. In the running app, turn on **Developer tools** in the top navigation.
2. Select **UI components** to open `/dev/ui`.
3. Turn Developer tools off to hide its navigation link and redirect away from the developer page.

The toggle preference is saved in the browser. It controls visibility only; it is not an authorization boundary.
Use the **Dark mode** switch in the top navigation to change the app theme. Your theme choice is saved in the browser.

## What's included

- **Frontend:** app shell, React Router routes, responsive light and dark themes, shared CSS design tokens, reusable UI components, and ESLint.
- **Backend:** FastAPI app, async SQLAlchemy connection to Supabase Postgres, API route structure, health check, and Ruff configuration.
- **Developer tools:** the `/dev/ui` component showcase and its page metadata.
- **Agent guidance:** `AGENTS.md` contains project-specific instructions for coding agents.

## Routes and developer-tool registry

- `/` — starter landing page with API/database connection status
- `/dev/ui` — showcase for buttons, text and select inputs, checkboxes, switches, cards, badges, alerts, avatars, progress, and loading states

Page metadata is stored in `frontend/src/config/pages.ts`. Mark a page with `developerTool: true` to include it in the show/hide toggle.

## Project layout

```text
frontend/
  src/app/           App shell and route configuration
  src/api/           Frontend API calls
  src/components/    Shared UI components and health status
  src/config/        Page registry, including developer-tool flags
  src/pages/         Home page and UI showcase page
  src/styles/        Global styles and design tokens
backend/
  app/api/           FastAPI route handlers
  app/core/          Settings and database connection
```

The starter intentionally has no product-specific tables. Once the hackathon idea defines the data model, add a Supabase migration and appropriate Row Level Security policies for tables exposed through Supabase APIs.
