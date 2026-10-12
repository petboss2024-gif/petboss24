# Marca Petboss 24 (Design System, versión maestra octubre 2026)

Esta carpeta es la referencia de marca para todo lo que se diseñe o programe:
la app en Bubble, la landing y las piezas de marketing. **Si otro documento de
este repositorio dice algo distinto sobre colores, letras o estilo, manda lo que
dice aquí.** Las notas anteriores se escribieron antes de definir el sistema.

- Sistema completo (vista visual con colores, componentes y logos):
  https://claude.ai/artifact/2cpP8NRdwzYvdapgNRG7a5
- Resumen para personas: [`PB24 Resumen de marca 2026.pdf`](PB24%20Resumen%20de%20marca%202026.pdf)
- Valores para programar: [`tokens.json`](tokens.json)
- Logos en SVG: [`logos/`](logos)

## Voz

- **Actitud Boss:** segura, minimalista, profesional, con humor ligero sin exagerar.
- Nombre público **Petboss 24**: solo la P en mayúscula. Nunca "PetBoss 24".
- Instagram `@petboss24`. Prefijo de archivos `PB24`.
- Textos de la app y del marketing en inglés.

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
| Gris claro | `#E5E5E5` | Superficies y botón secundario en modo claro. |
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
  va sobre blanco u oscuro, nunca sobre naranja.

## Tipografía

Solo dos familias: **Raleway** para títulos y **Poppins** para todo lo demás,
números incluidos (con cifras tabulares si deben alinearse). No se usa ninguna
otra fuente.

| Estilo | Fuente | Tamaño |
|---|---|---|
| Display (marketing) | Raleway 800 | 96 px |
| Heading | Raleway 800 | 28 px |
| Subheading | Raleway 700 | 20 px |
| Button | Poppins 700 | 18 px |
| Body | Poppins 400 | 16 px |
| Label | Poppins 600 | 13 px |

## Forma y espacio

- **Radio:** 14 px en botones, campos, tarjetas y hojas. 8 px para piezas dentro
  de otra. Forma de píldora solo en chips.
- **Sin sombras:** la profundidad la da el color (fondo, luego superficie).
- **Espaciado:** 4, 8, 16, 24. Botones: 14 px arriba y abajo, 24 px a los lados.
- **Ancho:** mobile-first, máximo 430 px.

## Componentes

- **Botón principal:** fondo naranja, texto blanco Poppins 700 18 px. Al presionar,
  naranja más oscuro. Uno solo por pantalla.
- **Botón secundario:** fondo gris suave (`#2C2C2C` oscuro, `#E5E5E5` claro),
  texto en el color de texto. Al presionar, el fondo se oscurece un poco.
- **Chip:** píldora gris; el seleccionado lleva un contorno naranja de 1,5 px.

## Logo

| Archivo | Cuándo |
|---|---|
| `PB24-logo-sobre-naranja.svg` | Fondo naranja (PET gris oscuro, BOSS24 blanco) |
| `PB24-logo-sobre-blanco.svg` | Fondo blanco o claro (PET gris oscuro, BOSS24 naranja) |
| `PB24-logo-sobre-oscuro.svg` | Fondo oscuro (PET naranja, BOSS24 blanco) |
| `PB24-perro-blanco.svg` | Cara del perro sobre naranja u oscuro (avatar, ícono) |
| `PB24-perro-naranja.svg` | Cara del perro sobre oscuro o blanco |

Nunca recolorear, deformar ni poner sobre violeta.

## Cómo se actualiza

El original es el Design System en Claude. Cuando cambie, se actualizan a la vez
esta carpeta, el PDF en Drive (Petboss 24 › HQM › Design System) y el documento
del proyecto PB24 en Claude.
