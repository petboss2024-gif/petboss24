# Changelog (cambios en Bubble)

Todos los cambios se hicieron en **Main (test)**. Nada se desplegó a Live desde
estas sesiones. Para revertir, usar el savepoint indicado (Version control en el
editor, o `restore_savepoint` con el timestamp).

## 2026-10-10

### Pets y Places: refactor (seguridad, backend, validaciones)
- Savepoint antes: **"Antes de refactor Pets y Places"** — `1791637355005`
- Savepoint después: **"Después de refactor Pets y Places (fases 1-4)"** — `1791639308907`
- Backend nuevo (no expuesto, respeta privacy rules): `pet-generate-ai-data`,
  `pet-delete`, `place-delete`, `place-set-default`,
  `place-revoke-petsitter-access`.
- `checkout_session_completed`: autoriza también las mascotas; acceso solo con
  `payment_status = paid`. `create-address`: sin ignore privacy, `user` debe
  ser Current User.
- Pets: validación, IA asíncrona con estado, una sola edición, Cancel
  arreglado, Edit/Delete solo para el dueño, lista de mascotas de
  customerProfile corregida.
- Places: detalle con datos, error visible sin dirección, ciudad/estado/ZIP
  conservados al editar, ZIP como texto (`zipCodeText`), códigos incompatibles
  vaciados, soft delete unificado.
- Reservas: selectores sin registros borrados.
- Privacy rules: petsitters solo ven datos básicos de mascotas; datos completos
  solo con reserva pagada; auto-binding desactivado en pets y address.
- Bubble: sin issues nuevos (9 pre-existentes). Detalle en `pets-places.md`.

## 2026-10-09

### Renombrado a camelCase
- Savepoint: **"Antes de renombrar campos a camelCase"** — `1791503705742`
- 21 campos renombrados (detalle en `conventions.md`). Bubble sin issues nuevos.
- Option sets no renombrables por herramienta → checklist manual.

### Reusables y vistas
- Savepoint: **"Antes de renombrar reusables y vistas a camelCase"** — `1791504176911`
- `header`, `footer`, `offlineBanner`, `updateApp`. Páginas web sin cambios.

### Wallet
- Savepoint: **"Antes de conectar wallet"** — `1791504737007`
- Lista de actividad (5 recientes, desc), totales del mes, vista petsitter
  ("Earnings"), estado vacío, borrado "Test 65".
- Nueva vista `walletHistory` + "View All".
- Campo `User.stripeCustomerId`; llamadas API *Create Customer* y
  *Create Setup Session* (sin inicializar); deep link en "Add Payment Method".

### Notifications
- Savepoint: **"Antes de rediseñar notifications"** — `1791505423063`
- Rediseño visual (detalle en `screens.md`). Se eliminaron las imágenes de stock
  de las celdas.

### Privacy rules y create-request
- Savepoint: **"Antes de privacy rules y create-request"** — `1791507626562`
- Campos nuevos: `address.authorizedPetsitters`, `trackingNew.customer`,
  `updateLocation.customer`.
- Workflows: `checkout_session_completed` (+ autoriza petsitter),
  `trackingPetsitter` (customer en trackingNew), `tracking-petsitter`
  (customer en updateLocation; quita autorización al terminar).
- `create-request`: requiere auth + condición `customer = Current User`.
- `create-transaction`: `stripeAccountId` leído en backend desde el petsitter.
- Privacy rules reescritas para 19 tipos (detalle en `privacy-rules.md`).
- Bubble: sin issues nuevos (9 pre-existentes).

### Login, signup y verificación de email
- Savepoints: **"Antes de arreglar login y signup"** — `1791511125227`;
  **"Antes de correcciones de la revisión de login/signup"** — `1791513607632`
- Campos User: `tokenExpiresAt`, `verificationAttempts`,
  `pendingVerificationCode`.
- Backend nuevos: `sendVerificationCode` (cooldown 60 s, expira 15 min) y
  `verifyEmailCode` (máx. 5 intentos).
- signup: `emailConfirmation = no`; envía el código por backend.
- login: sin política de contraseña en el input; si no está verificado abre
  `verificationCheck` y no navega.
- onbLoginStep1: solo entra si está verificado; si no, Log out.
- forgotPassword: termina si el email está vacío.
- verificationCheck: verificación por backend, éxito con *Do when condition is
  true*, resend por backend, estado limpio al abrir.
- profile y resetPassword: usan el mismo flujo.
- Borrados: workflow de carga de la página `email_confirmation` (verificaba sin
  código), eventos "Verify/Incorrect email code" de profile, eventos duplicados
  de signup y del reusable.
- Privacy rule *Self* de User: ya no muestra `tokenConfirmation` ni
  `pendingVerificationCode`.
- Texto "or" de Google/Apple oculto en login y signup.
- Bubble: sin issues nuevos (9 pre-existentes). Detalle en `auth-review.md`.

### Contraseña: requisitos y mostrar/ocultar
- Savepoint: **"Antes de requisitos de contraseña y mostrar/ocultar contraseña"** — `1791547780917`
- signup: el botón *Sign up* ya no crea la cuenta si la contraseña no cumple
  los 4 requisitos; la lista marca ✓ verde / rojo tras intentar; el espacio ya
  no cuenta como carácter especial; texto "(A-Z)" en mayúscula.
- signup y login: ícono de ojo para mostrar/ocultar la contraseña (texto de
  solo lectura); se vuelve a ocultar al enviar.
- Estados nuevos: `signup.showPassword`, `signup.submitAttempted`,
  `login.showPassword`. Elemento renombrado: "Text Lenght" → "Text Length".
- Bubble: sin issues nuevos (9 pre-existentes). Detalle en `auth-review.md`.

### resetPassword y settingsPrivacySecurity
- Savepoint: **"Antes de arreglar resetPassword y settingsPrivacySecurity"** — `1791551230519`
- Requisitos de contraseña y bloqueo igual que el signup en ambas pantallas.
- `resetPassword`: arreglado "The passwords don't match" (comparaba contra un
  texto fijo); política movida al campo de contraseña nueva.
- `settingsPrivacySecurity`: campos obligatorios, ID duplicado corregido,
  lista de requisitos y aviso de no coincidencia nuevos, limpieza de campos al
  guardar.
- Confirmado: la *password policy* de Bubble está activada (8, número,
  mayúscula, no alfanumérico).
- Bubble: sin issues nuevos (9 pre-existentes).

### Renombrado de páginas web y vistas móviles
- Savepoint: **"Antes de renombrar páginas web y vistas móviles"** — `1791565293480`
- 34 vistas y 10 páginas web renombradas según la convención nueva (tabla en
  `conventions.md`). `index`, `404`, `reset_pw`, `updateApp` y las vistas que
  ya cumplían quedaron igual.
- URLs escritas como texto actualizadas a los nombres nuevos (pago de
  reservas, Stripe Connect, términos y privacidad); detalle y pendientes en
  `backlog.md`.
- Bubble: sin issues nuevos (9 pre-existentes).

### Renombrado de reusables
- Savepoint: **"Antes de renombrar reusables"** — `1791566694459`
- 16 reusables renombrados (tabla en `conventions.md`). Bubble: sin issues nuevos.

### Revisiones sin cambios
- Auditoría del modelo de datos → `database.md` / `backlog.md`.
- Política de plugins → `plugins.md`.
