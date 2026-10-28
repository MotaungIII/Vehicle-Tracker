# DA GOTV Car Tracker — Private trial setup

## What this package includes
- Manager, Zone Controller and Driver roles; campaign/vehicle allocation, in-app messaging
- Expiring driver links, clock in/out, explicit GPS start/stop, live dashboard refresh
- Installable web app manifest, app icons and service worker (static shell only)
- Render Blueprint with persistent disk; built-in `/healthz` health endpoint
- Local Node.js server for developers (you do NOT need Node.js installed on your own laptop when using Render)

## Launch on Render (hosted HTTPS, suggested for trial)
1. Sign up at https://render.com and connect a GitHub account.
2. Create a **private GitHub repository** and upload the **contents of this folder to its root** (including `render.yaml`, `public/`, `server.js`, `package.json`). Do not upload secrets, `data/store.json`, or driver links.
3. On Render choose **New → Blueprint**, select the repository and review the plan and charges. The included Blueprint requests a paid **Starter** instance with a 1 GB persistent disk; costs depend on the provider's current plan.
4. Supply `GOTV_MANAGER_PASSWORD` as a unique strong secret (at least 12 characters) when prompted. Initial username is `manager`.
5. Deploy. When healthy, open the HTTPS URL shown in Render and log in as manager.
6. Create a trial campaign (for example `Ward 91 Pilot`), Driver and Zone Controller accounts, add voting stations from **verified IEC data**, add vehicles and assign one driver/zone/campaign to each.
7. In the manager screen create a driver link for each vehicle. Share links **privately** with the corresponding driver. Drivers open the link on their own phone, clock in, press Start GPS and allow location access.
8. Zone Controllers log in with their own credentials on a separate device and view only the campaigns/zones they are assigned.
9. Run a supervised trial while keeping the driver app visible and phones powered. Have drivers stop tracking and clock out at the end. Revoke links afterward.

## Install on a phone
- Android: open your hosted HTTPS URL with Chrome → menu → **Install app** / **Add to Home screen**.
- iPhone: open in Safari → Share → **Add to Home Screen**. A regular browser tab works too.
- GPS requires HTTPS and user location permission. **Neither installed PWA nor browser tracking can guarantee background GPS when the phone is locked.**

## Field trial acceptance checks
- [ ] Manager can login and create campaign
- [ ] Assign vehicle, driver, zone and imported IEC station (with verified VD identifier)
- [ ] Zone Controller sees only their zone/campaign
- [ ] Driver link works and expires/revokes
- [ ] Driver clocks in; GPS updates arrive on manager/controller screens
- [ ] Permission denied, poor signal and lost connectivity are clearly observed
- [ ] Controller sends in-app message; driver receives it
- [ ] Driver stops GPS and clocks out; no further GPS uploads accepted
- [ ] Redeploy retains campaign data (persistent disk)
- [ ] Delete/revoke trial access and export operational records as needed

## Essential limitations / safety
- This is **pilot software**, not production-hardened. It uses a single JSON store; avoid large fleet load and simultaneous writes; do not use for mission-critical election-day dispatch.
- Driving: **Do not interact with a phone while driving**. Have a passenger handle controls or stop safely.
- GPS tracks the phone, not the physical vehicle, and can stop on background/lock, low battery, no signal or app suspension. Never interpret stale GPS as proof of driver/vehicle inactivity.
- Secure driver links function as bearer credentials: never post publicly, revoke on loss, use short expirations.
- Obtain informed consent, disclose retention/access rules and provide a clear privacy notice before real-world tracking. Review POPIA compliance.
- The PWA icons supplied here are **temporary blue GOTV placeholders**; replace with authorised Democratic Alliance logo/lock-up files before official distribution. Do not present placeholders as the official DA logo.
- Render has deployment and persistent-disk costs; read its current price sheet before confirming.
- The app DOES NOT automatically import up-to-date IEC wards/stations. Use official, verified datasets; test scope filtering.

## Troubleshooting
- `GOTV_MANAGER_PASSWORD` must be at least 12 characters on a fresh deployment.
- If the app fails to install, use the HTTPS link in the phone browser; PWA installation support varies.
- GPS errors: check browser location permission, HTTPS, and phone location services. Confirm app remains foregrounded.
- If dashboard updates stop: check online connection and last GPS timestamp; keep phones charged.
- If app data disappears after deploy: verify `GOTV_DATA_DIR=/var/data` and Render persistent disk is attached at `/var/data`.
