# Example JSON quick reference

Use these annotated pointers when you want to understand what each sample file in this folder demonstrates. They pair with the `compression-menu.json` spec and help you check that the outputs you generate follow the same structure.

## `image_card_lite.json`
- Produced by running `Compress: images Compact representation=lite` on an image-heavy chat.
- Shows how captions, labels, scene graph triples, Q/A, and OCR text are packaged together in the Lite representation.
- Compare your own image summaries against these sections to confirm you are returning the required text-only fields.

## `conversation_summary.json`
- Result of `Compress: conversation 400` (or another target token count you supply).
- Demonstrates the canonical buckets—`goals`, `facts`, `decisions`, `constraints`, `todos`, and `open_questions`—that downstream workflows expect.
- Use it as a template when editing or validating conversation summaries before storing them.

## `context_snapshot.json`
- Example output from `Export: context JSON` right before trimming or starting a fresh chat.
- Captures a top-level `context_snapshot` wrapper plus nested image, notes, and code summaries, illustrating how everything fits together for archival.
- Helpful for verifying that your saved snapshots preserve the IDs and token estimates you need for later reloads.

### Need copy-ready commands?
`examples/commands.md` intentionally houses the quick copy/paste command list. Reference it whenever you need the exact prompts without repeating them here.
