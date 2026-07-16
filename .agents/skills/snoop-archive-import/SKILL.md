---
name: snoop-archive-import
description: Import source articles into the Snoop repository through a review-gated workflow. Use when Codex needs to read source articles such as Apple Notes, convert them to repository-style Markdown, classify them into the Snoop archive structure, prepare Temp and Upload_Log review artifacts, obtain user confirmation, copy and verify approved files, open a GitHub pull request, and clear Temp contents without deleting Temp itself.
---

# Snoop Archive Import

Prepare article imports for the Snoop repository and stop for user approval before publishing.

## Discover the repository

1. If the current working directory is inside a Git repository whose `origin` resolves to `qiuyu-jii/Snoop`, use that repository.
2. Otherwise check these workspace candidates in order, expanding `$HOME` for the active user:
   - `/Volumes/Q's Mac Extended Hard Disk/思脑谱 - snoop`
   - `$HOME/Developer/思脑谱 - snoop`
3. Accept a candidate only when it is a Git repository and its `origin` resolves to `qiuyu-jii/Snoop`. If no candidate passes, stop and ask the user for the active repository path.
4. Never use `$HOME/Documents/思脑谱 - snoop`; it is an obsolete workspace path.
5. After resolving the repository, read `README.md`, `ChangeLog.md`, the current directory tree, and representative Markdown files.
6. Preserve unrelated working-tree changes. Never use an obsolete workspace when the user has designated a formal workspace.
7. Pull or fetch before relying on repository state when remote changes are possible.

## Read and convert sources

1. Read ordinary Apple Notes through the application's accessibility text layer, not OCR. Use visual inspection only for image-only or handwritten notes, and tell the user when OCR or visual transcription would be required.
2. Use the note's own creation timestamp. Do not substitute the current time or a list-view relative date.
3. Preserve original wording, capitalization, punctuation, typos, and paragraph order. Make only Markdown formatting changes.
4. Write drafts under `<repo>/Temp/` before touching archive directories.
5. For `log/` and `lost-world/` drafts:
   - name files `YYYY-MM-DD-title.md`;
   - omit terminal sentence punctuation from filenames;
   - use `# Original title` as the first line;
   - separate source paragraphs with blank lines.
6. For `observations/` drafts:
   - allocate the next unused four-digit sequence across the repository and `Temp/`;
   - order a batch chronologically by creation timestamp before assigning numbers;
   - name files `NNNN-title.md`;
   - use `# Original title` as the first line;
   - append `> 记于YYYY-MM-DD。` after a blank line.
7. Verify every converted draft against the source text after normalizing Markdown-only syntax and whitespace. Report any mismatch instead of guessing.

## Classify drafts

Use the article's dominant function and the repository's current README definitions:

- `observations/`: immediate, situated records of direct sensory, bodily, emotional, or mental experience that a general reader can understand without decoding dense metaphor or stream of consciousness. Require clear people, places, events, or feelings. Readability is mandatory, not optional.
- `log/notes/`: raw notes, diary fragments, and thinking in progress without a primarily literary or argumentative form.
- `log/essays/`: structured arguments, analysis, criticism, and sustained idea-driven reflections.
- `log/prose/`: literary prose, narrative nonfiction, memoir, fiction, scene-driven writing, stream of consciousness, and metaphor-dense or deliberately opaque writing. Prefer this over `observations/` when direct experience is transformed into language that requires literary interpretation.
- `log/poems/`: poetry and lyrics whose lineation is structurally important.
- `lost-world/`: records centered on a complete world, place, relationship, or scene that once existed but cannot be returned to.

Create matching subdirectories inside `Temp/` and move each draft into its proposed destination. Do not classify by source folder alone. When two categories are plausible, choose the category that best describes why the piece exists and document the tradeoff. Apply this readability test before using `observations/`: if an ordinary reader must decode the language before understanding what happened or was felt, use `log/prose/` instead.

## Write the review log

1. Create `<repo>/Upload_Log/Upload_Log.md` on the first run.
2. Add a new review entry at the top on every run.
3. Begin the entry with the current local timestamp including timezone.
4. List every draft with proposed destination and a concise, content-based classification reason.
5. Mention filename or format transformations, observation sequence assignments, exclusions, and unresolved ambiguities.
6. End the entry with a line containing only `---`.

## Record human review changes

When the user or another human reviewer changes a proposed classification, filename, sequence, format, exclusion, or publication decision:

1. Create or refresh `<repo>/Upload_Log/Update.md` as the latest human-reviewed proposal. Include the current local timestamp, complete current file mapping, naming state, and confirmation status. End with `---`.
2. Create `<repo>/Upload_Log/Update_Log.md` on the first manual revision. Keep it append-only and add each newer entry at the top beneath the title.
3. Begin every Update Log entry with the current local timestamp including timezone.
4. Record the review instruction, old path or decision, new path or decision, naming or format changes, and concise reason.
5. Record observation renumbering explicitly and recheck sequence continuity.
6. End every entry with a line containing only `---`.
7. Never erase or silently rewrite a previous human-review entry. Treat `Update.md` as current state and `Update_Log.md` as audit history.

## Protect temporary files

Ensure the root `.gitignore` contains:

```gitignore
.DS_Store
Temp/
Upload_Log/
```

Never stage `Temp/`, `Upload_Log/`, or `.DS_Store`.

## Format the changelog

When creating or updating the repository changelog, use the exact filename `ChangeLog.md` and keep the bilingual content in this order:

1. Start with `# Changelog` and list all English versions from newest to oldest.
2. After the complete English section, add a blank line, a standalone `---`, and another blank line.
3. Continue with `# 更新日志（Changelog）` and list the matching Chinese versions from newest to oldest.
4. End the file with a standalone `---`.

Do not interleave the English and Chinese sections version by version.

## Confirmation gate

Stop after preparing `Temp/`, `Upload_Log/Upload_Log.md`, any applicable `Update.md` and `Update_Log.md`, and the proposed classification. Show the user the proposed file-to-destination mapping and request explicit confirmation. Do not copy into archive directories, create a publishing branch, commit, push, or open a PR before confirmation.

## Publish after confirmation

1. Synchronize the repository and create a `codex/` branch from the current target branch.
2. Copy each approved file from `Temp/` to its mirrored archive destination without changing content.
3. Compare source and destination bytes or normalized diffs. Proceed only when there are no unintended differences.
4. Stage only approved archive files and the intentional `.gitignore` change. Recheck the staged diff and ensure temporary directories are absent.
5. Commit, push, and open a draft PR to `main` with the classification summary and verification result.
6. After the PR succeeds, clear all contents inside `Temp/` but keep the `Temp` directory itself. Do not delete `Upload_Log` unless the user explicitly asks.
7. Report the PR URL, commit, files published, comparison result, and Temp cleanup result.

Never merge the PR without a separate user request.
