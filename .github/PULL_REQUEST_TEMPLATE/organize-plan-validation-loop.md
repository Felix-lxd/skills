# Pull Request: organize/plan-validation-loop → main

This branch moves the standalone skill content into a dedicated skill directory for monorepo packaging.

Changes included:
- Added directory: skills/plan-validation-loop/
  - SKILL.md (frontmatter metadata + full skill specification)
  - references/validation-phases.md (validated references content)
  - README.md (usage and installation notes)
  - skill.json (machine-readable manifest)
  - examples.md (trigger payload example)
- Updated skills/README.md index to point at the new directory
- Kept original `skills/plan-validation-loop.md` as a backup file in the repository root under `skills/` (not deleted)

Why:
- Organize skill into a self-contained directory so third-parties can easily import, copy, or reference the skill.
- Provide a manifest for programmatic discovery and usage.

Notes:
- License set to Apache-2.0 in SKILL.md and skill.json
- Version: 0.1.0

Approval steps:
- Merge branch `organize/plan-validation-loop` into `main` when ready.

