# AGENTS.md

Fork of [JunSeo99/claude-skill-codex-imagegen](https://github.com/JunSeo99/claude-skill-codex-imagegen):
a Claude Code skill that generates images through the user's Codex subscription (`codex exec`
as a thin, isolated relay to the built-in `image_gen` tool). `upstream` remote points at the original.

- Skill source: `skill/` (SKILL.md, references, Python stdlib scripts). Do not edit `dist/` by hand.
- After any change under `skill/`: `python3 scripts/package_skill.py` to rebuild `dist/codex-imagegen.skill`.
- Tests: `python3 -m unittest discover -s tests -v` and `python3 scripts/package_skill.py --check`.
- Never add an image API / `OPENAI_API_KEY` path, and never weaken the relay isolation flags.
- `--model` is the text relay, not the image model; the image backend is Codex-managed.
- When Codex CLI changes, re-probe the tool schema and record results in `docs/validation.md`
  and `docs/model-research.md`.
- Local-only (gitignored): `coderef/` (other implementations for reference), `test-runs/`.
- Tracker: `TODO.md`.
