# Backlog

Prioridad: 🔴 urgente · 🟠 importante · 🟡 cuando haya tiempo.

## 🟠 Marca: ajustar Bubble al Design System (ver `brand/bubble.md` y `brand/decisions.md`)

Acciones manuales en el editor de Bubble (la conexión no puede hacerlas):
- [ ] Styles → Style variables: comprobar que Primary = `#FF6600`, Primary
      contrast y Text = `#FFFFFF`, Background = `#161616`, Surface = `#2C2C2C`
      y que la fuente por defecto es Poppins. Anotar el resultado en
      `brand/bubble.md` (hoy figuran "Sin verificar").
- [ ] `Button Primary` → condición "is pressed": los dos colores del degradado a
      `Orange - Pressed - 100%` (DS-005).
- [ ] `Mobile Dropdown Default` y `Mobile Autocomplete Default` → condición "is focused": desactivar la
      sombra. `Web Popup Dark`: quitar la sombra (DS-004).
- [ ] `Mobile Button Secondary` → condición "is pressed": fondo a
      `Gray - Surface Pressed - 100%` (hoy azul; DS-008).

- [ ] Revisar en dispositivo que ningún botón principal corte la etiqueta con
      el texto nuevo de 700 18 px (DS-005).

Nombres de estilos (DS-016, propuestas pendientes de aprobación):
- [ ] Borrar `Web Popup Default Duplicate` (idéntico a `Web Popup Default`).
- [ ] Unificar `Mobile Icon Status` → `Icon Primary`, `Mobile Text Secondary`
      → `Mobile Text Secondary Centered` (con `#B3B3B3`) y
      `Mobile Text Title Large` → `Mobile Heading H5`, reasignando sus elementos.
- [ ] Borrar los 14 estilos sin uso (lista en `brand/bubble.md`).
- [ ] Etiquetas Age/Gender/Size de `bookingDetailsPetsitter`: de
      `Mobile Text Link Small` a `Mobile Text Label Secondary`.
- [ ] Definir un token de error y crear `Mobile Text Error` (hoy los errores
      usan `Text Body Small`).
- [ ] Revisar el nombre de `Mobile Text Label Strong` (métricas y propinas).
- [ ] Crear estilos para lo que hoy tiene formato a mano: etiquetas de estado,
      chips de especialidad, insignia de verificado, cajas de propina.

Fuera de los estilos globales (fase siguiente):
- [ ] Botones de la web (`Web Button Primary Small`, `Web Button Primary`): naranja
      liso y texto blanco en negrita de 18 px o más (DS-005, DS-007).
- [ ] Pestaña activa del Tab Bar en naranja (`primary`) en las 4 pestañas (DS-013).
- [ ] 🔴 `walk-tracking-customer`: la clave de Google Maps está escrita en el
      HTML. Las claves de Maps JS son públicas por diseño, pero hay que
      confirmar en Google Cloud que esté restringida por dominio (HTTP referrer)
      y solo a Maps JavaScript API; si no, rotarla.
- [ ] Título de los anuncios en notificaciones: "PetBoss 24" → "Petboss 24".
- [ ] Textos con `#8A8A8A` escrito a mano en `notificationList` → `#B3B3B3`.
- [ ] Números: comprobar si algún elemento usa DM Mono (ningún estilo la usa).
- [ ] Nombres técnicos con "Petboss" (B minúscula): nombre visible del proyecto de
      Google Cloud ("PetBoss 24" → "Petboss 24", el ID no cambia) y el plugin
      "Petboss Live Walk" cuando se cree.
- [ ] Página web `walk-tracking-customer`: rediseñar con la marca (hoy usa
      violeta `#635BFF`, fuente del sistema y fondo blanco) y cambiar el título de
      la pestaña "Valecans home" → "Petboss 24".

## 🟠 Onboarding pre-login (2026-10-13)

Plan aprobado por etapas (Next en pasos 1-2, cierre de sesión siempre a
`welcomeStep1`, volver desde el login, aviso de email sin verificar).
- [x] Etapa 1: reinicio de pila entre pasos (ver changelog).
- [ ] Probar en dispositivo (iOS y Android): pasos en cualquier orden sin
      historial creciente; Back de `authSignup` vuelve al paso de origen;
      atrás en un paso; animación de la transición; tocar el número "3" del
      círculo en el paso 1 (workflow vacío `cmrih`).
- [ ] Comprobar en el editor qué vista abre la app al iniciarse.
- [ ] Etapa 2: botones Next. Etapa 3: cierre de sesión unificado. Etapa 4:
      volver desde `authLogin`. Etapa 5: aviso de email sin verificar.

## 🔴 Seguridad

- [ ] **Verificar el webhook `checkout_session_completed`** contra Stripe
      (Retrieve Checkout Session por ID) antes de confirmar la reserva. Revisar
      lo mismo en `webhook_account_enabled` / `webhook_account_restricted`.
- [ ] **Probar las privacy rules** con un customer y un petsitter de prueba
      (checklist en `privacy-rules.md`).
- [ ] Rellenar `address.authorizedPetsitters` en reservas ya pagadas, si hay
      reservas activas.
- [x] Login/signup: verificación real de email (ver `auth-review.md`).
- [ ] **Desplegar a Live** el arreglo de verificación: la página
      `email_confirmation` en Live todavía marca el email como verificado sin
      código.

## 🔴 My Pets y My Places mobile (2026-10-11, ver `pets-places-mobile.md`)

- [ ] Probar en dispositivo: estados vacío/carga, borrado con Cancel, edad,
      ZIP con 0 inicial, safe area de `petList`, textos largos a 393 px.
- [ ] Pruebas por rol de la nueva regla de pets (petsitter no autorizado no
      puede buscar mascotas; sí ve las de una reserva Pending).
- [ ] Cambiar a mano la etiqueta "Gigant" → "Giant" (option set OS_Size).
- [ ] Decidir qué hacer con 6 direcciones y 3 mascotas sin `user` en test
      (ninguna reserva las usa). Revisar Live antes.
- [ ] Confirmar HEIC en Android/WebView.
- [ ] dgRequest: revisar auto-binding de las reglas Customer y Applied petsitter.
- [ ] Borrado: no hay aviso si el backend falla (Bubble no lo notifica). Valorar
      un tiempo máximo con mensaje.

## 🟠 Estados vacíos (2026-10-11, ver `empty-states.md`)

- [ ] Probar en dispositivo (393 px) las 13 ubicaciones, con cuenta customer y
      petsitter: posición, centrado, textos largos, iconos.
- [ ] Bookings cliente → Pending vacío → "Book a walk" abre `bookingStep1`.
- [ ] Revisar el parpadeo del estado vacío mientras cargan las ShortList /
      SelectableList (no exponen *is loading*).
- [x] Espacio superior de Notifications (120 px) y Places (32 px) recuperado
      con grupos slot (2026-10-11).
- [ ] Revisar el refinamiento visual en dispositivo a 320, 360, 375, 393 y
      430 px: círculo de 64 px, ritmo 16/8/16, variante compacta en los
      selectores y botón "Book a walk".
- [ ] Comprobar en dispositivo los iconos `phosphor bold calendar-blank`
      (usado para calendar-check) y `phosphor bold bell`.

## 🔴 Pets y Places (2026-10-10, ver `pets-places.md`)

- [ ] **Probar en test** con un customer y un petsitter:
  - crear y editar una mascota (IA en segundo plano, Cancel, sin género);
  - crear y editar una dirección (ZIP con 0 inicial, cambio de método de acceso, sin elegir dirección);
  - pagar una reserva, terminar el paseo y cancelar desde el chat (acceso del petsitter).
- [ ] **Confirmar** que los API workflows programados desde la app corren con
      Current User = quien los programó. Si no, ninguna acción se aplica (falla
      segura).
- [ ] **Decidir:** plazo de expiración del acceso del petsitter si el paseo no
      se cierra (no-show). Hoy no expira solo.
- [ ] **Decidir:** consentimiento para enviar datos de salud de la mascota a
      Gemini. Hoy se envían alergias, medicación, dieta y comportamiento.
- [ ] **Decidir:** qué pasa con la dirección por defecto al borrarla. Hoy queda
      sin default; no se asigna otra (sin cambios el 2026-10-11: no hay regla de negocio).
- [ ] **Autorizar** la migración de ZIP (procedimiento en `pets-places.md`).
      En test, 1 de 8 direcciones tiene el ZIP dañado.
- [ ] Rellenar `pets.authorizedPetsitters` en reservas ya pagadas y activas.
- [ ] Validar en backend que las mascotas y la dirección de una reserva sean
      del customer (la reserva se crea en el cliente).
- [ ] `bookingDetailsSheet`: el botón Cancel dispara dos workflows a la vez
      (`cnAAf` y `cnAAr`). El segundo hace refund.
- [x] Borrado: la hoja espera a que el backend confirme (2026-10-11).
- [ ] Data API: la regla *Owner* de address tiene `create/modify via API`
      activado. Revisar si el Data API está expuesto para address.

- [ ] Crear las carpetas de backend en el editor y mover los workflows (tabla
      en `conventions.md`).
- [ ] Decidir si se borran los 7 workflows de backend sin uso (marcados ⚪ en
      `conventions.md`).

## 🔴 URLs tras el renombrado (2026-10-09)

Actualizadas en test para apuntar a los nombres nuevos (sin cambiar nada más):

- [x] `bookingPaymentCard` `cmzBm`: `success_url` → `…/version-test/booking-payment-success/<id>`.
- [x] `petsitter-onboarding` `cmrTb` y `cmtvo`: `refresh_url` / `return_url` →
      `https://petboss24.com/petsitter-onboarding`.
- [x] Términos y privacidad: `authSignup` (`cmxtg0`, `cmxuL0`),
      `petsitterOnboarding` (`cmyHT`, `cmyHd`), HTML de `terms` (`cmxns`) y de
      `index` (`cmxrJ`) → `/terms` y `/privacy`.
- [x] Flujo antiguo oculto en `petsitterProfile` (`cmlqa`): `cancel_url` →
      `booking-payment-canceled` (su `success_url` apunta a una página que nunca
      existió; se dejó igual).

Pendiente (no se tocó a propósito):

- [ ] `success_url` de `cmzBm` sigue fija a `version-test`: en Live regresaría a
      test. Construirla con la URL base dinámica.
- [ ] Stripe Connect (`cmrTb`, `cmtvo`) usa el dominio Live sin
      `version-test`.
- [ ] Valores por defecto del API Connector (Stripe *Create Checkout Session* y
      *Create Onboarding Link*) aún con nombres viejos (`payment_success`,
      `transaction_mobile`, `onboarding_petsitter_web`). Solo se usan al
      inicializar.
- [ ] Fuera de Bubble (al desplegar a Live): URLs de privacidad y términos en
      App Store Connect, Google Play Console y Stripe.
- [ ] Opcional: en `404`, redirigir los nombres viejos a los nuevos.

## 🟠 Funcionalidad

- [ ] **Stripe setup mode en wallet:** el usuario debe inicializar en el API
      Connector:
  1. *Stripe - Create Customer* → Initialize call → copiar el `cus_...`.
  2. *Stripe - Create Setup Session* → poner ese id en `customer` → Initialize.
  3. Confirmar que ambas usan la clave de prueba.
  Luego: terminar el workflow del botón "Add Payment Method".
- [ ] Hacer que Create Checkout Session envíe `customer` para usar la tarjeta
      guardada.
- [x] Login/signup: bugs y plan de `auth-review.md`.
- [ ] Probar login/signup/verificación en dispositivo (checklist en
      `auth-review.md`).
- [ ] Login/signup: diseño y textos pendientes (`auth-review.md`).
- [x] Password policy activada en Settings → General (8, número, mayúscula,
      no alfanumérico).
- [x] Arreglar `authResetPassword` y requisitos en `settingsPrivacySecurity`.
- [ ] Probar en dispositivo: requisitos y ojito (signup, login), reset y cambio
      de contraseña en Settings.
- [ ] Notificaciones: título con remitente, marcar como leída, abrir la reserva.
- [ ] Habilitar `petboss24.com` en *Allowed domains* del entorno cloud para
      poder probar con navegador desde las sesiones.

## 🟡 Modelo de datos y limpieza

- [ ] **Seguridad:** `petsitterOnboardingSubmitted` tiene un botón *Done* que
      cambia el `osRole` del usuario a Petsitter desde el cliente, sin
      verificación. Hoy ninguna pantalla navega ahí, pero conviene borrarla o
      moverlo a backend. `petsitterOnboardingSubmitted` y
      `petsitterOnboardingPending` son pantallas duplicadas.
- [ ] Páginas sin uso: `auth-email-confirmation`,
      `petsitter-onboarding-complete`, `booking-payment-canceled`. Decidir si se
      borran.
- [ ] Títulos visibles de vistas copiados o mal escritos (p. ej. "Date &
      Duration" en `bookingStep2`/`bookingStep3`, "Forgot Password" en
      `authForgotPasswordSent`, "reset_password").
- [ ] Reusables con nombres poco claros: `bookingDetailsSheet`, `bookingEditSheet`,
      `addressDetailsSheet`, `info`, `photo`, `charge`.

- [ ] Renombrar option sets a camelCase (checklist en `conventions.md`).
- [ ] Campo `petsitter` en `dgRequest`; un solo `bookingStatus`.
- [ ] Entidad `walk` por fecha para reservas recurrentes.
- [ ] `isPetsitter` + `activeMode` en vez de un `osRole` mutable.
- [ ] Reseñas bidireccionales con referencia a la reserva.
- [ ] `seenBy` en notificaciones broadcast.
- [ ] `User.profile`.
- [ ] Congelar `price`, `tipAmount`, `platformFee` en la reserva.
- [ ] Corregir tipos (teléfono, zipCode → `zipCodeText` creado, falta migrar, phoneValid, experienceInMonths).
- [ ] Borrar `tracking` (legacy), `myTables`, option set `myTable`.
- [ ] Corregir firstName/lastName invertidos en `trustedContact`.
- [ ] Auditar uso real de plugins; desinstalar `Make` si no se usa.
- [ ] Corregir los issues pre-existentes que reporta Bubble (`workflows.md`).
