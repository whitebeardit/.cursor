# Referência: instrumentação frontend → Grafana

## Recomendações do plano (resumo acionável)

| Objetivo | Ação |
|----------|------|
| Nomes fáceis de achar | Constantes em `telemetry/names.ts`; usar em instrumentation e composables; referenciar no README/docs. |
| Consistência semconv | Atributos com namespace (ex.: `dom.target.element`, `dom.target.xpath`); evitar `target_element` sem namespace. |
| Evitar _total duplicado | Métrica de erros no código: `frontend.errors` (unit `1`); no Mimir o exporter gera `frontend_errors_total`. |
| Cardinalidade controlada | Counter de erros: labels apenas `type`, `page`, `action`; não usar `message` como label. |
| Single source para serviço | `SERVICE_NAME` em `names.ts`; usar em Resource, getTracer e getMeter. |
| Debug e monitoramento | Documentar no README/docs: métricas/spans, nome no código vs nome no Grafana (Mimir), label `job` vs `service_name`. Comentários em instrumentation indicando painel do dashboard. |

---

## Mapeamento: código → Mimir → Dashboard

| Constante / nome no código | Nome no Mimir (após export) | Painel / uso no Grafana |
|----------------------------|-----------------------------|--------------------------|
| `METRIC_WEB_VITALS` / `frontend.web_vitals` | `frontend_web_vitals_milliseconds_bucket` (histogram) | Core Web Vitals, LCP p95, TTFB p95 |
| `METRIC_PAGE_VIEWS` / `frontend.page_views` | `frontend_page_views_total` | Page views por página e rota |
| `METRIC_BUTTON_CLICKS` / `frontend.button_clicks` | `frontend_button_clicks_total` | Cliques por botão e página |
| `METRIC_ERRORS` / `frontend.errors` | `frontend_errors_total` | Erros por tipo/página, Total erros |
| `SPAN_USER_JOURNEY_CLICK` / `user.journey.click` | spans no Tempo | Traces por sessão; atributos: session.id, journey.route, dom.position.*, dom.selector, dom.target.* |
| (spanmetrics do Collector) | `traces_span_metrics_*` (RED) | RED por rota (Request, Error, Duration) |

---

## Labels no Grafana

- **Métricas OTLP do frontend** (page_views, button_clicks, errors, web_vitals): usam label **`job`** (valor = `service.name` do resource).
- **Métricas RED (spanmetrics)** geradas pelo Collector: usam label **`service_name`**.

No Explore (Mimir): filtrar por `job=~"$service_name"` para métricas do frontend; para RED, usar `service_name`.

---

## Arquivos de referência no repositório

| Path | Conteúdo |
|------|----------|
| `telemetria/frontend/src/telemetry/names.ts` | Constantes: SERVICE_NAME, METRIC_*, SPAN_*, ATTR_* |
| `telemetria/frontend/src/instrumentation.ts` | Traces, métricas, Web Vitals, jornada (cliques), recordError |
| `telemetria/frontend/src/composables/usePageTelemetry.ts` | usePageTelemetry(), trackClick(); uso nas views |
| `telemetria/docs/frontend-opentelemetry.md` | Arquitetura, env, feature flags, runbook, labels |
