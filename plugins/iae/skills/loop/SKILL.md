---
name: loop
description: Execute the next pending IAE subphase, verify its success criteria, update its execution record, and stop at explicit manual validation gates between phases. Use only after IAE decomposition or when continuing an existing IAE implementation cycle.
---

# IAE — Loop

Advance an IAE plan predictably. One invocation may implement at most one subphase. Never silently continue through multiple subphases or across a manual validation gate.

## Workspace and prerequisites

- Resolve the project root as the nearest Git worktree root, otherwise the current working directory. Use `<project-root>/iae`, never the filesystem root `/iae`.
- Read repository guidance, `iae/specification.md`, `iae/design.md`, and the relevant files in `iae/phases/`.
- Require `design.md` status `decomposto — pronto para execução` or an active execution status. If artifacts are missing, stop and direct the user to `$iae:specify`, `$iae:design`, or `$iae:decompose` as appropriate.
- Inspect the working tree before editing and preserve unrelated user changes.

## Status model

Subphases use: `pendente`, `em execução`, `concluída`, `bloqueada`, or `substituída por adaptação`.

Parent phases in `design.md` use: `pendente`, `em execução`, `aguardando validação manual`, `validada`, or `requer adaptação`.

Treat the files as the source of truth. Repair only obvious status inconsistencies and record what changed.

## Manual gate first

Before implementing, find the earliest phase not marked `validada`.

- If it is `aguardando validação manual` and the user has not reported a result, do not change product code. Present the phase's manual checklist, expected outcomes, and the exact evidence or reply needed.
- If the user reports success, append a dated validation entry to that phase in `design.md`, mark it `validada`, then continue to the next pending phase in the same invocation if one exists.
- If the user reports failure, append the evidence, mark the phase `requer adaptação`, do not improvise a fix, and direct the user to `$iae:adapt` with the observed failure.
- If every phase is validated, report the IAE cycle complete and make no code changes.

## Execute one subphase

1. Select the first `pendente` subphase in the earliest eligible phase, honoring dependencies. Never skip a blocked dependency.
2. Read its full instructions and all relevant code. Confirm the named paths still match the repository.
3. Mark it `em execução` and the parent phase `em execução`.
4. Implement only its stated scope. Do not opportunistically implement later subphases.
5. Run the listed automated validation and any proportionate regression checks. Automated tests are the AI's responsibility.
6. If successful, mark it `concluída` and replace `Registro de Execução` with:
   - date/time
   - concise summary
   - files actually changed or created
   - commands/checks run and their results
   - any criterion that could not be validated
7. If the work or verification cannot complete, mark it `bloqueada`, record evidence and the precise blocker, and stop. Do not claim success.

If implementation reveals that the plan itself is wrong, out of date, or missing required scope, do not expand the subphase. Mark the relevant phase `requer adaptação`, explain the planning mismatch, and direct the user to `$iae:adapt`.

## Decide the next gate

- When more subphases remain in the current parent phase, report the completed subphase and name the next one. Stop; another `$iae:loop` invocation executes it.
- When the completed subphase is the last one in its parent phase, ensure all its automated checks have run. Mark the parent phase `aguardando validação manual`, then give the user a prominent manual-test handoff containing prerequisites, numbered actions, expected result for each action, and which `AC-*` / `BTS-*` items it validates. Stop before the next phase.
- If the design lacks a usable manual checklist, derive one from the phase deliverable and mapped scenarios, write it into `design.md`, and then present it.

Never treat automated tests as a substitute for the manual phase gate, and never require manual testing between ordinary subphases unless an explicit safety or environment constraint makes further work impossible.
