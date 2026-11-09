# Extension Popup Dashboard Sync & Logo Icon Update

Sync the Extension Popup modal directly from Dashboard storage (`chrome.storage.local`), remove independent external API calls from `extension/popup.js`, update the popup "↻ Refresh" button to re-sync from Dashboard state, and replace the popup logo icon with the official Dashboard crescent moon brand logo (`icons/app-logo.png`).

---

## User Review & Critical Decisions

> [!IMPORTANT]
> 1. **Direct Dashboard Sync (No Independent External API Fetch in Popup)**:
>    - Remove independent external `fetch()` calls in `extension/popup.js`.
>    - On popup open and on clicking **↻ Refresh**, read pre-scanned `PRAYER_FOCUS_TIMES`, `PRAYER_FOCUS_LOCATION`, `PRAYER_FOCUS_OFFSETS`, `PRAYER_FOCUS_OVERRIDES`, and `PRAYER_FOCUS_COMPLETED` directly from `chrome.storage.local`.
>    - Display provider badge as `Dashboard Live Sync · Online`.
> 
> 2. **Extension Popup Logo Icon Update**:
>    - In `extension/popup.html`, replace the header SVG with the official crescent moon brand logo `<img src="icons/app-logo.png" width="24" height="24" />`.
>    - In `src/components/MiniWidget.tsx`, replace the inline SVG icon with `<img src="./icons/app-logo.png" className="w-5 h-5 object-contain" />`.

---

## 1. Overview & Core Concept

- **100% Dashboard Synchronization**: The Chrome Extension popup acts as a lightweight view mirror of the Dashboard. When you adjust timings or refresh in the Dashboard, the popup reflects those exact scanned API timings and minute offsets without making redundant network calls.
- **Brand Identity**: The Chrome Extension popup header displays the exact crescent moon logo matching the main Dashboard.

---

## 2. Technical Architecture & Component Changes

1. **`extension/popup.html`**:
   - Update `.logo-icon-wrap` to render `<img src="icons/app-logo.png" alt="Prayer Focus" style="width:24px; height:24px; object-fit:contain;" />`.

2. **`extension/popup.js`**:
   - Update `getStoredState()` to retrieve `PRAYER_FOCUS_TIMES`, `PRAYER_FOCUS_LOCATION`, `PRAYER_FOCUS_OFFSETS`, `PRAYER_FOCUS_OVERRIDES`, and `PRAYER_FOCUS_COMPLETED`.
   - Update `refreshBtn` listener to re-query `chrome.storage.local` and instantly re-render the view with the latest Dashboard times.

3. **`src/components/MiniWidget.tsx`**:
   - Replace inline header SVG with `<img src="./icons/app-logo.png" className="w-5 h-5 object-contain" alt="Prayer Focus" />`.

4. **Re-pack Extension Package**:
   - Run `npm run build && node scripts/pack-extension.js` to update `Prayer-Focus-v2.2.23-Fixed.zip`.
