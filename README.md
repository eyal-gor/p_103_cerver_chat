# cerver-chat

> **An AI chat for your site — cerver is the backend.** Models, streaming, sessions, memory, multi-model routing: all handled. You drop in the front-end (two lines) and skip the part everyone hates. **Don't worry about the backend.**

![cerver-chat — a drop-in AI chat for your site](https://raw.githubusercontent.com/eyal-gor/p_103_cerver_chat/main/hero.png)

```html
<script src="https://cerver.ai/embed/cerver-chat.js"></script>
<cerver-chat pk="pk_live_…"></cerver-chat>
```

## cerver is the backend for your chat

Like Stripe is the backend for payments, [cerver](https://cerver.ai) is the backend for chat — and that same backend runs **many different agents**, not just a chatbot. Other OSS chat *components* hand you a UI and say "now build your API, streaming, and storage." cerver-chat hands you the UI **and** the backend behind it:

- **You build no backend.** cerver runs the model calls, streaming, transcript store, per-visitor sessions, and routing. Two lines, and it works.
- **One backend, many agents.** Chat is one face of it. The same cerver backend runs coding agents, research agents, and scheduled jobs — switch the chat's model live today, and grow into the rest.
- **A real chat, not a toy.** Multi-model with **live switching** (Claude · GPT · Gemini · Grok · local), conversation memory, streaming.
- **Designed by an agent.** Describe the look — *"dark, rounded, like Linear"* — and an agent writes the CSS. No theming.
- **It's yours.** It runs on *your* cerver account and *your* models — not a third party's chat servers. Configure or restyle anytime; the same embed picks it up.
- **Safe to ship in client HTML.** The embed key is chat-only, domain-locked, and spend-capped.

**You'll need a free [cerver](https://cerver.ai) account** (that's the backend) — set up the chat in the dashboard, paste the two lines, done. The widget is open source (~4KB, MIT); the backend is cerver.

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

## Set it up from the terminal (skill)

A [Claude Code](https://claude.com/claude-code) skill ships with this repo — [`SKILL.md`](SKILL.md). Drop it in `~/.claude/skills/cerver-chat/` and an agent can **create, configure, design, and embed** a chat for you from the terminal — the same flow as the dashboard's UIs tab, in plain language ("add a chat to my docs site, dark and rounded, get me the embed code").

## License

MIT
