# Review boundaries

Review only the current task's cumulative changes since its recorded working-state baseline, including fixes made in subsequent rounds. A file modified during the task is not permission to audit every line in that file.

## Eligible mandatory findings

Require a concrete connection to at least one of:

- A defect introduced by the task's edits.
- A regression caused by those edits, including consequences in unchanged callers or consumers.
- An unmet acceptance criterion or explicit requirement of the task.
- A violation of an applicable project rule introduced by the task, such as missing translations, incompatible API contracts, or required checks omitted.

Read unchanged code as needed to establish behavior, compatibility, or a causal regression. The location of the visible failure may be outside the edited lines; explain how the task's change causes it. For missing behavior or missing files, cite the relevant acceptance criterion rather than requiring an existing changed line.

## Exclusions

Do not add mandatory findings for unrelated pre-existing defects, opportunistic refactors, speculative hardening, personal style preferences, or architectural improvements beyond the task. Existing defects that prevent the requested behavior or a required check are dependencies/blockers to explain, not permission for an unrestricted cleanup. Keep unrelated observations separate and do not feed them back into the correction loop.

If fixing a verified regression requires touching an additional file, include only the minimum necessary change, record its baseline before editing, and update the plan. Seek clarification only when the remedy changes the requested behavior or materially expands scope.

## Evidence and finding lifecycle

Use stable IDs such as R1 and R2. Each mandatory finding must contain:

- Severity and a precise location or unmet criterion.
- A concrete trigger and user-visible or technical impact.
- Evidence tying it to the task's changes or requirements.
- A bounded correction and a meaningful verification condition.

Do not manufacture findings to justify another round. Do not repeatedly reopen a stylistic decision already consistent with project conventions. Preserve a concise ledger of open and resolved findings through context compaction and worker replacement.

For example, if the task edits one form handler, a newly introduced validation bypass is in scope; an old naming inconsistency in another handler is not. A previously unchanged caller that breaks because the edited function changed its return type is also in scope.
