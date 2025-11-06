# Módulo 1.2: Elementos Básicos - Eventos, Actividades y Flujos

**Duración**: 150 minutos (2.5 horas)  
**Sesión**: 1 (Primera Semana)

---

## 🎯 Objetivos del Módulo

Al finalizar este módulo, serás capaz de:

- ✅ Identificar y usar correctamente los 3 tipos de eventos
- ✅ Crear y nombrar actividades y tareas apropiadamente
- ✅ Diferenciar entre tareas simples y subprocesos
- ✅ Conectar elementos con flujos de secuencia
- ✅ Aplicar los elementos básicos en diagramas sencillos

---

## 📋 Contenido

## PARTE 1: EVENTOS (40 minutos)

### 1.1 ¿Qué son los Eventos?

Los eventos representan **algo que sucede** durante el proceso. Son disparadores o resultados, no acciones.

**Representación gráfica**: Círculos ⭕

**Características**:

- Ocurren instantáneamente (no consumen tiempo por sí mismos)
- Afectan el flujo del proceso
- Pueden tener un "trigger" (disparador)

---

### 1.2 Evento de Inicio (Start Event)

**Símbolo**: ⭕ Círculo con borde simple

**Función**: Marca el comienzo del proceso

**Reglas**:

- ✓ Solo puede haber **un** evento de inicio por proceso (regla general)
- ✓ **No tiene** flujos entrantes
- ✓ **Sí tiene** un flujo saliente
- ✓ Puede tener un trigger que indica qué inicia el proceso

#### Tipos Comunes de Eventos de Inicio

| Tipo            | Ícono Interno | Cuándo Usar                 | Ejemplo                   |
| --------------- | ------------- | --------------------------- | ------------------------- |
| **None**        | (vacío)       | Inicio genérico             | "Día laboral inicia"      |
| **Message**     | ✉️ sobre      | Llega un mensaje/solicitud  | "Cliente envía orden"     |
| **Timer**       | ⏰ reloj      | Momento temporal específico | "Cada lunes 9am"          |
| **Signal**      | 🔺 triángulo  | Señal broadcast             | "Cierre mensual iniciado" |
| **Conditional** | 📋 documento  | Se cumple una condición     | "Stock < mínimo"          |

#### Ejemplos Reales

```
✉️ [Inicio: Cliente solicita producto]
   ↓
   (resto del proceso...)
```

```
⏰ [Inicio: Todos los lunes a las 9:00]
   ↓
   (Generar reporte semanal)
```

#### Buenas Prácticas para Nombrar

✅ **HACER**:

- "Cliente solicita cotización"
- "Empleado llega al trabajo"
- "Sistema detecta error"

❌ **EVITAR**:

- "Inicio" (muy genérico)
- "El proceso comienza cuando..." (muy largo)
- "Iniciar proceso" (redundante)

---

### 1.3 Evento Intermedio (Intermediate Event)

**Símbolo**: ⭕⭕ Círculo con doble borde

**Función**: Representa algo que ocurre **durante** la ejecución del proceso

**Tipos Principales**:

#### A) Catching (Captura) - Borde doble vacío

**Función**: **Espera** que algo suceda

Pausa el flujo hasta que el evento ocurra.

**Ejemplos**:

| Tipo               | Uso               | Ejemplo                          |
| ------------------ | ----------------- | -------------------------------- |
| **Timer** ⏰       | Esperar un tiempo | "Esperar 24 horas"               |
| **Message** ✉️     | Esperar respuesta | "Recibir aprobación del cliente" |
| **Signal** 🔺      | Esperar señal     | "Esperar inicio de producción"   |
| **Conditional** 📋 | Esperar condición | "Hasta que stock > 10"           |

```
(Enviar cotización al cliente)
   ↓
⏰ [Esperar 48 horas]
   ↓
(Hacer seguimiento)
```

#### B) Throwing (Lanzamiento) - Borde doble relleno

**Función**: **Genera** un evento/señal

No pausa, solo envía.

**Ejemplos**:

| Tipo           | Uso            | Ejemplo                         |
| -------------- | -------------- | ------------------------------- |
| **Message** ✉️ | Enviar mensaje | "Enviar notificación por email" |
| **Signal** 🔺  | Emitir señal   | "Avisar a producción iniciar"   |

```
(Aprobar orden)
   ↓
✉️ [Enviar confirmación al cliente]
   ↓
(Procesar pago)
```

---

### 1.4 Evento de Fin (End Event)

**Símbolo**: ⚫ Círculo con borde grueso

**Función**: Marca el final del proceso

**Reglas**:

- ✓ Puede haber **múltiples** eventos de fin (diferentes resultados)
- ✓ **Sí tiene** flujos entrantes
- ✓ **No tiene** flujos salientes
- ✓ Indica el resultado/tipo de finalización

#### Tipos Comunes de Eventos de Fin

| Tipo          | Ícono     | Significado                  | Ejemplo                     |
| ------------- | --------- | ---------------------------- | --------------------------- |
| **None**      | (vacío)   | Fin normal                   | "Proceso completado"        |
| **Message**   | ✉️        | Envía mensaje final          | "Enviar factura al cliente" |
| **Terminate** | ⬛ cuadro | Termina TODAS las instancias | "Cancelar proceso completo" |
| **Error**     | ⚡ rayo   | Fin con error                | "Proceso falló"             |

#### Ejemplo con Múltiples Finales

```
◇ ¿Aprobado?
  ├─ Sí → (Procesar) → ⚫ [Fin: Orden completada]
  └─ No → (Notificar) → ⚫ [Fin: Orden rechazada]
```

---

### 1.5 Tabla Resumen de Eventos

| Posición       | Símbolo | Flujos Entrantes | Flujos Salientes | Cantidad        |
| -------------- | ------- | ---------------- | ---------------- | --------------- |
| **Inicio**     | ⭕      | No               | Sí (uno)         | Uno por proceso |
| **Intermedio** | ⭕⭕    | Sí               | Sí               | Varios          |
| **Fin**        | ⚫      | Sí               | No               | Uno o más       |

---

## PARTE 2: ACTIVIDADES (50 minutos)

### 2.1 ¿Qué son las Actividades?

Las actividades representan **trabajo que se realiza** en el proceso.

**Representación gráfica**: 📋 Rectángulo con esquinas redondeadas

**Características**:

- Consumen tiempo
- Consumen recursos (personas, sistemas)
- Transforman o generan algo
- Tienen un verbo de acción

---

### 2.2 Tarea (Task)

**Símbolo**: 📋 Rectángulo redondeado

**Función**: Unidad de trabajo **atómica** (no se descompone más en el diagrama)

#### Tipos de Tareas

| Tipo                   | Ícono   | Descripción        | Ejecutada Por               | Ejemplo                     |
| ---------------------- | ------- | ------------------ | --------------------------- | --------------------------- |
| **Task**               | (vacío) | Tarea genérica     | No especificado             | "Revisar documento"         |
| **User Task**          | 👤      | Tarea manual       | Usuario humano              | "Aprobar solicitud"         |
| **Service Task**       | ⚙️      | Tarea automatizada | Sistema/API                 | "Consultar base de datos"   |
| **Script Task**        | 📜      | Ejecuta script     | Motor de proceso            | "Calcular total con IVA"    |
| **Send Task**          | 📤      | Envía mensaje      | Sistema                     | "Enviar email confirmación" |
| **Receive Task**       | 📥      | Espera mensaje     | Sistema                     | "Recibir confirmación pago" |
| **Manual Task**        | ✋      | Tarea física       | Persona (fuera del sistema) | "Imprimir y firmar"         |
| **Business Rule Task** | 📊      | Ejecuta regla      | Motor de reglas             | "Evaluar riesgo crediticio" |

#### Buenas Prácticas para Nombrar Tareas

**Formato recomendado**: `Verbo + Objeto`

✅ **HACER**:

- "Validar datos del cliente"
- "Enviar correo de confirmación"
- "Calcular monto total"
- "Aprobar presupuesto"
- "Registrar en sistema"

❌ **EVITAR**:

- "Validación" (sustantivo)
- "Datos" (sin verbo)
- "El sistema debe revisar y validar los datos ingresados por el usuario" (muy largo)
- "Tarea 1" (no descriptivo)

**Reglas de oro**:

1. Usa verbos en infinitivo o imperativo
2. Sé específico pero conciso
3. Máximo 5-7 palabras
4. Evita detalles técnicos a menos que sea nivel ejecutable

---

### 2.3 Subproceso (Sub-Process)

**Símbolo**: 📋 Rectángulo redondeado con símbolo **[+]** en el centro inferior

**Función**: Actividad que contiene un proceso completo dentro

#### Tipos de Subprocesos

##### A) Subproceso Colapsado

- Muestra solo el nombre
- Oculta los detalles internos
- Tiene el símbolo **[+]**
- Útil para mantener el diagrama simple

```
[Inicio] → (Recibir orden) → [Gestionar pago +] → (Enviar producto) → [Fin]
```

##### B) Subproceso Expandido

- Muestra el contenido interno
- El proceso interno tiene su propio flujo
- **No** tiene el símbolo [+]
- Útil cuando los detalles son relevantes

```
┌─ Gestionar Pago ──────────────────┐
│  [Inicio Sub] → (Validar tarjeta) │
│       ↓                             │
│  ◇ ¿Válida?                        │
│    ├─ Sí → (Procesar cargo)        │
│    └─ No → (Notificar error)       │
│       ↓                             │
│  [Fin Sub]                          │
└─────────────────────────────────────┘
```

#### ¿Cuándo usar un Subproceso?

✅ **Usar subproceso cuando**:

- Una tarea es demasiado compleja
- Quieres reutilizar en múltiples lugares
- Necesitas organizar mejor el diagrama
- El detalle no es relevante en el nivel actual
- Es un proceso estándar conocido

❌ **NO usar subproceso si**:

- Son solo 2-3 pasos simples
- No se reutiliza
- Dificulta la comprensión

---

### 2.4 Tarea Múltiple (Multi-Instance)

**Símbolo**: 📋 Rectángulo con **tres líneas paralelas** en la parte inferior |||

**Función**: Tarea que se ejecuta **múltiples veces**

#### Tipos

##### A) Multi-Instance Secuencial

**Símbolo**: ⚊⚊⚊ (líneas horizontales)

**Comportamiento**: Una instancia después de otra

**Ejemplo**:

```
📋 "Revisar facturas" ⚊⚊⚊
```

Si hay 5 facturas → se revisan una por una en orden.

**Casos de uso**:

- Procesar ítems de una lista en orden
- Aprobar documentos secuencialmente
- Validar datos paso a paso

##### B) Multi-Instance Paralelo

**Símbolo**: ⚊⚊⚊ (líneas verticales)

**Comportamiento**: Todas las instancias simultáneamente

**Ejemplo**:

```
📋 "Enviar notificación a aprobadores" ⚊⚊⚊
```

Si hay 3 aprobadores → todos reciben notificación al mismo tiempo.

**Casos de uso**:

- Enviar emails a múltiples destinatarios
- Votación/aprobación paralela
- Procesamiento en lote

---

## PARTE 3: FLUJOS DE SECUENCIA (30 minutos)

### 3.1 ¿Qué son los Flujos?

**Símbolo**: → Flecha sólida

**Función**: Conecta elementos y define el **orden de ejecución**

**Reglas**:

- ✓ Muestra la dirección del flujo
- ✓ Conecta eventos, actividades y compuertas
- ✓ **No puede** cruzar boundaries de Pools (usar mensajes)
- ✓ Puede tener condiciones (en compuertas)

---

### 3.2 Tipos de Flujos

#### A) Flujo Normal (Normal Flow)

Conexión estándar sin condiciones.

```
[Evento Inicio] → (Actividad A) → (Actividad B) → [Evento Fin]
```

#### B) Flujo Condicional (Conditional Flow)

Sale de una compuerta, tiene una condición.

```
◇ "¿Monto > $1000?"
  ├─→ [Sí, monto > 1000] → (Aprobación gerencial)
  └─→ [No, monto <= 1000] → (Aprobación automática)
```

**Buenas prácticas**:

- Etiquetar claramente cada condición
- Las condiciones deben ser mutuamente excluyentes
- Todas las salidas deben estar cubiertas

#### C) Flujo por Defecto (Default Flow)

**Símbolo**: →/ (flecha con barra diagonal al inicio)

Se toma cuando **ninguna otra condición** se cumple.

```
◇ "Clasificar cliente"
  ├─→ [VIP] → (Atención premium)
  ├─→ [Frecuente] → (Atención preferencial)
  └─→/ [Otros] → (Atención estándar) ← Flujo por defecto
```

---

### 3.3 Reglas de Conexión

#### ✅ Conexiones Permitidas

| Desde         | Hacia      | Válido |
| ------------- | ---------- | ------ |
| Evento Inicio | Actividad  | ✅     |
| Evento Inicio | Compuerta  | ✅     |
| Actividad     | Actividad  | ✅     |
| Actividad     | Compuerta  | ✅     |
| Actividad     | Evento Fin | ✅     |
| Compuerta     | Actividad  | ✅     |
| Compuerta     | Evento     | ✅     |

#### ❌ Conexiones NO Permitidas

| Desde         | Hacia         | Problema               |
| ------------- | ------------- | ---------------------- |
| Evento Inicio | Evento Inicio | No tiene sentido       |
| Evento Fin    | Cualquiera    | El fin no tiene salida |
| Cruzar Pools  | Usar Flujos   | Debe usar mensajes     |

---

## PARTE 4: EJERCICIOS PRÁCTICOS (30 minutos)

### Ejercicio 1: Identificar Elementos Básicos

Dado el siguiente proceso de "Solicitud de Vacaciones":

1. Empleado solicita vacaciones (inicia proceso)
2. Sistema valida si tiene días disponibles
3. Jefe revisa solicitud
4. Si aprueba → Registrar en sistema → Notificar empleado → Fin (aprobado)
5. Si rechaza → Notificar empleado → Fin (rechazado)

**Tarea**: Identifica:

- ¿Cuántos eventos de inicio hay?
- ¿Cuántas actividades/tareas hay?
- ¿Cuántos eventos de fin hay?
- ¿Qué tipo de evento de inicio es?

<details>
<summary>Ver respuesta</summary>

- **1 evento de inicio**: "Empleado solicita vacaciones" (Message Start)
- **4 actividades**: Validar días, Revisar solicitud, Registrar, Notificar
- **2 eventos de fin**: "Aprobado" y "Rechazado" (None End)
- **Tipo**: Message Start Event (porque llega una solicitud)

</details>

---

### Ejercicio 2: Nombrar Correctamente

Corrige los siguientes nombres de tareas:

1. ❌ "Revisión"
2. ❌ "El sistema debe validar los datos"
3. ❌ "Tarea 3"
4. ❌ "Validación y aprobación de documentos"

<details>
<summary>Ver respuestas sugeridas</summary>

1. ✅ "Revisar documento"
2. ✅ "Validar datos"
3. ✅ "Aprobar presupuesto" (depende del contexto)
4. ✅ Separar en dos tareas: "Validar documentos" y "Aprobar documentos"

</details>

---

### Ejercicio 3: Diseñar un Proceso Simple

**Escenario**: Proceso de "Registro de Usuario en Aplicación"

**Descripción**:

1. Usuario ingresa datos
2. Sistema valida formato de email
3. Si es válido → Enviar email de confirmación → Esperar clic → Activar cuenta → Fin (éxito)
4. Si es inválido → Mostrar error → Fin (error)

**Tarea**: Dibuja el diagrama con:

- Eventos de inicio y fin apropiados
- Actividades bien nombradas
- Flujos condicionales claros

<details>
<summary>Ver diagrama sugerido</summary>

```
⭕ [Inicio: Usuario se registra]
   ↓
📋 (Ingresar datos personales)
   ↓
⚙️ (Validar formato de email)
   ↓
◇ ¿Email válido?
  ├─ Sí → 📤 (Enviar email confirmación)
  │         ↓
  │      ✉️⭕⭕ [Esperar clic en link]
  │         ↓
  │      📋 (Activar cuenta)
  │         ↓
  │      ⚫ [Fin: Cuenta activada]
  │
  └─ No → 📋 (Mostrar mensaje de error)
            ↓
         ⚫ [Fin: Registro fallido]
```

</details>

---

## 🔑 Conceptos Clave para Recordar

| Elemento              | Forma | Función            | Ejemplo            |
| --------------------- | ----- | ------------------ | ------------------ |
| **Evento Inicio**     | ⭕    | Inicia el proceso  | "Cliente solicita" |
| **Evento Intermedio** | ⭕⭕  | Ocurre durante     | "Esperar 24h"      |
| **Evento Fin**        | ⚫    | Termina el proceso | "Completado"       |
| **Tarea**             | 📋    | Trabajo atómico    | "Validar datos"    |
| **Subproceso**        | 📋[+] | Proceso anidado    | "Gestionar pago"   |
| **Flujo**             | →     | Orden de ejecución | Conexión           |

---

## 🎯 Checkpoint - Autoevaluación

1. ¿Cuál es la diferencia entre un evento de inicio y un evento intermedio?
2. ¿Puede un evento de fin tener flujos salientes?
3. ¿Cuál es el formato recomendado para nombrar tareas?
4. ¿Cuándo usar un subproceso colapsado vs expandido?
5. ¿Qué significa el símbolo [+] en una actividad?
6. ¿Qué es un flujo por defecto y cuándo se usa?

<details>
<summary>Ver respuestas</summary>

1. Inicio: marca el comienzo; Intermedio: ocurre durante la ejecución
2. No, los eventos de fin no tienen flujos salientes
3. Verbo + Objeto (ej: "Validar datos")
4. Colapsado: cuando el detalle no es relevante; Expandido: cuando necesitas mostrar el contenido
5. Indica que es un subproceso que contiene más actividades dentro
6. Flujo que se toma cuando ninguna otra condición se cumple; útil en compuertas para casos no contemplados

</details>

---

## ⏭️ Siguiente Módulo

**Módulo 2.1: Compuertas y Control de Flujo**

Aprenderás:

- Compuerta Exclusiva (XOR)
- Compuerta Paralela (AND)
- Compuerta Inclusiva (OR)
- Compuerta Basada en Eventos
- Cuándo usar cada una

---

_Última actualización: Noviembre 2025_
