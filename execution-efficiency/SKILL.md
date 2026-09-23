---
name: execution-efficiency
description: Use when carrying out software-engineering work in a Git workspace and you need an efficient, safe workflow for repository preparation, implementation, validation, and cleanup.
---

# Efficient Software Engineering and Workspace Hygiene

Work thoroughly and correctly while minimizing unnecessary model, tool, terminal, and reasoning turns.

Efficiency must never come at the expense of correctness, security, data integrity, required compatibility, necessary testing, or explicit user requirements.

The objective is not to perform the maximum possible investigation. Gather enough evidence to make correct decisions, implement the requested work, validate it appropriately, and finish.

## Core execution loop

Prefer:

1. Understand the request and acceptance criteria.
2. Inspect the relevant existing implementation.
3. Identify the concrete gaps.
4. Form a plan proportional to the task's complexity.
5. Implement logically related changes together.
6. Run focused validation where useful.
7. Fix failures based on the evidence they provide.
8. Run appropriate final validation.
9. Review the final diff and repository state.
10. Complete requested Git operations.
11. Stop and report the result.

Avoid cycles such as:

```text
inspect -> inspect more -> reconfirm -> audit -> inspect again
```

or:

```text
tiny edit -> test -> inspect -> tiny edit -> test -> inspect -> test
```

Every substantial investigation should answer a concrete unresolved question and lead toward a decision, implementation, validation step, or identified blocker.

When the next required action is clear and does not require user input, perform it directly.

Do not spend separate turns narrating what you are about to do, restating the plan, summarizing intermediate progress solely for bookkeeping, or announcing routine tool usage.

## Planning discipline

Scale planning effort to task complexity.

For straightforward changes:

* identify the affected implementation,
* determine the required change,
* identify relevant validation,
* and proceed.

Do not produce or maintain an extensive implementation plan when the required implementation is already clear.

For complex changes, form enough of a coherent plan to avoid discovering the implementation one tiny edit at a time.

Planning exists to reduce rework, not to create another investigation phase.

Once the plan is sufficiently clear to implement safely, proceed rather than continuing to refine it without a concrete reason.

## Investigation and evidence

Before making changes:

* Understand the user's complete request.
* Identify explicit acceptance criteria when available.
* Inspect the relevant implementation, configuration, tests, and instructions.
* Batch related searches and file reads where practical.
* Prefer targeted searches over broad exploration.
* Distinguish required work from optional improvements.

Investigation exists to answer concrete questions.

Before another investigation step, identify what unresolved question its result is intended to answer.

Once sufficient evidence answers that question, make the decision and continue.

Do not replace a decision with another equivalent investigation merely to gain additional confidence.

### Evidence-sufficiency rule

Do not repeat the same or substantially equivalent investigation once the available evidence is sufficient to answer the question.

Repeat or broaden an investigation only when at least one of the following is true:

* the previous result was incomplete,
* the previous result was ambiguous,
* relevant state has changed,
* new evidence contradicts the previous conclusion,
* the previous method or scope could not answer the question,
* security or data integrity remains uncertain,
* compatibility remains unresolved,
* requirements conflict with implementation,
* or another concrete correctness question remains.

Examples of unnecessary repetition include:

* repeating repository searches with equivalent terms after the relevant implementation is already located,
* rereading unchanged files for the same purpose,
* repeatedly checking `git status` without relevant intervening changes,
* rerunning an unchanged successful test,
* repeatedly checking the same CI run without a state change,
* inspecting the same dependency or upstream implementation for a fact already established.

A different command is not automatically a different investigation.

If two operations seek the same fact from substantially the same evidence, treat them as equivalent unless the second method can resolve a specific uncertainty left by the first.

When new evidence contradicts an earlier conclusion, investigate the contradiction specifically rather than restarting the entire investigation.

A hypothetical possibility that an undiscovered problem might exist is not, by itself, a concrete unresolved concern.

## Scope discipline

Solve the task the user requested.

Do not silently expand an implementation task into:

* a repository-wide audit,
* dependency audit,
* architecture review,
* security review,
* upstream compatibility investigation,
* performance investigation,
* or unrelated cleanup

unless required for the requested work.

Report unrelated issues separately unless they must be fixed for the requested implementation to function correctly.

When the user explicitly requests a deep audit or exhaustive review, broader investigation is appropriate.

Even during a deep audit, every additional investigation should address a distinct risk, component, requirement, or unresolved question. Do not repeatedly validate findings already supported by sufficient evidence.

## Implementation

Implement logically related changes together.

Prefer:

```text
inspect
-> understand
-> plan
-> implement related changes
-> focused validation
-> fix evidence-backed issues
-> final validation
-> review diff
-> finish
```

Avoid:

```text
inspect
-> tiny edit
-> test
-> inspect
-> tiny edit
-> test
-> inspect
-> test
```

Do not make speculative changes.

Follow existing project architecture, conventions, abstractions, and patterns unless the task explicitly requires changing them.

Prefer simple, maintainable solutions over unnecessary abstractions.

When several files participate in one behavior, understand their relationship first and make a coherent change rather than treating each file as a separate investigation.

When several known changes are logically coupled, implement them together before validation unless an earlier validation step is necessary to reduce meaningful implementation risk.

## Testing and validation

Testing is required when appropriate, but every test execution should have a purpose.

During implementation:

* Prefer the smallest relevant test subset that provides useful feedback.
* Batch related edits before rerunning tests when practical.
* Do not rerun an unchanged successful test after every small edit.
* Use failures to narrow the next investigation.
* Do not respond to a focused failure by restarting broad repository investigation unless the failure invalidates the underlying assumptions.

After implementation:

1. Run relevant tests.
2. Run applicable linting, formatting, type checking, validation, and builds.
3. Fix genuine failures.
4. Rerun checks affected by those fixes.
5. Confirm that sufficient final validation exists for the completed state.

### Final-validation rule

"Final validation" does not mean automatically rerunning every previously successful check.

If a check passed after the last modification capable of affecting what that check validates, treat that result as final evidence.

Rerun a successful check only when:

* subsequent changes could invalidate it,
* the earlier run did not cover the final implementation,
* repository instructions explicitly require another run,
* or a concrete failure or uncertainty justifies it.

Do not rerun unrelated successful checks.

Never skip necessary validation merely to reduce tokens, time, or iterations.

A successful final validation is evidence that the implementation is ready for completion. It is not a reason to begin another audit pass.

## Failures and debugging

Treat failures as new evidence.

When a command, test, build, CI job, or runtime check fails:

1. Read the relevant failure output.
2. Identify the most likely concrete cause supported by that output.
3. Inspect only the code or state needed to confirm or reject that cause.
4. Make the appropriate fix.
5. Rerun the smallest validation capable of proving the fix.
6. Broaden investigation only if the focused approach fails or reveals a wider problem.

Do not respond to every failure by restarting repository discovery, rereading all relevant files, or rerunning unrelated checks.

Do not make multiple speculative fixes at once when the failure does not support them.

If the failure is environmental, external, flaky, or unrelated to the implementation, establish that with reasonable evidence and report it rather than modifying unrelated code to make the failure disappear.

## External and upstream investigation

When inspecting external or upstream code, dependencies, generated bundles, container images, or runtime behavior:

1. Identify the exact question that must be answered.
2. Use the narrowest effective investigation.
3. Record the conclusion.
4. Return to implementation or validation once the question is answered.

Do not allow upstream investigation to become open-ended exploration.

If existing evidence already answers the required question, use it instead of repeating the investigation.

Do not inspect upstream implementation merely because it is available. Inspect it when behavior, compatibility, or correctness of the requested work actually depends on it.

## Long-running tasks

For long or complex tasks, periodically compare progress against the user's explicit requirements.

At a checkpoint, determine:

* what is complete,
* what remains incomplete,
* what concrete blockers exist,
* and what next action closes a remaining gap.

Do not reopen completed requirements without new contradictory evidence.

As a task grows longer, become more selective about optional investigation rather than less selective.

Approaching an iteration, context, time, or cost limit is a reason to prioritize remaining required work and completion. It is not a reason to reduce necessary correctness or safety checks.

If the task cannot safely be completed within the available execution budget, preserve the working state and report the concrete remaining work instead of spending the remaining budget on repetitive investigation.

## Tool usage

Treat every tool call as purposeful.

Before using a tool, know either:

* what unresolved question its result should answer, or
* what required action it performs.

Prefer:

* one repository search for several related terms,
* reading several relevant files together,
* one well-constructed shell command,
* targeted tests,
* batched independent operations where safe,
* filtered output,
* and evidence already available in context.

### Batch independent operations

When several operations are independent, perform them together when the available tools support it safely.

For example, independent searches, file reads, repository-state checks, or validation commands should not automatically become separate reasoning cycles.

Keep operations sequential when:

* a later operation genuinely depends on the result of an earlier one,
* ordering affects repository state,
* commands modify shared state,
* failure of one operation determines whether another should run,
* or batching would make failures materially harder to diagnose.

Do not split one investigation into many tiny commands merely to observe each intermediate result.

### Minimize output

Tool output consumes context and should be treated as a limited resource.

Never request an entire file, log, diff, test-suite output, dependency tree, generated bundle, or other large artifact when a targeted query can answer the current question.

Prefer targeted:

* line ranges,
* filenames,
* test names,
* search patterns,
* error sections,
* changed files,
* summaries,
* filtered logs,
* and structured output.

Escalate to broader output only when targeted inspection is insufficient.

When broad output is necessary, filter or summarize it as early as practical instead of repeatedly carrying the raw output through subsequent reasoning.

Do not retrieve large output merely so it can immediately be summarized when a targeted query could answer the question directly.

Do not repeatedly retrieve output already available and still valid in context.

### Avoid narration through tools

Do not use terminal commands or other tool calls merely to narrate progress, print separators, restate conclusions, or create artificial checkpoints.

Use tools to gather evidence, modify state, validate work, or perform another required action.

## Token and context efficiency

Preserve context-window capacity for reasoning about the actual task.

Do not unnecessarily restate:

* the user's prompt,
* source files,
* test output,
* command output,
* previous reasoning,
* established findings,
* known repository state,
* or the current plan.

Summarize large findings rather than repeatedly carrying raw output forward.

Maintain conclusions, not transcripts.

After conversation or memory condensation, use preserved conclusions and working state rather than automatically rebuilding the entire investigation.

Reinspect only information that:

* may have changed,
* was lost,
* conflicts with new evidence,
* or is required to resolve a concrete uncertainty.

Do not trade many small model turns for one operation that can safely answer the same question.

When the next action is already determined, execute it rather than spending another reasoning turn restating why it should be executed.

Token efficiency is an optimization objective, not a correctness objective.

Never sacrifice:

* correctness,
* security,
* data integrity,
* backward compatibility,
* necessary investigation,
* required testing,
* or user requirements

merely to reduce token usage.

## Workspace preparation

Apply workspace preparation only when repository instructions, workspace policy, or the requested task requires a clean or synchronized baseline.

Do not perform destructive cleanup merely as routine preparation.

1. Identify the repositories in scope.
2. Read applicable repository instruction files such as:

   * `AGENTS.md`
   * `CLAUDE.md`
   * `CONTRIBUTING.md`
3. Read only additional documentation relevant to the requested task.
4. Determine the intended remote and base branch rather than assuming project names, paths, remotes, or `main`.
5. Inspect repository state before destructive operations.

Useful inspection commands include:

```bash
git -C <repo> status --short --branch
git -C <repo> clean -nxd
```

When a clean synchronized baseline is explicitly required, prefer discovering:

* `origin/HEAD` when available,
* the repository's documented default branch,
* then a conventional fallback such as `main`.

If workspace policy explicitly authorizes discarding local changes and ignored files, synchronization may use:

```bash
git -C <repo> fetch <remote> --prune
git -C <repo> switch <base-branch>
git -C <repo> reset --hard <remote>/<base-branch>
git -C <repo> clean -fdx
git -C <repo> status --short --branch
```

Replace placeholders with discovered values.

Never run `reset --hard`, `clean -fdx`, or equivalent destructive operations merely because they are convenient.

If local work exists and policy does not clearly authorize its removal, preserve it and ask for direction.

When a clean baseline is required, verify the repository is on the intended base branch, synchronized with its remote, and clean before implementation.

## Git workflow

Do not repeatedly check repository status unless repository state may have changed or the result is required for the next action.

Before finishing:

* inspect the final diff,
* confirm only intended changes were made,
* ensure required validation passes.

Do not turn final diff review into a new general repository audit.

If the final diff reveals a concrete problem, fix that problem and rerun only the checks affected by the fix.

If the user requested a commit, commit the completed verified changes.

If the user requested a push, push only after required verification succeeds.

Do not create unrelated commits or modify unrelated files.

If the user requested work on an existing branch or pull request, continue using it unless doing so is impossible or conflicts with explicit repository instructions.

Do not silently create replacement branches or pull requests when the user requested updates to an existing one.

## Cleanup after a pull request

Perform cleanup after creating or updating a pull request only when repository or workspace policy requires it.

When cleanup is required:

1. Return affected repositories to their documented base branches if policy requires it.
2. Leave PR branches available unless policy explicitly says otherwise.
3. Remove untracked or ignored files only when cleanup is authorized and the scope was first inspected with `git clean -nxd`.
4. Restore or remove only stashes created during the current task.
5. Never clear pre-existing user stashes merely to make the workspace clean.
6. Verify the resulting state.

Useful verification commands include:

```bash
git -C <repo> status --short --branch
git -C <repo> diff --stat
```

If cleanup cannot safely be completed, report the exact remaining state instead of hiding it.

## Stop conditions

Actively determine when the task is finished.

The task is ready to finish when all applicable conditions are true:

* every explicit user requirement has been addressed,
* the intended implementation is present,
* required edge cases have been considered,
* appropriate tests pass,
* applicable validation passes,
* the final diff contains only intended changes,
* requested Git operations are complete,
* and no concrete unresolved blocker or correctness concern remains.

Once these conditions are satisfied:

1. Stop exploratory investigation.
2. Complete any remaining requested Git operations.
3. Report the result.
4. Finish.

Do not start another audit pass, search for hypothetical additional problems, reread unchanged files merely to reconfirm conclusions, or rerun successful validation without new evidence.

If considering another investigation after the stop conditions appear satisfied, identify the specific unresolved question first.

If no concrete unresolved question exists, finish.

## Completion report

Keep the final report concise.

Report:

* what changed,
* what validation was performed,
* any concrete remaining limitation or follow-up that genuinely matters,
* and relevant Git or pull-request status when applicable.

Do not reproduce large logs, diffs, previous reasoning, or a detailed chronological account of every action performed.

Do not report routine investigative steps unless they materially explain the result.

Once the completion conditions are satisfied, finish immediately.
