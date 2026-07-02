# Updating a deployment

The API and frontend are separate deployments. Update both when a release touches both codebases.

## Repository layout reminder

```
trc-app/          ← parent repo (docs, submodule pointers)
├── api/          ← trc-api submodule
└── frontend/     ← trc-frontend submodule
```

Changes may live in submodule repos. Push submodule commits first, then update the parent repo's submodule reference if you use it for deployment.

## Standard update workflow

### 1. Pull latest code

```bash
cd trc-app
git pull
git submodule update --remote
```

Or pull directly in each submodule:

```bash
cd api && git pull
cd ../frontend && git pull
```

### 2. Deploy API changes

If `api/` changed:

**Render:** Push to the connected branch — Render auto-deploys. Confirm the deploy succeeds in the dashboard.

**Manual / VPS:**

```bash
cd api
git pull
npm install
# Restart — PM2 on Render restarts automatically; on VPS:
NODE_ENV=production npm run start:prod
```

Verify:

```bash
curl https://YOUR-API/api/
```

### 3. Deploy frontend changes

If `frontend/` changed:

**Render Static Site:** Push to the connected branch. Render rebuilds with env vars from the dashboard.

**Manual:**

```bash
cd frontend
git pull
npm install
npm run build:web
# Upload dist/ to your static host
```

### 4. Smoke test

- Log in as admin
- Open an event
- Add a test volunteer or repair
- Delete test data if desired

## When to update only one service

| Change location | Redeploy |
|-----------------|----------|
| API controllers, models, routes | API only |
| React components, requests, UI | Frontend only |
| `API_URL` or frontend env vars | Frontend rebuild required |
| Database connection or JWT settings | API only |
| Both submodules in same feature | Both |

## PM2 on Render

The API uses `pm2-runtime` in production. Pushing code triggers a new Render deploy, which replaces the running process. If you ever run PM2 manually on a VPS, restart after pulling:

```bash
pm2 restart all
```

## Submodule pointer updates

If you deploy from the monorepo and only updated a submodule:

```bash
cd trc-app
cd api && git pull && cd ..
git add api
git commit -m "Update api submodule"
git push
```

Repeat for `frontend` when needed.

## Related pages

- [Deploy the API](../deployment/deploy-api.md)
- [Deploy the frontend](../deployment/deploy-frontend.md)
- [Troubleshooting](./troubleshooting.md)
