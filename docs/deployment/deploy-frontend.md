# Deploy the frontend

The frontend is an Expo web app compiled to static files. Deploy those files to any static host (Render Static Site, Netlify, S3, nginx, etc.).

## Build the production bundle

Environment variables are embedded at **build time** via `react-native-dotenv`. Set them before building.

```bash
cd frontend
cp env.default .env
```

Edit `.env` for production:

```env
API_URL="https://YOUR-API-HOST.onrender.com/api"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
DEBUG="false"
```

Build:

```bash
nvm use 20.11.1
npm install
npm run build:web
```

Output is written to **`frontend/dist/`**.

Verify locally before deploying:

```bash
npx serve dist
```

Open the URL shown and confirm the login page loads and API calls reach your production API (check browser dev tools → Network).

<!-- SCREENSHOT: Browser Network tab showing API calls to production URL -->

## Render Static Site

1. **New** → **Static Site**
2. Connect the **`trc-frontend`** repo (or monorepo with root directory `frontend`)
3. Configure:

| Setting | Value |
|---------|-------|
| **Build command** | `npm install && npm run build:web` |
| **Publish directory** | `dist` |

4. Add **environment variables** in Render (same as `.env` above) — Render injects them during the build
5. Deploy

<!-- SCREENSHOT: Render static site build settings -->

### Custom domain

In Render → Static Site → **Settings** → **Custom Domains**, add e.g. `app.yourrepaircafe.org` and configure DNS per Render's instructions.

## Other static hosts

| Host | Publish directory | Notes |
|------|-------------------|-------|
| **Netlify** | `dist` | Set env vars in Netlify UI; build cmd `npm run build:web` |
| **Cloudflare Pages** | `dist` | Add `API_URL` as build env var |
| **nginx** | copy `dist/*` to `/var/www/html` | Simple; manage TLS with certbot |

Example nginx config for SPA routing (Expo web uses client-side routing):

```nginx
server {
    listen 443 ssl;
    server_name app.your-cafe.org;
    root /var/www/trc-frontend/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## CORS

The API enables CORS for all origins (`app.use(cors())` in `api/src/app.js`). No extra frontend configuration is needed for cross-origin API calls from your static site domain.

## Rebuild when API URL changes

If you move the API to a new host or custom domain, you **must rebuild and redeploy** the frontend. Changing API env vars on the API host alone is not enough.

## Mobile builds (optional)

The project supports Expo dev builds for iOS/Android (`npm run ios`, `npm run android`). Production mobile distribution is not covered in this guide; the web build is the supported deployment path for repair events.

## Related pages

- [Deploy the API](./deploy-api.md)
- [Environment variables](./environment-variables.md)
- [Updating a deployment](../operations/updating.md)
