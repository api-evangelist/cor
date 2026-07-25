---
name: cor-log-time
description: Log and review time entries (hours) for a user in COR, the agency management platform.
api: COR API v1
base_url: https://api.projectcor.com/v1
method: generated
generated: '2026-07-18'
source: openapi/cor-openapi.json
operations:
  - "POST /oauth/token"
  - "GET /me"
  - "GET /projects"
  - "GET /tasks"
  - "POST /hours"
  - "GET /hours"
  - "GET /hours/by-day/{datetime}"
---

# Log time in COR

Record and review tracked hours against projects and tasks.

## Steps

1. **Authenticate.** `POST /oauth/token` with the `client_credentials` grant to
   obtain a JWT `access_token`. Send it as `Authorization: Bearer <token>` on every
   request. (See `authentication/cor-authentication.yml`.)
2. **Resolve the user.** `GET /me` to confirm the authenticated user and workspace.
3. **Find the target work.** `GET /projects` and `GET /tasks` (both paginated with
   `page`/`perPage`, default 20) to locate the project/task the time belongs to.
   Use the `filters` query parameter to narrow results.
4. **Log the hours.** `POST /hours` with the task/project reference, date, and
   duration.
5. **Verify.** `GET /hours` or `GET /hours/by-day/{datetime}` to confirm the entry
   was recorded.

## Conventions & errors

- Pagination: `page` / `perPage`, default 20 per page.
- On failure COR returns the `CORCustomError` envelope (`status`, `name`, `code`,
  `message`) — see `errors/cor-problem-types.yml`. Handle `401` (expired token) by
  refreshing, and honor rate-limit responses.
