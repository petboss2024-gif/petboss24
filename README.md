# PetBoss 24

Marketplace mobile-first de cuidado de mascotas: conecta dueños de mascotas con
petsitters profesionales. Construido en **Bubble.io** (app nativa iOS/Android +
web en `petboss24.com`).

- **App de Bubble:** `valecans-home-89169` (display name "Petboss 24")
- **Plan:** Starter — sin ramas de desarrollo; se trabaja en **Main (test)** y se
  despliega a **Live**.
- **Stack:** Bubble (frontend, DB, workflows) · Google Maps JS API · Stripe
  (Checkout + Connect Express) · Twilio Verify · SendGrid · APNs/FCM.

Este repositorio es la **fuente de verdad de la documentación** del proyecto.
Bubble no tiene control de versiones legible, así que todo cambio relevante se
registra aquí.

## Índice

| Documento | Contenido |
|---|---|
| [docs/database.md](docs/database.md) | Modelo de datos: tipos, campos, option sets y problemas conocidos |
| [docs/privacy-rules.md](docs/privacy-rules.md) | Privacy rules por tipo de dato (quién ve qué) |
| [docs/workflows.md](docs/workflows.md) | Flujos clave: reserva, pago, tracking, backend workflows |
| [docs/pets-places.md](docs/pets-places.md) | Refactor de Pets y Places: backend, validaciones, privacidad |
| [docs/pets-places-mobile.md](docs/pets-places-mobile.md) | My Pets y My Places mobile: auditoría implementada, renombrado y pendientes |
| [docs/empty-states.md](docs/empty-states.md) | Estados vacíos: option set `emptyStateContext`, reusable `appEmptyState` y dónde se usa |
| [docs/screens.md](docs/screens.md) | Pantallas trabajadas: walletOverview, walletTransactionList, notificationList |
| [docs/auth-review.md](docs/auth-review.md) | Login, signup, verificación de email y forgot password: flujo actual y pendientes |
| [docs/plugins.md](docs/plugins.md) | Inventario de plugins y criterios para usarlos o crearlos |
| [docs/conventions.md](docs/conventions.md) | Convenciones de nombres (camelCase) y estado del renombrado |
| [docs/backlog.md](docs/backlog.md) | Pendientes priorizados |
| [docs/changelog.md](docs/changelog.md) | Historial de cambios en Bubble, con savepoints para revertir |
| [documentation.md](documentation.md) | Plugin **PetBoss Live Walk** (tracking GPS) — diseño y fases |

## Cómo trabajar

1. Antes de cambiar algo en Bubble, crear un **savepoint** (Version control) y
   anotarlo en `docs/changelog.md`.
2. Trabajar en **Main (test)**. Probar en el preview / TestFlight antes de
   desplegar a Live.
3. Actualizar el documento correspondiente en `docs/` en el mismo momento.
4. Nunca guardar claves, tokens ni secretos en este repo.
