# Tareas — Manager Library (Laravel + React)

Mismas tareas de [`../spec-python/tasks.md`](../spec-python/tasks.md). La evidencia se traduce al stack del reto.
Una tarea es **hecha** solo cuando su evidencia existe y se puede ejecutar (artículo XII).

**Cómo correr la evidencia** (repo `api`): `php artisan test` → 29 pruebas en verde.

| # | Tarea | Evidencia en Laravel/React | Estado |
|---|---|---|---|
| T-01 | Esqueleto y `GET /health` | `GET /api/v1/health` → `{"status":"ok"}` · `tests/Feature/ApiContractTest.php` | **hecha** |
| T-02 | Migración inicial: seis tablas, FK, índices, cinco categorías | `database/migrations/2026_10_22_000001_create_library_schema.php`; en `library` hay exactamente 6 tablas, 0 columnas con `DEFAULT`, 5 categorías (consultas de `data-model.md` §10) | **hecha** |
| T-03 | Objetos de valor `Isbn`, `Quantity`, `DateRange` | `tests/Unit/Domain/ValueObjectsTest.php` | **hecha** |
| T-04 | `Book` (`withdraw`, `restock`, baja lógica) | `tests/Unit/Domain/BookTest.php`: nunca < 0, nunca > total | **hecha** |
| T-05 | `Loan` (estado calculado, 14 días, máx. 3) | `tests/Unit/Domain/LoanTest.php` con fecha fija como reloj falso: `open`, `overdue`, `closed` | **hecha** |
| T-06 | `Member` y `User` | — | pendiente |
| T-07 | Autenticación con fakes | — | pendiente |
| T-08 | Libros con fakes | — | pendiente |
| T-09 | Prestar con fakes | Reglas de dominio cubiertas en T-05; falta el caso de uso | pendiente |
| T-10 | Devolver y vencidos | Reglas de dominio cubiertas en T-05; falta el caso de uso | pendiente |
| T-11 | Reporte de más prestados | — | pendiente |
| T-12 | Repositorios libro, socio, usuario | — | pendiente |
| T-13 | Repositorio de préstamos + versión | Columna `book.version` creada (hallazgo H-01) | pendiente |
| T-14 | Hash Argon2id y JWT | — | pendiente |
| T-15 | Portadas en disco | — | pendiente |
| T-16 | Adaptador HTTP completo | Cuerpo de error único (RNF-4): `app/Adapters/Inbound/Http/ApiErrorRenderer.php` + `tests/Feature/ApiContractTest.php`. Faltan las rutas de negocio | **parcial** |
| T-17 | Composición y admin inicial | — | pendiente |
| T-18 | `ck_book_total_positive`, `ck_user_role` en la base | — | pendiente |
| T-19 | Pruebas de arquitectura | Artículo I: `tests/Architecture/DomainIsPureTest.php`. Faltan los artículos II y III | **parcial** |
| T-20 | `infra`: `docker compose up` | El servicio `db` levanta PostgreSQL 16 vacío. Faltan los Dockerfile de `api` y `app` | **parcial** |
| T-21 | `tool`: sembrador por la API | — | pendiente |
| T-22 | `app`: React | — | pendiente |
| T-23 | `page`: sitio estático | — | pendiente |
