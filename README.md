# QueueSeva

QueueSeva is a government queue management **demonstration**. It is not an official government service and does not connect to government systems. Every office, statistic and queue shown is sample data.

## Run locally (Windows PowerShell)

```powershell
cd "C:\Users\piyus\OneDrive\Desktop\Queue Managment"
npm.cmd install
Copy-Item .env.example .env
# Edit .env and set SESSION_SECRET, ADMIN_EMAIL and ADMIN_PASSWORD
npm.cmd start
```

Open <http://localhost:3000>. For local HTTP, keep `COOKIE_SECURE=false`. Use a long random `SESSION_SECRET` and set `COOKIE_SECURE=true` behind HTTPS in deployment. The demo admin is configured from `ADMIN_NAME`, `ADMIN_EMAIL`, and `ADMIN_PASSWORD`; no administrator account can be registered through public signup. Admin credentials are bcrypt-hashed in the JSON user file on server startup.

## Features

- Citizen registration, profile and password updates, login/logout, a dedicated administrator login, role-based admin access, HttpOnly SameSite cookies, session regeneration, and login/signup rate limiting.
- Browse, search and category-filter five labelled sample offices. Citizens can reserve one active token per service, view booking history and cancel waiting tokens.
- Reception queue for call-next, status transitions and counter availability, plus service-hours open/close controls.
- JSON files in `data/` persist users, services, counters, token state and daily token counters. Passwords are bcrypt hashes; session IDs are held in server-side sessions and never in browser storage.

## Queue estimate

FIFO is maintained independently per service. The waiting estimate is `ceil(max(0, peopleAhead + 1 - freeSlots) / availableCounters) × configured average service duration`, where `freeSlots = max(0, availableCounters - active called/serving tokens)`. If no counters are available, the queue is paused and no wait estimate is returned. Real service times vary, so the display remains approximate and changes as tokens advance. A counter is reserved when a token is called, and only one token can be reserved per counter.

## Limitations

This small demo serializes JSON file writes within one Node process. JSON persistence is not safe for multiple server instances, concurrent processes, high traffic, or production reliability; process-local sessions also expire on restart. Use a proper database and durable session store before any real deployment. Password recovery only returns a generic response: email delivery and reset links are not configured. External Bootstrap assets require internet access. No real government service integration exists.
