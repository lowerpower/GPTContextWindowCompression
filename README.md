# GPTContextWindowCompression
<p align="center">
  <!-- Latest release version (needs a Release) -->
  <a href="https://github.com/lowerpower/GPTContextWindowCompression/releases">
    <img alt="Release" src="https://img.shields.io/github/v/release/lowerpower/GPTContextWindowCompression?sort=semver">
  </a>
  <!-- License badge (reads LICENSE file) -->
  <a href="https://github.com/lowerpower/GPTContextWindowCompression/blob/main/LICENSE">
    <img alt="License" src="https://img.shields.io/github/license/lowerpower/GPTContextWindowCompression">
  </a>
  <!-- Stars (social proof) -->
  <a href="https://github.com/lowerpower/GPTContextWindowCompression/stargazers">
    <img alt="GitHub stars" src="https://img.shields.io/github/stars/lowerpower/GPTContextWindowCompression">
  </a>
  <!-- CI badge (if you add .github/workflows/json-lint.yml) -->
  <a href="https://github.com/lowerpower/GPTContextWindowCompression/actions/workflows/json-lint.yml">
    <img alt="JSON Lint" src="https://github.com/lowerpower/GPTContextWindowCompression/actions/workflows/json-lint.yml/badge.svg">
  </a>
</p>

[![Blog post](https://img.shields.io/badge/Blog-Rolling_Compression_for_GPT_%26_Gemini-0B7285?logo=read.cv&logoColor=white)](https://blog.mycal.net/infinite-ai-chat-windows/?utm_campaign=github_readme_v1_2)

> Prefer the narrative overview? **Read the blog post** → https://blog.mycal.net/infinite-ai-chat-windows/?utm_campaign=github_readme_v1_2

Utilities you can load into ChatGPT/GPT-5 (and Gemini) to keep chats fast by compressing heavy context (images, long chats, code drafts, pasted docs) into tiny JSON summaries you can save and reuse. This **workflow** lets you roll projects forward indefinitely by exporting + reloading summaries (the model’s built-in window size does not change).  Supports both Soft Roll (same chat) and Hard roll (new chat) workflows.

**Version:** 1.2 • **Status:** Stable

---

## Overview
Context windows bloat fast. This menu gives you simple commands to compress on the fly while preserving useful state.

## Platforms

Works in **ChatGPT** and **Gemini** as documented. Also usable in **Claude**, **Perplexity** and other **LLM's** with the same per-chat pattern as **Gemini** (paste the menu/defaults each session, then use `Compress:` / `Export:` and seed new chats with `images.json` + `summary.json`). Web UIs show *estimated* savings only.

---

## 📦 Spec
- **`compression-menu.json` (v1.2)** — full machine-readable command spec.
  - **OpenAI GPT (chat.openai.com):** paste the file contents into a new chat, then send:  
    **Store this in memory and activate these commands.**
  - **Gemini (gemini.google.com):** paste the file contents into a new chat, then send:
    ```
    As my AI assistant, you are now a context compression tool. Store this JSON and use it to execute my commands. All commands start with "Compress:" or "Export:".
    ```
- **Memory tip (OpenAI GPT):** Save only a short activation note (command prefix + defaults) to memory; keep the full `compression-menu.json` in your repo and paste it when needed. Long JSON may not persist reliably in model memory.
- **Gemini note:** Instructions are per-chat and may not persist; paste the short defaults each session.

---

## 🚀 Quick Start — OpenAI GPT
**(Optional) Defaults for this chat**
> Remember: Recognize `Compress:` commands with defaults (images = Compact representation=lite; conversation = 400; notes = JSON; batch all images; show savings).

**1) Snapshot (optional)**  
> Export: context JSON  
Save as `snapshots/YYYY-MM-DD/context_snapshot.json`.

**2) Compress**  
> Compress: all  
You’ll get:
- `image_card_lite` JSON per image (compact caption + labels + tiny scene graph + micro Q/A + OCR — all text)
- `conversation_summary` JSON (~400 tokens)
- a token-savings estimate *(approximate in Web UI)*

**3) Save outputs**
- Combine image cards:  
  > Combine all image_card_lite outputs above into a single JSON array named images.json. Return only that array.  
  Save as `snapshots/YYYY-MM-DD/images.json`
- Save the conversation summary as `snapshots/YYYY-MM-DD/summary.json`

**4) Reuse later (fresh chat)**  
Paste your saved JSON and send:  
> Use these JSON summaries as context (they replace the original images and long history).

**Note on speed:** A **soft roll** (same chat) improves organization now; speed improves after old tokens naturally scroll out. For **instant** snappiness, start a **new chat** and seed it only with your JSON summaries (a **hard roll**).

---

## 🚀 Quick Start — Gemini
**(Optional) Defaults for this chat**  
> Remember: For this conversation, recognize `Compress:` commands with defaults (images = Compact representation=lite; conversation = 400; notes = JSON; batch all images; show savings).

**1) Snapshot (optional)**  
> Export: context JSON  
Save as `snapshots/YYYY-MM-DD/context_snapshot.json`.

**2) Compress**  
> Compress: all  
You’ll get `image_card_lite` blocks + a `conversation_summary` + a savings estimate.

**3) Save**
- Images → `snapshots/YYYY-MM-DD/images.json` (combine as above)  
- Conversation → `snapshots/YYYY-MM-DD/summary.json`

**4) Reuse later**  
> Use these JSON summaries as context (they replace the original images and long history).

---

## Commands (essentials)

- **Images**  
  `Compress: images [Ultra|Compact|Verbose] [representation=summary|lite] [max_labels=8] [max_triples=8] [max_qa=6] [include_ocr=true|false]`  
  **Default:** `Compact representation=lite` (processes **all** images in the chat)  
  **Representations:**  
  - `summary` → prose only (Ultra/Compact/Verbose)  
  - `lite` → prose **+** labels, small scene graph, micro Q/A, OCR *(text-only)*  
  - `full` → **offline-only** (embeddings, pHash, ANN index; not supported in ChatGPT/Gemini Web UI)

- **Conversation**  
  `Compress: conversation [target=N]` (default `400`) — concise synopsis (goals, facts, decisions, TODOs).

- **Code**  
  `Compress: code` — final clean snippet + lessons; drops intermediate drafts.

- **Notes**  
  `Compress: notes [JSON|bullets]` (default `JSON`) — compact schema grouped by topic.

- **Document**  
  `Compress: document [outline|summary|key points]` (default `outline`) — structured abstract of long pasted text.

- **Export**  
  `Export: context [JSON|Markdown] [target=N]` — snapshot before trimming.

- **All**  
  `Compress: all` → Images **Compact+Lite**, Conversation **400**, Code **final+lessons**, Notes **JSON**; returns an estimated token-savings report.

---

## Token Savings (Typical)

| Item                      | Before (tokens) | After (tokens)      | % Saved   |
|--------------------------|------------------|---------------------|----------:|
| Image (raw encoding)     | ~3,000           | Ultra ~40           | ~99%      |
|                          |                  | Compact ~400        | ~85–90%   |
|                          |                  | Compact+Lite ~500   | ~82–85%   |
| Conversation long → 400  | 6–10k → 400      | 400                 | ~90%      |
| Notes → JSON             | variable         | ~½–¼ size           | ~50–75%   |

> **Web UI note:** savings are estimates. Exact token counts are available only via API usage metrics.

---

## Outputs (examples)

**Image (Lite)**
```json
{
  "type": "image_card_lite",
  "version": "1.0",
  "id": "auto-or-your-handle",
  "source_hint": "filename-or-caption",
  "alt": "One-sentence alt text",
  "captions": { "ultra": "≤40 tokens", "compact": "≈400 tokens" },
  "labels": ["hat", "scarf", "wooden-frame", "desert", "mountains"],
  "scene_graph": [
    ["person-left", "wears", "wide-brim hat"],
    ["person-center", "wears", "orange scarf"],
    ["structure", "stands-in", "desert"]
  ],
  "qa": [
    { "q": "How many people?", "a": "3" },
    { "q": "Environment?", "a": "Desert with wooden frame and mountains" }
  ],
  "ocr": { "text": "" }
}

``` 

** Conversation summary output (400 tokens target) **
```json
{
  "type": "conversation_summary",
  "version": "1.0",
  "goals": ["..."],
  "facts": ["..."],
  "decisions": ["..."],
  "constraints": ["..."],
  "todos": ["..."],
  "open_questions": ["..."]
}
```

** Export snapshot **
```json
{
  "type": "context_snapshot",
  "version": "1.0",
  "timestamp": "ISO8601",
  "conversation_summary": { "tokens_estimated": 0, "goals": [], "facts": [], "decisions": [] },
  "images": [{ "id": "image-label", "raw_tokens_estimated": 3000, "summary_compact": "..." }],
  "notes": [{"topic": "Topic","points":["p1","p2"]}],
  "code_snippets": [{"final": "snippet","drafts_count": 0}]
}
```

---

## (Optional) Persistence

### OpenAI GPT — one-time Memory (type in any chat)
Remember: Recognize `Compress:` commands with defaults (images = Compact representation=lite; conversation = 400; notes = JSON; batch all images; show savings).

### OpenAI GPT — Custom Instructions (Settings → Personalization)

**What should ChatGPT know about you?**
Recognize commands starting with “Compress:” (images, conversation, code, notes, document, all) and run them with compact defaults. For images, if I say “Summarize [Ultra/Compact/Verbose] and drop tokens,” generate that summary and continue only with the text (no heavy image tokens). Prefer structured outputs; don’t add identities to images.

**How should ChatGPT respond?**
Be concise and structured. After compression, show a brief estimated token-savings report. Default images to Compact; if I say representation=lite, include labels, tiny scene graph, micro Q/A, OCR (text only).

> Tip: Save only this short note to Memory. Keep `compression-menu.json` in the repo and paste it when needed.

### Gemini note
Gemini doesn’t offer durable user-controlled memory; paste the short defaults at the start of each session.


---


## Soft vs. Hard Roll

- **Soft roll (same chat):** organize now; speed improves later when old tokens scroll out.
- **Hard roll (new chat):** paste only `images.json` + `summary.json` (and optional snapshot) → **instant** snappiness.

## Privacy

- Don’t add identities to image outputs unless you supply them explicitly.
- Strip GPS/EXIF before committing.
- Redact sensitive content in summaries.

## Suggested save layout

```
snapshots/
└── YYYY-MM-DD/
    ├── context_snapshot.json
    ├── images.json
    └── summary.json
```

## One-line Cheat Sheet
```
Compress: images [level] [representation=summary|lite] | conversation [N] | code | notes [format] | document [mode] | all  •  Export: context [JSON|Markdown] [N]
```

## Further reading
- Blog: **Make Your Chat Context Feel Infinite**  
  https://blog.mycal.net/infinite-ai-chat-windows/?utm_campaign=github_readme_v1_2
- Teaser: *image_card_medium* (coming soon)



