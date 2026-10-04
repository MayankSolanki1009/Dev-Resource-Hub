# Developer Resource Hub

A calm, searchable developer resource library built with React, Vite, Tailwind CSS, and Supabase.

## Frontend

The existing visual system remains intact. Components call `useResources`; only `src/services/resourceService.js` knows about Supabase. Search and category filtering stay derived from the fetched `resources` array. The UI supports loading, retry, empty, validation, duplicate, network, and generic error states.

## Run locally

Do not double-click `index.html` or open it with a `file://` URL. Vite uses ES modules, and browsers block those module requests from local files because of CORS/security rules. Start the development server from the project folder instead:

```bash
npm install
npm run dev
```

Then open the HTTP URL printed by Vite, normally `http://localhost:3000/`. Keep the terminal running while using the app.

## Connect Supabase

```bash
cp .env.example .env
# Fill VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY with the public values.
npm install
npm run dev
```

Then run `supabase/schema.sql` and `supabase/seed.sql` in the Supabase SQL Editor. Full setup and security notes are in [`supabase/README.md`](supabase/README.md). The exact data contract is in [`BACKEND_FRONTEND_CONTRACT.md`](BACKEND_FRONTEND_CONTRACT.md).

## Verification checklist

- Fetch returns approximately 24 rows in `created_at DESC` order.
- Refresh keeps seeded and newly inserted rows because they persist in PostgreSQL.
- Valid insert returns the created row and prepends it in the UI.
- Duplicate URLs are rejected by the unique lower-case URL index.
- Invalid categories, schemes, field lengths, and more than five tags are rejected by service validation and database constraints.
- Anonymous insert cannot set `is_featured = true`; public update/delete are denied by RLS.
- No service-role key is committed or exposed.

## V1 limitation

There is intentionally no authentication, moderation, rate limiting, or anti-spam protection yet. Anyone can submit a resource through the public insert policy. Those are appropriate V2 additions, not part of this college-project V1 scope.
