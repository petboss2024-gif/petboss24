# Design System en Bubble

Cómo se representa la marca en la app de Bubble (`valecans-home-89169`,
Main/test): variables, estilos, estados, excepciones y límites. Los **valores**
de la marca se definen en [`tokens.json`](tokens.json) y las **reglas** en
[`README.md`](README.md); este archivo solo dice dónde y cómo vive cada uno en
Bubble y qué falta. Las decisiones y su estado están en
[`decisions.md`](decisions.md).

Revisado contra Bubble el 2026-10-12. Savepoint antes de los cambios:
`1791841900553`.

## Compatibilidad

- **Exacta:** Bubble guarda el mismo valor y se comprobó leyendo la variable o el estilo.
- **Aproximada:** se ve casi igual, pero Bubble lo guarda de otra forma; se explica la diferencia.
- **Sin verificar:** Bubble usa una variable que no se puede leer desde la conexión (ver Límites).
- **No soportada:** Bubble no tiene una propiedad equivalente.

## Variables de color

| Token (`tokens.json`) | Valor | Variable en Bubble | Compatibilidad |
|---|---|---|---|
| `orange` | `#FF6600` | Primary (incorporada) | Sin verificar |
| `violet` | `#9C2CF3` | `cmVqG` "Purple - Secondary - 100%" | Exacta (ningún estilo la usa todavía) |
| `white` / texto oscuro | `#FFFFFF` | Text y Primary contrast (incorporadas) | Sin verificar |
| `app-bg` | `#161616` | Background (incorporada) | Sin verificar. También existe `cmmAL` "Gray - DarkFonts - 100%" = `#161616` (ningún estilo la usa) |
| `app-surface` | `#2C2C2C` | Surface (incorporada) | Sin verificar |
| `text-secondary` (oscuro) | `#B3B3B3` | `cnBrT` "Gray - Text Secondary - 100%" | Exacta (creada 2026-10-12) |
| `button.primary.pressed` | `#A84400` | `cnBrU` "Orange - Pressed - 100%" | Exacta (creada 2026-10-12) |
| `button.secondary.dark.pressed` | `#272727` | — | Pendiente (DS-008) |
| `gray-dark`, `gray-light`, modo claro | `#404040`, `#E5E5E5`, `#6B6B6B` | — | No se crean hasta que la app tenga modo claro |

Variables de Bubble **fuera del Design System** (siguen en uso):

| Variable | Valor | Usada en | Estado |
|---|---|---|---|
| `cmyip` "Blue - Text Link - 100%" | `#007AFF` | `mobileSecondary` (fondo), `mobileVerify` (fondo), `link`, `body info`, icono `blue` | Pendiente: DS-008 y DS-012 |
| `cnAXF` "Gray" | `#959595` | Bordes y placeholder de campos, icono `Default`, color del icono `Empty`, `Shape`, `mobileSelect`, `Message` | Pendiente: decidir si es un token de borde (ver Diferencias) |
| `cmyjB0` "Orange Gradient 100%" | `#FF8C00` | `webPrimary`, `web2` (degradado) | Ya no la usa `mobilePrimary`; queda para la web (DS-005) |

Nomenclatura de variables nuevas: `Color - Rol - Opacidad`, igual que las
existentes (ver DS-002).

## Variables de fuente

| Token | Variable en Bubble | Compatibilidad |
|---|---|---|
| `font.text` Poppins | Fuente por defecto (`var(--font_default)`) | Sin verificar. Las dos variables creadas a mano (`AI Default Font`, `AI Secondary Font`) son Poppins, pero ningún estilo las usa |
| `font.display` Raleway | `cnBrS` "Display - Raleway" | Exacta como variable; **ningún estilo la usa todavía** (DS-009) |

## Estilos reutilizables

Solo se listan los que tocan la marca. Ningún estilo se borró ni se renombró en esta fase.

| Estilo | Token / regla | Estado en Bubble |
|---|---|---|
| `mobilePrimary` (Button) | Botón principal | Fondo `primary` liso (degradado con los dos extremos en `primary`), sin sombra, radio 14, relleno 14/24. **Pendiente:** texto 700 18 (hoy 500 16, DS-005) y presionado (hoy degradado `#A84400` → `#D45500`, aproximado) |
| `mobileSecondary` (Button) | Botón secundario | Sin sombra. **Pendiente:** fondo azul `cmyip` y presionado azul `rgba(42,93,174)` (DS-008) |
| `mobileCancel`, `mobileDelete` | Sin sombras | Sin sombra. Fuera del Design System en lo demás (texto y fondo propios) |
| `mobileSH5` (Text) | `text-secondary` | `cnBrT` `#B3B3B3`. Exacta |
| `Footer Link`, `Label 10 m` (Text) | `text-secondary` | `cnBrT`. Antes apuntaban a variables borradas (DS-006) |
| `Popup Basic`, `Popup Conflict` | Superficie | `surface`. Antes apuntaban a una variable borrada (DS-006). Radio 40 pendiente (DS-011) |
| `onboardingSteps` (Group) | Sin sombras | Sin sombra |
| `mobileH1`…`mobileH5`, `Header Web` | Títulos en Raleway | Usan la fuente por defecto. Pendiente (DS-009) |
| `mobileContenedor`, `ShortListItem` | Superficie lisa | Degradado `#404040` → `#363636`. Pendiente (DS-010) |
| `Empty` (Icon) | Superficie | Círculo `#363636`. Pendiente (DS-010) |
| `Input`, `MultiLineInput`, `DateInput`, `Dropdown`, `Autocomplete` | Radio 14, foco en naranja | Cumplen. `Dropdown` y `Autocomplete` aún suman sombra al enfocar (DS-004) |
| `TabItem` | — | Gris `#454545` (contraste 1,9:1 sobre el fondo). Pendiente (DS-013) |

## Estados interactivos

| Estado | Cómo se hace en Bubble | Notas |
|---|---|---|
| Presionado (botones) | Condición "This Button is pressed" en el estilo | Solo se puede editar desde el editor de Bubble (ver Límites) |
| Foco (campos) | Condición "is focused": borde `primary` | Cumple |
| Inválido (campos) | Condición "isn't valid": borde `destructive` | Fuera del Design System (no define error); se mantiene |
| Hover (web) | Condición "is hovered" | Solo en estilos web |
| Deshabilitado | No hay estilo definido | Pendiente de definir |

## Excepciones justificadas

- **Círculos** (radio 100): icono `Empty`, `AppBarButton`, `onboardingSteps`.
  Son avatares, botones de ícono o indicadores de paso, no chips. Se proponen
  como excepción documentada en DS-011.
- **Estados de error** usan la variable Destructive de Bubble; el Design System
  todavía no define un color de error.

## Límites de Bubble y de la conexión

1. **Las variables incorporadas (Primary, Primary contrast, Text, Surface,
   Background, Destructive) y la fuente por defecto no se pueden leer ni
   cambiar** desde la conexión. Hay que comprobarlas en el editor: Styles →
   Style variables. Por eso su compatibilidad figura "Sin verificar".
2. **Las condiciones de un estilo** (presionado, foco, hover) no se pueden
   crear ni cambiar desde la conexión. Se hacen en el editor.
3. **No hay forma de contar qué elementos usan cada estilo.** El impacto se
   estima por el tipo de estilo.
4. **No se puede ver la app en ejecución** desde esta sesión (los dominios de
   Bubble están bloqueados por la red del entorno). Ninguna verificación
   visual se ha hecho todavía.
5. Las variables borradas en Bubble siguen guardadas con su valor (marcadas
   `DELETED`); un estilo que las usa no avisa como error.

## Diferencias pendientes

Ver [`decisions.md`](decisions.md): DS-005, DS-007 a DS-014. Las acciones
manuales en el editor están en [`../docs/backlog.md`](../docs/backlog.md),
sección "Marca".
