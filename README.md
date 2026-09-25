# 🎧 BeatPulse AI

A premium, white-labeled music discovery platform. Recognize songs from audio, identify artists from photos, search & play any track, and chat with an AI music assistant — all wrapped in membership tiers with daily limits.

## ✨ Features

| Feature | Description |
|---|---|
| 🎤 Song Recognition | Record a clip (max 30s) — the AI identifies title, artist, album, year and confidence. |
| 📸 Artist Lens | Upload a photo of an artist, poster or album cover — the AI identifies who it is. |
| 🔍 Search & Play | Search by song name, lyric fragment or vibe. Embedded player starts playback instantly; VIP+ can download MP3s via yt-dlp. |
| 💬 PulseChat | AI music assistant for chords, theory, recommendations and history. PRO gets daily standard chat, KING gets the premium engine with unlimited messages. |
| 💎 Membership Tiers | FREE, VIP, PRO and KING plans with per-day limits enforced server-side. |

## 🧱 Tech Stack

- **Backend:** Node.js, Express, MongoDB (Mongoose), JWT auth (bcrypt-hashed passwords)
- **Frontend:** Vanilla HTML / CSS / JS (single-page, responsive, premium dark UI)
- **AI:** Google Gemini API — Flash tier for standard features, Pro tier for KING chat
- **Downloads:** [yt-dlp](https://github.com/yt-dlp/yt-dlp) (optional, server-side)

## 🚀 Quick Start

```bash
# 1. Install dependencies
npm install

# 2. Configure environment
cp .env.example .env
#    - set MONGODB_URI
#    - set JWT_SECRET
#    - set GEMINI_API_KEY (get one free at https://aistudio.google.com)

# 3. (Optional) Install yt-dlp for MP3 downloads
#    macOS:   brew install yt-dlp
#    Linux:   sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && sudo chmod a+rx /usr/local/bin/yt-dlp
#    Windows: winget install yt-dlp

# 4. Start MongoDB, then run
npm run dev        # → http://localhost:3000
```

> No AI key? The app still runs — the server returns clearly-marked **demo responses** so you can evaluate the full UX end-to-end. Add a key later to go live.

## 💎 Plan Limits

| Feature | FREE | VIP | PRO | KING |
|---|---|---|---|---|
| Searches / day | 20 | ∞ | ∞ | ∞ |
| Recognitions / day | 3 | 15 | 50 | ∞ |
| Artist Lens / day | 3 | 15 | 50 | ∞ |
| PulseChat | 🔒 | 🔒 | 30/day (standard) | ∞ (premium engine) |
| MP3 downloads / day | 🔒 | 50 | 200 | ∞ |

> Checkout is a **demo instant-activation** (`POST /api/auth/upgrade`) — swap in Stripe/Paddle/etc. for production.

## 📁 Structure

```
beatpulse-ai/
├── server.js            # Express entry point
├── config/db.js         # MongoDB connection
├── models/User.js       # user + plan + daily-usage logic
├── middleware/auth.js   # JWT guard + tier guard
├── utils/gemini.js      # AI provider client (server-side only)
├── routes/
│   ├── auth.js          # register / login / me / plans / upgrade
│   ├── recognize.js     # audio → song identification
│   ├── artist.js        # photo → artist identification
│   ├── search.js        # lyric/name → track + embed + yt-dlp download
│   └── chat.js          # tier-gated AI chat (PRO/KING)
└── public/              # white-labeled SPA (no AI provider references)
    ├── index.html
    ├── css/style.css
    └── js/app.js
```

## 🌐 Frontend Playback & Downloads

- Playback uses an embedded player with provider-side search, so **no extra API key is needed** for audio/video playback.
- Downloads are proxied through `GET /api/download?q=...` (VIP+), which runs `yt-dlp -x --audio-format mp3` on the server.

## ⚠️ Notes

- **Microphone recording requires HTTPS** (or `localhost`) in modern browsers.
- Vision models may decline to identify a person from a face photo; Artist Lens will then describe the image instead.
- Only download content you have the rights to; respect artists and copyright.
- The frontend is fully white-labeled — the AI provider is referenced **only** in server code and `.env`.
