# CHANGELOG

> Registra aquí todo cambio significativo al grafo o al proyecto.  
> Formato: `[graph_version] YYYY-MM-DD — Tipo — Descripción`  
> Tipos: `nodo_nuevo` | `nodo_actualizado` | `nodo_deprecado` | `conflicto_resuelto` | `cambio_estructural` | `spec_actualizada`

---

## Cómo registrar una entrada

```
## [0.1.1] 2025-06-13

### nodo_nuevo
- uuid: a3f9c2d1 | tipo: Decision | "Elegimos PostgreSQL sobre MongoDB"

### nodo_actualizado
- uuid: b7e1a4f2 | campo: status | active → deprecated | superseded_by: c9d2b5e3
```

---

## Registro

### [1.1.1] 2026-06-13

### spec_actualizada
- F-001: Se formalizan los estados específicos de Hipotesis (validated, refuted, superseded) en su ciclo de vida, manteniendo el enumerado genérico de status inalterado.
- F-002: Se añade el campo `superseded_by` a la plantilla de nodo tipo `Hipotesis`.
- F-003: Se añade el campo transversal `priority` a `EJEMPLO_descarte.md`.
- F-004: Se define oficialmente en Semver la creación de nuevos nodos compatibles como incremento de tipo `Patch`.
- F-006: Se documenta explícitamente el Protocolo de Drift en Nivel 3.2 para resolver y modelar discrepancias en el grafo.

### [1.1.0] 2026-06-13

### cambio_estructural
- Se incorpora `Hipotesis` como tipo de nodo epistémico.
- Se incorpora `priority: critical | important | normal` como campo transversal.
- Se formaliza la taxonomía ontológica FACTUAL / EPISTÉMICO.
- Se actualiza la tabla de enrutamiento para cargar nodos críticos activos.

### [0.1.0] YYYY-MM-DD

### cambio_estructural
- Proyecto iniciado. Grafo creado. `STATE.md` y `SYSTEM.md` cargados.
