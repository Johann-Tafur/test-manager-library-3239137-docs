# Tareas — Manager Library (Laravel + React)

Mismas tareas de [`../spec-python/tasks.md`](../spec-python/tasks.md). La evidencia se traduce al stack del reto.
Una tarea es **hecha** solo cuando su evidencia existe y se puede ejecutar (artículo XII).

| # | Tarea | Evidencia en Laravel/React | Estado |
|---|---|---|---|
| T-01 | Esqueleto y `GET /health` | `GET /api/v1/health` → `{"status":"ok"}` | pendiente |
| T-02 | Migración inicial: seis tablas, FK, índices, cinco categorías | Consultas de `data-model.md` §10 coinciden; sin `DEFAULT` | pendiente |
| T-03 | Objetos de valor `Isbn`, `Quantity`, `DateRange` | `tests/Unit/Domain` | pendiente |
| T-04 | `Book` (`withdraw`, `restock`, baja lógica) | `tests/Unit/Domain` | pendiente |
| T-05 | `Loan` (`addItem`, `returnItem`, estado, 14 días, máx. 3) | `tests/Unit/Domain` con `FixedClock` | pendiente |
| T-06 | `Member` y `User` | `"Ana "` → `ana`; rol `admin` rechazado | pendiente |
| T-07 | Autenticación con fakes | `tests/Unit/Application` | pendiente |
| T-08 | Libros con fakes | `tests/Unit/Application` | pendiente |
| T-09 | Prestar con fakes | `tests/Unit/Application` | pendiente |
| T-10 | Devolver y vencidos | `tests/Unit/Application` | pendiente |
| T-11 | Reporte de más prestados | título viejo tras renombrar | pendiente |
| T-12 | Repositorios libro, socio, usuario | PostgreSQL real | pendiente |
| T-13 | Repositorio de préstamos + versión | dos préstamos del último ejemplar: 201 y 409 | pendiente |
| T-14 | Hash Argon2id y JWT | la clave no aparece en respuestas | pendiente |
| T-15 | Portadas en disco | PNG ≤ 2 MB ok; 3 MB → 422 | pendiente |
| T-16 | Adaptador HTTP completo | rutas del contrato y cuerpo de error único | pendiente |
| T-17 | Composición y admin inicial | arranca con base vacía y se entra como admin | pendiente |
| T-18 | `ck_book_total_positive`, `ck_user_role` en la base | `INSERT` que las viola falla | pendiente |
| T-19 | Pruebas de arquitectura | `tests/Architecture` | pendiente |
| T-20 | `infra`: `docker compose up` | un comando | pendiente |
| T-21 | `tool`: sembrador por la API | `/loans/overdue` devuelve 3 líneas | pendiente |
| T-22 | `app`: React | recorrido login → prestar → devolver → reporte | pendiente |
| T-23 | `page`: sitio estático | se abre sin la API | pendiente |
