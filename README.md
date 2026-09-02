# FreshCart

FreshCart is a small grocery-delivery app you'll build on throughout this program. It's not a finished product — it's the same working codebase you'll containerize, provision infrastructure for, ship through a pipeline, orchestrate, and monitor as the course goes on. Same app, every week, so the work compounds instead of starting over each time.

## What's actually here

Two services and a database:

- **`checkout-api/`** — an Express + TypeScript API backed by Postgres. Lists products, handles search, and places orders.
- **`storefront/`** — a static site (Vite + TypeScript, no framework) that calls the checkout API. Builds to plain HTML/CSS/JS.
- **Postgres** — not included in this repo. You'll stand it up yourself, in whatever way each week's lesson asks for.

Neither service ships with a Dockerfile, a `docker-compose.yml`, or any infrastructure code. That's intentional — writing those *is* the assignment in the weeks that need them.

## Running it locally, before any of that

You'll need a Postgres database reachable from your machine, and Node.js 20+.

**checkout-api**
```
cd checkout-api
cp .env.example .env      # edit DATABASE_URL to point at your Postgres
npm install
npm run dev                # tsx watch, restarts on save
```
Load `db/init.sql` into your database once, however you'd normally run a `.sql` file against Postgres — it creates the schema and seeds about 15 products. (If you're loading it into a container's Postgres, note that this file is written to work automatically if placed in Postgres's official `/docker-entrypoint-initdb.d/` — worth knowing before Week 4.)

Once it's running: `curl localhost:3000/healthz` should return `{"status":"ok"}`, and `curl localhost:3000/api/products` should return the seeded list.

**storefront**
```
cd storefront
npm install
npm run dev
```
Vite's dev server proxies `/api` to `localhost:3000` automatically (see `vite.config.ts`) — open the URL it prints, and you should see a product grid you can actually buy from.

## API reference

| Method | Path | Does |
|---|---|---|
| GET | `/healthz` | Checks the API can reach the database. |
| GET | `/api/products` | Lists all products. Add `?search=term` to filter by name. |
| GET | `/api/products/:id` | One product. |
| POST | `/api/orders` | `{ customerName, customerEmail, items: [{ productId, quantity }] }`. Validates stock, records the order transactionally. |
| GET | `/api/orders/:id` | An order and its line items. |

## What you'll do to this app, week by week

- **Week 4 (Docker):** write a multi-stage Dockerfile for each service (non-root, minimal base image), and a `docker-compose.yml` that runs both alongside Postgres on a shared network, with the database's data in a named volume.
- **Week 5 (Terraform):** provision real cloud infrastructure for it — a VPC, a private backend, a load balancer, and either a real static-site deployment or a container running your Week 4 image.
- **Weeks 6–8:** CI/CD, Kubernetes, and observability build directly on whatever you produced in Weeks 4 and 5. Details land with those weeks' content.

## Environment variables

**checkout-api** (`.env.example`): `DATABASE_URL`, `PORT`.
**storefront** (`.env.example`): `VITE_API_BASE_URL` — leave unset for local dev (the Vite proxy handles it); set it for a production build where the storefront is deployed separately from the API.
Triggering PR CI check
Triggering PR CI check 2
