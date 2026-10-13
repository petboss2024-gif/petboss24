# Design System en Bubble

Cómo se representa la marca en la app de Bubble (`valecans-home-89169`,
Main/test): variables, estilos, estados, excepciones y límites. Los **valores**
de la marca se definen en [`tokens.json`](tokens.json) y las **reglas** en
[`README.md`](README.md); este archivo solo dice dónde y cómo vive cada uno en
Bubble y qué falta. Las decisiones y su estado están en
[`decisions.md`](decisions.md).

Revisado contra Bubble el 2026-10-13. Savepoints: antes de los cambios de
marca `1791841900553`; antes y después de los nombres semánticos (DS-016)
`1791845098353` y `1791897738738`.

## Nombres de los estilos

Desde el 2026-10-13 (DS-016) los estilos se nombran por **función**, no por
su aspecto ni por el nombre que tenían:
`[Plataforma] [Componente] [Variante] [Rol o tamaño]`, en Title Case con
espacios, usando solo los segmentos que aportan información. Regla completa
en [`docs/conventions.md`](../docs/conventions.md#estilos-de-bubble).

- **Plataforma:** `Mobile` si solo lo usa la app (vistas, reusables mobile y
  páginas "Native Mobile": booking-payment*, petsitter-onboarding*,
  walk-tracking-petsitter); `Web` si solo lo usa el sitio web (index,
  webHeader, webFooter, páginas legales). **Sin prefijo** si lo usan las dos:
  `Button Primary`, `Heading H1`, `Input Default`, `Container Page`. Cambiar
  un estilo sin prefijo cambia la app y la web.
- **Componente:** qué es para quien diseña (`Heading`, `Text`, `Button`,
  `Input`, `Card`, `Container`, `List`, `Sheet`, `Popup`, `App Bar`,
  `Tab Bar Item`, `Icon`, `Divider`, `Map`…), no siempre el tipo de elemento
  de Bubble: un Group con superficie es `Card`; un Button que hace de pestaña
  es `Tab Segment`.
- **Variante y rol:** jerarquía o intención (`Primary`, `Secondary`,
  `Tertiary`, `Destructive`, `Strong`, `Accent`, `Link`, `Caption`…) y tamaño
  solo cuando hay dos del mismo rol (`Small`, `Large`).

Bubble no separa los estilos por plataforma: cualquiera se puede asignar a
cualquier elemento. La plataforma y la función se decidieron por uso real:
escaneo de los ~2.400 elementos de la app (1.844 en vistas mobile, 169 en
reusables) y de la web (393), 2026-10-12/13. Los elementos guardan el **id**
del estilo, no el nombre, así que renombrar no cambia ningún elemento.

Niveles de compatibilidad usados en las tablas de abajo:

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
| `cmyip` "Blue - Text Link - 100%" | `#007AFF` | Solo la condición de presionado de `Mobile Button Secondary` | Ningún estilo la usa en su valor base desde DS-008 y DS-012; se retira cuando se cambie esa condición en el editor |
| `cnAXF` "Gray" | `#959595` | Bordes y placeholder de campos, icono `Mobile Icon Secondary`, color del icono `Mobile Icon Container Empty State`, `Divider`, `Mobile Card Option`, `Mobile Floating Message Bar` | Pendiente: decidir si es un token de borde (ver Diferencias) |
| `cmyjB0` "Orange Gradient 100%" | `#FF8C00` | `Web Button Primary Small`, `Web Button Primary` (degradado) | Ya no la usa `Button Primary`; queda para la web (DS-005) |

Nomenclatura de variables nuevas: `Color - Rol - Opacidad`, igual que las
existentes (ver DS-002).

## Variables de fuente

| Token | Variable en Bubble | Compatibilidad |
|---|---|---|
| `font.text` Poppins | Fuente por defecto (`var(--font_default)`) | Sin verificar. Las dos variables creadas a mano (`AI Default Font`, `AI Secondary Font`) son Poppins, pero ningún estilo las usa |
| `font.display` Raleway | `cnBrS` "Display - Raleway" | Exacta. La usan `Heading H1`, `Mobile Heading H2`, `Heading H3`, `Heading H4`, `Mobile Heading H5` y `Web Navigation Item` (DS-009) |

## Estilos reutilizables

Los que aplican la marca. Nombres según el catálogo de abajo.

| Estilo | Token / regla | Estado en Bubble |
|---|---|---|
| `Button Primary` | Botón principal | `primary` liso, texto 700 18, sin sombra, radio 14, relleno 14/24 (DS-005, DS-007). **Pendiente (editor):** presionado exacto (hoy degradado `#A84400` → `#D45500`) |
| `Mobile Button Secondary` | Botón secundario | `surface`, texto `text`, sin sombra. **Pendiente (editor):** presionado azul → `cnBrY` (DS-008) |
| `Mobile Button Secondary Small` | Pieza pequeña | `surface`, texto `text`, radio 8 (DS-008, DS-011) |
| `Mobile Button Tertiary`, `Mobile Button Destructive` | Sin sombras | Sin sombra. Texto y fondo propios (fuera del Design System) |
| `Web Button Primary Small`, `Web Button Primary`, `Web Button Secondary` | Botones web | Radio 14 (DS-011). Siguen con degradado, sombra al pasar el ratón y texto menor de 18 px sobre naranja: se tratan con la web (DS-005, DS-007) |
| `Heading H1`, `Mobile Heading H2`, `Heading H3`, `Heading H4`, `Mobile Heading H5`, `Web Navigation Item` | Títulos en Raleway | `cnBrS` Raleway; tamaños de `appTypeScale` en `tokens.json` (DS-009) |
| `Mobile Text Secondary`, `Web Footer Link` | `text-secondary` | `cnBrT` (DS-003, DS-006, DS-016) |
| `Mobile Text Link Small`, `Mobile Text Accent` | Enlaces en naranja | `primary` (DS-012) |
| `Mobile Tab Bar Item` | Pestaña inactiva | `cnBrT` (DS-013) |
| `Mobile Card Default`, `Mobile List Item Card`, `Mobile Icon Container Empty State` | Superficie lisa | `surface` (DS-010) |
| `Web Popup Default` | Superficie, radio 14 | `surface`, radio 14 (DS-006, DS-011) |
| `Mobile Selectable Item Card` | Radio 14 | Radio 14 (DS-011) |
| `Mobile Text Error` | Error | Variable Destructive (valor sin verificar). Creado en DS-016 |
| `Mobile Progress Step` | Sin sombras | Sin sombra (DS-004) |
| `Input Default`, `Mobile Input Multiline`, `Mobile Input Date`, `Mobile Dropdown Default`, `Mobile Autocomplete Default` | Radio 14, foco en naranja | Cumplen. Desplegable y autocompletar aún suman sombra al enfocar (DS-004) |

## Estados interactivos

| Estado | Cómo se hace en Bubble | Notas |
|---|---|---|
| Presionado (botones) | Condición "This Button is pressed" en el estilo | Solo se puede editar desde el editor de Bubble (ver Límites) |
| Foco (campos) | Condición "is focused": borde `primary` | Cumple |
| Inválido (campos) | Condición "isn't valid": borde `destructive` | Fuera del Design System (no define error); se mantiene |
| Hover (web) | Condición "is hovered" | Solo en estilos web |
| Deshabilitado | No hay estilo definido | Pendiente de definir |

## Excepciones justificadas

- **Círculos** (radio 100): icono `Mobile Icon Container Empty State`, `AppBarButton`, `Mobile Progress Step`.
  Son avatares, botones de ícono o indicadores de paso, no chips. Se proponen
  como excepción documentada en DS-011.
- **Estados de error** usan la variable Destructive de Bubble (estilo
  `Mobile Text Error` y borde de campos inválidos); el Design System todavía no
  tiene su valor en `tokens.json`.

## Catálogo de estilos

66 estilos (76 tras DS-016, 11 borrados y 1 creado en la consolidación), agrupados por familia. **Antes** es el nombre del 2026-10-12
(DS-015) y **Original**, el que tenía en Bubble antes de cualquier cambio.
**Usos** = elementos que lo tienen asignado (vistas y reusables mobile; "+web"
si además aparece en la web). **Confianza**: qué tan seguro es que el nombre
describe la función (Baja = revisar).

### Tipografía

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Heading H1` | `sharedTextH1` | mobileH1 | Compartido | 6+web | Alta | Raleway 34/800. Títulos de pantalla (Welcome back, Forgot Password?) |
| `Mobile Heading H2` | `mobileTextH2` | mobileH2 | Mobile | 7 | Alta | Raleway 30/800. Títulos de sección grandes (Services, onboarding) |
| `Heading H3` | `sharedTextH3` | mobileH3 | Compartido | 6+web | Alta | Raleway 28/700 |
| `Heading H4` | `sharedTextH4` | mobileH4 | Compartido | 32+web | Alta | Raleway 24/700. Títulos de hoja y de bloque |
| `Mobile Heading H5` | `mobileTextH5` | mobileH5 | Mobile | 20 | Alta | Raleway 18/600. Títulos de tarjeta y de estado vacío. Absorbió `Mobile Text Title Large` (2026-10-13) |
| `Mobile Text Title` | `mobileTextTitle` | mobileTitle | Mobile | 116 | Alta | 16/600. Títulos de fila, de campo y de tarjeta |
| `Mobile Text Subtitle` | `mobileTextSubtitle` | mobileSt1 | Mobile | 6 | Alta | 18/500 centrado. Subtítulo bajo el título de pantalla |
| `Mobile Text Body Strong` | `mobileTextPretitle` | mobilePretitle | Mobile | 62 | Alta | 600, tamaño por defecto. Texto destacado y descripciones (no es un pretítulo) |
| `Mobile Text Error` | — | — | Mobile | 8 | Alta | 13/500, variable Destructive de Bubble. Mensajes de error y validación (creado 2026-10-13) |
| `Text Body Small` | `sharedTextSmall` | mobileSmallBody | Compartido | 190+web | Alta | 12/500. Texto corrido pequeño. Los errores pasaron a `Mobile Text Error` (2026-10-13) |
| `Mobile Text Secondary` | `mobileTextSubtitleMuted` | Subtitle 14 | Mobile | 20 | Alta | 500, `#B3B3B3` (`cnBrT`). Datos secundarios y descripciones. Absorbió `Mobile Text Secondary Centered`; el centrado va en cada elemento (2026-10-13) |
| `Mobile Text Label Secondary` | `mobileTextLabelMuted` | Profile | Mobile | 25 | Alta | 13/600 blanco al 50 %. Etiquetas de métricas y de campos (Distance, Age, Breed…) |
| `Mobile Text Label Strong` | `mobileTextInputLabel` | mobileInputLabel | Mobile | 13 | Baja | 15/700. Se usa para métricas (Total Bookings, 0), propinas ($5) y un título; no para etiquetas de formulario. Revisar |
| `Mobile Text Link` | `mobileTextLink` | mobileLink | Mobile | 3 | Alta | 13/400 subrayado, naranja. Enlaces legales |
| `Mobile Text Link Small` | `mobileTextLinkSmall` | link | Mobile | 3 | Alta | 12/700 subrayado, naranja. Enlaces de acción (Resend Code). Las etiquetas de campo pasaron a `Mobile Text Label Secondary` (2026-10-13); queda el valor de talla en `bookingDetailsPetsitter` |
| `Mobile Text Action` | `mobileTextButton` | mobileBtnText | Mobile | 10 | Alta | 700 naranja sin subrayado. Acciones de texto (Forgot password?, Sign up, Cancel) |
| `Mobile Text Accent` | `mobileTextInfo` | body info | Mobile | 1 | Media | 14 naranja. Texto destacado en authForgotPasswordSent |
| `Mobile Text Chat Message` | `mobileTextChat` | Chatting 13 m | Mobile | 1 | Alta | 13/500. Texto del mensaje en chatMessages |
| `Web Text Caption` | `webTextSmall` | small text web | Web | 2 | Alta | 11 px, blanco al 80 % |

### Botones

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Button Primary` | `sharedButtonPrimary` | mobilePrimary | Compartido | 35+web | Alta | Naranja, 700/18 |
| `Mobile Button Secondary` | `mobileButtonSecondary` | mobileSecondary | Mobile | 8 | Alta | Superficie gris (View Sitters, Edit, Take Photo) |
| `Mobile Button Secondary Small` | `mobileButtonVerify` | mobileVerify | Mobile | 2 | Alta | Superficie gris, 12/600, radio 8 (Verify) |
| `Mobile Button Tertiary` | `mobileButtonCancel` | mobileCancel | Mobile | 6 | Alta | Fondo translúcido neutro. Cancel, Save, Choose Photo: botón neutro, no solo de cancelar |
| `Mobile Button Destructive` | `mobileButtonDelete` | mobileDelete | Mobile | 4 | Alta | Rojo (Delete, Sign Out, Cancel Request) |
| `Mobile Button Apple Sign In` | `mobileButtonApple` | apple | Mobile | 2 | Alta | Excepción de marca de Apple (blanco con texto oscuro) |
| `Web Button Primary` | `webButtonPrimaryLarge` | web2 | Web | 3 | Alta | CTA naranja del index |
| `Web Button Secondary` | `webButtonDarkLarge` | web3 | Web | 2 | Alta | CTA oscuro del index junto al principal |
| `Web Button Primary Small` | `webButtonPrimary` | webPrimary | Web | 1 | Alta | Botón naranja pequeño del index (`cmgpf`). El escaneo anterior lo dio por error sin uso |

### Formularios

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Input Default` | `sharedInputDefault` | mobile | Compartido | 19+web | Alta | Campo con borde gris, foco naranja |
| `Mobile Input Borderless` | `mobileInputPlain` | noBorders | Mobile | 11 | Alta | Sin borde, dentro de un grupo con borde (login, búsqueda) |
| `Mobile Input Multiline` | `mobileMultilineInputDefault` | mobile | Mobile | 17 | Alta | Texto largo (alergias, notas) |
| `Mobile Input Date` | `mobileDateInputDefault` | mobile | Mobile | 9 | Alta | Selector de fecha y hora |
| `Mobile Dropdown Default` | `mobileDropdownDefault` | defaultMobile | Mobile | 0 | Alta | Sin uso. Se conserva: único estilo de desplegable con la marca (radio 14, foco naranja) |
| `Mobile Autocomplete Default` | `mobileAutocompleteDefault` | defaultMobile | Mobile | 0 | Alta | Sin uso. Se conserva: único estilo de autocompletar con la marca |
| `Mobile Toggle Default` | `mobileToggleDefault` | defaultMobile | Mobile | 2 | Alta | Plugin Toggle Switch |

### Superficies

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Container Default` | `sharedGroupDefault` | defaultMobile | Compartido | 412+web | Alta | Grupo transparente de maquetación |
| `Container Page` | `sharedGroupPage` | mobileDark | Compartido | 37+web | Alta | Fondo de página y relleno 16; envoltorio de cada pantalla |
| `Mobile Card Default` | `mobileGroupCard` | mobileContenedor | Mobile | 48 | Alta | Superficie, radio 14 (filas de menú, tarjetas de mascota) |
| `Mobile Card Option` | `mobileGroupSelect` | mobileSelect | Mobile | 4 | Alta | Superficie con borde gris: opción elegible (duración del paseo) |
| `Mobile List Default` | `mobileListShort` | mobile | Mobile | 3 | Alta | Lista corta de reservas |
| `Mobile List Item Card` | `mobileListItemCard` | mobile | Mobile | 3 | Alta | Tarjeta de reserva en la lista |
| `Mobile Selectable List` | `mobileListSelectable` | mobile | Mobile | 6 | Alta | Lista seleccionable (tipo de paseo, mascotas, lugares) |
| `Mobile Selectable Item Card` | `mobileListItemSelectable` | default | Mobile | 3 | Alta | Ítem con superficie y radio 14 (mascota, postulante) |
| `Mobile Selectable Item Plain` | `mobileListItemPlain` | mobile | Mobile | 5 | Alta | Ítem sin fondo ni relleno |
| `Mobile Progress Step` | `mobileGroupStep` | onboardingSteps | Mobile | 9 | Alta | Círculo de paso del onboarding (welcomeStep1-3), sin sombra |

### Navegación

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Web Navigation Item` | `webTextNav` | Header Web | Web | 5 | Alta | Raleway 500, naranja al pasar. Menú del index |
| `Web Footer Link` | `webTextFooterLink` | Footer Link | Web | 12 | Alta | 12/500 #B3B3B3, naranja al pasar |
| `Mobile Tab Segment` | `mobileButtonMenu` | mobileMenu | Mobile | 15 | Alta | Pestañas segmentadas (Pending/Booked/Past, About/Info) |
| `Mobile App Bar` | `mobileAppBarDefault` | defaultMobile | Mobile | 32 | Alta | Barra superior |
| `Mobile App Bar Button` | `mobileAppBarButton` | defaultMobile | Mobile | 2 | Alta | Botón circular de la barra (ajustes, campana) |
| `Mobile Tab Bar Item` | `mobileTabItem` | defaultMobile | Mobile | 4 | Alta | Pestañas del Tab Bar |

### Elementos flotantes

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Mobile Sheet Default` | `mobileSheetDefault` | mobile | Mobile | 4 | Alta | Hoja inferior |
| `Web Popup Default` | `webPopupDefault` | Basic | Web | 0 | Alta | Sin uso. Se conserva: único estilo de popup (superficie, radio 14) |
| `Mobile Floating Action Bar` | `mobileFloatingActions` | containerButtons  | Mobile | 2 | Alta | Barra inferior con botón Add (petList, addressList) |
| `Mobile Floating Header` | `mobileFloatingTabs` | Tabs | Mobile | 1 | Baja | Solo fondo. Lo usa la definición del reusable `appEmptyState` (resto de cuando pasó a FloatingGroup); nombre engañoso, ver Pendiente |

### Iconografía

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Icon Primary` | `sharedIconPrimary` | Cards | Compartido | 67+web | Alta | Naranja sin fondo. Absorbió `Mobile Icon Status` (2026-10-13) |
| `Mobile Icon Primary Padded` | `mobileIconPrimaryPadded` | Main | Mobile | 14 | Alta | Naranja con relleno 3 (filas de menú, spinner) |
| `Mobile Icon Secondary` | `mobileIconMuted` | Default | Mobile | 64 | Alta | Gris #959595 (flechas, mostrar contraseña) |
| `Mobile Icon Container Empty State` | `mobileIconEmptyState` | Empty | Mobile | 2 | Alta | Círculo de superficie del estado vacío |

### Otros

| Nombre | Antes (2026-10-12) | Original | Plataforma | Usos | Confianza | Función y notas |
|---|---|---|---|---|---|---|
| `Divider` | `sharedShapeDivider` | Mobile | Compartido | 20+web | Alta | Línea gris |
| `HTML Default` | `sharedHtmlDefault` | defaultMobile | Compartido | web | Alta | Sin propiedades |
| `Web Repeating Group Default` | `webRepeatingGroupDefault` | defaultMobile | Web | 3 | Alta | Sin separadores |
| `Mobile Page Default` | `mobilePageDefault` | defaultMobile | Mobile | vista | Alta | Fondo de página de las vistas |
| `Mobile Web View` | `mobileWebViewDefault` | mobile | Mobile | 1 | Alta | Pago en bookingPayment |
| `Mobile Map Default` | `mobileMapDefault` | mobile | Mobile | 2 | Alta | Mapa con borde, estilo blue_gray |
| `Mobile Map Preview` | `mobileMapPreview` | defaultMobile | Mobile | 1 | Media | Mapa con estilo apple en `walkTrackingPetsitter` (`cnAsR0`). El escaneo anterior lo dio por error sin uso |

### Consolidación (2026-10-13, DS-016)

Savepoints: antes `1791914436049`, después `1791915463317`.

| Cambio | Elementos |
|---|---|
| `Mobile Icon Status` → `Icon Primary` (idénticos); estilo borrado | 7 iconos (estados en `tabBookings` y `bookingDetailsCustomer`, `addressDetailsSheet`, `walkTrackingPetsitter`, `appInfoCallout`). Sus condicionales de color se conservan |
| `Mobile Text Secondary Centered` → `Mobile Text Secondary`, que pasa de blanco al 80 % a `#B3B3B3`; estilo borrado | 4 textos (descripción de `appEmptyState` y de su copia, `appPhotoPickerSheet`, "Identity Verified"); el centrado, el tamaño 14 y el interlineado 1,5 quedan en cada elemento |
| `Mobile Text Title Large` → `Mobile Heading H5` (Poppins → Raleway); estilo borrado | 3 textos en `walkTrackingPetsitter` |
| Etiquetas de campo: `Mobile Text Link Small` → `Mobile Text Label Secondary` | 10 en `bookingDetailsPetsitter` (Age, Gender, Size, Breed, Allergies, Medication, Spayed/Neutered, Behavioral Traits, Diet, Additional Notes) |
| Estilo nuevo `Mobile Text Error` (13/500, Destructive) | 8 mensajes de error: formularios de lugar, mascota y reserva (`#FF4D4F` escrito a mano), contraseñas que no coinciden (2), "Code incorrect", "Incorrect or expired code" |
| Borrados sin uso | `Web Popup Default Duplicate`, `Web Popup Dark`, `Mobile Text Lead`, `Mobile Text Body`, `Mobile Text Body Large`, `Mobile Text Caption`, `Mobile Floating Footer`, `Mobile Floating Message Bar` |

**No se borraron**, aunque estaban en la propuesta:

- `Web Button Primary Small` y `Mobile Map Preview`: sí se usan (1 cada uno);
  el escaneo de usos anterior estaba incompleto.
- `Mobile Floating Header`: lo usa la definición de `appEmptyState`. Al
  borrarlo, Bubble marcó el error "None (Custom) is not a possible option" y
  se deshizo.
- `Web Popup Default`, `Mobile Dropdown Default` y `Mobile Autocomplete
  Default`: sin uso, pero son el único estilo de su tipo con la marca; sin
  ellos, un popup, desplegable o autocompletar nuevo nacería sin estilo.

Con esto ya no quedan duplicados conocidos.

### Pendiente

- `appEmptyState` (un Group) usa un estilo de FloatingGroup (`Mobile Floating
  Header`). Asignarle `Container Default` y borrar ese estilo; revisar antes
  que el estado vacío se vea igual.
- `Mobile Text Label Strong` se usa para métricas y propinas: valorar
  renombrarlo `Mobile Text Metric`.
- El token de error: `Mobile Text Error` usa la variable incorporada
  Destructive, cuyo valor no se puede leer desde la conexión. Comprobarlo en el
  editor y añadirlo a `tokens.json`.

### Elementos con formato a mano

Repetidos en varias pantallas sin estilo; candidatos a estilos nuevos:
etiquetas de estado de la reserva, chips de especialidad, cajas de propina,
tarjeta "Become a PetBoss", insignias de verificado, botones de género, campo
del código de verificación. ("Incorrect or expired code" ya usa `Mobile Text Error`.)

## Límites de Bubble y de la conexión

1. **Las variables incorporadas (Primary, Primary contrast, Text, Surface,
   Background, Destructive) y la fuente por defecto no se pueden leer ni
   cambiar** desde la conexión. Hay que comprobarlas en el editor: Styles →
   Style variables. Por eso su compatibilidad figura "Sin verificar".
2. **Las condiciones de un estilo** (presionado, foco, hover) no se pueden
   crear ni cambiar desde la conexión. Se hacen en el editor.
3. **La conexión no lista qué elementos usan cada estilo.** Los usos del
   catálogo salen de leer cada vista, página y reusable elemento por elemento
   (2026-10-12/13); un elemento añadido después no está contado.
4. **No se puede ver la app en ejecución** desde esta sesión (los dominios de
   Bubble están bloqueados por la red del entorno). Ninguna verificación
   visual se ha hecho todavía.
5. Las variables borradas en Bubble siguen guardadas con su valor (marcadas
   `DELETED`); un estilo que las usa no avisa como error.

## Diferencias pendientes

Ver [`decisions.md`](decisions.md): pendientes de DS-004, DS-005, DS-007, DS-008 y DS-016 (duplicados, estilos sin uso y usos indebidos). Las acciones
manuales en el editor están en [`../docs/backlog.md`](../docs/backlog.md),
sección "Marca".
