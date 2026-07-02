# Environment variables

Complete reference for API and frontend configuration.

## API (`api/.env`)

Copy from `api/env.default` and adjust. The API loads variables via `dotenv` at startup.

### Required

| Variable | When | Description |
|----------|------|-------------|
| `NODE_ENV` | Always | `development`, `staging`, or `production` — selects database connection |
| `DEV_DATABASE_CONNECTION_STRING` | `NODE_ENV=development` | MongoDB URI for local dev |
| `STAGING_DATABASE_CONNECTION_STRING` | `NODE_ENV=staging` | MongoDB URI for staging |
| `PRODUCTION_DATABASE_CONNECTION_STRING` | `NODE_ENV=production` | MongoDB URI for production |
| `JWT_SECRET` | Always | Secret for signing JWTs — use 32+ random characters |
| `JWT_EXPIRATION` | Always | Token lifetime, e.g. `6d`, `24h` |
| `JWT_ISSUER` | Always | JWT issuer claim (any string identifying your cafe) |
| `JWT_AUDIENCE` | Always | JWT audience claim |

### Optional

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `3000` | HTTP listen port (set by Render automatically) |
| `COST_UNITS` | `euros` in code | Display label for cost fields — e.g. `usd`, `euros` |
| `WEIGHT_UNITS` | `kg` in code | Display label for weight — e.g. `lbs`, `kg` |
| `SENDGRID_KEY` | — | SendGrid API key (email features) |
| `EMAIL_FROM` | — | From address for SendGrid emails |
| `DB_LISTEN_PORT` | — | Legacy; server uses `PORT` |

### Database selection logic

From `api/src/database/database-config.js`:

```
NODE_ENV=production  → PRODUCTION_DATABASE_CONNECTION_STRING
NODE_ENV=staging     → STAGING_DATABASE_CONNECTION_STRING
NODE_ENV=development → DEV_DATABASE_CONNECTION_STRING
```

Invalid `NODE_ENV` values cause the server to exit on startup.

### Example — local development

```env
NODE_ENV="development"
DEV_DATABASE_CONNECTION_STRING="mongodb+srv://user:pass@cluster0.xxxxx.mongodb.net/repaircafe-dev?retryWrites=true&w=majority"
JWT_SECRET="change-me-to-a-long-random-string"
JWT_EXPIRATION="6d"
JWT_ISSUER="my-cafe-dev"
JWT_AUDIENCE="my-cafe-dev"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
```

### Example — production (Render)

```env
NODE_ENV=production
PRODUCTION_DATABASE_CONNECTION_STRING=mongodb+srv://...
JWT_SECRET=...
JWT_EXPIRATION=6d
JWT_ISSUER=my-repair-cafe
JWT_AUDIENCE=my-repair-cafe
COST_UNITS=usd
WEIGHT_UNITS=lbs
```

> **Outdated name in env.default:** `DATABASE_CONNECTION_STRING` in `env.default` is not read by the app. Use the `DEV_` / `STAGING_` / `PRODUCTION_` prefixed variables above.

---

## Frontend (`frontend/.env`)

Copy from `frontend/env.default`. Variables are imported as `@env` and baked in at build time.

### Required

| Variable | Description |
|----------|-------------|
| `API_URL` | Full base URL to API routes, **including `/api`** — e.g. `https://api.example.org/api` |

### Optional

| Variable | Default | Description |
|----------|---------|-------------|
| `COST_UNITS` | — | Must match API display preference |
| `WEIGHT_UNITS` | — | Must match API display preference |
| `DEBUG` | `false` | If `true`, auto-login using test credentials (dev only) |
| `TEST_EMAIL` | — | Used when `DEBUG=true` |
| `TEST_PASSWORD` | — | Used when `DEBUG=true` |

Never set `DEBUG=true` in production builds.

### Example — local

```env
API_URL="http://localhost:3000/api"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
DEBUG="false"
```

### Example — production

```env
API_URL="https://my-cafe-api.onrender.com/api"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
DEBUG="false"
```

---

## Generating a JWT secret

```bash
openssl rand -base64 32
```

Store the result in `JWT_SECRET` on your API host only — never in the frontend.

## Related pages

- [Local development](../getting-started/local-development.md)
- [Deploy the API](./deploy-api.md)
- [Deploy the frontend](./deploy-frontend.md)
