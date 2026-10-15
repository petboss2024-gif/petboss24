# Auditoría Google Authentication — Login y Sign Up (2026-10-15)

> Solo lectura. **No se modificó nada** en Bubble. Main (test).

## Estado encontrado

| Punto | Hallazgo |
|---|---|
| Vistas | `authLogin` (`cmitN`) y `authSignup` (`cmiyd`), mobile nativo. Se llega a ambas desde `welcomeStep3` y entre sí. |
| Botón en Login | `btnLoginGoogle` (`cmivT`) visible, workflow `loginWithGoogle` (`cnBsP`) + `loginWithGoogleError` (`cnBsY`). |
| Botón en Sign Up | Existe `Group Login Button Google` (`cmjLs`) dentro de `Container buttons` (`cmjNq`), **oculto** y **sin workflow**; estilo antiguo. "or" (`cmizJ`) oculto. Apple (`cmjLx`) también ahí. |
| Mecanismo | Plugin oficial **Google** de Bubble (web + mobile) + acción nativa **Signup/login with OAuth provider** (`cnBsS`, proveedor `google`). Es una acción **combinada**: si la identidad no existe, Bubble crea el usuario; si existe, inicia sesión. No hay acciones separadas de login y de alta. |
| Credenciales | Bubble ya no muestra el aviso "fill your app id and secret" → hay valores en el plugin. **No se puede confirmar que sean correctos** sin un login real. El secreto vive en la configuración del plugin (servidor); no aparece en elementos, workflows ni datos. |
| Uso real | **Ningún login con Google ejecutado**: 0 entradas OAuth/Google en los logs (14–15 oct) y ningún usuario con `googleAccount = yes`. |
| Backend compartido | `auth-google-complete` (`cnBsJ`), solo si `Current User's Google` no está vacío: (1) `googleAccount`/`googleId` siempre; (2) `emailConfirmation = yes` si el email coincide; (3) rol Customer + verificación None solo si `osRole` vacío; (4) `profile` con nombre de Google si no tiene. |
| Login tradicional | `Log the user in` → si no verificado: código + `verificationCheck` y termina; si verificado → `tabBookings` (Petsitter) / `tabHome`. |
| Sign Up tradicional | Valida contraseña → `Sign the user up` con **osRole = Customer**, `googleAccount = no`, `emailConfirmation = no` → `profile` con el nombre → código de verificación. |
| Modelo User | `emailConfirmation`, `googleAccount`, `googleId`, `osRole`, `osStatusVerificationPetsitter`, `tokenConfirmation`, `tokenExpiresAt`, `verificationAttempts`, Stripe ids. Privacy: "Self" no ve el código; "Logged in users" solo rol/verificación; Admin todo. |
| Logout / reset | Logout → `welcomeStep1`. Reset → email de Bubble (`authForgotPassword`). Un usuario solo-Google no tiene contraseña: "Forgot password" le crearía una. |

## Señal fiable de "usuario nuevo creado por Google"

El alta tradicional **siempre** pone `osRole = Customer`. Un usuario que Bubble
acaba de crear por OAuth llega con **`osRole` vacío**. Es un dato del servidor,
no del cliente → sirve para distinguir nuevo vs existente.

## Problemas

1. **Sign Up sin Google** (oculto, sin workflow).
2. **Vinculación solo por email** (contra la regla pedida): si Bubble enlaza una
   cuenta de contraseña con el mismo email, el backend marca `googleAccount`,
   `googleId` y `emailConfirmation` sin más comprobación. Comportamiento real de
   Bubble en este caso **sin verificar** (puede enlazar o dar error).
3. **Login crea cuentas**: la acción es combinada; desde Login una identidad
   nueva crea usuario. Hay que decidir la política.
4. La navegación tras Google no distingue "nuevo", "existente vinculado" ni
   "existente sin vincular".
5. Email verificado: el plugin expone `id`, `email`, `given_name`,
   `family_name`, `picture`; **no** expone `verified_email`. Se confía en que
   Google exige email verificado en la cuenta (hipótesis razonable, no
   comprobable desde Bubble).
6. Nombre: si Google no da `given_name`, el perfil queda sin nombre.
7. App nativa: vuelta desde Google a la app **sin probar**.

## Plan propuesto (pendiente de autorización)

Backend `auth-google-complete` (una sola lógica compartida):
- Vincular (`googleAccount`, `googleId`), confirmar email, rol Customer y
  perfil **solo si `osRole` está vacío** (usuario recién creado por Google).
- Cuenta existente con `googleAccount = yes` → no tocar nada.
- Cuenta existente sin vincular → no tocar nada (el cliente la rechaza).
- Nombre: si falta `given_name`, usar la parte del email antes de `@`.

Login (`cnBsP`):
- Guardar si el usuario era nuevo (`osRole` vacío) antes del backend.
- Sesión + `googleAccount = yes` → navegación actual por rol.
- Sesión + `googleAccount` no → **Log out** + mensaje "This email already has a
  password account. Log in with your password." (sin éxito falso).
- Identidad nueva desde Login → según decisión 1.

Sign Up (`authSignup`):
- Mostrar `cmjNq` y "or"; ocultar Apple; mismo estilo que el botón del Login;
  estados `googleLoading` / `googleError`; workflow `signupWithGoogle` +
  `OnError`.
- Nuevo → `tabHome`. Existente vinculado → entra con aviso "You already have an
  account". Existente sin vincular → Log out + mensaje + enlace a Login.

Pruebas: todas requieren navegador/dispositivo y cuentas de Google de prueba;
desde este entorno no se pueden ejecutar.
