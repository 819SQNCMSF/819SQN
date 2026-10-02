---
name: repo-navigation
description: Build, update and use navigation.md, a keyword-routed map of this repo (or sub-repo) that lets a session jump straight to the right file instead of searching. Use at the start of every session, before any file search, whenever a file is added, moved, renamed or deleted, whenever the user asks where something is, which file covers a topic, or what the repo has on a subject, and when setting up a new repo or sub-repo.
---

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.

# repo-navigation

`navigation.md` is the map every session reads before touching anything else. It works like graphify's graph report, in plain markdown with no tooling: **hubs** (graphify's "god nodes", the most central files), **clusters** (its communities, topic groups), and **relations** tagged by how sure you are (graphify's EXTRACTED / INFERRED / AMBIGUOUS). Consult the map first, search second.

## File format

```
# navigation: <repo name>
Entries: N

## Hubs
- `path` — why it is central (max 8)

## Sub-repos            (main repo only)
- `name` — subject · scope · location or URL · status

## Clusters
### <Cluster name> — one-line scope
- `path/file.md` — summary in the user's own phrasing (max 25 words). keys: k1, k2, synonyms. rel: `other.md` (stated)
```

Entry rules:
- The key is the backticked repo-relative path. Folder entries end with `/`. One line per entry.
- The summary says what question the file answers, in the words a user would use. Put likely ask-phrases and synonyms in `keys`.
- Copy `summary` and `keywords` from the file's own frontmatter when present. Never reopen a file just to summarise it.
- `rel:` lists at most 3 related files. Tag `(stated)` if the file itself says so, `(inferred)` if you reasoned it. If unsure, leave it out.
- Never index `archive (to ignore)/`, `.git/`, `.claude/`, `navigation.md` or `session-memory.md`.

## Size and hierarchy
Keep the root `navigation.md` at 200 lines or fewer. When a cluster passes about 25 entries, move it into `<folder>/navigation.md` (same format, paths relative to that folder) and leave one folder entry in the root.

## Using it (lookup)
1. Read `navigation.md`. If it is over about 100 lines, Grep it with `-i` for 2–3 of the user's keywords and read only the matching entries.
2. Rank by keyword overlap with the request, synonyms included. Open the single best file; open a second only if the first lacks the answer. Follow `rel:` links before searching.
3. A folder entry points to a nested navigation file: read it and repeat.
4. No match: do one targeted Grep (archive excluded), answer from what you find, then add the missing entry.
5. A listed path is missing or moved: fix the entry immediately.

## Keeping it current (same turn as the change)
- File added or converted: add an entry. Moved or renamed: update the path, and Grep the old path in navigation files to fix `rel:` links. Deleted: remove the entry. Content changed meaningfully: refresh summary and keys.
- Update the `Entries` line.
- Audit only when the user asks or after bulk changes: run `git ls-files` (drop the archive folder), compare with the listed paths, add what is missing and delete what is dead.

## Bootstrapping (no navigation.md yet)
1. List files with `git ls-files`, not `find` or `ls -R`. Drop `archive (to ignore)/`, `.git/` and `.claude/`.
2. Group them by top-level folder or topic into clusters. Take each summary from frontmatter, or from the first heading plus a Read limited to 30 lines. Do not read whole files.
3. Choose hubs: the natural entry points and the files others refer to most.
4. Write `navigation.md` in one Write, then give the user a three-line summary.

For a new sub-repo, do the same inside it, then add its line to "Sub-repos" in the main repo's `navigation.md`.

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.
