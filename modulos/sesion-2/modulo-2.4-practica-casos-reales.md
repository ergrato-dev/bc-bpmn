# Módulo 2.4: Práctica Integrada, Casos Reales y Mejores Prácticas

**Duración**: 70 minutos  
**Sesión**: 2 (Segunda Semana)

---

## 🎯 Objetivos del Módulo

Al finalizar este módulo, serás capaz de:

- ✅ Aplicar todos los elementos BPMN aprendidos en casos reales
- ✅ Identificar y corregir errores comunes en diagramas
- ✅ Aplicar mejores prácticas de modelado
- ✅ Usar herramientas de modelado BPMN
- ✅ Diseñar procesos completos end-to-end
- ✅ Validar la correctitud de tus diagramas

---

## 📋 Contenido

### 1. Repaso Rápido de Elementos BPMN (5 min)

#### Elementos Básicos

| Categoría       | Elementos                          | Uso                    |
| --------------- | ---------------------------------- | ---------------------- |
| **Eventos**     | Inicio ⭕, Intermedio ⭕⭕, Fin ⚫ | Algo que sucede        |
| **Actividades** | Tarea 📋, Subproceso 📋[+]         | Trabajo que se realiza |
| **Compuertas**  | XOR ◇, AND ◇+, OR ◇O, Eventos ◇⬠   | Control de flujo       |
| **Flujos**      | Secuencia →, Mensaje ✉️--→         | Conexiones             |
| **Swimlanes**   | Pools, Lanes                       | Organización por roles |

#### Elementos Avanzados

| Elemento            | Símbolo     | Uso Principal   |
| ------------------- | ----------- | --------------- |
| **Boundary Events** | ⭕ en borde | Excepciones     |
| **Error**           | ⚡          | Fallas          |
| **Timer**           | ⏰          | Timeouts        |
| **Escalation**      | ⬆️          | Avisar superior |
| **Compensation**    | ↩️          | Revertir        |
| **Message**         | ✉️          | Comunicación    |

---

### 2. Caso de Estudio 1: Proceso de Compra Online (15 min)

#### Descripción del Proceso

**Contexto**: E-commerce que vende productos físicos.

**Participantes**:

- Cliente
- Sistema de Tienda
- Pasarela de Pago
- Logística

**Flujo del proceso**:

1. Cliente busca y selecciona productos
2. Agrega al carrito
3. Procede al checkout
4. Ingresa datos de envío
5. Selecciona método de pago
6. Sistema valida stock
7. Si hay stock → Procesa pago
8. Si pago exitoso → Crea orden
9. Notifica a logística
10. Prepara y envía producto
11. Cliente recibe y confirma

**Excepciones a manejar**:

- Sin stock disponible
- Pago rechazado
- Timeout en procesamiento (>5 min)
- Cliente cancela durante el proceso
- Error en sistema de logística

#### Diagrama Completo

```
┌─ Pool: Cliente ──────────────────────────────────────────────┐
│                                                               │
│ [Inicio] → (Buscar productos)                                │
│               ↓                                               │
│          (Agregar al carrito)                                │
│               ↓                                               │
│          (Proceder al checkout)                              │
│               ↓                                               │
│          (Ingresar datos envío)                              │
│               ↓                                               │
│          (Seleccionar método pago)                           │
│               ↓                                               │
│          (Confirmar orden) ─── ✉️ Orden ──→                 │
│               ↓                                               │
│          ✉️⭕⭕ Esperar confirmación                          │
│               ↓                                               │
│          ◇ ¿Confirmada?                                      │
│           ├─ Sí → (Realizar pago) ─── ✉️ ──→                │
│           │          ↓                                        │
│           │   ✉️⭕⭕ Esperar resultado pago                   │
│           │          ↓                                        │
│           │   ◇ ¿Pago exitoso?                               │
│           │    ├─ Sí → ✉️⭕⭕ Esperar tracking              │
│           │    │          ↓                                  │
│           │    │   (Recibir producto)                        │
│           │    │          ↓                                  │
│           │    │   ⚫ [Fin: Compra exitosa]                  │
│           │    │                                             │
│           │    └─ No → (Ver error)                          │
│           │              ↓                                   │
│           │         ⚫ [Fin: Pago fallido]                   │
│           │                                                  │
│           └─ No → ⚫ [Fin: Sin stock]                        │
│                                                               │
└───────────────────────────────────────────────────────────────┘
                           ↓ ↑ Mensajes
┌─ Pool: Sistema Tienda ───────────────────────────────────────┐
│                                                               │
│ ✉️⭕ Inicio: Orden recibida                                  │
│          ↓                                                    │
│     (Validar orden)                                          │
│          ↓                                                    │
│     (Verificar stock)                                        │
│          │⭕⏰ 5 min (timeout)                                │
│          │    ↓                                              │
│          │ (Cancelar por timeout)                            │
│          │    ↓                                              │
│          │ ✉️⚫ Notificar timeout                             │
│          │    ↓                                              │
│          │ ⚫ [Fin: Timeout]                                  │
│          ↓                                                    │
│     ◇ ¿Hay stock?                                           │
│      ├─ Sí → (Reservar stock)                                │
│      │          ↓                                            │
│      │     (Confirmar disponibilidad) ─── ✉️ ──→            │
│      │          ↓                                            │
│      │     (Crear orden preliminar)                          │
│      │          ↓                                            │
│      │     (Enviar a pasarela) ─── ✉️ ──→                   │
│      │          ↓                                            │
│      │     ✉️⭕⭕ Esperar resultado pago                      │
│      │          ↓                                            │
│      │     ◇ ¿Pago OK?                                       │
│      │      ├─ Sí → (Confirmar orden)                        │
│      │      │          ↓                                     │
│      │      │     (Notificar logística) ─── ✉️ ──→          │
│      │      │          ↓                                     │
│      │      │     ✉️⭕⭕ Esperar confirmación envío           │
│      │      │          ↓                                     │
│      │      │     (Enviar tracking) ─── ✉️ ──→              │
│      │      │          ↓                                     │
│      │      │     ⚫ [Fin: Orden procesada]                  │
│      │      │                                                │
│      │      └─ No → (Liberar stock)                          │
│      │                  ↓                                    │
│      │            (Notificar fallo) ─── ✉️ ──→              │
│      │                  ↓                                    │
│      │            ⚫ [Fin: Pago rechazado]                   │
│      │                                                       │
│      └─ No → (Notificar sin stock) ─── ✉️ ──→              │
│                  ↓                                           │
│             ⚫ [Fin: Sin stock]                               │
└───────────────────────────────────────────────────────────────┘
                           ↓ Mensajes
┌─ Pool: Pasarela de Pago (Colapsada) ────────────────────────┐
│ ✉️⭕ Recibir solicitud pago                                  │
│          ↓                                                    │
│     (Procesar pago)                                          │
│          │⭕⚡ Error de pago                                  │
│          │    ↓                                              │
│          │ ✉️⚫ Notificar error                               │
│          ↓                                                    │
│     ✉️⚫ Enviar resultado                                     │
│          ↓                                                    │
│     ⚫ [Fin]                                                  │
└───────────────────────────────────────────────────────────────┘
                           ↓ Mensajes
┌─ Pool: Logística ────────────────────────────────────────────┐
│ ✉️⭕ Inicio: Orden a despachar                               │
│          ↓                                                    │
│     (Preparar paquete)                                       │
│          ↓                                                    │
│     (Coordinar envío)                                        │
│          │⭕⚡ Error logístico                                │
│          │    ↓                                              │
│          │ (Notificar tienda) ─── ✉️ ──→                    │
│          │    ↓                                              │
│          │ ⚫⚡ [Fin: Error]                                  │
│          ↓                                                    │
│     (Confirmar despacho) ─── ✉️ ──→                         │
│          ↓                                                    │
│     (Entregar producto)                                      │
│          ↓                                                    │
│     ⚫ [Fin: Entregado]                                       │
└───────────────────────────────────────────────────────────────┘
```

#### Análisis del Caso

**Elementos usados**:

- ✅ 4 Pools (participantes diferentes)
- ✅ Mensajes entre Pools
- ✅ Compuertas exclusivas (decisiones)
- ✅ Boundary Events (Timer, Error)
- ✅ Múltiples eventos de fin (diferentes resultados)
- ✅ Eventos intermedios de mensaje (esperas)

**Mejores prácticas aplicadas**:

- ✅ Manejo de excepciones (timeouts, errores)
- ✅ Flujos alternativos claros
- ✅ Nombres descriptivos
- ✅ Organización por participante

---

### 3. Caso de Estudio 2: Aprobación de Crédito Bancario (12 min)

#### Descripción del Proceso

**Contexto**: Banco que evalúa solicitudes de crédito personal.

**Participantes**:

- Cliente (Solicitante)
- Banco:
  - Oficial de Crédito
  - Sistema Automático
  - Comité de Crédito
  - Legal

**Reglas de negocio**:

- Montos < $5,000: Aprobación automática (si score >700)
- Montos $5,000-$50,000: Requiere oficial de crédito
- Montos > $50,000: Requiere comité + legal
- Timeout: 72 horas para aprobar o rechazar
- Si score < 600: Rechazo automático

#### Diagrama Simplificado con Lanes

```
┌─ Pool: Cliente ──────────────────────────────────────────────┐
│ [Inicio] → (Solicitar crédito) ─── ✉️ Solicitud ──→         │
│                 ↓                                             │
│     ✉️⭕⭕ Esperar respuesta                                  │
│                 ↓                                             │
│     ◇ ¿Aprobado?                                             │
│      ├─ Sí → (Firmar contrato) → ⚫ [Fin: Aprobado]          │
│      └─ No → ⚫ [Fin: Rechazado]                             │
└───────────────────────────────────────────────────────────────┘
                           ↓ ↑ Mensajes
┌─ Pool: Banco ────────────────────────────────────────────────┐
│                                                               │
│ ┌─ Lane: Sistema Automático ─────────────────────────────┐  │
│ │ ✉️⭕ Recibir solicitud                                   │  │
│ │         ↓                                                │  │
│ │    (Consultar buró de crédito)                           │  │
│ │         ↓                                                │  │
│ │    (Calcular score)                                      │  │
│ │         ↓                                                │  │
│ │    ◇ Score < 600?                                        │  │
│ │     ├─ Sí → (Rechazar auto) ─── ✉️ ──→                  │  │
│ │     │          ↓                                         │  │
│ │     │     ⚫ [Fin: Auto-rechazo]                         │  │
│ │     └─ No → (Clasificar por monto) ──────┐              │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                              ↓                │
│ ┌─ Lane: Oficial de Crédito ─────────────────────────────┐  │
│ │         ◇ Clasificación                                 │  │
│ │          ├─ < $5K y score > 700 → (Aprobar auto)        │  │
│ │          │                           ↓                   │  │
│ │          │                    (Generar contrato)         │  │
│ │          │                           ↓                   │  │
│ │          │                    ✉️⚫ Notificar              │  │
│ │          │                           ↓                   │  │
│ │          │                    ⚫ [Fin]                    │  │
│ │          │                                               │  │
│ │          ├─ $5K-$50K → (Revisar solicitud)              │  │
│ │          │     │⭕⏰ 72h timeout                          │  │
│ │          │     │    ↓                                    │  │
│ │          │     │ (Rechazar por timeout)                  │  │
│ │          │     │    ↓                                    │  │
│ │          │     │ ✉️⚫ Notificar                           │  │
│ │          │     ↓                                         │  │
│ │          │ (Evaluar capacidad pago)                      │  │
│ │          │     ↓                                         │  │
│ │          │ (Evaluar historial)                           │  │
│ │          │     ↓                                         │  │
│ │          │ ◇ ¿Aprobar?                                   │  │
│ │          │  ├─ Sí → (Aprobar) ─── ✉️ ──→                │  │
│ │          │  └─ No → (Rechazar) ─── ✉️ ──→               │  │
│ │          │                                               │  │
│ │          └─ > $50K → (Escalar a comité) ─────┐          │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                              ↓                │
│ ┌─ Lane: Comité de Crédito ──────────────────────────────┐  │
│ │         (Revisar caso)                                  │  │
│ │               │⭕⬆️ Escalar a gerencia (si es complejo) │  │
│ │               │    (no interrumpe)                       │  │
│ │               ↓                                         │  │
│ │         ◇ (+) Votación paralela                         │  │
│ │          ├─→ (Miembro 1 vota) ─┐                       │  │
│ │          ├─→ (Miembro 2 vota) ─┤                       │  │
│ │          └─→ (Miembro 3 vota) ─┤                       │  │
│ │                                 ↓                       │  │
│ │                           ◇ (+) Sincronizar             │  │
│ │                                 ↓                       │  │
│ │                           (Contar votos)                │  │
│ │                                 ↓                       │  │
│ │                           ◇ ¿Mayoría aprueba?           │  │
│ │                            ├─ Sí → (Pasar a Legal) ──┐  │  │
│ │                            └─ No → (Rechazar) ─────┐  │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                              ↓         ↓      │
│ ┌─ Lane: Legal ───────────────────────────────────────────┐  │
│ │                 (Revisar aspectos legales)              │  │
│ │                         ↓                               │  │
│ │                 (Aprobar términos)                      │  │
│ │                         ↓                               │  │
│ │                 (Generar contrato)                      │  │
│ │                         ↓                               │  │
│ │                 ✉️⚫ Notificar aprobación                │  │
│ │                         ↓                               │  │
│ │                 ⚫ [Fin]                                 │  │
│ └─────────────────────────────────────────────────────────┘  │
│                                              ↓                │
│                         ✉️⚫ Notificar rechazo                │
│                                ↓                              │
│                         ⚫ [Fin]                               │
└───────────────────────────────────────────────────────────────┘
```

#### Análisis del Caso

**Complejidad agregada**:

- ✅ Lanes dentro de un Pool (organización interna)
- ✅ Compuerta paralela (votación del comité)
- ✅ Escalamiento no interrupting
- ✅ Timer boundary para timeout
- ✅ Múltiples niveles de decisión

**Lecciones aprendidas**:

- Los Lanes ayudan a mostrar responsabilidades claras
- Las compuertas paralelas son útiles para consensos
- Los timers evitan procesos eternos
- El escalamiento permite visibilidad sin bloquear

---

### 4. Errores Comunes y Cómo Evitarlos (10 min)

#### Error 1: Flujos Cruzando Pools Incorrectamente

❌ **Incorrecto**:

```
Pool A: (Actividad 1) ──→ Pool B: (Actividad 2)
                    (flecha sólida cruzando pools)
```

✅ **Correcto**:

```
Pool A: (Actividad 1) ──→ ✉️⚫ Enviar mensaje
                              ↓ (mensaje punteado)
Pool B: ✉️⭕⭕ Recibir mensaje ──→ (Actividad 2)
```

**Regla**: Flujos de secuencia NO cruzan Pools. Usa mensajes.

---

#### Error 2: Compuertas sin Convergencia

❌ **Incorrecto**:

```
◇ (+) Split
  ├─→ (Tarea A) → (Continúa...)
  └─→ (Tarea B) → (Otra cosa...)
```

**Problema**: Flujos se bifurcan sin sincronizar.

✅ **Correcto**:

```
◇ (+) Split
  ├─→ (Tarea A) ─┐
  └─→ (Tarea B) ─┤
                  ↓
            ◇ (+) Join
                  ↓
          (Continúa...)
```

---

#### Error 3: Uso Incorrecto de Compuertas

❌ **Incorrecto**:

```
◇ (+) "¿Aprobado?"
  ├─→ Sí → ...
  └─→ No → ...
```

**Problema**: Compuerta paralela usada para decisión.

✅ **Correcto**:

```
◇ (XOR) "¿Aprobado?"
  ├─→ Sí → ...
  └─→ No → ...
```

**Regla**:

- XOR para decisiones (uno u otro)
- AND para paralelismo (todos)
- OR para opciones múltiples

---

#### Error 4: Eventos de Inicio/Fin Mal Colocados

❌ **Incorrecto**:

```
(Actividad 1) → [Inicio] → (Actividad 2)
```

**Problema**: Evento de inicio en medio del proceso.

✅ **Correcto**:

```
[Inicio] → (Actividad 1) → (Actividad 2) → [Fin]
```

**Regla**:

- Inicio al principio (sin flujos entrantes)
- Fin al final (sin flujos salientes)

---

#### Error 5: Nombres Vagos o Genéricos

❌ **Evitar**:

- "Tarea 1"
- "Proceso"
- "Validación"
- "Revisar"

✅ **Usar**:

- "Validar datos del cliente"
- "Aprobar presupuesto"
- "Enviar correo de confirmación"
- "Calcular total con descuentos"

---

#### Error 6: Diagramas Demasiado Complejos

❌ **Síntomas**:

- Más de 20-30 actividades
- Flujos que cruzan el diagrama múltiples veces
- Imposible seguir el flujo visualmente
- Demasiadas compuertas anidadas

✅ **Solución**:

- Dividir en subprocesos
- Crear múltiples niveles (alto nivel → detalle)
- Usar subprocesos colapsados
- Simplificar lógica de decisión

---

#### Error 7: No Manejar Excepciones

❌ **Problema**:

```
[Inicio] → (Procesar pago) → (Enviar producto) → [Fin]
```

**¿Qué pasa si el pago falla?**

✅ **Solución**:

```
[Inicio] → (Procesar pago)
                │⭕⚡ Error pago
                │    ↓
                │ (Notificar error)
                │    ↓
                │ ⚫ [Fin: Pago fallido]
                ↓
           (Enviar producto) → ⚫ [Fin: Exitoso]
```

---

### 5. Mejores Prácticas de Modelado (8 min)

#### Principios Fundamentales

##### 1. Claridad sobre Completitud

**Principio**: Es mejor un diagrama simple y claro que uno complejo y confuso.

✅ **Hacer**:

- Ajustar el nivel de detalle a la audiencia
- Omitir detalles innecesarios
- Usar subprocesos colapsados cuando sea apropiado

❌ **Evitar**:

- Mostrar TODO en un solo diagrama
- Detalles técnicos en diagramas de alto nivel
- Complejidad innecesaria

---

##### 2. Consistencia

**Principio**: Mantén un estilo uniforme en todos tus diagramas.

✅ **Hacer**:

- Usar las mismas convenciones de nombres
- Misma orientación (horizontal preferentemente)
- Mismo nivel de detalle en actividades similares
- Colores consistentes (si se usan)

❌ **Evitar**:

- Mezclar estilos de nombrado
- Cambiar convenciones entre diagramas
- Inconsistencias visuales

---

##### 3. Validación

**Principio**: Verifica que tu diagrama sea lógicamente correcto.

**Checklist de validación**:

- [ ] ¿Hay al menos un evento de inicio?
- [ ] ¿Hay al menos un evento de fin?
- [ ] ¿Todas las actividades están conectadas?
- [ ] ¿Las compuertas que divergen, convergen?
- [ ] ¿Las condiciones son mutuamente excluyentes (XOR)?
- [ ] ¿Hay flujo por defecto en compuertas XOR?
- [ ] ¿Los mensajes cruzan solo entre Pools?
- [ ] ¿Todos los elementos tienen nombre?
- [ ] ¿El flujo tiene sentido lógico?

---

##### 4. Documentación Complementaria

**Principio**: BPMN muestra el "cómo", documenta el "por qué".

**Usa anotaciones para**:

- Explicar reglas de negocio complejas
- Aclarar excepciones o casos especiales
- Referenciar sistemas externos
- Notas para implementadores

**Ejemplo**:

```
(Calcular descuento)
    ↓
📝 Nota: Descuento =
    - VIP: 15%
    - Frecuente: 10%
    - Nuevo: 5%
    - Aplica sobre subtotal antes de IVA
```

---

##### 5. Orientación y Flujo Visual

**Principio**: El diagrama debe ser fácil de seguir visualmente.

✅ **Hacer**:

- Fluir de izquierda a derecha o arriba hacia abajo
- Mantener Pools horizontales
- Minimizar cruces de líneas
- Alinear elementos

❌ **Evitar**:

- Flujos que van hacia atrás
- Líneas que se cruzan múltiples veces
- Disposición caótica
- Elementos desalineados

---

### 6. Herramientas de Modelado BPMN (10 min)

#### Herramientas Recomendadas

##### 1. **Camunda Modeler** (Gratuito)

**Pros**:

- ✅ Gratuito y open source
- ✅ Específico para BPMN 2.0
- ✅ Validación en tiempo real
- ✅ Exporta XML ejecutable
- ✅ Genera código

**Cons**:

- ⚠️ Interfaz técnica
- ⚠️ Enfocado en procesos ejecutables

**Ideal para**: Desarrolladores, procesos automatizables

**Descarga**: https://camunda.com/download/modeler/

---

##### 2. **Bizagi Modeler** (Gratuito)

**Pros**:

- ✅ Gratuito
- ✅ Interfaz amigable
- ✅ Simulación de procesos
- ✅ Genera documentación automática
- ✅ Colaboración

**Cons**:

- ⚠️ Requiere registro
- ⚠️ Algunas funciones solo en versión paga

**Ideal para**: Analistas de negocio, consultores

**Descarga**: https://www.bizagi.com/modeler

---

##### 3. **Draw.io / diagrams.net** (Gratuito)

**Pros**:

- ✅ Completamente gratuito
- ✅ Online y offline
- ✅ Integración con Google Drive, GitHub
- ✅ Fácil de usar
- ✅ Múltiples formatos de exportación

**Cons**:

- ⚠️ No valida BPMN automáticamente
- ⚠️ No genera XML ejecutable
- ⚠️ Requiere conocer bien BPMN

**Ideal para**: Diagramas visuales, documentación

**URL**: https://app.diagrams.net/

---

##### 4. **Visual Paradigm Community** (Gratuito con limitaciones)

**Pros**:

- ✅ Versión Community gratuita
- ✅ Suite completa (UML, BPMN, ERD)
- ✅ Generación de código
- ✅ Ingeniería reversa

**Cons**:

- ⚠️ Interfaz compleja
- ⚠️ Curva de aprendizaje

**Ideal para**: Proyectos grandes, equipos

**Descarga**: https://www.visual-paradigm.com/

---

##### 5. **BPMN.io** (Online, Gratuito)

**Pros**:

- ✅ 100% online, no requiere instalación
- ✅ Gratuito
- ✅ Validación BPMN
- ✅ Open source

**Cons**:

- ⚠️ Funcionalidad básica
- ⚠️ Requiere conexión

**Ideal para**: Pruebas rápidas, aprendizaje

**URL**: https://demo.bpmn.io/

---

#### Tabla Comparativa

| Herramienta         | Precio   | Nivel      | Validación | Ejecutable | Simulación |
| ------------------- | -------- | ---------- | ---------- | ---------- | ---------- |
| **Camunda**         | Gratis   | Avanzado   | ✅         | ✅         | ❌         |
| **Bizagi**          | Gratis   | Intermedio | ✅         | Limitado   | ✅         |
| **Draw.io**         | Gratis   | Básico     | ❌         | ❌         | ❌         |
| **Visual Paradigm** | Freemium | Avanzado   | ✅         | ✅         | ✅         |
| **BPMN.io**         | Gratis   | Básico     | ✅         | ❌         | ❌         |

---

### 7. Proyecto Integrador Final (10 min)

#### Escenario: "Proceso de Onboarding de Empleados"

**Contexto**: Tu empresa necesita documentar y mejorar el proceso de incorporación de nuevos empleados.

**Requerimientos**:

**Participantes**:

- Nuevo Empleado
- Empresa (con departamentos: RRHH, IT, Finanzas, Jefe Directo)

**Proceso actual (as-is)**:

1. RRHH notifica la contratación
2. IT debe crear cuentas (email, accesos)
3. Finanzas debe dar de alta en nómina
4. Jefe debe programar inducción
5. Empleado firma documentos
6. Empleado recibe equipo
7. Empleado asiste a inducción
8. Proceso completa cuando todo está listo

**Reglas de negocio**:

- IT tiene 24h para crear cuentas
- Si tarda más → Escalar a supervisor IT
- Finanzas debe completar antes del primer día laboral
- Si falta firma de documentos → No puede iniciar
- El equipo puede entregarse el primer día o antes

**Tu tarea**:

1. **Diseña el diagrama BPMN** que incluya:

   - Pools y Lanes apropiados
   - Compuertas (al menos 2 tipos diferentes)
   - Boundary Events para el timeout de IT
   - Escalamiento si es necesario
   - Manejo de la excepción "documentos sin firmar"
   - Flujos de mensaje entre participantes

2. **Identifica**:

   - ¿Qué actividades pueden hacerse en paralelo?
   - ¿Cuáles son las dependencias críticas?
   - ¿Qué puntos de sincronización necesitas?

3. **Valida**:
   - ¿Todos los caminos llevan a un fin?
   - ¿Las compuertas están balanceadas?
   - ¿Los nombres son claros?

<details>
<summary>Ver solución sugerida (una de muchas posibles)</summary>

```
┌─ Pool: Nuevo Empleado ────────────────────────────────────┐
│ [Inicio: Contrato firmado]                                │
│         ↓                                                  │
│ ✉️⭕⭕ Esperar bienvenida de RRHH                          │
│         ↓                                                  │
│ (Firmar documentos legales) ─── ✉️ Docs ──→              │
│         ↓                                                  │
│ ✉️⭕⭕ Esperar confirmación equipo                         │
│         ↓                                                  │
│ (Recibir equipo)                                          │
│         ↓                                                  │
│ (Asistir a inducción)                                     │
│         ↓                                                  │
│ ⚫ [Fin: Onboarding completo]                              │
└────────────────────────────────────────────────────────────┘
                         ↓ ↑ Mensajes
┌─ Pool: Empresa ────────────────────────────────────────────┐
│                                                             │
│ ┌─ Lane: RRHH ─────────────────────────────────────────┐  │
│ │ [Inicio: Nueva contratación]                         │  │
│ │         ↓                                             │  │
│ │ (Enviar bienvenida) ─── ✉️ ──→                       │  │
│ │         ↓                                             │  │
│ │ ◇ (+) Iniciar procesos paralelos                     │  │
│ │  ├─→ (Notificar a IT) ──────────────────┐            │  │
│ │  ├─→ (Notificar a Finanzas) ────────────┤            │  │
│ │  └─→ (Notificar a Jefe) ────────────────┤            │  │
│ │                                          ↓            │  │
│ │ ✉️⭕⭕ Esperar docs firmados                          │  │
│ │         ↓                                             │  │
│ │ ◇ ¿Docs OK?                                          │  │
│ │  ├─ Sí → (Archivar) ───────────────┐                │  │
│ │  └─ No → (Solicitar corrección) ──┘                 │  │
│ │             (loop)                                   │  │
│ └──────────────────────────────────────────────────────┘  │
│                                          ↓                 │
│ ┌─ Lane: IT ──────────────────────────────────────────┐  │
│ │ (Crear cuenta email)                                │  │
│ │         │⭕⏰ 24h                                     │  │
│ │         │    ↓                                      │  │
│ │         │ ⚫⬆️ Escalar a Supervisor                  │  │
│ │         │    (no interrumpe)                         │  │
│ │         ↓                                            │  │
│ │ (Configurar accesos)                                │  │
│ │         ↓                                            │  │
│ │ (Preparar equipo)                                   │  │
│ │         ↓                                            │  │
│ │ (Notificar listo) ─── ✉️ ──→                        │  │
│ │         ↓                                            │  │
│ │ (Entregar equipo) ──────────────┐                   │  │
│ └──────────────────────────────────────────────────────┘  │
│                                   ↓                        │
│ ┌─ Lane: Finanzas ───────────────────────────────────┐  │
│ │ (Alta en sistema de nómina)                         │  │
│ │         ↓                                            │  │
│ │ (Configurar datos bancarios) ─────────┐             │  │
│ └──────────────────────────────────────────────────────┘  │
│                                   ↓                        │
│ ┌─ Lane: Jefe Directo ───────────────────────────────┐  │
│ │ (Programar inducción)                               │  │
│ │         ↓                                            │  │
│ │ (Preparar plan de trabajo) ────────────┐            │  │
│ └──────────────────────────────────────────────────────┘  │
│                                   ↓                        │
│         ◇ (+) Sincronizar todos                           │
│                  ↓                                         │
│         (Confirmar onboarding completo)                   │
│                  ↓                                         │
│         ⚫ [Fin]                                            │
└────────────────────────────────────────────────────────────┘
```

**Elementos aplicados**:

- 2 Pools (Empleado, Empresa)
- 4 Lanes en Empresa (roles diferentes)
- Compuerta paralela (iniciar procesos simultáneos)
- Compuerta paralela convergencia (sincronizar)
- Compuerta exclusiva (validar documentos)
- Timer boundary event (timeout IT)
- Escalation event (notificar supervisor)
- Mensajes entre Pools

</details>

---

## 🎓 Cierre del Curso

### Lo que has aprendido

#### Sesión 1:

- ✅ Fundamentos de BPMN
- ✅ Eventos, Actividades, Flujos
- ✅ Elementos básicos de notación

#### Sesión 2:

- ✅ Compuertas y control de flujo
- ✅ Pools, Lanes y colaboración
- ✅ Elementos avanzados y excepciones
- ✅ Casos reales y mejores prácticas

### Próximos Pasos

1. **Practica**: Modela procesos de tu trabajo
2. **Herramientas**: Descarga y prueba herramientas
3. **Comunidad**: Únete a foros y grupos de BPM
4. **Profundiza**: Lee especificación BPMN 2.0
5. **Certifícate**: Considera certificaciones BPMN

### Recursos Adicionales

**Sitios oficiales**:

- OMG BPMN: https://www.omg.org/spec/BPMN/
- BPMN.org: https://www.bpmn.org/

**Comunidades**:

- BPM Forum (LinkedIn)
- Camunda Community
- Stack Overflow [bpmn] tag

**Libros recomendados**:

- "BPMN Method and Style" - Bruce Silver
- "Real-Life BPMN" - Jakob Freund & Bernd Rücker
- "BPMN 2.0 Handbook" - Varios autores

---

## 🎯 Evaluación Final

¿Te sientes capaz de:

- [ ] Explicar qué es BPMN y para qué sirve
- [ ] Identificar todos los elementos básicos
- [ ] Crear diagramas de procesos simples
- [ ] Usar compuertas correctamente
- [ ] Organizar procesos con Pools y Lanes
- [ ] Manejar excepciones con Boundary Events
- [ ] Aplicar mejores prácticas
- [ ] Usar al menos una herramienta de modelado

**Si marcaste 6 o más: ¡Felicitaciones! Has completado exitosamente el curso.**

---

## 📜 Certificado de Finalización

Has completado el **Curso Intensivo de BPMN 2.0**:

- 8 horas de capacitación
- 4 módulos completados
- Casos prácticos resueltos
- Proyecto integrador finalizado

**¡Continúa practicando y modelando procesos!**

---

_Última actualización: Noviembre 2025_

**Fin del Curso BPMN** 🎓
