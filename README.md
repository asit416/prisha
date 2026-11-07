# A Little Diwali Surprise for Prisha
React + Vite + React Three Fiber + Framer Motion.
## Run locally
```
npm install
npm run dev      # open http://localhost:5173
npm run build    # production build in /dist
```
## Edit text, timeline dates, captions
Open `src/content.js`. Timeline dates are placeholders ("Add your date"): write your own.
## Photos: option A (no setup)
Put photos in `public/photos/` named `1.jpg` … `6.jpg`. Captions: `defaultCaptions` in `src/content.js`.
## Photos: option B (cloud uploads at /customize, permanent)
1. Create a free project at supabase.com.
2. Authentication → Users → **Add user** (your email + password). Then Authentication → Providers → Email → **disable "Allow new users to sign up"** so only you can log in.
3. Storage → New bucket named `photos`, set **Public**.
4. SQL Editor → run:
```sql
create table site_config(key text primary key, value jsonb);
alter table site_config enable row level security;
create policy "public read" on site_config for select using (true);
create policy "owner write" on site_config for all to authenticated using (true) with check (true);
create policy "public read photos" on storage.objects for select using (bucket_id='photos');
create policy "owner write photos" on storage.objects for all to authenticated using (bucket_id='photos') with check (bucket_id='photos');
```
5. Settings → API: copy **Project URL** and the **anon public** key into `.env` (copy `.env.example`). Never use or commit the `service_role` key. The anon key is meant to be public; the policies above are what keep writes owner-only.
6. Visit `/customize`, sign in, upload, caption, reorder, Preview, **Save**.
## Deploy
Vercel or Netlify: import the repo, build `npm run build`, output `dist`. Add the two `VITE_SUPABASE_*` env vars in the dashboard. `vercel.json` and `public/_redirects` already make `/customize` work.
## Audio
Music is synthesized in the browser (no copyrighted files) and only starts after the first tap.
