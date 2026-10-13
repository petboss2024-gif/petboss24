# Registro de decisiones del Design System

Cada decisión que cambia una regla, la forma de implementarla en Bubble o la
identidad visual. Estados: **Propuesto** (espera aprobación), **Aprobado**,
**Implementado en Bubble**, **Documentado**, **Verificado**, **Pendiente**
(falta información, acceso o una acción manual).

"Verificado" significa que el valor se leyó de vuelta en Bubble. Ninguna
decisión tiene todavía verificación visual en la app (ver límites en
[`bubble.md`](bubble.md)).

Los estilos se citan con su nombre actual (DS-016). El nombre que tenían en
cada fecha está en el catálogo de [`bubble.md`](bubble.md#catálogo-de-estilos).

---

### DS-001 · Fuentes de verdad y estructura documental
- **Problema:** los mismos valores estaban copiados en `brand/README.md`,
  `brand/tokens.json`, `brand/design-system/`, `docs/conventions.md` y el PDF,
  y no había un lugar para la relación con Bubble ni para las decisiones.
- **Decisión:** `tokens.json` es la fuente de los valores; `README.md`, de las
  reglas; `bubble.md`, de cómo se representa cada token en Bubble; este
  archivo, de las decisiones. `design-system/` es la copia literal del sistema
  publicado en Claude (referencia, no se edita a mano) y `docs/conventions.md`
  solo enlaza a `brand/`.
- **Archivos:** `brand/README.md`, `brand/bubble.md`, `brand/decisions.md`, `docs/conventions.md`.
- **Estado:** Documentado.

### DS-002 · Nombres de las variables en Bubble
- **Problema:** las variables creadas a mano siguen el patrón
  `Color - Rol - Opacidad` ("Purple - Secondary - 100%"), pero el Design System
  usa nombres en inglés tipo `text-secondary`.
- **Decisión:** las variables nuevas siguen el patrón de Bubble (`Color - Rol - 100%`)
  y llevan en la descripción el nombre del token. No se renombran las
  existentes en esta fase.
- **Alternativa descartada:** renombrar todas a los nombres del token: cambia
  lo que el equipo ya reconoce en el editor sin aportar funcionalidad.
- **Estado:** Implementado en Bubble (`cnBrT`, `cnBrU`, `cnBrS`) · Documentado · Verificado.

### DS-003 · Texto secundario `#B3B3B3`
- **Problema:** `Mobile Text Secondary Centered` usaba `#959595` escrito a mano.
- **Decisión:** variable `cnBrT` "Gray - Text Secondary - 100%" (`#B3B3B3`) y
  `Mobile Text Secondary Centered` apunta a ella. Contraste 6,7:1 sobre `#2C2C2C` (antes 4,7:1).
- **Efecto:** todos los textos con `Mobile Text Secondary Centered` se ven un poco más claros.
- **Estado:** Implementado en Bubble · Documentado · Verificado (valor del estilo leído de vuelta).

### DS-004 · Sin sombras
- **Decisión:** se quita la sombra de `Button Primary`, `Mobile Button Secondary`,
  `Mobile Button Tertiary`, `Mobile Button Destructive` y `Mobile Progress Step`.
- **Pendiente:** la sombra que `Dropdown` y `Autocomplete` suman al enfocar, y
  la de `Web Popup Dark`, están en condiciones del estilo; solo se pueden quitar en
  el editor. El foco ya queda marcado con el borde naranja, así que la sombra no
  hace falta.
- **Estado:** Implementado en parte · Documentado · Pendiente (acción manual).

### DS-005 · Botón principal liso
- **Problema:** degradado de `primary` a `#FF8C00`, texto 500 16, sombra.
- **Decisión:** fondo liso. Bubble no deja editar la condición de presionado
  desde la conexión, así que se dejó el fondo como degradado con los dos
  extremos en `primary` (se ve liso y conserva la condición existente).
- **Compatibilidad:** aproximada en el estado presionado (degradado `#A84400` →
  `#D45500` en vez de `#A84400` liso).
- **Texto (aprobado 2026-10-12):** 700 18 px en `Button Primary`
  (antes 500 16). Es un estilo `shared`: también cambia los botones naranjas de
  la web que lo usan (reset_pw, webHeader, webFooter). Riesgo: botones angostos
  pueden cortar la etiqueta; revisar en dispositivo.
- **Pendiente:**
  - Presionado exacto: en el editor, cambiar en `Button Primary` la condición
    "is pressed" a `Orange - Pressed - 100%` en los dos extremos.
  - `Web Button Primary Small` (14/500) y `Web Button Primary` (16/600) siguen con
    degradado y texto más chico que la regla de DS-007: se tratan con la web.
- **Estado:** Aprobado · Implementado en Bubble (salvo presionado) · Documentado · Verificado.

### DS-006 · Estilos que apuntaban a variables borradas
- **Problema:** `Web Popup Default` y `Web Popup Default Duplicate` (fondo), `Web Footer Link` y
  `Mobile Text Caption` (texto) usaban variables marcadas como borradas en Bubble.
- **Decisión:** popups a `surface`; los dos textos a `cnBrT` (texto secundario).
- **Estado:** Implementado en Bubble · Documentado · Verificado.

### DS-007 · Contraste del texto blanco sobre naranja
- **Problema:** `brand/README.md` decía que blanco sobre `#FF6600` tiene
  contraste 3:1. El valor real es **2,94:1**: no llega al mínimo de WCAG ni para
  texto grande (3:1).
- **Corrección de la documentación:** se corrige la cifra (hecho, no decisión).
- **Decisión (aprobada 2026-10-12):** se mantiene la marca: texto blanco sobre
  naranja, **siempre en negrita y a 18 px o más**, como excepción documentada a
  WCAG (2,94:1 frente a 3:1). Texto más chico va sobre blanco u oscuro, nunca
  sobre naranja.
- **Alternativas descartadas:** oscurecer el naranja en botones (cambia el color
  de marca) y texto oscuro sobre naranja (6,2:1; cambia la identidad del botón).
- **En Bubble:** `Button Primary` cumple desde DS-005 (700 18).
  `Mobile Button Secondary Small` ya no va sobre naranja (DS-008). Pendientes de la web:
  `Web Button Primary Small` (14/500) y `Web Button Primary` (16/600).
- **Estado:** Aprobado · Documentado · Implementado en la app · Pendiente en la web.

### DS-008 · Botón secundario azul
- **Problema:** `Mobile Button Secondary` usa de fondo `cmyip` "Blue - Text Link"
  (`#007AFF`); al presionar, otro azul. El Design System lo define gris
  (`#2C2C2C`, presionado `#272727`) para que el naranja sea el único color fuerte.
- **Decisión (aprobada 2026-10-12):** fondo `surface`, texto `text`. Variable
  `cnBrY` "Gray - Surface Pressed - 100%" (`#272727`) creada para el presionado.
  `Mobile Button Secondary Small` también deja el azul: fondo `surface`, texto `text` (el texto
  de 12 px no puede ir sobre naranja) y radio 8.
- **Pendiente (editor):** condición "is pressed" de `Mobile Button Secondary`: hoy pone
  el fondo azul `rgba(42,93,174)`; cambiarla a `Gray - Surface Pressed - 100%`.
- **Estado:** Aprobado · Implementado en Bubble (salvo presionado) · Documentado · Verificado.

### DS-009 · Títulos en Raleway y escala tipográfica de la app
- **Problema:** los títulos de la app no tienen fuente propia (usan la fuente
  por defecto). Además la escala del Design System es de marketing (28 y 20 px
  para títulos) y la app usa 34, 30, 28, 24 y 18.
- **Hecho:** variable de fuente `cnBrS` "Display - Raleway" creada (sin uso).
- **Decisión (aprobada 2026-10-12):** Raleway (`cnBrS`) en los cinco títulos
  mobile y en el texto del encabezado web, conservando sus tamaños. El título
  más grande pasa de peso 900 a 800. La escala de la app queda documentada en
  `tokens.json` (`appTypeScale`) como ampliación de la de marketing.
- **Nota:** el texto del encabezado web usa peso 500, que no está entre los
  pesos de Raleway del Design System (700 y 800); Bubble carga la fuente desde
  Google Fonts con cualquier peso. Revisar cuando se rediseñe la web.
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-010 · Tarjetas con degradado
- **Problema:** `Mobile Card Default`, `Mobile List Item Card` y el círculo del icono
  `Mobile Icon Container Empty State` usan grises fuera de la paleta (`#404040` → `#363636` y `#363636`).
- **Decisión (aprobada 2026-10-12):** superficie lisa (`surface`) en las dos
  tarjetas y en el círculo del icono `Mobile Icon Container Empty State`.
- **Efecto:** las tarjetas se ven un poco más oscuras. Si un estado vacío cae
  dentro de una tarjeta, el círculo del icono se confunde con el fondo de la
  tarjeta (revisar en dispositivo).
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-011 · Radios fuera de la regla
- **Problema:** `Web Popup Default`/`Web Popup Default Duplicate` 40, `Mobile Button Secondary Small` 6, botones web 8,
  `Mobile Selectable Item Card` 10, `Mobile Floating Message Bar` 20.
- **Decisión (aprobada 2026-10-12):** 14 en los tres popups, los tres botones
  web, `Mobile Selectable Item Card` y las esquinas de `Mobile Floating Message Bar`; 8 en
  `Mobile Button Secondary Small`. Los círculos quedan como excepción documentada en `bubble.md`.
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-012 · Color de los enlaces
- **Problema:** `Mobile Text Link Small`, `Mobile Text Accent` y `Mobile Icon Status` usan azul `#007AFF`. El
  Design System no define enlaces, pero dice que el naranja es para "todo lo que
  el usuario toca para avanzar".
- **Decisión (aprobada 2026-10-12):** `Mobile Text Link Small`, `Mobile Text Accent` y `Mobile Icon Status`
  pasan a `primary`. Ningún estilo usa ya la variable azul `cmyip` salvo la
  condición de presionado de `Mobile Button Secondary` (ver DS-008). No se borra la
  variable: algún elemento de página podría usarla directamente.
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-013 · Pestañas inactivas
- **Problema:** `Mobile Tab Bar Item` usa `#454545`, contraste 1,9:1 sobre el fondo: casi
  ilegible.
- **Decisión (aprobada 2026-10-12):** `Mobile Tab Bar Item` en `cnBrT` (`#B3B3B3`, 8,6:1).
- **Efecto:** la pestaña activa se marca en cada pestaña con el color de texto
  (blanco); la diferencia entre activa (blanco) e inactiva (`#B3B3B3`) es
  pequeña. Propuesta para la fase de componentes: activa en naranja `primary`
  (se cambia en las 4 pestañas del Tab Bar, no en el estilo).
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-014 · `appEmptyState` convertido en FloatingGroup (fuera de alcance)
- **Problema:** el reusable `appEmptyState` pasó de tipo Group a FloatingGroup
  después del 2026-10-11 (cambio hecho en el editor, no en estas sesiones).
  Bubble marca 13 errores, uno por cada pantalla que lo usa, y los estados
  vacíos pueden aparecer flotando fuera de su lugar. `appEmptyState_copy`
  sigue siendo Group.
- **Decisión (aprobada 2026-10-12):** volver a Group. Al cambiar de tipo se
  habían perdido los rellenos (24/16) y se restauraron. Bubble pasa de 19 a 6
  errores (los 6 que ya existían antes).
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-015 · Nombres de estilos por plataforma
- **Problema:** 76 estilos con nombres mezclados (`mobileH1`, `web2`, `Cards`,
  `mobileContenedor`, tres distintos llamados `defaultMobile`). El nombre no
  decía la plataforma: 11 estilos "mobile" se usan también en la web.
- **Decisión (aprobada 2026-10-12):** `{plataforma}{Elemento}{Variante}`, con
  plataforma `mobile`, `web` o `shared` según el uso real. El uso en la web se
  comprobó elemento por elemento (393 elementos de las 13 páginas y 4 reusables
  web); las páginas Native Mobile cuentan como app.
- **Efecto:** solo cambia el nombre visible; los ids y los elementos que usan
  cada estilo no cambian. Bubble: sin errores nuevos.
- **Límite:** no se recorrieron las vistas mobile elemento por elemento; los
  estilos sin uso en la web se marcaron `mobile`. Si un estilo `web` aparece en
  la app, se renombra a `shared`.
- **Archivos:** `docs/conventions.md` (regla), `brand/bubble.md` (catálogo).
- **Estado:** Sustituido por DS-016 (2026-10-13). Fue Aprobado · Implementado en Bubble · Documentado · Verificado.

### DS-016 · Nombres de estilos por función
- **Problema:** DS-015 solo cambió el prefijo. Muchos nombres seguían
  heredando la historia del estilo y no su función: `mobileButtonMenu` son
  pestañas segmentadas, `mobileButtonCancel` también guarda y elige foto,
  `mobileTextPretitle` es texto destacado, `mobileTextInputLabel` muestra
  métricas, `mobileIconLink` marca estados.
- **Decisión (pedida 2026-10-13):** `[Plataforma] [Componente] [Variante]
  [Rol o tamaño]` en Title Case con espacios; sin prefijo para los estilos que
  usan la app y la web. Cada nombre se decidió por el uso real (lectura de
  ~2.400 elementos: 1.844 en vistas mobile, 169 en reusables, 393 en la web).
  Regla en `docs/conventions.md`; catálogo, duplicados y estilos sin uso en
  `brand/bubble.md`.
- **Alcance:** solo nombres. No cambió ningún color, tamaño ni condición.
  No se fusionó ni borró ningún estilo.
- **Efecto:** los elementos guardan el id del estilo, así que ninguno cambia.
  76 de 76 nombres leídos de vuelta y comparados con el catálogo; el
  comprobador de Bubble da los mismos 6 errores de antes, ajenos a los estilos.
- **Propuesto, falta aprobar:** borrar `Web Popup Default Duplicate`
  (idéntico a `Web Popup Default`); unificar `Mobile Icon Status` con
  `Icon Primary`, `Mobile Text Secondary` con `Mobile Text Secondary Centered`
  y `Mobile Text Title Large` con `Mobile Heading H5`; borrar los 14 estilos
  sin uso; corregir los usos indebidos (`Mobile Text Link Small` como
  etiqueta, `Text Body Small` como error) y definir un token de error.
- **Confianza baja:** `Mobile Text Label Strong` (se usa para métricas y
  propinas) y `Mobile Floating Header` (sin uso, función no confirmada).
- **Savepoints:** antes `1791845098353`, después `1791897738738`.
- **Archivos:** `docs/conventions.md`, `brand/bubble.md`, `brand/decisions.md`,
  `docs/backlog.md`, `docs/empty-states.md`.
- **Estado:** Aprobado · Implementado en Bubble · Documentado · Verificado
  (nombres leídos de vuelta; sin verificación visual, que no hace falta para
  un renombrado). Consolidación: Propuesto.
