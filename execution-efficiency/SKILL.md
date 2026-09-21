---
name: execution-efficiency
description: Use when carrying out software-engineering work in a Git workspace and you need an efficient, safe workflow for repository preparation, implementation, validation, and cleanup.
---

# Efficient Software Engineering and Workspace Hygiene

Work thoroughly and correctly while minimizing unnecessary model, tool, and
terminal turns. Efficiency must never come at the expense of correctness,
security, testing, or required investigation.

## General Execution

Before making changes:

- Understand the user's complete request.
- Inspect the relevant repository structure, files, tests, configuration, and
  current implementation before editing.
- Batch related searches, file reads, and repository inspection whenever
  practical.
- Gather enough evidence to form a coherent implementation plan before making
  changes.
- Prefer targeted searches over broad repeated exploration.

Do not repeatedly inspect information that is already known unless:

- the underlying file or state may have changed,
- a test failure provides new evidence,
- or additional investigation is genuinely required.

Avoid unnecessary repeated calls to:

- git status
- git diff
- git log
- repository-wide searches
- dependency inspection
- identical tests
- CI status
- unchanged files

Combine related shell commands when doing so remains readable and safe.

## Implementation

Implement logically related changes together.

Prefer:

inspect -> understand -> plan -> implement related changes -> focused validation
-> fix issues -> full validation -> review diff

Avoid:

inspect -> tiny edit -> test -> inspect -> tiny edit -> test -> inspect -> test

Do not make speculative changes.

Do not expand the scope of the task merely because unrelated improvements are
noticed. Report unrelated issues separately unless they must be fixed for the
requested work to function correctly.

Follow existing project architecture, conventions, abstractions, and patterns
unless the task explicitly requires changing them.

Prefer simple, maintainable solutions over unnecessary abstractions.

## Testing and Validation

Testing is required when appropriate, but use it efficiently.

During implementation:

- Run focused tests when they provide useful feedback.
- Prefer the smallest relevant test subset while actively developing.
- Do not rerun an unchanged test suite after every small edit.
- If several related edits are needed, complete them before rerunning the
  relevant tests when practical.

After implementation:

1. Run the relevant tests.
2. Run applicable linting, formatting, type checking, validation, and builds.
3. Fix genuine failures.
4. Rerun affected checks after fixes.
5. Perform one final complete validation appropriate for the repository.

Do not repeatedly rerun successful full validation without a reason.

If a change after validation could affect previously validated behavior,
rerun the appropriate checks.

Never skip necessary tests or validation merely to save tokens.

## Investigation

Investigate deeply when necessary to establish correctness.

Continue investigating when:

- behavior is unclear,
- requirements conflict with implementation,
- a failure is unexplained,
- security or data integrity may be affected,
- compatibility is uncertain,
- or assumptions cannot safely be made.

Stop investigating when sufficient evidence already establishes the answer.

Do not continue searching merely to reconfirm the same conclusion through
additional equivalent evidence.

When inspecting external/upstream code, dependencies, generated bundles, or
container images, first determine exactly what information is required and
use the narrowest effective investigation.

## Tool Usage

Treat every tool call as purposeful.

Before using a tool, know what question the result is intended to answer.

Prefer:

- one repository search for several related terms,
- reading several relevant files together,
- one well-constructed shell command,
- targeted tests,
- batched independent operations where safe.

Avoid breaking a single investigation into many tiny commands when they can
reasonably be combined.

Do not use terminal commands simply to narrate progress.

Do not repeatedly retrieve large files or outputs that are already available
in context.

When command output is very large, narrow or filter it whenever possible.

## Token and Context Efficiency

Preserve context-window capacity for reasoning about the actual task.

Do not restate large portions of:

- the user's prompt,
- source files,
- test output,
- command output,
- previous reasoning,
- or already-established findings.

Keep internal planning focused on decisions that affect implementation.

Summarize large findings rather than repeatedly carrying raw output forward
when possible.

Token efficiency is an optimization objective, not a correctness objective.

Never sacrifice:

- correctness,
- security,
- data integrity,
- backward compatibility,
- necessary investigation,
- required testing,
- or user requirements

merely to reduce token usage.

## Workspace preparation

Apply this procedure when the workspace or repository instructions require a
clean, up-to-date baseline before work begins. It is intentionally repository-
agnostic: discover the repositories and their default branches instead of
assuming a project name, path, remote, or branch called `main`.

1. Identify every repository in scope. For a multi-repository workspace, list
   them explicitly and avoid operating on nested repositories twice.
2. Read applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, and repository
   documentation before changing Git state.
3. For each repository, determine the intended remote and base branch. Prefer
   the remote's default branch (`origin/HEAD` when available), then the
   repository's documented default, and only then a conventional fallback such
   as `main`.
4. Inspect the current state before any destructive operation:

   ```bash
   git -C <repo> status --short --branch
   git -C <repo> clean -nxd
   ```

5. If the applicable workspace policy explicitly authorizes discarding local
   changes and ignored files, synchronize the repository to its base branch:

   ```bash
   git -C <repo> fetch <remote> --prune
   git -C <repo> switch <base-branch>
   git -C <repo> reset --hard <remote>/<base-branch>
   git -C <repo> clean -fdx
   git -C <repo> status --short --branch
   ```

   Replace the placeholders with values discovered for that repository. Do not
   run `reset --hard`, `clean -fdx`, or an equivalent operation merely because
   it is convenient. If local work is present and the policy does not clearly
   authorize its removal, preserve it and ask for direction.

6. Verify that every repository is on the intended base branch, synchronized
   with its remote, and clean before starting implementation.

Do not assume all workspaces contain two repositories, use `/root/projects`, or
have `origin/main`. The same procedure should work for one repository, a
monorepo, or several repositories with different remotes and default branches.

## Workspace cleanup after a pull request

When a task finishes by creating a pull request, return affected repositories
to the documented base branch before reporting completion. For each affected
repository:

1. Switch back to its base branch. Leave the PR branch available locally so it
   can be checked out again if the PR needs updates; do not delete it.
2. Remove untracked or ignored files only when the repository policy requires a
   disposable checkout and the cleanup scope was inspected with `git clean -nxd`.
3. Restore or remove only stashes created during the current task. Never clear
   a user's pre-existing stash entries just to make the workspace look clean.
4. Verify the final state:

   ```bash
   git -C <repo> status --short --branch
   git -C <repo> diff --stat
   ```

   The expected result is the base branch with no working-tree changes. If
   cleanup is not authorized or cannot be completed safely, report the exact
   remaining state instead of hiding it.

## Git

Do not repeatedly check repository status throughout the task unless needed.

Before finishing:

- inspect the final diff,
- confirm only intended changes were made,
- ensure required validation passes.

If the user requested a commit:

- commit the completed verified changes.

If the user requested a push:

- push only after verification succeeds.

Do not create unrelated commits or modify unrelated files.

## Completion

Before declaring the task complete, verify:

- every explicit user requirement was addressed,
- relevant edge cases were considered,
- tests appropriate to the change pass,
- applicable validation passes,
- no accidental changes remain,
- and requested Git operations were completed.

Report the result concisely.

Do not continue performing additional investigation or validation after these
conditions are satisfied unless there is a concrete unresolved concern.
