# AMTA Starter Kit

Copia esta carpeta a la raíz de cualquier proyecto. El grafo arranca en 5 minutos.

---

## Estructura

```
context/
  SYSTEM.md                  ← Contrato de comportamiento (no editar)
  STATE.md                   ← Nodo raíz: edita esto primero
  CHANGELOG.md               ← Registro de cambios: agrega una entrada por cada acción
  graph/
    nodes/
      EJEMPLO_nodo.md        ← Eliminar después de leer
      EJEMPLO_descarte.md    ← Eliminar después de leer
      EJEMPLO_hipotesis.md   ← Eliminar después de leer
```

---

## Inicio rápido

### Paso 1 — Edita `STATE.md`

Rellena:
- El resumen ejecutivo del proyecto (2 frases).
- La fase actual.
- Los bloqueadores activos si los hay.

### Paso 2 — Crea tu primer nodo real

Copia `EJEMPLO_nodo.md`, renómbralo con un UUID nuevo y describe la primera decisión, entidad, artefacto, evento, descarte o hipótesis del proyecto.

Genera un UUID en bash:
```bash
uuidgen | tr '[:upper:]' '[:lower:]'
```

O en Python:
```python
import uuid; print(uuid.uuid4())
```

### Paso 3 — Registra en `CHANGELOG.md`

Agrega una línea con el tipo `nodo_nuevo` y el UUID creado.

### Paso 4 — Sigue el protocolo

A partir de aquí, todo agente (humano o IA) que actúe en el proyecto:

1. Lee `STATE.md`.
2. Carga el nodo relevante.
3. Actúa.
4. Escribe el nodo actualizado.
5. Registra en `CHANGELOG.md`.

Para consultas globales, carga primero nodos `active` con `priority: critical`.

---

## Tipos de nodo disponibles

| Tipo | Cuándo usarlo |
|---|---|
| `Decision` | Elección tomada entre alternativas |
| `Evento` | Hecho ocurrido (reunión, incidente, hito) |
| `Entidad` | Concepto del dominio (usuario, sistema, servicio) |
| `Artefacto` | Documento, código, diagrama o entregable |
| `Descarte` | Opción evaluada y descartada — incluye `reason` |
| `Hipotesis` | Creencia verificable bajo evaluación — incluye `evidence` |

---

## Convención de nombres de archivo

```
{uuid}.md           ← producción
EJEMPLO_{nombre}.md ← solo para referencia, eliminar antes de usar
```

---

## Preguntas frecuentes

**¿Puedo usar AMTA sin `amta_write`?**  
Sí. Edita los archivos directamente. `amta_write` es una conveniencia, no un requisito.

**¿Dónde vive `SYSTEM.md`?**  
En `context/`. Es el contrato del sistema. Léelo. No lo edites salvo para migrar a una versión nueva de AMTA.

**¿Qué hago con proyectos existentes?**  
Crea `context/`, copia estos archivos, agrega manualmente los 5–10 nodos más importantes del dominio y sigue el protocolo desde ese momento.
