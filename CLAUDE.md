> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.

# Image-Generator — sub-repo of 819SQN

Image-Generator is a topic sub-repo of the 819SQN library, held as the `Image-Generator` branch of the 819SQN repository. It houses an independent Claude session for image generation: briefs, prompts, outputs and reference material. Shared rules and skills come from the 819SQN admin branch; this branch carries its own identical copies.

## Orient (start of every session)
1. Rules: `RULEBOOK.md` is imported at the bottom of this file. It binds this session.
2. Read `navigation.md` before searching for anything. Missing: build it with the repo-navigation skill.
3. Read `session-memory.md` (RULEBOOK D). Missing: create it from the template there.
4. Look for new raw files (no `.md` twin yet) with this command; hand any hits to file-simplifier, do not convert more than 5 without asking:
   ```
   git ls-files -co --exclude-standard | grep -iE '\.(pdf|docx?|pptx?|xlsx?|odt|rtf|epub|html?|png|jpe?g|gif|webp|tiff?)$' | grep -v -e '^archive (to ignore)/' -e '^\.claude/' -e '^outputs/'
   ```

## Skills (`.claude/skills/`, loaded only when triggered)
- **file-simplifier**: a new non-markdown file arrives. Convert it to a compact `.md`, move the original to `archive (to ignore)/originals/`, then use only the `.md`.
- **repo-navigation**: build, update or consult `navigation.md`, the keyword-routed map of this branch.

## Special paths
- `navigation.md`: repo map.
- `session-memory.md`: working memory.
- `outputs/`: generated images, each beside a `.md` twin with its prompt, settings and a 1–2 line summary with keywords. Never converted, never archived, never flagged by the Orient raw-file command.
- `archive (to ignore)/`: subfolders `originals/`, `history/` and `finished-work/`; user-only except as RULEBOOK A2 and F3 allow.

## External services
Before sending any prompt, file or image to an external service, ask the user: OK to send `<content>` to `<service>`? Never send what the user called private. Never print, log or write an API key into any file. Add no service yourself.

## Rules (imported)
@RULEBOOK.md

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.
