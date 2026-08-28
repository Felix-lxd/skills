# Skills index

This directory collects individual skills as sub-paths inside this repository (monorepo layout).

Current skills:

- plan-validation-loop
  - path: skills/plan-validation-loop.md
  - recommended canonical location: skills/plan-validation-loop/README.md (not yet moved)
  - description: Multi-round plan validation loop that verifies and refines repair plans (does not execute repairs).

Guidelines for adding new skills

- Put each skill under `skills/<skill-name>/` as a directory.
- Include a `README.md` or `skill.md` with the canonical skill content and metadata.
- Add a short entry here (this file) with link and one-line description.

Recommended next steps performed by maintainers or CI:

- Move `skills/plan-validation-loop.md` into `skills/plan-validation-loop/README.md` and update links.
- Add a `skills/.skill-template.md` or `skills/<skill>/SKILL.md` for metadata validation.

