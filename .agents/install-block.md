# The canonical install block

One install story, one wording. `README.md` must say **this** and nothing else; a page under `docs/` carries no install wording of its own (see below). Change it here first, then propagate.

This repo is a fork (`yanfeng98/nano-mattpocock-skills`) that is not published anywhere: no registry, no listing of its own, no skills.sh entry. It installs **from source**: clone it, then run the link script. Every skill lands in the local harness skill directories as a symlink back into the user's clone, so `git pull` is the update and nothing changes behind their back.

## Source deploy: the link script

<canonical-block name="source-deploy">

```bash
git clone https://github.com/yanfeng98/nano-mattpocock-skills.git
cd nano-mattpocock-skills
bash scripts/link-skills.sh
```

It links every skill into `~/.claude/skills` (Claude Code) and `~/.agents/skills` (the cross-client path Codex reads, alongside its own native `~/.codex/skills`), one symlink per skill pointing back into the clone. An edit takes effect the next time you start a session, and `git pull` updates the installed set. Adding or renaming a skill means re-running the script, which never prunes: a skill you renamed or removed leaves its old symlink behind, so delete that by hand. `misc/` is skipped on purpose; `in-progress/` is linked.

</canonical-block>

That block is the whole story. Cloning is not optional, and there is no second route.

## `docs/` pages are not a consumer

A page carries no install command of its own. The install story is a property of this repo's `README.md` and of this file, and a per-page copy drifts. See [writing-docs.md](./writing-docs.md).

## Superseded: the upstream plugin, and skills.sh

`mattpocock-skills` is listed in Claude Code's official marketplace (configured name `claude-plugins-official`, promoted skills only), and [skills.sh](https://skills.sh/mattpocock/skills) serves upstream's whole repo. Both hand you **upstream's** code from `mattpocock/skills`, and neither reaches this repository: this fork ships no plugin of its own. `CHANGELOG.md`'s v1.2.0 entry and ADR 0002 describe the plugin upstream shipped, and both are kept as frozen history.

Recorded here only so neither gets reinvented as this repo's install story. Two things worth knowing:

- Anyone who already installed the upstream plugin should remove it (`claude plugin uninstall mattpocock-skills@claude-plugins-official`) before linking this repo, or they will have every skill twice.
- That removal command is the only plugin command that survives in user-facing prose, and it only ever removes. Neither route's install command appears in `README.md` or in any `docs/` page.

## Not the install story

There is no plugin of this repo's own to load, documented or otherwise. `.claude-plugin/`, `package.json`, `package-lock.json`, the release workflow, and `.changeset/` were removed on 2026-09-22, for the reasons ADR 0002 records. Nothing that was removed is load-bearing for installing the skills.
