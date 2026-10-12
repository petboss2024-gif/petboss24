# Plan de marca y Design System (octubre 2026)

Registro de la sesión que creó el Design System de Petboss 24 (10–12 de octubre
de 2026): qué se decidió, dónde quedó cada cosa y qué falta. Las reglas en sí
están en [`brand/README.md`](../brand/README.md).

## Dónde vive cada cosa

| Qué | Dónde | Para qué |
|---|---|---|
| Design System (original) | Claude, artefacto "Petboss 24 Design System": https://claude.ai/artifact/2cpP8NRdwzYvdapgNRG7a5 | Versión maestra: colores, componentes y logos en vivo |
| Resumen para Claude | Proyecto PB24 en Claude, documento `PB24 Design System.md` | Que cada chat del proyecto conozca la marca |
| Instrucciones del proyecto | Proyecto PB24 en Claude | "Toda pieza visual o de marketing debe seguir el Design System de PB24" |
| PDF para personas | Drive: Petboss 24 › HQM › Design System › `PB24 Resumen de marca 2026.pdf` | Leer, compartir, imprimir |
| Logos SVG | Drive: Petboss 24 › HQM › Design System › Logos (y `brand/logos/` aquí) | 3 logos completos + 2 caras del perro |
| LEEME | Drive: Petboss 24 › HQM › Design System › `LEEME - Design System PB24.txt` | Avisa que esa carpeta es la única vigente |
| Copia para programar | Este repositorio, carpeta `brand/` | Reglas, `tokens.json`, logos, PDF |
| Material viejo | Drive: Mi unidad › Archivo › `Branding 2024 (histórico - NO USAR)` | Respaldo histórico, fuera de Petboss 24 |

Cuando cambie la marca, se actualizan los cuatro a la vez: artefacto,
documento del proyecto, PDF/LEEME en Drive y `brand/`.

## Plan de 6 pasos

1. ✅ Juntar el material de PB24 (manual 2024, logos de Drive, estilos de la app).
2. ✅ Crear el Design System. Aprobado como versión maestra el 2026-10-10.
3. ✅ Instrucciones del proyecto PB24 en Claude y documento `PB24 Design System.md`.
4. ✅ Copiar la marca a GitHub: carpeta `brand/` (PR #5, fusionado el 2026-10-12).
5. ⏳ Aplicar la marca en Bubble (ver abajo).
6. ⏳ Auditar Figma. La parte de Drive ya está hecha.

## Decisiones tomadas

- **Nombre:** "Petboss 24", solo la P en mayúscula, también en nombres técnicos
  (plugin "Petboss Live Walk", proyecto de Google Cloud). Abreviación **PB24**
  en mayúsculas. "Daily Boss Plan" no es regla de marca (fue de un flyer).
- **Voz:** Actitud Boss: segura, minimalista, profesional, humor ligero sin exagerar.
- **Modos:** claro y oscuro valen para todo (app, landing, marketing). Hoy la app
  y la landing están en oscuro.
- **Color:** el naranja actúa, el violeta destaca (ofertas, máximo uno por
  pantalla, nunca botón, en claro y oscuro). Texto sobre naranja: blanco,
  negrita, 18 px o más. Texto secundario `#B3B3B3` oscuro / `#6B6B6B` claro.
- **Letras:** Raleway (títulos) y Poppins (todo lo demás). Fuera: Impetus y DM Mono.
- **Forma:** radio 14 (tomado de la app), 8 anidado, píldora solo en chips. Sin sombras.
- **Botones:** principal naranja con texto blanco Poppins 700 18 px; secundario
  gris suave (`#2C2C2C` / `#E5E5E5`). Se descartaron el secundario con borde
  naranja y las variantes con violeta.
- **Logos:** se simplificaron a 5 piezas. Fuera: versión sobre morado, imagotipo
  compactado y patrón gráfico. El logo sobre blanco es el original del manual
  (PET gris, BOSS24 naranja).
- **Componentes de campaña** (bloque Daily Boss Plan, pestaña desprendible del
  flyer, perrito ilustrado) no son parte del sistema.

## Paso 5: revisión de Bubble (2026-10-12, solo lectura)

Plan propuesto, pendiente de aprobación del dueño. Siempre con savepoint antes y
en Main (test).

**Parte 1 (arreglos pequeños):**
- `notificationList`: texto `cmpIs` "PetBoss 24" → "Petboss 24".
- `notificationList`: "FOR YOU" (`cnBiC`), "ANNOUNCEMENTS" (`cnBiE`) y la hora
  de cada notificación (`cmwXJ` y su par en la lista personal): `#8A8A8A` → `#B3B3B3`.
- Estilo `mobileSecondary`: al presionar pasa de azul `rgba(42,93,174)` a `#3A3A3A`.

**Parte 2 (todos los botones naranjas):**
- Estilo `mobilePrimary`: texto 18 px bold, fondo naranja liso en vez de
  degradado, al presionar naranja oscuro liso. Radio 14 y relleno 14/24 ya cumplen.

**Hallazgos:**
- La conexión con Bubble no permite leer ni cambiar los colores y fuentes
  globales predeterminados: el dueño los revisa en Styles → Style variables.
- DM Mono no aparece en los estilos revisados.
- La página web `walk-tracking-customer` está fuera de marca (violeta `#635BFF`,
  fuente del sistema, fondo blanco) y su título de pestaña dice "Valecans home".

## Otros cambios de la sesión

- GitHub: el usuario pasó de `petboss2024-gif` a `petboss24` y el repositorio de
  `petboss24` a `PB24`. GitHub redirige los nombres viejos.
- Drive: se borraron dos archivos de Valecans Home (tarjeta y buyer persona) y la
  carpeta Branding pasó a Archivo.

## Marketing

- Hecho: flyer para vecinos de Eaves Foster City, versión normal y versión con
  tiras desprendibles (Drive: Petboss 24 › HQM › Marketing › Flyers, prefijo PB24).
- Pendiente: letrero para la zona de bolsas de perro y un post de Instagram,
  ya con el Design System.
