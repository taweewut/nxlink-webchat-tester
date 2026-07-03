# NXLINK WebChat Tester (generic POC)

A **tenant-agnostic**, single-page test harness for driving any NXLINK webchat bot in a
real browser. Paste a widget **JWT**, save it under a name for reuse, and launch the bot —
**no login, fully anonymous**. Everything runs client-side; nothing is uploaded.

**Live page:** _(enabled via GitHub Pages — see the repo's Pages settings)_

## What it does

- **Save JWTs by name** — profiles are kept in your browser's `localStorage` only.
- **Decode the JWT** — shows `tenant_id`, `config_id`, config name, and expiry at a glance.
- **Launch the embedded widget** — injects the exact NXLINK embed:
  ```js
  const s = document.createElement('script');
  s.id  = 'live-chat-script';
  s.src = 'https://<host>/chatbot/client/js/live_chat.min.js?jwt=<JWT>&vtime=' + Date.now();
  document.head.appendChild(s);
  ```
  The widget starts anonymous and collects identity in its own pre-chat form.
- **Chat Proxy (optional)** — open a stable full-page session that survives refresh
  (`…/NXAI_ChatProxy/chat/start?jwt=&name=&phone=`), for identity-based re-attach tests.

## Config, not code

Nothing tenant-specific is hard-coded. Per bot you supply:

| Field | What it is |
|---|---|
| JWT | non-secret visitor token (encodes `tenant_id` / `config_id`) |
| Client script URL | NXLINK `live_chat.min.js` base (host/region differs per deployment) |
| Proxy start URL | optional `…/NXAI_ChatProxy/chat/start` endpoint |
| Name / Phone | optional identity for the Chat Proxy only |

Save all of the above together as a **profile** and pick it from the dropdown next time.

## Security

- **No secrets are committed to this repo.** JWTs are pasted at runtime and stored only in
  your browser. The default client-script URL is a public loader, not a credential.
- Widget JWTs are non-secret visitor tokens by design, but **use non-production tenants**
  for testing — proxy URLs carry the JWT/name/phone as query params (visible in history).

## Run locally

Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Deploy

Static single file — host anywhere. This repo publishes `index.html` at the repo root via
**GitHub Pages** (Settings → Pages → Deploy from branch → `main` / root).
