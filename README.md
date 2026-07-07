# Cockpit Best Practices

A Codex skill for making safer AGI Cockpit operating decisions.

This skill is intentionally policy-focused. It does not replace the `cockpit`
skill command reference. Use `cockpit` for syntax and actions, and use
`cockpit-best-practices` when deciding what should happen to tasks, child tasks,
worktrees, autoruns, and cleanup candidates.

## What It Covers

- Task lifecycle judgment for `running`, `waiting_confirmation`, `completed`,
  `needsResume`, and pinned tasks.
- Complete vs remove policy.
- Parent and child task delegation rules.
- Worktree task closure checks.
- Autorun hygiene.
- How to interpret `cockpit-gc` cleanup candidates.
- Safety rules for destructive or irreversible cleanup.

## Installation

Copy this directory into your Codex skills directory:

```sh
mkdir -p ~/.codex/skills
cp -R cockpit-best-practices ~/.codex/skills/
```

Then start a new Codex session and invoke:

```text
$cockpit-best-practices
```

## Recommended Usage

Use this skill together with the `cockpit` skill:

```text
$cockpit $cockpit-best-practices
Review waiting Cockpit tasks and recommend which ones are safe to complete.
```

For backlog cleanup, pair it with `cockpit-gc`:

```text
$cockpit-gc $cockpit-best-practices
Find cleanup candidates, but do not complete or remove anything without approval.
```

## Safety Model

The skill favors preserving useful task history. It recommends promoting durable
knowledge into project files, commits, docs, issues, or PRs before closing tasks.

It also treats task completion, removal, process termination, worktree deletion,
autorun changes, and discarded child output as actions that require explicit user
approval.

## Repository Layout

- `SKILL.md` - Main Codex skill instructions.
- `agents/openai.yaml` - Optional display metadata for AGI Cockpit/OpenAI usage.
- `README_JA.md` - Japanese documentation.

