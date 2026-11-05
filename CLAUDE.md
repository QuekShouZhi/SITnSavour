# SITnSavour

Global food-ordering web app (may later become a food directory).

## Stack
Next.js 15 (App Router, src/) · TypeScript strict · Tailwind v4 · Prisma 6 + Neon Postgres · Upstash Redis (cart, rate limiting) · Stripe Checkout + webhooks · Vercel

## Rules
- No separate backend: Server Actions and API Routes only.
- Money is stored as integer cents. Totals are recalculated on the server from DB prices.
- Only the Stripe webhook can mark an order PAID.
- One merchant per order.
- Secrets only in .env / .env.local; never commit them.

## Git
- Branches: feature/* → PR into staging (squash) → PR staging → main (merge commit).
- main and staging are protected; never push to them directly or force-push.
- Commit messages: imperative summary. A `Co-Authored-By: Claude` trailer is fine.

## Notes
Design notes live in Obsidian: G:\My Drive\Obsidian Vault\SITnSavour\ (one .md per topic, link with [[...]]).
Never edit the .obsidian folder

## Current task: schema (on feature/schema)
- Tables mapped to snake_case plurals with @@map.
- Models: User, Outlet, Merchant, MenuItem, Order, OrderItem, Payment, PaymentEvent.
- Outlet: drop slug, city, country; keep address, lat/lng, timezone.
- Merchant: drop slug, currency, ownerId. App uses one currency (SGD) from config; currency is still stored on Order and Payment.
- User: firstName, lastName, email, phone (drop name). No Role enum or owners yet.
- Move Stripe fields from Order to Payment (one order → many payment attempts; card brand and last4, never the full card number).
- PaymentEvent: unique Stripe event id (webhook idempotency) and the raw payload as JSON.
- Open: confirm SGD; confirm MenuItem keeps description, imageUrl, category, isAvailable.