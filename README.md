# 🎬 Advanced Video Effects Generator

> Chrome extension — Generate MP4 videos with cinematic transitions, Ken Burns effects and zoom from your photos, entirely in your browser.

[![Chrome Web Store](https://img.shields.io/badge/Chrome%20Web%20Store-Available-4285F4?logo=googlechrome&logoColor=white)](https://chrome.google.com/webstore)
[![Manifest V3](https://img.shields.io/badge/Manifest-V3-green)](https://developer.chrome.com/docs/extensions/mv3/)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)
[![Languages](https://img.shields.io/badge/Languages-13-orange)](/_locales)

---

## ✨ Features

- **16 cinematic transitions** — Crossfade, 360° rotation (CW/CCW), slide (4 directions), zoom in/out, circle wipe, dissolve, cube, swirl, flip (H/V)
- **9 zoom effects** — Ken Burns in/out, pan (4 directions), pulse, slow rotation
- **Showcase mode** — automatically alternates all transitions and zooms
- **Overlay text** — custom font, color, size and position (top / center / bottom)
- **Optional audio** — MP3, AAC, M4A, OGG, WAV, FLAC, WEBM with optional loop
- **Direct MP4/H.264 encoding** via WebCodecs — no intermediate WebM, no server
- **Drag & drop** image reordering
- **13 languages** — FR, EN, ES, PT, DE, IT, NO, RU, UK, ID, HI, ZH, AR
- **100% local** — your files never leave your device

---
### Rendering pipeline

```
User images (File[])
       │
       ▼
createImageBitmap()          — no resize options (Chrome extension compat)
       │
       ▼
OffscreenCanvas resize       — scale to target resolution (e.g. 1280×720)
       │
       ▼
EffectsEngine.getZoomFrame() — Ken Burns, pan, pulse, rotation…
       │
       ▼
EffectsEngine.getTransitionFrame() — crossfade, cube, swirl…
       │
       ▼
OffscreenCanvas[]            — zero getImageData/putImageData
       │
       ├──► WebCodecs (H.264) ──► MP4  ✅ primary path
       └──► MediaRecorder     ──► WebM ⚠️  fallback
```

### Key design decisions

| Decision | Reason |
|---|---|
| `OffscreenCanvas` only, no `getImageData` | Avoids alpha channel bugs in Chrome extensions |
| `createImageBitmap` without resize options | More reliable in extension context |
| WebCodecs as primary encoder | Direct MP4/H.264, no intermediate container |
| `localStorage` for preferences | No server, no tracking, GDPR-friendly |

---

## ⚙️ Configuration

All parameters are adjustable from the UI:

| Parameter | Range | Default |
|---|---|---|
| Resolution | 854×480 → 3840×2160 | 1280×720 |
| FPS | 24 / 30 / 60 | 30 |
| Duration per image | 1s → 10s | 3s |
| Transition duration | 0.2s → 2s | 0.8s |
| Zoom factor | 1.0× → 2.0× | 1.3× |
| Overlay text size | 1% → 10% | 4% |

---

## 🌐 Internationalization

The extension supports 13 languages via Chrome's native i18n system (`_locales/`).  
The UI language follows the user's browser language automatically.

| Code | Language | Code | Language |
|---|---|---|---|
| `fr` | Français | `ru` | Русский |
| `en` | English | `uk` | Українська |
| `es` | Español | `id` | Indonesia |
| `pt` | Português | `hi` | हिन्दी |
| `de` | Deutsch | `zh_CN` | 中文 |
| `it` | Italiano | `ar` | العربية |
| `no` | Norsk | | |

---

## 🔒 Privacy

This extension is **fully local**. It does not collect, transmit or store any personal data on external servers.

- ✅ No analytics, no tracking, no cookies
- ✅ Images and videos never leave your device
- ✅ Preferences stored locally via `localStorage` only
- ⚠️ The optional contact form transmits your email via [Web3Forms](https://web3forms.com) — voluntary only

**Permissions used:**

| Permission | Reason |
|---|---|
| `downloads` | Trigger the video download to your device |
| `tabs` | Open the extension UI in a new tab |
| `https://api.web3forms.com/*` | Optional contact form only |

→ [Full Privacy Policy](https://YOUR_USERNAME.github.io/video-effects-generator/privacy-policy.html)

---

## 🛠️ Development

### Prerequisites
- Google Chrome 110+
- No build step required — pure vanilla JS

### Local development
```bash
# Load the extension in developer mode
# chrome://extensions/ → Load unpacked → select project folder

# Edit files and click the reload button in chrome://extensions/
# No hot reload — manual reload required after each change
```

### Adding a new transition

1. Add the key to `TransitionType` in `engine.js`
2. Add a `case` in `EffectsEngine.getTransitionFrame()`
3. Add the UI radio button in `popup.html`
4. Add the translation key `tr_yourkey` in all 13 language blocks in `popup.js`

### Adding a new language

1. Create `_locales/XX/messages.json` (max 132 chars for `appDescription`)
2. Add the translation object in `TRANSLATIONS` in `popup.js`
3. Add the flag button in `popup.html`
4. Add `setLanguage('XX')` handler

---

## 📦 Build & Package

```bash
# Create the ZIP for Chrome Web Store submission
# Include everything EXCEPT: .git/, node_modules/, *.md, .DS_Store

zip -r video-effects-generator.zip . \
  --exclude "*.git*" \
  --exclude "*.DS_Store" \
  --exclude "README.md" \
  --exclude "*.zip"
```

> ⚠️ The ZIP must contain files at the root, not inside a subfolder.

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/new-transition`)
3. Commit your changes (`git commit -m 'Add swirl-2 transition'`)
4. Push to the branch (`git push origin feature/new-transition`)
5. Open a Pull Request

---

<p align="center">Made with ❤️ — <a href="https://chrome.google.com/webstore">Available on Chrome Web Store</a></p>
