# Índice de Assets Visuales BPMN

Este directorio contiene todos los recursos visuales en formato SVG para el curso de BPMN.

## 📁 Estructura

```
assets/
├── banner/
│   └── bpmn-banner.svg                    # Banner principal del README
├── diagramas/                              # Diagramas conceptuales
│   ├── eventos-basicos.svg
│   ├── actividades-basicas.svg
│   ├── compuertas-comparacion.svg
│   ├── pools-lanes-ejemplo.svg
│   └── boundary-events-completo.svg
├── ejemplos/                               # Casos de uso completos
│   └── proceso-compra-ecommerce.svg
└── referencias/                            # Guías de referencia
    └── bpmn-referencia-completa.svg
```

## 🎨 Diagramas Disponibles

### Diagramas Conceptuales (`/diagramas`)

| Archivo                        | Descripción                               | Usado en Módulo |
| ------------------------------ | ----------------------------------------- | --------------- |
| `eventos-basicos.svg`          | Eventos de inicio, intermedio y fin       | 1.2             |
| `actividades-basicas.svg`      | Tareas, subprocesos y multi-instancia     | 1.2             |
| `compuertas-comparacion.svg`   | Comparación de XOR, AND, OR y Event-Based | 2.1             |
| `pools-lanes-ejemplo.svg`      | Pools, lanes y mensajes con ejemplo       | 2.2             |
| `boundary-events-completo.svg` | Todos los tipos de boundary events        | 2.3             |

### Ejemplos Completos (`/ejemplos`)

| Archivo                        | Descripción                                         | Usado en Módulo |
| ------------------------------ | --------------------------------------------------- | --------------- |
| `proceso-compra-ecommerce.svg` | Caso completo con 4 pools, decisiones y excepciones | 2.4             |

### Referencias (`/referencias`)

| Archivo                        | Descripción                                       | Usado en Módulo |
| ------------------------------ | ------------------------------------------------- | --------------- |
| `bpmn-referencia-completa.svg` | Referencia rápida de todos los elementos BPMN 2.0 | Todos           |

## 🎯 Características de los SVGs

Todos los SVGs siguen el mismo estilo visual:

- **Tema oscuro**: Fondo `#1a1a2e`
- **Color de acento**: `#00d4ff` (cyan)
- **Tipografía**: Inter, sans-serif
- **Sin gradientes**: Diseño plano y moderno
- **Vectorial**: Escalable sin pérdida de calidad
- **Accesible**: Textos legibles, alto contraste

## 📖 Cómo Usar en los Módulos

### Markdown

```markdown
![Eventos Básicos](../../assets/diagramas/eventos-basicos.svg)
```

### HTML

```html
<img
  src="../../assets/diagramas/eventos-basicos.svg"
  alt="Eventos Básicos BPMN"
  width="100%" />
```

### Visualizar directamente

Los SVG se pueden abrir directamente en cualquier navegador moderno.

## 🔄 Reemplazar ASCII Art

Los diagramas SVG reemplazan los bloques de ASCII art en los módulos. Para actualizar:

1. Localizar el bloque de código ASCII en el módulo
2. Reemplazarlo con la referencia al SVG correspondiente
3. Opcionalmente, mantener una versión simplificada en texto para accesibilidad

**Ejemplo de reemplazo:**

❌ **Antes (ASCII):**

```
[Inicio] → (Actividad) → [Fin]
```

✅ **Después (SVG):**

```markdown
![Flujo básico](../../assets/diagramas/eventos-basicos.svg)

_Flujo simple: Evento Inicio → Actividad → Evento Fin_
```

## 📊 Mapeo de SVGs a Contenido

### Módulo 1.1 - Introducción

- Puede usar: `bpmn-referencia-completa.svg` (overview general)

### Módulo 1.2 - Elementos Básicos

- `eventos-basicos.svg` - Para explicar eventos
- `actividades-basicas.svg` - Para explicar actividades
- `bpmn-referencia-completa.svg` - Como referencia

### Módulo 2.1 - Compuertas

- `compuertas-comparacion.svg` - Comparación de todos los tipos

### Módulo 2.2 - Pools y Lanes

- `pools-lanes-ejemplo.svg` - Ejemplo completo con colaboración

### Módulo 2.3 - Elementos Avanzados

- `boundary-events-completo.svg` - Todos los boundary events

### Módulo 2.4 - Práctica

- `proceso-compra-ecommerce.svg` - Caso de estudio completo
- `bpmn-referencia-completa.svg` - Referencia para validación

## 🛠️ Creación de Nuevos SVGs

Si necesitas crear más SVGs, sigue estas pautas:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 800">
  <defs>
    <style>
      .bg { fill: #1a1a2e; }
      .title { fill: #ffffff; font-family: 'Inter', sans-serif; }
      .accent { stroke: #00d4ff; }
    </style>
  </defs>

  <rect class="bg" width="1200" height="800" rx="10"/>
  <!-- Tu contenido aquí -->
</svg>
```

### Paleta de Colores

| Uso              | Color        | Hex       |
| ---------------- | ------------ | --------- |
| Fondo principal  | Muy oscuro   | `#1a1a2e` |
| Fondo secundario | Oscuro       | `#1f1f2e` |
| Elementos        | Oscuro claro | `#2a2a3e` |
| Texto principal  | Blanco       | `#ffffff` |
| Texto secundario | Gris         | `#b0b0b0` |
| Acento principal | Cyan         | `#00d4ff` |
| Mensajes         | Rojo claro   | `#ff6b6b` |
| Errores          | Rojo         | `#ff6b6b` |

## 📝 Licencia

Estos assets son parte del curso BPMN y están sujetos a la misma licencia del repositorio.

---

_Última actualización: Noviembre 2025_
