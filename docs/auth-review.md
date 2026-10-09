# Auditoría: login, signup y verificación

> Revisión de configuración del 2026-10-09. **Sin cambios aplicados** y **sin
> prueba en vivo** (el entorno no puede abrir `petboss24.com`).

## Flujo actual

- **onbLoginStep1–3:** onboarding; si el usuario ya está logueado, va directo a
  navHome (Customer) o navBookings (Petsitter).
- **login:** Log the user in (stay logged in, remember email) → navBookings si
  Petsitter, navHome si Customer.
- **signup:** Sign the user up (rol Customer, verificación None,
  `tokenConfirmation` = 6 dígitos aleatorios, **`emailConfirmation = yes`**) →
  envía email con el código → crea `profile` con `firstName` → muestra el sheet
  `verificationCheck`.
- **verificationCheck (reusable):** compara `Current User's tokenConfirmation`
  con el input; si coincide marca `emailConfirmation = yes` y va a navHome.
  "Resend" genera un código nuevo y reenvía.
- **forgotPassword:** envía el email de reset de Bubble → `checkYourEmail`.

## 🔴 Bugs

1. **La verificación de email es decorativa:** el signup ya pone
   `emailConfirmation = yes`; si el usuario cierra la app en el sheet, al volver
   entra directo. El login no revisa la verificación.
2. **El login probablemente rechaza contraseñas antiguas:** el input de
   password del login tiene *"Check the password while typing"* (política). Solo
   debe estar en el signup.
3. **Admin o usuario sin rol se quedan atascados** tras el login: no hay
   navegación para esos casos.
4. **Forgot password continúa con el email vacío:** muestra el aviso pero no
   termina el workflow.
5. **Google y Apple:** los botones no tienen workflow (login y signup).

## 🟠 Workflows

- El código se compara **en el cliente** (se puede leer sin el email). Mover a
  backend.
- Sin límite de intentos ni expiración del código.
- Errores con el mensaje por defecto de Bubble.
- Custom event "Verify code" duplicado e inútil en `signup`.
- El perfil se crea sin apellido.

## 🟡 Diseño y textos

- Email de verificación: el código sale entre corchetes `[123456]` y falta un
  punto en "Copy and paste this code into the app if you didn't…".
- Input del código: sombra de foco azul `#52A8EC` en vez de `#FF6600`.
- Requisitos de contraseña visibles antes de escribir; elemento "Lenght".
- Sin botón mostrar/ocultar contraseña en login.
- Texto legal sin checkbox; links apuntan a la versión live.

## Plan propuesto (pendiente de aprobación)

1. Verificación real: `emailConfirmation = no` al registrarse, validar código en
   backend, expiración + máx. 5 intentos, y redirigir a no verificados al sheet.
2. Quitar la política de contraseña del input de login.
3. Navegación para Admin / sin rol.
4. Detener forgot password con email vacío.
5. Corregir texto del email y sombra del input.
6. Google/Apple: conectar (requiere credenciales OAuth) u ocultar.
