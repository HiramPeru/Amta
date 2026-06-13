---
id: d4e5f6a7-b8c9-4d0e-9f1a-2b3c4d5e6f7a
type: Hipotesis
status: active
priority: important
created: 2026-06-13T10:00:00Z
updated: 2026-06-13T10:00:00Z
evidence:
  - type: metric
    reference: "baseline_operativo"
    description: "Métrica inicial requerida antes de validar o refutar la hipótesis"
edges_out:
  - relation: "relacionada_con"
    target: a3f9c2d1-b4e2-4f1a-9c3d-e5f6a7b8c9d0
---

# Hipótesis: La automatización reduce retrabajo operativo

Creemos que incorporar una fuente única de verdad reducirá preguntas repetidas y retrabajo entre agentes.

## Criterio de validación

Validar si un agente nuevo puede reconstruir el estado del proyecto leyendo `STATE.md` y los nodos críticos activos sin revisar conversaciones antiguas.

## Evidencia requerida

- Registro de una sesión nueva con agente externo.
- Comparación entre preguntas repetidas antes y después de AMTA.
- Resultado documentado en un nodo `Evento` o `Artefacto`.
