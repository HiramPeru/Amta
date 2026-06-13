---
id: a3f9c2d1-b4e2-4f1a-9c3d-e5f6a7b8c9d0
type: Decision
status: active
priority: important
created: 2025-06-13T10:00:00Z
updated: 2025-06-13T10:00:00Z
superseded_by:          # completar solo si status = deprecated
edges_out:
  - relation: "depende_de"
    target: b7e1a4f2-c5d3-4a2b-8e1f-a9b0c1d2e3f4   # uuid del nodo relacionado
  - relation: "contiene"
    target:                                           # uuid opcional
---

# Elegimos PostgreSQL como base de datos principal

Evaluamos tres opciones para la capa de persistencia del proyecto.
Seleccionamos PostgreSQL por su soporte nativo de JSONB, transacciones ACID y
madurez del ecosistema en el stack actual.

## Criterios de decisión

- Consistencia transaccional requerida por el modelo de negocio.
- El equipo tiene experiencia previa con PostgreSQL.
- Menor overhead operativo frente a soluciones distribuidas.

## Referencias

- [Ver esquema inicial](../../src/db/schema.sql)
- [Ver análisis comparativo](../nodes/EJEMPLO_descarte.md)
