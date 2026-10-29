# TODO

## Done (2026-10-25, codex-cli 0.164.0)
- [x] Evaluate alternatives (see `CODE_REVIEW_2026-10-25_claude_opus_5.5.md`); fork JunSeo99
- [x] Relay sets `transparent_background=true` when the brief asks for it (confirmed in a session rollout)
- [x] Default relay `gpt-6-luna`; minimum CLI 0.164.0
- [x] Fix: read PNGs from the session directory (relay never saw the saved path on 0.164)
- [x] Token breakdown from a rollout; strip `$imagegen` (it injected Codex's whole imagegen skill) and disable permission/collaboration instructions: about 10,800 → 5,900 input tokens per request
- [x] Re-measured injected context on 0.164 (`docs/validation.md`)
- [x] `solidify_png_alpha.py` for near-opaque subjects (alpha 250–254)
- [x] `codex_status.py` readiness check (no model call)
- [x] Claude Code plugin manifests; `npx skills add` verified to discover the skill
- [x] SKILL.md: save the brief next to exported images as `<image-name>.md`
- [x] Live end-to-end runs through the launcher with alpha check
- [x] Docs: validation, model research, transparency guidance, changelog

- [x] Installed globally for Claude Code (2026-10-26): `npx skills add <repo> --skill codex-imagegen -g -a claude-code --copy` from branch `codex-cli-0.164` → `~/.claude/skills/codex-imagegen`

- [x] Fork `main` fast-forwarded to `codex-0.164-update`; `npx skills add kastruppro/claude-skill-codex-imagegen` installs the new version (2026-10-29)

## Open
- [x] Rewritten commits pushed with noreply author email (2026-10-29)
- [ ] After the upstream merge, reinstall from `JunSeo99/claude-skill-codex-imagegen`
- [x] PR upstream: https://github.com/JunSeo99/claude-skill-codex-imagegen/pull/1 (branch `codex-cli-0.164`)
- [ ] Follow up on PR review; once merged, sync fork main with upstream
- [ ] Codex still injects a multi-agent role note (~2,400 chars) and the user's global AGENTS.md; look for a setting in later CLI versions
- [ ] Optional: plugin icon (`.claude-plugin/icon.png`, 512–2048 px)
