# Modelo de datos

> Estado: versión **Main (test)**, revisado el 2026-10-09.
> Los nombres son los **visibles** en el editor. Entre paréntesis, la clave
> interna cuando difiere (Bubble no permite cambiar claves internas).

## Resumen

- **24 tipos de datos**, **48 option sets**, tipos de respuesta de API
  (Stripe, Twilio, Google Maps).
- Las relaciones principales giran alrededor de **`dgRequest`** (la reserva).

```
User ──< pets
  │  ──< address (locations)
  │  ──< trustedContact
  │  ──  profile  (relación solo por Creator; no hay User.profile)
  │
  └──< dgRequest (request) ── address
           │── listOfPets ─> pets
           │── listOfPetsitters ─> User
           │── conversation ─> conversations ──< messages
           │── trackingNew ─> trackingNew ──< updateLocation
           │── reviewsCustomer ─> review
           └── requestAddTime
  transaction (__payments) ── dgRequest, customer, petsitter
  dgDisponibility (scheduled) ──< dgSubServices (dog_walking_options)
```

## Tipos de datos

### Usuarios y perfiles

**User**
`email` · `osRole` (Admin/Customer/Petsitter) · `osStatusVerificationPetsitter` ·
`stripeAccountId` · `sellerAccountId` · `stripeCustomerId` *(nuevo, para Stripe
setup mode)* · `playerId` · `googleAccount` · `googleId` · `emailConfirmation` ·
`tokenConfirmation` · `tokenExpiresAt` · `verificationAttempts` ·
`pendingVerificationCode` *(nuevos — ver auth-review.md)*

**profile** — `firstName`, `lastName`, `birthdate`, `about`, `details`,
`profilePicture`, `osGender`, `country`, `countryCode`, `dialCode`,
`phoneNumberRaw`, `phoneNumberFormatted`, `phoneStatus`, `phoneValid`,
`experienceInMonths`

**trustedContact** — `firstName` (`apellido_text`), `lastName` (`nombre_text`),
`contact`, `email`, `relationship`, `user`

**Device** (tipo nativo de Bubble) — Current user, Type, Push notification token

**favorites** — `customer`, `favoritePetsitter`

### Mascotas y direcciones

**pets** — `name`, `breed`, `birthdate`, `gender`, `size`, `spayedNeutered`,
`photo`, `personality`, `behavioralTraits`, `diet`, `allergies`, `medication`,
`additionalNotes`, `aIBasicinfo`, `aIHealthInformation`,
`aIAdditionalInformation`, `delete`, `user`

**address** (`locations`) — `addressName`, `geographicAddress`, `apartment`,
`city`, `state`, `zipCode`, `osAccessMethod`, `accessCode`, `lockboxCode`,
`otherAccess`, `accessInstructions`, `homeAccess`, `image`, `setAsDefault`,
`delete`, `user`, `authorizedPetsitters` *(nuevo — ver privacy-rules.md)*

### Reservas y servicios

**dgRequest** (`request`) — `customer`, `listOfPets`, `address`,
`geographicAddress`, `services`, `osDgType`, `listOfDates`, `hourFrom`,
`hourTo`, `duration`, `navSchedule`, `osTip`, `specialInstructions`,
`listOfPetsitters`, `petsitterConfirmed`, `navStatusRequest`,
`subStatusBooked`, `subStatusPending`, `subStatusPast`, `active`, `canceled`,
`userCanceled`, `conversation`, `trackingNew`, `requestAddTime`, `addTime`,
`reviewsCustomer`

**requestAddTime** — `dgRequest`, `customer`, `petsitter`, `osAddTime`,
`accepted`, `created`

**dgDisponibility** (`scheduled`) — `petsitter`, `services`, `dgSubServices`,
`address`, `mileageRestriction`, `active`, `scheduled`, `excludedDatesAllDay`,
`excludedDatesHours`

**dgSubServices** (`dog_walking_options`) — `dgDisponibility`, `osDgType`,
`duration`, `price`, `user`

### Tracking GPS

**trackingNew** (`trackingtest1`) — `active`, `addressStart`, `addressEnd`,
`timeStart`, `timeEnd`, `miles`, `updateLocation` (lista), `customer` *(nuevo)*

**updateLocation** — `json` (texto con el punto GPS), `customer` *(nuevo)*

**tracking** (`test`) — **legacy**, reemplazado por `trackingNew`. Solo visible
para Admin. Candidato a borrar.

### Mensajería y notificaciones

**conversations** (`converstion`) — `dgRequest`, `participants`, `lastUpdated`

**messages** — `conversation`, `content`, `image`, `messageDate`,
`participants`, `seenCustomer`, `seenPetsitter`

**notifications** — `sender`, `recipient`, `typeUser`, `body`, `dgRequest`,
`geographicAddress`, `seen`, `all`

### Pagos

**transaction** (`__payments`) — `dgRequest`, `customer`, `petsitter`,
`amount` (centavos), `platformFee` (centavos), `checkoutSessionId`,
`paymentIntentId`, `paymentIntentStatus`, `chargeId` (`stripe_customer_id_text`),
`customerStripeId`, `stripeAccountId`, `sellerAccountId`, `idTransaction`,
`paymentStatus` (`subscription_status_text`), `url`, `errorStatusCode`,
`errorBody`, `errorStatusMessage`, `returnedAnError`

**subscription** (`payment`) — `petsitter`, `stripeCustomerId`,
`subscriptionId`, `subscriptionStatus`, `subscriptionStart`, `subscriptionEnd`

### Reseñas, soporte y otros

**review** (`__reviews_customer`) — `customer`, `petsitter`, `userReview`,
`pet`, `rating`, `description`

**feedback** (`feedback1`) — `description`, `osFeedbackCategory`, `stars`

**support** (`feedback`) — `message`, `ticket`, `typeSupport`

**aiResponses** — `prompt`, `tokens` *(no guarda la respuesta)*

**myTables** — `email` *(sobra de pruebas)*

## Option sets

| Grupo | Option sets |
|---|---|
| Navegación | Nav_General, Nav_Schedule, Nav_Petsitter, Nav_Logged_Out, Nav_Help_Support, Nav_About_Customer, Nav_Service_DogWalking, Sub_Nav_Profile |
| Estados de reserva | Nav_Status_Request (Booked/Pending/Past), Sub_Status_Booked, Sub_Status_Pending, Sub_Status_Past, bookedStatus |
| Servicios | services, dgType (Quick & Ready / Boss in Action / Big Boss), addTime, tip |
| Mascotas | OS_Size, OS_Gender, OS_Spayed/Neutered, OS_Onb_Pet, OS_Visible_Pet_Invormation |
| Tracking | OS_Markers (Pee/Poop/Water/Emergency), OS_Markers_Type |
| Reseñas | OS_Rates, OS_Review_Pet, OS_Review_Petsitter, OS_Review_Customer, feedback, feedbackCategory |
| Roles | OS_Role, OS_Status_Verification_Petsitter, OS_ID, badges |
| Contenido / marketing | carrouselInfo, howItWorks, forPetOwners, forPetSitters, What_Our_Users_Say, Get_the_Petboss_24_App, OS_Frequently_Asked_Questions, OS_Social_Media, OS_Onboarding |
| Otros | OS_Metrics, OS_Filter_Date_Metrics, OS_Filter_By_Service, OS_Sort_By, OS_Info_Calendar, accessMethod, myTable |

> Los option sets **aún no están en camelCase** — ver `conventions.md`.

## Problemas conocidos del modelo (pendientes, no resueltos)

1. **Reservas recurrentes:** `dgRequest` guarda varias fechas pero un solo
   `trackingNew`, una transaction y una reseña. Propuesta: entidad `walk` por
   fecha.
2. **No hay petsitter asignado explícito:** solo `listOfPetsitters` +
   `petsitterConfirmed`. Tras el pago la lista queda con un solo petsitter.
   Propuesta: campo `petsitter`.
3. **Estado repartido en 7 campos** (`navStatusRequest`, sub-statuses,
   `active`, `canceled`, `petsitterConfirmed`). Propuesta: un option set
   `bookingStatus`.
4. **Rol único y mutable:** el switch de modo sobrescribe `osRole`. Propuesta:
   `isPetsitter` + `activeMode`.
5. **Reseñas no bidireccionales** y sin referencia a la reserva.
6. **`seen` compartido** en notificaciones broadcast.
7. **User ↔ profile** sin campo de relación.
8. **Precio no congelado** en la reserva.
9. **Tipos incorrectos:** teléfono y `zipCode` como número, `phoneValid` texto,
   `experienceInMonths` fecha.
10. **Duplicados/confusos:** `sellerAccountId` vs `stripeAccountId`;
    `chargeId` guardado en `stripe_customer_id_text`; push vía `playerId` y
    `Device`.
11. **Sobras:** `tracking` (legacy), `myTables`, option set `myTable`.
12. **Nombres invertidos:** `trustedContact` firstName/lastName.
