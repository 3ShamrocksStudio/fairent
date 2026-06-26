# Saved-data flow — FREE Supabase tier (no spend, ever)

Fairent's community reports + confirm/deny votes run through one tiny table.
With **no config**, the exact same flow persists on-device via `localStorage`.
Drop a free-tier Supabase URL + anon key in and the pool becomes **shared
across every renter, globally** — still $0, no card, no paid API.

> The anon (public) key is *designed* to be shipped in client code. It is not a
> secret; access is governed by Row-Level Security (RLS) policies, below.

## One-time setup (≈3 minutes, free)

1. Create a free project at <https://supabase.com> (Free plan — no card required).
2. In **SQL Editor**, run:

   ```sql
   create table public.reports (
     id        bigint generated always as identity primary key,
     key       text not null,
     city      text, street text, num text,
     rooms     numeric, sqm numeric, paid numeric,
     tags      jsonb default '[]'::jsonb,
     cmt       text,
     confirms  int default 0,
     denies    int default 0,
     voters    jsonb default '{}'::jsonb,
     demo      boolean default false,
     t         bigint
   );
   alter table public.reports enable row level security;

   -- PoC policies: anonymous renters may read all reports, add a report,
   -- and update vote tallies. Tighten later (e.g. an Edge Function for votes,
   -- rate limiting) without touching the client.
   create policy "anon read"   on public.reports for select using (true);
   create policy "anon insert" on public.reports for insert with check (true);
   create policy "anon vote"   on public.reports for update using (true) with check (true);
   ```

3. In **Project Settings → API**, copy the **Project URL** and the **anon public** key.
4. In `index.html`, set:

   ```js
   const SUPABASE = { url: "https://YOUR-PROJECT.supabase.co", anonKey: "eyJ...anon-public..." };
   ```

5. Commit + push. Pages redeploys; reports are now shared globally. Done.

## How the client uses it

- `sbAll()` → `GET /rest/v1/reports?select=*` loads the shared pool on start.
- `sbInsert()` → `POST /rest/v1/reports` when a renter publishes a report.
- `sbVotes()` → `PATCH /rest/v1/reports?id=eq.<id>` on every confirm/deny.
- Any network error falls back to the on-device `localStorage` cache, so the UI
  never breaks. Demo seed data is **only** used in local-only mode — it is never
  written to your database.

## Free-tier headroom

Supabase Free includes 500 MB Postgres + 50k monthly active users + unlimited
API requests. A text-only reports table uses a trivial fraction of that. There
is no metered/paid API in this stack (maps are free OpenStreetMap/Leaflet, no
key). **No billing is ever enabled.**

© 2026 3Shamrocks Studio. All rights reserved.
