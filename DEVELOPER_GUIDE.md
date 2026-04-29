# Developer Guide — My Blog

This document helps contributors and maintainers get the project running, understand structure, and make common changes.

**Project Overview**
- **Purpose:** A small blog app built with Express, EJS, and MongoDB.
- **Stack:** Node.js (ES modules), Express, EJS views, MongoDB.
- **Entry point:** `server.js`

**Repository Structure**
- views/: EJS templates (pages and layout partials).
  - views/layout/: shared fragments (`head.ejs`, `nav.ejs`, `footer.ejs`).
- public/: static assets (CSS, client files).
- server.js: application server and routes.
- package.json: scripts and dependencies.
- .env: environment variables (not committed).

Getting started
- Prerequisites:
  - Node.js (>=16 recommended)
  - pnpm, npm, or yarn (project uses pnpm in local workflow)
  - MongoDB: local instance or cloud cluster

- Install dependencies:

```bash
pnpm install
```

- Create `.env` (example shown in the repo). Minimum variables used by the app:

```
PORT=3000
CLUSTER_NAME=cluster0
DB_NAME=my_blog_db
DB_USER=testing
DB_PASSWORD=xxxxx
```

Note: For local MongoDB you can set `uri` in `server.js` to `mongodb://127.0.0.1:27017/` (the app already uses this by default).

Run the app (development):

```bash
pnpm dev
# or
nodemon server.js
# or
node server.js
```

Application details
- Views: templates live in `views/`. Main page is `index.ejs`, post detail is `detail.ejs`, and new post form is `create.ejs`.
- Static files: served from `public/` (CSS, fonts). Bootstrap and Font Awesome are served from `node_modules` at `/bootstrap` and `/fontawesome`.

Database
- Database name: from `DB_NAME` or default in code.
- Collection used: `posts`.
- Post document shape (when created by app):
  - `slug` (string)
  - `title` (string)
  - `subtitle` (string)
  - `body` (string)
  - `createdAt` (Date)
  - `updatedAt` (Date, optional)

Routes and views (quick map)
- GET `/` → `index.ejs` (list posts)
- GET `/about` → `about.ejs`
- GET `/posts/create` → `create.ejs`
- POST `/posts/create` → creates a post, renders `success.ejs` on success
- GET `/posts/:id` → `detail.ejs` (single post)
- GET `/posts/:id/edit` → `edit.ejs`
- POST `/posts/:id/edit` → updates post and redirects
- POST `/posts/:id/delete` → deletes post and redirects

Important implementation notes & known issues
- `slugify(title)` in `server.js` — the code attempts regex replacements but uses string literals for the patterns. Replace usages like `.replace("/[^\\w\\s]/g", "")` with proper RegExp calls, e.g.:

```js
function slugify(title) {
  return String(title)
    .toLowerCase()
    .trim()
    .replace(/[^
        \w\s]/g, "")
    .replace(/[\s_\-]+/g, "-")
    .replace(/^[-]+|[-]+$/g, "");
}
```
Development workflow and conventions
- ES modules: files use `import` / `export`. Keep `type: "module"` in `package.json`.
- Format: keep existing coding style. Avoid large unrelated reformatting.
- Logging: Morgan is used for HTTP logging.

Adding a new feature (suggested steps)
1. Create a branch: `git checkout -b feat/short-description`.
2. Implement server logic in `server.js` or add a file and import it there.
3. Add or update EJS templates in `views/` and layout fragments in `views/layout/`.
4. Add client-side assets to `public/` if needed.
5. Test locally with `pnpm dev` and manual flows.
6. Create a concise PR description and link related issues.

Testing and validation
- There are no automated tests in this repo. Test manually by exercising UI flows:
  - Create a post, edit it, view detail, and delete.
  - Check logs for DB connection errors.

Deployment notes
- Production: ensure `PORT` and DB credentials are provided in environment.
- If using a MongoDB Atlas cluster, set the connection URI accordingly (example commented in `server.js`).
- Close the Mongo client on process termination — the app already listens for `SIGINT` to close the client.

Troubleshooting
- "Database connection not established": ensure MongoDB is running and `DB_NAME` is set; inspect startup logs for connection errors.
- Slugs generate incorrectly: see `slugify` fix above.
- Failed deletes: confirm `deleteOne` code checks `result.deletedCount`.

Contributing
- Keep PRs small and focused.
- Describe intent and test steps in PR description.
- For frontend changes, update `views/` partials and test across pages.

Where to look in code
- Server & routes: [server.js](server.js)
- Views: [views/](views/)
- Static assets: [public/style.css](public/style.css)

---
Last updated: 2026-04-29
