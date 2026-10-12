# Marca Petboss 24 (Design System, versión maestra octubre 2026)

Esta carpeta es la referencia de marca para todo lo que se diseñe o programe:
la app en Bubble, la landing y las piezas de marketing. **Si otro documento de
este repositorio dice algo distinto sobre colores, letras o estilo, manda lo que
dice aquí.** Las notas anteriores se escribieron antes de definir el sistema.

- Sistema completo (vista visual con colores, componentes y logos):
  https://claude.ai/artifact/2cpP8NRdwzYvdapgNRG7a5
- Resumen para personas: [`PB24 Resumen de marca 2026.pdf`](PB24%20Resumen%20de%20marca%202026.pdf)
- Valores para programar: [`tokens.json`](tokens.json)
- **Copia íntegra del Design System** (sin resumir: tokens con sus notas de uso, estilos de texto completos, CSS de componentes, vistas previas, portada, fuentes): [`design-system/`](design-system/README.md)
- Logos en SVG: [`logos/`](logos)

## Voz

- **Slogan:** Pet-Sitters First, Pets Always.
- **Actitud Boss:** segura, minimalista, profesional, con humor ligero sin exagerar.
- Nombre público **Petboss 24**: solo la P en mayúscula. Nunca "PetBoss 24".
- Instagram `@petboss24`. Prefijo de archivos `PB24`.
- Textos de la app y del marketing en inglés, para el SF Bay Area (San Mateo,
  Foster City).
- Contacto en piezas de marketing: text o WhatsApp, (650) 773-2089.

## Modo claro y oscuro

La marca tiene dos modos y los dos valen para todo: app, landing y marketing.
Hoy la app y la landing están en oscuro; el claro llegará después. En marketing
se elige el modo según la pieza. El fondo naranja también es parte de la marca.

## Color

| Nombre | Hex | Uso |
|---|---|---|
| Naranja | `#FF6600` | Acciones: botones principales y todo lo que el usuario toca para avanzar. Fondo de marketing. |
| Violeta | `#9C2CF3` | Ofertas, promociones y novedades. Pequeño, máximo uno por pantalla, nunca un botón. |
| Gris oscuro | `#404040` | Texto principal en modo claro. PET del logo sobre naranja. |
| Blanco | `#FFFFFF` | Texto principal en modo oscuro y sobre naranja o violeta. |
| Gris claro | `#E5E5E5` | Superficies, divisores y botón secundario en modo claro. |
| Fondo app | `#161616` | Fondo en modo oscuro. |
| Superficie app | `#2C2C2C` | Tarjetas, hojas y botón secundario en modo oscuro. |
| Texto secundario | `#B3B3B3` oscuro · `#6B6B6B` claro | Fechas, descripciones, ayudas. Nunca en botones ni títulos. |

Por modo:

| Rol | Oscuro | Claro |
|---|---|---|
| Fondo | `#161616` | `#FFFFFF` |
| Superficie | `#2C2C2C` | `#E5E5E5` |
| Texto | `#FFFFFF` | `#404040` |
| Texto secundario | `#B3B3B3` | `#6B6B6B` |

Reglas:
- **El naranja actúa, el violeta destaca.** Misma regla en claro y oscuro.
- **Texto sobre naranja:** blanco, en negrita, de 18 px o más. El texto pequeño
  va sobre blanco u oscuro, nunca sobre naranja. (Blanco sobre naranja tiene
  contraste 3:1: solo alcanza para texto grande o en negrita).
- **Texto sobre violeta:** blanco, se lee bien en cualquier tamaño.

## Tipografía

Solo dos familias: **Raleway** para títulos y **Poppins** para todo lo demás,
números incluidos (con `font-variant-numeric: tabular-nums` si deben
alinearse). No se usa ninguna otra fuente. Pilas CSS:
`"Raleway", system-ui, sans-serif` y `"Poppins", system-ui, sans-serif`.
Archivos: Raleway 700 y 800; Poppins 400, 500, 600 y 700
(en [`design-system/fonts/`](design-system/fonts)).

| Estilo | Fuente | Tamaño / interlineado | Peso | Uso |
|---|---|---|---|---|
| display | Raleway | 96 / 91 px | 800 | Titular de marketing sobre naranja (los flyers usaron 96–124 px) |
| heading | Raleway | 28 / 32 px | 800 | Títulos de sección y de servicio, teléfono |
| subheading | Raleway | 20 / 26 px | 700 | Títulos de tarjeta |
| button | Poppins | 18 / 24 px | 700 | Texto de botones |
| body | Poppins | 16 / 23 px | 400 | Texto corrido |
| body-sm | Poppins | 15 / 21 px | 400 | Descripciones de servicios |
| label | Poppins | 13 / 18 px, espaciado 0.02em | 600 | Chips y notas |

## Forma y espacio

- **Radio:** 14 px en botones, campos, tarjetas y hojas (tomado de la app). 8 px
  para piezas dentro de otra con relleno (ej. un QR o una imagen interna). Forma
  de píldora (999 px) solo en chips.
- **Sin sombras:** la profundidad la da el color (fondo, luego superficie).
- **Espaciado:** 4 (huecos mínimos), 8 (entre ícono y texto, dentro de chips),
  16 (relleno de grupos, hojas y tarjetas), 24 (entre bloques). Botones: 14 px
  arriba y abajo, 24 px a los lados.
- **Ancho:** mobile-first, máximo 430 px.

## Componentes

- **Botón principal** (`pb-btn`): fondo naranja `#FF6600`, texto blanco Poppins
  700 18/24 px, radio 14, relleno 14/24, sin borde. Al presionar: `#A84400`.
  Uno solo por pantalla. Textos en verbo: "Book now", "Message sitter".
- **Botón secundario** (`pb-btn pb-btn--secondary`): fondo de superficie
  (`#2C2C2C` oscuro, `#E5E5E5` claro), texto en el color de texto (blanco en
  oscuro, `#404040` en claro). Al presionar, el fondo se oscurece 12 %:
  `#272727` en oscuro y `#CACACA` en claro. Misma forma en los dos modos.
- **Chip** (`pb-chip`): píldora con fondo de superficie y texto label (Poppins 600
  13/18 px, espaciado 0.02em), relleno 8/16. El seleccionado (`pb-chip--on`)
  lleva contorno naranja interior de 1,5 px. Nunca relleno naranja con texto
  pequeño. Uso: elegir una opción de una lista corta (Visits, Walks, Boarding).

El CSS exacto está en [`design-system/components/bundle.css`](design-system/components/bundle.css).

## Logo

| Archivo | Cuándo |
|---|---|
| `PB24-logo-sobre-naranja.svg` | Fondo naranja (PET gris oscuro, BOSS24 blanco) |
| `PB24-logo-sobre-blanco.svg` | Fondo blanco o claro (PET gris oscuro, BOSS24 naranja) |
| `PB24-logo-sobre-oscuro.svg` | Fondo oscuro (PET naranja, BOSS24 blanco) |
| `PB24-perro-blanco.svg` | Cara del perro sobre naranja u oscuro (avatar, ícono) |
| `PB24-perro-naranja.svg` | Cara del perro sobre oscuro o blanco |

Todos tienen fondo transparente. Espacio libre alrededor igual a la altura de la
letra T. El hueco de la frente de la cara del perro es un recorte: deja ver el
fondo. Nunca recolorear, deformar ni poner sobre violeta.

## Cómo se actualiza

El original es el Design System en Claude. Cuando cambie, se actualizan a la vez
esta carpeta, el PDF en Drive (Petboss 24 › HQM › Design System) y el documento
del proyecto PB24 en Claude.
