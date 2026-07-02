# First admin user

The app has no public sign-up. Admin and volunteer accounts must exist in the MongoDB `users` collection before anyone can log in.

## Who can log in?

Only users with `role` set to **`admin`** or **`volunteer`** can sign in. Other roles are rejected at login.

| Role | Access |
|------|--------|
| `admin` | Full management — events, repairs, volunteers |
| `volunteer` | Repair-day access |

Create at least one **admin** account before going live.

## User document fields

| Field | Required | Notes |
|-------|----------|-------|
| `first` | Yes | First name |
| `last` | Yes | Last name |
| `email` | Yes | Unique; used as login username |
| `password` | Yes | **bcrypt hash** — not plain text |
| `role` | Yes | `admin` or `volunteer` |
| `image` | No | `{ public_id: "", url: "" }` |
| `resetCode` | No | Default `""` |

Timestamps (`createdAt`, `updatedAt`) are added automatically by Mongoose.

## Method 1: Node script (recommended)

Run this once from the `api/` directory after `npm install`:

```bash
cd api
node -e "
require('dotenv').config();
const mongoose = require('mongoose');
const { hashPassword } = require('./src/helpers/auth-helpers');
const User = require('./src/models/user');

const email = 'admin@yourrepaircafe.org';
const plainPassword = 'CHANGE-ME-strong-password';
const first = 'Admin';
const last = 'User';

(async () => {
  const conn = process.env.NODE_ENV === 'production'
    ? process.env.PRODUCTION_DATABASE_CONNECTION_STRING
    : process.env.DEV_DATABASE_CONNECTION_STRING;
  await mongoose.connect(conn);
  const existing = await User.findOne({ email });
  if (existing) {
    console.log('User already exists:', email);
    process.exit(0);
  }
  const hash = await hashPassword(plainPassword);
  await User.create({ first, last, email, password: hash, role: 'admin' });
  console.log('Created admin user:', email);
  await mongoose.disconnect();
})();
"
```

Edit `email`, `plainPassword`, `first`, and `last` before running. For production, set `NODE_ENV=production` and ensure `PRODUCTION_DATABASE_CONNECTION_STRING` is in `.env`, or run against production from a machine that has those env vars.

## Method 2: MongoDB Atlas UI

1. Open Atlas → **Browse Collections** → your database → `users`
2. Click **Insert Document**
3. Insert a document — but you must supply a **bcrypt hash** for `password`

Generate a hash locally:

```bash
cd api
node -e "require('./src/helpers/auth-helpers').hashPassword('YourPassword123').then(h => console.log(h))"
```

Example document:

```json
{
  "first": "Admin",
  "last": "User",
  "email": "admin@yourrepaircafe.org",
  "password": "$2b$12$....",
  "role": "admin",
  "image": { "public_id": "", "url": "" },
  "resetCode": ""
}
```

<!-- SCREENSHOT: Atlas Insert Document dialog for users collection -->

## Method 3: mongosh

```javascript
use repaircafe
db.users.insertOne({
  first: "Admin",
  last: "User",
  email: "admin@yourrepaircafe.org",
  password: "$2b$12$...",  // bcrypt hash from script above
  role: "admin",
  image: { public_id: "", url: "" },
  resetCode: ""
})
```

## Verify login

1. Open your deployed frontend URL
2. Sign in with the admin email and plain-text password (not the hash)
3. Confirm you reach the main app screens

<!-- SCREENSHOT: Login screen after successful admin sign-in -->

## Adding volunteer logins

Repeat the same process with `role: "volunteer"`. Volunteers use the same login form as admins; the app checks role after authentication.

## Security reminders

- Use a strong password for admin accounts
- Do not store plain-text passwords in MongoDB
- Limit the number of admin accounts
- Change passwords if a volunteer leaves the organization

## Related pages

- [MongoDB Atlas](../deployment/mongodb-atlas.md)
- [Local development](../getting-started/local-development.md)
- [Troubleshooting](./troubleshooting.md)
