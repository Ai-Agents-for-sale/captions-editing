# captions-editing

Telegram Mini App for correcting Submagic subtitles on already-edited ZeroWork clips.

Static single-page app (GitHub Pages). n8n is the backend.

## How it works

Opened from the Tinder clip app with URL params:

| Param | Meaning |
|-------|---------|
| `projectId` | Submagic project id (premium) or clip id (shorts) — parsed from the Dropbox filename |
| `dropboxPath` | full Dropbox path of the clip (for the video player + file replace) |
| `chatId` | client's Telegram chat id (needed so the re-render notification reaches them) |
| `api` | optional override of the n8n webhook base (default: `https://aiagentsforsale.app.n8n.cloud/webhook`) |

Flow:
1. On load → `POST {api}/subtitle-fetch {projectId, dropboxPath}` → `{ words, videoUrl }`.
2. Video plays on top; words are tappable/editable below.
3. "שלח לעריכה" → `POST {api}/subtitle-update {projectId, dropboxPath, chatId, words}`.
4. n8n updates Submagic captions, re-renders, replaces the Dropbox file, and notifies the client on Telegram.

n8n workflow: `cw3JCM9Zjx0FZXKr` ("Subtitle Editor — Submagic ↔ Dropbox").
