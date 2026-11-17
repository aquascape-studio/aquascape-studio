# aquascape-studio

Umbrella repo: docs (`docs/ROADMAP.md`, `docs/iterations/`), e2e tests, and submodules under `repos/`.

## Language rule (owner instruction, 2026-11-17) — applies to every agent

**Everything public on GitHub is written in English.** This covers, in every
aquascape-studio repo:

- issues, issue comments, PR titles/descriptions, review comments
- commit messages and branch names
- docs committed to repos (`docs/ROADMAP.md`, `docs/iterations/*.md`, ADRs, READMEs)
- code comments and contract doc blocks

Not affected:
- the owner-facing chat summary and push notification at the end of a run
  (those stay in Chinese, as the daily task asks)
- product UI copy in the app itself (bilingual zh + en by design)
- data values that are inherently Chinese (e.g. `commonNames.zh` in plants.json)

When docs, issues or PRs quote UI copy or owner remarks, give the English
version only (note "bilingual in the UI" / "translated" if useful) — no Chinese
characters in GitHub prose. All earlier Chinese docs, issues, comments and PR
descriptions were translated on 2026-11-17. Three old aquascape-web commit
messages quote Chinese UI copy; git history cannot be rewritten, so their
English versions live in aquascape-web `docs/commit-message-translations.md`.
