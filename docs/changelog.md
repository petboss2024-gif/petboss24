# Changelog (cambios en Bubble)

Todos los cambios se hicieron en **Main (test)**. Nada se desplegó a Live desde
estas sesiones. Para revertir, usar el savepoint indicado (Version control en el
editor, o `restore_savepoint` con el timestamp).

## 2026-10-13

### Onboarding pre-login: etapa 2 (botones Next)
- Savepoint antes: el de después de la etapa 1 (`1791904628496`; sin cambios entre medias).
- Savepoint después: **"Después de onboarding etapa 2: botones Next en welcomeStep1 y welcomeStep2"** — `1791904988355`
- `btnOnboardingNext` ("Next", estilo `Mobile Button Secondary`) debajo de
  "Join our community" y encima de "Already a member?":
  - `welcomeStep1`: elemento `cnBrZ`, workflow `onboardingNext` `cnBrb` → acción
    `cnBrd`: ir a `welcomeStep2` (stack, reinicia la pila).
  - `welcomeStep2`: elemento `cnBre`, workflow `onboardingNext` `cnBrg` → acción
    `cnBri`: ir a `welcomeStep3` (stack, reinicia la pila).
- Tamaño y alineación: los valores por defecto del editor, los mismos que usa
  el botón Join (que no tiene ninguno propio). Se reordenó "Already a member?"
  para que quede al final en los dos pasos.
- `welcomeStep3` sin cambios (último paso: Join y Sign in).
- Verificado leyendo elementos, orden y workflows; Bubble: los mismos 6 errores
  preexistentes. **Sin probar en dispositivo.**

### Onboarding pre-login: etapas 0 y 1
- Savepoint antes: **"Antes de onboarding etapa 1 (reinicio de pila entre welcomeStep1-3)"** — `1791904585158`
- Savepoint después: **"Después de onboarding etapa 1: 6 transiciones entre pasos con reinicio de pila"** — `1791904628496`
- Las 6 navegaciones entre pasos (círculos del indicador) pasan a *stack* con
  **Reset navigation stack**: `cmjIr` (1→2), `cmjLf` (1→3), `cmjIz` (2→1),
  `cmjKp` (2→3), `cmjJv` (3→1), `cmjKT` (3→2). Antes apilaban una vista por
  toque.
- Efecto esperado: la pila contiene un solo paso del onboarding; Back desde
  `authSignup` vuelve al paso de origen. En Android, atrás en un paso cierra la
  app (no hay paso anterior en la pila).
- Verificado leyendo las 6 acciones; Bubble: los mismos 6 errores
  preexistentes. **Sin probar en dispositivo.**
- Vista inicial de la app: la conexión no la expone; comprobar en el editor.

### Design System: nombres de estilos por función (DS-016)
- Savepoint antes: **"Antes de la refactorización semántica de nombres de estilos (DS-016)"** — `1791845098353`
- Savepoint después: **"Después de la refactorización semántica de nombres de estilos (DS-016): 76 estilos renombrados"** — `1791897738738`
- 76 estilos renombrados al formato `[Plataforma] [Componente] [Variante] [Rol]`
  (por ejemplo `sharedButtonPrimary` → `Button Primary`, `mobileButtonMenu` →
  `Mobile Tab Segment`). Tabla completa en `brand/bubble.md`.
- En grupos: tipografía (24), botones (9), formularios (7), superficies (10),
  navegación y flotantes (14), iconos y otros (12).
- Solo nombres: ningún color, tamaño ni condición cambió; nada se fusionó ni
  se borró.
- Verificación: los 76 nombres leídos de vuelta coinciden con el catálogo;
  Bubble sigue con los mismos 6 errores (preexistentes, ajenos a los estilos).

## 2026-10-12

### Design System: DS-005 y DS-007
- Savepoint antes: **"Antes de DS-005 (texto del botón principal 700 18)"** — `1791844915638`
- `sharedButtonPrimary`: texto 500 16 → 700 18 (también en la web que usa el estilo).
- DS-007: blanco sobre naranja se mantiene como excepción de marca (siempre
  negrita, 18 px o más). Sin cambios en Bubble aparte de DS-005.
- Bubble: sin errores nuevos (6 preexistentes).

### Design System: decisiones DS-008 a DS-015
- Savepoint antes: **"Antes de DS-008 a DS-014 y renombrado de estilos"** — `1791843833156`
- Savepoint después: **"Después de DS-008 a DS-014 y renombrado de 76 estilos (mobile/web/shared)"** — `1791844336429`
- Botón secundario y `mobileButtonVerify`: de azul a superficie gris. Variable
  `cnBrY` `#272727` para el presionado.
- Títulos (H1–H5 y navegación web) en Raleway; el título más grande de 900 a 800.
- Tarjetas e icono del estado vacío: superficie lisa.
- Radios a 14 (popups, botones web, ítem seleccionable, mensaje) y 8 en verificar.
- Enlaces de azul a naranja. Pestañas inactivas a `#B3B3B3`.
- `appEmptyState` vuelve a Group (había pasado a FloatingGroup en el editor);
  rellenos restaurados. Bubble: de 19 a 6 errores (los preexistentes).
- 76 estilos renombrados con prefijo `mobile` / `web` / `shared`
  (catálogo en `brand/bubble.md`).

### Design System: variables y estilos globales (fase 1)
- Savepoint antes: **"Antes de alinear Style Variables y Global Styles con el Design System (fase 1)"** — `1791841900553`
- Variables nuevas: `cnBrT` texto secundario `#B3B3B3`, `cnBrU` naranja presionado
  `#A84400`, fuente `cnBrS` Raleway (sin uso todavía).
- `mobileSH5` → `#B3B3B3`. `mobilePrimary` liso (sin degradado a `#FF8C00`).
- Sin sombra: `mobilePrimary`, `mobileSecondary`, `mobileCancel`, `mobileDelete`,
  `onboardingSteps`.
- Estilos que apuntaban a variables borradas: `Popup Basic`/`Conflict` → `surface`;
  `Footer Link`, `Label 10 m` → `cnBrT`.
- Se corrigió en `brand/README.md` el contraste de blanco sobre naranja (2,94:1).
- Detalle y pendientes: `brand/bubble.md`, `brand/decisions.md`.
- Hallazgo fuera de alcance: `appEmptyState` pasó a FloatingGroup en el editor
  (13 errores en Bubble, DS-014).

## 2026-10-11

### Refinamiento visual de `appEmptyState`
- Savepoint antes: **"Antes de refinamiento visual de appEmptyState"** — `1791728105039`
- Savepoint después: `1791728286836`
- Corregido el layout del reusable (raíz y contenido estaban en fila, un grupo
  interno superponía los hijos, textos con ancho fijo de 100 px, icono sin
  espacio para el glifo).
- Tipografía y ritmo: mobileH5 / mobileSH5 14 px, interlineado 1.3 / 1.5,
  círculo de 64 px, espaciado 16/8/16.
- Variante compacta automática en los selectores (sin icono, padding 12).
- Places y Notifications recuperan su espacio superior con grupos slot.
- Sin cambios en textos, option set, condiciones, workflows ni navegación.
  Bubble: sin issues nuevos (6 pre-existentes). Detalle: `empty-states.md`.

### Sistema de estados vacíos (`appEmptyState`)
- Savepoint antes: **"Antes de appEmptyState (sistema de estados vacíos)"** — `1791727229224`
- Savepoint después: **"Después de appEmptyState: sistema de estados vacíos en 13 ubicaciones (Main/test)"** — `1791727809392`
- Option set nuevo `emptyStateContext` (15 opciones) y reusable `appEmptyState`
  con acción opcional vía estado `actionRequested`.
- Migrados al reusable: My Pets, My Places, Chats, Notifications y Wallet
  history (se borraron los bloques viejos). Nuevos: Bookings cliente y
  petsitter, Wallet overview, candidatos de la reserva, selectores de mascota y
  lugar (step 2 y edición), perfil de cliente (privacidad) y ubicaciones del
  servicio del petsitter.
- "Book a walk" (Bookings cliente, Pending) abre `bookingStep1` igual que Home.
- Sin cambios en búsquedas, privacidad, base de datos ni workflows de
  pago/GPS/auth. Bubble: sin issues nuevos (6 pre-existentes).
- Detalle: `empty-states.md`.

### My Pets y My Places: implementación de la auditoría (solo mobile)
- Savepoint antes: **"Antes de implementar auditoría My Pets y My Places (2026-10-10)"** — `1791642060025`
- Savepoint después: **"Después de implementar auditoría My Pets y My Places (fases 2-7)"** — `1791725583744`
- Reservas: editar guarda exactamente las mascotas elegidas; solo se edita en
  Pending; no se continúa ni se guarda sin mascota y dirección.
- Borrado con confirmación y Cancel en todos los caminos (`deleteConfirmSheet`,
  antes `addressDeleteSheet`); se cierra solo cuando el backend confirma.
- Estados vacío y de carga fiables en ambas listas; búsqueda por celda eliminada.
- ZIP con ceros iniciales en tarjeta, detalle y selectores; formato de dirección.
- Edad en años y meses; fechas de nacimiento futuras bloqueadas; IA no se
  regenera sin motivo.
- Quitados "Dog" fijo y plantillas sin datos; imagen de reemplazo; safe area en `petList`.
- Privacy: pets / Petsitters (basic info) ya no permite buscar.
- ~120 elementos y workflows renombrados. Bubble: 6 issues (los mismos de antes).
- Detalle en `pets-places-mobile.md`.

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

### Backend: nombres y carpetas
- Savepoint: **"Antes de renombrar workflows de backend"** — `1791640111509`
- 14 workflows renombrados a `area-accion` (tabla en `conventions.md`). Los 3
  webhooks de Stripe quedan igual porque su nombre es su URL.
- Carpetas propuestas en `conventions.md`. Hay que crearlas a mano: la
  herramienta no lo permite.
- Bubble: 6 issues (antes 9). Desaparecieron los 3 avisos de nombres con espacios.

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
