# Troubleshooting

Common issues when deploying and running the Repair Cafe Management App.

## API won't start

### `Invalid NODE_ENV`

**Symptom:** Server exits immediately with `Invalid NODE_ENV ...`

**Fix:** Set `NODE_ENV` to exactly `development`, `staging`, or `production`.

### Database connection fails

**Symptom:** MongoDB connection timeout or authentication error in logs

**Checklist:**
- Connection string username/password are correct
- Special characters in password are URL-encoded (`@` → `%40`)
- Database name is included in the URI path
- Atlas **Network Access** allows your API server's IP (or `0.0.0.0/0`)
- Correct env var for your mode: `DEV_` / `STAGING_` / `PRODUCTION_DATABASE_CONNECTION_STRING`

### Wrong database variable name

**Symptom:** `undefined` connection or connecting to wrong database

**Fix:** The app does **not** read `DATABASE_CONNECTION_STRING` from `env.default`. Use the prefixed variables documented in [Environment variables](../deployment/environment-variables.md).

---

## Frontend can't reach API

### Login fails / network errors

**Checklist:**
- `API_URL` in frontend `.env` includes `/api` suffix
- `API_URL` uses `https://` in production (not `http://`)
- Frontend was **rebuilt** after changing `API_URL`
- API health check works: `curl https://YOUR-API/api/`

### CORS errors

The API allows all origins via `cors()`. If you still see CORS errors, verify the browser is calling the correct API URL and that the API is actually running.

---

## Deployed fix not visible

### API change works locally but not production

- Confirm you pushed to the branch Render deploys from
- Check Render deploy logs for success
- Render hobby tier may spin down — first request can be slow (cold start)

### Frontend change not visible

**Most common cause:** Frontend was not rebuilt/redeployed.

The API and frontend deploy separately. Pushing API code does not update the JavaScript bundle users download. Trigger a new static site build on Render (or re-run `npm run build:web` and upload `dist/`).

Hard-refresh the browser (Ctrl+Shift+R / Cmd+Shift+R) or clear cache after frontend deploy.

---

## Login issues

### "User not found" or "Wrong password"

- Confirm the user exists in the `users` collection
- Email must match exactly (case-sensitive in MongoDB query)
- Password in database must be a **bcrypt hash**, not plain text

See [First admin user](./first-admin-user.md).

### "User not admin" / unauthorized

- User's `role` must be `admin` or `volunteer`
- Role `user` (default) cannot sign in

---

## Wrong units displayed (lbs vs kg, usd vs euros)

Set matching values on **both** API and frontend:

```env
COST_UNITS=usd
WEIGHT_UNITS=lbs
```

Rebuild the frontend after changing its env vars.

---

## Node version errors

| Directory | Required Node |
|-----------|---------------|
| `api/` | 22.14.0 |
| `frontend/` | 20.11.1 |

Use `nvm use` in each directory. Render should use the version in each package's `engines` field.

---

## Docker image fails

The bundled `api/Dockerfile` uses Node 16 — older than the project's Node 22 requirement. Update the `FROM` line to `node:22.14.0` before building for production.

---

## Getting help

When asking for support, include:

1. API deploy logs (startup and error lines)
2. Browser console / Network tab errors
3. `NODE_ENV` and whether health check `GET /api/` succeeds
4. Whether the issue is API-only, frontend-only, or both

## Related pages

- [Environment variables](../deployment/environment-variables.md)
- [Updating a deployment](./updating.md)
- [First admin user](./first-admin-user.md)
