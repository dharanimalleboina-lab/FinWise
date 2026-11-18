# FinWise — Supabase-ready project

This folder contains the complete FinWise website plus its Supabase database setup. The website code stays in this folder. You do **not** upload the whole website folder into Supabase.

## Start here

1. Extract the ZIP file.
2. Open the `FinWise_Supabase_Ready` folder in VS Code.
3. Read `SUPABASE_SETUP.md` from top to bottom.
4. Start the site with VS Code Live Server after the Supabase setup is complete.

## Project files

- `index.html` — the FinWise website: login screen and finance dashboard.
- `js/supabase-config.js` — the ONLY file where you paste your own Supabase Project URL and Publishable key.
- `js/auth.js` — sign-up, login, logout, password recovery, and saving/loading the signed-in user's finance data.
- `supabase/schema.sql` — SQL script to run once in Supabase SQL Editor. It creates the six FinWise data tables and their Row Level Security policies.
- `SUPABASE_SETUP.md` — beginner-friendly setup guide and explanation of what is created in Supabase.
- `backup/index-before-upgrade.html` — untouched copy of the original uploaded FinWise HTML.

## What is stored in Supabase?

The SQL script creates these tables:

1. `transactions` — income and expense records.
2. `budgets` — category budget limits.
3. `savings_goals` — savings goal names, targets, current amounts and target dates.
4. `expected_expenses` — planned bills/expenses, due dates and paid status.
5. `wishlist_items` — item name, price, link, priority and purchased status.
6. `user_settings` — monthly savings target, sound preference and avatar choice.

Supabase Authentication separately manages account credentials. FinWise does not store users' passwords in its own tables.

## Important security notes

- Use only your Project URL and Publishable key in `js/supabase-config.js`. A legacy `anon` key is also accepted.
- NEVER put a Supabase Secret key or `service_role` key in browser code or GitHub.
- Do not turn off Row Level Security (RLS). The included SQL creates policies that restrict each user's rows to their own account.
- This is an educational finance tracker, not a bank connection or financial institution. Do not enter bank passwords, card PINs, OTPs or other banking credentials.
- The project cannot connect until you add the URL and Publishable key belonging to your own Supabase project and run the SQL setup.

## Run locally

Open `index.html` with VS Code Live Server. Do not use a `file://` URL. Internet access is needed for the external styling/chart/audio libraries and Supabase JavaScript client.
