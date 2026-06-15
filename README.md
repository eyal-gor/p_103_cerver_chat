# cerver-chat

> Drop-in AI chat for any page. One script tag. Switch models live. Runs on **local or remote agents** — your compute, your subscription, ~$0 per message.

```html
<script src="https://cerver.ai/embed/cerver-chat.js"></script>
<cerver-chat model="claude-haiku-4-5-20251001"></cerver-chat>
```

That's the whole install. You get a streaming chat with **live model switching**, conversation memory, and per-turn cost — no backend, no message store, no streaming code to write.

## Why

Every other chat widget meters you per message. This one runs on [cerver](https://cerver.ai): agents on compute you own, on the subscription you already pay for. Marginal cost: ~zero. And you can switch models *mid-conversation* — Claude → GPT → Gemini → Grok → a local model — because that's the point of running through cerver, not one provider.

## What you get

- **One custom element.** No build step, no framework, ~4KB, zero dependencies.
- **Live model switching** — a dropdown, mid-thread; each reply is labeled with who answered.
- **Streaming** tokens (SSE).
- **Memory** — conversation history persists across reloads, threaded back into each turn.
- **Per-turn cost**, shown.
- **Per-visitor sessions** — multi-tenant; one customer never sees another's thread.

## Install

Script tag (above), or via npm for bundled apps:

```bash
npm i cerver-chat
```
```js
import "cerver-chat";   // registers the <cerver-chat> element
```

## Configure

| Attribute | What it does |
|---|---|
| `model` | default model id (e.g. `claude-haiku-4-5-20251001`) |
| `models` | the picker list — `"Claude:claude-…,Gemini:gemini-…"` |
| `system-prompt` | prime it — e.g. paste your docs to make a docs assistant |
| `pk` | a publishable cerver key (production). Omit → anonymous trial session |
| `api-base` | gateway URL (default `https://gateway.cerver.ai`) |

## Examples

**Chat with your docs**
```html
<cerver-chat system-prompt="You answer questions about Acme's API. Be concise."></cerver-chat>
```

**Pin the model menu**
```html
<cerver-chat models="Claude Sonnet:claude-sonnet-4-6,Gemini:gemini-3.1-flash,Grok:grok-4"></cerver-chat>
```

## Auth & cost

- **Anonymous (default):** no key needed — the widget mints a trial session. Great for demos and docs; rate-limited.
- **Production:** pass a publishable `pk` bound to your cerver app. Sessions then run on your account/compute and show in your dashboard. Set spend caps on the key.

A non-Claude model only works if that provider's key is configured on the account in use.

## License

MIT
