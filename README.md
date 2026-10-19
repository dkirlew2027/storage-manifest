# Storage Manifest

A phone-first app for logging what goes into storage. Point your phone's camera at an item, Claude identifies and categorizes it, you file it into a box, and later you can find anything by searching or by snapping a photo of what you're looking for.

**Live app:** https://dkirlew2027.github.io/storage-manifest/

## What it does

- **Live camera scanning.** Tap the shutter to log an item. Lay out a box's contents and one photo logs each item separately.
- **Categories built for storage.** 16 category codes (KIT, TLS, HOL and so on), a confidence score, other possibilities when it isn't sure, and a packing tip for each item.
- **Box suggestions.** Items are pointed to the box that already holds their category, or the box you're filling. New boxes get labels like `KIT-02`.
- **Closest-match search.** Text search tolerates typos ("skilet" finds the skillet) and ranks results by how close they are. **Match photo** compares a picture against your whole log and shows where the closest items are stored.
- **Boxes view.** Boxes grouped by where they're kept, with a packing list you can copy.
- **Works offline.** After the first visit the app opens with no signal, which helps at a storage unit. Identification still needs a connection.
- **Export.** CSV for spreadsheets or pandas, and a full JSON backup with photos.

## Setup

1. **Turn on GitHub Pages.** In this repo go to **Settings > Pages**, set **Source** to *Deploy from a branch*, pick `main` and `/ (root)`, then save. The site is live at the link above a minute or two later.
2. **Open it on your phone** and add it to your home screen (Safari: Share > Add to Home Screen). It then opens full screen like an app, and iOS treats its storage more reliably.
3. **Add a Claude API key.** Create one at [platform.claude.com/settings/keys](https://platform.claude.com/settings/keys), then paste it in the app under **Settings > Claude API key**. Setting a monthly spend limit in the Console is a good idea. Without a key the app still works, and you name items yourself.

## Where your data lives

- **Your log and photos** are stored in your browser on your phone (IndexedDB). Nothing is uploaded to GitHub, and this repo only contains the app's code.
- **Your API key** is stored in your browser on your phone. It is never part of the code. Photos are sent to the Claude API only when you identify or match one.
- **Back up regularly.** Browsers can clear site data. Use **Settings > Save backup file** and keep the file in Files or iCloud. **Restore from file** brings it back on any device.

## Cost

Identification uses `claude-haiku-5-5` by default, which costs a fraction of a cent per photo. **Careful mode** switches to `claude-sonnet-5-5`, which is slower and costs more but reads labels and brands more closely. Both model names can be changed under **Settings > Models**.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: layout, styles, and code in one file |
| `sw.js` | Service worker that lets the app open offline |
| `manifest.webmanifest` | Name and icons for adding to the home screen |
| `icons/` | Home screen and browser icons |
| `.nojekyll` | Tells GitHub Pages to serve files as they are |

## Run it locally

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. Camera access works on `localhost` and on HTTPS sites like GitHub Pages.

## Working with the CSV in Python

```python
import pandas as pd

log = pd.read_csv("storage-manifest-2026-10-18.csv")
print(log.groupby(["Box", "Category"]).size())
```
