# Estados vacíos (`appEmptyState`)

Sistema único para los estados vacíos de la app mobile. Implementado en
**Main (test)** el 2026-10-11. Nada se desplegó a Live.

- Savepoint antes: **"Antes de appEmptyState (sistema de estados vacíos)"** — `1791727229224`
- Savepoint después: **"Después de appEmptyState: sistema de estados vacíos en 13 ubicaciones (Main/test)"** — `1791727809392`

## Cómo funciona

Los textos e icono de cada caso viven en el option set **`emptyStateContext`**.
El reusable **`appEmptyState`** recibe una opción como *data source* y la
muestra. La pantalla que lo usa decide **cuándo** se ve (conditional de
visibilidad) y **qué** opción pasa (data source, con conditionals si cambia
por rol o pestaña).

### Option set `emptyStateContext`

Atributos (todos texto): `title`, `description`, `iconKey`, `actionLabel`.

| Opción | Título | Descripción | iconKey | Acción |
|---|---|---|---|---|
| petsEmpty | No pets yet | Add your first pet to get started. | paw | — |
| placesEmpty | No saved places yet | Save a place to make booking easier. | map-pin | — |
| bookingsCustomerPending | No pending requests | Book a walk and your requests will appear here. | calendar | Book a walk |
| bookingsCustomerBooked | No upcoming walks | Confirmed walks will appear here. | calendar | — |
| bookingsPast | No past walks yet | Completed and canceled walks will appear here. | calendar-check | — |
| bookingsPetsitterPending | No requests nearby | New walk requests in your area will appear here. | calendar | — |
| bookingsPetsitterBooked | No upcoming walks | Walks you’ve been booked for will appear here. | calendar | — |
| chatsEmpty | No chats right now | Chats will appear once you’ve booked or matched with a pet sitter. | chat | — |
| notificationsEmpty | You’re all caught up | We’ll let you know when there’s news about your bookings, walks or messages. | bell | — |
| walletCustomer | No transactions yet | Your payments will appear here. | wallet | — |
| walletPetsitter | No earnings yet | Payments from completed walks will appear here. | wallet | — |
| bookingApplicantsEmpty | No pet sitters yet | Pet sitters who accept your request will appear here. | user | — |
| bookingPetSelector | No pets yet | Tap + to add one. | paw | — |
| bookingPlaceSelector | No saved places yet | Tap + to add one. | map-pin | — |
| customerPetsPrivate | Pet details are private | You’ll see this customer’s pets once a booking is confirmed. | lock | — |

> Copiado de Bubble (Main) el 2026-10-11.

### Reusable `appEmptyState`

Revisado visualmente el 2026-10-11 (ver "Refinamiento visual").

Estructura: `appEmptyState` (columna) → `grpEmptyStateContent` (columna,
centrado, máx. 320 px) → `grpEmptyStateStack` (columna, gap 8) → icono,
título, descripción, botón.

| Pieza | Diseño |
|---|---|
| Raíz | Padding 24 arriba/abajo, 16 a los lados. Sin fondo |
| `icnEmptyState` | Estilo *Empty*: círculo `#363636` (fuera de la paleta; DS-010) con icono gris `cnAXF`. 64×64 fijo (glifo de 24 px). 8 px extra debajo |
| `txtEmptyStateTitle` | mobileH5 (18 px, 600, blanco), centrado, interlineado 1.3, ancho completo |
| `txtEmptyStateDescription` | mobileSH5 (gris), 14 px, interlineado 1.5, centrado, máx. 280 px. Se oculta si está vacía |
| `btnEmptyStateAction` | mobilePrimary (naranja liso desde 2026-10-12), ancho al contenido, 8 px extra encima. Solo si `actionLabel` tiene texto |

Ritmo vertical: icono → 16 → título → 8 → descripción → 16 → botón.

**Variante compacta** (automática para `bookingPetSelector` y
`bookingPlaceSelector`, es decir, selectores de reserva y ubicaciones del
servicio): padding 12 arriba/abajo, sin icono, título a 16 px. Misma
tipografía y colores.

- Icono por conditionals según `iconKey`; si no coincide, queda
  `material regular info` con el mismo círculo.
- Estado interno **`actionRequested`** (sí/no). El botón lo pone en "sí"; la
  pantalla padre lo escucha con *Do when condition is true* (cada vez), lo
  vuelve a "no" y ejecuta su propia acción. El reusable no navega por su cuenta.

**Regla de posición:** el reusable controla su composición interna; la
pantalla controla dónde va. Si una pantalla necesita más espacio alrededor,
se envuelve la instancia en un grupo "slot" con ese padding y la misma
condición de visibilidad (ver Places y Notifications).

Iconos usados (todos ya existían en la app): `phosphor regular paw`,
`phosphor fill map-pin`, `phosphor regular calendar-blank`,
`phosphor bold calendar-blank` (para calendar-check),
`phosphor outlined chat-circle-slash`, `phosphor bold bell`,
`phosphor outlined wallet`, `feather user`, `feather lock`.

### Añadir un caso nuevo

1. Crear la opción en `emptyStateContext` (textos en inglés).
2. Si necesita otro icono, añadir un conditional en `icnEmptyState`.
3. Poner `appEmptyState` junto a la lista, oculto y con *collapse when hidden*.
4. Data source = la opción; conditional "visible" cuando la lista está vacía
   (y cargada, si la lista lo permite).
5. Si tiene acción: evento *Do when* en la pantalla padre sobre
   `actionRequested`.

## Dónde se usa

| # | Pantalla | Instancia | Lista vigilada | Opción(es) | Antes |
|---|---|---|---|---|---|
| 1 | `petList` | `appEmptyStatePets` | `listPets` | petsEmpty | Sustituye `grpPetsEmptyState` |
| 2 | `addressList` | `appEmptyStatePlaces` | `listPlaces` | placesEmpty | Sustituye `grpPlacesEmptyState` |
| 3 | `tabBookings` (cliente) | `appEmptyStateBookingsCustomer` | ShortList DogWalking Request | Pending / Booked / Past según la pestaña | Nuevo |
| 4 | `tabBookings` (petsitter) | `appEmptyStateBookingsPetsitter` | Confirm No + Confirm Yes | Pending / Booked / Past según la pestaña | Nuevo |
| 5 | `tabChats` | `appEmptyStateChats` | parámetro Conversations | chatsEmpty | Sustituye "Block Chat" |
| 6 | `notificationList` | `appEmptyStateNotifications` | Personal + All | notificationsEmpty | Sustituye "Block Notifications" |
| 7 | `walletOverview` | `appEmptyStateWallet` | Recent Activity | walletCustomer / walletPetsitter por rol | Nuevo |
| 8 | `walletTransactionList` | `appEmptyStateTransactions` | lista de transacciones | walletCustomer / walletPetsitter por rol | Sustituye el texto "No transactions yet" |
| 9 | `bookingDetailsCustomer` | `appEmptyStateApplicants` | SelectableList User | bookingApplicantsEmpty | Nuevo |
| 10 | `bookingStep2` | `appEmptyStatePetSelector`, `appEmptyStatePlaceSelector` | `selPets`, `selPlaces` | bookingPetSelector / bookingPlaceSelector | Nuevo |
| 11 | `bookingEditSheet` | `appEmptyStatePetSelector`, `appEmptyStatePlaceSelector` | `selPets`, `selPlaces` | bookingPetSelector / bookingPlaceSelector | Nuevo |
| 12 | `customerProfile` | `appEmptyStateCustomerPets` | SelectableList Pets | customerPetsPrivate (solo si quien mira es petsitter) | Nuevo |
| 13 | `petsitterServiceEdit` | `appEmptyStateServiceLocations` | SelectableList Locations | bookingPlaceSelector | Nuevo |

Reglas de visibilidad especiales:

- **Bookings petsitter**: no aparece si se muestra el aviso de configuración
  (disponibilidad inactiva o sin dirección).
- **Wallet overview**: no aparece si se muestra la bienvenida/onboarding de
  Wallet (`Block Wallet`, que no se tocó).
- **Notifications**: el grupo "FOR YOU" ahora se oculta cuando se ve el estado
  vacío nuevo (antes miraba el bloque viejo).
- **Applicants**: solo si la reserva ya está cargada.
- **customerProfile**: nunca muestra "No pets yet"; si la lista sale vacía para
  un petsitter (puede ser por privacidad) dice "Pet details are private".

## Acción "Book a walk"

Solo en **Bookings cliente → Pending**. Se comprobó el destino real: en Home,
tocar un servicio abre `bookingStep1` con el servicio *Dog Walking* (valor
fijo). El evento nuevo en `tabBookings` hace exactamente lo mismo, así que el
botón **empieza una reserva** y la etiqueta es correcta. (El *Page is loaded*
de Home que manda a Bookings solo aplica a petsitters.)

## Limitaciones de carga

- Con *loading status* (no parpadea): `petList`, `addressList`.
- Sin *loading status* (ShortList / SelectableList no lo exponen): el estado
  vacío puede verse un instante mientras carga en Bookings, Wallet, Wallet
  history, selectores de reserva, `customerProfile` y `petsitterServiceEdit`.
- Chats depende de la lista que le pasa la pantalla anterior (ya cargada).

## Diferencias de diseño respecto a lo anterior

- Icono de Places: antes casa (`feather home`), ahora `map-pin` (tabla
  aprobada). Notifications: antes imagen PNG de campana, ahora icono `bell`.
- Textos según la tabla aprobada: Places cambia ("No places yet. Add your first
  address…" → título + descripción nuevos); Pets, Chats, Notifications y
  Wallet history mantienen el mensaje y ganan título/descripción separados.

## Refinamiento visual (2026-10-11)

- Savepoint antes: **"Antes de refinamiento visual de appEmptyState"** — `1791728105039`
- Savepoint después: **"Después de refinamiento visual de appEmptyState (layout, tipografía, variante compacta, espaciado Places/Notifications)"** — `1791728286836`

Defectos de layout encontrados y corregidos:

| Elemento | Antes | Ahora |
|---|---|---|
| `appEmptyState` | Contenedor **fila** | Columna |
| `grpEmptyStateContent` | Fila, margen superior **-80 px**, ancho mín. 56 px | Columna, sin margen, ancho mín. 0 |
| Grupo interno (`Box_1_…`, apareció tras crear el reusable) | **Align to parent**: hijos superpuestos | Renombrado `grpEmptyStateStack`, columna con gap 8 |
| `icnEmptyState` | Caja fija de 30 px con 20 px de padding del estilo (sin sitio para el glifo) | 64×64 |
| Título y descripción | **Ancho fijo de 100 px**, alto mín. 36 px | Ancho completo (descripción máx. 280 px), alto al contenido, interlineado 1.3 / 1.5 |
| Botón | Alto fijo 44 px | Alto al contenido (padding del estilo) |

Espaciado restaurado con grupos "slot" (el padding vive en la pantalla, no en
el reusable):

- **Places** — `grpPlacesEmptyStateSlot` en `fgrpPlacesActions`: padding
  superior 8 + 24 del reusable = **32 px**, como el margen original. Primero
  en el orden, como antes.
- **Notifications** — `grpNotificationsEmptyStateSlot` en `Container`:
  padding 96 arriba / 16 abajo + 24 del reusable = **120 / 40 px**, como el
  bloque original.
- Ambos slots tienen la misma condición de visibilidad que su instancia y
  *collapse when hidden*, así que no ocupan espacio con la lista llena.
- Las otras 11 instancias no tenían márgenes propios que recuperar.

## No probado

No hubo prueba en dispositivo ni en el preview desde esta sesión: el preview
mobile exige sesión de colaborador de Bubble y un usuario de la app. Todo lo
anterior se verificó solo en el editor (estructura, propiedades e issue
checker). Ver la lista en `backlog.md`.
