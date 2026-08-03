# Link Rotator & Bio Pages

> Self-hosted link rotation and bio-page platform with an admin dashboard, click tracking, and Meta Pixel integration. Built for paid-traffic funnels that need rotating destinations — like WhatsApp groups that fill up — with zero downtime.

![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue)
![Deploy](https://img.shields.io/badge/deploy-Railway-0B0D0E?logo=railway&logoColor=white)

## The problem this solves

If you run ads that send people into WhatsApp groups (or any destination with a capacity limit), you hit a wall: groups fill up, links die, and your ad spend leaks into broken destinations. Manually swapping links inside a live ad campaign is slow and risky.

This platform gives you **one permanent URL** for your ads. Behind it, destinations rotate automatically — when a group is full, you flip to the next one in the admin panel, and the ad never changes.

## Features

- **Link rotation** — one stable public URL, multiple destinations, instant switching
- **Bio pages** — lightweight link-in-bio pages served from the same app
- **Admin dashboard** — create, edit, activate/deactivate links from a protected panel; no redeploys needed
- **Click tracking** — per-link click counts and history
- **Meta Pixel integration** — fires PageView/ViewContent events on redirect pages, feeding conversion data back to your ad campaigns
- **Persistent storage** — data survives restarts and redeploys (Railway Volumes / local disk)
- **Zero external database** — file-backed storage keeps it simple and cheap to run

## How it works

```
                 ┌─────────────────────────────────────────────┐
  Ad click ────▶ │  GET /r/:slug                                │
                 │   1. load rotation config from storage       │
                 │   2. pick the active destination             │
                 │   3. render pixel page → fire Meta Pixel     │
                 │   4. redirect user to destination            │
                 └─────────────────────────────────────────────┘
                              ▲
                              │ manage links, destinations, order
                 ┌─────────────────────────────────────────────┐
                 │  Admin dashboard  (/admin, password-gated)   │
                 └─────────────────────────────────────────────┘
```

## Quick start

```bash
git clone https://github.com/YOUR_USERNAME/link-rotator.git
cd link-rotator
npm install
cp .env.example .env   # set your admin password
npm start
```

Open `http://localhost:3000/admin`, log in, and create your first rotator.

## Configuration

| Variable | Required | Description |
|---|---|---|
| `ADMIN_PASSWORD` | ✅ | Password for the admin dashboard |
| `PORT` | — | HTTP port (default: `3000`) |
| `DATA_DIR` | — | Where link data is persisted (default: `./data`) |
| `META_PIXEL_ID` | — | If set, redirect pages fire Meta Pixel events |
| `BASE_URL` | — | Public base URL, used to display full short links in the panel |

## Deploying on Railway

1. Create a new Railway project from this repo.
2. Add a **Volume** mounted at `/data` and set `DATA_DIR=/data` — your links survive every redeploy.
3. Set `ADMIN_PASSWORD` (and `META_PIXEL_ID` if you use Meta Ads).
4. Deploy and point your custom domain at the service.

## Typical use cases

- **Meta Ads → WhatsApp group funnels** — rotate groups as they fill, keep one URL in the ad forever
- **Creator bio links** — a self-hosted alternative to link-in-bio SaaS, with your own pixel
- **Launch campaigns** — switch a single shared URL between waitlist, checkout, and sold-out pages in real time

## Project structure

```
src/
├── index.js        # Express app + routes
├── admin/          # dashboard UI and auth
├── rotator.js      # rotation logic + click tracking
├── storage.js      # file-backed persistence layer
└── views/          # redirect + bio page templates (pixel injection)
```

## Roadmap

- [ ] Automatic destination health checks (mark dead links)
- [ ] UTM parameter pass-through
- [ ] CSV export of click data
- [ ] Multi-user admin accounts

## License

MIT — see [LICENSE](LICENSE).

---

Built by **Matheus Perez** · Automation Specialist · [LinkedIn](https://linkedin.com/in/YOUR_USERNAME)
