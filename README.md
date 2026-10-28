# DA GOTV Car Tracker v2.0 (pilot)

Three roles: **GOTV Manager**, **Zone Controller**, **Driver**.

## Start locally

Node.js 18 or later is required. On Linux/macOS:

```sh
export GOTV_MANAGER_USER=manager
export GOTV_MANAGER_PASSWORD='REPLACE-WITH-YOUR-OWN-STRONG-SECRET'
npm start
```

On Windows PowerShell:

```powershell
$env:GOTV_MANAGER_USER='manager'
$env:GOTV_MANAGER_PASSWORD='REPLACE-WITH-YOUR-OWN-STRONG-SECRET'
npm start
```

Open http://localhost:3000 and log in with the bootstrap account. An initial manager account is created only if none exists. Never use the sample secret. The `data/store.json` file contains hashed passwords, audit records and sensitive coordinates and must be secured.

## Operations

1. Manager creates BY_ELECTION, NPE, or LGE campaigns.
2. Manager creates controller accounts with *zone* and *campaign access*, and driver accounts.
3. Manager imports the official IEC stations as CSV (columns: `id,name,ward,municipality,province,lat,lng`) and optional ward GeoJSON. VD is the `id`. Data is not bundled or synchronised automatically.
4. Manager adds vehicles, allocates one driver and one zone and selects *one or more campaigns*. Managers can remove allocations by editing, or remove a vehicle entirely when not on duty.
5. Drivers sign in and **clock in** to their assigned vehicle. They tap Start GPS and accept location permission. They stop GPS and clock out after their shift.
6. Zone controllers view vehicles only where both assigned zone and campaign access match. They can send zone messages within their authorised campaigns. Drivers see messages for their allocated zones and campaigns.

## Permissions

| Feature | Driver | Zone controller | GOTV manager |
|---|---|---|---|
| Clock in/out | Own assigned vehicle | No | No |
| Share GPS | Own active shift | No | No |
| View fleet | Own vehicle | Own zone + campaign | All |
| Send messages | Read only | Own zone + authorised campaign | All campaigns |
| Campaign/vehicle management | No | No | Yes |
| Create/disable users | No | No | Yes |
| IEC station and ward imports | No | No | Yes |

### Important pilot limitations

- Mobile browser geolocation generally works only over **HTTPS** (localhost is an exception). You must deploy behind TLS for remote use.
- Browsers may suspend GPS updates in background/locked states. A dedicated mobile app or GPS hardware integration is required for dependable 24/7 tracking.
- Sessions expire after 12 hours; logout removes the session; disabling an account revokes its sessions.
- No push notifications: messages are shown in the app and polled at 10-second intervals.
- No enterprise authentication, rate limits, encrypted database, location retention controls or production security audit yet. **Do not expose the pilot directly to the public internet without hardening.**
- The station CSV parser in the interface is basic and does not support comma-quoted fields. Prepare normalized CSV before import.
- Clock-out clears the vehicle's last map location but administrative/audit retention controls should be added before real-world use. Get informed driver consent and implement a POPIA privacy policy.
- If migrating v1.1 store files, back them up; existing vehicles without assigned driver IDs will need to be reassigned. This version intentionally does not accept old bearerless GPS URLs or old admin-key-based APIs.

## Version 3: Driver smartphone GPS links

- Manager signs in, creates a driver and assigns that driver to a vehicle and an election campaign.
- Open **Vehicles → Secure driver phone link**, select the vehicle and select **Generate 8-hour link**. Copy the URL and send it privately to that vehicle's assigned driver. Anyone possessing the URL can operate that vehicle's clock/tracking controls until it expires or is revoked. Avoid public channels.
- Driver opens the link on a mobile phone using **HTTPS**, clocks in, selects **Start GPS sharing**, and grants the browser location permission. Updates are sent to the server when GPS readings are received, no more often than about every four seconds.
- Manager and Zone Controllers monitor GPS on the Live Tracking dashboard (refreshes every three seconds when visible). Position timestamps older than two minutes are considered stale. Manager can revoke the link immediately. Drivers can stop tracking or clock out; the server clears the last visible coordinates on either action.
- Driver links are scoped to a single vehicle, valid up to 24 hours (eight hours from the supplied interface), stored as SHA-256 hashes in the server database, and invalidated on reassignment or revocation.
- The driver link URL contains its token in a fragment; the driver page removes the fragment from the browser address bar after loading.

**Limitations:** Browser GPS may suspend if the phone locks, the browser is backgrounded, or connectivity drops. The app does not provide reliable background tracking, push notifications, SMS, turn-by-turn navigation or offline replay. The location shown is the last reported fix, not guaranteed real-time movement. A public deployment requires HTTPS termination, backups, rate limiting, account and device hardening, administrative review, and an appropriate driver consent and privacy policy under POPIA. The included JSON store is intended for a controlled pilot, not high-volume election-day production usage. Staff should not use this while operating a moving vehicle.

## DA branding
The manager/zone dashboard and driver portal now include the DA branding slots. Read `public/BRANDING-ASSETS.md`. For the official offline logo, place the approved DA logo as `public/da-logo.png`; until then the interface retrieves the emblem from a public institutional site. Campaign-specific lockups should be provided as official assets.
