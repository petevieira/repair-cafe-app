# Deploy the API

The API is a Node.js Express server. This page covers Render (recommended), Docker, and general VPS deployment.

## Render (recommended)

### Connect the repository

The API lives in the **`trc-api`** submodule. You can deploy either:

- The **`trc-api`** repository directly (simplest), or
- The parent **`trc-app`** repo with root directory set to `api`

1. Log in to [Render](https://render.com)
2. Click **New** → **Web Service**
3. Connect your Git provider and select the API repository
4. Configure the service:

| Setting | Value |
|---------|-------|
| **Name** | e.g. `my-cafe-api` |
| **Region** | Same region as MongoDB when possible |
| **Branch** | `main` (or your production branch) |
| **Root directory** | `api` (if deploying from monorepo) |
| **Runtime** | Node |
| **Build command** | `npm install` |
| **Start command** | `npm run start:prod` |

<!-- SCREENSHOT: Render new web service settings -->

### Environment variables

In Render → your service → **Environment**, add:

| Variable | Example |
|----------|---------|
| `NODE_ENV` | `production` |
| `PRODUCTION_DATABASE_CONNECTION_STRING` | `mongodb+srv://...` |
| `JWT_SECRET` | long random string |
| `JWT_EXPIRATION` | `6d` |
| `JWT_ISSUER` | `my-repair-cafe` |
| `JWT_AUDIENCE` | `my-repair-cafe` |
| `COST_UNITS` | `usd` |
| `WEIGHT_UNITS` | `lbs` |

Render injects `PORT` automatically — the server reads `process.env.PORT`.

<!-- SCREENSHOT: Render environment variables panel -->

### Deploy and verify

1. Click **Create Web Service**
2. Wait for the deploy to finish
3. Test the health endpoint:

```bash
curl https://YOUR-SERVICE.onrender.com/api/
```

You should receive a JSON response. Note your API base URL — the frontend needs `https://YOUR-SERVICE.onrender.com/api`.

### PM2 note

Production uses PM2 via `npm run start:prod`. Render does **not** auto-reload when you push code unless a new deploy is triggered. After each git push, confirm the deploy completed in the Render dashboard.

## Docker

A Dockerfile exists at `api/Dockerfile`:

```dockerfile
FROM node:16.20.0   # ← update to node:22 before production use
WORKDIR /app
COPY package*.json ./
EXPOSE 3000
RUN npm install pm2 -g
RUN npm install
COPY src ./
COPY .env ./
CMD ["npm", "run", "start:prod"]
```

Build and run locally:

```bash
cd api
docker build -t trc-api .
docker run --interactive --tty --publish 3000:3000 --env-file .env trc-api
```

For production:

1. Update the base image to `node:22.14.0`
2. Pass environment variables via `--env-file` or orchestrator secrets (do not bake secrets into the image)
3. Place the container behind a reverse proxy with HTTPS (nginx, Caddy, Traefik)

## VPS (PM2 + nginx)

On a Linux server with Node 22 installed:

```bash
cd /var/www/trc-api
git pull
npm install
```

Create `/etc/systemd/system/trc-api.service` or use PM2 directly:

```bash
NODE_ENV=production npm run start:prod
```

Example nginx reverse proxy:

```nginx
server {
    listen 443 ssl;
    server_name api.your-cafe.org;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Use Let's Encrypt (certbot) for TLS certificates.

## Health check

| Endpoint | Expected |
|----------|----------|
| `GET /api/` | JSON success message |

Swagger docs (`/docs`) are **disabled** when `NODE_ENV=production`.

## Related pages

- [MongoDB Atlas](./mongodb-atlas.md)
- [Deploy the frontend](./deploy-frontend.md)
- [Environment variables](./environment-variables.md)
- [Troubleshooting](../operations/troubleshooting.md)
