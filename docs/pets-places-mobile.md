# My Pets y My Places: implementación de la auditoría (2026-10-11)

> Hecho en **Main (test)**, solo vistas y reusables **mobile**. **Nada desplegado a Live.**
> Ninguna prueba en dispositivo todavía (ver "Pendiente de verificar").
>
> Savepoints:
> - Antes: "Antes de implementar auditoría My Pets y My Places (2026-10-10)" — `1791642060025`
> - Después: "Después de implementar auditoría My Pets y My Places (fases 2-7)" — `1791725583744`
>
> Rollback: Version control del editor → restaurar el savepoint "Antes…", o
> `restore_savepoint` con `1791642060025`. Restaura todo el editor de Main
> (incluida la privacy rule). No toca datos.

## Alcance y plataformas

- Todo lo tocado es mobile: vistas `petList`, `addressList`, `petDetails`,
  `bookingStep2` y reusables de tipo *Sheet* (`petEditSheet`, `addressEditSheet`,
  `addressDetailsSheet`, `deleteConfirmSheet`, `bookingEditSheet`,
  `bookingDetailsSheet`). Ninguna página web cambió.
- Lo único global es la privacy rule de `pets` (afecta a cualquier plataforma).

## Cambios funcionales

| Defecto (auditoría) | Corrección | Dónde |
|---|---|---|
| H-01 Editar reserva no quitaba mascotas | `listOfPets` se guarda con **set list** (la selección exacta) | `bookingEditSheet` → `booking-update-save` |
| H-18 Reserva pagada editable (autorización desincronizada, precio) | Editar solo en **Pending**: botón oculto fuera de Pending; Save condicionado a Pending + customer = Current User + ≥1 mascota + dirección; texto "Only pending bookings can be edited" | `bookingDetailsSheet` (`btnEditBooking`), `bookingEditSheet` |
| Selección vacía en reservas | `bookingStep2` no avanza sin mascota y dirección; mensaje tras intentar continuar. `bookingEditSheet` muestra aviso si se vacía la selección | `booking-step2-continue`, `txtSelectionError` |
| H-02 Estado vacío de My Places nunca visible | Movido fuera de la lista, al grupo flotante del botón "Add"; visible si la lista **terminó de cargar** y tiene 0 elementos | `grpPlacesEmptyState` |
| Estado vacío de My Pets dependía de render no verificado | Igual que Places | `grpPetsEmptyState` |
| H-13 Sin estado de carga | `loadingPets` (overlay existente) y `txtPlacesLoading` visibles mientras la lista carga (`is loading`) | `petList`, `addressList` |
| H-06 Borrar sin confirmación en los detalles | Hoja de confirmación con **Cancel** en los 3 caminos (detalle de mascota, detalle de dirección, deslizar) | `deleteConfirmSheet` |
| H-07 Borrado sin resultado | El botón pasa a "Deleting…" (deshabilitado) y la hoja **solo se cierra cuando el backend marca `delete = yes`**. Luego el detalle se cierra / vuelve atrás | `delete-close-on-success`, `place-close-on-deleted`, `pet-close-on-deleted` |
| H-14 Título vacío en la hoja de borrado | La dirección se asigna antes de mostrar la hoja | `place-confirm-delete-swipe` |
| H-04 ZIP sin cero inicial | Tarjeta, detalle y los 2 selectores usan `zipCodeText`; si está vacío, el ZIP de `geographicAddress` (Google). Ya no se muestra `zipCode` numérico | `txtPlaceCityStateZip`, `txtPlaceZip` |
| H-10 Formato de dirección | "6201 Loch Raven Blvd, Apt 408" y "Baltimore, Maryland 21239" (antes "6201, Loch Raven Blvd" y "Maryland, Baltimore") | tarjeta y selectores |
| H-09 "code" literal con Physical Key | Valor vacío por defecto; oculto con Physical Key o sin método | `txtAccessValue` |
| H-08 Edad por resta de años | Meses exactos: (año·12+mes actual) − (año·12+mes nacimiento), −1 si el día aún no llegó; se muestra "X yr Y mo". Oculta sin fecha o con fecha futura | `txtPetAge`, `grpPetAge` (2 selectores) |
| H-16 Fechas de nacimiento futuras | Selector con máximo = hoy; crear y editar terminan si la fecha es futura; mensaje actualizado | `inpPetBirthdate`, `pet-create-save`, `pet-update-save`, `txtPetFormError` |
| H-15 IA se regeneraba en cada edición de mascotas antiguas | `aiChanged` ya no usa "aiStatus ≠ Done"; regenera si cambió un dato, si `aiStatus = Failed` o si no hay resumen IA guardado | `pet-update-save` |
| H-12 Búsqueda repetida por celda | Eliminado el condicional de `grpPlaceCard` | `addressList` |

**Corrección de la auditoría:** en `bookingStep2` la edad **ya** se ocultaba sin
fecha (no aparecía "2026 years"). El cálculo sí era incorrecto.

## Backend

Sin workflows nuevos. Se conservan `pet-delete`, `place-delete`,
`place-set-default`, `pet-generate-ai-data` (validan dueño = Current User).
Cambio: ahora la app **espera su resultado** (ver H-07) en vez de dar éxito al
programarlos.

No se movió a backend:
- **Crear/editar mascotas y direcciones:** los API workflows programados son
  asíncronos y no devuelven resultado; mover el guardado obligaría a mostrar
  éxito sin confirmación. Siguen en cliente con control de dueño e `isSaving`.
- **Mascotas/dirección de la reserva:** al limitar la edición a Pending ya no
  hay autorizaciones que sincronizar (se otorgan al pagar).
- `place-create` sigue **sin llamadores** y no escribe `zipCodeText`. Si se
  usa en el futuro, hay que añadirlo.

## Privacidad

| Regla | Antes | Ahora |
|---|---|---|
| pets / Petsitters (basic info) | Cualquier petsitter podía **buscar** mascotas de todos los clientes (filtrando por `user`/`delete`) | `Find this in searches` = **no**, sin campos filtrables. Sigue viendo los datos básicos de una mascota **referenciada** (p. ej. la lista de mascotas de una reserva Pending que ve) |

Efecto colateral conocido: en `customerProfile` (vista del petsitter) la lista
de mascotas del cliente (es una búsqueda) muestra solo las mascotas en las que
el petsitter está autorizado. Es la intención (impedir la enumeración).

Sin cambios, revisado:
- **address:** Owner, Authorized petsitter, Admin; everyone sin acceso. Los
  códigos solo se muestran en el detalle. El petsitter autorizado también ve
  `authorizedPetsitters` y `user`; no se recortó porque
  `place-revoke-petsitter-access` corre a veces con el petsitter como Current
  User y modifica esa lista.
- **dgRequest:** las reglas Customer y Applied petsitter tienen
  **auto-binding** en muchos campos (incluidos `pet_list` y `address`). Fuera
  de alcance; anotado en backlog.

Pruebas por rol: **no ejecutadas** (requieren sesiones reales). Ver pendiente.

## Datos y migraciones

Nada ejecutado.

- **Huérfanos en test:** 6/8 direcciones y 3/16 mascotas sin `user`
  ("Lukas" borrada, "Lukass" y "Buddy" activas). Ninguna reserva de test los
  referencia (solo hay 2 reservas visibles y no tienen dirección ni mascotas).
  No hay evidencia fiable de su dueño (`Created By` también vacío), así que
  **no se reasignaron**. Opciones: dejarlos (nadie los ve), o borrarlos tras
  confirmar en Live que tampoco hay referencias.
- **ZIP:** procedimiento de backfill en `pets-places.md` (sigue sin ejecutar).
  La interfaz ya no depende de él: cae al ZIP de Google si `zipCodeText` está vacío.
- **aiStatus vacío** en mascotas antiguas: no se rellenó. La interfaz los trata
  bien (muestra el resumen existente y solo regenera si falta).

## Renombrado

Convención (extiende `conventions.md`, lowerCamelCase):
`grp` grupo · `fgrp` floating group · `txt` texto · `img` imagen · `icn` icono ·
`btn` botón · `inp` input · `sel` selectable list · `list`/`item` lista nativa ·
`sheet` instancia de reusable tipo Sheet · `badge` insignia · `swipe` acción de
deslizar. Workflows: `entidad-acción-contexto` (como los de backend).

No se renombraron campos, option sets ni custom states (ya eran claros).

| Ubicación | Antes | Ahora |
|---|---|---|
| petList | VerticalList pets | listPets |
| petList | VerticalListItem item | itemPet |
| petList | Group pets (×3) | grpPetCard / grpPetCardContent / grpPetInfo |
| petList | Group breed copy 2 · Group breed · Group pets | grpPetNameRow · grpPetBreed · grpPetAttributes |
| petList | Group gender · Group gender copy | grpPetSize · grpPetGender |
| petList | Image A | imgPetPhoto |
| petList | Text Parent group's pets' (×4) | txtPetName · txtPetBreed · txtPetSize · txtPetGender |
| petList | Icon fa fa-angle-right | icnPetChevron |
| petList | No Pets · Text Add your first pet… | grpPetsEmptyState · txtPetsEmptyMessage |
| petList | Charge | loadingPets |
| petList | FloatingGroup Button · Button Add Pets | fgrpPetsActions · btnAddPet |
| petList | petAddEdit | sheetPetCreate |
| petList (wf) | Button Add Pets is tapped · VerticalListItem item is tapped | pet-open-create-sheet · pet-open-details |
| addressList | VerticalList address · VerticalListItem A · SwipeAction A | listPlaces · itemPlace · swipeDeletePlace |
| addressList | listAddress · Group Current cell trusted | grpPlaceCard · grpPlaceCardContent |
| addressList | Group address · Group Container · Group address | grpPlaceInfo · grpPlaceTitleRow · grpPlaceTitle |
| addressList | Text Parent group's addre (×3) | txtPlaceName · txtPlaceStreet · txtPlaceCityStateZip |
| addressList | Group Address · Text Default · Group address | badgeDefaultPlace · txtDefaultBadge · grpPlaceStreet |
| addressList | Icon phosphor regular map · Icon fa fa-angle-right | icnPlace · icnPlaceChevron |
| addressList | noAddress · Text Add your first pet a… · (nuevo) | grpPlacesEmptyState · txtPlacesEmptyMessage · txtPlacesLoading |
| addressList | deleteConfirmation · detailAddress · addAddress | sheetDeletePlace · sheetPlaceDetails · sheetPlaceCreate |
| addressList | FloatingGroup button · Button Add Address | fgrpPlacesActions · btnAddPlace |
| addressList (wf) | Button A · Group Current cell trusted · SwipeAction A is tapped | place-open-create-sheet · place-open-details · place-confirm-delete-swipe |
| reusable | addressDeleteSheet | **deleteConfirmSheet** (ahora sirve para mascotas y direcciones) |
| deleteConfirmSheet | Text… · Button… · (nuevo) | txtDeleteTitle · btnConfirmDelete · btnCancelDelete |
| deleteConfirmSheet (wf) | Button A is tapped · (nuevos) | place-delete-confirm · pet-delete-confirm · delete-close-on-success · delete-cancel |
| petDetails | image · Button Edit · Button Delete · petAddEdit | grpPetHeader · btnEditPet · btnDeletePet · sheetPetEdit |
| petDetails | Text G · (nuevo) | txtPetAiSummary · sheetDeletePet |
| petDetails (wf) | Button A/B is tapped · (nuevo) | pet-open-edit-sheet · pet-confirm-delete · pet-close-on-deleted |
| addressDetailsSheet | Button Edit/Delete Address · addressAddEdit · (nuevo) | btnEditPlace · btnDeletePlace · sheetPlaceEdit · sheetDeletePlace |
| addressDetailsSheet | Text Code · Text code · Text Zip | txtAccessMethod · txtAccessValue · txtPlaceZip |
| addressDetailsSheet (wf) | Button A is tapped (×2) · (nuevo) | place-open-edit-sheet · place-confirm-delete · place-close-on-deleted |
| addressEditSheet | Button save · Button cancel · Text Please choose… · Input Zip | btnSavePlace · btnCancelPlace · txtPlaceFormError · inpPlaceZip |
| addressEditSheet (wf) | Button save is tapped (×3) · Button cancel | place-create-save · place-update-save · place-save-mark-attempt · place-cancel-edit |
| petEditSheet | Button Save · Button Cancel · Text Please add… | btnSavePet · btnCancelPet · txtPetFormError |
| petEditSheet | Input Name/Breed/Age Pet | inpPetName · inpPetBreed · inpPetBirthdate |
| petEditSheet (wf) | Button Save is tapped (×3) · Button Cancel | pet-create-save · pet-update-save · pet-save-mark-attempt · pet-cancel-edit |
| bookingStep2 | SelectableList Pets/Locations · Image ASAP img pet | selPets · selPlaces · imgPetPhoto |
| bookingStep2 | Text …copy 3 · Group pets · Text … · Button Continue · (nuevo) | txtPetAge · grpPetAge · txtPlaceCityStateZip · btnContinue · txtSelectionError |
| bookingStep2 (wf) | Button Continue is tapped · (nuevo) | booking-step2-continue · booking-step2-mark-attempt |
| bookingEditSheet | igual que bookingStep2 · Button Edit Address Save · (nuevo) | selPets · selPlaces · imgPetPhoto · txtPetAge · grpPetAge · txtPlaceCityStateZip · btnSaveBooking · txtSelectionError |
| bookingEditSheet (wf) | Button Edit Address Save is tapped | booking-update-save |
| bookingDetailsSheet | Button Edit Request | btnEditBooking |

## Diseño mobile

- Imagen de reemplazo (la misma de `petEditSheet`) en lista, cabecera del
  detalle y los 2 selectores.
- Quitado el texto fijo "Dog" (y su icono/separador) en lista, detalle y
  selectores: el modelo no guarda especie.
- Borradas las plantillas ocultas sin datos de `petDetails`: "Vaccinated • Up
  to date" y "Emergency Contacts" (veterinario, teléfono y dirección inventados).
- `petList`: safe area y status bar activados, igual que `addressList`.
- Textos de estado vacío reescritos ("No places yet. Add your first address to book a walk.").

## Verificación realizada

- Issue checker de Bubble en cada vista/reusable tocado y en `pets`: **0 issues**.
- App completa: **6 issues**, los mismos 6 preexistentes
  (`walk-tracking-petsitter` y backend). Ninguno nuevo.
- Expresiones nuevas validadas con el validador de Bubble antes de escribirlas.
- Datos: solo lecturas (Data API) para huérfanos y referencias.

## Pendiente de verificar (no probado)

- **En dispositivo (393 px y otros):** estados vacío/carga en el grupo
  flotante, safe area de `petList`, textos largos, hoja de borrado con Cancel.
- **Borrado:** si el backend falla o no corre (supuesto de Current User en API
  workflows programados), la hoja se queda en "Deleting…" hasta pulsar Cancel.
  No hay aviso de error: Bubble no notifica fallos de workflows programados.
- **Edad:** probar fechas límite (mismo mes, fin de mes, 29-feb).
- **Privacidad por rol:** cliente dueño, cliente ajeno, petsitter autorizado,
  petsitter no autorizado (debe ver la mascota de una reserva Pending pero no
  poder buscar otras), admin.
- **HEIC:** 6 fotos de test son `.heic`; confirmar que Imgix las sirve en
  Android/WebView.
- **Error de carga** de listas: Bubble no lo distingue de "vacía"; no se
  implementó.
- **"Gigant" → "Giant":** la herramienta no puede editar etiquetas de option
  sets. Cambiarlo a mano en el editor (Data → Option sets → OS_Size → xl). Solo
  cambia la etiqueta, no el valor guardado.
