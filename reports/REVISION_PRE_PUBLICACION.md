# Revisión previa a los primeros commits

Fecha: 16 de noviembre de 2026. **Recomendación: APTO PARA PRIMER COMMIT**, con las limitaciones indicadas. Esta recomendación se refiere a la incorporación local al control de versiones; no autoriza commits, push, despliegue ni aprobación de producción.

## Estado e inventario

Los cuatro repositorios apuntan a sus remotos oficiales y tienen `main` local sin commits. Los archivos de esta etapa siguen sin seguimiento y no hay cambios en staging. La referencia `origin/main` aún no existe porque los remotos se encontraban vacíos. La carpeta raíz y `_local/` no son parte de ninguno de los cuatro repositorios.

| Repositorio | Archivos candidatos | Alcance del primer commit propuesto | Mensaje sugerido |
|---|---:|---|---|
| `LawWare-backend` | 16 | README, instrucciones, configuración de ejemplo, contrato y directorios modulares preparados. No hay backend ejecutable. | `chore: prepare backend structure and integration contracts` |
| `LawWare-frontend` | 13 | README, instrucciones, configuración de ejemplo, contrato del flujo de privacidad y directorios preparados. No hay frontend ejecutable. | `chore: prepare frontend structure and privacy flow` |
| `LawWare-database` | 36 | SQL v2.1 corregido, DBML, pruebas, scripts, documentación y evidencia anonimizada. | `feat: add locally validated LawWare v2.1 database model` |
| `LawWare-context` | 11 | Arquitectura, decisiones, dictámenes, reorganización y esta revisión. | `docs: document architecture and local validation status` |

El manifiesto completo y verificable de rutas, tamaños y SHA-256, incluido el hash de este informe, se generó fuera de los repositorios en `_local/temporary/auditoria_prepublicacion/MANIFIESTO_PUBLICACION.json`. Se mantiene fuera de Git para evitar una referencia circular de un archivo a su propio hash. Los archivos enumerados son candidatos, no están confirmados ni publicados; el manifiesto debe regenerarse si alguno cambia.

## Seguridad de los archivos

Se examinó cada archivo candidato con `detect-secrets` 1.5.0, sin verificaciones de red, y con patrones adicionales para JWT, tokens de GitHub y nube, claves privadas, URL PostgreSQL con credenciales, rutas personales y correos no reservados. No se detectaron credenciales reales ni datos personales por esos métodos. Hubo 13 alertas en base de datos, revisadas como falsos positivos: 12 son hashes SHA-256 del manifiesto de evidencia y una es el nombre de una comprobación estática sobre el hash de un token. El examen no garantiza que no exista ningún secreto y debe repetirse sobre los archivos definitivos antes de publicar.

Los correos de los fixtures usan `example.invalid`; las direcciones `192.0.2.x` son ejemplos sintéticos y `127.0.0.1` es localhost. Las seis copias de logs incluidas son evidencia anonimizada: se contrastaron con sus hashes y se revisaron con los mismos análisis. Los originales históricos y el respaldo se conservaron intactos en `_local/` y no se incorporan a Git.

Cada `.gitignore` se comprobó por separado con `git check-ignore`. Se excluyen `.env` reales, `_local/`, clústeres PostgreSQL, `pgdata`, respaldos, volcados, claves privadas, archivos temporales, credenciales locales habituales y logs arbitrarios. En el repositorio de base de datos solo se permiten explícitamente las seis copias de evidencia revisadas; cualquier log nuevo permanece ignorado. Los `.env.example` no contienen credenciales y sí son candidatos.

## Integridad, rutas y reproducibilidad

- Los tres archivos baseline y la migración v2.1 coinciden byte a byte con la versión corregida y validada antes de la reorganización. No se reemplazó el SQL por el del ZIP. Se verificaron el respaldo y los logs históricos originales frente a sus hashes, así como los hashes de las copias anonimizadas.
- Se incorporó `.gitattributes` a cada repositorio para conservar los bytes entre plataformas; se comprobó que los filtros Git no alteran los candidatos. En particular, la migración y los baselines usan finales de línea distintos que se preservarán, junto con sus hashes.
- Los scripts localizan `psql` mediante `PSQL` o `PATH`, con un fallback de Windows, y calculan sus rutas desde la ubicación del repositorio. Las comprobaciones de seguridad rechazan conexiones no locales y las variables `PGHOSTADDR` no local, `PGSERVICE`, `PGSERVICEFILE` y `PGOPTIONS` antes de conectar. El directorio de datos debe ser el clúster aislado esperado dentro de `_local`.
- Se simuló un checkout Git sin crear commits, con `core.autocrlf=true`, en un directorio diferente. Los 36 archivos candidatos de base de datos conservaron sus bytes. En ese checkout se inicializó un clúster local nuevo, se encontró `psql` en `PATH` sin `PSQL`, y la instalación y suite finalizaron correctamente en PostgreSQL 17.11. La prueba se hizo en Windows en esta máquina; Linux, macOS y otra computadora física siguen sin probarse.
- Se revisaron enlaces locales, README, AGENTS, DBML y documentación; el vínculo desde la guía histórica al repositorio de contexto se corrigió para repositorios independientes. Los enlaces entre repositorios de GitHub dependerán de la publicación posterior de `main`.
- Los logs de la validación anterior conservan los hashes de los scripts de ese momento. Los ajustes de portabilidad y seguridad de esta revisión cambian algunos hashes de scripts, no el DDL; la ejecución del checkout simulado es evidencia adicional de la versión preparada ahora.

## Estado técnico y riesgos pendientes

Las dos ejecuciones anteriores de PostgreSQL 17.11 y la prueba del checkout simulado son validaciones locales. La cobertura de RLS y permisos no es completa, las baterías históricas mantienen algunos pasos encadenados y el DBML no representa todas las restricciones e índices del SQL. `app.user_id` es modificable por un cliente con SQL arbitrario y no constituye identidad fuerte; Auth0, el pool real y el registro externo de fallos siguen pendientes. La retención y las demás decisiones de privacidad requieren confirmación.

Render está previsto para backend y PostgreSQL administrado; Vercel, para frontend; Auth0, para autenticación. Ninguno se ha integrado o validado en esta fase. PostgreSQL 17.11 es la versión comprobada localmente, no una afirmación sobre el servicio de Render. No hay aprobación de producción, seguridad integral ni cumplimiento legal.

No se ha creado ningún commit, realizado push, fusión o despliegue. Antes de los commits y la publicación se requiere la autorización del usuario y una comprobación final de que el manifiesto sigue coincidiendo con el árbol de trabajo.
