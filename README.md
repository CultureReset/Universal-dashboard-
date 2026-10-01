# App catalog

The list of apps every workspace sees in the **App Store** inside Paperclip.
The store (the Paperclip plugin in
[`CultureReset/saas-cybercheck`](https://github.com/CultureReset/saas-cybercheck))
reads [`catalog.json`](catalog.json) from this repo's `main` branch. Change this
file and every workspace's store changes within a minute.

```json
{ "apps": [ { "id": "faq", "version": "1.0.0", "name": "FAQ", ... }, ... ] }
```

Each entry is a **Modular app manifest**, the same format as
`CultureReset/App-build-` (`src/lib/modules/spec.ts`): the fields, lists,
settings and permissions an app has. Apps are descriptions, never code, so one
app cannot break another or reach outside its own data.

The first ten apps are Modular's built-in apps (Listings, Action Buttons,
Social Links, Enquiry Form, Gallery, Video, FAQ, Link Hub, QR Menu, Song
Requests), copied from `App-build-/src/modules/*/manifest.ts`.

## Publish a new app

Add an entry to `apps`. It needs a unique `id` (kebab-case), a `version`
(`1.0.0`), and the rest of the manifest. It shows up in every workspace's Store
tab, ready to add.

## Push an update to everyone

Change the app's entry and **raise its `version`** (`1.0.0` → `1.1.0`). Every
workspace that has the app sees **Update available**. If the new version asks
for a permission the workspace has not granted, the owner has to accept it
before the update applies; nothing is widened silently.

## Take an app out of the store

Delete its entry. Workspaces that already have it keep it at the version they
have, marked "no longer in the store, still works". New workspaces can no
longer add it.

## Check before you publish

Every push runs [`check.yml`](.github/workflows/check.yml), which validates the
whole file with the store's own checker. Locally:

```bash
git clone https://github.com/CultureReset/saas-cybercheck ../saas-cybercheck
node ../saas-cybercheck/dist/check-catalog.js catalog.json
```

A broken entry is never shown: the store skips it and keeps the rest, so one
bad edit cannot take the store down.
