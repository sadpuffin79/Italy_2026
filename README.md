# Italy 2026 — Family Trip Website

A self-contained Progressive Web App for our family trip to Lake Garda and Milan, 28 Jun – 8 Jul 2026.

## Features

- 📅 **Itinerary** — full 11-day plan with logistics, tips, and Google Maps links per day
- 🏠 **Today** — auto-highlights the current day during the trip; shows a countdown before
- 🧳 **Packing list** — categorised checklist with progress tracking, saved on your device
- 🇮🇹 **Italian phrases** — tap any phrase to hear it spoken in Italian (uses device voice)
- 📞 **Contacts** — one-tap calling for emergencies, the campsite, hotel, taxi
- 💶 **Budget tracker** — log expenses by category, see running total
- 📷 **Photos** — add a photo from each day, saved on your device
- 📱 **Installable** — adds to your phone home screen; works offline (handy in Venice!)

## Deploy to GitHub Pages

1. **Create a new GitHub repo**, e.g. `italy-2026`. Make it public.
2. **Upload all the files** in this folder to the repo root:
   - `index.html`
   - `manifest.json`
   - `service-worker.js`
   - `icon-192.png`
   - `icon-512.png`
   - `README.md` (optional)
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set:
   - **Source:** Deploy from a branch
   - **Branch:** `main` / `(root)`
5. Save. After ~1 minute, your site will be live at:
   `https://<your-username>.github.io/italy-2026/`
6. Open that URL on your phone. In Safari (iOS) tap Share → Add to Home Screen. In Chrome (Android), the in-app "Install" prompt will appear.

## Before You Travel

Edit `index.html` to add a few personal details:

- Inside the `contactsTrip` array, replace the placeholder text for **Travel Insurance** and **Bank lost-card line** with your actual numbers
- Double-check the **campsite, hotel, and taxi numbers** match what was on your booking confirmations (the numbers in there are best guesses)
- Anything else you'd like to add (e.g. a friend's number, GP back home)

## Data Storage

All your data (packing checkboxes, photos, budget entries) is stored **locally on the device** in `localStorage`. Nothing is sent anywhere. If you clear your browser data, you'll lose your entries. The site is hosted on GitHub but stores no data server-side.

The same site opened on a different device will have its own separate data.

## Photo Storage Limits

Photos are resized to ~800px and stored as base64 in localStorage, which has a ~5–10MB limit depending on browser. That's roughly 50–80 photos total across all days. The app will warn you if storage is full.

## Updates

If you make changes to `index.html`, push them to GitHub. The service worker caches files for offline use, so changes may take one refresh to appear. To force an update on your phone, close and reopen the installed app.

## Tech

Vanilla JavaScript, no build step, no dependencies. Single HTML file with inlined CSS and JS. PWA via `manifest.json` and `service-worker.js`. Maps are Google Maps web links (no API key needed).
