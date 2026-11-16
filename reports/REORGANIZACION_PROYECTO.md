# Reorganización de LawWare

Fecha: 16 de noviembre de 2026. Estado: **validado en desarrollo local, con observaciones**. No se modificaron recursos de producción, no se accedió a Render y no se hizo push ni fusión.

## Inventario anterior y respaldo

Antes de reorganizar, la raíz no era un repositorio Git y contenía `modelo/`, `docs/`, `paquete_original/`, `.pg-local/`, `.pg-local.log`, `README.md` y `PLAN_REORGANIZACION.md`. Las cuatro carpetas de los repositorios oficiales no existían. Los 12 archivos de la extracción coincidían con el ZIP y los cuatro remotos estaban vacíos.

Se creó `_local/backups/estado_previo_20261116_120754.zip` y se verificaron 1014 archivos frente a sus hashes. SHA-256 del respaldo: `be233698cfbb9bdf0cac0360769d75b29ee369370835e4a92ab8c1c96792c557`. El manifiesto completo está junto al respaldo. También se preservó el estado anterior en `_local/imports/estado_previo/`, incluidos los cuatro documentos y los logs originales, y se copió el ZIP a `_local/imports/` verificando su hash. Los documentos anteriores contenían literalmente algunos `?` por pérdida de acentos: se conservaron tal cual y se redactaron copias legibles basadas en su contenido y la evidencia.

El clúster `.pg-local` se detuvo, trasladó a `_local/temporary/pg-local`, arrancó de nuevo y se verificaron `data_directory`, versión, instalación y pruebas. Quedó detenido. El respaldo contiene su estado previo; no se incorporó a Git.

## Estructura final

```text
LawWare/
├── README.md                         # mapa local, raíz sin Git
├── LawWare-backend/                  # repositorio independiente
│   ├── README.md, AGENTS.md, .env.example, .gitignore
│   ├── docs/CONTRATOS_INTEGRACION.md
│   └── src/                          # módulos y límites preparados, sin aplicación
├── LawWare-frontend/                 # repositorio independiente
│   ├── README.md, AGENTS.md, .env.example, .gitignore
│   ├── docs/FLUJO_PRIVACIDAD.md
│   └── src/                          # estructura preparada, sin aplicación
├── LawWare-database/                 # repositorio independiente
│   ├── sql/baseline/, sql/migrations/v2.1/
│   ├── tests/adversarial/, audit/, concurrency/, fixtures/, integration/, privacy/, security/
│   ├── diagrams/, scripts/utilities/, scripts/validation/
│   └── docs/test-results/            # evidencia anonimizada y catálogo
├── LawWare-context/                  # repositorio independiente
│   ├── architecture/, decisions/, reports/
│   └── README.md, AGENTS.md, .gitignore
└── _local/
    ├── backups/                      # ZIP y manifiestos de hashes
    ├── imports/                      # ZIP y estado anterior intacto
    └── temporary/                    # clúster detenido y logs locales originales
```

## Registro de archivos

El manifiesto `_local/backups/registro_copias.json` conserva cada origen, destino y hash de las 19 copias iniciales. Las copias iniciales de logs en el repositorio se sustituyeron después por versiones anonimizadas con nombres distintos; los originales siguen intactos en `_local/imports/estado_previo/`. Las rutas siguientes se refieren al estado anterior y al destino canónico:

| Origen | Destino y tratamiento |
|---|---|
| `modelo/baseline_v2/*.sql` | `LawWare-database/sql/baseline/`, idénticos byte a byte. |
| `modelo/01_migracion_trazabilidad_auditoria_v2_1.sql` | `LawWare-database/sql/migrations/v2.1/`, idéntica a la versión corregida anterior; SHA-256 `6a1573dd4c4eb4acaa56564b4f2de38771b72d854379d118ffe2def0958fc12e`. |
| `modelo/02_pruebas_adversariales_trazabilidad_v2_1.sql` | `LawWare-database/tests/adversarial/`, batería corregida preservada. |
| `modelo/06_pruebas_roles_runtime.sql` | `LawWare-database/tests/integration/`, batería histórica preservada. |
| `modelo/03_modelo_dbdiagram_v2_1.dbml` | `LawWare-database/diagrams/modelo_dbdiagram_v2_1.dbml`; solo se actualizó la nota de alcance de representación. |
| `modelo/04_consultas_forenses.sql` | `LawWare-database/scripts/utilities/`, preservado. |
| `modelo/05_guia_integracion_nodejs_render.md` | `LawWare-database/docs/guia_integracion_historica.md`, se reparó la nota inicial dañada y su referencia. |
| `modelo/07_comparar_catalogo.py`, `99_validacion_estatica.py`, `ejecutar_validacion_local.py` | `LawWare-database/scripts/validation/`, rutas adaptadas; el ejecutor ahora verifica ubicación, imprime versión y hashes y ejecuta las suites nuevas. |
| `modelo/08_pruebas_concurrencia.py` | `LawWare-database/tests/concurrency/`, rutas adaptadas, fixture explícito y dos escenarios ejecutables en bases separadas. |
| `modelo/resultado_validacion_estatica_v2_1.json` | `LawWare-database/docs/test-results/resultado_validacion_estatica_historico.json`, preservado como resultado estático histórico. |
| `modelo/DICTAMEN_DE_REVISION.md` | `LawWare-context/reports/DICTAMEN_DE_REVISION_HISTORICO.md`, preservado. |
| `docs/VALIDACION_LOCAL.md` | Original preservado en `_local/imports/estado_previo/`; nueva versión legible y actualizada en `LawWare-database/docs/VALIDACION_LOCAL.md`. |
| `docs/HALLAZGOS_AUDITORIA.md`, `ESTADO_APROBACION.md` | Originales preservados; nuevas versiones legibles en `LawWare-context/reports/`. |
| `docs/DECISIONES_PENDIENTES.md` | Original preservado; nueva versión legible en `LawWare-context/decisions/`. |
| `docs/evidencia/*.log` | Originales preservados en `_local/imports/estado_previo/`; copias anonimizadas en `LawWare-database/docs/test-results/`, con hashes de original y copia. |
| `paquete_original/`, `modelo/`, `docs/`, README y plan anteriores | Preservados en `_local/imports/estado_previo/` y en el respaldo. No se sustituyó el SQL corregido por el ZIP. |

Archivos nuevos principales: `tests/security/casos_independientes.py`, `tests/privacy/casos_independientes.py`, `tests/audit/casos_independientes.py`, `tests/fixtures/`, `tests/safe_test_cluster.py` y `scripts/validation/08_comparar_restricciones.py`; el inventario detallado de restricciones y los logs finales están en `docs/test-results/`. Se crearon README, AGENTS, configuraciones de ejemplo, `.gitignore`, contratos y estructura de módulos en los repositorios correspondientes. Backend y frontend no son aplicaciones ejecutables todavía.

## Resultados y discrepancias

- Dos ejecuciones completas finales pasaron con PostgreSQL 17.11 y registraron versión, comandos y hashes. Cada ejecución realizó 15 casos de seguridad, 10 de privacidad y 5 de auditoría individualmente y en orden inverso. Las carreras de vínculo y revocación se ejecutaron individualmente en bases distintas, con sesiones separadas y bloqueos observados. Las baterías históricas siguen siendo integradas y algunos pasos dependen de los anteriores.
- El DDL reorganizado coincide byte a byte con el baseline y la migración corregida anteriores. El catálogo y DBML coinciden en 31 tablas, 223 columnas, 50 FK, nulabilidad, 31 PK y 57 índices únicos no parciales. SQL contiene 88 índices, de los cuales 31 no se representan en DBML; el diagrama tampoco expresa 60 CHECK, 78 defaults ni las acciones de 50 FK. Estos detalles están inventariados; no se afirma equivalencia semántica completa.
- El contexto `app.user_id` se puede manipular con SQL arbitrario. Se verificó que un contexto transaccional se limpia tras rollback en una conexión de prueba, pero no existe una prueba de pool real. La cobertura RLS y de permisos es parcial; Auth0, logger externo, Render, Vercel, recuperación de respaldo y aprobación jurídica permanecen pendientes.
- Los logs históricos no incorporaban hashes de fuentes ni versión exacta de servidor; las ejecuciones nuevas sí los registran. Las copias de logs de los repositorios fueron anonimizadas y los originales se conservaron fuera de Git. Un examen por patrones no garantiza por sí solo ausencia de secretos y debe repetirse antes de publicar.

## Estado Git

Los cuatro repositorios tienen remoto `origin` hacia sus URL oficiales y rama local sin commits `main`. Todos los archivos nuevos están sin seguimiento; no hay commits, cambios staged, push ni fusiones. La referencia `origin/main` no existe todavía porque los repositorios remotos están vacíos:

| Repositorio | Contenido propuesto para futura publicación |
|---|---|
| `LawWare-backend` | README, AGENTS, `.gitignore`, `.env.example`, contrato y estructura modular vacía. |
| `LawWare-frontend` | README, AGENTS, `.gitignore`, `.env.example`, flujo de privacidad y estructura de interfaz vacía. |
| `LawWare-database` | SQL, DBML, pruebas, scripts, documentación, manifiestos y evidencia anonimizada. No se incorporan el clúster ni respaldos. |
| `LawWare-context` | README, AGENTS, arquitectura, decisiones, hallazgos, dictamen histórico, estado e informe. |

Antes de publicar, revisar nuevamente el inventario y su sensibilidad y obtener autorización explícita para push o fusión.
