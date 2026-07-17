# Repair Cafe Management App

A web app for repair cafes to manage events, log repairs, and track volunteers. Built with Expo (React Native web) and a Node/Express API backed by MongoDB.

**→ [Full documentation](./docs/README.md)** — deployment, configuration, operations, and customization for other repair cafes hosting their own instance.

---

## Quick start (local)

```bash
git clone git@github.com:petevieira/repair-cafe-app.git
cd repair-cafe-app
git submodule init && git submodule update

# API
cd api && cp env.default .env && npm install && npm run start:dev

# Frontend — separate terminal
cd frontend && cp env.default .env && npm install && npm run web
```

See [Local development](./docs/getting-started/local-development.md) for environment variable details and [First admin user](./docs/operations/first-admin-user.md) before logging in.

---

## Repository structure

| Path        | Description                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| `api/`      | Node/Express REST API ([repair-cafe-app-api](https://github.com/petevieira/repair-cafe-app-api) submodule)  |
| `frontend/` | Expo web app ([repair-cafe-app-frontend](https://github.com/petevieira/repair-cafe-app-frontend) submodule) |
| `docs/`     | Deployment and operations guide                                                                             |

The API and frontend deploy as **separate services**. Update both when releasing changes that touch both codebases.

---

## Features

- Admin and volunteer login (JWT)
- Repair event management
- Repair logging with status, repairer assignment, and follow-ups
- Volunteer registration and reuse from past events
- Configurable cost/weight units (USD/lbs, EUR/kg, etc.)
- Terms of service / waiver tracking

---

## Deployment (summary)

Typical production stack:

| Component | Suggested host                                                      |
| --------- | ------------------------------------------------------------------- |
| Database  | [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) (free M0 tier) |
| API       | [Render](https://render.com) web service                            |
| Frontend  | Render static site (or any static host)                             |

Step-by-step instructions: **[Deployment overview](./docs/deployment/overview.md)**

---

## Documentation index

- [Architecture](./docs/overview/architecture.md)
- [Prerequisites](./docs/getting-started/prerequisites.md)
- [MongoDB Atlas setup](./docs/deployment/mongodb-atlas.md)
- [Deploy the API](./docs/deployment/deploy-api.md)
- [Deploy the frontend](./docs/deployment/deploy-frontend.md)
- [Environment variables](./docs/deployment/environment-variables.md)
- [Updating & troubleshooting](./docs/operations/updating.md)

---

## Contributing

Fork the repository, make changes in the relevant submodule (`api/` or `frontend/`), and open a pull request. See [Updating a deployment](./docs/operations/updating.md) for how API and frontend releases relate.

---

## License

MIT License (see LICENSE file if present).
