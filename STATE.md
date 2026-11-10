# STATE.md

Estado del proyecto. Léalo al iniciar cada sesión de trabajo y actualícelo al cerrarla.

## Estado actual

La aplicación FastAPI conserva el estado de plantilla: ofrece la página de inicio (`/`),
el indicador de salud (`/salud`), los datos del proyecto (`/api/proyecto`), la versión
del proyecto (`/api/version`) y la documentación (`/docs`). Incluye recursos de PWA y
siete pruebas de ejemplo en `tests/test_inicio.py`. El prompt y el resultado del cambio
del endpoint están registrados en `prompts/semana-1-version.md`.

Todavía no hay lógica de cursos o sesiones, modelos de datos funcionales ni conexión a
una base de datos. `app/servicios/` y `app/modelos/` solo contienen sus archivos
iniciales, y `specs/spec.md` sigue siendo una plantilla con marcadores.

## Decisiones

| Fecha | Decisión | Motivo |
| --- | --- | --- |
| — | No hay decisiones adicionales registradas. | — |

## Siguiente paso

Completar `specs/spec.md` con historias y criterios de aceptación antes de implementar
la lógica del producto.
