# 🐾 Pawprint

Pet services discovery and comparison marketplace for **Mumbai, Bengaluru, Delhi, Gurugram and Noida**.
Search, filter, compare up to 4 providers, then go to the provider's own booking page. Referral clicks are tracked.

Pawprint is a **discovery and referral marketplace, not an appointment system**. "Book now" redirects to the provider's booking page, then their website,
then shows phone and Google Maps link. It never claims a booking is confirmed.

## Features
- 7 categories, city browsing, search, filters (service, pet type, price, rating, open now, home service, emergency), 5 sort options
- Side-by-side comparison (up to 4), saved comparison lists
- Provider pages with SEO markup (canonical + JSON-LD), sitemap and robots.txt
- Accounts (bcrypt + JWT), pet profiles, favourites, recently visited
- Reviews: one per user, link-spam held for moderation, "unverified" unless an admin marks verified
- Business registration queue with admin approval
- Admin dashboard: approve/reject, edit booking URLs, verify/hide/delete providers, moderate reviews, users and roles, referral click stats
- Security: input validation, parameterised SQL, rate limits, strict content security policy

## Tech
Node 18+, Express, SQLite (better-sqlite3), vanilla JS single-page frontend. No build step.

## Quick start
```bash
npm install
ADMIN_EMAIL=you@example.com ADMIN_PASSWORD='a-long-password' npm run seed
ADMIN_EMAIL=you@example.com ADMIN_PASSWORD='a-long-password' npm start
```
Open http://localhost:3000. If `ADMIN_PASSWORD` is not set, a random admin password is printed once on first start.
Run `npm test` for the integration tests.

## Environment variables
| Name | Purpose |
|---|---|
| `PORT` | Server port (default 3000) |
| `BASE_URL` | Public URL, used in sitemap and canonical links |
| `JWT_SECRET` | Token signing secret (otherwise generated into `data/.secret`) |
| `ADMIN_EMAIL`, `ADMIN_PASSWORD` | First admin account, created only if no admin exists |
| `DB_PATH` | SQLite file location |

## Data
`data/providers.json` holds 65 real businesses compiled from public web pages (name, area, address, phone/website where public, plus the source URL).
All are marked **Unverified** until an admin verifies them. No ratings, prices or reviews were copied from other platforms.
Where a listing has no website, the card shows its phone number, else a Google Maps link.
Load more listings with `node seed.js yourfile.json` (same shape; re-seeding only fills empty fields).

## Frontend files
The app is served from `public/` as `index.html`, `style.css` and `app.js`. The server's security policy blocks inline scripts, so keep them separate when using the bundled server.
`pawprint-index.html` is the same frontend combined into one file for reading or for hosting elsewhere; it expects the API at `/api` on the same origin and needs the policy relaxed to run inline.

## Not built yet
Google login, SMS/email OTP, photo uploads, Google Maps embed and geocoding (so "Nearest first" needs coordinates), website analytics, verified-customer review proof.

## Before going live
Use HTTPS, set `JWT_SECRET` and `BASE_URL`, back up `data/pawprint.db`, and consider moving the token from localStorage to an httpOnly cookie with CSRF protection.

## Disclaimer
Listings come from public web sources and may be out of date. Confirm details with the provider.
