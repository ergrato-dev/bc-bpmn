# Módulo 2.2: Pools, Lanes y Colaboración entre Procesos

**Duración**: 70 minutos  
**Sesión**: 2 (Segunda Semana)

---

## 🎯 Objetivos del Módulo

Al finalizar este módulo, serás capaz de:

- ✅ Comprender qué son los Pools y Lanes en BPMN
- ✅ Organizar procesos por participantes y roles
- ✅ Modelar la colaboración entre diferentes actores
- ✅ Usar mensajes para comunicación entre procesos
- ✅ Diferenciar entre procesos públicos y privados
- ✅ Crear diagramas de colaboración efectivos

---

## 📋 Contenido

### 1. Introducción a Swimlanes (10 min)

#### ¿Qué son las Swimlanes?

Las **Swimlanes** (carriles de natación) son elementos organizacionales en BPMN que permiten agrupar actividades según **quién** las ejecuta. Son llamadas así porque visualmente se asemejan a los carriles de una piscina olímpica.

**Beneficios de usar Swimlanes**:

- ✅ Clarifica **responsabilidades** (quién hace qué)
- ✅ Identifica **transferencias** entre áreas o personas
- ✅ Facilita el **análisis de carga de trabajo**
- ✅ Detecta **cuellos de botella** organizacionales
- ✅ Mejora la **comunicación** entre equipos

#### Tipos de Swimlanes en BPMN

BPMN define dos niveles de organización:

1. **Pool** (Piscina): Representa un **participante** en el proceso
2. **Lane** (Carril): Representa un **rol o sub-organización** dentro de un participante

**Analogía**: Piensa en una empresa (Pool) con diferentes departamentos (Lanes) como Ventas, Operaciones, Finanzas.

---

### 2. Pools - Participantes del Proceso (15 min)

#### ¿Qué es un Pool?

Un **Pool** representa un **participante** en el proceso. Puede ser:

- Una organización completa
- Un sistema
- Un rol de negocio general
- Una entidad externa

**Representación gráfica**: Rectángulo grande que contiene el proceso completo.

#### Características de los Pools

**Reglas fundamentales**:

1. ✅ Cada Pool contiene **un proceso independiente**
2. ✅ Los flujos de secuencia **NO pueden cruzar** entre Pools
3. ✅ La comunicación entre Pools se hace mediante **mensajes**
4. ✅ Cada Pool tiene su propio **evento de inicio y fin**
5. ✅ Los Pools se dibujan **horizontalmente** (por convención)

#### Tipos de Pools

##### A) Pool Expandido (Pool con proceso visible)

Muestra el proceso interno completo con todas sus actividades.

```
┌─ Pool: Cliente ─────────────────────────────────────┐
│ [Inicio] → (Solicitar producto) → (Recibir producto) │
│                ↓ Mensaje                              │
│           (Confirmar recepción) → [Fin]               │
└───────────────────────────────────────────────────────┘
```

**Cuándo usar**:

- Cuando necesitas mostrar los detalles del proceso
- Para procesos internos de tu organización
- Para análisis y mejora de procesos

##### B) Pool Colapsado (Caja negra)

Muestra solo el nombre del participante sin detalles internos.

```
┌─ Pool: Sistema de Pagos ────────────────┐
│ (proceso oculto - caja negra)            │
└──────────────────────────────────────────┘
```

**Cuándo usar**:

- Para participantes externos
- Cuando los detalles internos no son relevantes
- Para mantener el diagrama simple
- Para procesos de terceros (ej: pasarela de pago)

#### Ejemplo Completo: Proceso de Compra Online

```
┌─ Pool: Cliente ──────────────────────────────────────┐
│ [Inicio] → (Buscar producto) → (Agregar al carrito)  │
│               ↓                                       │
│          (Realizar pedido) ─── Mensaje: Orden ──→    │
│               ↓                                       │
│    ✉️⭕⭕ Esperar confirmación ←── Mensaje ──────    │
│               ↓                                       │
│          (Recibir producto) → [Fin]                   │
└───────────────────────────────────────────────────────┘
                                    ↓ ↑ Mensajes
┌─ Pool: Tienda Online ────────────────────────────────┐
│    ✉️⭕⭕ Recibir orden                               │
│          ↓                                            │
│    (Verificar stock) → ◇ ¿Hay stock?                 │
│          ├─ Sí → (Procesar pago) ─── Mensaje ──→     │
│          │         ↓                                  │
│          │    (Preparar envío)                        │
│          │         ↓                                  │
│          │    (Enviar confirmación) ── Mensaje ──→   │
│          │         ↓                                  │
│          │    [Fin: Enviado]                          │
│          │                                            │
│          └─ No → (Notificar falta) ── Mensaje ──→    │
│                   ↓                                   │
│              [Fin: Cancelado]                         │
└───────────────────────────────────────────────────────┘
```

**Observaciones importantes**:

- Cada Pool tiene su propio flujo independiente
- Las flechas de mensaje (punteadas) cruzan entre Pools
- Los flujos de secuencia (sólidos) NO cruzan Pools
- Cada Pool puede tener múltiples finales

---

### 3. Lanes - Roles y Responsabilidades (15 min)

#### ¿Qué es un Lane?

Un **Lane** (carril) representa una **sub-organización, rol o responsabilidad** dentro de un Pool.

**Representación gráfica**: Subdivisión horizontal o vertical dentro de un Pool.

#### Características de los Lanes

**Reglas**:

1. ✅ Los Lanes están **dentro** de un Pool
2. ✅ Los flujos **SÍ pueden cruzar** entre Lanes del mismo Pool
3. ✅ Ayudan a organizar el proceso por **responsabilidades**
4. ✅ No afectan la ejecución, son solo organizacionales
5. ✅ Pueden estar **anidados** (Lanes dentro de Lanes)

#### Ejemplo: Proceso de Aprobación de Vacaciones

```
┌─ Pool: Empresa ──────────────────────────────────────────┐
│                                                            │
│ ┌─ Lane: Empleado ──────────────────────────────────┐    │
│ │ [Inicio] → (Solicitar vacaciones) ─────────┐      │    │
│ │                                             ↓      │    │
│ │                              ✉️⭕⭕ Esperar decisión│    │
│ │                                             ↓      │    │
│ │                            ◇ ¿Aprobado?           │    │
│ │                             ├─ Sí → [Fin: Aprobado]│   │
│ │                             └─ No → [Fin: Rechazado]   │
│ └────────────────────────────────────────────────────┘    │
│                                 ↓ ↑                       │
│ ┌─ Lane: Jefe Directo ──────────────────────────────┐    │
│ │       ✉️⭕⭕ Recibir solicitud                      │    │
│ │              ↓                                      │    │
│ │       (Revisar días disponibles)                   │    │
│ │              ↓                                      │    │
│ │       ◇ ¿Más de 10 días?                          │    │
│ │        ├─ No → (Aprobar) ─── Mensaje ──→          │    │
│ │        │        ↓                                  │    │
│ │        │   [Fin: Aprobado]                         │    │
│ │        │                                           │    │
│ │        └─ Sí → (Escalar a Gerente) ───────┐       │    │
│ └───────────────────────────────────────────────────┘    │
│                                              ↓            │
│ ┌─ Lane: Gerente ───────────────────────────────────┐    │
│ │              (Revisar impacto)                     │    │
│ │                   ↓                                │    │
│ │              (Decidir)                             │    │
│ │                   ↓                                │    │
│ │         (Notificar decisión) ─── Mensaje ──→      │    │
│ │                   ↓                                │    │
│ │              [Fin]                                 │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

**Beneficios visibles**:

- Claridad de **quién hace cada actividad**
- Identificación de **transferencias** entre roles (handoffs)
- Análisis de **carga de trabajo** por rol
- Detección de **cuellos de botella** organizacionales

#### Lanes Anidados (Sub-Lanes)

Puedes tener Lanes dentro de Lanes para mayor granularidad.

```
┌─ Pool: Hospital ─────────────────────────────────────┐
│                                                       │
│ ┌─ Lane: Área Médica ────────────────────────────┐  │
│ │                                                 │  │
│ │ ┌─ Sub-Lane: Médico General ────────────┐      │  │
│ │ │ (Revisar paciente) → (Diagnosticar)   │      │  │
│ │ └────────────────────────────────────────┘      │  │
│ │                                                 │  │
│ │ ┌─ Sub-Lane: Especialista ───────────────┐     │  │
│ │ │ (Evaluación especializada)              │     │  │
│ │ └─────────────────────────────────────────┘     │  │
│ └─────────────────────────────────────────────────┘  │
│                                                       │
│ ┌─ Lane: Administración ─────────────────────────┐  │
│ │ (Facturar) → (Programar seguimiento)           │  │
│ └────────────────────────────────────────────────┘  │
│                                                       │
└───────────────────────────────────────────────────────┘
```

---

### 4. Mensajes - Comunicación entre Procesos (15 min)

#### ¿Qué son los Mensajes?

Los **mensajes** representan **comunicación** entre diferentes participantes (Pools).

**Representación gráfica**: Flecha **punteada** con sobre ✉️

#### Tipos de Flujos de Mensajes

##### A) Mensaje Simple

Comunicación unidireccional sin esperar respuesta.

```
(Enviar notificación) ─── ✉️ ──→ ✉️⭕⭕ (Recibir notificación)
    Pool A                            Pool B
```

##### B) Mensaje con Respuesta

Patrón solicitud-respuesta.

```
(Solicitar aprobación) ─── ✉️ ──→ ✉️⭕⭕ (Recibir solicitud)
    Pool A                              Pool B
                                             ↓
                                        (Aprobar)
                                             ↓
✉️⭕⭕ (Recibir respuesta) ←─── ✉️ ─── (Enviar respuesta)
    Pool A                              Pool B
```

#### Reglas de los Mensajes

1. ✅ **Solo pueden conectar** diferentes Pools
2. ✅ **No pueden conectar** elementos dentro del mismo Pool
3. ✅ Pueden conectar:
   - Actividad → Evento intermedio de mensaje
   - Evento intermedio → Actividad
   - Actividad → Actividad (menos común)
4. ✅ Se representan con **línea punteada**
5. ✅ Llevan un **sobre** como decoración

#### Eventos de Mensaje

##### Evento de Inicio por Mensaje

```
✉️⭕ [Inicio: Cliente envía orden]
```

El proceso comienza cuando llega un mensaje.

##### Evento Intermedio de Captura de Mensaje

```
✉️⭕⭕ [Esperar confirmación del proveedor]
```

Pausa el proceso esperando un mensaje.

##### Evento Intermedio de Lanzamiento de Mensaje

```
✉️⚫⭕ [Enviar recordatorio]
```

Envía un mensaje durante el proceso.

##### Evento de Fin por Mensaje

```
✉️⚫ [Fin: Enviar factura al cliente]
```

Termina el proceso enviando un mensaje.

#### Ejemplo Completo: Sistema de Órdenes

```
┌─ Pool: Cliente ──────────────────────────────────────┐
│ [Inicio] → (Crear orden) ─── ✉️ Orden ──→           │
│                 ↓                                     │
│     ✉️⭕⭕ Esperar confirmación                       │
│                 ↓                                     │
│     ◇ ¿Confirmada?                                   │
│      ├─ Sí → (Realizar pago) ─── ✉️ Pago ──→        │
│      │          ↓                                    │
│      │   ✉️⭕⭕ Esperar recibo                        │
│      │          ↓                                    │
│      │     [Fin: Completado]                         │
│      │                                               │
│      └─ No → [Fin: Cancelado]                        │
└───────────────────────────────────────────────────────┘
                        ↓ ↑ Mensajes
┌─ Pool: Sistema de Ventas ────────────────────────────┐
│     ✉️⭕ Inicio: Orden recibida                      │
│              ↓                                        │
│      (Validar orden)                                 │
│              ↓                                        │
│      ◇ ¿Válida?                                      │
│       ├─ Sí → (Confirmar orden) ─── ✉️ ──→          │
│       │           ↓                                  │
│       │   ✉️⭕⭕ Esperar pago                         │
│       │           ↓                                  │
│       │   (Procesar pago)                            │
│       │           ↓                                  │
│       │   (Enviar recibo) ─── ✉️ ──→               │
│       │           ↓                                  │
│       │   (Despachar orden)                          │
│       │           ↓                                  │
│       │   [Fin: Despachado]                          │
│       │                                              │
│       └─ No → (Rechazar) ─── ✉️ Rechazo ──→        │
│                   ↓                                  │
│              [Fin: Rechazado]                        │
└──────────────────────────────────────────────────────┘
```

---

### 5. Procesos Públicos vs Privados (10 min)

#### Proceso Privado (Private Process)

**Definición**: Proceso interno que se ejecuta dentro de un Pool, **no visible** para otros participantes.

**Características**:

- Solo el participante dueño conoce los detalles
- Contiene la lógica de negocio interna
- Puede tener compuertas, subprocesos, etc.

**Ejemplo**: Los pasos internos que realiza un banco para aprobar un préstamo.

#### Proceso Público (Public Process)

**Definición**: Parte del proceso que **interactúa** con otros participantes. Son los puntos de contacto.

**Características**:

- Representa la "interfaz" del proceso
- Solo muestra actividades de envío/recepción de mensajes
- Oculta la lógica interna

**Ejemplo**: El banco notifica al cliente el resultado de la aprobación.

#### Visualización: Proceso Público vs Privado

```
┌─ Pool: Banco (Proceso Privado) ──────────────────────┐
│                                                        │
│ ✉️⭕ Recibir solicitud de préstamo ← PÚBLICO         │
│         ↓                                              │
│ (Verificar identidad) ← PRIVADO                       │
│         ↓                                              │
│ (Consultar buró de crédito) ← PRIVADO                │
│         ↓                                              │
│ (Evaluar riesgo) ← PRIVADO                            │
│         ↓                                              │
│ ◇ ¿Aprobar? ← PRIVADO                                │
│  ├─ Sí → (Generar contrato) ← PRIVADO                │
│  │         ↓                                          │
│  │   ✉️⚫ Enviar aprobación ← PÚBLICO                 │
│  │                                                    │
│  └─ No → ✉️⚫ Enviar rechazo ← PÚBLICO                │
│                                                        │
└────────────────────────────────────────────────────────┘
```

**Importante**: Los otros participantes solo ven los mensajes (puntos PÚBLICOS), no saben qué pasa internamente.

---

### 6. Diagramas de Colaboración (10 min)

#### ¿Qué es un Diagrama de Colaboración?

Un **Diagrama de Colaboración** muestra cómo **múltiples participantes** interactúan en un proceso de negocio completo.

**Elementos clave**:

- Múltiples Pools (participantes)
- Mensajes entre Pools
- Coordinación de actividades
- Flujos independientes por participante

#### Ejemplo Completo: Proceso de Atención Médica

```
┌─ Pool: Paciente ──────────────────────────────────────┐
│ [Inicio] → (Solicitar cita) ─── ✉️ ──→               │
│                 ↓                                      │
│     ✉️⭕⭕ Esperar confirmación                        │
│                 ↓                                      │
│     (Asistir a cita)                                  │
│                 ↓                                      │
│     ✉️⭕⭕ Recibir diagnóstico                         │
│                 ↓                                      │
│     (Ir a farmacia) ─── ✉️ Receta ──→                │
│                 ↓                                      │
│     ✉️⭕⭕ Recibir medicamentos                        │
│                 ↓                                      │
│     [Fin]                                             │
└───────────────────────────────────────────────────────┘
                         ↓ ↑ Mensajes
┌─ Pool: Hospital ──────────────────────────────────────┐
│                                                        │
│ ┌─ Lane: Recepción ───────────────────────────────┐  │
│ │ ✉️⭕ Recibir solicitud cita                      │  │
│ │         ↓                                        │  │
│ │ (Verificar disponibilidad)                       │  │
│ │         ↓                                        │  │
│ │ (Agendar cita) ─── ✉️ ──→                       │  │
│ └──────────────────────────────────────────────────┘  │
│                                                        │
│ ┌─ Lane: Médico ──────────────────────────────────┐  │
│ │ (Atender paciente)                               │  │
│ │         ↓                                        │  │
│ │ (Diagnosticar)                                   │  │
│ │         ↓                                        │  │
│ │ (Prescribir tratamiento)                         │  │
│ │         ↓                                        │  │
│ │ (Enviar diagnóstico) ─── ✉️ ──→                 │  │
│ │         ↓                                        │  │
│ │ (Generar receta) ─── ✉️ Receta ──→              │  │
│ └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
                         ↓ Mensaje Receta
┌─ Pool: Farmacia (Colapsado) ──────────────────────────┐
│ ✉️⭕ Recibir receta                                   │
│         ↓                                              │
│ (Preparar medicamentos)                               │
│         ↓                                              │
│ (Entregar) ─── ✉️ ──→                                │
│         ↓                                              │
│ [Fin]                                                 │
└────────────────────────────────────────────────────────┘
```

**Beneficios de este tipo de diagrama**:

- Visión completa del proceso end-to-end
- Identifica todos los participantes involucrados
- Muestra dependencias entre organizaciones
- Facilita la coordinación entre áreas

---

## 🧩 Ejercicios Prácticos (5 min)

### Ejercicio 1: Identificar Pools y Lanes

**Escenario**: Proceso de Contratación de Personal

**Participantes**:

- Candidato (externo)
- Empresa (con departamentos: RRHH, Gerente del área, Legal)

**Pregunta**: ¿Cuántos Pools y cuántos Lanes necesitas?

<details>
<summary>Ver respuesta</summary>

**Pools**: 2

1. Pool: Candidato
2. Pool: Empresa

**Lanes dentro de "Empresa"**: 3

1. Lane: RRHH
2. Lane: Gerente del área
3. Lane: Legal

**Justificación**:

- Candidato es un participante externo → Pool separado
- Los departamentos son roles dentro de la misma organización → Lanes dentro del Pool "Empresa"

</details>

---

### Ejercicio 2: Diseñar Colaboración Simple

**Escenario**: "Solicitud de Soporte Técnico"

**Descripción**:

1. Cliente reporta problema (envía ticket)
2. Mesa de ayuda recibe ticket y lo analiza
3. Si puede resolver → Resuelve y notifica
4. Si no puede resolver → Escala a especialista
5. Especialista resuelve y notifica
6. Cliente confirma resolución

**Tarea**: Dibuja el diagrama con:

- Pools apropiados
- Mensajes entre participantes
- Lanes si es necesario

<details>
<summary>Ver diagrama sugerido</summary>

```
┌─ Pool: Cliente ──────────────────────────────────────┐
│ [Inicio] → (Reportar problema) ─── ✉️ Ticket ──→    │
│                 ↓                                     │
│     ✉️⭕⭕ Esperar resolución                         │
│                 ↓                                     │
│     (Verificar solución)                             │
│                 ↓                                     │
│     (Confirmar) ─── ✉️ Confirmación ──→             │
│                 ↓                                     │
│     [Fin]                                            │
└──────────────────────────────────────────────────────┘
                         ↓ ↑ Mensajes
┌─ Pool: Soporte ───────────────────────────────────────┐
│                                                        │
│ ┌─ Lane: Mesa de Ayuda ──────────────────────────┐   │
│ │ ✉️⭕ Recibir ticket                             │   │
│ │         ↓                                       │   │
│ │ (Analizar problema)                             │   │
│ │         ↓                                       │   │
│ │ ◇ ¿Puede resolver?                             │   │
│ │  ├─ Sí → (Resolver)                            │   │
│ │  │         ↓                                   │   │
│ │  │   (Notificar cliente) ─── ✉️ ──→           │   │
│ │  │         ↓                                   │   │
│ │  │   ✉️⭕⭕ Esperar confirmación                │   │
│ │  │         ↓                                   │   │
│ │  │   [Fin]                                     │   │
│ │  │                                             │   │
│ │  └─ No → (Escalar a especialista) ───────┐    │   │
│ └──────────────────────────────────────────────────┘   │
│                                              ↓         │
│ ┌─ Lane: Especialista ─────────────────────────────┐  │
│ │         (Recibir escalamiento)                   │  │
│ │                 ↓                                │  │
│ │         (Resolver problema)                      │  │
│ │                 ↓                                │  │
│ │         (Notificar cliente) ─── ✉️ ──→          │  │
│ │                 ↓                                │  │
│ │         ✉️⭕⭕ Esperar confirmación               │  │
│ │                 ↓                                │  │
│ │         [Fin]                                    │  │
│ └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

</details>

---

## 🎯 Mejores Prácticas

### Para Pools

✅ **HACER**:

- Usar Pools para participantes claramente diferentes
- Colapsar Pools de participantes externos si no son relevantes
- Mantener un Pool por organización/sistema
- Nombrar Pools claramente (ej: "Cliente", "Banco", "Sistema ERP")

❌ **EVITAR**:

- Crear Pools innecesarios para roles internos (usar Lanes)
- Mezclar procesos de diferentes contextos en un Pool
- Pools muy complejos (dividir en múltiples diagramas)

### Para Lanes

✅ **HACER**:

- Usar Lanes para roles dentro de la misma organización
- Alinear Lanes horizontalmente para mejor legibilidad
- Nombrar Lanes por rol o departamento
- Mantener 3-5 Lanes máximo por diagrama

❌ **EVITAR**:

- Demasiados Lanes (dificulta la lectura)
- Lanes para cosas que no son roles (ej: "Proceso urgente")
- Lanes vacíos o con pocas actividades

### Para Mensajes

✅ **HACER**:

- Etiquetar mensajes con el contenido (ej: "Orden", "Factura")
- Usar eventos de mensaje cuando sea apropiado
- Mantener mensajes cortos y directos
- Indicar claramente la dirección del mensaje

❌ **EVITAR**:

- Mensajes dentro del mismo Pool (usar flujos de secuencia)
- Demasiados mensajes cruzados (confunde el diagrama)
- Mensajes sin etiquetar

---

## 🔑 Conceptos Clave para Recordar

| Elemento            | Descripción                | Uso                      |
| ------------------- | -------------------------- | ------------------------ |
| **Pool**            | Participante en el proceso | Organizaciones, sistemas |
| **Lane**            | Rol dentro de un Pool      | Departamentos, roles     |
| **Mensaje**         | Comunicación entre Pools   | Línea punteada con sobre |
| **Proceso Privado** | Lógica interna             | No visible externamente  |
| **Proceso Público** | Interfaz del proceso       | Puntos de comunicación   |
| **Colaboración**    | Múltiples participantes    | Diagrama completo        |

---

## 🎓 Comparación Rápida

| Aspecto                | Pool                        | Lane                          |
| ---------------------- | --------------------------- | ----------------------------- |
| **Representa**         | Participante/Organización   | Rol/Departamento              |
| **Contiene**           | Proceso completo            | Actividades específicas       |
| **Flujos entre ellos** | Solo mensajes (punteados)   | Flujos de secuencia (sólidos) |
| **Independencia**      | Procesos independientes     | Mismo proceso                 |
| **Ejemplo**            | Cliente, Proveedor, Sistema | Ventas, Finanzas, Gerente     |

---

## 🎯 Checkpoint - Autoevaluación

1. ¿Cuál es la diferencia entre un Pool y un Lane?
2. ¿Pueden los flujos de secuencia cruzar entre Pools?
3. ¿Cómo se comunican diferentes Pools?
4. ¿Qué representa un Pool colapsado?
5. ¿Cuándo usar Lanes anidados?
6. ¿Qué es un proceso público vs privado?

<details>
<summary>Ver respuestas</summary>

1. Pool = participante/organización; Lane = rol dentro de un Pool
2. No, solo mensajes pueden cruzar entre Pools
3. Mediante mensajes (flechas punteadas)
4. Un participante del que no mostramos los detalles internos
5. Cuando hay jerarquía de roles dentro de un área
6. Público = interfaz visible; Privado = lógica interna oculta

</details>

---

## ⏭️ Siguiente Módulo

**Módulo 2.3: Elementos Avanzados y Manejo de Excepciones**

Aprenderás:

- Eventos de frontera (Boundary Events)
- Manejo de excepciones y errores
- Eventos de compensación
- Eventos de escalamiento
- Transacciones en BPMN

---

## 📚 Casos de Uso Reales

### Caso 1: E-commerce

- Pool: Cliente, Tienda, Pasarela de Pago, Logística
- Lanes en Tienda: Ventas, Almacén, Atención al Cliente

### Caso 2: Aprobación de Crédito

- Pool: Solicitante, Banco
- Lanes en Banco: Oficial de Crédito, Comité, Legal

### Caso 3: Proceso de Compras

- Pool: Empresa Compradora, Proveedor
- Lanes en Empresa: Solicitante, Compras, Finanzas, Gerencia

---

_Última actualización: Noviembre 2025_
