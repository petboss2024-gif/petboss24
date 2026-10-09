# Plugins

## Instalados (2026-10-09)

| Plugin | Uso / opinión |
|---|---|
| API Connector | Stripe, Twilio. Esencial |
| Google | Esencial |
| Google Places Autocomplete | Direcciones. Bien |
| 1T - List of Dates | Fechas recurrentes. Verificar mantenimiento |
| International Phone Input (Mobile) | Teléfono. Verificar mantenimiento |
| Slider Input (Mobile), Star Rating (Mobile), Toggle Switch (Mobile), GS Native Mobile Progress Bar | Interfaz. Son los que más se rompen con actualizaciones móviles |
| Gemini IA | Notas de mascota con IA. Confirmar que la clave vive en el servidor |
| Make | **Revisar si se usa.** Si no, desinstalar (salida de datos innecesaria) |
| Toolbox (JS↔Bubble) | Sus eventos (id `1488796042609…`) se usan en `walk-tracking-petsitter`, pero **no aparece** en la lista de plugins del outline. Verificar que siga instalado |

> Pendiente: auditar qué plugins se usan realmente y quitar los que no.

## Criterios

**Plugins de terceros**
- Usar cuando resuelven algo difícil (nativo, mapas, pagos) y tienen buena
  reputación, actualizaciones recientes y muchos usuarios.
- Evitar si se puede con elementos nativos de Bubble o el API Connector.
- Revisarlos periódicamente.

**Plugins propios**
- Ventajas: confianza, control, sin abandono, sin mensualidades, a medida,
  reutilizables o vendibles.
- Costos: el mantenimiento es nuestro (Bubble, iOS y Android cambian), hay que
  probar en dispositivos reales, siguen sumando peso, la seguridad de claves es
  nuestra, y el conocimiento debe quedar documentado.
- Valen la pena para lo **central del producto** (tracking GPS en segundo
  plano) o cuando no existe una alternativa mantenida.
- Regla: **todo plugin propio vive en este repo**, con versiones, documentación
  y una guía de prueba en dispositivo.

## Plugins propios planeados

| Plugin | Estado | Doc |
|---|---|---|
| PetBoss Live Walk (tracking GPS) | Fase 3 | [documentation.md](../documentation.md) |
| Tracking en segundo plano (nativo) | Idea — resuelve que `watchPosition` se congela al bloquear el teléfono | — |

## Qué puede hacer Claude con plugins

- Escribir el código completo (elementos, acciones, eventos, estados) y dejarlo
  en el repo listo para pegar en el Plugin Editor.
- Instalar y conectar plugins ya publicados.
- **No puede** crear/publicar plugins vía el conector de Bubble, ni compilar o
  probar en un teléfono.
