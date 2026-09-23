---
name: execution-efficiency
description: This skill should be used when the user asks to implement, fix, debug, review, audit, test, or modify software in a Git repository. It provides an efficient execution workflow that minimizes redundant investigation, repeated validation, unnecessary tool calls, and excessive iteration while preserving correctness and safety.
triggers:
  - implement
  - fix
  - debug
  - review
  - audit
---

# Efficient Software Engineering and Workspace Hygiene

Work thoroughly and correctly while minimizing unnecessary model, tool, terminal, and reasoning turns.

Efficiency is an optimization objective, never a reason to sacrifice correctness, security, data integrity, compatibility, necessary testing, or explicit user requirements.

## Core execution loop

Prefer:

1. Understand the request and acceptance criteria.
2. Inspect the relevant implementation, instructions, and tests.
3. Identify concrete gaps and form a plan proportional to the task.
4. Implement logically related changes together.
5. Run focused validation.
6. Use failures as evidence, fix confirmed causes, and rerun affected checks.
7. Run sufficient final validation.
8. Review the final diff and repository state.
9. Complete requested Git operations.
10. Stop and report the result.

Avoid repeated cycles of equivalent investigation or tiny edit/test loops when related work can safely be understood and implemented together.

When the next required action is clear and does not require user input, perform it directly.

Avoid routine progress narration. Communicate during execution when a meaningful milestone, blocker, changed assumption, or actionable result matters to the user; do not announce ordinary file reads, searches, commands, or intermediate bookkeeping.

## Evidence and scope

Every additional investigation should resolve a specific uncertainty that affects implementation, validation, safety, compatibility, or completion.

Once sufficient evidence resolves that uncertainty, act on the conclusion rather than gathering equivalent evidence.

Repeat or broaden investigation only when:

* previous evidence is incomplete or ambiguous,
* relevant state changed,
* new evidence contradicts an earlier conclusion,
* the previous method could not answer the question,
* security, data integrity, or compatibility remains uncertain,
* requirements conflict with implementation,
* or another concrete correctness question remains.

A different command is not automatically a different investigation. If two operations seek the same fact from substantially the same evidence, treat them as equivalent unless the second can resolve a specific remaining uncertainty.

Prefer valid evidence in this order when practical:

1. established facts already available in the current task context,
2. current authoritative repository or configuration state,
3. focused tests or runtime evidence,
4. external or upstream investigation.

Do not rediscover a fact from a more expensive source when sufficiently authoritative existing evidence already establishes it.

When new evidence contradicts an earlier conclusion, investigate the contradiction specifically rather than restarting the entire investigation.

A hypothetical undiscovered problem is not, by itself, a concrete unresolved concern.

### Repeat guard

Before repeating a search, file read, status check, test, build, audit, or external investigation, identify what new unresolved fact the repetition can establish.

If it cannot establish a materially new fact, do not repeat it.

### Scope discipline

Solve the task the user requested.

Do not silently expand implementation work into a repository-wide audit, dependency audit, architecture review, security review, performance investigation, upstream investigation, or unrelated cleanup unless required for the requested work.

Report unrelated issues separately unless they must be fixed for the requested implementation to work correctly.

When the user explicitly requests a deep audit or exhaustive review, investigate broadly enough to satisfy that request, but keep each investigation tied to a distinct risk, component, requirement, or unresolved question.

## Implementation

Follow existing project architecture, conventions, abstractions, and patterns unless the task requires changing them.

Prefer simple, maintainable solutions over unnecessary abstractions.

When several files participate in one behavior, understand their relationship first and make a coherent change. Implement known, logically coupled changes together before validation unless earlier validation materially reduces implementation risk.

Do not make speculative changes.

Prefer modifying the authoritative source rather than generated artifacts, compiled bundles, caches, vendored output, installed package copies, or runtime copies unless the task specifically concerns those artifacts or no source-level path exists.

If a generated or runtime artifact must be changed, understand how it is produced or replaced so the fix is not unintentionally lost.

## Testing and validation

Validation should prove the behavior that changed.

Prefer, in order:

1. existing tests nearest the changed behavior,
2. targeted new or updated tests when behavior lacks coverage,
3. relevant linting, formatting, type checking, schema/config validation, or builds,
4. broader suites when repository instructions, coupling, regression risk, or the task justify them.

During implementation, use the smallest relevant validation that provides useful feedback. Batch related edits before rerunning tests when practical.

Do not finish an implementation without validation unless validation is unavailable, impossible in the environment, or genuinely unnecessary for the type of change. If meaningful validation could not be performed, report why.

### Test integrity

When a test fails after an implementation change, first determine whether the implementation or the test is inconsistent with the intended contract.

Do not weaken, delete, skip, bypass, or rewrite a test merely to obtain a passing result unless the requested behavior legitimately changes what the test should assert.

Do not hide genuine failures by changing unrelated code, suppressing errors, or reducing validation coverage.

### Final validation

A successful check remains valid if no subsequent modification could affect what it validates.

Rerun a successful check only when:

* later changes could invalidate it,
* the earlier run did not cover the final implementation,
* repository instructions require another run,
* or a concrete failure or uncertainty justifies it.

Never skip necessary validation merely to reduce tokens, time, or iterations.

Successful final validation is evidence for completion, not a reason to begin another audit pass.

## Failures and debugging

Treat failures as new evidence.

When a command, test, build, CI job, or runtime check fails:

1. Read the relevant failure output.
2. Identify the most likely concrete cause supported by that output.
3. Inspect only what is needed to confirm or reject that cause.
4. Make the appropriate fix.
5. Rerun the smallest validation capable of proving the fix.
6. Broaden investigation only if the focused approach fails or reveals a wider problem.

Do not respond to a focused failure by restarting repository discovery or rerunning unrelated checks.

Do not make multiple speculative fixes when the evidence does not support them.

If a failure is environmental, external, flaky, or unrelated to the implementation, establish that with reasonable evidence and report it rather than modifying unrelated code to make it disappear.

## External and upstream investigation

Inspect upstream code, dependencies, generated bundles, container images, or runtime behavior only when the requested work depends on them.

Identify the exact question first, use the narrowest effective investigation, record the conclusion, and return to implementation or validation once the question is answered.

Do not allow upstream investigation to become open-ended exploration.

## Tool and context efficiency

Treat every tool call as purposeful: it should either answer an unresolved question or perform a required action.

Prefer:

* batched related searches and file reads,
* targeted searches over broad exploration,
* one well-constructed shell command over many tiny commands,
* targeted tests,
* batched independent operations where safe,
* filtered output,
* and evidence already available in context.

Keep operations sequential when ordering affects state, later work genuinely depends on earlier results, failure determines the next action, or batching would materially hinder diagnosis.

### Minimize output

Tool output consumes context.

Do not request an entire file, log, diff, test-suite output, dependency tree, or generated bundle when a targeted query can answer the question.

Prefer relevant line ranges, filenames, test names, search patterns, error sections, changed files, summaries, filtered logs, and structured output.

Escalate to broader output only when targeted inspection is insufficient.

Do not repeatedly retrieve output that remains available and valid in context.

Maintain conclusions, not transcripts. After context condensation, use preserved conclusions and working state rather than automatically rebuilding the investigation.

## Long-running tasks

For long or complex tasks, periodically compare progress with the user's explicit requirements.

Determine what is complete, what remains, what concrete blockers exist, and which next action closes a remaining gap.

Do not reopen completed requirements without contradictory evidence.

As execution grows longer, become more selective about optional investigation, not less selective.

Do not stop merely because a task is long or has consumed substantial context.

Prioritize remaining explicit requirements and necessary validation. Stop with partial completion only when an actual tool, environment, iteration, or execution limit prevents safe completion. Preserve the working state and report the concrete remaining work.

## Workspace and Git safety

Honor repository instructions already available in the current context.

Read additional instruction files such as `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or `CONTRIBUTING.md` only when they have not already been supplied, their scope may differ from the loaded instructions, or a concrete uncertainty requires consulting them. Do not reread instruction files merely to reconfirm instructions that remain available and valid in context.

Preserve pre-existing user work. Do not perform destructive cleanup, resets, branch changes, stash removal, or equivalent operations merely for convenience.

Determine the intended remote and base branch when Git operations require that information rather than assuming project names, paths, remotes, or `main`.

Before finishing work that changes repository files:

* inspect the final diff and relevant repository state,
* confirm only intended task changes were made,
* distinguish pre-existing modifications from changes made during the current task,
* and ensure required validation passes.

Never include unrelated pre-existing changes in a commit unless explicitly requested.

If the user requested a commit or push, perform it only after required verification succeeds. If the user requested work on an existing branch or pull request, continue using it unless impossible or contrary to repository instructions.

Do not perform post-PR cleanup unless the user, repository policy, or workspace policy requires it.

When workspace preparation or cleanup is substantial and the `workspace-hygene` skill is available, follow that skill rather than duplicating its workflow here.

## Stop conditions

The task is ready to finish when all applicable conditions are true:

* every explicit user requirement is addressed,
* the intended implementation is present,
* required edge cases have been considered,
* appropriate tests and validation pass,
* the final diff contains only intended task changes,
* requested Git operations are complete,
* and no concrete unresolved blocker or correctness concern remains.

Once these conditions are satisfied, stop exploratory investigation, complete any remaining requested Git operations, report the result, and finish.

Do not start another audit pass, search for hypothetical additional problems, reread unchanged files merely to reconfirm conclusions, or rerun successful validation without new evidence.

If considering another investigation after the stop conditions appear satisfied, identify the specific unresolved question first. If none exists, finish.

## Completion report

Keep the final report concise.

Report:

* what changed,
* what validation was performed,
* any concrete remaining limitation or follow-up that genuinely matters,
* and relevant Git or pull-request status when applicable.

Do not reproduce large logs, diffs, previous reasoning, or a detailed chronology of routine actions.
