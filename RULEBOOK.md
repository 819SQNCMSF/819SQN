> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.

# RULEBOOK — binding on every session in this repo and in every sub-repo

Rules are numbered so you can cite them. If a user instruction conflicts with a rule, point out the conflict once; an explicit "do it anyway" from the user wins. Sub-repo copies of this file must stay identical to the main repo's copy.

## A. Protect configuration and the archive
- **A1.** Never edit, rename, move or delete a READ-ONLY file (CLAUDE.md, RULEBOOK.md, any SKILL.md) unless the user explicitly approved that exact change earlier in this session. To propose a change, show the exact diff and wait.
- **A2.** `archive (to ignore)/` is for the user only. Never read, open, list, search, grep, glob, edit or delete anything in it. The only permitted touches are moving a freshly converted original into it (file-simplifier skill) and writing or opening a `NOT-CURRENT-TRUTH_` history file as F3 allows. Exclude it from every search (Grep glob `!archive (to ignore)/**`).
- **A3.** Files sessions may change by design: `navigation.md`, `session-memory.md`, converted `.md` files, and the repo's content. Anything else: ask first.

## B. No subagents
- **B1.** Never spawn subagents, agent teams, background agents or multi-agent workflows (Agent tool, Explore, Plan, general-purpose, or any skill that fans out to agents), even for "thorough" or multi-part tasks. Reading a file or looking something up is one Read or Grep call. Do it yourself.
- **B2.** The only exception is the user explicitly asking for a subagent in the current session.

## C. Truth and evidence
- **C1.** State only what you can support. A factual claim must come from (a) a repo file you actually opened, cited as `path` + heading or line; (b) an online source you actually fetched, cited as title + URL + date accessed; or (c) your own reasoning, labelled as reasoning.
- **C2.** Online research: cite at the claim and finish with a `Sources:` list. Never cite a page you did not open. Prefer primary and official sources. If sources disagree, show the disagreement.
- **C3.** If you do not know or cannot verify, say so plainly ("not found in repo", "unverified"). Never guess paths, names, numbers, quotes, versions or dates, and never fill a gap with plausible text.
- **C4.** Label confidence: **verified** (opened or fetched), **inferred** (say from what), **unknown**. When you change an answer, say what changed and why.

## D. Session memory (never in CLAUDE.md)
- **D1.** Working memory lives in `session-memory.md` at the repo root. Never write session notes, progress or preferences into CLAUDE.md or RULEBOOK.md.
- **D2.** Read it at session start (cap: 100 lines). If missing, create it from the template below. It is the one config-adjacent file that carries no read-only note.
- **D3.** Update at milestones only: decision made, task finished or paused, user preference stated, blocker hit, and before any handoff or compaction. One terse line per item.
- **D4.** Over 100 lines: consolidate. Merge items, drop stale ones, roll finished work into one line each.
- **D5.** Store only what cannot be re-derived from files or navigation.md. No transcripts, no file contents, no duplicates.
- **D6.** Commit it together with the work it describes. Follow the user's git instructions; never push to main unless told.

Template:

```
# Session memory
## Active
## Decisions
## User preferences
## Open questions
## Done (one line each)
```

## E. Token efficiency
- **E1.** Navigate first: read `navigation.md`, then open the single best-matching file. No blind repo-wide Glob, Grep, `ls -R` or `find`.
- **E2.** Use converted `.md` files only. Never read an original once its `.md` twin exists.
- **E3.** Read narrow: Grep with line numbers to locate, then Read with offset and limit. Do not read a whole file for one fact. Do not re-read a file already in context unless it changed.
- **E4.** Cap tool output: `head`/`tail`, match limits, `files_with_matches` for broad searches.
- **E5.** Batch independent tool calls into one turn (parallel calls, not subagents).
- **E6.** Answer first and keep it short. Do not restate the question, narrate steps, or paste file contents back; point to path and line range.
- **E7.** Create only what was asked. No extra reports, READMEs, summaries or copies. One source of truth: link instead of duplicating.
- **E8.** Edit, do not rewrite. Use targeted edits; never regenerate a whole file to change a few lines.
- **E9.** Ask one short question before expensive or ambiguous work (converting more than 5 files, restructuring many files); otherwise proceed.
- **E10.** Web: search only when the repo cannot answer; one targeted query first; fetch with a specific extraction prompt; never re-fetch a URL in the same session. If a source will clearly be reused, offer to save a short cited note.
- **E11.** Load a skill only when its trigger applies.
- **E12.** Context hygiene: after each finished task update `session-memory.md`; when the topic changes or context is large, tell the user a fresh session or compaction will be cheaper.
- **E13.** Keep configuration small. Fix problems by pruning, not by adding rules.

## F. Current truth only
- **F1.** Every file in the repo except `archive (to ignore)/` states only the current truth, so old information cannot cause ambiguity or confusion. It holds no history: no edit dates, no "changed from", no "added because", no name of who supplied a fact. (i.e. write "Reports are saved as PDF", not "Reports are saved as PDF since 05/03/2026 because the team preferred it")
- **F2.** A truth file may hold only these extras, and only when needed to apply it correctly: a justification statement, an example of how to use a statement, file or paragraph, or an online source as supporting evidence. State a justification or example without naming who provided it and without the reasoning behind it. Give a source as a URL with section, paragraph and line number. (i.e. "Reports are saved as PDF so every reader can open them. Source: <URL>, section 2, paragraph 3, line 4")
- **F3.** History and the reason for a change are kept only in `archive (to ignore)/NOT-CURRENT-TRUTH_<name of the truth file>.md`. Such a file is never a source of truth. A session opens one only when the task explicitly requires verifying history, by its exact path and never by listing the folder, and never as part of a default read.

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.
