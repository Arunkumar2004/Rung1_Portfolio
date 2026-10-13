# Screenshots (to capture later)

No images are in this folder yet. Capture them only from the synthetic reference tenant ("Bharat Traders", generated
data), never from a real customer, and check every image for tokens, keys, email addresses or database names before
adding it.

## Screens to capture

| # | Screen | What it should show |
|---|---|---|
| 1 | Today | The day's work list with real runs from the API, not mock data |
| 2 | New request, then Plan | A plain-language request, the agent's clarifying questions, and the resulting plan |
| 3 | Review | The change for approval: diff, gate results and the approver (who is not the requester) |
| 4 | Gate progress | Gates G1–G8 running against the sandbox, with pass, fail and the reason. Until S10 these pages carry a visible "Demo" label; capture that label if it is present |
| 5 | Credits | Credits and spend, with a refused run shown at zero cost |
| 6 | Agents | Agents, people and API keys, including the "pause all writes" control |

Take each at desktop width (1440 px) and one at phone width (375 px) to show the responsive layout.

## Running the console against the backend

Requirements: Node 20, Python 3.12 with `uv`, PostgreSQL 16, the Temporal CLI, and an ERPNext bench on WSL 2
(Ubuntu 24.04). Follow the project's own `README.md` for the exact commands; in outline:

1. **Backend settings.** Copy the backend's example environment file to a local one and fill in local values. Never
   commit it and never put it in a screenshot.
2. **Databases.** Install the backend dependencies, run the database migrations, and seed the development tenant.
3. **Services.** Start the Temporal dev server, the orchestrator workers, and the ERPNext bench (the reference site).
4. **API.** Start the `write_api` service locally.
5. **Console.** In the front end, point the API address setting at the local API and turn the mock-data switch off,
   then start the Vite dev server. With mocks on (the default without a backend), the console shows sample data only,
   which should not be presented as live.
6. **Sign in** as a seeded development user of the reference tenant and capture the screens above.

To prove a screen is live before capturing it, change a value in the development database and confirm the screen
changes.
