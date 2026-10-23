# Spec Laravel + React — Manager Library

Traducción del spec de referencia (`spec-python/` y `spec-.net/`) al stack del reto: **PHP con Laravel** (backend) y **React** (frontend).

## Qué se respeta tal cual y qué se traduce

| Documento original | Tratamiento en el reto |
|---|---|
| `spec.md` (historias, CA, reglas, RNF) | **Se respeta sin cambios.** Es independiente del lenguaje |
| `api-contract.md` | **Se respeta sin cambios.** Mismas rutas, campos, códigos y cuerpo de error |
| `data-model.md` | **Se respeta**, con una corrección documentada en [`hallazgos.md`](hallazgos.md) (H-01: columna `version`) |
| `adr/` 001 a 004 | **Se respetan.** Solo cambia la herramienta que las implementa |
| `constitution.md` | Se respeta el principio de cada artículo; la verificación se traduce a herramientas PHP ([`constitution-laravel.md`](constitution-laravel.md)) |
| `plan.md`, `architecture.md` | **Se traducen**: [`plan.md`](plan.md) y [`architecture.md`](architecture.md) |
| `tasks.md` | Mismas tareas T-01 a T-23 con su evidencia en Laravel/React y su estado real: [`tasks.md`](tasks.md) |

## Orden de lectura

1. [`plan.md`](plan.md) — tabla de traducción de cada decisión técnica (D-01 a D-12).
2. [`architecture.md`](architecture.md) — capas hexagonales en carpetas de Laravel.
3. [`constitution-laravel.md`](constitution-laravel.md) — cómo se verifica cada artículo en PHP.
4. [`hallazgos.md`](hallazgos.md) — contradicciones y huecos del spec, y cómo se resolvieron.
5. [`tasks.md`](tasks.md) — estado de cada tarea con su evidencia.

## Repositorios del reto

| Repo | Contenido |
|---|---|
| `test-manager-library-3239137-api` | Laravel: dominio, casos de uso, adaptadores, migraciones, pruebas |
| `test-manager-library-3239137-app` | React (Vite): login, catálogo, socios, préstamos, devoluciones, vencidos, reporte |
| `test-manager-library-3239137-infra` | `docker compose up`: PostgreSQL 16 vacío + API + app |
| `test-manager-library-3239137-tool` | Sembrador de demostración que usa **solo la API** |
| `test-manager-library-3239137-page` | Sitio público estático, sin llamar a la API |
