# Agent skills

My collection of reusable instructions for AI coding agents, packaged in the [Agent Skills](https://agentskills.io/) format.

## Installation

Use the [skills CLI](https://github.com/vercel-labs/skills) with Node.js and npm installed:

```bash
npx skills add sean-yeoh/skills
```

Follow the prompts to choose skills and target agents. The CLI supports Claude Code, Codex, Cursor, and other coding agents.

### Install a specific skill

```bash
npx skills add sean-yeoh/skills --skill commit
```

To target a particular agent:

```bash
npx skills add sean-yeoh/skills --skill commit --agent claude-code
```

Installation is project-scoped by default. Add `--global` to make a skill available across projects:

```bash
npx skills add sean-yeoh/skills --skill commit --global
```

### Browse available skills

```bash
npx skills add sean-yeoh/skills --list
```

This lists the repository's skills without installing them.

## Available skills

| Skill | Description |
| --- | --- |
| [commit](skills/commit/SKILL.md) | Prepare commit messages, stage task changes, or create Git commits while preserving unrelated work and following repository conventions. |

## Usage

After installation, ask your agent to perform the task. For the `commit` skill, examples include:

- "Commit the changes from this task. Leave unrelated work untouched."
- "Prepare a commit message for these changes without staging or committing."
- "Stage the changes from this task, but don't create a commit."
- "Split this task's changes into coherent commits."

The skill distinguishes between message preparation, staging, and committing. It instructs the agent to preserve unrelated staged and unstaged changes, and requires an explicit request before amending commits, rewriting history, pushing, or bypassing hooks.

## Updates

Update installed skills with:

```bash
npx skills update
```

## Repository structure

Each skill lives in its own directory under `skills/`. Its `SKILL.md` contains YAML metadata (`name` and `description`) followed by the agent instructions.

```text
skills/
└── commit/
    └── SKILL.md
```
