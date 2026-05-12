# My YT Bookmarks

A Chrome extension that lets you save and revisit timestamps in any YouTube video — bookmarks persist across sessions using Chrome's sync storage.

## Features

- **Bookmark any moment** — adds a bookmark button directly into the YouTube player controls
- **Jump to timestamp** — click play on any saved bookmark to seek to that point instantly
- **Delete bookmarks** — remove individual bookmarks from the popup
- **Per-video storage** — bookmarks are saved per video ID and synced across Chrome sessions

## File Structure

```
├── manifest.json        # Extension config (permissions, entry points)
├── background.js        # Detects YouTube video navigation, notifies content script
├── contentScript.js     # Injects bookmark button into YouTube player
├── popup.html           # Extension popup UI
├── popup.css            # Popup styles
├── popup.js             # Renders and manages bookmarks in the popup
├── utils.js             # Helper to get the active tab
└── assets/
    ├── bookmark.png     # Bookmark button icon (injected into player)
    ├── play.png         # Play/seek icon
    ├── delete.png       # Delete icon
    └── ext-icon.png     # Extension toolbar icon
```

## Installation

This extension is not on the Chrome Web Store — load it manually as an unpacked extension:

1. Clone or download this repository
2. Open Chrome and go to `chrome://extensions`
3. Enable **Developer mode** (toggle in the top right)
4. Click **Load unpacked** and select the project folder
5. Navigate to any YouTube video — the bookmark button will appear in the player controls

> Make sure the `assets/` folder contains all required icons before loading.

## How It Works

| Component | Role |
|---|---|
| `background.js` | Listens for tab URL changes; when a YouTube watch URL is detected, sends a `NEW` message with the video ID to the content script |
| `contentScript.js` | Injects a bookmark button into the YouTube player. Handles `NEW` (load bookmarks), `PLAY` (seek video), and `DELETE` (remove bookmark) messages |
| `popup.js` | Reads bookmarks for the current video from `chrome.storage.sync` and renders them with play/delete controls |

Bookmarks are stored as a JSON array keyed by video ID in `chrome.storage.sync`, so they persist across browser sessions and sync across devices if Chrome sync is enabled.
