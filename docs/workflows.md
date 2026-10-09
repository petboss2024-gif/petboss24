# Workflows clave

## Flujo de una reserva (dog walking)

1. **Customer crea la reserva** → backend `create-request`
   - Crea `dgRequest` con `navStatusRequest = Pending`,
     `subStatusPending = Request`, `navSchedule = ASAP`, `active = yes`.
   - Crea notificaciones para petsitters.
   - **Cambio 2026-10-09:** ahora requiere autenticación y solo corre si el
     parámetro `customer` es el Current User. (No estaba expuesto como API
     pública, así que el riesgo previo era bajo.)
2. **Petsitters se postulan** → vista `bookingDetailsPetsitter`, botón Accept
   - Alterna al petsitter en `listOfPetsitters` (add / remove).
3. **Customer elige y paga** → reusable `customerPayment`
   - Crea un deep link de vuelta y agenda `create-transaction`
     (ignorando privacy rules).
   - `create-transaction`: crea `transaction` (status "Created"), llama a
     **Stripe - Create Checkout Session** con `application_fee_amount` y
     `transfer_data[destination]` = **Petsitter's stripeAccountId** (cambio
     2026-10-09), guarda `url`, `id`, `payment_status`, etc.
   - La vista `bookingPayment` abre la página `booking-payment` en un
     WebView.
4. **Stripe confirma el pago** → backend `checkout_session_completed` (webhook)
   - Actualiza la transaction (`paymentStatus`, `paymentIntentId`).
   - Crea el chat (`create-chat`).
   - En la reserva: deja en `listOfPetsitters` solo al petsitter pagado,
     `petsitterConfirmed = yes`, `active = no`, status Booked / Upcoming.
   - **Nuevo:** agrega ese petsitter a `address.authorizedPetsitters`.
   - Envía notificaciones.
5. **Paseo** → vista `walkTrackingPetsitter` + página web `walk-tracking-petsitter`
   (dentro de un WebView)
   - Al empezar: crea `trackingNew` (**nuevo:** con `customer`) y lo liga a la reserva.
   - Al iniciar el tracking: status Booked / In Progress, `trackingNew.active`,
     `addressStart`, `timeStart`.
   - Cada ping (Toolbox JS→Bubble): crea `updateLocation` (**nuevo:** con
     `customer`) y lo agrega a `trackingNew`.
   - Al terminar: geocoding, actualiza `trackingNew`, status Past / Completed,
     y **nuevo:** quita a los petsitters de `address.authorizedPetsitters`.

## ⚠️ Riesgo abierto: webhook de Stripe

`checkout_session_completed` está **expuesto como API pública y sin
autenticación**, y no verifica que el evento venga de Stripe. Cualquiera que
conozca la URL podría marcar una reserva como pagada (y dar acceso a la casa).
Solución propuesta: al recibir el evento, consultar a Stripe la sesión por ID
(Retrieve Checkout Session) y continuar solo si `payment_status = paid`.
**No corregido aún** — ver backlog.

## Wallet: agregar método de pago (Stripe setup mode)

Diseño acordado; **pendiente de inicializar las llamadas**:

1. Si `Current User's stripeCustomerId` está vacío → **Stripe - Create
   Customer** (`POST /v1/customers`, email) → guardar el id en el usuario.
2. **Stripe - Create Setup Session** (`POST /v1/checkout/sessions`,
   `mode=setup`, `currency=usd` como parámetros privados, `customer`,
   `success_url`, `cancel_url`).
3. Deep link de vuelta a `walletOverview` como success/cancel URL (este paso ya está
   en el workflow del botón).
4. Open external website con la `url` de la sesión.

Para terminarlo, el usuario debe inicializar ambas llamadas en el API
Connector (ver backlog). Nota: el Checkout actual envía `customer_email`, no
`customer`, así que la tarjeta guardada **no** se ofrecerá al pagar hasta
cambiar eso.

## Otros backend workflows

| Workflow | Notas |
|---|---|
| `add a list range of dates`, `remove a list range of dates`, `update credentials` | Bubble avisa: nombres con espacios no recomendados para Workflow API |
| `sendVerificationCode`, `verifyEmailCode` | Custom events de verificación de email (nuevos) — ver `auth-review.md` |
| `create-chat`, `create-message`, `notifications`, `create-review`, `create-address`, `create-ai-response`, `create-request-date` | Sin revisión detallada |
| `webhook_account_enabled`, `webhook_account_restricted` | Webhooks de Stripe Connect — revisar la misma vulnerabilidad que `checkout_session_completed` |

## Issues que Bubble ya reportaba (pre-existentes)

- `walk-tracking-petsitter`: textos con expresiones no imprimibles; "Make changes to
  trackingNew: Value should be number but is empty".
- `create-request`: "Search for profiles: value should be User but is empty".
- `create-ai-response`: "Value should be number but is empty".
