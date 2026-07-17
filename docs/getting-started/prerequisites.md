# Prerequisites

Before you deploy the app, gather the following.

## Accounts

| Service                                              | Required?   | Purpose                                              |
| ---------------------------------------------------- | ----------- | ---------------------------------------------------- |
| [GitHub](https://github.com)                         | Yes         | Clone the repository and submodules                  |
| [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) | Yes         | Hosted MongoDB database                              |
| [Render.com](https://render.com)                     | Recommended | Easy API + static site hosting (free tier available) |
| Domain registrar                                     | Optional    | Custom domain for your repair cafe                   |

SendGrid (email) is optional and not required for core repair-day use.

## Local development tools

| Tool        | Version                           | Used for                                            |
| ----------- | --------------------------------- | --------------------------------------------------- |
| **Git**     | recent                            | Clone repo and submodules                           |
| **Node.js** | 22.14.0 (API), 20.11.1 (frontend) | Run API and frontend locally                        |
| **npm**     | comes with Node                   | Install dependencies                                |
| **nvm**     | optional but recommended          | Switch Node versions between `api/` and `frontend/` |

Install nvm: [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

## Production hosting options

You do not have to use Render. Any host that can run Node.js (API) and serve static files (frontend) works:

- **Render** — web service + static site (documented in this guide)
- **Docker** — use the included `api/Dockerfile` (update Node version if needed)
- **VPS** (DigitalOcean, Linode, etc.) — nginx + PM2 + MongoDB Atlas
- **Other PaaS** — Railway, Fly.io, Heroku-style platforms

## Skills assumed

- Basic command-line use (`cd`, `npm install`, editing text files)
- Creating environment variables on your hosting platform
- MongoDB Atlas dashboard navigation (or willingness to follow step-by-step screenshots)

## Cost estimate (smallest setup)

| Item                          | Typical cost                              |
| ----------------------------- | ----------------------------------------- |
| MongoDB Atlas M0              | Free (512 MB)                             |
| Render web service (API)      | Free hobby tier (cold starts<sup>1</sup>) |
| Render static site (frontend) | Free                                      |
| Custom domain                 | ~$10–15/year                              |

1. **Cold start** means that if the server hasn't be pinged in a while, it shuts down and requires ~30 seconds to start up again. But once in regular use, like during a repair event, that shouldn't happen.

## Related pages

- [Local development](./local-development.md)
- [Deployment overview](../deployment/overview.md)
