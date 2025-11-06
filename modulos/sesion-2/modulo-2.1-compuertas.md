# Módulo 2.1: Compuertas y Control de Flujo

**Duración**: 70 minutos  
**Sesión**: 2 (Segunda Semana)

---

## 🎯 Objetivos del Módulo

Al finalizar este módulo, serás capaz de:

- ✅ Comprender qué son las compuertas y su función
- ✅ Usar correctamente la Compuerta Exclusiva (XOR)
- ✅ Aplicar la Compuerta Paralela (AND)
- ✅ Diferenciar y usar la Compuerta Inclusiva (OR)
- ✅ Implementar la Compuerta Basada en Eventos
- ✅ Elegir la compuerta correcta según el escenario

---

## 📋 Contenido

### 1. ¿Qué son las Compuertas? (10 min)

Las compuertas (Gateways) controlan el **flujo de decisión**, **divergencia** y **convergencia** en el proceso.

**Representación gráfica**: ◇ Rombo

**Funciones principales**:

1. **Dividir** el flujo (divergencia)
2. **Unir** el flujo (convergencia)
3. **Tomar decisiones** basadas en condiciones
4. **Sincronizar** múltiples caminos

**Regla de oro**: Si una compuerta **diverge** (abre caminos), debe **converger** (cerrarlos) con el mismo tipo de compuerta.

---

### 2. Compuerta Exclusiva (Exclusive Gateway) - XOR (15 min)

**Símbolo**: ◇ Rombo con **X** o rombo vacío

**Comportamiento**:

- **Divergencia**: Solo **UNO** de los caminos se toma
- **Convergencia**: Espera por **UNO** de los caminos entrantes

**Es la compuerta MÁS COMÚN** (80% de los casos)

#### Ejemplo de Divergencia

```
◇ "¿Monto > $1000?"
  ├─→ [Sí] → (Aprobación gerencial)
  └─→ [No] → (Aprobación automática)
```

**Explicación**:

- Solo un camino se ejecuta
- Si monto > $1000 → va por arriba
- Si monto <= $1000 → va por abajo
- Nunca ambos simultáneamente

#### Ejemplo de Convergencia

```
(Aprobación gerencial) ─┐
                         ├→ ◇ → (Continuar proceso)
(Aprobación automática) ─┘
```

**Explicación**:

- La compuerta espera a que llegue UNO de los caminos
- Cuando uno llega, continúa inmediatamente
- No espera al otro camino

#### Reglas Importantes

1. **Cada flujo saliente debe tener una condición** (excepto el flujo por defecto)
2. **Las condiciones deben ser mutuamente excluyentes** (solo una puede ser verdadera)
3. **Debe haber un flujo por defecto** para casos no contemplados
4. **Usar para decisiones binarias o múltiples opciones excluyentes**

#### Ejemplo Completo

```
⭕ [Inicio: Solicitud de crédito]
   ↓
📋 (Evaluar historial crediticio)
   ↓
◇ "Calificación crediticia"
  ├─→ [Excelente] → (Aprobar automáticamente) ─┐
  ├─→ [Buena] → (Revisar manualmente) ────────┤
  ├─→ [Regular] → (Solicitar garantía) ────────┤
  └─→/ [Mala] → (Rechazar) ─────────────────────┤
                                                 ↓
                                            ◇ (Convergencia)
                                                 ↓
                                          ⚫ [Fin]
```

---

### 3. Compuerta Paralela (Parallel Gateway) - AND (15 min)

**Símbolo**: ◇ Rombo con **+**

**Comportamiento**:

- **Divergencia**: **TODOS** los caminos se ejecutan simultáneamente
- **Convergencia**: Espera a **TODOS** los caminos entrantes

**Uso**: Tareas independientes que pueden hacerse al mismo tiempo

#### Ejemplo de Divergencia

```
◇ (+) Split Paralelo
  ├─→ (Preparar documentos legales)
  ├─→ (Solicitar avalúo)
  └─→ (Verificar referencias)
```

**Explicación**:

- Las 3 actividades se ejecutan **simultáneamente**
- No hay decisión, no hay condiciones
- Es automático

#### Ejemplo de Convergencia

```
(Preparar documentos) ─┐
(Solicitar avalúo) ────┤─→ ◇ (+) → (Continuar)
(Verificar referencias)─┘
```

**Explicación**:

- La compuerta espera a que **TODAS** las tareas terminen
- Solo cuando las 3 están listas, continúa
- Es una **sincronización**

#### Ejemplo Completo: Onboarding de Empleado

```
⭕ [Inicio: Empleado contratado]
   ↓
◇ (+) Iniciar procesos paralelos
  ├─→ (Crear cuenta de email) ──────────┐
  ├─→ (Solicitar equipo de cómputo) ────┤
  ├─→ (Programar inducción) ────────────┤
  └─→ (Dar de alta en nómina) ──────────┤
                                          ↓
                                   ◇ (+) Sincronizar
                                          ↓
                               (Notificar gerente)
                                          ↓
                                    ⚫ [Fin]
```

#### Cuándo Usar Compuerta Paralela

✅ **Usar cuando**:

- Las tareas son independientes entre sí
- Pueden ejecutarse al mismo tiempo
- Quieres reducir el tiempo total del proceso
- No hay decisiones, solo división de trabajo

❌ **NO usar cuando**:

- Hay que tomar una decisión
- Solo un camino debe ejecutarse
- Los caminos dependen uno del otro

---

### 4. Compuerta Inclusiva (Inclusive Gateway) - OR (15 min)

**Símbolo**: ◇ Rombo con **O** (círculo)

**Comportamiento**:

- **Divergencia**: **UNO o MÁS** caminos se ejecutan según condiciones
- **Convergencia**: Espera solo a los caminos que fueron activados

**Uso**: Cuando múltiples opciones pueden ser verdaderas simultáneamente

#### Ejemplo de Divergencia

```
◇ (O) "¿Qué notificaciones enviar?"
  ├─→ [Cliente VIP] → (Enviar SMS)
  ├─→ [Tiene email] → (Enviar correo)
  └─→ [App instalada] → (Notificación push)
```

**Explicación**:

- Si es VIP Y tiene email → se ejecutan AMBOS caminos
- Si solo tiene email → solo correo
- Si cumple las 3 condiciones → las 3 se ejecutan
- Si no cumple ninguna → ninguno se ejecuta (opcional: flujo por defecto)

#### Ejemplo de Convergencia

```
(Enviar SMS) ──────────┐
(Enviar correo) ───────┤─→ ◇ (O) → (Continuar)
(Notificación push) ───┘
```

**Explicación**:

- La compuerta espera solo los caminos que fueron activados
- Si se activaron 2 → espera 2
- Si se activó 1 → espera 1
- Es una sincronización inteligente

#### Diferencia con Exclusiva y Paralela

| Compuerta           | Caminos Activados           | Ejemplo                               |
| ------------------- | --------------------------- | ------------------------------------- |
| **Exclusiva (XOR)** | Solo UNO                    | ¿Aprobado? Sí o No                    |
| **Paralela (AND)**  | TODOS siempre               | Hacer A, B y C                        |
| **Inclusiva (OR)**  | UNO o MÁS según condiciones | Cliente importante → SMS, Email, Push |

#### Ejemplo Completo: Validaciones de Seguridad

```
⭕ [Inicio: Usuario intenta login]
   ↓
(Ingresar credenciales)
   ↓
◇ (O) "Validaciones requeridas"
  ├─→ [Primer login] → (Cambiar contraseña) ─────┐
  ├─→ [Desde nuevo dispositivo] → (Enviar código 2FA) ─┤
  └─→ [IP sospechosa] → (Verificar identidad) ────┤
                                                    ↓
                                            ◇ (O) Convergencia
                                                    ↓
                                             (Permitir acceso)
                                                    ↓
                                                 ⚫ [Fin]
```

---

### 5. Compuerta Basada en Eventos (Event-Based Gateway) (10 min)

**Símbolo**: ◇ Rombo con pentágono ⬠ o doble círculo

**Comportamiento**:

- Espera por el **primer evento** que ocurra
- Solo puede ir seguida de **eventos** o **tareas de recepción**
- Cuando un evento ocurre, los otros caminos se **cancelan**

**Uso**: Escenarios con timeouts, múltiples respuestas posibles, o carreras de eventos

#### Ejemplo: Espera de Respuesta con Timeout

```
(Enviar cotización al cliente)
   ↓
⬠ Event Gateway
  ├─→ ✉️⭕⭕ [Cliente responde] → (Procesar respuesta)
  ├─→ ⏰⭕⭕ [Timeout 48 horas] → (Hacer seguimiento)
  └─→ ✉️⭕⭕ [Cliente cancela] → (Cerrar caso)
```

**Explicación**:

- El proceso espera
- Si el cliente responde primero → procesa respuesta (otros se cancelan)
- Si pasan 48h primero → hace seguimiento (otros se cancelan)
- Si cancela primero → cierra caso (otros se cancelan)

#### Ejemplo 2: Aprobación con Múltiples Aprobadores

```
(Solicitar aprobación a 3 gerentes)
   ↓
⬠ Event Gateway
  ├─→ ✉️⭕⭕ [Gerente A aprueba] → (Continuar)
  ├─→ ✉️⭕⭕ [Gerente B aprueba] → (Continuar)
  └─→ ✉️⭕⭕ [Gerente C aprueba] → (Continuar)
```

**Explicación**:

- Solo necesita la aprobación del primero que responda
- Cuando uno aprueba, ya no espera a los otros

---

### 6. Tabla Comparativa de Compuertas (5 min)

| Compuerta             | Símbolo | Divergencia        | Convergencia         | Uso Principal        |
| --------------------- | ------- | ------------------ | -------------------- | -------------------- |
| **Exclusiva (XOR)**   | ◇ X     | Solo UN camino     | Espera UNO           | Decisión if/else     |
| **Paralela (AND)**    | ◇ +     | TODOS los caminos  | Espera TODOS         | Tareas simultáneas   |
| **Inclusiva (OR)**    | ◇ O     | Uno o MÁS caminos  | Espera los activados | Decisión OR múltiple |
| **Basada en Eventos** | ◇ ⬠     | Primer evento gana | N/A                  | Timeout, carreras    |

---

## 🧩 Ejercicios Prácticos (10 min)

### Ejercicio 1: Identificar la Compuerta Correcta

Para cada escenario, indica qué compuerta usar:

1. **Escenario**: El cliente puede pagar con tarjeta de crédito, débito o transferencia. Solo puede elegir una.
2. **Escenario**: Al iniciar el proyecto, debemos contratar personal, comprar equipos y preparar oficinas simultáneamente.

3. **Escenario**: Si el pedido es urgente, enviamos por mensajería express. Si el cliente es VIP, enviamos con seguro. Si ambas condiciones se cumplen, hacemos ambas cosas.

4. **Escenario**: Enviamos recordatorio al cliente. Si responde en 24 horas, procesamos. Si no responde, escalamos a supervisor.

<details>
<summary>Ver respuestas</summary>

1. **Compuerta Exclusiva (XOR)** - Solo una forma de pago
2. **Compuerta Paralela (AND)** - Todas las tareas en paralelo
3. **Compuerta Inclusiva (OR)** - Una o ambas opciones
4. **Compuerta Basada en Eventos** - Espera respuesta o timeout

</details>

---

### Ejercicio 2: Diseñar Proceso con Compuertas

**Escenario**: "Aprobación de Presupuesto"

1. Empleado solicita aprobación de presupuesto
2. Si monto < $500 → Aprobación automática → Fin
3. Si monto entre $500 y $5000 → Aprobación jefe → Fin
4. Si monto > $5000 → Aprobación jefe Y gerente (paralelo) → Ambos deben aprobar → Fin

**Tarea**: Dibuja el diagrama usando las compuertas apropiadas.

<details>
<summary>Ver diagrama sugerido</summary>

```
⭕ [Inicio: Solicitud de presupuesto]
   ↓
(Ingresar monto)
   ↓
◇ "Clasificar por monto"
  ├─→ [< $500] → (Aprobación automática) ────────────┐
  ├─→ [$500-$5000] → (Aprobación jefe) ──────────────┤
  └─→ [> $5000] → ◇ (+) Dividir                      │
                    ├─→ (Aprobación jefe) ──────┐    │
                    └─→ (Aprobación gerente) ───┤    │
                                                  ↓    │
                                            ◇ (+) Sincronizar
                                                  │
                                                  ├────┘
                                                  ↓
                                            ◇ Converger
                                                  ↓
                                          (Registrar decisión)
                                                  ↓
                                             ⚫ [Fin]
```

</details>

---

## 🎯 Errores Comunes y Cómo Evitarlos

### Error 1: Mezclar Tipos de Compuertas

❌ **Incorrecto**:

```
◇ (XOR) Divergencia
  ├─→ Camino A ─┐
  └─→ Camino B ─┤
                 ↓
           ◇ (AND) Convergencia ← ERROR
```

✅ **Correcto**:

```
◇ (XOR) Divergencia
  ├─→ Camino A ─┐
  └─→ Camino B ─┤
                 ↓
           ◇ (XOR) Convergencia ← Mismo tipo
```

### Error 2: Olvidar Converger

❌ **Incorrecto**:

```
◇ (+) Divergencia
  ├─→ (Tarea A) → (Siguiente actividad)
  └─→ (Tarea B) → (Otra actividad diferente)
```

**Problema**: Flujos se pierden

✅ **Correcto**:

```
◇ (+) Divergencia
  ├─→ (Tarea A) ─┐
  └─→ (Tarea B) ─┤
                  ↓
            ◇ (+) Convergencia
                  ↓
          (Continuar proceso)
```

### Error 3: Usar Paralela para Decisiones

❌ **Incorrecto**:

```
◇ (+) "¿Aprobado?"
  ├─→ Sí → (Procesar)
  └─→ No → (Rechazar)
```

**Problema**: Se ejecutarían AMBOS caminos

✅ **Correcto**:

```
◇ (XOR) "¿Aprobado?"
  ├─→ Sí → (Procesar)
  └─→ No → (Rechazar)
```

---

## 🔑 Conceptos Clave

| Compuerta   | Cuándo Usar        | Ejemplo Mental         |
| ----------- | ------------------ | ---------------------- |
| **XOR**     | Decisión           | Tenedor en el camino   |
| **AND**     | Sincronización     | Trabajo en equipo      |
| **OR**      | Opciones múltiples | Buffet (elige 1 o más) |
| **Eventos** | Carreras/Timeouts  | El primero que llegue  |

---

## ⏭️ Siguiente Módulo

**Módulo 2.2: Pools, Lanes y Colaboración**

Aprenderás:

- Qué son Pools y Lanes
- Cómo organizar procesos por roles
- Colaboración entre participantes
- Mensajes entre procesos

---

_Última actualización: Noviembre 2025_
