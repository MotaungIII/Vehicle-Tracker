# DA GOTV Car Tracker — map fix v3.1

This is the **complete GitHub repository content**, not a snippet. Upload the contents of this directory to the **root** of MotaungIII/Vehicle-Tracker on the `main` branch, preserving `public/`. Do not upload the ZIP itself as a file.

## Changes
- Map requests use CARTO basemap tiles rather than OpenStreetMap volunteer tile servers. CARTO usage is subject to its own terms and may need a contracted tile provider for larger deployments.
- Replaces `Referrer-Policy: no-referrer` with `strict-origin-when-cross-origin`, allowing reasonable tile-provider origin information while not exposing full URLs.
- Maintains roles, driver links, GPS and campaigns from the previous release.
- Service worker cache name incremented to clear older app shell.
- Retains `public/` layout used by `server.js`.

## Render
- Keep the existing Render web service and persistent disk; don't create another service.
- Do not overwrite, delete, or reset the persistent volume /var/data. The repository includes no production `store.json`.
- Keep `GOTV_MANAGER_PASSWORD` secret in Render environment settings.
- Commit changes to `main`; Render should deploy automatically, or use Manual Deploy → Deploy latest commit.
- Open https://da-gotv-car-tracker-trial.onrender.com and refresh (Ctrl+Shift+R) after Live status.
- If 403 persists, inspect browser Developer Tools > Network for the failing map URL. A dedicated map tile account may be needed.

## Limitations
- This is an unverified hosted field pilot, not a security-audited production system.
- PWA GPS can stop updating when the phone is locked or the app is backgrounded.
- No guarantee of map tile availability from external provider.
- Never use live location tracking without the driver's informed permission.
