# Hallazgos al leer el spec y cómo se resolvieron

Leer un spec también es encontrar dónde se contradice o calla. Cada hallazgo cita el documento y deja la decisión tomada.

| # | Hallazgo | Dónde | Decisión |
|---|---|---|---|
| H-01 | ADR-002 dice que el libro «lleva un testigo de versión», pero `data-model.md` §3 **no** tiene columna para él | ADR-002 vs. `data-model.md` §3 | Se añade `book.version integer not null` (sin `DEFAULT`, la pone el dominio: 1 al crear, +1 en cada escritura). Por el artículo X, se documenta aquí como corrección del modelo de datos |
| H-02 | Laravel trae por defecto tablas `users`, `password_reset_tokens`, `sessions`, `cache`, `jobs` y columnas con `DEFAULT` (`created_at`, `remember_token`) | `data-model.md` §2: «seis entidades, seis tablas»; §3: «ninguna columna lleva `DEFAULT`» | Se eliminan las migraciones por defecto. Sesión, caché y colas no usan la base. La tabla es `user` (singular), no `users` |
| H-03 | La tabla de control de migraciones de Laravel sería una séptima tabla en el esquema | `data-model.md` §2 | La tabla `migrations` vive en el esquema `public`, fuera de `library`, que queda con exactamente seis tablas |
| H-04 | `GET /books/{id}/cover` se menciona en el texto pero no está en la tabla de rutas | `api-contract.md` §3.4 | Se implementa: rol autenticado, devuelve el binario o 404 |
| H-05 | `GET /loans/overdue` y `GET /loans/{id}` chocan en el enrutador | `api-contract.md` §3.6 | En `routes/api.php` se declara `overdue` antes de `{id}` y `{id}` se restringe a formato UUID |
| H-06 | ADR-002 permite «reintentar una vez si el ejemplar sigue disponible» | ADR-002 | Se elige **no** reintentar: el conflicto responde 409 `BOOK_NOT_AVAILABLE` directamente. Más simple y cumple RNF-2 |
| H-07 | CA-06.4 pide «422 con el conteo actual», el contrato no dice dónde | CA-06.4, `api-contract.md` §1 | El conteo va en `details`: `[{"field":"items","issue":"sin devolver: 2"}]` y en el `message` |
| H-08 | El reporte recibe `from`/`to` como fechas «ambos inclusive», pero `loaned_at` es un instante | `api-contract.md` §3.7 | Se filtra `loaned_at >= from 00:00:00Z` y `loaned_at < (to + 1 día) 00:00:00Z` |
| H-09 | `GET /members/{id}` incluye `active_loans`, pero no se define si cuenta préstamos o libros | `api-contract.md` §3.5 | Cuenta **líneas sin devolver**, igual que la regla del máximo 3 (RN-2) |
| H-10 | Sanctum (autenticación habitual de Laravel) crea la tabla `personal_access_tokens` y usa tokens opacos | D-11 | Se usa JWT HS256 con `firebase/php-jwt`, como pide D-11 |
