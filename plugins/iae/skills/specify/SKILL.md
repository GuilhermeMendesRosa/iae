---
name: specify
description: Clarify a software change and write an IAE business specification before technical design or implementation. Use for the first IAE stage when requirements, rules, flows, edge cases, acceptance criteria, and business test scenarios must be established without making business decisions for the user.
---

# IAE — Specify

Transform the user's inputs into a decision-complete business specification. Do not design the solution or change product code in this stage.

## Workspace contract

- Resolve the project root as the nearest Git worktree root; when none exists, use the current working directory.
- Store IAE artifacts in `<project-root>/iae`. In this methodology, `/iae` means that project-relative directory, never the filesystem root `/iae`.
- Treat the artifacts as temporary working documents, but never delete them unless the user asks.
- Preserve useful existing IAE content and unrelated user changes.

## Workflow

1. Read every input the user supplied or referenced: task text, tickets, diagrams, images, documentation, business rules, discussions, and relevant project context.
2. Separate known facts, business decisions, technical constraints, and open questions.
3. Identify material ambiguities and edge cases. Never silently choose business behavior, policy, permissions, pricing, user-visible semantics, or acceptance criteria.
4. If a material business question remains, ask a compact, prioritized batch of questions and stop before calling the specification complete. Avoid asking about facts that can be discovered from the supplied artifacts or codebase.
5. When the answers are sufficient, create or update `iae/specification.md`. Do not modify product code, tests, migrations, or runtime configuration.

## Required document

Write `iae/specification.md` with:

- `# Especificação IAE — <título>`
- `Status: pronta para design`
- `## Objetivo`
- `## Escopo` with explicit in-scope and out-of-scope items
- `## Regras de Negócio`
- `## Fluxos Principais`
- `## Fluxos Alternativos`
- `## Edge Cases`
- `## Critérios de Aceite`
- `## Business Test Scenarios`
- `## Dúvidas Resolvidas`
- `## Dúvidas em Aberto`, which must say `Nenhuma` before the status is ready

Give each acceptance criterion a stable ID such as `AC-01` and each business scenario a stable ID such as `BTS-01`. Express scenarios in Gherkin-style `Dado / Quando / Então` language and link them to relevant acceptance criteria.

Record resolved questions as the question, the user's decision, and any consequence. Label technical assumptions explicitly and keep them reversible; do not disguise them as business decisions.

## Completion

Summarize what was captured, state the artifact path, and recommend `$iae:design`. If questions remain, say precisely what blocks completion and do not recommend implementation.
