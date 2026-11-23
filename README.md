# DevOps Task 1 – Sample Node.js Application

A small Task Tracker used as the application source for the DevOps deployment assignment.

## Components
- `frontend/`: Node.js + Express app serving a simple browser UI on port **8080**.
- `backend/`: Node.js + Express REST API on port **9000**.
- `db`: PostgreSQL 16, provided by Docker Compose.
- `deploy/nginx-arshad.conf`: example host Nginx virtual hosts for `arshad.com` and `api.arshad.com`.

## Application features
- List tasks
- Add a task
- Mark a task as complete / incomplete
- Delete a task
- Health endpoint: `GET /health`
- API endpoint: `GET /tasks`

## Local run with Docker Compose
1. Copy `.env.example` to `.env`.
2. Change the sample database password.
3. Run:

   ```bash
   docker compose up -d --build
   docker compose ps
   docker compose logs -f
   ```

4. Open `http://localhost:8080`.
5. Test the API: `curl http://localhost:9000/health` and `curl http://localhost:9000/tasks`.

Stop the stack with `docker compose down`. To remove the database volume too (this permanently deletes stored task data), use `docker compose down -v`.

## Deployment notes
- Compose binds frontend and backend to `127.0.0.1` on the host, so they are reachable by host Nginx but not directly exposed to the public internet.
- Host Nginx listens on ports 80/443 and forwards `arshad.com` to `127.0.0.1:8080`, and `api.arshad.com` to `127.0.0.1:9000`.
- Set `API_BASE_URL=https://api.arshad.com` in `.env` before deploying the frontend.
- Backend CORS allows the configured `FRONTEND_ORIGIN`. Set it to `https://arshad.com` in production.
- The database is only on the private Compose network. Its data is stored in the named volume `postgres_data`.
- For production, use a strong unique `DB_PASSWORD`, protect `.env`, configure HTTPS, backups, updates, and SSH/firewall restrictions.

## Important
This is a training sample, not a hardened production application. The credentials in `.env.example` are placeholders. Never commit your real `.env` file.
