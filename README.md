# BETA Stack

A minimal monorepo showcasing **Bun**, **Elysia** (with Eden Treaty), **Tailwind** (with daisyUI), and **Astro**.

**License:** [MIT](LICENSE)

## Stack

- **Bun** – runtime & package manager
- **Elysia** – fast backend API with Eden Treaty for end-to-end type safety
- **Astro** – landing & blog with heavy SEO
- **Tailwind + daisyUI** – styling

## Structure

```
apps/
  api/       → Elysia server (auth demo for Eden Treaty)
  landing/   → Astro site (blog, login section)
```

## Quick start

```bash
bun install
bun run dev
```

Then open:

- **Landing:** http://localhost:4321  
- **API:** http://localhost:3000  

`bun run dev` starts both: the API runs in the background and the landing app in the foreground. For the login demo on the landing page to work, both must be running (or run `bun run dev:api` and `bun run dev:landing` in two terminals).

## Setup & env

- **Landing** – Optional: create `apps/landing/.env` and set `PUBLIC_API_URL` if your API is not at `http://localhost:3000`. See `apps/landing/.env.example`.
- **API** – Optional: set `PORT` (default `3000`) and `CORS_ORIGIN` (e.g. your frontend URL in production).

## Scripts

| Script | Description |
|--------|-------------|
| `bun run dev` | Run API + landing (API in background, landing in foreground) |
| `bun run dev:api` | Run API only → http://localhost:3000 |
| `bun run dev:landing` | Run landing only → http://localhost:4321 |
| `bun run build` | Build API + landing |
| `bun run build:landing` | Build landing (static output) |
| `bun run check` | Run tests and build |
| `bun run test` | Run API tests |
| `bun run lint` | Lint API and landing src |

## Eden Treaty

The landing app uses Eden Treaty to call the API with full TypeScript types:

1. **API** exports its Elysia app type: `export type App = typeof app` (see `apps/api/src/index.ts`).
2. **Landing** imports that type and creates a client: `treaty<App>(url)` in `apps/landing/src/lib/eden.ts`.
3. Every route and response is **typed** — no codegen, full autocomplete on the frontend.

Try the login section on the site: the form calls `POST /auth/login` and the response type comes from the server.

## Blog

Posts live in `apps/landing/src/content/blog/` as Markdown (frontmatter: title, description, pubDate, tags). The blog index, individual post pages, related posts, and RSS feed are generated from this content collection.

## Production & deployment

- **Landing** – Set `site` in `apps/landing/astro.config.mjs` to your real domain. Set `PUBLIC_API_URL` to your deployed API URL.
- **API** – Deploy the API (e.g. Railway, Fly) and set `PORT` if needed. Set `CORS_ORIGIN` to your frontend origin (e.g. `https://yoursite.com`) so the browser allows requests. The login demo requires the API to be reachable and CORS to allow your frontend.
- **Security** – If you add a server (e.g. Node adapter for Astro or a reverse proxy), set headers like `X-Content-Type-Options: nosniff` and `X-Frame-Options: DENY`; see your platform’s docs.

## License

MIT © See [LICENSE](LICENSE) for details.
