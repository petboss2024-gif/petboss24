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

- Tipo de contenido: `emptyStateContext`. Columna centrada, máx. 320 px.
- `icnEmptyState`: estilo *Empty*, 30×30. Icono por conditionals según
  `iconKey`; si no coincide, queda `material regular info`.
- `txtEmptyStateTitle` (mobileH5, centrado) y `txtEmptyStateDescription`
  (mobileSH5, centrado, se oculta si está vacía).
- `btnEmptyStateAction` (mobilePrimary): solo visible si `actionLabel` no está
  vacío.
- Estado interno **`actionRequested`** (sí/no). El botón lo pone en "sí"; la
  pantalla padre lo escucha con *Do when condition is true* (cada vez), lo
  vuelve a "no" y ejecuta su propia acción. El reusable no navega por su cuenta.

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

- Las instancias no permiten márgenes/padding propios: se perdió el
  `margin-top: 32px` de Places y el `padding-top: 120px` de Notifications
  (ahora usan el padding del reusable, 24 px).
- Icono de Places: antes casa (`feather home`), ahora `map-pin` (tabla
  aprobada). Notifications: antes imagen PNG de campana, ahora icono `bell`.
- Textos según la tabla aprobada: Places cambia ("No places yet. Add your first
  address…" → título + descripción nuevos); Pets, Chats, Notifications y
  Wallet history mantienen el mensaje y ganan título/descripción separados.

## No probado

No hubo prueba en dispositivo ni en el preview desde esta sesión. Ver la lista
en `backlog.md`.
