---
name: development-review-loop
description: Coordinate software implementation and fixes through a scoped plan, a delegated implementer, and iterative code review. Use for software development tasks; exclude explanation-only requests and broad audits unless explicitly requested.
---

# Development review loop

Keep the original conversation responsible for requirements, planning, review, and delivery. Delegate implementation and corrections to a bounded worker. Read the project's entry point and relevant harness instructions before planning; preserve its conventions and authorization boundaries.

## Roles and availability

- Coordinator and every review: GPT-6 Astra (`gpt-6-astra`), reasoning effort `low`.
- Implementation and corrections: GPT-5.6 Luna (`gpt-5.6-luna`), reasoning effort `high`.
- This skill explicitly requests delegation for implementation. Prefer native subagents within the current task, not separate user-owned tasks.
- A skill does not change the running coordinator's model or effort. Use reliable runtime metadata when available; otherwise state that the coordinator setting is unverified. If a mismatch is known, ask the user to select Astra Low or explicitly authorize a different configuration before delegating implementation or performing the prescribed review; continue read-only preparation where useful. If metadata is unavailable, accept the user's stated setting without asking repeatedly, while keeping verification status explicit. Do not silently substitute another model or claim the prescribed workflow ran.
- Select the worker model and effort explicitly. Where full-history inheritance prevents overrides, start with a fresh context or a supported limited-history fork and provide a self-contained handoff.
- If required delegation or models are unavailable, report the limitation and ask for an alternative before running a different implementation workflow. Continue independent inspection and planning where useful.

## 1. Clarify intent and plan

Inspect the relevant code, interfaces, and available validation environment before planning. Preserve the original intent; ask only about ambiguities that materially affect correctness or scope. Separate observed facts, chosen decisions, and unverified assumptions so the worker need not rediscover settled questions or mistake a hypothesis for a requirement.

Use one concise plan as both the working specification and the implementation handoff. Follow the project's plan location and format; otherwise keep it in the conversation. For substantial work, use an agreed internal project location if one exists. Do not introduce public planning artifacts by default. Cover the following information, merging fields for small tasks rather than expanding them into boilerplate:

- **Outcome:** observable behavior to achieve, constraints, and explicit exclusions.
- **Intervention:** inspected file paths and symbols, intended changes and dependencies, and material decisions with brief reasons. Give enough direction to avoid repeating discovery; leave routine coding choices to the worker instead of prewriting the implementation.
- **Acceptance:** stable IDs such as A1 and A2, each linking a required behavior to concrete verification evidence. Include applicable variants explicitly (for example, both portrait and landscape), not only in a general testing paragraph. Do not invent additional requirements or require a new automated test for every criterion.
- **Validation:** exact commands where known, expected outcomes, and prerequisites. Distinguish required executable checks, blocked required checks, and optional or later checks. A missing environment does not turn a required check into an optional one.
- **Replanning triggers:** discoveries that invalidate a material assumption, change requested behavior, or exceed the permitted scope; state what the worker should report before dependent work continues.
- **Delivery:** changed files, material deviations, acceptance IDs with pass/fail/blocked status and evidence, and remaining limitations. A code inspection can be evidence when appropriate; it is not a substitute for an explicitly required runtime check.

Before delegation, check that every explicit requirement maps to an acceptance criterion, relevant variants have evidence planned, and environment limits are visible. Resolve cheap, material unknowns now; avoid turning planning into a second implementation or a broad repository audit.

## 2. Capture the task baseline

Before any edits, record the current Git revision, staged and unstaged changes, and untracked files relevant to the task. Record the initial contents or a retrievable snapshot of files that will be edited, including pre-existing uncommitted work. Keep snapshots private/local and do not include secrets in reports.

Use the working state at task start as the review baseline, not merely HEAD. Preserve pre-existing work and staging. For newly discovered files, capture their initial state before editing. Track the cumulative task changes through every correction round. If concurrent user or external edits appear, reconcile attribution; do not overwrite or treat those edits as the worker's work. Without Git, use equivalent file snapshots.

## 3. Delegate implementation

Give the worker the current plan once, plus workspace, relevant instruction paths, baseline references, and its bounded assignment. When the plan is in a shared file, point to its exact path instead of duplicating its contents in the handoff; otherwise include the concise plan inline. Do not assume the worker can see the parent conversation. Link supporting files and the relevant sections instead of copying full logs, source files, or exploration history. The worker must still read applicable instructions and inspect code needed to implement safely.

For corrections in the same worker, send the changed decisions, outstanding finding IDs, and verification conditions rather than repeating the whole plan. If the plan changes, identify the current revision or changed sections. Request the delivery report specified above, with short evidence and paths to longer outputs where useful.

Allow one implementation writer at a time. The coordinator can independently prepare acceptance checks or inspect relevant interfaces while the worker works, but must not concurrently edit the worker's files. If the runtime permits delegation only alongside independent useful work, respect that restriction; do not invent busywork to bypass it.

The worker implements and runs proportionate checks. It does not self-approve the task, expand scope, publish, commit, or push unless the user has authorized those actions. Request changes to the plan if implementation reveals a material requirement conflict.

## 4. Review the actual result

Read [review-scope.md](references/review-scope.md) before each review and enforce it throughout the loop.

Inspect the actual cumulative task diff and relevant code, not just the worker summary. Reconcile every acceptance ID with its evidence, including planned variants and blocked checks; a worker's pass label alone is not verification. Maintain findings with stable IDs, severity, file/location, concrete failure scenario, required correction, and verification condition. Distinguish mandatory findings from optional observations.

Return actionable findings to the worker. Default to reusing the same worker with a follow-up so it retains implementation context. Start a fresh Luna High worker if the previous one is unavailable, repeatedly misses the same problem, or its context has become counterproductive. Transfer the current specification, decisions, baseline, outstanding findings, and latest check results; do not rely on lost conversation history.

## 5. Iterate and finish

Review corrections and any regressions they introduce. Close a finding only after inspecting evidence; reopen a resolved finding only when new evidence or a later change warrants it. After two unsuccessful correction attempts for the same issue, diagnose the failure and change approach or replace the worker. Do not use retries as a substitute for a missing user decision or external dependency. If meaningful progress remains blocked, report the exact blocker and unresolved findings; never label the task approved.

Finish when no mandatory in-scope findings remain, acceptance criteria are met, and required checks pass. Perform a final cumulative review against the task baseline, since an incremental correction can invalidate an earlier decision. A blocked required check means verification is incomplete; optional checks not run must be identified separately. Zero findings means no remaining issues identified within the reviewed scope, not proof of defect-free software.

Apply project-specific documentation and delivery rules. Report the implemented outcome, checks performed, remaining limitations, and review status. Commit, push, or publish only within existing user authorization.
