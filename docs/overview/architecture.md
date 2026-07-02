# Architecture

The Repair Cafe Management App has three main parts:

```
┌─────────────────┐       HTTPS/JSON        ┌─────────────────┐
│   Web browser   │  ────────────────────►  │   API server    │
│   (Expo web)    │  ◄────────────────────  │   (Node/Express)│
└─────────────────┘                         └────────┬────────┘
                                                     │
                                                     │ MongoDB driver
                                                     ▼
                                            ┌─────────────────┐
                                            │  MongoDB Atlas  │
                                            │  (or self-host) │
                                            └─────────────────┘
```

## Components

### Frontend (`frontend/`)

- Built with **Expo** and **React Native**, targeting **web** for repair-day use on laptops, phones, and tablets
- Talks to the API over HTTPS using the `API_URL` environment variable
- Built as static files with `npm run build:web` (output in `dist/`)
- Requires **Node 20.11.1**

### API (`api/`)

- **Node.js / Express** REST API
- Routes are mounted under `/api/*` (e.g. `/api/users/sign-in`, `/api/repairs`)
- Uses **MongoDB** via Mongoose for all persistent data
- Authentication uses **JWT** tokens (signed with `JWT_SECRET`)
- Requires **Node 22.14.0**
- Production start command uses **PM2** (`npm run start:prod`)

### Database

- **MongoDB** is required — [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) free tier works well for small repair cafes
- The API selects its connection string based on `NODE_ENV`:
  - `development` → `DEV_DATABASE_CONNECTION_STRING`
  - `staging` → `STAGING_DATABASE_CONNECTION_STRING`
  - `production` → `PRODUCTION_DATABASE_CONNECTION_STRING`

## Deployment model

| Service | Typical host | Notes |
|---------|--------------|-------|
| API | Render web service, VPS, or Docker | Must expose HTTPS; set `PORT` if host provides it |
| Frontend | Render static site, Netlify, S3+CloudFront, nginx | Static files from `build:web` |
| Database | MongoDB Atlas | Allow API server IP (or `0.0.0.0/0` for cloud hosts) |

**Important:** The API and frontend deploy independently. Pushing API changes does not update the frontend bundle, and vice versa. See [Updating a deployment](../operations/updating.md).

## Authentication roles

Users stored in MongoDB have a `role` field:

| Role | Can sign in? | Typical use |
|------|--------------|-------------|
| `admin` | Yes | Full access — manage events, repairs, volunteers |
| `volunteer` | Yes | Repair-day access |
| `user` | No | Reserved; not used for app login today. Can see latest event's repair list. |

Admin and volunteer accounts are created manually in the database — there is no public sign-up flow. See [First admin user](../operations/first-admin-user.md).

## Optional services

- **SendGrid** — referenced in config for email; not required for core repair-day workflows
- **Swagger API docs** — available at `/docs` when `NODE_ENV` is not `production`

## Related pages

- [Prerequisites](../getting-started/prerequisites.md)
- [Environment variables](../deployment/environment-variables.md)
- [Deployment overview](../deployment/overview.md)
