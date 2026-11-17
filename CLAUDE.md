# aquascape-studio

Umbrella repo: docs (`docs/ROADMAP.md`, `docs/iterations/`), e2e tests, and submodules under `repos/`.

## Language rule (owner instruction, 2026-11-17) — applies to every agent

**Everything public on GitHub is written in English.** This covers, in every
aquascape-studio repo:

- issues, issue comments, PR titles/descriptions, review comments
- commit messages and branch names
- docs committed to repos (`docs/ROADMAP.md`, `docs/iterations/*.md`, ADRs, READMEs)
- code comments and contract doc blocks

**The product is English-only too (owner, 2026-11-17).** App UI copy (web,
mobile, ui package) is English only — no bilingual zh + en labels — and the
plant library has no Chinese names (botany schema 0.8.0 removed
`commonNames.zh` and `pinyin`; `commonNames` is strict). This overrides the
daily task's older instruction to use "real Chinese trade names" for new
species: new species get English common names only.

The only exception: the owner-facing chat summary and push notification at
the end of a run stay in Chinese, as the daily task asks.

When docs, issues or PRs quote owner remarks, give the English version only
(mark it "translated") — no Chinese characters anywhere on GitHub. All earlier Chinese docs, issues, comments and PR
descriptions were translated on 2026-11-17. Three old aquascape-web commit
messages quote Chinese UI copy; git history cannot be rewritten, so their
English versions live in aquascape-web `docs/commit-message-translations.md`.
