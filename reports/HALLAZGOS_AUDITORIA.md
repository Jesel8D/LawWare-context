# Hallazgos de auditoría de LawWare v2.1

Documento legible reconstruido del original preservado antes de la reorganización y actualizado con las pruebas de esta etapa. La severidad es técnica, no jurídica. La evidencia y el detalle de comandos residen en `LawWare-database/docs/`.

| Hallazgo | Resolución o estado |
|---|---|
| La migración referenciaba el token de vinculación como `uuid`, pero la función lo recibe como `text`. | Firma corregida y reinstalada localmente. |
| Los triggers de privacidad requerían bloqueos sin permisos para el servicio. | Tres funciones de trigger se ejecutan como propietario confiable con `search_path` fijado; el flujo se probó localmente. |
| Era posible preparar revocaciones duplicadas sin sellar en una transacción. | La comprobación incluye revocaciones no selladas; existen pruebas negativas y concurrentes locales. |
| Podía registrarse una presentación autenticada para otro titular y la función devolvía un sello incompleto. | Se comprueba el contexto y la completitud; hay pruebas negativas locales. El contexto sigue siendo falsificable por SQL arbitrario. |
| Faltaba auditoría de definiciones RBAC y finalidades; el rol escritor podía insertar eventos directamente. | Se añadieron triggers y se revocó la inserción directa. Las pruebas observan eventos, actor, correlación, publicación y rollback. |
| Los validadores estáticos y dos casos históricos tenían rutas o condiciones de prueba incorrectas. | Se corrigieron y se añadieron casos con fixtures propios. Las baterías históricas completas siguen conteniendo pasos encadenados. |
| La comparación previa solo cubría parte de la semántica del DBML. | Se compararon nulabilidad, PK y unicidad declarada y se inventariaron CHECK, defaults, índices y acciones FK omitidos; la equivalencia completa permanece pendiente. |

## Riesgos abiertos

- `app.user_id`, servicio y correlación son contexto suministrado por el backend; la prueba de manipulación demuestra que un cliente con SQL arbitrario puede suplantar el usuario. Autenticación Auth0, autorización y aislamiento real del pool necesitan integración y pruebas separadas.
- El servicio de privacidad tiene acceso amplio; el compromiso del propietario o superusuario supera RLS y append-only. Verificar funciones privilegiadas y permisos antes de cualquier despliegue.
- La lista de claves de auditoría no garantiza que valores libres no contengan secretos. Los intentos fallidos y las lecturas no quedan registrados por triggers en la misma transacción: se necesita un logger externo y controles sobre sus datos.
- El hash es una comprobación de integridad, no una firma ni prueba de lectura o identidad. El formato de serialización debe mantenerse documentado y revisarse ante cambios de versión.
- Algunas FK con `ON DELETE SET NULL` pueden entrar en conflicto con auditoría append-only. La política de anonimización y eliminación sigue sin decidirse.
- No se han probado todas las políticas RLS, las combinaciones de privilegios, otros niveles de aislamiento ni restauración. No extrapolar PostgreSQL 17.11 local a Render.
