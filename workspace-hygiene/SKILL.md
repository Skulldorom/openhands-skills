---
name: workspace-hygiene
description: Use in permanent or long-lived Git workspaces to prepare repositories, preserve local state, and leave workspaces predictable across sessions and pull requests.
---

# Workspace Hygiene for Permanent Workspaces

This is a global workflow skill for workspaces that persist between tasks.
Treat the workspace as user data: preserve uncommitted work, dependencies,
caches, local configuration, and stashes unless the user or repository policy
explicitly authorizes changing or removing them.

## Before starting a task

1. Discover the workspace scope. Identify every Git repository involved in the
   task, including repositories in a multi-repository workspace. Do not assume
   a project name, directory layout, operating system, remote name, or default
   branch. Avoid operating on nested repositories twice.
2. Read applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, and repository
   documentation. Repository-specific instructions control the required base
   branch, validation, and cleanup policy.
3. Inspect each repository before changing Git state:

   ```bash
   git -C <repo> status --short --branch
   git -C <repo> branch --show-current
   git -C <repo> remote -v
   git -C <repo> stash list
   ```

4. Determine the intended base branch and remote from repository instructions
   or the remote's default branch. If no base branch is specified, use the
   repository's configured default rather than assuming `main`.
5. Establish a clean starting point without destroying local state. If the
   repository has changes, untracked files, or stashes, preserve them and
   account for them before starting. Do not silently reset, clean, switch away
   from a working branch, or overwrite files.
6. If the repository policy requires synchronization to its base branch, use a
   fast-forward-only update when the working tree is already safe to switch:

   ```bash
   git -C <repo> fetch <remote> --prune
   git -C <repo> switch <base-branch>
   git -C <repo> pull --ff-only <remote> <base-branch>
   git -C <repo> status --short --branch
   ```

   If switching or updating would discard work, stop and ask for direction.
   Use `reset --hard` only when the user or an explicit repository policy has
   authorized that exact destructive operation.

## During the task

- Keep changes isolated to the repositories and files in scope.
- Do not use `git clean -fdx` in a permanent workspace as routine hygiene. It
  removes ignored dependencies, virtual environments, caches, build output,
  and local tooling.
- If cleanup is required, inspect first:

  ```bash
  git -C <repo> status --porcelain
  git -C <repo> clean -nxd
  ```

  Remove only the approved paths. Use `git clean -fd` for approved untracked
  files while preserving ignored files; use `git clean -fdx` only when the
  policy explicitly requires ignored files to be removed too.
- Never run `git stash clear` in a permanent workspace. Preserve pre-existing
  stashes and remove only a stash created by the current task, after confirming
  it is no longer needed.
- Do not delete local branches merely to make the workspace look tidy. A PR
  branch may be needed for review updates.

## After creating a pull request

When a PR is created, return affected repositories to the documented base
branch only if doing so is safe:

1. Confirm the task's changes are committed or otherwise accounted for.
2. Switch to the base branch without overwriting changes.
3. Remove only session-created temporary artifacts that are known to be safe to
   remove. Leave dependencies, caches, ignored tooling, pre-existing stashes,
   and local branches intact unless explicit instructions say otherwise.
4. Verify and report the final state:

   ```bash
   git -C <repo> status --short --branch
   git -C <repo> diff --stat
   ```

If the workspace cannot safely return to the base branch, do not force it. Tell
the user which repository, branch, changes, or artifacts remain and why.

## Completion criteria

Before reporting completion, verify that:

- every repository in scope was identified;
- applicable repository instructions were followed;
- no user changes, stashes, dependencies, caches, or branches were removed
  without authorization; and
- the final Git state and any intentional deviations are clearly reported.
