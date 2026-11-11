# Wiki.js on Render

Deploy [Wiki.js](https://js.wiki/) 2.x on Render with a managed Postgres database.

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/render-examples/wikijs)

## What this creates

| Resource | Type | Plan | Notes |
|---|---|---|---|
| `wikijs` | Web service (prebuilt image `ghcr.io/requarks/wiki:2`) | Standard | Health check at `/healthz` |
| `wikijs-db` | Render Postgres 16 | Basic-256mb, 5 GB | Internal connections only |

The service reads its database settings from `DB_*` environment variables.
The Blueprint fills them in from the database with `fromDatabase`, so you
never copy the password by hand. Wiki.js stores pages and uploads in Postgres,
so the service doesn't need a persistent disk.

## Deploy

1. Click **Deploy to Render** above, or create a Blueprint in the Render
   Dashboard that points at this repository.
2. When the deploy is live, open the service URL and finish the setup wizard
   right away. Set the admin email and password, and set the site URL to your
   `onrender.com` URL or custom domain.

> **Important:** Until you finish the setup wizard, anyone who opens the URL
> can create the admin account.

## Troubleshooting

**`Database Connection Error: ECONNREFUSED` on the first deploy.** A new
database can report "available" a short time before it accepts connections.
Wiki.js retries 10 times in about 30 seconds and then exits. Trigger a manual
deploy of `wikijs` and it connects.

## Upgrades

The `:2` tag follows the latest Wiki.js 2.x release. To pick up a new 2.x
release, trigger a manual deploy; Render pulls the image again. To pin a
version, change the tag (for example `ghcr.io/requarks/wiki:2.5.317`).
