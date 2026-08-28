# plan-validation-loop

This directory contains the plan-validation-loop Skill packaged for monorepo use.

Quick start

- Location: `skills/plan-validation-loop/`
- Main declaration: `SKILL.md`
- References: `references/validation-phases.md`
- Manifest: `skill.json`

Usage

- To include this skill in another project, copy the entire directory or add it as a git submodule.
- The SKILL.md file contains human-readable spec and frontmatter metadata (id, version, license, triggers).

Installation examples

1. Copy into your repo:
   - cp -r skills/plan-validation-loop/ /path/to/your/project/skills/
2. Add as submodule:
   - git submodule add https://github.com/Felix-lxd/skills.git skills
   - (or, if split later into independent repo, add that repo as a submodule)

Notes

- This skill is review-only and must never execute repair actions. See SKILL.md for strict operation boundaries.
