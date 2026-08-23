---
name: design
description: Convert a completed IAE specification and the current codebase architecture into small, functional, testable implementation phases. Use after IAE Specify and before decomposition; do not implement code in this stage.
---

# IAE — Design

Create an incremental technical solution grounded in the accepted specification and the actual repository.

## Preconditions and workspace

- Resolve the project root as the nearest Git worktree root, otherwise the current working directory. Use `<project-root>/iae`, never the filesystem root `/iae`.
- Require `iae/specification.md` with status `pronta para design` and no material open business questions. If it is absent or incomplete, stop and direct the user to `$iae:specify`.
- Read repository guidance such as `AGENTS.md`, then inspect the relevant architecture, modules, patterns, tests, migrations, configuration, and dependency files.
- Do not modify product code in this stage.

## Design rules

- Trace every proposed behavior to acceptance criteria and `BTS-*` scenarios.
- Prefer the repository's established architecture and conventions unless the specification requires a deliberate change.
- Divide work into the smallest useful phases that each produce a functional, independently testable increment. Avoid phases that are only arbitrary file groups or leave the system intentionally broken.
- State dependencies and ordering explicitly.
- Define both automated verification and a concrete manual validation strategy for every phase.
- Do not choose unresolved business behavior. Ask the user when a design choice would change product semantics or scope.
- Do not decompose into file-level execution instructions yet.

## Required document

Create or update `iae/design.md` with:

- `# Design IAE — <título>`
- `Status: pronto para decomposição`
- `## Visão Geral da Solução`
- `## Contexto da Arquitetura Atual`
- `## Estratégia de Implementação`
- `## Decisões Técnicas`
- `## Dependências entre Fases`
- `## Lista de Fases`
- `## Estratégia de Testes`
- `## Riscos e Mitigações`

For each phase, include:

- stable ID and title, such as `Fase 1 — <título>`
- `Status: pendente`
- objective and functional deliverable
- boundaries: included and excluded work
- dependencies
- automated verification
- manual validation checklist
- mapped `AC-*` and `BTS-*` IDs

In `Estratégia de Testes`, provide a coverage matrix mapping every `BTS-*` to one or more phases. If a phase changes backend logic, note that decomposition must end that phase with a dedicated automated-test subphase.

## Completion

Report the artifact path, number of phases, uncovered scenarios if any, and recommend `$iae:decompose`. Do not claim readiness while any business scenario is unmapped.
