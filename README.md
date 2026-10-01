# Desk Lah! V1 Maintenance — Photo Restore 006

- Fixes reconnect/sign-in flow where Cloud draft text could be pulled while Cloud originals were not downloaded into the device IndexedDB cache.
- Storage re-check now immediately pulls draft text + Cloud originals and retries pending local photo uploads.
- Sign-in now verifies Storage and pulls original photos in the same flow.
- Preserves Photo Sync Hotfix 004 and Leave Ledger Compact 005 behavior.

# Desk Lah! V1 — Deployment / PWA Pack

This package is prepared for HTTPS static hosting and phone home-screen installation.

## Important privacy change
The company OT Claim XLSX is no longer embedded inside `index.html`. The deployed app fetches it only after login from the private Supabase Storage bucket `desk-templates`.

Before the first deployed Excel export, run `06-create-private-template-storage.sql` once in Supabase SQL Editor. Then, on desktop, click `導出 OT Claim`; if no template is stored yet, Desk Lah! asks you to choose the real company `.xlsx` once and uploads it to your private user folder.

## Files
- `index.html` — sealed Desk Lah! V1 app
- `manifest.webmanifest` — PWA install metadata
- `sw.js` — app-shell cache
- `icons/` — Desk Lah! home-screen icons
- `06-create-private-template-storage.sql` — private template bucket + RLS

## Deployment
Upload the contents of this folder to an HTTPS static host. Keep all files at the same directory level shown here.

## GitHub direct-upload layout

This package is intentionally FLAT for GitHub web upload.
Upload every file in this folder directly to the repository root.

The repository root should contain:
- index.html
- manifest.webmanifest
- sw.js
- apple-touch-icon.png
- icon-192.png
- icon-512.png
- icon.svg

No `icons/` folder is required in this version.

## Maintenance Hotfix 004 — Photo Cloud Sync
Diagnostic 003 confirmed `400 InvalidRequest: No content provided` during Storage Upload for iPhone/IndexedDB photos. Hotfix 004 sends IndexedDB photo bytes as ArrayBuffer instead of Blob/FormData, while preserving original MIME type and metadata.


## Maintenance UX 005 — Compact Leave Ledger
Leave records are now condensed for faster mobile scanning: date stays left, leave type/note stays center, day amount stays right on the same row. Empty notes no longer duplicate the day count, and long notes truncate cleanly. OT, Cloud sync, balances, Excel, and photo-sync Hotfix 004 are unchanged.
