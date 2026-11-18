# FinWise + Supabase: setup guide for beginners

## First understand what goes where

You are building two connected parts:

- **FinWise website (your VS Code folder):** HTML, CSS and JavaScript. This is the screen people use.
- **Supabase project (online):** user sign-in and the database where FinWise saves finance records.

You do **not** upload `index.html`, `auth.js`, CSS, images or the whole ZIP into Supabase. You run one SQL file in Supabase, then copy two project connection values from Supabase into your local website.

## Part A — Create your Supabase project

1. Open <https://supabase.com/dashboard> and sign in.
2. Choose **New project**.
3. Name it `FinWise`.
4. Choose a strong database password and save it somewhere private. This is the database password; it is not the password your FinWise users will use to log in.
5. Choose the nearest available region and create the project.
6. Wait until the project has finished provisioning.

## Part B — Create the FinWise database tables

1. In the Supabase dashboard, open your `FinWise` project.
2. Open **SQL Editor**.
3. Choose **New query**.
4. In VS Code, open `supabase/schema.sql` from this project folder.
5. Copy the entire contents of `schema.sql` and paste it into the Supabase SQL Editor.
6. Click **Run**.
7. The script should complete without an error. Open **Table Editor** and check that the following six tables exist:
   - `transactions`
   - `budgets`
   - `savings_goals`
   - `expected_expenses`
   - `wishlist_items`
   - `user_settings`

This script creates the structure and security rules. It does not upload your personal transactions or any existing sample data.

## Part C — Enable email sign-in

1. Open **Authentication** in your Supabase project.
2. Open **Sign In / Providers** (the label may appear as **Providers**).
3. Make sure **Email** sign-in is enabled.
4. For the first test, email confirmation can remain enabled. When you register, check the inbox for the confirmation email.
5. Open **Authentication → URL Configuration**.
6. In **Redirect URLs**, add the local Live Server address you will use. For example, if Live Server opens `http://127.0.0.1:5500/index.html`, add `http://127.0.0.1:5500/**`. Use the host and port shown in your own browser address bar; Live Server may choose a different port.
7. For local testing, set the **Site URL** to your local site base address, such as `http://127.0.0.1:5500`. Later, after publishing, add the deployed website URL to Redirect URLs and update Site URL to the live website. Supabase only redirects to URLs allowed in this setting.

## Part D — Copy the two connection values

1. In the Supabase project, open the **Connect** dialog, or go to **Project Settings → API Keys**.
2. Copy the **Project URL**. It looks like `https://your-project-ref.supabase.co`.
3. Copy the **Publishable key**. It usually begins with `sb_publishable_`.
4. In VS Code, open `js/supabase-config.js`.
5. Replace only the two placeholder values:

   ```js
   window.FINWISE_SUPABASE_CONFIG = Object.freeze({
       url: "PASTE_YOUR_SUPABASE_PROJECT_URL_HERE",
       publishableKey: "PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE"
   });
   ```

   with your own values, for example (these are examples, not real credentials):

   ```js
   window.FINWISE_SUPABASE_CONFIG = Object.freeze({
       url: "https://abcxyzexample.supabase.co",
       publishableKey: "sb_publishable_example_only"
   });
   ```

6. Save the file (`Ctrl + S`).

**Never use the Secret key (`sb_secret_...`) or old `service_role` key here.** Those keys bypass database security and must never be placed in a browser website. Use the Publishable key only. A legacy `anon` key is accepted while migrating old projects.

## Part E — Run FinWise on your laptop

1. Open the whole extracted `FinWise_Supabase_Ready` folder in VS Code (not just `index.html`).
2. Install/use the VS Code **Live Server** extension if you do not already have it.
3. Right-click `index.html` and choose **Open with Live Server**.
4. The FinWise login screen should appear without the old “Add your Supabase Project URL and Publishable key in js/auth.js first” message.
5. Choose **Create account**, enter your name, email and password, and submit.
6. Confirm your email if Supabase asks you to.
7. Sign in. Add a small test transaction, refresh the page and check that it is still there.
8. In Supabase **Table Editor → transactions**, you should see the saved test row. It includes your `user_id` so records belong to the signed-in user.

## What exactly will be in Supabase?

The SQL creates these six tables. The actual records are saved later as people use the FinWise website.

| Supabase table | What FinWise stores there |
|---|---|
| `transactions` | `user_id`, `id`, `type`, `amount`, `category`, `description`, `transaction_date`, `created_at` |
| `budgets` | `user_id`, `category`, `amount_limit`, `created_at` |
| `savings_goals` | `user_id`, `id`, `name`, `target_amount`, `current_amount`, `target_date`, `created_at` |
| `expected_expenses` | `user_id`, `id`, `title`, `category`, `amount`, `due_date`, `paid`, `created_at` |
| `wishlist_items` | `user_id`, `id`, `name`, `price`, `link`, `priority`, `purchased`, `created_at` |
| `user_settings` | `user_id`, `savings_target`, `sound_enabled`, `avatar_id`, `updated_at` |

Supabase Authentication stores account credentials separately. Passwords are handled by Supabase Auth, not written into the FinWise tables.

## If you still see an error

- **“FinWise is not connected yet”** — check that you replaced the values in `js/supabase-config.js`, saved the file, and loaded the page with Live Server.
- **“Supabase library did not load”** — check your internet connection and reload; the Supabase JavaScript library is loaded from a CDN.
- **“database tables are not ready” / table not found** — run the full `supabase/schema.sql` file in SQL Editor and verify all six table names in Table Editor. If your project restricts Data API exposure, expose these tables to the Data API.
- **Email confirmation not received** — check spam/junk and the email provider settings in Supabase Authentication.
- Do not disable RLS or paste a Secret/service_role key to bypass an error.

## Before sharing publicly

Test with two separate accounts. Confirm that account B cannot see account A's records. Keep RLS enabled. Configure a production email provider and correct allowed redirect URLs for your deployed website before opening registration to everyone.
