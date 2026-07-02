# MongoDB Atlas setup

MongoDB Atlas provides a managed MongoDB cluster. The free **M0** tier is sufficient for most repair cafes starting out.

## 1. Create an Atlas account and project

1. Go to [https://www.mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register)
2. Create an organization and project for your repair cafe

<!-- SCREENSHOT: Atlas dashboard after login -->

## 2. Create a cluster

1. Click **Build a Database**
2. Choose **M0 Free** (Shared)
3. Pick a cloud provider and region close to your API server (e.g. AWS `us-west-2` if API is on Render US West)
4. Name the cluster (default `Cluster0` is fine)
5. Click **Create**

Cluster provisioning takes a few minutes.

<!-- SCREENSHOT: Cluster tier selection (M0 Free) -->

## 3. Create a database user

1. In the left sidebar, go to **Database Access**
2. Click **Add New Database User**
3. Choose **Password** authentication
4. Set a username and strong password — save these securely
5. Under **Database User Privileges**, choose **Read and write to any database** (or restrict to your database name)
6. Click **Add User**

<!-- SCREENSHOT: Database Access — new user form -->

## 4. Configure network access

The API server must be allowed to connect to Atlas.

1. Go to **Network Access**
2. Click **Add IP Address**
3. For cloud hosts (Render, Railway, etc.) where the IP may change, add **Allow Access from Anywhere** (`0.0.0.0/0`)
4. For a fixed VPS, add that server's public IP only

> For production, prefer IP allowlisting when your host provides a stable egress IP. Cloud PaaS often requires `0.0.0.0/0`.

<!-- SCREENSHOT: Network Access — IP allowlist -->

## 5. Get the connection string

1. Go to **Database** → **Connect** on your cluster
2. Choose **Connect your application**
3. Driver: **Node.js**, version 4.1 or later
4. Copy the connection string:

```
mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
```

5. Replace `<username>` and `<password>` with your database user credentials
6. Add your database name before the `?`:

```
mongodb+srv://myuser:myP%40ss@cluster0.xxxxx.mongodb.net/repaircafe?retryWrites=true&w=majority
```

URL-encode special characters in passwords (e.g. `@` → `%40`).

Use this string as:

- `DEV_DATABASE_CONNECTION_STRING` for local development
- `PRODUCTION_DATABASE_CONNECTION_STRING` for production (`NODE_ENV=production`)

## 6. Verify connectivity

After deploying the API, check logs for:

```
Connecting in production mode to real MongoDB cluster...
App is running in production mode and listening on port ...
```

If connection fails, double-check username/password encoding, network access, and that the connection string includes the database name.

## Collections

The app creates collections automatically when data is first written. You do not need to create schemas manually in Atlas. Main collections include:

- `users` — admin and volunteer login accounts
- `volunteers` — volunteers registered for events
- `repairs` — repair records
- `repairevents` — repair cafe events

## Related pages

- [Environment variables](./environment-variables.md)
- [First admin user](../operations/first-admin-user.md)
- [Backups](../operations/backups.md)
