> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.

# 819SQN — administrator repo

819SQN is the admin hub of a library of topic-subject sub-repos. Each sub-repo is a branch of this repo with its own independent Claude session. This repo holds the shared rules, skills and navigation for all of them, keeps the registry of sub-repos, and builds new sub-repos when the user asks. Subject content lives in sub-repos, not here.

## Orient (start of every session)
1. Rules: `RULEBOOK.md` is imported at the bottom of this file. It binds this repo and every sub-repo.
2. Read `navigation.md` before searching for anything. Missing: build it with the repo-navigation skill.
3. Read `session-memory.md` (RULEBOOK D). Missing: create it from the template there.
4. Look for new raw files (no `.md` twin yet) with this command; hand any hits to file-simplifier, do not convert more than 5 without asking:
   ```
   git ls-files -co --exclude-standard | grep -iE '\.(pdf|docx?|pptx?|xlsx?|odt|rtf|epub|html?|png|jpe?g|gif|webp|tiff?)$' | grep -v -e '^archive (to ignore)/' -e '^\.claude/'
   ```

## Skills (`.claude/skills/`, loaded only when triggered)
- **file-simplifier**: a new non-markdown file arrives. Convert it to a compact `.md`, move the original to `archive (to ignore)/originals/`, then use only the `.md`. Costs tokens once so every later session costs fewer.
- **repo-navigation**: build, update or consult `navigation.md`, a keyword-routed map of the repo modelled on graphify (hubs, clusters, tagged relations). Used by every session here and in every sub-repo.

## Special paths
- `navigation.md`: repo map, plus the "Sub-repos" registry.
- `session-memory.md`: working memory.
- `archive (to ignore)/`: subfolders `originals/`, `history/` and `finished-work/`; user-only except as RULEBOOK A2 and F3 allow.
- Branch `<name>`: one sub-repo, an orphan branch of this repo, listed under "Sub-repos" in `navigation.md`.

## Building a sub-repo (only when the user asks)
1. If not already given, ask once: subject, scope in 1–2 lines, source files, proposed name.
2. Create the orphan branch `<name>`, built in a separate git worktree so this branch's files stay untouched, containing: verbatim copies of `RULEBOOK.md` and both skill folders; a short sub-repo `CLAUDE.md` (purpose, the same Orient steps, the skills list; no repeated rules, same read-only notes); empty `navigation.md` and `session-memory.md`; `archive (to ignore)/` with the subfolders `originals/`, `history/` and `finished-work/`, each holding a `.gitkeep`. Show the user the new CLAUDE.md before finishing.
3. Convert the supplied files with file-simplifier and build the sub-repo's `navigation.md`.
4. Add one line to "Sub-repos" in this branch's `navigation.md` (name, subject, scope, branch, status) and push the new branch to origin.

## Changing the configuration
Configuration means this file, `RULEBOOK.md` and the `SKILL.md` files. Never change it unasked. After the user approves a change and it is applied, offer to prepare the same change for every sub-repo listed in `navigation.md`.

## Rules (imported)
@RULEBOOK.md

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.
