# Pantallas trabajadas

## walletOverview (vista móvil)

**Navegación:** `tabAccount` → "Wallet & Payments" → `walletOverview`.

| Parte | Comportamiento |
|---|---|
| Estado vacío ("Block Wallet") | Visible si el usuario **no es Petsitter** y no tiene transacciones (excluyendo `unpaid`). El contenedor de actividad se oculta en ese caso |
| Recent Activity | Últimas **5** transacciones, **más recientes primero**. Customer: donde es `customer`. Petsitter (conditional): donde es `petsitter` |
| Tarjeta "This Month" | Total y conteo de transacciones `paid` **del mes actual** (`Created Date > Current date/time rounded down to month`) |
| Total (petsitter) | Etiqueta "Earnings"; valor = suma de `amount` − suma de `platformFee` del mes |
| View All | Navega a `walletTransactionList` |
| Add Payment Method | Paso 1 (deep link a wallet) creado; resto pendiente — ver workflows.md |

Montos en **centavos**: se muestran `/ 100` con formato `$1,028.58`.
Se borró el workflow vacío "Test 65".

## walletTransactionList (vista móvil, nueva)

- App bar "Transaction History" (estilo `AppBar_mobile_`, con back) + banner
  `appOfflineBanner`.
- Lista completa (SelectableList, deshabilitada para selección) con la misma
  lógica customer/petsitter, sin límite, más recientes primero.
- Cada fila: tarjeta con borde `rgba(59,59,59,1)`, radio 12 — servicio y fecha
  a la izquierda; monto y estado a la derecha.
- Texto "No transactions yet" cuando la lista está vacía.

## notificationList (vista móvil, rediseño)

Solo diseño; búsquedas, condiciones y workflows intactos.

- Contenedor columna, padding 16, gap 24.
- Secciones: **"FOR YOU"** (personales) y **"ANNOUNCEMENTS"** (broadcast para
  petsitters). Encabezados 12px, semibold, `#8A8A8A`, letter spacing 1.
- Tarjeta: fondo `#2C2C2C`, radio 14, padding 14, borde
  `rgba(255,255,255,0.06)`.
- Ícono circular 40px, fondo `rgba(255,102,0,0.15)`, ícono `#FF6600`:
  🔔 `phosphor bold bell` (personal), 📣 `phosphor bold megaphone` (anuncios).
  Reemplaza una foto de stock que era igual para todos.
- Título 14px semibold blanco; tiempo relativo 12px `#8A8A8A`; cuerpo 13px
  `#B3B3B3`, line height 1.4.
- **No leída** (`seen = no`, solo personales): borde `rgba(255,102,0,0.55)`
  y fondo `#332519`.
- Título de anuncios: "PetBoss 24".
- Estado vacío: campana 72px, "You're all caught up" + subtítulo.
- Pendiente: título personal con el remitente, marcar como leída y abrir la
  reserva al tocar.

## Renombres de interfaz

- Reusables: `Header` → `header`, `Footer` → `footer`,
  `offline_banner` → `appOfflineBanner`.
- Vista móvil: `update_app` → `updateApp`.
- Páginas web **no** renombradas: su nombre es la URL (Stripe return URLs,
  WebViews de tracking, links de correo) y Bubble no admite mayúsculas.

> Ojo: la plantilla de "nueva vista móvil" de Bubble inserta un
> `offline_banner` que ya no existe; al crear vistas nuevas hay que
> reemplazarlo por `appOfflineBanner`.
