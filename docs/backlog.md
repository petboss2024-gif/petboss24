# Backlog

Prioridad: 🔴 urgente · 🟠 importante · 🟡 cuando haya tiempo.

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
- [ ] Decidir si se recupera el espacio superior de Notifications (antes
      120 px) y Places (antes 32 px); la instancia no admite márgenes.
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
