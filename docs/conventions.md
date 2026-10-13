# Convenciones

## Nombres: lowerCamelCase

Primera palabra en minúscula, las siguientes con mayúscula inicial, sin
espacios, guiones bajos ni emojis. Prefijo `os` para campos y option sets que
referencian un option set; `nav` para navegación.

Ejemplos: `osRole`, `navStatusRequest`, `listOfPets`, `seenCustomer`.

Solo se cambian **nombres visibles** (Bubble mantiene la clave interna, así que
workflows y datos no se rompen). Las **páginas web** usan kebab-case porque su
nombre es la URL (ver "Páginas web y vistas móviles"). Los **estilos** usan
Title Case con espacios (ver "Estilos de Bubble").

## Estado del renombrado

### Hecho (2026-10-09)

Campos de tipos de datos:

| Tipo | Antes → Ahora |
|---|---|
| User | OS_role → `osRole`; OS_statusVerificationPetsitter → `osStatusVerificationPetsitter` |
| address | OS_accessMethod → `osAccessMethod`; User → `user` |
| profile | OS_gender → `osGender` |
| dgDisponibility | Services → `services` |
| dgRequest | nav_schedule → `navSchedule`; nav_StatusRequest → `navStatusRequest`; Addtime → `addTime`; os_tip → `osTip`; os_dgType → `osDgType`; listOfPetses → `listOfPets` |
| dgSubServices | os_dgtype → `osDgType` |
| requestAddTime | OS_addTime → `osAddTime` |
| feedback | os_feedbackCategory → `osFeedbackCategory`; Stars → `stars` |
| messages | message_date → `messageDate`; seen customer → `seenCustomer`; seen petsitter → `seenPetsitter` |
| support | type_support → `typeSupport` |
| transaction | paymenStatus → `paymentStatus` |

Reusables y vistas: ver "Páginas web y vistas móviles" → Reusables.

### No posible por herramienta

- **Device** (tipo nativo de Bubble).
- **Option sets y sus atributos**: el conector de Bubble no permite
  renombrarlos. Hacerlo a mano en *Data → Option sets* (es seguro).

### Checklist manual de option sets

- [ ] `➡️ Nav_General` → `navGeneral`
- [ ] `➡️ Nav_Schedule` → `navSchedule`
- [ ] `➡️ Nav_Petsitter` → `navPetsitter`
- [ ] `➡️ Nav_Logged_Out` → `navLoggedOut`
- [ ] `➡️Nav_Help_Support` → `navHelpSupport`
- [ ] `➡️Nav_Status_Request` → `navStatusRequest`
- [ ] `➡️ Nav_About_Customer` → `navAboutCustomer`
- [ ] `➡️ Nav_Service_DogWalking` → `navServiceDogWalking`
- [ ] `➡️ Sub_Nav_Profile` → `subNavProfile`
- [ ] `Sub_Status_Booked` / `Sub_Status_Pending` / `Sub_Status_Past` → `subStatusBooked` / `subStatusPending` / `subStatusPast`
- [ ] `OS_Role`, `OS_Size`, `OS_Gender`, `OS_ID`, `OS_Rates` → `osRole`, `osSize`, `osGender`, `osId`, `osRates`
- [ ] `OS_Markers`, `OS_Markers_Type` → `osMarkers`, `osMarkersType`
- [ ] `OS_Metrics`, `OS_Onb_Pet`, `OS_Sort_By`, `OS_Onboarding` → `osMetrics`, `osOnbPet`, `osSortBy`, `osOnboarding`
- [ ] `OS_Review_Pet`, `OS_Review_Petsitter`, `OS_Review_Customer` → `osReviewPet`, `osReviewPetsitter`, `osReviewCustomer`
- [ ] `OS_Social_Media`, `OS_Spayed/Neutered`, `OS_Info_Calendar` → `osSocialMedia`, `osSpayedNeutered`, `osInfoCalendar`
- [ ] `OS_Filter_By_Service`, `OS_Filter_Date_Metrics` → `osFilterByService`, `osFilterDateMetrics`
- [ ] `OS_Frequently_Asked_Questions` → `osFrequentlyAskedQuestions`
- [ ] `OS_Status_Verification_Petsitter` → `osStatusVerificationPetsitter`
- [ ] `OS_Visible_Pet_Invormation` → `osVisiblePetInformation`
- [ ] `What_Our_Users_Say` → `whatOurUsersSay`
- [ ] `Get_the_Petboss_24_App` → `getThePetboss24App`
- [ ] `carrouselInfo` → `carouselInfo`

Atributos (ejemplos): `Description` → `description`, `Image` → `image`,
`Icon_Selected` → `iconSelected`, `image no selected` → `imageNoSelected`,
`time/min` → `timeMin`, `Coming soon` → `comingSoon`, `Img Active` →
`imgActive`, `Sub text` → `subText`, `TYPE` → `type`, `URL` → `url`,
`CTA` → `cta`, `ID` → `id`.

## Páginas web y vistas móviles

Aplicado el 2026-10-09 en Main (test). Savepoint: **"Antes de renombrar páginas
web y vistas móviles"** (`1791565293480`).

### Páginas web (son la URL)

- **kebab-case en minúsculas**, en inglés: solo `a-z`, `0-9` y guiones entre
  palabras. Sin guiones bajos ni sufijos técnicos (`_web`, `_url`).
  Bubble solo admite minúsculas y Google recomienda guiones en las URL.
- Forma `[módulo-]tema[-rol][-resultado]`. Una página que se muestra dentro de
  una vista usa **las mismas palabras** que esa vista (`bookingPayment` ↔
  `booking-payment`).
- Páginas públicas: una palabra corta (`privacy`, `terms`).
- Páginas de Bubble que no se tocan: `index`, `404`, `reset_pw`.

### Vistas móviles

- **lowerCamelCase** en inglés, solo letras y números.
- Forma **módulo + Tema + Tipo + Rol**, con el módulo primero para que cada
  familia quede junta en el editor.
- Módulos: `auth`, `welcome`, `tab`, `booking`, `walk`, `chat`, `account`,
  `settings`, `notification`, `pet`, `address`, `wallet`, `petsitter`,
  `customer`.
- Tipos: `Step1..N` (pantallas numeradas: pasos de un formulario o slides),
  `List`, `Details`, `Edit`, `Menu`, `Overview`, `Profile`.
- Resultados: `Sent`, `Submitted`, `Pending`, `Success`, `Canceled`
  (inglés de EE. UU.).
- Rol al final (`…Customer` / `…Petsitter`) cuando la pantalla existe para los
  dos roles. Rol al inicio (`petsitter…`, `customer…`) cuando la pantalla trata
  de esa persona o es la configuración propia de ese rol.
- Pestañas: `tab` + nombre de la pestaña.
- Palabras completas, sin abreviaturas (`onb`, `nav`, `dg`). Nombres únicos y
  distintos de tipos de datos, option sets y reusables.
- `updateApp` se deja igual (vista de actualización obligatoria de Bubble).

### Reusables

Misma forma que las vistas (**módulo + Tema + Pieza**). Módulos extra: `web`
(piezas de las páginas web) y `app` (piezas genéricas). Piezas: `Sheet` (hoja
que sube desde abajo), `Card`, `Banner`, `Callout`, `Overlay`, `Header`,
`Footer`. Savepoint: **"Antes de renombrar reusables"** (`1791566694459`).

| Antes | Ahora | Qué es |
|---|---|---|
| header | `webHeader` | Encabezado de la web |
| footer | `webFooter` | Pie de la web |
| offlineBanner | `appOfflineBanner` | Aviso "sin conexión" |
| customerPayment | `bookingPaymentCard` | Bloque de pago/confirmación de una reserva |
| info | `appInfoCallout` | Recuadro informativo |
| aboutUser | `petsitterProfileCard` | Tarjeta de perfil del petsitter |
| addressAddEdit | `addressEditSheet` | Crear/editar dirección |
| addressDetail | `addressDetailsSheet` | Detalle de una dirección |
| dgRequestDetails | `bookingDetailsSheet` | Detalle de una reserva (editar/cancelar) |
| dgRequestEdit | `bookingEditSheet` | Editar una reserva |
| petAddEdit | `petEditSheet` | Crear/editar mascota |
| photo | `appPhotoPickerSheet` | Tomar o elegir foto |
| review | `bookingReviewSheet` | Calificar después del paseo |
| verificationCheck | `authVerificationSheet` | Código de verificación de email |
| deleteConfirmation | `addressDeleteSheet` | Confirmar borrado de dirección |
| charge | `appLoadingOverlay` | Pantalla de carga |
| — (nuevo 2026-10-11) | `appEmptyState` | Estado vacío de listas (ver `empty-states.md`) |

### Tabla de cambios

| Antes | Ahora | Qué es |
|---|---|---|
| login | `authLogin` | Iniciar sesión |
| signup | `authSignup` | Crear cuenta (siempre Customer) |
| forgotPassword | `authForgotPassword` | Pedir enlace de recuperación |
| checkYourEmail | `authForgotPasswordSent` | "Revisa tu correo" tras pedir el enlace |
| resetPassword | `authResetPassword` | Poner contraseña nueva |
| onbLoginStep1/2/3 | `welcomeStep1/2/3` | Slides de bienvenida (sin sesión) |
| onbPetsitter | `petsitterOnboarding` | Solicitud para ser petsitter + pagos Stripe |
| onbConfirmationPetsitter | `petsitterOnboardingSubmitted` | "Estamos confirmando tus datos" |
| onbPendingPetsitter | `petsitterOnboardingPending` | "Estamos revisando tus datos" |
| navHome | `tabHome` | Pestaña Inicio |
| navBookings | `tabBookings` | Pestaña Reservas (ambos roles) |
| navChat | `tabChats` | Pestaña Chats |
| navAccount | `tabAccount` | Pestaña Cuenta |
| profile | `accountProfileEdit` | Editar mi perfil |
| settings | `settingsMenu` | Menú de ajustes |
| settingsHelpSupport, settingsNotifications, settingsPrivacySecurity, settingsFeedback | sin cambio | Ya cumplían |
| notifications | `notificationList` | Bandeja de notificaciones |
| requestWalkStep1/2/3 | `bookingStep1/2/3` | Pasos para reservar un paseo |
| customerBookingDetails | `bookingDetailsCustomer` | Una reserva vista por el customer |
| dgPetsitterAccept | `bookingDetailsPetsitter` | Una solicitud vista por el petsitter (postularse) |
| confirmPayment | `bookingPayment` | Resumen antes de pagar (muestra `booking-payment`) |
| chatMessages | sin cambio | Mensajes de una conversación |
| trackingPetsitter | `walkTrackingPetsitter` | Paseo del petsitter (muestra `walk-tracking-petsitter`) |
| trackingCustomer | `walkTrackingCustomer` | Mapa en vivo del customer (muestra `walk-tracking-customer`) |
| pets | `petList` | Mis mascotas |
| aboutPet | `petDetails` | Detalle de una mascota |
| address | `addressList` | Mis direcciones |
| aboutPetsitter | `petsitterProfile` | Perfil de un petsitter (visto por customers) |
| aboutCustomer | `customerProfile` | Perfil de un customer (visto por el petsitter) |
| services | `petsitterServiceList` | Servicios que ofrece el petsitter |
| service | `petsitterServiceEdit` | Precios y radio de un servicio |
| wallet | `walletOverview` | Billetera |
| walletHistory | `walletTransactionList` | Historial de transacciones |
| updateApp | sin cambio | Actualización obligatoria (Bubble) |
| tracking-petsitter (web) | `walk-tracking-petsitter` | GPS del paseo (dentro de la vista) |
| tracking-customer (web) | `walk-tracking-customer` | Mapa del customer (dentro de la vista) |
| email_confirmation (web) | `auth-email-confirmation` | Página antigua "Confirmed account!" (sin uso) |
| confirm_payment_web (web) | `booking-payment` | Resumen antes de Stripe (dentro de la vista) |
| onboarding_petsitter_web (web) | `petsitter-onboarding` | Alta de pagos Stripe Connect (dentro de la vista) |
| welcome_petboss_web (web) | `petsitter-onboarding-complete` | "Ready to Get Paid!" (sin uso) |
| payment_success (web) | `booking-payment-success` | Regreso de Stripe tras pagar |
| cancel_url (web) | `booking-payment-canceled` | Regreso al cancelar (sin uso: hoy se usa deep link) |
| policy_privacy (web) | `privacy` | Política de privacidad |
| terms_conditions (web) | `terms` | Términos y condiciones |
| index, 404, reset_pw (web) | sin cambio | Páginas de Bubble |

Las referencias por ID (WebViews, "Go to view", "Go to page", pestañas) se
actualizan solas. Las URL escritas como texto se actualizaron a mano (ver `backlog.md`).

## Estilos de Bubble

Desde el 2026-10-13 (DS-016; sustituye la regla `{plataforma}{Elemento}{Variante}`
de DS-015).

**Formato:** `[Plataforma] [Componente] [Variante] [Rol o tamaño]`, en Title
Case con espacios, solo con los segmentos que aportan información.
Ejemplos: `Button Primary`, `Mobile Heading H5`, `Web Navigation Item`,
`Mobile Card Default`, `Mobile Button Destructive`.

1. **Nombrar por función, no por aspecto ni por historia.** Antes de nombrar,
   mirar dónde se usa el estilo. `Mobile Tab Segment` (no "botón oscuro"),
   `Mobile Button Tertiary` (no "cancelar", porque también guarda y elige foto).
2. **Plataforma:** `Mobile` si solo lo usa la app (incluidas las páginas
   Native Mobile), `Web` si solo lo usa el sitio. Sin prefijo si lo usan las
   dos. Un estilo de una plataforma que pasa a usarse en la otra pierde el
   prefijo.
3. **Componente:** el nombre de diseño (`Heading`, `Text`, `Button`, `Input`,
   `Dropdown`, `Autocomplete`, `Toggle`, `Container`, `Card`, `List`,
   `List Item`, `Selectable List`, `Selectable Item`, `Sheet`, `Popup`,
   `Floating …`, `App Bar`, `Tab Bar Item`, `Tab Segment`, `Icon`,
   `Divider`, `Map`, `Web View`, `Page`, `Progress Step`).
4. **Variante y rol:** `Primary`, `Secondary`, `Tertiary`, `Destructive`,
   `Default`, `Strong`, `Accent`, `Link`, `Caption`, `Label`, `Title`… El
   tamaño (`Small`, `Large`) solo si hay dos del mismo rol. Los títulos usan el
   nivel (`H1`–`H5`).
5. Nada de colores en el nombre (`Blue`, `Orange`) ni de pantallas (`Login`,
   `Profile`), salvo excepciones de marca de terceros (`Mobile Button Apple Sign In`).
6. Un estilo sin uso no se marca en el nombre: se anota en el catálogo como
   candidato a borrar.
7. Renombrar es solo cambiar el nombre: no se tocan colores, tamaños ni
   condiciones en el mismo paso. Los elementos guardan el id del estilo, así
   que renombrar no cambia ningún elemento.

Familias del catálogo: Tipografía, Botones, Formularios, Superficies,
Navegación, Elementos flotantes, Iconografía y Otros.

Catálogo completo (nombre, nombre anterior, plataforma, usos, función,
duplicados y estilos sin uso):
[`brand/bubble.md`](../brand/bubble.md#catálogo-de-estilos).

## Colores y tipografía

La marca vive en [`brand/`](../brand/README.md) y no se copia aquí: valores
en `brand/tokens.json`, reglas en `brand/README.md`, su implementación en
Bubble en `brand/bubble.md` y las decisiones en `brand/decisions.md`.

## Workflows de backend (2026-10-10)

**Nombre:** `area-accion[-objeto]` en kebab-case, sin espacios, tanto en API
workflows como en custom events de backend. Ejemplos: `pet-delete`,
`chat-send-message`. Las llamadas internas apuntan por ID, así que renombrar un
workflow **no expuesto** no rompe nada. Un workflow **expuesto** no se renombra:
su nombre es la URL que usa el servicio externo.

**Carpetas:** la herramienta de Bubble no puede crearlas, así que se crean a mano
en el editor (Backend workflows → panel izquierdo → *New folder*, y arrastrar
cada workflow).

| Carpeta | Workflows |
|---|---|
| **Auth** | `auth-send-verification-code`, `auth-verify-email-code`, `auth-update-credentials` ⚪ |
| **Bookings** | `booking-create` ⚪, `booking-create-with-dates` ⚪ |
| **Availability** | `availability-add-dates` ⚪, `availability-remove-dates` ⚪ |
| **Payments (Stripe)** | `payment-create-checkout`, `checkout_session_completed` 🌐, `webhook_account_enabled` 🌐, `webhook_account_restricted` 🌐 |
| **Pets** | `pet-generate-ai-data`, `pet-delete` |
| **Places** | `place-create` ⚪, `place-delete`, `place-set-default`, `place-revoke-petsitter-access` |
| **Chat** | `chat-create`, `chat-send-message` |
| **Notifications** | `notification-send` |
| **Reviews** | `review-create` |
| **AI** | `ai-create-response` ⚪ |

🌐 = expuesto (webhook de Stripe): nombre sin cambiar. ⚪ = sin llamadores en la
app y no expuesto. Candidato a borrar, pendiente de confirmación.

| Nombre anterior | Nombre nuevo |
|---|---|
| Add a list range of dates | `availability-add-dates` |
| Remove a list range of dates_copy | `availability-remove-dates` |
| Update credentials | `auth-update-credentials` |
| create-request-date | `booking-create-with-dates` |
| create-request | `booking-create` |
| create-transaction | `payment-create-checkout` |
| create-chat | `chat-create` |
| create-message | `chat-send-message` |
| notifications | `notification-send` |
| create-ai-response | `ai-create-response` |
| create-review | `review-create` |
| create-address | `place-create` |
| sendVerificationCode | `auth-send-verification-code` |
| verifyEmailCode | `auth-verify-email-code` |
