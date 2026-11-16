# Estado de aprobación de LawWare v2.1

**Validado en desarrollo local, con observaciones.** No existe aprobación de producción ni certificación de seguridad o cumplimiento legal. Versión legible reconstruida del documento anterior preservado y actualizada con las pruebas ejecutadas en esta etapa.

| Clasificación | Control | Alcance |
|---|---|---|
| Aprobado local | Instalación y reproducibilidad | Dos ejecuciones reorganizadas, bases nuevas, PostgreSQL 17.11, `ON_ERROR_STOP`, comandos y hashes registrados. |
| Aprobado local | Privacidad y auditoría | Casos positivos y negativos descritos en las evidencias; hashes recalculados, correlación, eventos y rollback. No constituyen prueba jurídica. |
| Aprobado local | Concurrencia | Vínculo y revocación ejecutados individualmente en bases y sesiones separadas, con esperas observadas, bajo READ COMMITTED. |
| Aprobado local parcialmente | Roles y RLS | Los ocho roles se ejercitaron en escenarios concretos; dos identidades, ausencia/manipulación de contexto, y escritura propia/ajena de progreso y evaluación. No cubre todas las combinaciones ni tablas. |
| Aprobado local parcialmente | Independencia | Los casos nuevos de privacidad, seguridad y auditoría se ejecutaron individualmente y en orden inverso; las baterías históricas incluyen pasos encadenados. |
| Conforme por revisión de catálogo | DBML representado | 31 tablas, 223 columnas, 50 FK, nulabilidad, 31 PK y 57 índices únicos no parciales coinciden. Los CHECK, defaults, acciones FK y 31 índices omitidos del DBML están inventariados, no representados de forma equivalente. |
| Pendiente de integración | Auth0, backend, pool y logs externos | No se implementaron ni ejecutaron. `app.user_id` es manipulable por SQL y no acredita identidad. |
| No probado | PostgreSQL 16, Render, Vercel, otros aislamientos, carga y restauración | No se ha evaluado. |
| Bloqueado por un fallo | Ninguno documentado en la suite final | La validación final terminó correctamente; las limitaciones anteriores siguen abiertas. |

La evidencia técnica y los comandos canónicos están en `LawWare-database/docs/VALIDACION_LOCAL.md`. Los documentos y logs históricos originales se conservaron fuera de Git en el respaldo local.
