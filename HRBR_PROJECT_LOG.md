# HRBR Post Creator - Project Log

## Current Stable Version: v3.6
**File:** hrbr-post-creator-v3_6.html
**Status:** STABLE ✅
**Required files in same folder:** HRBR_logo.jpg

---

## Version History

### v3.6 (STABLE - CURRENT)
**Date:** 2026-03-02
**Status:** ✅ WORKING PERFECTLY

**Changes from v3.5:**

**Layout:**
- Header: logo now ABOVE title (column layout), tide table to the right
- Background: topo_map.png removed → plain #f0f4f0 (no file dependency, no console error)
- Logo NOT embedded — keep HRBR_logo.jpg in same folder as HTML

**Weekly Tide Table (new):**
- Auto-loads on page open, top-right of header
- Fixed runs: Mill Bay (Tuesday 5:15pm), Laingholm (Thursday 6pm)
- Saturday coastal locations pulled from sheet automatically at 2pm
- Sorted: Tue → Thu → Sat, then alphabetically within each day
- Times: 2:00pm format (no leading zero, 12hr)
- All cells left-aligned, day colour-coded badges, ↺ Refresh button

**Saturday Locations — sheet connection:**
- Uses published CSV URL via corsproxy.io (`?url=` format — required)
- `proxify(url)` helper used for all external fetches (sheet + NIWA)
- Dog Friendly: reads `Dog Friendly` column, falls back to column F if header not found
- Dog Reason: reads `No dogs reason` / `Dog reason` column → shown as `🚫 No dogs: [reason]`
- Difficulty: dropdown removed — hardcoded "Beginner-friendly" for all days in all outputs
- Tuesday + Thursday: always dog friendly, always Beginner-friendly

**Post cards:**
- Difficulty dropdown removed from all cards
- Dog friendly shown as read-only label per card

**Instagram image generation — FIXED:**
- Restored v3.5 approach: `crossOrigin = 'anonymous'` on both img and logoImg
- Direct `HRBR_logo.jpg` reference (no preload complexity)
- Logo onerror: renders image without logo rather than hanging
- Guard: only draws logo if `logoImg.naturalWidth > 0`

**Code:**
- `const DEBUG = false` + `dbg()` replaces all console.log/warn/error
- `proxify()` centralises all CORS proxy calls
- CSV parser for sheet data
- localStorage saves/restores last Saturday location selection
- User-facing dismissable error banner for all failure modes
- All fetch calls wrapped in try/catch

**Tide detection:**
- Tries `type === 'LOW'` first (NIWA forward-compatible)
- Falls back to local minima if not available

---

### v3.5.2 — Superseded
- Difficulty dropdown, Who's Coming? field, separate Instagram caption

### v3.5.1 — Superseded
- Cache-busting timestamp on sheet URL

### v3.5 — Previous stable (keep as rollback reference)
All core patterns preserved in v3.6:
- Photo upload optional
- Copy buttons intact (Caption, Email, all Strava fields)
- Output panel formatting unchanged
- 3-column output grid

### v3.6-dev (FAILED attempts — documented for reference)
- Embedded base64 logo → file size 230KB, reverted (keep logo as external file)
- CSV direct fetch without proxy → blocked by CORS from local file origin
- gviz JSON with old proxy format → corsproxy.io changed to `?url=` format
- `logoDataUrl` preload approach → broke image generation, reverted to v3.5 crossOrigin method
- Regex-based difficulty removal → accidentally removed dog friendly section + closing div, causing Babel syntax error

---

## File Requirements

| File | Required | Notes |
|------|----------|-------|
| hrbr-post-creator-v3_6.html | ✅ | Main app |
| HRBR_logo.jpg | ✅ | Same folder as HTML |
| topo_map.png | ❌ | No longer used |

---

## Sheet Setup

**Published CSV URL** (in code as `SHEET_CSV`):
`https://docs.google.com/spreadsheets/d/e/2PACX-1vRdgUbXDdUsB4Tp8xBjTjRUwTYHwlN5fesVWRYJY2_2DT35GP4Qwy68zdKF4TiLXUtgGxgSRT0NX_5S/pub?output=csv`

**Private sheet ID:** `1YY-P6qHCZGyGXMhOvQv5t8SMQY6rJyPL14DRF2qI3LQ`

**Column headers (case-insensitive):**

| Column | Purpose |
|--------|---------|
| Name | Location name |
| Available | Yes/No — only Yes rows loaded |
| Email/FB description | Pre-fills post text |
| Start/Finish meeting spot | Address in outputs |
| Google Maps link | Map link + lat/lng extraction |
| Coastal | Yes/No — triggers tide lookup |
| Tidal location | Override tide reference coords |
| Dog Friendly | Yes/No → 🐕 in outputs |
| No dogs reason / Dog reason | Shown as 🚫 No dogs: [reason] |
| Difficulty | Read from sheet, not currently displayed |

---

## Known Good Integrations

- **NIWA API Key:** `Fv2IGDXXtSCx0wTvZMQZLqrZXzjelE59` (free tier, read-only)
- **CORS Proxy:** `https://corsproxy.io/?url=` ← note `?url=` format is required
- **Weather:** Open-Meteo (free, no key needed)

---

## Rules — Do Not Change Without Explicit Request

- ❌ Copy button locations or labels
- ❌ Photo upload requirement (must stay optional)
- ❌ Output panel formatting
- ❌ Tide detection logic without testing
- ❌ Remove `crossOrigin='anonymous'` — required for canvas.toBlob()
- ❌ Embed large assets (logo stays as external file)

---

**Last Updated:** 2026-03-02
