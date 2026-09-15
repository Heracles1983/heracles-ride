# HERACLES RIDE

A China-friendly, local-first cycling replay PWA for Intervals.icu data.

HERACLES RIDE focuses on an interaction pattern that is useful for cycling analysis:

- Read recent activities from Intervals.icu
- Replay GPS routes
- Scrub heart rate / power / speed / cadence / elevation charts
- Synchronize chart position with the route marker
- Display Intervals.icu activity intervals
- Cache previously opened activities locally with IndexedDB
- Install as a PWA on iPhone / iPad
- Use Baidu Maps as an optional China-friendly basemap
- Fall back to an offline SVG route view when maps are unavailable
- Export / restore local backups through the system Files sheet (including iCloud Drive on Apple devices)

## Demo

A temporary demo deployment may be available during development, but the repository itself is designed to run as a static site.

## Data flow

```text
Intervals.icu
    ↓
Browser / PWA
    ↓
IndexedDB local cache
    ↓
Replay / charts / route analysis

Optional:
WGS84 GPS
    ↓
Baidu Maps JS API Convertor
    ↓
BD-09 basemap display
```

No application backend is required for the current version.

## Privacy

HERACLES RIDE does not bundle any personal API key.

- Intervals.icu API Key is entered by the user and stored only in the local browser database.
- Baidu Maps browser AK is entered by the user and stored only in the local browser database.
- Activity caches are stored locally in IndexedDB.
- The app does not include analytics or advertising SDKs.

For public deployments, use a restricted Baidu browser AK with an appropriate Referer whitelist.

## Intervals.icu

Create / obtain an Intervals.icu API key from your own account, then paste it into the app.

The current build reads activity summaries, activity details and activity streams used for:

- GPS
- heart rate
- power
- cadence
- elevation
- speed
- time / distance

Availability of individual streams depends on the source activity and what Intervals.icu has access to.

## Baidu Maps

Baidu Maps is optional.

Create a **browser-side JavaScript API AK** in Baidu Maps Open Platform. If Referer restrictions are enabled, add your production hostname to the whitelist.

GPS tracks are expected to originate as WGS84 coordinates. Before displaying them over Baidu Maps in mainland China, the app uses Baidu's JavaScript API `Convertor` to convert sampled route points to BD-09.

If the Baidu Maps SDK or AK is unavailable, HERACLES RIDE falls back to its built-in SVG route view.

## PWA installation

### iPhone / iPad

1. Open the deployed site in Safari.
2. Tap **Share**.
3. Choose **Add to Home Screen**.

The service worker caches the application shell. Previously opened rides remain in IndexedDB for offline replay.

Baidu map tiles still require network access.

## Local backup / iCloud Drive

The app stores ride caches in IndexedDB.

Use **Export Backup** in the app, then choose **Save to Files** on iPhone/iPad. You can save the JSON backup to iCloud Drive and later restore it in HERACLES RIDE.

This is file-based backup, not automatic CloudKit synchronization.

## Deploy to GitHub Pages

This repository is static and does not require a build step.

1. Push the repository to GitHub.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the default branch and `/ (root)`.
5. Save.

`.nojekyll` is included.

After Pages is live, add the Pages hostname to the Baidu Maps Referer whitelist if required.

## Files

```text
index.html
app.js
sw.js
manifest.webmanifest
icon.svg
.nojekyll
LICENSE
README.md
```

## Current limitations

- No automatic CloudKit synchronization yet.
- No FIT / GPX file import yet.
- No automatic repeated-route / Ghost matching yet.
- Baidu Maps requires the user's own browser AK.
- This project is not affiliated with Intervals.icu or Baidu Maps.

## Roadmap

Potential next steps:

- FIT / GPX import
- repeated-route detection
- Ghost ride comparison
- route segment PB comparison
- power / HR delta overlays
- optional CloudKit private-database sync
- route library and heatmap
- ride-to-ride trend comparison

## License

MIT License.

Copyright (c) 2026 Heracles Lab
