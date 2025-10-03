# Mini Next-Action Dataset

Goal: predict the next coherent edit (grouped diff) across files, not token autocomplete.

Anchor: mirrors Zeta’s “editable region → next edit,” but built from public GitHub PR diffs.

Current scope:
- Python: pandas PRs #62444, #62465
- Java: Apache Commons Lang PR #<num>
- Each entry pairs a source hunk with a test hunk.

Schema fields: `repo, pr_number, language, trigger, context_files, next_edit{files[path, op, span, hunk_header, summary]}, notes, gold_link`.

Heuristic:
- Treat each `@@ -a,b +c,d @@` hunk as one action.
- `span.start_line = c`, `span.end_line = c + max(d,1) - 1`.
- `op`: `replace` for source edits, `insert` for added tests (adjust if deletion).

Evaluation sketch:
- File-set match (did we select the right files?).
- Span overlap (line-range IoU).
- Future: build/test success; optional LLM-as-judge.

Next steps:
- Scale to 20–50 datapoints.
- Automate extraction.
- Add baseline next-file classifier.

