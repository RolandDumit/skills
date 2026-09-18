---
name: development-review-loop
description: Coordinate software implementation and fixes through scoped planning, selective delegation, and evidence-based review. Use for software development tasks; exclude explanation-only requests and broad audits unless explicitly requested.
---

# Development review loop

Keep the original conversation responsible for requirements, planning, review, and delivery. Optimize for completed, verified tasks within the user's usage allowance, not raw token counts or maximum parallelism. Use the project's harness as the source of truth: follow its entry point, index or graph to relevant instructions and checks. Do not recreate its documentation, routing, or policies in this skill or the task plan.

## Roles and availability

- Coordinator and every review: GPT-6 Astra (`gpt-6-astra`), reasoning effort `low`.
- Delegated implementation and corrections: GPT-5.6 Luna (`gpt-5.6-luna`), reasoning effort `high`. Keep this default; use Medium only for an explicitly requested comparison, not as an assumed saving.
- Choose the route using ambiguity, risk, repetition, and verifiability. Astra implements tiny, already-understood changes directly when handoff overhead would dominate. Delegate clear, bounded work to Luna, including substantial repetitive work. Astra resolves material design uncertainty first; retain decision-heavy implementation in Astra when delegation would require continuous supervision. State the route and a brief reason without adding an approval step.
- This skill explicitly requests delegation when the Luna route is selected. Prefer native subagents with fresh, bounded context (`fork_turns: "none"` where supported), not manual thread changes or full conversation inheritance. On the direct route, apply the same baseline, acceptance, checks, and scoped review requirements; do not claim independent review.
- A skill does not change the running coordinator's model or effort. Use reliable runtime metadata when available; otherwise state that the coordinator setting is unverified. If a mismatch is known, ask the user to select Astra Low or explicitly authorize a different configuration before delegating implementation or performing the prescribed review; continue read-only preparation where useful. If metadata is unavailable, accept the user's stated setting without asking repeatedly, while keeping verification status explicit. Do not silently substitute another model or claim the prescribed workflow ran.
- Select the worker model and effort explicitly. Where full-history inheritance prevents overrides, start with a fresh context or a supported limited-history fork and provide a self-contained handoff.
- If Luna is unavailable, report it and use the authorized direct Astra route when feasible. If the required coordinator model is unavailable, ask for an alternative; continue independent inspection and planning where useful.

## 1. Clarify intent and plan

Inspect enough relevant code, interfaces, and validation prerequisites to identify the problem, risk, and decisions needed for the chosen route. Leave detailed implementation discovery to the implementer; do not systematically explore the same code in both agents. Preserve the original intent; ask only about ambiguities that materially affect correctness or scope. Separate observed facts, chosen decisions, and unverified assumptions so the worker need not rediscover settled questions or mistake a hypothesis for a requirement.

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

## 3. Implement with bounded autonomy

On the direct route, Astra implements and verifies the scoped plan without creating a worker. Otherwise use the following handoff and coordination rules. A worker receiving a bounded assignment executes it; it does not restart coordinator planning or delegate another implementation loop.

Give the worker the current plan once, plus workspace, relevant instruction paths, baseline references, and its bounded assignment. When the plan is in a shared file, point to its exact path instead of duplicating its contents in the handoff; otherwise include the concise plan inline. Do not assume the worker can see the parent conversation. Link supporting files and the relevant sections instead of copying full logs, source files, or exploration history. The worker must still read applicable instructions and inspect code needed to implement safely.

For corrections in the same worker, send the changed decisions, outstanding finding IDs, and verification conditions rather than repeating the whole plan. If the plan changes, identify the current revision or changed sections. Request the delivery report specified above, with short evidence and paths to longer outputs where useful.

Allow one implementation writer at a time. The coordinator may do genuinely independent work while the worker runs, but must not duplicate its exploration, checks, or edits merely to stay active. If the runtime permits delegation only alongside independent useful work and none remains, use the direct route; do not invent busywork.

Give a complete assignment, then let the worker execute. Do not request routine status, poll agent lists, or send step-by-step suggestions without new evidence or a decision to communicate. Use completion notifications or supported waits within runtime limits. Keep required user-facing updates concise and based on available evidence; they do not require extra worker queries. Intervene for a concrete blocker, requirement conflict, unexpected risk, or user steering.

The worker completes implementation and required executable checks before delivery, fixing ordinary build, lint, and test failures autonomously within scope. It reports blockers rather than looping on unavailable dependencies or changing requirements. It does not self-approve the task, expand scope, publish, commit, or push unless the user has authorized those actions. Request changes to the plan if implementation reveals a material requirement conflict.

## 4. Review the actual result

Read [review-scope.md](references/review-scope.md) before the first review and enforce it throughout the loop. Reread only if it changes or its content is no longer available in context.

Inspect the actual cumulative task diff and relevant code, not just the worker summary. Reconcile every acceptance ID with its evidence, including planned variants and blocked checks; a worker's pass label alone is not verification. Maintain findings with stable IDs, severity, file/location, concrete failure scenario, required correction, and verification condition. Distinguish mandatory findings from optional observations.

Review the complete delivery instead of supervising each implementation step. Inspect check evidence; rerun checks when evidence is insufficient, changes invalidate it, project rules require it, or a concrete concern warrants reproduction. Do not repeat valid checks merely because a different agent ran them.

Return actionable findings to the same worker by default. Diagnose repeated misses: clarify the contract if unclear; let Astra resolve design or reasoning problems and, when needed, take over implementation after stopping the worker. Replace Luna only when availability or counterproductive context warrants it, transferring current decisions, baseline, outstanding findings, and check results. Do not replace the worker as a substitute for fixing the assignment.

## 5. Iterate and finish

Review corrections and any regressions they introduce. Close a finding only after inspecting evidence; reopen a resolved finding only when new evidence or a later change warrants it. Change approach as soon as a design or comprehension blocker is evident, and at latest after two unsuccessful correction attempts for the same issue. Do not use retries as a substitute for a missing user decision or external dependency. If meaningful progress remains blocked, report the exact blocker and unresolved findings; never label the task approved.

Finish when no mandatory in-scope findings remain, acceptance criteria are met, and required checks pass. A complete, passing first review is the final review; do not repeat it without changes or new evidence. After corrections, review the changed portions and affected interactions against the baseline, reconciling the cumulative result with prior findings and acceptance evidence. Broaden review when corrections invalidate earlier conclusions; do not automatically reread every unchanged portion. A blocked required check means verification is incomplete; optional checks not run must be identified separately. Zero findings means no remaining issues identified within the reviewed scope, not proof of defect-free software.

Apply project-specific documentation and delivery rules. Report the implemented outcome, checks performed, remaining limitations, and review status. Commit, push, or publish only within existing user authorization.

When evaluating efficiency, use completed tasks, correction rounds, and subsequent defects alongside actual usage-allowance changes if available. Token totals by model are diagnostic, not a verified conversion to weekly quota. Do not claim measured savings without comparable observations or run extra benchmark tasks unless requested.
