# ROCK203 registration repair: setup and deployment

## Files

- `ROCK203-registration-repaired.html` — complete website page with changes limited to tournament registration.
- `ROCK203-supabase-registration-setup.sql` — idempotent table/column setup and Row Level Security policies.

## Supabase setup

1. Sign in to the Supabase project already configured in the HTML file.
2. Open **SQL Editor**, paste the supplied SQL file, and run it once.
3. Confirm the existing owner account uses `rockesprorts.official@gmail.com` (the spelling is preserved from the website's current configuration). Sign in through the site's Admin/Edit control with that account.
4. Keep the storage bucket private. The SQL creates/configures `rock-player-documents`; public visitors may upload registration images and only the owner account can read them.

The SQL creates the settings and registrations tables when absent and adds the scheduling columns to existing settings tables. It also defines public settings reads, schedule-checked pending submissions, owner-only registration viewing/status updates, and storage access policies. Review any existing custom policies before running this on a project with separate registration security rules.

## Publish the page

1. Back up the current site file.
2. Replace the deployed page with `ROCK203-registration-repaired.html` using the site's existing static hosting workflow.
3. Open the live page, sign in as the owner, open the registration admin panel, select event types, and set the open/close date and time. The date/time inputs use the administrator's local timezone; they are stored as UTC instants.
4. Submit one test registration with test contact details and small JPG/PNG/WebP images. Confirm it appears in the owner list, use **DOWNLOAD FILE**, and download **ALL (EXCEL)**. Remove the test record and files through Supabase when finished.

The owner can also enable registration without either timestamp for an immediate open-ended window, or leave **registration open** unchecked to close it manually. A scheduled open/close is additionally enforced by the database insertion policy, so a stale browser cannot submit outside the configured window.

## Export behavior

- **DOWNLOAD FILE** creates one self-contained HTML player file with the submitted details, player photo, and team logo embedded.
- **DOWNLOAD ALL (EXCEL)** exports every registration record as an Excel-readable `.xls` workbook, including storage object paths. It does not package every player's images into a bulk ZIP; download each complete player file from its row when needed.
- **PRINT / SAVE PDF** remains available in each player's detail view.

## Email/SMS notifications

The page already calls the Supabase Edge Function `rock-registration-notify` after a successful submission. The function is not included in the uploaded HTML source. Email/SMS delivery requires that function to be deployed in the configured Supabase project and connected to the provider credentials/secrets it expects. Without it, registration still saves; notifications are optional and are not claimed as sent.

No deployment or live Supabase change was performed here. The SQL must be run and the HTML uploaded through the existing hosting account.
