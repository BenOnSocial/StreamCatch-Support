# StreamCatch Chrome Extension

Catches M3U8 manifest and subtitle links (VTT, SRT) from network requests on web pages.

## Features

*   Detects `.m3u8` playlist URLs requested by the page.
*   Detects common subtitle file URLs (`.vtt`, `.srt`) requested by the page.
*   Parses detected M3U8 master playlists to find subtitle links declared within them (`#EXT-X-MEDIA TYPE=SUBTITLES`).
*   Displays found links conveniently in the extension popup, separated by type (Manifests / Subtitles).
*   Provides "Copy" buttons for easy URL copying.
*   Shows a badge count of detected manifests on the extension icon for the active tab.
*   Allows clearing the list for the current tab.
*   Data is stored per-tab and cleared when the tab is closed, navigated away, or the browser session ends (`chrome.storage.session`).

## Installation (Development)

1.  Clone this repository or download the source code as a ZIP file and extract it.
2.  Open Google Chrome.
3.  Go to `chrome://extensions/`.
4.  Enable "Developer mode" using the toggle switch in the top-right corner.
5.  Click the "Load unpacked" button.
6.  Navigate to the `StreamCatch/` directory (the one containing `manifest.json`) and select it.
7.  The StreamCatch extension icon should appear in your Chrome toolbar.

## How to Use

1.  Navigate to a web page containing an HLS video stream (using M3U8).
2.  Play the video if necessary (sometimes links are only loaded on playback).
3.  If manifests are detected, a number badge will appear on the StreamCatch icon.
4.  Click the StreamCatch icon in your toolbar.
5.  The popup will display any caught M3U8 manifest and subtitle links found for that tab.
6.  Use the "Copy" button next to each link to copy it to your clipboard.
7.  Use the "Clear List" button to remove all links shown for the current tab.

## Permissions Required

*   **Read and change all your data on all websites (`<all_urls>`):** Necessary for the `webRequest` API to intercept network requests from *any* site potentially hosting video streams. This allows the extension to work broadly without knowing specific video sites in advance. During Chrome Web Store review, this broad permission requires clear justification.
*   **storage:** Used to temporarily store the list of found URLs associated with each browser tab.
*   **tabs:** Used to identify which tab a network request came from and to update the badge text specific to a tab.

## Limitations

*   May not detect all methods of loading subtitles (e.g., complex JavaScript fetching without standard file extensions or manifest entries).
*   Will likely **not** work for DRM-protected streams (Widevine, FairPlay) as decryption keys are handled securely. You might see the manifest, but it won't be playable externally.
*   Performance on pages with extremely high numbers of network requests hasn't been heavily optimized.
*   Relies on URL patterns (`.m3u8`, `.vtt`, `.srt`) and basic manifest parsing. It might miss streams served via URLs without these extensions unless Content-Type sniffing were added (more complex).
