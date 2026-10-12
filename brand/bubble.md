# Design System en Bubble

Cómo se representa la marca en la app de Bubble (`valecans-home-89169`,
Main/test): variables, estilos, estados, excepciones y límites. Los **valores**
de la marca se definen en [`tokens.json`](tokens.json) y las **reglas** en
[`README.md`](README.md); este archivo solo dice dónde y cómo vive cada uno en
Bubble y qué falta. Las decisiones y su estado están en
[`decisions.md`](decisions.md).

Revisado contra Bubble el 2026-10-12. Savepoint antes de los cambios:
`1791841900553`.

## Nombres de los estilos

Formato `{plataforma}{Elemento}{Variante}` en lowerCamelCase (regla en
[`docs/conventions.md`](../docs/conventions.md)):

- `mobile`: solo en la app (vistas mobile, reusables mobile y páginas
  "Native Mobile" que la app abre: booking-payment*, petsitter-onboarding*,
  walk-tracking-petsitter).
- `web`: solo en el sitio web (index, webHeader, webFooter, páginas legales).
- `shared`: lo usan los dos. **Cambiar un estilo `shared` cambia la app y la web.**

Bubble no separa los estilos por plataforma: cualquiera se puede asignar a
cualquier elemento. La plataforma se decidió por uso real (escaneo de los 393
elementos de la web, 2026-10-12); los estilos que no aparecen en la web se
marcaron `mobile`. Los ids internos no cambian, así que renombrar no rompe nada.



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
| `button.secondary.dark.pressed` | `#272727` | `cnBrY` "Gray - Surface Pressed - 100%" | Exacta (creada 2026-10-12; la condición de presionado se asigna en el editor, DS-008) |
| `gray-dark`, `gray-light`, modo claro | `#404040`, `#E5E5E5`, `#6B6B6B` | — | No se crean hasta que la app tenga modo claro |

Variables de Bubble **fuera del Design System** (siguen en uso):

| Variable | Valor | Usada en | Estado |
|---|---|---|---|
| `cmyip` "Blue - Text Link - 100%" | `#007AFF` | Solo la condición de presionado de `mobileButtonSecondary` | Ningún estilo la usa en su valor base desde DS-008 y DS-012; se retira cuando se cambie esa condición en el editor |
| `cnAXF` "Gray" | `#959595` | Bordes y placeholder de campos, icono `mobileIconMuted`, color del icono `mobileIconEmptyState`, `sharedShapeDivider`, `mobileGroupSelect`, `mobileFloatingMessage` | Pendiente: decidir si es un token de borde (ver Diferencias) |
| `cmyjB0` "Orange Gradient 100%" | `#FF8C00` | `webButtonPrimary`, `webButtonPrimaryLarge` (degradado) | Ya no la usa `sharedButtonPrimary`; queda para la web (DS-005) |

Nomenclatura de variables nuevas: `Color - Rol - Opacidad`, igual que las
existentes (ver DS-002).

## Variables de fuente

| Token | Variable en Bubble | Compatibilidad |
|---|---|---|
| `font.text` Poppins | Fuente por defecto (`var(--font_default)`) | Sin verificar. Las dos variables creadas a mano (`AI Default Font`, `AI Secondary Font`) son Poppins, pero ningún estilo las usa |
| `font.display` Raleway | `cnBrS` "Display - Raleway" | Exacta. La usan `sharedTextH1`, `mobileTextH2`, `sharedTextH3`, `sharedTextH4`, `mobileTextH5` y `webTextNav` (DS-009) |

## Estilos reutilizables

Los que aplican la marca. Nombres según el catálogo de abajo.

| Estilo | Token / regla | Estado en Bubble |
|---|---|---|
| `sharedButtonPrimary` | Botón principal | `primary` liso, sin sombra, radio 14, relleno 14/24. **Pendiente:** texto 700 18 (hoy 500 16, DS-005) y presionado exacto (hoy degradado `#A84400` → `#D45500`) |
| `mobileButtonSecondary` | Botón secundario | `surface`, texto `text`, sin sombra. **Pendiente (editor):** presionado azul → `cnBrY` (DS-008) |
| `mobileButtonVerify` | Pieza pequeña | `surface`, texto `text`, radio 8 (DS-008, DS-011) |
| `mobileButtonCancel`, `mobileButtonDelete` | Sin sombras | Sin sombra. Texto y fondo propios (fuera del Design System) |
| `webButtonPrimary`, `webButtonPrimaryLarge`, `webButtonDarkLarge` | Botones web | Radio 14 (DS-011). Siguen con degradado y sombra al pasar el ratón: se tratan con la web (DS-005) |
| `sharedTextH1`, `mobileTextH2`, `sharedTextH3`, `sharedTextH4`, `mobileTextH5`, `webTextNav` | Títulos en Raleway | `cnBrS` Raleway; tamaños de `appTypeScale` en `tokens.json` (DS-009) |
| `mobileTextSecondary`, `webTextFooterLink`, `mobileTextLabelTiny` | `text-secondary` | `cnBrT` (DS-003, DS-006) |
| `mobileTextLinkSmall`, `mobileTextInfo`, `mobileIconLink` | Enlaces en naranja | `primary` (DS-012) |
| `mobileTabItem` | Pestaña inactiva | `cnBrT` (DS-013) |
| `mobileGroupCard`, `mobileListItemCard`, `mobileIconEmptyState` | Superficie lisa | `surface` (DS-010) |
| `webPopupDefault`, `webPopupConflict`, `webPopupDark` | Superficie, radio 14 | `surface` en los dos primeros; radio 14 en los tres (DS-006, DS-011). `webPopupDark` aún con sombra (DS-004) |
| `mobileListItemSelectable`, `mobileFloatingMessage` | Radio 14 | Radio 14 (DS-011) |
| `mobileGroupStep` | Sin sombras | Sin sombra (DS-004) |
| `sharedInputDefault`, `mobileMultilineInputDefault`, `mobileDateInputDefault`, `mobileDropdownDefault`, `mobileAutocompleteDefault` | Radio 14, foco en naranja | Cumplen. Desplegable y autocompletar aún suman sombra al enfocar (DS-004) |

## Estados interactivos

| Estado | Cómo se hace en Bubble | Notas |
|---|---|---|
| Presionado (botones) | Condición "This Button is pressed" en el estilo | Solo se puede editar desde el editor de Bubble (ver Límites) |
| Foco (campos) | Condición "is focused": borde `primary` | Cumple |
| Inválido (campos) | Condición "isn't valid": borde `destructive` | Fuera del Design System (no define error); se mantiene |
| Hover (web) | Condición "is hovered" | Solo en estilos web |
| Deshabilitado | No hay estilo definido | Pendiente de definir |

## Excepciones justificadas

- **Círculos** (radio 100): icono `mobileIconEmptyState`, `AppBarButton`, `mobileGroupStep`.
  Son avatares, botones de ícono o indicadores de paso, no chips. Se proponen
  como excepción documentada en DS-011.
- **Estados de error** usan la variable Destructive de Bubble; el Design System
  todavía no define un color de error.

## Catálogo de estilos

| Estilo (nombre) | Antes | Elemento | Plataforma | Uso en la web (escaneo 2026-10-12) |
|---|---|---|---|---|
| `mobileAppBarButton` | defaultMobile | AppBarButton | mobile | — |
| `mobileAppBarDefault` | defaultMobile | AppBar | mobile | — |
| `mobileAutocompleteDefault` | defaultMobile | AutocompleteDropdown | mobile | — |
| `mobileButtonApple` | apple | Button | mobile | — |
| `mobileButtonCancel` | mobileCancel | Button | mobile | — |
| `mobileButtonDelete` | mobileDelete | Button | mobile | — |
| `mobileButtonMenu` | mobileMenu | Button | mobile | — |
| `mobileButtonSecondary` | mobileSecondary | Button | mobile | — |
| `mobileButtonVerify` | mobileVerify | Button | mobile | — |
| `mobileDateInputDefault` | mobile | DateInput | mobile | — |
| `mobileDropdownDefault` | defaultMobile | Dropdown | mobile | — |
| `mobileFloatingActions` | containerButtons | FloatingGroup | mobile | — |
| `mobileFloatingButtons` | Mobile Buttons | FloatingGroup | mobile | — |
| `mobileFloatingMessage` | Message | FloatingGroup | mobile | — |
| `mobileFloatingTabs` | Tabs | FloatingGroup | mobile | — |
| `mobileGroupCard` | mobileContenedor | Group | mobile | — |
| `mobileGroupSelect` | mobileSelect | Group | mobile | — |
| `mobileGroupStep` | onboardingSteps | Group | mobile | — |
| `mobileIconEmptyState` | Empty | Icon | mobile | — |
| `mobileIconLink` | blue | Icon | mobile | — |
| `mobileIconMuted` | Default | Icon | mobile | — |
| `mobileIconPrimaryPadded` | Main | Icon | mobile | — |
| `mobileInputPlain` | noBorders | Input | mobile | — |
| `mobileListItemCard` | mobile | ShortListItem | mobile | — |
| `mobileListItemPlain` | mobile | SelectableListItem | mobile | — |
| `mobileListItemSelectable` | default | SelectableListItem | mobile | — |
| `mobileListSelectable` | mobile | SelectableList | mobile | — |
| `mobileListShort` | mobile | ShortList | mobile | — |
| `mobileMapDefault` | mobile | GoogleMap | mobile | — |
| `mobileMapPreview` | defaultMobile | GoogleMap | mobile | — |
| `mobileMultilineInputDefault` | mobile | MultiLineInput | mobile | — |
| `mobilePageDefault` | defaultMobile | Page | mobile | — |
| `mobileSheetDefault` | mobile | Sheet | mobile | — |
| `mobileTabItem` | defaultMobile | TabItem | mobile | — |
| `mobileTextBody` | Label 2 regular | Text | mobile | — |
| `mobileTextButton` | mobileBtnText | Text | mobile | — |
| `mobileTextChat` | Chatting 13 m | Text | mobile | — |
| `mobileTextH2` | mobileH2 | Text | mobile | — |
| `mobileTextH5` | mobileH5 | Text | mobile | — |
| `mobileTextInfo` | body info | Text | mobile | — |
| `mobileTextInputLabel` | mobileInputLabel | Text | mobile | — |
| `mobileTextLabelLarge` | Label 2 SB | Text | mobile | — |
| `mobileTextLabelMedium` | Label Medium | Text | mobile | — |
| `mobileTextLabelMuted` | Profile | Text | mobile | — |
| `mobileTextLabelTiny` | Label 10 m | Text | mobile | — |
| `mobileTextLink` | mobileLink | Text | mobile | — |
| `mobileTextLinkSmall` | link | Text | mobile | — |
| `mobileTextPretitle` | mobilePretitle | Text | mobile | — |
| `mobileTextSecondary` | mobileSH5 | Text | mobile | — |
| `mobileTextSubtitle` | mobileSt1 | Text | mobile | — |
| `mobileTextSubtitleLarge` | Subtitle | Text | mobile | — |
| `mobileTextSubtitleMuted` | Subtitle 14 | Text | mobile | — |
| `mobileTextTitle` | mobileTitle | Text | mobile | — |
| `mobileToggleDefault` | defaultMobile | Toggle (plugin) | mobile | — |
| `mobileWebViewDefault` | mobile | WebView | mobile | — |
| `sharedButtonPrimary` | mobilePrimary | Button | shared | reset_pw, webHeader, webFooter |
| `sharedGroupDefault` | defaultMobile | Group | shared | index, reset_pw, 404, webHeader |
| `sharedGroupPage` | mobileDark | Group | shared | index, reset_pw, webFooter |
| `sharedHtmlDefault` | defaultMobile | HTML | shared | index, privacy, terms, walk-tracking-customer |
| `sharedIconPrimary` | Cards | Icon | shared | webFooter |
| `sharedInputDefault` | mobile | Input | shared | reset_pw |
| `sharedShapeDivider` | Mobile | Shape | shared | index |
| `sharedTextH1` | mobileH1 | Text | shared | reset_pw |
| `sharedTextH3` | mobileH3 | Text | shared | auth-email-confirmation |
| `sharedTextH4` | mobileH4 | Text | shared | reset_pw, webFooter |
| `sharedTextSmall` | mobileSmallBody | Text | shared | webFooter |
| `webButtonDarkLarge` | web3 | Button | web | index |
| `webButtonPrimary` | webPrimary | Button | web | — |
| `webButtonPrimaryLarge` | web2 | Button | web | index |
| `webPopupConflict` | Conflict | Popup | web | — |
| `webPopupDark` | Black | Popup | web | — |
| `webPopupDefault` | Basic | Popup | web | — |
| `webRepeatingGroupDefault` | defaultMobile | RepeatingGroup | web | index |
| `webTextFooterLink` | Footer Link | Text | web | webFooter |
| `webTextNav` | Header Web | Text | web | index |
| `webTextSmall` | small text web | Text | web | index, webFooter |

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
