---
name: qa-code-review
description: Perform QA Playwright code review by applying the perspectives of maintain, add-fixture, and add-flow agents; produce a consolidated improvement summary focused on maintenance, quality, and standardization. Use when the user asks for a code review, review of specs/test-data, or improvement summary.
---

# QA Code Review

This skill defines how to review QA Playwright code (specs, test-data, fixtures) by **considering the perspective of each QA subagent**, then **consolidating** their concerns into one report focused on **manutenção**, **qualidade** e **padronização**.

## When to Use

- User asks for a code review of specs, test-data, or flow.
- User asks "o que acha do código?" or "revise esse teste".
- User wants a consolidated improvement list from the point of view of the project's QA standards.

## Process

1. **Identify scope:** What is under review? (single spec, whole flow, test-data, fixtures.)
2. **Apply each relevant QA perspective** by reading and applying the criteria from:
   - **Conventions (maintain):** [skill-qa-playwright-maintain-conventions](../skill-qa-playwright-maintain-conventions/SKILL.md) — imports, test-data usage, structure, locators.
   - **Fixtures (add-fixture):** [skill-qa-playwright-add-fixture](../skill-qa-playwright-add-fixture/SKILL.md) — whether fixtures would reduce duplication or improve structure.
   - **Flow (add-flow):** [skill-qa-playwright-add-new-flow](../skill-qa-playwright-add-new-flow/SKILL.md) — when reviewing a flow: folders, inputs, baseURL, naming.
3. **Optionally** consider:
   - **Test-data:** [skill-qa-playwright-add-test-data](../skill-qa-playwright-add-test-data/SKILL.md) — if the review involves inputs.json, builder.ts.
   - **Frontend-friendly:** [skill-frontend-qa-friendly](../skill-frontend-qa-friendly/SKILL.md) — locators, accessibility, stability.
4. **Produce a single report** with the structure below.

## Output Structure

Produce the review in this order:

### 1. Manutenção (conventions)

- Checklist from maintain skill: imports from fixtures, test-data vs hardcode, directory structure, baseURL.
- Explicit: ✅ atende / ⚠️ atenção / ❌ não atende, with short reason.

### 2. Qualidade

- Locators: resilient (getByRole, getByLabel, getByText) vs brittle (XPath por posição, classes).
- Assertions: web-first, timeouts adequados, critérios de sucesso claros.
- Legibilidade: Arrange–Act–Assert, comentários úteis, nome do teste descritivo.

### 3. Padronização

- Estrutura: tests/ e test-data/ alinhados ao fluxo; nomenclatura de pastas e arquivos.
- Dados: inputs.json/builder.ts conforme docs; uso de data-factory quando fizer sentido.
- Fixtures: se faz sentido sugerir fixture (ex.: página logada, inputs por fluxo) para evitar duplicação.

### 4. Resumo de melhorias

- Lista objetiva e acionável (ex.: "Usar inputs.produto.categoria no spec ou remover do JSON").
- Priorize: quebra de convenção > qualidade > padronização > sugestões opcionais.

## Delegation Note

You cannot invoke other agents programmatically. Instead, **apply their criteria yourself**: read the skills listed above (maintain, add-fixture, add-flow, and optionally add-test-data, frontend-qa-friendly) and evaluate the code under review as each of those agents would. Then merge the findings into the single report (maintenance, quality, standardization, summary).

## References

- [AGENTS.md](../../../AGENTS.md) — list of QA agents and skills
- [docs/02-estrutura-de-diretórios.md](../../../docs/02-estrutura-de-diretórios.md) — directory roles
- [docs/03-fixtures.md](../../../docs/03-fixtures.md) — fixtures
- [docs/07-como-adicionar-novo-fluxo.md](../../../docs/07-como-adicionar-novo-fluxo.md) — flow structure
