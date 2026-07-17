# Local development

Run the full stack on your machine before deploying to production.

## 1. Clone the repository

```bash
git clone git@github.com:petevieira/repair-cafe-app.git
cd repair-cafe-app
git submodule init
git submodule update
```

If you use HTTPS instead of SSH, adjust the clone URL accordingly. Submodule URLs are defined in `.gitmodules`:

- `api` → `repair-cafe-app-api`
- `frontend` → `repair-cafe-app-frontend`

## 2. Set up MongoDB

Create a free MongoDB Atlas cluster (see [MongoDB Atlas](../deployment/mongodb-atlas.md)) or use a local MongoDB instance.

For development, you only need one database. Copy the connection string for use in the API `.env` file.

## 3. Configure the API

```bash
cd api
cp env.default .env
```

Edit `.env`. At minimum for local dev:

```env
NODE_ENV="development"
DEV_DATABASE_CONNECTION_STRING="mongodb+srv://USER:PASSWORD@cluster.../your-db-name"
JWT_SECRET="use-a-long-random-string-at-least-32-characters"
JWT_EXPIRATION="6d"
JWT_ISSUER="trc-local"
JWT_AUDIENCE="trc-local"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
```

> **Note:** `database-config.js` uses `DEV_DATABASE_CONNECTION_STRING` when `NODE_ENV=development`. The older `DATABASE_CONNECTION_STRING` name in `env.default` is not used — use the names in [Environment variables](../deployment/environment-variables.md).

Install dependencies and start the API:

```bash
nvm use 22.14.0   # or install Node 22.14.0
npm install
npm run start:dev
```

The API listens on **http://localhost:3000**. Test it:

```bash
curl http://localhost:3000/api/
```

You should get a JSON health response.

In non-production mode, Swagger docs are at **http://localhost:3000/docs**.

## 4. Configure the frontend

Open a second terminal:

```bash
cd frontend
cp env.default .env
```

Edit `.env`:

```env
API_URL="http://localhost:3000/api"
COST_UNITS="usd"
WEIGHT_UNITS="lbs"
DEBUG="false"
```

Install dependencies and start the web app:

```bash
nvm use 20.11.1
npm install
npm run web
```

Expo opens the app in your browser (typically **http://localhost:8081**).

## 5. Create a test admin user

You cannot log in until at least one user exists in MongoDB. See [First admin user](../operations/first-admin-user.md) for step-by-step instructions.

## Node version switching

The API and frontend require different Node versions. When switching directories:

```bash
# In api/
nvm use 22.14.0

# In frontend/
nvm use 20.11.1
```

You can add `.nvmrc` files in each folder to automate this (`nvm use` with no version reads `.nvmrc`).

## Related pages

- [Environment variables](../deployment/environment-variables.md)
- [First admin user](../operations/first-admin-user.md)
- [Troubleshooting](../operations/troubleshooting.md)
