# CHANGELOG.md

## [1.2] - 2025-09-22
### Added
- **Image “Lite” representation**: summary + labels + scene graph + micro Q/A + OCR (all text, token-lean).
- **Export: context** command (JSON/Markdown) with optional `target_tokens` cap.
- **Estimated savings report** after each compression run.

### Changed
- Defaults: Images → `Compact representation=lite`; Conversation → `400` tokens; Notes → `JSON`.
- Batch behavior: process **all images** in the current conversation by default.

### Notes
- “full” image representation (embeddings/pHash/ANN) remains **offline-only**.
- Prefer Compact+Lite for day-to-day—~82–85% token savings vs. raw image encodings.

## [1.1] - 2025-09-21
### Added
- Initial **Export: context** command and snapshot schema.

## [1.0] - 2025-09-21
### Added
- Base **Context Compression Menu**: images, conversation, code, notes, document, all.
- Ultra/Compact/Verbose image summaries and 400-token conversation rollups.

