# Backend ↔ frontend contract

## Resource object

The Supabase `resources` row is returned directly as:

```js
{
  id: string,
  title: string,
  description: string,
  category: 'web-dev' | 'app-dev' | 'ai-ml' | 'tools',
  url: string,
  tags: string[],
  is_featured: boolean,
  created_at: string
}
```

`hostname` and category icons are derived in the frontend. `all` is a UI-only filter and is never stored in the database.

## Operations

- `resourceService.getAll()` calls `resources.select('*').order('created_at', { ascending: false })`.
- `resourceService.create(input)` sends only `title`, `description`, `category`, `url`, and `tags`. PostgreSQL generates `id`, `created_at`, and `is_featured = false` through the insert policy.
- The created row is returned and prepended by `useResources`.

## Validation

The browser validates title 3–80 characters, description 10–200 characters, one of the four database categories, HTTP(S) URLs, and at most five tags. The service trims strings, normalizes URLs and tags, and maps Postgres constraint errors. Database constraints remain the final authority.

## Error mapping

| Service type | User-facing message |
| --- | --- |
| `NETWORK` | You appear to be offline. Check your connection. |
| `DUPLICATE` | This link is already in the Hub. |
| `VALIDATION` | Some fields aren't valid. Please review them. |
| `UNKNOWN` | Something went wrong. Please try again. |

## RLS behavior

Public `SELECT` is allowed. Public `INSERT` is allowed only with `is_featured = false`. Public `UPDATE` and `DELETE` are not allowed. The service-role key is never used client-side.

## Environment

```bash
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

If these variables are empty, the existing 24-item read/write demo fixture is used locally so the UI remains usable before Supabase setup. Once both variables are present, the app uses Supabase PostgREST.
