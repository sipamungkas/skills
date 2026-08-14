# Personal Agent Skills

Reusable agent skills that follow the [Agent Skills](https://agentskills.io/specification) convention. Each skill is a folder under `skills/` containing a required `SKILL.md` and optional resources.

## Install

```bash
# install every skill in this repository
npx skills add <github-user>/sipamungkas-skills

# install one skill
npx skills add <github-user>/sipamungkas-skills --skill expo-feature-based-structure
```

## Skills

- **expo-feature-based-structure** — Feature-first folder structure for Expo Router apps: routing in `src/app`, screens in `src/features`, and shared UI in `src/components/app`.

## Structure

```text
skills/
  <skill-name>/
    SKILL.md              # required
    agents/openai.yaml    # optional Codex UI metadata
    references/           # optional reference docs
    scripts/              # optional executable helpers
    assets/               # optional templates and media
```
