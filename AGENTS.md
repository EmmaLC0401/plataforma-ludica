# AGENTS.md

Contexto del proyecto para los agentes de IA. Léalo antes de cualquier cambio.
Al cerrar una sesión, actualice `STATE.md`.

## Proyecto

Prototipo web lúdico de cursos de inglés por niveles para niños.
Roles previstos: docente (inicio de sesión) y niño (código de clase y avatar, sin correo).
El estado actual es una plantilla desplegada: solo existen `/`, `/salud`, `/api/proyecto`,
`/docs` y la PWA. `app/servicios/`, `app/modelos/` y `tests/aceptacion/` están vacíos y
`specs/spec.md` conserva marcadores `(complete)`; no suponga que ya hay lógica de cursos,
sesiones ni base de datos.

## Comandos (Windows)

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements-dev.txt
uvicorn app.main:app --reload
pytest -v
ruff check .
```

- Una sola prueba: `pytest tests/test_inicio.py::test_salud_responde_ok -v` o `pytest -k salud -v`.
- Orden del CI: `ruff check .` y después `pytest -v` (no hay formateador configurado).
- `pre-commit install` activa Ruff y `pytest -q` antes de cada commit.

## Arquitectura

- `app/main.py` es el punto de entrada: monta `/static` e incluye los routers. Todo router nuevo
  debe registrarse allí.
- `app/rutas/`: solo HTTP. `paginas.py` sirve HTML y `/sw.js`; `api.py` sirve la API REST visible
  en `/docs`. La regla de negocio va en servicios, no aquí.
- `app/servicios/`: regla de negocio, con pruebas propias. `app/modelos/`: tablas SQLModel.
  Aún no hay motor, sesión de base de datos ni migraciones configuradas.
- `app/proyecto.py` es la única fuente de `APRENDIZ`, `CURSO`, `MECANICA`, `URL_PUBLICA` y
  `VERSION`: los usan la página de inicio, la API y `entregar.py`. Actualícelos allí, no en las
  plantillas.
- PWA: `/sw.js` se sirve desde `paginas.py` en la raíz (no desde `/static`) para que controle todo
  el sitio. Mantenga esa ruta. Si cambian los archivos estáticos, suba `CACHE` en
  `app/static/sw.js`.

## Pruebas

- `pyproject.toml` fija `testpaths = ["tests"]` y `pythonpath = ["."]`. No hay `conftest.py` ni
  fixtures; las pruebas usan `TestClient(app)` directamente.
- Las pruebas de aceptación van en `tests/aceptacion/test_<historia>.py` y traducen los criterios
  Dado/Cuando/Entonces de `specs/spec.md`.
- El CI corre en PR y en pushes a `main` con Python 3.13: Ruff, pytest y luego arranca Uvicorn y
  comprueba `/salud`. Reproduzca esa verificación antes de abrir un PR.

## Despliegue y configuración

- Render instala solo `requirements.txt` y arranca
  `uvicorn app.main:app --host 0.0.0.0 --port $PORT`; el health check es `/salud` y solo despliega
  si pasan los checks.
- Copie `.env.example` a `.env` (está ignorado por git). Hoy la aplicación no carga `.env` ni lee
  esas variables: si agrega `SECRET_KEY` o `DATABASE_URL`, léalas con `os.environ`.
- En desarrollo se documenta SQLite y en producción PostgreSQL, pero el código aún no conecta
  ninguna base de datos.

## Entrega semanal

1. Copie `entregas/plantilla.md` como `entregas/semana-N.md` y responda las tres secciones.
2. Guarde los prompts relevantes como `prompts/semana-N-tema.md`.
3. Ejecute `python entregar.py N`. El script corre pytest y Ruff, comprueba la URL publicada, lee
   la sección «Lecciones aprendidas» de este archivo y genera `entregas/semana-N.pdf` (ignorado
   por git).

## Convenciones y prohibiciones

- Nombres en español, claros y completos. Cambios pequeños, una tarea por rama y prueba antes del
  código.
- No escriba claves ni contraseñas en el código: se leen de variables de entorno.
- No instale dependencias sin verificar que existen en pypi.org.
- No modifique ni borre pruebas para que pasen.
- No use datos personales reales.
- Al iniciar cada sesión, leer STATE.md.

## Lecciones aprendidas

Cada vez que el agente se equivoque, escriba aquí la regla que evita repetir el error.

- No hacer commit ni push: los hace el desarrollador.