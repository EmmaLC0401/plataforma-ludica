## Objetivo

Agregar el endpoint `GET /api/version` con su prueba.

## Herramienta y modelo

Copilot, modelo Auto.

## Prompt

Lee AGENTS.md. Agrega el endpoint GET /api/version, que devuelva la versión definida en app/proyecto.py, con su prueba en tests/test_inicio.py. Antes de escribir código, explícame qué archivos vas a modificar y espera mi aprobación.

## Resultado

El agente agregó el endpoint en `app/rutas/api.py` y una prueba en `tests/test_inicio.py`. También actualizó `STATE.md` para registrar el estado, según las instrucciones del proyecto; por eso se modificaron tres archivos en total, no solo los dos previstos para la funcionalidad. La prueba específica del endpoint pasó y Ruff no reportó problemas en los dos archivos Python. Como `pytest` y `ruff` no estaban disponibles como comandos directos, se ejecutaron mediante el intérprete de `.venv`. No se ejecutó la suite completa.
