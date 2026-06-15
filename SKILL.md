---
name: cerver-chat
description: Set up and manage an embeddable AI chat (cerver-chat) on a project from the terminal — create a chat UI on an app, configure its prompt/models/look, design it with an agent, mint a publishable embed key, and produce the two-line snippet. Use when the user types /cerver-chat or asks to "add a chat to my site/app", "embed a chat", "create a chat UI", "configure my chat", "restyle/design my chat", or "get the embed code". Drives the cerver gateway API with the local cerver key.
---

# cerver-chat — embeddable AI chat, from the terminal

cerver-chat is a drop-in `<cerver-chat>` web component that runs on the user's own
cerver account (their compute/subscription, ~$0/message). A chat is a **UI attached
to an app** (project). This skill does, from the terminal, what the dashboard's
**UIs** tab does in clicks.

## Setup (resolve once)
- **Key:** read `CERVER_API_KEY` from `~/.cerver/cerver.env` (the owner `ck_…` key).
  Missing? Tell the user: `curl -fsSL https://cerver.ai/install.sh | bash`, then `cerver login`.
- **Gateway:** `https://gateway.cerver.ai`. Auth header: `Authorization: Bearer $CERVER_API_KEY`.
- Use **`curl`** for API calls (Cloudflare 1010-blocks python-urllib's user-agent).

## 1 — pick the app
A chat attaches to an app. `GET /v2/apps` lists them; use the slug the user names.
No app yet? `POST /v2/apps {"name":"...","slug":"..."}`.

## 2 — configure (the brain + the look)
Read current: `GET /v2/apps/<slug>/chat-config`.
Save: `PUT /v2/apps/<slug>/chat-config` with:
```json
{
  "system_prompt": "what it does — e.g. Answer questions about Acme. Be concise.",
  "context": "optional docs / facts / FAQ stuffed into every turn",
  "models": [{"label":"Claude Haiku","id":"claude-haiku-4-5-20251001"}],
  "branding": { "name": "Acme", "accent": "#d2588f", "css": "" }
}
```
- Models to choose from (first = default): `claude-haiku-4-5-20251001`, `claude-sonnet-4-6`,
  `claude-opus-4-8`, `gemini-3.1-flash`, `grok-4`.
- `system_prompt` + `context` stay **server-side** — never in the page source.

## 3 — design it with an agent (optional)
To restyle the chat from a plain-English description, run an agent on cerver:
1. `POST /v2/sessions {"session_type":"chat","workload":"general"}` → `session_id`.
2. `POST /v2/sessions/<sid>/run-llm {"input":"<design prompt>","model":"claude-sonnet-4-6"}`.
   It streams SSE — accumulate the `content` from each `event: text_delta`, then strip any
   ``` fences. That text is the CSS.
3. Save it into `branding.css` via the chat-config `PUT` above, then re-mint isn't needed
   (the live widget reads branding on load).

Design prompt template:
> You are a UI designer writing CSS for a chat web component. Restyle it to look like
> "<DESCRIPTION>". Target these shadow-DOM classes: `.wrap` (container), `.head` (header),
> `.head select` (model picker), `.head .cost`, `.msgs` (message list), `.turn`, `.turn .who`,
> `.turn.user .who`, `.turn .body`, `.row` (input row), `.row input`, `.row button` (send),
> `.foot`. The accent is `var(--accent)`. Return ONLY raw CSS using those exact class names.

## 4 — mint the embed key + give the snippet
`POST /v2/auth/keys {"kind":"publishable","app_slug":"<slug>","allowed_origins":["acme.com"]}`
(omit/empty `allowed_origins` = works on any domain while testing). Returns `{"key":"pk_…"}`.
The `pk_` is **client-safe**: chat-only, domain-locked, spend-capped. Hand the user:
```html
<script src="https://cerver.ai/embed/cerver-chat.js"></script>
<cerver-chat pk="pk_…"></cerver-chat>
```

## Notes
- Everything here is also clickable at **cerver.ai/dashboard → UIs**. This skill is the
  agent/terminal path to the same thing.
- The OSS widget repo: https://github.com/eyal-gor/p_103_cerver_chat

## Don't
- Don't print the owner `ck_` / `CERVER_API_KEY` back to the user. The `pk_` is fine to show
  (it's meant to be public).
- Never put a `ck_` key in the embed snippet — always mint a publishable `pk_`.
