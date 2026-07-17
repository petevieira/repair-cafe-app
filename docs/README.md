# Repair Cafe Management App — Documentation

Welcome to the deployment and operations guide for the Repair Cafe Management App. This documentation is written for repair cafe organizers who want to run their own instance on their own servers and database.

## Quick links

| Goal                                 | Start here                                                     |
| ------------------------------------ | -------------------------------------------------------------- |
| Understand what you're deploying     | [Architecture overview](./overview/architecture.md)            |
| Run locally for testing              | [Local development](./getting-started/local-development.md)    |
| Deploy to production                 | [Deployment overview](./deployment/overview.md)                |
| Create your first admin login        | [First admin user](./operations/first-admin-user.md)           |
| Configure API keys and database URLs | [Environment variables](./deployment/environment-variables.md) |

## Documentation map

### Overview

- [Architecture](./overview/architecture.md) — how the API, frontend, and database fit together
- [Features](./overview/features.md) — what the app does today

### Getting started

- [Prerequisites](./getting-started/prerequisites.md) — accounts, tools, and Node versions
- [Local development](./getting-started/local-development.md) — clone, configure, and run locally

### Deployment

- [Deployment overview](./deployment/overview.md) — recommended paths and checklist
- [MongoDB Atlas](./deployment/mongodb-atlas.md) — create a database cluster
- [Deploy the API](./deployment/deploy-api.md) — Render, Docker, or your own VPS
- [Deploy the frontend](./deployment/deploy-frontend.md) — build and host the web app
- [Environment variables](./deployment/environment-variables.md) — full reference for API and frontend

### Operations

- [First admin user](./operations/first-admin-user.md) — create login accounts
- [Updating a deployment](./operations/updating.md) — deploy API and frontend changes
- [Backups](./operations/backups.md) — export and restore MongoDB data
- [Troubleshooting](./operations/troubleshooting.md) — common problems and fixes

### Customization

- [Branding and units](./customization/branding-and-units.md) — currency, weight units, and app identity

## Screenshots

Several pages include `<!-- SCREENSHOT: ... -->` placeholders where you can add images later. Suggested captures:

1. MongoDB Atlas — cluster creation, database user, network access, connection string
2. Render — new web service, environment variables, deploy logs
3. Frontend static site — build output and publish settings
4. App login screen and admin dashboard after first deploy

## Repository structure

This project is a **monorepo with git submodules**:

```
repair-cafe-app/
├── api/          → repair-cafe-app-api submodule (Node/Express API)
├── frontend/     → repair-cafe-app-frontend submodule (Expo/React Native web app)
└── docs/         → this documentation
```

When you deploy, the **API and frontend are separate services** that must both be updated when you release changes.
