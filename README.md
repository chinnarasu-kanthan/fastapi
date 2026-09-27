# FastAPI Microservice

## Run locally

This project uses SQLite by default for local development. Install dependencies
with `uv sync`, then start the API with:

```bash
uv run uvicorn main:app --reload
```

The API is available at `http://127.0.0.1:8000`; interactive docs are at
`/docs` and the health endpoint is `/health`.

## Deploy to Render

1. Push this repository to GitHub or GitLab.
2. In Render, create a new Blueprint and select this repository. Render reads
	`render.yaml`, builds the `Dockerfile`, and provisions a PostgreSQL database.
3. Wait for the web service deployment to finish, then open its `onrender.com`
	URL. Render supplies the `DATABASE_URL` value from the managed database.

The Render database is configured on the free plan in `render.yaml`. Render's
free PostgreSQL instances are temporary and expire; select a paid database plan
in that file before relying on the service for persistent production data.

The app uses local SQLite when `DATABASE_URL` is unset. For a PostgreSQL URL,
the app uses the psycopg driver. Do not commit database credentials or `.env`
files.

## Docker

Build and run the API container locally:

```bash
docker build -t microservice .
docker run --rm -p 8000:8000 -e PORT=8000 microservice
```

## Optional Nginx proxy

`nginx.conf` is for a separately managed Nginx server in front of Uvicorn on
`127.0.0.1:8000`. It is not used by the Render deployment: Render terminates
HTTPS and proxies requests to the container itself.
