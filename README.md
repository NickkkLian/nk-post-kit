# nk-post-kit

![nk-post-kit](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-post-kit.png)

An agent skill for [Claude Code](https://code.claude.com) and [OpenAI Codex](https://developers.openai.com/codex). Turn a finished repository, tool or skill into a set of posts a person publishes by hand — a short X post, a 400–800 word build log, a 5–10 minute video script, a 60–90 second vertical cut, a GIF recording script, and a Chinese Xiaohongshu version — every claim traced to a file or a real run, with a gate that blocks invented numbers, superlatives, private names and contact details.

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — skills that stop an AI coding agent's
"done, tested, safe" from being taken on faith.

![nk-post-kit demo: before and after](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-post-kit.gif)

## What it does

- Drafts an X post, a build log, a video script, a vertical cut, a GIF recording script and a Chinese Xiaohongshu version from the repository and a real before/after run.
- `scripts/termgif.py` renders a scripted terminal session (or a saved run) to a GIF and a poster PNG, no screen recording needed.
- `scripts/postkit_check.py` gates the kit: lengths, links, invented-sounding claims, superlatives, local paths, contact details, and a private-words list you keep outside the repository.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. Collect the sources first
2. Record a real run
3. Write the six pieces
4. Render the GIF without recording
5. Look at the cards before the gate
6. Gate the kit

## Install

Pick one of four ways: three for Claude Code, one for OpenAI Codex. Skills load when a session starts, so open a **new** session after installing.

### 1 · Terminal, one command

```bash
git clone https://github.com/NickkkLian/nk-post-kit ~/.claude/skills/nk-post-kit
```

1. Run the command above (for one project only, clone into `.claude/skills/nk-post-kit` inside that project).
2. Start a new Claude Code session.
3. Check it loaded: type `/nk-post-kit` — it appears in the slash-command menu. Or just ask for the task; the skill triggers on its own.

### 2 · Claude Code in a terminal session (plugin)

The plugin route goes through the [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) marketplace. Add it once; after that each skill is one command.

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-post-kit@nickkk-skills
```

1. In a Claude Code session, run the first line (once per machine).
2. Run the second line.
3. Start a new session (or run `/reload-plugins`). The skill shows up as `nk-post-kit:nk-post-kit`.

Without opening a session, the same two steps work from a shell: `claude plugin marketplace add NickkkLian/nickkk-skills` then `claude plugin install nk-post-kit@nickkk-skills`.

### 3 · Claude desktop app (Code tab)

**Add the marketplace first — Discover only searches marketplaces you have already added.**

<img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/panel-route.gif" alt="Adding the marketplace and installing a skill in the desktop app" width="640">

<sub>Recorded on 2026-09-16, when the marketplace listed ten skills, all at version 0.1.0; it lists more now. The repository list in this recording shows the recorder's own repositories because a GitHub account is connected; yours will show yours. Type the full name as in step 4.</sub>

1. In the chat box, type `/plugin marketplace` and press Enter (or open **Settings → Customize → Plugins**). The **Plugins** panel opens.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step1-type-plugin-marketplace.png" alt="/plugin marketplace typed in the chat box" width="480">
2. Top right, open **Add ▾** and choose **Add marketplace**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step2-add-menu.png" alt="The Add menu with Add marketplace" width="480">
3. Choose **Add from a repository**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step3-add-from-repository.png" alt="Add marketplace dialog: Add from a repository" width="480">
4. In **URL**, type the full `NickkkLian/nickkk-skills`. At the bottom of the list choose the row **Use "NickkkLian/nickkk-skills"**, then press **Sync**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step4-url-then-sync.png" alt="URL filled in, Sync button" width="480">
5. You land on **Discover**, filtered to the new marketplace (**Filter · 1**). Find **Nk post kit** and press **Add**. Installed ones show **✓ Added**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step5-discover-add.png" alt="Discover list with Added and Add buttons" width="480">
6. Close the panel and start a new session.

To try it for one session without installing anything: `claude --plugin-dir ./nk-post-kit` from a clone.

### 4 · OpenAI Codex CLI

```bash
git clone https://github.com/NickkkLian/nk-post-kit.git ~/.agents/skills/nk-post-kit
```

1. Run the command above (for one project only, clone into `.agents/skills/nk-post-kit` inside that project).
2. Start a new Codex session.
3. Check it loaded, without spending a model call: `codex debug prompt-input | grep -o -- '- nk-post-kit[a-z0-9:-]*' | sort -u` prints `- nk-post-kit:nk-post-kit:`. Codex adds the `nk-post-kit:` prefix because this repository also carries a Claude Code plugin manifest. Ask for the task and the skill triggers on its own, or type `$` and pick it from the list.

## Compatibility

| Agent | Tested | What was checked |
|---|---|---|
| Claude Code (CLI 2.1.173, macOS) | partly | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. The route 2 plugin commands were also run from a shell with an isolated config: marketplace add, install, list. Two runs, both recorded. With a 14-turn limit the session loaded the skill, read the tool, re-ran its self-test to check the evidence files were true, and ran out of turns before writing anything — the limit had been copied from another skill instead of estimated from eight output files. With 26 turns it wrote all six pieces plus the sources file, wrote and ran its own gif-script.sh, rendered the GIF, drew the preview cards, and used them to catch its own root post at 11 characters over 280 and trim it — then hit the limit before running scripts/postkit_check.py. Gated here afterwards, that kit reports 8 findings (the reply carries a placeholder instead of a link, because the sample folder has no repository URL, and the Chinese piece and the sources file are missing the sections the newer P4 and P9 rules ask for). |
| OpenAI Codex CLI (0.155.0-alpha.9.2, gpt-5.6-sol, low reasoning, macOS) | yes | Copied into `~/.agents/skills` of a temporary home, in a fresh project, without the user's Codex config. From a plain request that never names the skill, Codex read SKILL.md and the template, verified the sample tool by re-running it, wrote and ran its own gif-script.sh, wrote the six pieces and the sources file, and ran scripts/postkit_check.py itself: 0 findings, re-checked here against the kept files. It left `REPLACE-WITH-REPOSITORY-URL` in the reply rather than inventing a link. One environment note, not a skill problem: the default python3 on that machine had a Pillow built for the other architecture, so `from PIL import Image` failed; Codex diagnosed it and re-ran the two renderers under `arch -x86_64`. The gate needs no Pillow at all. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

Route 4 was checked for this repository: cloned from GitHub into a temporary home's `~/.agents/skills`, it was listed by the step 3 command. This skill's frontmatter uses only name, description, license, compatibility and metadata.

## Verify

```bash
python3 scripts/postkit_check.py --selftest
python3 scripts/preview_cards.py --selftest
python3 scripts/termgif.py --selftest
```

Python 3.9+. postkit_check.py needs only the standard library; termgif.py and preview_cards.py also
need Pillow (`pip install pillow`). Before publishing, the guarded lines of each script were
mutated one at a time in a sandbox copy and the self-test was confirmed to go red on the named
assertion, without a traceback; the unmutated control stayed green.

## Limits

- The gate checks shape and red-line words; it cannot tell whether a sentence is true. The story-source file and a reviewer are for that.
- termgif renders text frames; it does not capture real GUIs. For interface demos, record the screen and convert with ffmpeg.
- The Chinese checks count characters and look for contact patterns; they do not judge tone.
- A preview card is this skill's own drawing, not a screenshot of X or Xiaohongshu: the fonts and the line breaks are close, not identical. It answers how long and how dense, not how it will look pixel for pixel.

## License

MIT. Read a script before letting it run in your environment.
