# AMTA — Fuente Única de Verdad para Agentes

> **Versión:** 1.1.1  
> **Última actualización:** 2026-06-13  
> **Naturaleza:** Contrato de comportamiento. Agnóstico a dominio, lenguaje y tipo de proyecto.

---

## Principio Fundacional

El grafo es la realidad del proyecto.  
Léelo antes de actuar. Escríbelo después de actuar.  
La fuente de verdad vive en los archivos — siempre.

---

## El problema que AMTA resuelve

Sin una fuente común:

- Cada agente construye su propia versión de la realidad.
- Las decisiones se toman desde conversaciones antiguas o memoria volátil.
- El contexto se fragmenta entre sesiones, personas y herramientas.
- El resultado: contradicciones, trabajo repetido, pérdida de trazabilidad.

Con AMTA:

- Un solo estado compartido, accesible a cualquier agente.
- Toda acción parte del grafo y regresa al grafo.
- La trazabilidad es completa desde el primer nodo.

---

## Sistema Lingüístico Imperativo-Asociativo (SLIA)

AMTA opera bajo SLIA: todas las instrucciones del sistema se expresan en **imperativo directo y positivo**, y todas las relaciones entre entidades se expresan como **asociaciones**, nunca como jerarquías abstractas o negaciones.

**¿Por qué?**

El subconsciente — humano y computacional — procesa instrucciones de forma literal y directa. Una negación exige una traducción previa antes de ejecutar. Una instrucción positiva activa la acción inmediatamente.

| Forma evitada | Forma SLIA |
|---|---|
| "No uses tu memoria privada" | "Lee el grafo. Actúa desde el grafo." |
| "No escribas sin leer primero" | "Lee. Luego escribe." |
| "Si hay error, detente" | "Detecta drift → crea nodo `disputed` → continúa." |
| "No dupliques entidades" | "Busca el nodo existente. Actualízalo." |

Todo protocolo, regla y convención en este documento sigue SLIA.

---

## Arquitectura en Tres Niveles

### Nivel 1 — Principios Inmutables

1. **El grafo es la realidad** — Léelo primero. La memoria del agente es referencia temporal.
2. **Audita antes de cambiar** — Carga el estado actual antes de proponer o ejecutar.
3. **Detecta drift** — Verifica que el grafo coincida con el código, BD o runtime real. Al encontrar discrepancia: crea nodo `disputed`.
4. **Una entidad, un nodo** — Busca el nodo existente. Actualízalo. Crea uno nuevo solo cuando la entidad sea genuinamente nueva.
5. **Todo cambio deja rastro** — Cada modificación genera o actualiza un nodo con timestamp.
6. **Carga lo relevante** — Los agentes cargan subgrafos específicos. El grafo completo es el sistema, no la sesión.

---

### Nivel 1.1 — Taxonomía Ontológica

AMTA almacena dos categorías de conocimiento.

#### FACTUAL

Describe la realidad observable del proyecto.

| Tipo | Pregunta que responde |
|---|---|
| `Entidad` | ¿Qué existe? |
| `Evento` | ¿Qué ocurrió? |
| `Artefacto` | ¿Qué se construyó? |

#### EPISTÉMICO

Describe el conocimiento y razonamiento del equipo.

| Tipo | Pregunta que responde |
|---|---|
| `Decision` | ¿Qué elegimos? |
| `Descarte` | ¿Qué rechazamos? |
| `Hipotesis` | ¿Qué creemos? |

AMTA almacena hechos y conocimiento del equipo.

Las tareas viven en sistemas transaccionales.

Las opiniones se convierten en nodos únicamente cuando adquieren evidencia (`Hipotesis`) o compromiso explícito (`Decision`).

---

### Nivel 2 — Estructura de Archivos (Portable)

```
/project-root/
  context/
    SYSTEM.md          ← Este documento (contrato de comportamiento)
    STATE.md           ← Nodo raíz del grafo: estado actual + graph_version
    CHANGELOG.md       ← Histórico de cambios significativos
    graph/
      nodes/           ← Un archivo .md por nodo
        {uuid}.md
      edges/           ← Opcional: relaciones separadas si no van en frontmatter
    Daily/             ← Logs diarios y resúmenes automáticos
    Intelligence/      ← Análisis derivados: tendencias, riesgos, patrones
  [código, BD y resto del proyecto sin modificar]
```

**Archivos de control (fuera de `graph/nodes/`):**

`SYSTEM.md` y `STATE.md` son archivos de control del sistema, distintos de los nodos del grafo. Viven directamente en `context/` porque son precondición de cualquier operación.

`STATE.md` es el **nodo raíz implícito del grafo**. Todo agente lo carga antes que cualquier otro nodo. Contiene el resumen del estado actual del proyecto y el `graph_version` vigente. Si en el futuro el grafo requiere migración a un backend estructurado, `STATE.md` se convierte en un nodo de tipo `Meta` dentro de `graph/nodes/` sin pérdida de información.

---

### Nivel 2.1 — Formato de un Nodo

**Archivo:** `context/graph/nodes/{uuid}.md`

```yaml
---
id: uuid_generado
type: Decision | Evento | Entidad | Artefacto | Descarte | Hipotesis
priority: critical | important | normal
status: active | deprecated | archived | disputed
created: 2025-06-13T10:00:00Z
updated: 2025-06-13T15:30:00Z
superseded_by: uuid_del_nodo_sucesor   # solo si status = deprecated
edges_out:
  - relation: "depende_de"
    target: uuid_otro
  - relation: "contiene"
    target: uuid_tercero
  - relation: "alternativa_de"     # solo en nodos tipo Descarte
    target: uuid_decision_tomada
---

# Título del nodo

Descripción concisa del hecho, decisión, entidad, artefacto, descarte o hipótesis.
```

---

### Nivel 2.2 — Ciclo de Vida del Nodo

| Status | Significado | Acción requerida |
|---|---|---|
| `active` | Verdad vigente | Ninguna |
| `deprecated` | Reemplazado por otro nodo | Completar campo `superseded_by` |
| `archived` | Ya no relevante; conservado para trazabilidad | Ninguna |
| `disputed` | Dos versiones contradictorias sin resolución | Resolución humana explícita |

**Para deprecar un nodo:**

1. Cambia su `status` a `deprecated`.
2. Agrega `superseded_by: {uuid_del_nuevo_nodo}`.
3. Crea el nodo sucesor con `status: active`.
4. Registra el cambio en `CHANGELOG.md`.

---

### Nivel 2.3 — Nodo Tipo Descarte

El nodo `Descarte` captura lo que se evaluó y se descartó — el conocimiento más valioso a largo plazo.

```yaml
---
id: uuid_generado
type: Descarte
status: archived
created: 2025-06-13T10:00:00Z
edges_out:
  - relation: "alternativa_de"
    target: uuid_de_la_decision_que_se_tomó
reason: "Descripción clara de por qué se descartó esta opción"
---

# Opción descartada: [nombre]

Contexto de evaluación y criterios que llevaron al descarte.
```

Crea un nodo `Descarte` cada vez que el equipo evalúe una opción y elija otra. Esto previene que futuros agentes repitan evaluaciones ya realizadas.


---

### Nivel 2.4 — Nodo Tipo Hipotesis

El nodo `Hipotesis` captura una afirmación prospectiva bajo evaluación.

Una hipótesis describe una creencia verificable mediante evidencia. Una hipótesis sin evidencia es una opinión y permanece fuera del grafo.

```yaml
---
id: uuid_generado
type: Hipotesis
status: active | validated | refuted | superseded
priority: critical | important | normal
created: 2026-06-13T10:00:00Z
updated: 2026-06-13T10:00:00Z
superseded_by: uuid_del_nodo_sucesor   # solo si status = superseded
evidence:
  - type: metric | event | artifact | human_validation
    reference: uuid_o_referencia_externa
    description: "Métrica, hecho o artefacto que confirma o refuta la hipótesis"
edges_out:
  - relation: "relacionada_con"
    target: uuid_relacionado
---

# Hipótesis: [nombre]

Afirmación bajo evaluación y criterio observable para validarla o refutarla.
```

**Ciclo de vida de `Hipotesis`:**

| Status | Significado | Acción requerida |
|---|---|---|
| `active` | Bajo evaluación | Mantener evidencia actualizada |
| `validated` | Confirmada por evidencia | Registrar evidencia suficiente |
| `refuted` | Refutada por evidencia | Registrar evidencia que la refuta |
| `superseded` | Reemplazada por hipótesis más precisa | Completar `superseded_by` |

Una hipótesis refutada permanece como `Hipotesis` con `status: refuted`.

Una hipótesis refutada no se convierte en `Descarte`.

La refutación proviene de evidencia. El descarte proviene de evaluación deliberada entre alternativas.

---

### Nivel 2.5 — Versionado del Grafo

El archivo `STATE.md` incluye siempre:

```yaml
graph_version: 1.1.0
last_updated: 2026-06-13T15:00:00Z
conflict_window_seconds: 60   # ajustar según latencia del sistema
```

**Semver ligero:**

| Incremento | Cuándo |
|---|---|
| **Major** (1.x.x → 2.0.0) | Cambio estructural del grafo (nuevos tipos, nuevo esquema) |
| **Minor** (1.0.x → 1.1.0) | Nuevo tipo, campo o regla compatible incorporada |
| **Patch** (1.0.0 → 1.0.1) | Actualización de nodo existente o creación de un nuevo nodo compatible |

Actualiza `graph_version` en `STATE.md` después de cada escritura al grafo.

---

### Nivel 3 — Protocolo de Operación para Agentes

#### Antes de cualquier acción:

1. Lee `context/SYSTEM.md` y `context/STATE.md`.
2. Identifica la entidad relevante para la consulta.
3. Carga su nodo desde `graph/nodes/{uuid}.md`.
4. Expande solo las aristas necesarias (tabla de enrutamiento abajo).
5. Verifica drift: ¿El nodo coincide con el código, BD o runtime real? Al encontrar discrepancia → crea nodo `disputed` → continúa.

#### Tabla de enrutamiento (carga selectiva):

| Consulta | Acción del agente |
|---|---|
| "¿Qué es [entidad]?" | Carga el nodo de esa entidad. Aristas: ninguna. |
| "¿Qué relación tiene [A] con [B]?" | Carga ambos nodos. Expande aristas hasta profundidad 2. |
| "¿Qué cambió recientemente?" | Carga nodos en `Daily/` de los últimos 2 días. |
| "Actualiza [entidad] con [nuevo estado]" | Ejecuta `amta_write`. Lee el nodo actual primero. |
| "¿Qué es lo más importante ahora?" | Carga nodos `active` con `priority: critical`. |
| Consulta sin entidades específicas | Carga solo `SYSTEM.md`. |

#### Después de cualquier cambio:

1. Crea o actualiza el nodo correspondiente en `graph/nodes/`.
2. Agrega entrada en `CHANGELOG.md`.
3. Incrementa `graph_version` en `STATE.md`.
4. Si el cambio afecta el estado general del proyecto → actualiza el resumen en `STATE.md`.

---

### Nivel 3.1 — Protocolo de Conflicto

**Regla:** Cuando dos escrituras ocurran sobre el mismo nodo dentro del intervalo de conflicto configurado (por defecto: 60 segundos; ajustable según la latencia típica del sistema en `STATE.md` bajo el campo `conflict_window_seconds`):

1. La escritura con timestamp más reciente prevalece en el nodo original.
2. El sistema crea automáticamente un nodo `disputed` que referencia ambas versiones.
3. El nodo `disputed` permanece con `status: disputed` hasta resolución humana explícita.
4. La resolución humana marca el nodo `disputed` como `archived` y actualiza el nodo original con la versión correcta.

**Formato del nodo `disputed`:**

```yaml
---
id: uuid_nuevo
type: Evento
status: disputed
created: [timestamp_del_conflicto]
edges_out:
  - relation: "versión_a"
    target: uuid_escritura_1
  - relation: "versión_b"
    target: uuid_escritura_2
---

# Conflicto detectado en: [uuid_nodo_original]

Descripción de las dos versiones en conflicto y contexto del momento.
```

---

### Nivel 3.2 — Protocolo de Drift

**Regla:** Cuando un agente detecte drift (discrepancia entre el grafo y el código, BD o runtime real):

1. El agente actualiza inmediatamente el nodo original en `graph/nodes/{uuid}.md` para que refleje la realidad (el grafo sigue a la realidad).
2. El agente crea automáticamente un nodo `disputed` (tipo `Evento`, `status: disputed`) para registrar y rastrear el drift.
3. El nodo `disputed` referencia el nodo modificado y describe la discrepancia.

**Formato del nodo `disputed` por drift:**

```yaml
---
id: uuid_nuevo
type: Evento
status: disputed
priority: normal
created: [timestamp_del_descubrimiento]
edges_out:
  - relation: "versión_a"
    target: uuid_nodo_previo_obsoleto   # copia opcional del nodo en estado obsoleto
  - relation: "versión_b"
    target: uuid_nodo_corregido         # el nodo original actualizado para reflejar la realidad
---

# Drift detectado en: [uuid_nodo_original]

Descripción de la discrepancia encontrada entre el grafo y el sistema real (código, BD o runtime), y el cambio realizado para corregirla.
```

---

## CLI: contrato de `amta_write`

`amta_write` es la interfaz de escritura del grafo. La implementación es libre (Python, bash, Node, Go). AMTA especifica el contrato, no la herramienta.

```bash
# Actualizar campo de un nodo existente
amta_write --node <uuid> --field <campo> --value <valor>

# Crear nodo nuevo desde archivo
amta_write --new --type <tipo> --content <archivo.md>

# Deprecar un nodo y señalar su sucesor
amta_write --deprecate <uuid> --superseded-by <uuid_sucesor>

# Resolver un conflicto
amta_write --resolve <uuid_disputed> --winner <uuid_version_correcta>
```

Toda escritura via `amta_write` genera automáticamente:
- Timestamp `updated` en el nodo modificado.
- Entrada en `CHANGELOG.md`.
- Incremento de `patch` en `graph_version` dentro de `STATE.md`, salvo cambios estructurales que incrementan `minor` o `major`.
- Actualización de `last_updated` en `STATE.md`.

`amta_write` trata `STATE.md` como destino de escritura obligatorio en cada operación — no solo los nodos en `graph/nodes/`.

---

## Adopción en Proyectos Existentes

1. Crea la carpeta `context/` en la raíz del proyecto.
2. Copia `SYSTEM.md` (este documento) y las plantillas de `STATE.md` y `CHANGELOG.md`.
3. Ejecuta `seed_graph.sh` — escanea el historial de Git y genera nodos iniciales para hitos relevantes.
4. Agrega manualmente los 5–10 nodos más importantes del dominio actual (arquitectura, base de datos, usuarios tipo, decisiones clave).
5. A partir de ese momento, toda decisión nueva sigue el protocolo.

---

## Adopción en Proyectos Nuevos

1. Crea `context/` vacío al iniciar.
2. Por cada decisión de diseño, reunión o cambio relevante: crea el nodo **antes** de tocar código.
3. El grafo se convierte en el diario de a bordo del proyecto desde el primer día.

---

## Integración con Fuentes Transaccionales (Git, BD, Runtime)

El grafo actúa como **índice**. Las fuentes transaccionales contienen el dato real.

- Los nodos referencian código y datos mediante enlaces (`[ver código](file:///src/...)`).
- Los scripts de drift comparan el estado del grafo con el estado real del sistema.
- Al detectar discrepancia: actualiza el grafo para que refleje la realidad. El grafo sigue a la realidad, no al revés.

---

## Contrato Final

AMTA es un **contrato de comportamiento** para todo agente — humano o IA — que participe en un proyecto.

El contrato dice:

> **Lee la fuente única. Actúa. Escríbela.**

Eso garantiza coherencia, trazabilidad, eficiencia de tokens y portabilidad total entre agentes, modelos, sesiones y equipos — sin importar el dominio ni el momento de adopción.
