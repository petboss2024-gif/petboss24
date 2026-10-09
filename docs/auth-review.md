# Login, signup y verificación de email

> Arreglado en **Main (test)** el 2026-10-09 y revisado con una auditoría
> adversarial (21 revisores). **Sin prueba en dispositivo** (el entorno no puede
> abrir `petboss24.com`). Nada desplegado a Live.
>
> Savepoints: "Antes de arreglar login y signup" (`1791511125227`) y
> "Antes de correcciones de la revisión de login/signup" (`1791513607632`).

## Flujo actual

- **onbLoginStep1:** si el usuario está logueado **y verificado** → navHome
  (Customer) o navBookings (Petsitter). Si está logueado pero **no verificado**
  → Log out (vuelve a entrar por login y verifica).
- **signup:** Sign the user up (rol Customer, `emailConfirmation = no`, sin
  token) → crea `profile` → backend `sendVerificationCode` → abre el sheet
  `verificationCheck` con el email del usuario y estado limpio.
- **login:** Log the user in. Si no está verificado: backend
  `sendVerificationCode` → abre `verificationCheck` → termina el workflow (no
  navega). Si está verificado: navBookings (Petsitter) o navHome (resto).
  El input de password ya **no** valida la política de contraseña.
- **profile → Verify email:** usa el mismo backend `sendVerificationCode` y el
  mismo sheet.
- **resetPassword (vista nativa):** mismo control de verificación y navegación
  por rol que el login. (Según la revisión, el reset real usa la página web
  `reset_pw`; esta vista parece no alcanzable, pero quedó protegida igual.)
- **forgotPassword:** si el email está vacío, muestra el aviso y **termina** el
  workflow; si no, envía el email de reset de Bubble → `checkYourEmail`.

## Contraseña (signup y login)

Savepoint: "Antes de requisitos de contraseña y mostrar/ocultar contraseña"
(`1791547780917`).

**Requisitos en signup** (lista bajo el input, visible al enfocar, al escribir o
tras tocar *Sign up*):

| Regla | Regex / condición |
|---|---|
| 1 mayúscula (A-Z) | `[A-Z]` |
| 1 número | `[0-9]` |
| 1 carácter especial | `[^A-Za-z0-9\s]` (el espacio no cuenta) |
| 8 caracteres mínimo | `number of characters ≥ 8` |

- Cada regla muestra `•` gris → `✓` verde al cumplirse; si se tocó *Sign up* y
  no se cumple, se pone **roja**.
- El workflow de *Sign up* termina (no crea la cuenta) si falta cualquier regla.
- Un workflow aparte en el mismo botón marca `submitAttempted` sin leer inputs,
  para que la lista aparezca aunque Bubble bloquee el workflow principal por un
  campo obligatorio vacío.
- **Esto es solo del lado del cliente.** La validación real es la *password
  policy* de Settings → General (mín. 8, número, mayúscula, no alfanumérico),
  que hay que activar a mano para que coincida.
- Limitaciones conocidas: `Á`, `Ñ` no cuentan como mayúscula (el texto dice
  A-Z); `ñ`, `é` o un emoji sí cuentan como carácter especial (probablemente
  igual que la policy de Bubble; confirmarlo con un registro de prueba).

**Mostrar/ocultar contraseña** (signup y login): ícono de ojo a la derecha del
input. Al tocarlo, el input seguro se oculta y se muestra un texto de solo
lectura con la contraseña; el ícono cambia a ojo tachado. Tocar el texto o el
ojo vuelve a ocultarla. Al tocar *Sign up* / *Log in* se vuelve a ocultar.
- Se eligió texto de solo lectura (no un input de texto) para conservar el
  autocompletado de iOS/Android y evitar mayúscula automática y autocorrector.
- En iOS, al volver a escribir en un campo de contraseña que perdió el foco, el
  sistema borra todo lo escrito con la primera tecla. Es comportamiento normal
  de iOS.

## verificationCheck (reusable)

- **Verify** (solo corre si el input tiene 6 dígitos): estado "verificando" →
  guarda el código en `pendingVerificationCode` → backend `verifyEmailCode` →
  pausa 2,5 s → si sigue sin verificar, muestra error.
- **Éxito** (*Do when condition is true*, cada vez): cuando
  `emailConfirmation = yes` y el sheet está visible → estado éxito, oculta el
  sheet, navega por rol.
- **Resend:** backend `sendVerificationCode` y reinicia el estado del sheet.
- Error: "Incorrect or expired code"; con 5 intentos fallidos indica pedir un
  código nuevo.

## Backend

| Workflow | Qué hace |
|---|---|
| `sendVerificationCode` | Solo si no hay código o el actual tiene más de 1 minuto (**cooldown 60 s**). Genera 6 dígitos, expira en **15 min**, intentos = 0, envía el email ("This code expires in 15 minutes"). |
| `verifyEmailCode` | Compara `pendingVerificationCode` con `tokenConfirmation`. **Falla** si no coincide, no hay token, expiró o hay ≥ 5 intentos → intentos + 1 y borra el pendiente. **Éxito** → `emailConfirmation = yes`, borra token/pendiente/expiración, intentos = 0. |

Campos nuevos en User: `tokenExpiresAt`, `verificationAttempts`,
`pendingVerificationCode`. La privacy rule *Self* **no** deja ver
`tokenConfirmation` ni `pendingVerificationCode` al propio usuario: el código
solo se puede leer desde el email.

## Bugs de la auditoría inicial

| # | Bug | Estado |
|---|---|---|
| 1 | Verificación decorativa (signup ponía `emailConfirmation = yes`) | ✅ Arreglado |
| 2 | Login validaba la política de contraseña | ✅ Arreglado |
| 3 | Admin / sin rol atascados tras login | ✅ Van a navHome |
| 4 | Forgot password seguía con email vacío | ✅ Arreglado |
| 5 | Google/Apple sin workflow | ✅ Botones ya ocultos; se ocultó también el texto "or" huérfano |
| — | Código comparado en el cliente | ✅ Movido a backend |
| — | Sin expiración ni límite de intentos | ✅ 15 min, 5 intentos, cooldown 60 s |
| — | Custom events duplicados/inútiles (signup, profile) | ✅ Borrados |

## Encontrado en la revisión y corregido

- 🔴 La página web antigua `email_confirmation` ponía
  `emailConfirmation = yes` al cargar, sin código. Borrado en test.
  **Sigue activo en Live hasta el próximo deploy.**
- Profile tenía un evento cliente "Verify email code" que verificaba sin
  código. Borrado.
- Resend sin cooldown (reiniciaba intentos y permitía spam de emails).
- Botón Verify quedaba bloqueado con el input vacío.
- El sheet no mostraba el email (`senTo` nunca se llenaba) y reabría con
  estado viejo.
- Resend no reiniciaba el estado "cargando".

## Pendiente

- **Probar en dispositivo** (TestFlight / preview): signup, login sin
  verificar, código incorrecto ×5, código expirado, resend, cerrar la app en el
  sheet y volver, forgot password.
- **Desplegar a Live** para cerrar el bypass de `email_confirmation`.
- Con red lenta, un código correcto puede mostrar el error un instante antes
  de navegar (la detección usa una pausa de 2,5 s).
- `verificationAttempts` vuelve a 0 con cada código nuevo: no hay bloqueo
  total (≈ 5 intentos por minuto como máximo). Opcional: tope por hora/día.
- La verificación se aplica en la interfaz (login, onboarding), no en las
  privacy rules. Hoy no es un problema porque "verificado" no da acceso extra.
  Si algún día lo da, agregar `emailConfirmation is yes` a las reglas.
- Desde profile, al verificar, ahora navega a navHome (antes navAccount).
- Diseño y textos sin tocar: sombra azul del input del código, texto legal sin
  checkbox, links a la versión live.
- Contraseña: activar la *password policy* en Settings → General; probar en
  dispositivo el ojito (que el input oculto conserve el valor y que el ícono
  de 44 px se vea del tamaño correcto).
- Otras pantallas con contraseña sin estos cambios: `resetPassword` (el texto
  "The passwords don't match" compara contra un texto fijo y se muestra casi
  siempre; la política está en el input de confirmación) y
  `settingsPrivacySecurity` (sin requisitos; dos inputs con el mismo ID).
- Datos: en test los 3 usuarios tienen `emailConfirmation = yes`. En Live el
  signup antiguo siempre lo ponía en yes, así que nadie debería quedar fuera;
  confirmar en *Data → App data (Live)* antes de desplegar.
