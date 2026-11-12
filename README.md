# golang-todoapp

A REST API for users and tasks with task statistics, written in Go. Includes a small web page for working with the API from the browser.

## Stack

Go (net/http) · PostgreSQL (pgx) · golang-migrate · validator · Swagger · zap · Docker Compose

## API

All endpoints are under `/api/v1`. Interactive docs: `/swagger/`.

| Method | Path | Description |
| --- | --- | --- |
| `POST` | `/users` | Create a user |
| `GET` | `/users` | List users |
| `GET` / `PATCH` / `DELETE` | `/users/{id}` | Get, partially update or delete a user |
| `POST` | `/tasks` | Create a task |
| `GET` | `/tasks` | List tasks (`user_id`, `limit`, `offset`) |
| `GET` / `PATCH` / `DELETE` | `/tasks/{id}` | Get, partially update or delete a task |
| `GET` | `/statistics` | Task statistics |

## Architecture

- Code is split by feature (`internal/features/users`, `tasks`, `statistics`, `web`), each with `transport → service → repository` layers; dependencies are wired in `main.go`.
- Middleware: CORS, request ID, logging, tracing, panic recovery.
- `PATCH` updates use optimistic locking by a `version` column.
- Graceful shutdown on `SIGINT` / `SIGTERM`.

## Run

```bash
cp .env.example .env    # fill in the database settings
make env-up             # start PostgreSQL
make migrate-up         # apply migrations
make todoapp-deploy     # build and start the app on :5050
```
