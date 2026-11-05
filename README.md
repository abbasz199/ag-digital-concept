# AG Digital Concept

E-commerce website for computer accessories, built with **Next.js 15 (App Router) + TypeScript + Tailwind CSS**, with **Supabase** (database, auth, storage) and deployed on **Vercel** from **GitHub**.

**Business:** AG Digital Concept · garbadeen199@gmail.com · 08162456504 · First Gate Dadin Kowa, Jos South, Plateau State

## Features

- Home, products (search, category filter, sort), product detail, cart, checkout
- Guest cart in local storage; logged-in carts sync to Supabase
- Email/password signup, login, logout; account page with order history
- Orders priced server-side from the database (the browser cannot change prices)
- Services page: contact details, feedback form, returns policy
- About page with business details and Google Maps link
- Admin area (`/admin`): add/edit/delete products, upload images, view orders, update order status
- Row Level Security on every table

## 1. Run locally

```bash
npm install
cp .env.example .env.local     # then fill in your Supabase values
npm run dev                    # http://localhost:3000
npm run build                  # production build check
npm run typecheck
```

## 2. Set up Supabase

1. In your Supabase project open **SQL Editor** and run, in order:
   - `supabase/migrations/001_schema.sql` (tables, RLS policies, storage bucket)
   - `supabase/migrations/002_seed.sql` (categories and 12 sample products)
2. Copy **Project URL** and **anon key** (Project Settings > API) into `.env.local`.
3. Authentication > URL Configuration: set **Site URL** to your Vercel URL and add `http://localhost:3000` to **Redirect URLs**.
4. For quick testing you can turn off email confirmation (Authentication > Providers > Email).

## 3. Make yourself admin

Sign up on the site first, then run in the SQL Editor:

```sql
update public.profiles set role = 'admin'
where id = (select id from auth.users where email = 'garbadeen199@gmail.com');
```

Use the email you signed up with. Log out and in again, then open `/admin`.

## 4. GitHub and Vercel

```bash
git init
git add .
git commit -m "Initial commit: AG Digital Concept store"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ag-digital-concept.git
git push -u origin main
```

In Vercel: **Add New > Project > import the repo**, then add these Environment Variables (Production, Preview, Development):

| Name | Value |
| --- | --- |
| `NEXT_PUBLIC_SUPABASE_URL` | your Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | your anon key |

Every push to `main` deploys automatically. Never commit `.env.local` or the service role key.

## Payments

Checkout supports **pay on delivery** and **bank transfer**. To add Paystack or Flutterwave later, extend `payment_method` in `src/app/checkout/actions.ts` and the `orders` check constraint, and verify payments on the server with a webhook.

## Known limits

- Stock is checked when an order is placed but not reduced automatically; update stock in the admin area.
- Edit the About text and returns policy to match your real business terms.
