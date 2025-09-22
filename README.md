# GPTContextWindowCompression
Loadable into your ChatGPT memory, utilities to compress your context window to make it usable forever

# 🧩 Context Compression Menu (GPT-5)
**Version:** 1.2 • **Status:** Stable

## Overview
GPT-5 context windows bloat fast (images, long chats, code drafts, pasted docs).  
This menu gives you quick commands to compress on the fly while preserving useful state.

**What’s new (v1.2)**
- ✅ **Image “Lite” representation** (summary + labels + scene graph + micro Q/A + OCR) — all text, still tiny, much more searchable
- ✅ **Export: context** command (JSON/Markdown snapshot before you compress)
- ✅ Smarter defaults: **Compact + Lite** for images; batch all images; savings report after each run

---

## Quick Start
- Set defaults once (optional):  
  `Default images = Compact representation=lite; conversation = 400 tokens; notes = JSON`
- Use the one-liner:  
  `Compress: all`

---

## Commands

### Images
`Compress: images [Ultra | Compact | Verbose] [representation=summary|lite|full] [max_labels=8] [max_triples=8] [max_qa=6] [include_ocr=true|false]`  
- **Default:** `Compact representation=lite max_labels=8 max_triples=8 max_qa=6 include_ocr=true`  
- **Behavior:** Processes **all images in the conversation**; replaces heavy image tokens with text outputs.  
- **Representations:**
  - `summary` → prose only (Ultra/Compact/Verbose)
  - `lite` → prose **+** labels, small scene graph, micro Q/A, OCR (text-only)
  - `full` → *offline only* (embeddings, pHash, ANN index; not in ChatGPT UI)
- **Savings vs ~3k-token image:**  
  - Ultra (~40): ~99%  
  - Compact (~400): ~85–90%  
  - Compact+Lite (~480–550): ~82–85%

### Conversation
`Compress: conversation [target=N tokens]`  
- **Default:** `400`  
- Rolls entire chat into a concise synopsis (goals, facts, decisions, TODOs).

### Code
`Compress: code`  
- Collapses drafts → **final clean snippet** + **lessons learned**; drops intermediate attempts.

### Notes
`Compress: notes [JSON | bullets]`  
- **Default:** `JSON`  
- Converts brainstorms into compact schema grouped by topic.

### Document
`Compress: document [outline | summary | key points]`  
- **Default:** `outline`  
- Replaces long pasted docs with a structured abstract.

### Export (snapshot before trimming)
`Export: context [JSON | Markdown] [target=N tokens]`  
- **Default:** `JSON`  
- Creates a portable snapshot (conversation, images, notes, code). Optional token-capped export.

### All
`Compress: all`  
- Runs: Images → **Compact+Lite**, Conversation → **400**, Code → **final+lessons**, Notes → **JSON**.  
- Emits an **estimated token savings report**.

---

## Token Savings (Typical)

| Item                      | Before (tokens) | After (tokens) | % Saved |
|--------------------------|------------------|----------------|---------|
| Image (raw encoding)     | ~3,000           | Ultra ~40      | ~99%    |
|                          |                  | Compact ~400   | ~85–90% |
|                          |                  | Compact+Lite ~500 | ~82–85% |
| Conversation long → 400  | 6–10k → 400      | ~90%           |
| Notes → JSON             | variable         | ~½–¼ size      | ~50–75% |

---

## Outputs

**Image (Lite) example**
```json
{
  "type": "image_card_lite",
  "version": "1.0",
  "id": "auto-or-your-handle",
  "source_hint": "filename-or-caption",
  "alt": "One-sentence alt text",
  "captions": { "ultra": "≤40 tokens", "compact": "≈400 tokens" },
  "labels": ["hat","scarf","wooden-frame","desert","mountains"],
  "scene_graph": [
    ["person-left","wears","wide-brim hat"],
    ["person-center","wears","orange scarf"],
    ["structure","stands-in","desert"]
  ],
  "qa": [
    {"q":"How many people?","a":"3"},
    {"q":"Environment?","a":"Desert with wooden frame and mountains"}
  ],
  "ocr": {"text": ""}
}



