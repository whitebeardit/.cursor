# Instrumentação frontend (OpenTelemetry / Grafana)

Regras ao editar código de telemetria ou views que usam `usePageTelemetry`. Aplicar em projetos com stack Vite 3 + TypeScript e OpenTelemetry para Grafana/Tempo/Mimir.

## Nomes centralizados

- Usar **apenas** constantes de `telemetry/names.ts` (ou `telemetria/frontend/src/telemetry/names.ts` no projeto de referência) para nomes de métricas, spans e atributos.
- Nenhum literal de métrica/span/atributo em `instrumentation.ts` ou composables.

## SERVICE_NAME

- **SERVICE_NAME** é a fonte única para Resource, `getTracer` e `getMeter` (definido em `names.ts`).

## Atributos de span (semconv)

- Convenção: lowercase, dot notation, namespace (ex.: `dom.target.element`, `dom.target.xpath`, `journey.route`, `session.id`).
- Evitar atributos sem namespace (ex.: `target_element`).

## Métricas e cardinalidade

- Nomes de métricas no código **sem** sufixo `_total` (ex.: `frontend.errors`); o exporter adiciona.
- Counter de erros: apenas labels de baixa cardinalidade (`type`, `page`, `action`). **Não** usar `message` como label.

## Views

- Em cada view: chamar `usePageTelemetry()` no setup; usar `trackClick(buttonId, label?)` em botões.
- Preferir `data-telemetry-id` ou `data-testid` nos elementos para seletores estáveis no span de jornada.

## Documentação

- Comentário ou JSDoc indicando qual painel do Grafana usa cada métrica/span quando relevante.
- Referência: `telemetria/docs/frontend-opentelemetry.md` e README do frontend (job vs service_name).

---

## Exemplos

**DO — constantes:**

```typescript
import { METRIC_PAGE_VIEWS, SERVICE_NAME } from '@/telemetry/names'
telemetryCounters.pageViews.add(1, { page, path })
const tracer = trace.getTracer(SERVICE_NAME, version)
```

**DON'T — literais:**

```typescript
meter.createCounter('frontend.page_views', ...)
trace.getTracer('telemetria-demo-frontend', version)
```

**DO — recordError (labels de baixa cardinalidade):**

```typescript
recordError(err, { type: 'validation', page: route.name, action: 'submit' })
```

**DON'T — message como label:**

```typescript
recordError(err, { message: err.message })  // evita: explosão de séries
```
