---
name: cockpit-best-practices
description: Use when planning, reviewing, or cleaning up AGI Cockpit workflows, including task lifecycle decisions, complete vs remove policy, parent/child task delegation, worktree task closure, autorun hygiene, cockpit-gc interpretation, and multi-agent operating conventions.
---

# Cockpit Best Practices

Use this skill with the `cockpit` skill when the task is not just "which command do I run?" but "what is the right operating decision?"

Do not duplicate the `cockpit` command reference here. Load the `cockpit` skill for command syntax and this skill for lifecycle judgment.

## Core Principle

Cockpit tasks are work sessions, not long-term memory.

Durable knowledge should be promoted into files such as `CURRENT_TASK.md`, `AGENTS.md`, README, docs, design notes, commits, issue or PR text, or project-specific status files. Do not keep tasks open only because their conversation may be useful someday.

## Task Lifecycle

- `running`: leave it alone unless the user asks you to inspect, wait, or intervene.
- `waiting_confirmation`: inspect the latest report, then decide whether to continue, summarize, complete, or ask the user.
- `completed`: treat as retained history. Review when useful; remove only when cleanup is explicitly requested and the task has low future value.
- `needsResume`: resume only when the stopped session context is still useful. Prefer summarizing and closing stale sessions whose useful output has already been captured.
- `pinned`: treat as current focus. Keep the pinned set small and intentional.

`turn_complete` means one agent turn is done. It does not mean the whole task is complete.

## Complete Vs Remove

Prefer `complete` for work sessions that produced useful history.

Use `complete` when:

- the task's result has been reviewed
- child task output has been summarized into the parent task or durable files
- implementation has been merged, abandoned, or handed off
- a research task has delivered its conclusion
- a user-facing answer has been given
- the task is no longer an active coordination surface

Use `remove` only for low-value noise:

- duplicate tasks
- empty or failed startup tasks
- one-off terminal probes
- response-shape tests
- accidental tasks
- temporary tasks whose content has no future review value

Never remove a task just because it is old. Age is a reason to inspect and summarize, not a reason to discard.

## Promote Knowledge Before Closing

Before completing a task, ask what durable knowledge must survive.

Promote important information into:

- `CURRENT_TASK.md` for current project state, decisions, blockers, and next actions
- `AGENTS.md` only for cross-task rules or durable operating policy
- README or docs for user-facing or project-facing documentation
- design notes for decisions, tradeoffs, or research conclusions
- commits for implementation state
- issue or PR text when the project uses GitHub workflows

If there is nothing worth promoting, complete or remove according to the task's review value.

## Parent And Child Tasks

Parent tasks coordinate. Child tasks produce bounded outputs.

When a child task reports back:

1. Inspect the child output.
2. Extract decisions, findings, diffs, verification results, and unresolved risks.
3. Summarize the useful parts in the parent task or durable files.
4. Decide whether another child turn is needed.
5. Complete the child when its output has been absorbed and no more turns are needed.

Do not leave child tasks open after their output has been absorbed. Do not discard unreviewed child output.

For delegation, make the parent responsible for:

- defining the child scope and expected output
- checking the child's claims against files, diffs, or command output when risk warrants it
- integrating or rejecting the child's work
- deciding what knowledge becomes durable
- closing or explicitly preserving the child task

Make the child responsible for:

- staying within the delegated scope
- reporting concrete files changed, decisions made, tests run, and remaining risks
- avoiding task lifecycle changes unless explicitly instructed

## Worktree Tasks

Before completing a worktree task:

- inspect `git status`
- confirm whether changes are committed, merged, handed off, or intentionally abandoned
- preserve useful verification output in the parent task, PR, commit, or project status file
- use `--keep-worktree` only when further manual inspection is needed

Do not complete a task with unresolved worktree changes unless the user has chosen to abandon them, preserve the worktree, or hand them off.

Do not delete worktrees automatically. Treat worktree removal as destructive cleanup that needs explicit approval.

## Waiting Confirmation

For `waiting_confirmation` tasks, first determine why the task is waiting:

- permission or user-choice prompt: ask the user or provide the requested decision only when you are authorized to decide
- completed useful work: inspect output, promote durable knowledge, then complete with approval
- partial progress: continue only if the original goal is still active and the next turn is clear
- stale or unclear context: summarize what is known and ask the user before resuming risky work

Avoid sending blind "continue" prompts to stale tasks.

## Autorun Tasks

Treat autorun tasks as recurring automation surfaces, not disposable chat threads.

- Keep useful autorun output summarized into durable project files.
- Close stale waiting sessions after their output has been absorbed.
- Do not modify autorun settings without explicit user approval.
- If autorun creates repeated noisy tasks, diagnose the source before removing history.

## Hygiene Budgets

Use these as soft thresholds:

- pinned tasks: 5 or fewer
- `waiting_confirmation`: 20 or fewer
- active implementation tasks per repo: as few as practical
- stale child tasks: summarize, absorb, then complete

If the thresholds are exceeded, run a read-only cleanup scan before creating more tasks. Prefer incremental cleanup before Cockpit becomes slow or hard to scan.

## Cockpit-GC Workflow

`cockpit-gc` is a diagnostic helper, not an automatic janitor.

Recommended flow:

1. Run a read-only scan.
2. Review categories and sample tasks.
3. Promote any durable knowledge.
4. Complete safe tasks only with explicit user approval.
5. Remove only low-value noise with explicit user approval.

Treat `cockpit-gc` categories as recommendations, not authority. A category can identify candidates, but the agent still owns the judgment.

## Never Do Automatically

Without explicit user approval, do not:

- complete tasks
- remove tasks
- kill processes
- delete worktrees
- modify autoruns
- restart Cockpit
- discard unreviewed child output

## Response Pattern

When advising cleanup, report:

- what is active and should be left alone
- what needs summary before closing
- what is safe to complete
- what is safe to remove
- what needs user confirmation

Keep recommendations tied to observable evidence: task status, latest report, worktree state, summary location, or cleanup category.
