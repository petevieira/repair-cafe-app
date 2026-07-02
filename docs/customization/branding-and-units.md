# Branding and units

Customize display settings and app identity for your repair cafe.

## Currency and weight units

The app shows cost and weight fields with labels driven by environment variables.

Set the **same values** on API and frontend:

| Variable | Examples | Used for |
|----------|----------|----------|
| `COST_UNITS` | `usd`, `euros`, `cad` | Repair cost field label |
| `WEIGHT_UNITS` | `lbs`, `kg` | Item weight field label |

**API:** set in `.env` or hosting platform env vars.

**Frontend:** set in `.env` and **rebuild** (`npm run build:web`).

Defaults in code (if unset): `COST_UNITS=euros`, `WEIGHT_UNITS=kg`.

---

## App name and icons

Expo configuration lives in `frontend/app.json`:

```json
{
  "expo": {
    "name": "TRC App",
    "slug": "TRC App",
    "web": {
      "favicon": "./favicon.png"
    }
  }
}
```

To rebrand:

1. Change `expo.name` to your cafe's name
2. Replace `frontend/favicon.png` and `frontend/favicon.ico`
3. Replace splash/icon assets under `frontend/assets/` as needed
4. Rebuild the web app

<!-- SCREENSHOT: Browser tab showing custom favicon and title -->

---

## Product categories and repair areas

Repair categories and groupings are defined in `frontend/src/globals/ords.tsx`. To add or rename categories for your locale:

1. Edit the category lists in that file
2. Rebuild and redeploy the frontend

No API change is required unless you add server-side validation for new categories.

---

## Terms of service / waiver text

Waiver and terms content is stored in the MongoDB `texts` collection and managed through the app by admins after login. You do not need to edit source code for initial waiver text — configure it in the running app.

---

## Custom domain

| Service | Typical hostname |
|---------|------------------|
| Frontend | `app.yourrepaircafe.org` |
| API | `api.yourrepaircafe.org` |

Configure DNS at your registrar and add custom domains in Render (or your host). Update `API_URL` and rebuild the frontend when the API hostname changes.

---

## Forking for your organization

This project uses git submodules for `api` and `frontend`. Repair cafes can:

1. Fork or clone the repos under their own GitHub organization
2. Point Render deploys at their forks
3. Customize branding in `app.json` and assets
4. Keep pulling upstream updates via git merge/rebase

Document your fork-specific env vars and domains in an internal runbook.

## Related pages

- [Environment variables](../deployment/environment-variables.md)
- [Deploy the frontend](../deployment/deploy-frontend.md)
