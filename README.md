# Add-API (Gorilla / API Zoo)

Tooling for adding APIs to the Gorilla LLM "API Zoo." Consolidates the
previously separate backend and frontend repositories.

## Structure

- **[backend/](./backend)** — Python service (Flask/WSGI) that extracts API
  details from documentation HTML. Includes gunicorn/nginx/systemd deployment
  notes in [backend/README.txt](./backend/README.txt).
- **[frontend/](./frontend)** — Next.js (TypeScript + Tailwind) web client.

> Note: the frontend is an early scaffold; the backend holds the implemented logic.
