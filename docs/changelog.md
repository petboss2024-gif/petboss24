# Changelog (cambios en Bubble)

Todos los cambios se hicieron en **Main (test)**. Nada se desplegó a Live desde
estas sesiones. Para revertir, usar el savepoint indicado (Version control en el
editor, o `restore_savepoint` con el timestamp).

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

### Revisiones sin cambios
- Auditoría del modelo de datos → `database.md` / `backlog.md`.
- Auditoría de login/signup → `auth-review.md`.
- Política de plugins → `plugins.md`.
