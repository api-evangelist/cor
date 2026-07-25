---
name: cor-manage-project
description: Create a project in COR, add tasks and collaborators, and post an update message.
api: COR API v1
base_url: https://api.projectcor.com/v1
method: generated
generated: '2026-07-18'
source: openapi/cor-openapi.json
operations:
  - "POST /oauth/token"
  - "GET /clients"
  - "POST /projects"
  - "GET /projects/{project_id}"
  - "POST /tasks"
  - "POST /projects/{project_id}/collaborators"
  - "POST /projects/{id}/messages"
---

# Set up and manage a COR project

Stand up a new project, staff it, and communicate on it.

## Steps

1. **Authenticate.** `POST /oauth/token` (client_credentials) for a JWT; send it as
   `Authorization: Bearer <token>`.
2. **Pick the client.** `GET /clients` (paginated) to find the client the project
   belongs to.
3. **Create the project.** `POST /projects` with the client reference and project
   details.
4. **Confirm.** `GET /projects/{project_id}` to read back the created project.
5. **Add work.** `POST /tasks` to create tasks under the project.
6. **Staff it.** `POST /projects/{project_id}/collaborators` to add collaborators.
7. **Communicate.** `POST /projects/{id}/messages` to post an update (supports
   @mentions).

## Conventions & errors

- Pagination: `page` / `perPage`, default 20; filter list endpoints with `filters`.
- Errors use the `CORCustomError` envelope (`status`/`name`/`code`/`message`); see
  `errors/cor-problem-types.yml`. There is no Idempotency-Key header, so guard
  against duplicate `POST /projects` retries client-side.
