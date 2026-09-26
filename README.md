# Agency Report Kit — FINAL MVP

A GitHub Pages-ready marketing agency reporting SaaS.

## What is actually functional

### Product
- Premium responsive UI
- Demo dataset
- Real CSV file input
- Browser-side CSV parsing
- KPI calculations: spend, leads, CPL, ROAS
- Campaign breakdown
- Performance visualization
- Rule-based client story
- Print / Save as PDF
- $5/month Pro CTA via configurable Stripe Payment Link

### User system
- Google OAuth UI
- Supabase Auth integration
- User/profile schema
- Event tracking schema
- Report schema
- RLS policies

### Admin
- Admin UI
- Local fallback counters
- Supabase-ready architecture

## What you must configure

A genuinely secure Google login and cross-device user tracking cannot be created from a static GitHub Pages file alone. This repository includes the production integration points, but you must create the free third-party accounts and add your own credentials.

### 1. Supabase

Create a project at Supabase.

Then:
1. Authentication → Providers → Google → enable.
2. Configure Google OAuth credentials.
3. Add your GitHub Pages URL to Authentication → URL Configuration.
4. Open SQL Editor.
5. Run `schema.sql`.
6. Copy your project URL and ANON/PUBLIC key into `config.js`.

Never put a Supabase service-role key in `config.js`.

### 2. Stripe

Create a recurring $5/month Payment Link in Stripe.

Put the Payment Link into `config.js` as `STRIPE_PAYMENT_LINK`.

This gives you a real subscription checkout. For automatic Pro entitlement, connect Stripe webhooks to a secure backend/Edge Function that updates `profiles.plan`. Do not trust a client-side “paid” flag.

### 3. GitHub Pages

Upload all files:
- `index.html`
- `config.js`
- `schema.sql`
- `README.md`
- `LICENSE`

Settings → Pages → Deploy from `main` / root.

## Why the raw CSV does not go to Supabase

The current report engine parses the CSV in the browser. This is intentional for the MVP.

Only authenticated usage events/reports should be stored remotely.

## Production security

The included Admin UI is not a secure admin console until you implement an admin role check on the server/database.

Recommended:
- Supabase Auth
- RLS
- admin user ID allowlist or role
- server-side/Edge Function for admin aggregates
- Stripe webhook for subscription state

Never expose:
- Stripe secret key
- Supabase service-role key
- Google client secret

## Testing

1. Open `index.html`.
2. Click **Try the live demo**.
3. Click **Generate Report**.
4. Click **Export PDF**.
5. Upload your own CSV.
6. Click **Sign in with Google** after Supabase is configured.
7. Click **Admin** to inspect the dashboard.

## CSV example

```csv
Date,Campaign,Spend,Impressions,Clicks,Leads,Revenue
2026-09-01,Search,420,4200,310,72,980
2026-09-05,Meta Lead Gen,520,6800,410,96,1420
```

## License

MIT
