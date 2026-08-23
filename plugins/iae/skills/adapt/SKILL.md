---
name: adapt
description: Diagnose a failed IAE phase validation, distinguish implementation errors from planning errors, and evolve code or future plan artifacts without erasing history. Use after a manual or automated validation failure in an active IAE cycle.
---

# IAE — Adapt

Turn validation failure into a traceable correction while preserving the original plan and decisions.

## Preconditions and workspace

- Resolve the project root as the nearest Git worktree root, otherwise the current working directory. Use `<project-root>/iae`, never the filesystem root `/iae`.
- Read repository guidance and all IAE artifacts. Require concrete failure evidence: reproduction steps, actual versus expected behavior, logs, screenshots, failing checks, or the user's manual-test report.
- If the failure is too vague to classify after safe inspection, ask for the smallest missing evidence and stop.
- Preserve unrelated user changes and never delete prior IAE decisions, phase files, or execution records.

## Diagnose first

Reproduce or inspect the failure when possible, then classify it:

1. **Implementation error:** the accepted specification and design are sound, but the code does not follow them.
2. **Planning error:** the implementation follows the plan, but the requirement, architecture, decomposition, dependency, or validation strategy is incomplete or wrong.

Do not classify solely from intuition. Cite the artifact and code evidence that supports the diagnosis. Never decide missing business behavior on the user's behalf.

## Implementation error

- Correct only the faulty implementation within the affected subphase's intended scope.
- Update that subphase's `Registro de Execução` with an `Adaptação` entry describing cause, changes, and verification.
- Re-run its success criteria and relevant regression checks.
- Mark the subphase `concluída` only when those checks pass; otherwise leave it `bloqueada` with evidence.
- When the parent deliverable is again ready, mark the phase `aguardando validação manual` and present the full manual checklist again.

## Planning error

- Append a numbered `Adaptação` section to the affected `iae/specification.md` and/or `iae/design.md`; do not rewrite or remove the historical decision. Include evidence, impact, new decision, and affected `AC-*` / `BTS-*` items.
- If a business decision is needed, ask the user before finalizing the adaptation.
- Mark superseded future instructions `substituída por adaptação` while keeping their contents.
- Create new adjustment subphases after the existing ones, continuing the letter sequence. Mark them clearly as `Tipo: adaptação`, state what they supersede, and use the same complete subphase structure as `$iae:decompose`.
- Update every future phase affected by the change and the scenario coverage matrix. Preserve unaffected work and completed execution evidence.
- Do not implement newly planned adjustment subphases during the same planning-error invocation. Return control to `$iae:loop`.

## Completion

Report the classification, evidence, artifacts or code changed, verification performed, and the next command:

- `$iae:loop` when a planning correction has produced pending adjustment subphases;
- manual validation when an implementation correction restored the phase deliverable;
- `$iae:specify` only when the failure exposes a new unresolved business requirement that cannot yet be adapted.
