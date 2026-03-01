---
name: skill-frontend-instrumentation-otel
description: Instrumenta frontends Vite 3 + TypeScript com OpenTelemetry para Grafana, Tempo e Mimir (traces, Web Vitals, page views, cliques, erros). Use ao adicionar telemetria em novas páginas, configurar RUM ou revisar código de instrumentação existente.
---

# Instrumentação frontend OpenTelemetry (Grafana / Tempo / Mimir)

## Quando usar

- Instrumentar nova página ou feature no frontend (Vite 3 + TypeScript / Vue).
- Revisar PR ou trecho de código de instrumentação.
- Dúvidas sobre nomes de métricas/spans ou mapeamento código → Grafana.

---

## Fluxo: instrumentar nova página

Checklist:

```
- [ ] Entrypoint importa ./instrumentation antes do framework e do router
- [ ] View importa usePageTelemetry e chama usePageTelemetry() no setup
- [ ] Botões/ações principais chamam trackClick(buttonId, label?) com button_id estável
- [ ] Elementos críticos têm data-telemetry-id ou data-testid para seletores estáveis
- [ ] Novas métricas/atributos definidos em telemetry/names.ts; uso via constantes
```

**Passos:**

1. **Entrypoint**  
   Garantir que o primeiro import seja da instrumentação (ex.: `import './instrumentation'` em `main.ts`) antes de `createApp`, router e montagem.

2. **View**  
   No componente de página: `import { usePageTelemetry } from '@/composables/usePageTelemetry'` e no setup chamar `usePageTelemetry()`. O composable já registra page view no `onMounted`.

3. **Cliques**  
   Em botões/ações principais: `trackClick(buttonId, label?)`. Usar `button_id` estável (ex.: `'save_settings'`, `'cta_home'`). Preferir `data-telemetry-id` ou `data-testid` no elemento para o span de jornada (`user.journey.click`) ter seletor estável.

4. **Novas métricas/atributos**  
   Não usar literais. Adicionar constantes em `telemetry/names.ts` (ex.: nova métrica, novo atributo de span) e importar onde for usar.

---

## Fluxo: revisar código de instrumentação

Checklist de revisão:

```
- [ ] Nomes centralizados em telemetry/names.ts; sem literais em instrumentation ou composables
- [ ] SERVICE_NAME definido em names.ts e usado em Resource, getTracer, getMeter
- [ ] Atributos de span com namespace semconv (dom.*, journey.*, session.id)
- [ ] Métrica de erros: nome sem _total; labels apenas type/page/action (sem message)
- [ ] Views usam usePageTelemetry() e trackClick com button_id estável
- [ ] Comentários ou docs indicam mapeamento para o dashboard (opcional, recomendado)
```

Para cada item em falta: reportar e sugerir alteração concreta (trecho de código ou path do arquivo).

---

## Referências

- Detalhes do plano, mapeamento código → Grafana e labels (`job` vs `service_name`): [reference.md](reference.md).
- Código de referência no projeto: `telemetria/frontend/src/telemetry/names.ts`, `telemetria/frontend/src/instrumentation.ts`, `telemetria/frontend/src/composables/usePageTelemetry.ts`.
- Doc de arquitetura: `telemetria/docs/frontend-opentelemetry.md`.
