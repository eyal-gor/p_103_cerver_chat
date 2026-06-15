# cerver-chat

> Add a fully-working AI chat to your project in a few clicks — no backend, no message store, no streaming code. It runs on **your own compute**, on the subscription you already pay for.

Most "add a chatbot" tools make you build the UI, wire up streaming, store the transcript, manage keys, and pay per message. cerver-chat is the opposite: you click through a short setup, paste two lines, and you have a real chat — multi-model, streaming, with memory — running on your account.

```html
<script src="https://cerver.ai/embed/cerver-chat.js"></script>
<cerver-chat pk="pk_live_…"></cerver-chat>
```

## A few clicks to a running chat

You don't hand-write config. You set it up in the cerver dashboard:

1. **Dashboard → UIs → + New UI** — pick **Chat** and the project (app) it belongs to.
2. **Say what it should do** — one box (e.g. *"Answer questions about our docs. Be concise."*). Optionally add a name, color, knowledge, models, and allowed domains under **More settings**.
3. **Create chat & get code** — one click saves it and mints a safe embed key.
4. **Paste the two lines** on your site. Done — the chat is live.

Change anything later in the dashboard; the same snippet picks it up. No redeploy.

## What you get

- **One custom element**, ~4KB, zero dependencies. No build step.
- **Live model switching** — Claude, GPT, Gemini, Grok, local — mid-conversation; each reply is labeled with who answered.
- **Streaming** tokens, **memory** across reloads, **per-turn cost** shown.
- **Per-visitor sessions** — one customer never sees another's thread.
- **Configured server-side** — your prompt, context, and tools never live in the page source.

## Why it's different

- **Runs on your compute / subscription.** Marginal cost ≈ $0 — not metered per message like every other chat widget.
- **The key is safe in the page.** A `pk_…` key can *only* drive chat for its app, only on the domains you allow, with spend caps. So it's fine to ship in client HTML.
- **Open source.** This widget is the client; the brain (models, routing, compute) is [cerver](https://cerver.ai). You can read it, fork it, self-host the script.

## For developers

Skip the dashboard if you want — drive it with attributes:

```html
<cerver-chat
  pk="pk_live_…"
  models="Claude Sonnet:claude-sonnet-4-6,Gemini:gemini-3.1-flash"
  system-prompt="You are the Acme docs assistant."></cerver-chat>
```

Or `npm i cerver-chat` and `import "cerver-chat"` in a bundled app. With a publishable `pk`, the model list, branding, prompt, context, and tools all come from the dashboard config — attributes are just optional overrides.

Without a key it runs an **anonymous trial** session (rate-limited) — great for a quick demo.

| Attribute | What |
|---|---|
| `pk` | publishable key (from the dashboard). Omit for the anonymous trial. |
| `model` / `models` | default model / the picker list (`"Label:id,…"`) |
| `system-prompt` | prime it (server config wins when a `pk` is set) |
| `api-base` | gateway URL (default `https://gateway.cerver.ai`) |

## License

MIT
