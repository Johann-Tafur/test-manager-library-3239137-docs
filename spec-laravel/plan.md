# Plan técnico — Manager Library (Laravel + React)

Traducción de [`../spec-python/plan.md`](../spec-python/plan.md). Lo que el spec define sobre el **negocio** no cambia; aquí solo cambia
la **herramienta**.

## 1. Decisiones técnicas traducidas

| # | Decisión | Python (referencia) | .NET (referencia) | **Laravel + React (reto)** | Por qué se conserva el principio |
|---|---|---|---|---|---|
| D-01 | Lenguaje | Python 3.12 | .NET | **PHP 8.3** | Stack del reto |
| D-02 | Framework HTTP | FastAPI | ASP.NET Core | **Laravel 12** (rutas en `routes/api.php`, controladores como adaptador de entrada) | Contrato de `api-contract.md` sin cambios |
| D-03 | Persistencia | SQLAlchemy 2 + Alembic | EF Core + migraciones | **Migraciones de Laravel** sobre **PostgreSQL 16**, esquema `library`; repositorios con el Query Builder | ADR-001: el esquema es de las migraciones |
| D-04 | Concurrencia | Testigo de versión + `CHECK` | Igual | **Columna `version`** en `book` + `UPDATE … WHERE id = ? AND version = ?`; si afecta 0 filas → 409 `BOOK_NOT_AVAILABLE`. Última barrera: `ck_book_available_range` | ADR-002 (ver hallazgo H-01) |
| D-05 | Fechas | `timestamptz`, puerto `Clock` | Igual | `timestamptz`, puerto `App\Application\Ports\Clock`; en pruebas un `FixedClock` | Probar vencimientos sin esperar 14 días |
| D-06 | Reporte | Consulta agregada por puerto de lectura | Igual | Puerto `ReportReader`, una sola consulta `GROUP BY book_id, book_title, category_name` | ADR-004 |
| D-07 | Objetos de valor | `Isbn`, `Quantity`, `DocumentNumber`, `DateRange` | Igual | Clases PHP `final readonly` en `app/Domain/ValueObjects`, sin tabla | No tienen identidad |
| D-08 | Portadas | Disco / S3, la base guarda `cover_key` | Igual | Puerto `CoverStorage`; adaptador `LocalDiskCoverStorage` (`storage/app/covers`). Cambiar a S3 = una línea en la composición | OCP |
| D-09 | Contraseñas | Argon2id tras `PasswordHasher` | Igual | `password_hash(..., PASSWORD_ARGON2ID)` tras el puerto `PasswordHasher` | Artículo IX |
| D-10 | Admin inicial | Arranque desde `ADMIN_USERNAME`/`ADMIN_PASSWORD` | Igual | Comando `php artisan library:ensure-admin`, ejecutado por el arranque del contenedor tras `migrate` | La base no puede producir el hash |
| D-11 | Autenticación | JWT HS256 8 h, `JWT_SECRET` | Igual | `firebase/php-jwt`, HS256, 8 h, claims `sub` y `role`; middleware propio. **No** se usa Sanctum (sus tokens son opacos y viven en una tabla extra) | Sin estado y sin séptima tabla |
| D-12 | Pruebas | pytest + fakes | xUnit + fakes | **PHPUnit** (incluido en Laravel) con fakes en memoria para los puertos; PostgreSQL real solo en integración | Pirámide de pruebas |
| D-13 | Frontend | React | React | **React 18 + Vite + react-router**; cliente HTTP con `fetch` | Stack del reto |

## 2. Configuración por entorno (`.env`, solo se versiona `.env.example`)

| Variable | Obligatoria | Ejemplo |
|---|---|---|
| `DB_CONNECTION` / `DB_HOST` / `DB_PORT` / `DB_DATABASE` / `DB_USERNAME` / `DB_PASSWORD` | sí | `pgsql` / `db` / `5432` / `library` / `library` / *(secreto)* |
| `JWT_SECRET` | sí | cadena larga aleatoria |
| `ADMIN_USERNAME` / `ADMIN_PASSWORD` | sí | definidos en `.env`, nunca en el repo |
| `COVERS_DIR` | no | `storage/app/covers` |
| `CORS_ORIGIN` | sí | `http://localhost:5173` |

`DATABASE_URL` del original se descompone en las variables `DB_*` que usa Laravel.
`SESSION_DRIVER`, `CACHE_STORE` y `QUEUE_CONNECTION` se fijan en `array`/`file`/`sync` para que Laravel **no** cree tablas propias en la base.

## 3. Estrategia de pruebas

| Nivel | Qué prueba | Herramienta |
|---|---|---|
| Dominio | Máximo 3, vencimiento a 14 días, disponibles en rango | PHPUnit, objetos puros (`tests/Unit/Domain`) |
| Aplicación | Prestar, devolver, reporte | PHPUnit con fakes en memoria (`tests/Unit/Application`) |
| Arquitectura | Artículos I, II, III | PHPUnit que inspecciona los `use` de cada archivo (`tests/Architecture`) |
| Contrato | Cuerpo de error único y códigos | PHPUnit `Feature` contra la API |
| Concurrencia | Dos préstamos del último ejemplar | Script contra la API levantada con PostgreSQL |

## 4. Orden de construcción

Igual que el original: esqueleto y salud → migración inicial con semilla → dominio → casos de uso con fakes → adaptadores → API →
pruebas de contrato y concurrencia → infra → app → tool → page.
