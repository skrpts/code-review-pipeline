# Release Notes

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
