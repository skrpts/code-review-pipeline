# Release Notes

## v1.1.31
GH#1100 consumer wave batch 5 — declare the data flow these steps were resolving positionally. 1 binding(s) added: `extract-structured-data` ← Code Analysis. Each dependency was also re-pinned to the version that introduced its `source` slot, with checksums resolved from the live catalogue rather than transcribed — a binding against a pin below the slot version is declared but inert, which the canonical scan reports as `execution_bindings_unused_key`.

`extract-structured-data` is bound to **Code Analysis**, not to the entry before its parallel block. Its slot label ("Text to Process") is generic, so the fallback would be positional — but the entry before the block emits an `approval_decision` of type `decision`, and a decision is not text to process. `review_summary` is.

## v1.1.30
GH#1044 — document the inputs this workflow's prompts REQUIRE but its Inputs table never listed. A user supplying exactly what the table asked for was running with required parameters unsatisfied — a silent wrong run, not a documentation nicety. documented 1 input(s): code_diff. Every row is generated from the requiring prompt's own declared description, example and required flag — none invented. No behaviour change; documentation only.

## v1.1.29
GH#1011 — 2 step(s) that declared the non-existent `local.transform` now declare the LLM step type they were already dispatched as, and reference the shared prompt their skill never had: `format-conversion` → `content`; `defang-content` → `validation`. The `format-conversion` step declares `context.target_format: "markdown"` — the skill supports six formats, so the format belongs to the step rather than to the shared prompt.

These steps had no `prompt:` because `local.*` types need none, so the engine's LLM default had nothing to send: the author saw the model complaining about a prompt nobody wrote. **Output changes** — from a complaint to the thing the skill describes. Shared deps re-pinned to 1.0.4 with the published bundle checksum. Note: format-conversion and defang-content were pinned at 1.0.1, so this re-pin jumps to 1.0.4 — the intervening 1.0.1/1.0.2/1.0.3 are manifest-id alignment, `dependencies: []` and an en-US copy flip respectively, all explicitly content-free.

## v1.1.28
GH#863 Wave 2 (K-045 intent/output-mismatch + mis-wire) — rewire the pipeline so a real line-level review is actually produced. Previously the review summary synthesised outputs that did not exist yet (Security/Style resolved empty) and `style-checking` ran the wrong prompt (`approval-decision` instead of `style-violations`), leaving `inline-comments`, `style-violations`, and `approval-decision` orphaned. Fixes: `style-checking`→`style-violations`; new backing skills `inline-comment-generation` (runs `inline-comments`) and `approval-decision-making` (runs `approval-decision`); security + style now run first (parallel), then inline comments, then the `review-summary` synthesis (still the `output_step`), then the homed approval decision. Positional `{{steps.X.output}}` refs in review-summary/inline-comments/style-violations/approval-decision converted to named `context_params` wired by explicit `from_step`/`from_input` bindings (from_step resolves by skill title). contents.skills 2→4, total 10→12.

## v1.1.27
GH#833 — drop the embedded `github-mcp` service node; dep-reference the canonical `_shared/github-mcp@1.0.0` instead (kills per-skrpt drift so the app can content-dedup). Keeps the single-copy `ollama-local` embedded. contents.services 2→1.

## v1.1.26
GH#745 — declare per-step `output: {name, type}` on every execution step (review_summary/text, extracted_data/json, security_report/text, approval_decision/decision, formatted_review/text, sanitised_review/text). Lights up the #744 rich flow-map with named, typed outputs. Content-only; no bindings or logic changes.

## v1.1.25
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 6 inline shared-content files and declare 6 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Closes pre-Step-3 inline-vendoring for this bundle.

## v1.1.24
Wave 2: re-signed with canonical engine signing pipeline.

## v1.1.23
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.1.22
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.1.21
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.1.20
Initial catalog release with full structural and content-quality validation. All scanner checks pass.
