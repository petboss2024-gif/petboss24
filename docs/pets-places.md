# Pets y Places: refactor (2026-10-10)

> Hecho en **Main (test)**. **Nada desplegado a Live.** Sin prueba en dispositivo
> todavía (ver "Pendiente").
>
> Savepoints:
> - Antes: "Antes de refactor Pets y Places" (`1791637355005`)
> - Después: "Después de refactor Pets y Places (fases 1-4)" (`1791639308907`)

## Workflows de backend

Todos son API workflows **no expuestos** (`expose = no`), con
`ignore privacy rules = no`. Se programan desde la app con *Schedule API
workflow*. Cada acción comprueba que el dueño del registro sea **Current User**.
Si Current User no es el dueño, no hace nada.

| Workflow | Parámetro | Qué hace |
|---|---|---|
| `pet-generate-ai-data` | pet | `aiStatus = Pending` y `aiRequestedAt = ahora`. Luego 4 llamadas a Gemini con los datos **guardados**. Si hay texto: guarda `aIBasicinfo`, `aIHealthInformation`, `aIAdditionalInformation` y `personality`, con `aiStatus = Done`. Si la primera respuesta viene vacía: `aiStatus = Failed`. |
| `pet-delete` | pet | Soft delete: `delete = yes`. |
| `place-delete` | address | Soft delete: `delete = yes` y `setAsDefault = no`. |
| `place-set-default` | address | Quita `setAsDefault` a las otras direcciones del usuario y lo pone en esta. No aplica a direcciones borradas. |
| `place-revoke-petsitter-access` | request | Quita a los `listOfPetsitters` de la reserva de `address.authorizedPetsitters` y de `pets.authorizedPetsitters`. Condiciones: Current User es el customer o el petsitter de la reserva, la reserva está confirmada, y ese petsitter no tiene **otra** reserva *Booked* con el mismo customer. |

**Modificados:**
- `checkout_session_completed`:
  - Nuevo paso que agrega el petsitter a `authorizedPetsitters` de cada mascota de la reserva.
  - Los dos pasos que otorgan acceso (dirección y mascotas) ahora exigen además `payment_status = paid` en el evento de Stripe.
- `create-address`: no tiene llamadores. Ahora `ignore privacy rules = no` y solo crea si el parámetro `user` es Current User. No se borró.

**Por qué Schedule API workflow y no custom events:** la herramienta no logra
definir parámetros en un custom event de backend (Bubble pide "Name/Type").
Se probó de nuevo con 3 formatos y no funcionó; el evento de prueba se borró.
Los API workflows sí aceptan parámetros. **Limitación:** son asíncronos, así
que no devuelven resultado a la pantalla. Por eso **crear y editar** siguen
siendo del lado del cliente, con controles de dueño (ver abajo).

> **Supuesto a confirmar en una prueba:** Bubble ejecuta un API workflow
> programado desde la app con *Current User* = el usuario que lo programó.
> Si no fuera así, las condiciones de dueño fallan y no se modifica nada.
> Nunca se modifica un registro ajeno.

## Pets (petEditSheet, petDetails)

- **Guardar (crear):**
  - No guarda si faltan nombre, raza, tamaño o género. Muestra "Please add your pet's name, breed, size and gender."
  - Crea con `user = Current User`, `delete = no` y `aiStatus = Pending`. Después programa `pet-generate-ai-data`.
  - **La IA ya no bloquea el guardado.**
- **Guardar (editar):**
  - Una sola acción *Make changes*. Antes eran dos según hubiera foto. Si no se eligió foto nueva, se conserva la actual.
  - Solo corre si la mascota es del usuario.
  - Programa la IA **solo** si cambió algún dato que usa la IA o si el estado no es *Done*.
  - Los prompts usan el género **guardado**, que ahora es el nuevo.
- **Doble envío:** estado `isSaving`. El botón muestra "Saving..." y queda inactivo, y el workflow no corre si ya está guardando. **No es idempotencia de servidor:** dos dispositivos podrían crear duplicados.
- **Cancel:** antes ocultaba solo el contenido interno y el sheet quedaba abierto. Ahora cierra el sheet, lo resetea y limpia foto, género e intento.
- **petDetails:**
  - Edit y Delete solo se ven si la mascota es del usuario. Antes un petsitter los veía.
  - Delete usa `pet-delete`.
  - El resumen IA muestra "Generating AI summary..." mientras está *Pending*. Si falló, o lleva más de 3 minutos en *Pending*, muestra "AI summary unavailable. Edit and save the pet to try again."
- **customerProfile:** la lista de mascotas mostraba las del **usuario actual** (el petsitter). Ahora muestra las del customer, sin borradas.

## Places (addressEditSheet, addressDetailsSheet, addressList)

- **PL-1:** al tocar una dirección en `addressList`, el sheet de detalle recibe la dirección. Antes se abría vacío.
- **Sin dirección del buscador:** muestra "Please choose an address from the search results before saving." Antes terminaba en silencio.
- **Editar:**
  - Ciudad, estado y ZIP muestran lo **guardado** mientras no se elija otra dirección. Antes se recalculaban desde Google y pisaban lo editado.
  - Solo corre si la dirección es del usuario.
- **Códigos de acceso:** al guardar se vacían los códigos que no corresponden al método elegido. Si *Home access* está apagado, se vacían todos.
- **ZIP:** el input ahora es texto. Se guarda en el campo nuevo `zipCodeText` y también en `zipCode` (número), por compatibilidad. El detalle muestra `zipCodeText` si existe. Antes "07032" se guardaba como 7032.
- **Dirección por defecto:** la marca `place-set-default` en backend. Antes el cliente cambiaba todas las direcciones.
- **Borrar:** los dos caminos (detalle y deslizar) usan `place-delete` (soft delete). Antes el deslizar hacía **borrado definitivo**.
- **Doble envío:** igual que en Pets (`isSaving` + "Saving...").

## Reservas

- Los selectores de mascotas y direcciones de `bookingStep2` y `bookingEditSheet` ya no muestran registros borrados.
- Las direcciones se ordenan de la más nueva a la más vieja.
- **No implementado:** el chequeo en backend de que las mascotas y la dirección elegidas sean del usuario. La reserva se crea en el cliente (fuera del alcance de Pets/Places). Hoy las listas solo ofrecen registros propios.

## Autorización temporal del petsitter

| Momento | Dónde | Efecto |
|---|---|---|
| Pago confirmado (`payment_status = paid`) | `checkout_session_completed` | Petsitter agregado a la dirección y a las mascotas de la reserva |
| Fin del paseo | Página `walk-tracking-petsitter`, workflow *walkCompleted* | Programa `place-revoke-petsitter-access` (reemplaza el paso del cliente que solo quitaba la dirección) |
| Cancelación desde el chat | Vista `chatMessages`, botón de cancelar | Programa `place-revoke-petsitter-access` |
| Cancelación desde `bookingDetailsSheet` | — | Sin cambio: solo se permite en *Pending*, cuando aún no hay acceso |

**No se inventó un plazo de expiración.** Si el paseo nunca se marca como
terminado (no-show, la app se cierra), el acceso no se revoca solo. Decisión
pendiente (ver backlog).

## Privacy rules

- **pets / Petsitters (basic info)** (antes "read only", con todo visible):
  - Solo `name`, `photo`, `breed`, `size`, `gender`, `birthdate`, `spayedNeutered`, `personality`, `user`, `delete`.
  - Búsqueda permitida solo por `user` y `delete`. Sin adjuntos.
- **pets / Authorized petsitter** (nueva, `authorizedPetsitters contains me`): además salud, dieta, comportamiento, notas y resúmenes IA.
- **pets / Owner** y **address / Owner**: auto-binding desactivado. Ningún input lo usaba. Se quitó también la clave vieja `finished__boolean`.

## Campos nuevos

| Tipo | Campo | Tipo de dato |
|---|---|---|
| pets | `authorizedPetsitters` | lista de User |
| pets | `aiStatus` | option set nuevo `petAiStatus` (Pending, Done, Failed) |
| pets | `aiRequestedAt` | fecha |
| address | `zipCodeText` | texto |

Ningún dato existente se modificó ni se borró.

## Migración de ZIP (preparada, **no ejecutada**)

En test hay 8 direcciones. Una tiene el ZIP dañado (`7032`, debería ser
`07032`). Procedimiento propuesto, que necesita autorización antes de correrlo:

1. Hacer un savepoint y exportar el CSV de *address* (Data → App data).
2. Hacer *Bulk* → API workflow `migrate-zip`, que haga `zipCodeText = geographicAddress:extract zip code`. Si eso viene vacío, usar `zipCode` con relleno a 5 dígitos.
3. Comparar en una muestra y verificar que no queden vacíos.
4. Repetir en Live solo después de desplegar y con otro export.
5. Más adelante (opcional): dejar de escribir `zipCode` y borrarlo.

## Pendiente / decisiones

Ver `backlog.md`, sección "Pets y Places".
