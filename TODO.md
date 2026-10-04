# Developer Resource Hub — Integration Checklist

- [x] Existing frontend visual design preserved; no CSS, spacing, color, breakpoint, card, modal, navbar, or search redesign.
- [x] Added `@supabase/supabase-js` with a single `resourceService` integration boundary.
- [x] Added exact snake_case resource contract: `id`, `title`, `description`, `category`, `url`, `tags`, `is_featured`, `created_at`.
- [x] Added database category values `web-dev`, `app-dev`, `ai-ml`, and `tools`; `all` remains a UI-only filter.
- [x] Added Supabase schema with UUID IDs, generated timestamps, field/category/URL/tag constraints, case-insensitive duplicate URL protection, and RLS.
- [x] Added public SELECT and restricted public INSERT policies; no public UPDATE or DELETE policies.
- [x] Added idempotent 24-resource seed SQL with three featured resources.
- [x] Added service-level URL/tag normalization, three-layer validation, and NETWORK/DUPLICATE/VALIDATION/UNKNOWN error mapping.
- [x] Wired `useResources()` to `resourceService.getAll()` and `.create()` while preserving the existing hook contract.
- [x] Added `.env.example`, root setup README, Supabase setup README, and backend/frontend contract documentation.
- [x] Verified local fallback build works without credentials, so the existing demo remains usable.
- [ ] Enable the Supabase connector and set `VITE_SUPABASE_URL` plus `VITE_SUPABASE_ANON_KEY` in the runtime environment; then run the SQL files and verify live GET/POST persistence and RLS behavior.
