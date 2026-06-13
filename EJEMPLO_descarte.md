---
id: c9d2b5e3-f6a7-4b8c-9d0e-1f2a3b4c5d6e
type: Descarte
status: archived
priority: normal
created: 2025-06-13T10:00:00Z
updated: 2025-06-13T10:00:00Z
reason: "Overhead operativo alto para el tamaño del equipo. Escalabilidad horizontal no requerida en esta etapa."
edges_out:
  - relation: "alternativa_de"
    target: a3f9c2d1-b4e2-4f1a-9c3d-e5f6a7b8c9d0   # uuid de la decisión tomada
---

# Opción descartada: MongoDB

Evaluada como alternativa a PostgreSQL para la capa de persistencia.

## Por qué se descartó

- El modelo de datos del proyecto es altamente relacional — MongoDB añadiría
  complejidad de joins en capa de aplicación.
- Consistencia eventual no es aceptable para el flujo de pagos.
- El equipo carece de experiencia operativa con MongoDB en producción.

## Condición de reapertura

Reevaluar si el volumen de documentos no estructurados supera el 40% del total
de datos almacenados, o si se requiere escala horizontal en una fase futura.
