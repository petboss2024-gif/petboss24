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

## 🔴 URLs por actualizar tras el renombrado (2026-10-09)

Las páginas web cambiaron de nombre; estas URL están escritas como texto y
siguen apuntando al nombre viejo (darán 404). Las referencias por ID no se
rompen.

- [ ] **Pago de reservas:** reusable `customerPayment`, acción `cmzBm`
      (Schedule `create-transaction`), `success_url` =
      `https://petboss24.com/version-test/payment_success/…` →
      `booking-payment-success`. Además está fija a `version-test`: en Live
      regresaría a test. Construirla con la URL base dinámica.
- [ ] **Stripe Connect:** página `petsitter-onboarding`, acciones `cmrTb` y
      `cmtvo` (Create Onboarding Link), `refresh_url` / `return_url` =
      `https://petboss24.com/onboarding_petsitter_web` → `petsitter-onboarding`.
- [ ] **Términos y privacidad:** vista `authSignup` (`cmxtg0`, `cmxuL0`), vista
      `petsitterOnboarding` (`cmyHT`, `cmyHd`), HTML de la página `terms`
      (`cmxns`, sección 16) y HTML oculto de `index` (`cmxrJ`) →
      `https://petboss24.com/terms` y `/privacy`.
- [ ] **Valores por defecto del API Connector** (Stripe): *Create Checkout
      Session* (`success_url` con `payment_success`, `cancel_url` con
      `transaction_mobile`, que no existe) y *Create Onboarding Link*
      (`refresh_url` con `onboarding_petsitter_web`). Solo se usan al
      inicializar, pero conviene corregirlos.
- [ ] **Flujo antiguo oculto** en `petsitterProfile` (acción `cmlqa`): usa
      `success_url` y `cancel_url` de páginas que no existen. Borrar o
      reconectar.
- [ ] **Fuera de Bubble** (al desplegar a Live): URLs de privacidad y términos
      en App Store Connect, Google Play Console y Stripe.
- [ ] Opcional: en la página `404`, redirigir los nombres viejos
      (`policy_privacy`, `terms_conditions`, `payment_success`…) a los nuevos,
      para links ya publicados.

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
- [ ] Reusables con nombres poco claros: `dgRequestDetails`, `dgRequestEdit`,
      `addressDetail`, `info`, `photo`, `charge`.

- [ ] Renombrar option sets a camelCase (checklist en `conventions.md`).
- [ ] Campo `petsitter` en `dgRequest`; un solo `bookingStatus`.
- [ ] Entidad `walk` por fecha para reservas recurrentes.
- [ ] `isPetsitter` + `activeMode` en vez de un `osRole` mutable.
- [ ] Reseñas bidireccionales con referencia a la reserva.
- [ ] `seenBy` en notificaciones broadcast.
- [ ] `User.profile`.
- [ ] Congelar `price`, `tipAmount`, `platformFee` en la reserva.
- [ ] Corregir tipos (teléfono, zipCode, phoneValid, experienceInMonths).
- [ ] Borrar `tracking` (legacy), `myTables`, option set `myTable`.
- [ ] Corregir firstName/lastName invertidos en `trustedContact`.
- [ ] Auditar uso real de plugins; desinstalar `Make` si no se usa.
- [ ] Corregir los issues pre-existentes que reporta Bubble (`workflows.md`).
