<p align="center">
  <a href="https://skills.sh/sipamungkas/skills">
    <img src="https://skills.sh/b/sipamungkas/skills" alt="skills.sh badge" />
  </a>
</p>

<p align="center">
  <a href="https://agentskills.io/specification">
    <img src="https://img.shields.io/badge/spec-Agent%20Skills-5A67D8" alt="Agent Skills specification" />
  </a>
  <a href="https://www.npmjs.com/package/skills">
    <img src="https://img.shields.io/badge/install-npx%20skills-111111" alt="Install with npx skills" />
  </a>
</p>

# sipamungkas/skills

A personal, version-controlled collection of reusable agent skills. Every skill is a self-contained `SKILL.md` that follows the open [Agent Skills](https://agentskills.io/specification) specification, so it works with Claude Code, Codex, Cursor, OpenCode, and any other agent that installs skills with `npx skills`.

## Quick start

```bash
# list the skills available in this repository
npx skills add sipamungkas/skills --list

# install every skill
npx skills add sipamungkas/skills

# install a single skill
npx skills add sipamungkas/skills --skill expo-feature-based-structure

# install globally so it is available across all projects
npx skills add sipamungkas/skills -g
```

## Skills

| Skill | Description |
| --- | --- |
| [`expo-feature-based-structure`](skills/expo-feature-based-structure/SKILL.md) | Feature-first folder structure for Expo Router apps: routing in `src/app`, screens in `src/features`, and shared UI in `src/components/app`. |

## Repository structure

```text
skills/
  expo-feature-based-structure/
    SKILL.md              # required: YAML frontmatter with name and description
    agents/
      openai.yaml         # optional Codex/ChatGPT UI metadata
```

## Compatibility

- **Claude Code** reads skills from `~/.claude/skills/` or `.claude/skills/`.
- **Codex** reads skills from `~/.codex/skills/` or `$CODEX_HOME/skills/`.
- **Cursor, OpenCode, and other agents** install skills with `npx skills add`.

The optional `agents/openai.yaml` file is only read by Codex and ChatGPT-style agents for UI metadata. Other agents safely ignore it.

## Adding a skill

```bash
npx skills init my-skill-name
```

Write the `SKILL.md` body, keep the `name` and `description` frontmatter, and add optional `references/`, `scripts/`, or `assets/` folders only when the skill actually needs them.
