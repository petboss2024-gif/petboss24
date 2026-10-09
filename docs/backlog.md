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
- [ ] Login/signup: verificación real de email (ver `auth-review.md`).

## 🟠 Funcionalidad

- [ ] **Stripe setup mode en wallet:** el usuario debe inicializar en el API
      Connector:
  1. *Stripe - Create Customer* → Initialize call → copiar el `cus_...`.
  2. *Stripe - Create Setup Session* → poner ese id en `customer` → Initialize.
  3. Confirmar que ambas usan la clave de prueba.
  Luego: terminar el workflow del botón "Add Payment Method".
- [ ] Hacer que Create Checkout Session envíe `customer` para usar la tarjeta
      guardada.
- [ ] Login/signup: bugs y plan de `auth-review.md`.
- [ ] Notificaciones: título con remitente, marcar como leída, abrir la reserva.
- [ ] Habilitar `petboss24.com` en *Allowed domains* del entorno cloud para
      poder probar con navegador desde las sesiones.

## 🟡 Modelo de datos y limpieza

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
