---
name: decompose
description: Break an approved IAE design into ordered, executable subphase files with explicit file targets, implementation steps, objective success criteria, risks, and test coverage. Use after IAE Design and before the implementation loop; do not implement code in this stage.
---

# IAE — Decompose

Turn each design phase into implementation instructions small and concrete enough to execute with limited context.

## Preconditions and workspace

- Resolve the project root as the nearest Git worktree root, otherwise the current working directory. Use `<project-root>/iae`, never the filesystem root `/iae`.
- Require `iae/specification.md` and `iae/design.md` with status `pronto para decomposição`. If a prerequisite is missing or incomplete, stop and direct the user to the corresponding IAE skill.
- Read repository guidance and inspect every relevant file before naming it in a subphase.
- Create `iae/phases/` when needed. Do not modify product code in this stage.

## Decomposition rules

- Preserve the phase order and scope from `design.md`.
- Each subphase must be independently understandable, have one coherent outcome, and leave the repository in a valid state whenever reasonably possible.
- List explicit repository-relative file paths. Do not invent paths; when a path genuinely depends on an earlier subphase, state the resolution rule and risk clearly.
- Include enough implementation detail to remove architectural guesswork without prescribing incidental syntax that the repository should determine.
- Map all work back to `AC-*` and `BTS-*` identifiers.
- For every phase that changes backend logic, make its final subphase exclusively about automated tests and reference the assigned `BTS-*` scenarios. Do not place later production-code work after that test subphase.
- Add other automated-test work where the design requires it, even when the phase is not backend-focused.
- Never implement code while decomposing.

## Subphase files

Create one file per subphase using `iae/phases/phase_<number>_<letter>.md`, for example `phase_1_A.md`. Continue letters deterministically (`A` through `Z`, then `AA`, `AB`, and so on) if necessary.

Each file must contain:

- `# Fase <number>.<letter> — <título>`
- `Status: pendente`
- `Fase pai: <number>`
- `Depende de: <IDs or nenhuma>`
- `## Objetivo`
- `## Contexto`
- `## Escopo` with included and excluded work
- `## Arquivos a Alterar`
- `## Arquivos a Criar`
- `## Passos de Implementação`
- `## Critérios de Sucesso`
- `## Validação Automatizada`
- `## Business Test Scenarios Cobertos`
- `## Riscos Conhecidos`
- `## Registro de Execução`, initially `Ainda não executada.`

Use objective criteria such as build success, named test commands, response behavior, persisted state, or absence of a specific regression. Do not use vague criteria such as “funciona corretamente” without an observable definition.

After writing the files, update each phase in `iae/design.md` with an ordered `Subfases` list and set the document status to `decomposto — pronto para execução`. Preserve the original design content.

## Completion

Report the number of phases and subphases, identify the first pending file, and recommend `$iae:loop`. Flag any unresolved path or dependency instead of hiding it.
