# Italy 2026 — Family Trip Website

A self-contained Progressive Web App for our family trip to Lake Garda and Milan, 28 Jun – 8 Jul 2026.

## Features

- 📅 **Itinerary** — 11-day plan with logistics, tips, places, weather and Google Maps links
- 🏠 **Today** — auto-highlights the current day, shows countdown, weather, currency converter
- 🧳 **Packing list** — categorised checklist with progress, synced across phones
- 🇮🇹 **Italian phrases** — tap any phrase to hear it spoken
- 📞 **Contacts** — one-tap calling for emergency/trip contacts
- 💶 **Budget** — log expenses by category, running total, synced
- 📷 **Photos** — add photos per day, synced across phones
- 🌤️ **Weather** — daily forecast from Open-Meteo, refreshed daily
- 💱 **Currency** — GBP↔EUR converter with live rate
- 👨‍👩‍👦‍👦 **Family sync** — Firebase real-time sync across both phones
- 📱 **Installable PWA** — works offline once loaded

## Deploy to GitHub Pages

1. Create a new GitHub repo (e.g. `italy-2026`), public.
2. Upload all files in this folder to the repo root.
3. Settings → Pages → Source: **Deploy from a branch** → Branch: **main** / **/ (root)** → Save.
4. Site goes live at `https://<your-username>.github.io/italy-2026/` in ~1 minute.
5. Open the URL on each phone, install to home screen (Chrome: ⋮ → Install app).

## Firebase Setup (for family sync + photos)

The site works locally without this — but for syncing packing/budget/photos across both phones, you need to enable Firebase. Your config is already baked into `index.html`. Two remaining things to do in the Firebase console:

### 1. Enable Firestore Database

If not already done: Firebase console → Build → Firestore Database → Create database → location **eur3 (europe-west)** → start in production mode → Enable.

### 2. Enable Anonymous Authentication

Build → Authentication → Get started → Anonymous provider → Enable → Save.

### 3. Deploy the security rules

The `firestore.rules` file in this folder protects your database. To deploy:

1. Firebase console → Build → Firestore Database → **Rules** tab.
2. Replace the existing rules with the contents of `firestore.rules`.
3. Tap **Publish**.

The rules require:
- Anonymous auth sign-in (the app does this automatically)
- Valid 8-char alphanumeric trip codes
- Document size limits (prevents abuse)
- Photos must include a valid day index and reasonable size

**Without these rules, anyone could read/write your database.** Publish them before sharing the site URL.

### 4. Stay on the Spark (free) plan

The free tier provides:
- 50,000 Firestore reads/day, 20,000 writes/day, 1GB storage
- More than 100× what a family trip would use

**Critical:** never upgrade to Blaze (pay-as-you-go). If you stay on Spark, you can never be charged — Firebase will rate-limit you before billing kicks in. The site is designed to stay well within Spark limits:

- Packing/budget updates are debounced (~1 write per 800ms of activity)
- Max 10 photos per day (110 total)
- Photos resized to ~1000px / ~250KB
- Real-time listeners stay open while app is open, not when closed

## How Family Sync Works

1. Open the site (on either phone) — anonymous auth kicks in invisibly.
2. A setup prompt appears: **Create New Trip** generates an 8-character code; **Join Existing Trip** asks for a code.
3. One of you creates a trip, sends the other the code via WhatsApp.
4. The other phone enters the code → instantly synced.
5. Tick a packing item on one phone, it ticks on the other within ~1 second.

**To leave / change trip:** Contacts tab → Family Sync section → Manage.

## Before You Travel

Edit `index.html` and find the `contactsTrip` array. Add proper numbers for:
- Travel insurance emergency line
- Bank lost-card line(s)

Also verify the campsite, hotel, and taxi numbers match your bookings.

## Data Storage & Privacy

- Without sync: data is local to each device's browser storage
- With sync: data is on Google's Firestore servers, accessible to anyone with your trip code
- Trip codes are 8 random alphanumeric chars = 2.8 trillion combinations, unguessable
- API key in the HTML is public-by-design (it identifies the project, not authorises access; the security rules do that)
- Photos are compressed to ~1000px / ~250KB before upload — fine for in-app viewing, not full archival quality (keep originals in Google Photos for that)

## Photo Limits

- Max 10 photos per day = 110 total trip photos
- Each ~250KB after compression
- Total ~27MB — about 2-3% of the 1GB Firestore free tier

## Tech

Vanilla JavaScript, no build step. Single HTML file, manifest, service worker, Firebase v10 modular SDK loaded from CDN. APIs used:
- **Firebase Firestore** — family sync
- **Open-Meteo** — daily weather (no key)
- **Frankfurter** — currency rate (no key)
- **Google Maps** — place links (no key, just URL params)

## Updating the Site

To make changes:
1. Edit files locally
2. Push to GitHub (or edit in the GitHub web UI)
3. Site rebuilds in ~1 minute
4. On your phone, close and reopen the installed PWA to pick up the changes (the service worker caches files between visits)

## Troubleshooting

**Sync says "Sync error" or "Sync unavailable"** — Firebase isn't reachable or rules are blocking writes. Check that you published the rules in `firestore.rules`, and that Anonymous Auth is enabled.

**Photos won't upload** — check daily count (10 max), or the file might be too large after compression (very high-res photos can exceed limits).

**Other phone doesn't see my changes** — both devices must be using the same trip code. Open Contacts → Family Sync to verify.

**The PWA won't install** — must be served over HTTPS (GitHub Pages does this automatically) and have a registered service worker. Try Chrome rather than Samsung Internet on Android.
