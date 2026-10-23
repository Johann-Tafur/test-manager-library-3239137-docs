# Constitución — cómo se verifica cada artículo en Laravel

Los artículos de [`../spec-python/constitution.md`](../spec-python/constitution.md) **no cambian**. Cambia la herramienta de verificación.

| Artículo | Principio | Verificación en Laravel |
|---|---|---|
| I | El dominio no depende de nada | `tests/Architecture/LayerDependenciesTest.php`: ningún archivo de `app/Domain` contiene `use Illuminate\`, `use App\Application`, `use App\Adapters` ni paquetes de `vendor` |
| II | El núcleo declara sus puertos | El mismo test: `app/Application` no importa `App\Adapters` ni `Illuminate\`. Los tests de aplicación usan fakes |
| III | Un único punto de composición | El test busca `new App\Adapters\Outbound\...` y solo lo permite en `app/Providers/PortBindingsServiceProvider.php` |
| IV | SOLID | Un servicio = un caso de uso (`app/Application/Services`); puertos pequeños (`Clock`, `PasswordHasher`, `TokenIssuer`, `CoverStorage`) |
| V | El esquema es de las migraciones | `infra` no contiene `CREATE TABLE`; todo el DDL está en `database/migrations` |
| VI | La base impone lo que puede | `CHECK`, `UNIQUE` y FK declarados en la migración; lo pendiente se marca en `data-model.md` |
| VII | Lo derivado se calcula | No hay columna `status` ni `days_late`; se calculan en dominio y en SQL |
| VIII | Lo congelado no sigue al catálogo | `loan_item` guarda `book_title`, `isbn`, `category_name`; el reporte agrupa por ellos |
| IX | Ningún secreto en el repo | Solo `.env.example`; `.env` en `.gitignore`; las respuestas de usuario no incluyen `password` ni `password_hash` |
| X | Gana el motor | Si discrepan, se corrige el documento (ver H-01 en [`hallazgos.md`](hallazgos.md)) |
| XI | Idioma | Código, columnas y rutas en inglés; mensajes y documentación en español |
| XII | Nada sin evidencia | [`tasks.md`](tasks.md) solo marca **hecha** una tarea con evidencia ejecutable |
