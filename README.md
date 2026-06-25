# Add-API (Gorilla / API Zoo)

![License](https://img.shields.io/github/license/Amokhalad/addapi-gorilla)
![Top language](https://img.shields.io/github/languages/top/Amokhalad/addapi-gorilla)
![Last commit](https://img.shields.io/github/last-commit/Amokhalad/addapi-gorilla)

Tooling for adding APIs to the Gorilla LLM "API Zoo."

Point the backend at API documentation URLs and it scrapes each page, uses an
LLM to extract structured API details (name, call signature, arguments,
example code, etc.), and can open a GitHub pull request against the upstream
[`ShishirPatil/gorilla`](https://github.com/ShishirPatil/gorilla) `apizoo` data
set on your behalf via GitHub OAuth.

## Structure

- **[backend/](./backend)** — Python service (Flask/WSGI) that extracts API
  details from documentation HTML. Includes gunicorn/nginx/systemd deployment
  notes in [backend/README.txt](./backend/README.txt).
- **[frontend/](./frontend)** — Next.js (TypeScript + Tailwind) web client.

> Note: the frontend is an early scaffold; the backend holds the implemented logic.

## Tech Stack

- **Backend:** Python, [Flask](https://flask.palletsprojects.com/) +
  Flask-Cors, [LangChain](https://www.langchain.com/) /
  [OpenAI](https://platform.openai.com/) for extraction, BeautifulSoup +
  html2text for HTML processing, served via gunicorn in deployment.
- **Frontend:** [Next.js](https://nextjs.org/) 14, TypeScript, and
  [Tailwind CSS](https://tailwindcss.com/).

## Backend — running locally

Requirements: Python 3.x.

```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in `backend/` with the variables the service reads (see
[Environment variables](#environment-variables) below), then run it.

Development (Flask, with reload):

```bash
python addapi_server.py          # serves on http://localhost:8080
```

Production-style (gunicorn + WSGI entrypoint):

```bash
gunicorn --bind 0.0.0.0:8080 wsgi:app
```

Either way, a quick health check is available at `GET /hello`. See
[backend/README.txt](./backend/README.txt) for the gunicorn/nginx/systemd
deployment notes.

### Environment variables

The backend reads the following from the environment (via `.env`):

| Variable               | Used for                                                      |
| ---------------------- | ------------------------------------------------------------- |
| `OPENAI_API_KEY`       | LLM-based extraction of API details from documentation.       |
| `GITHUB_CLIENT_ID`     | GitHub OAuth — exchanging codes and validating access tokens. |
| `GITHUB_CLIENT_SECRET` | GitHub OAuth — exchanging codes and validating access tokens. |

Do not commit your `.env`; it is already covered by `backend/.gitignore`.

## Frontend — running locally

```bash
cd frontend
npm install
npm run dev                      # serves on http://localhost:3000
```

## License

[MIT](./LICENSE)
