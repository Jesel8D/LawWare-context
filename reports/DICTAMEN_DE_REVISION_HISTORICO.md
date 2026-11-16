# Dictamen de revisión cruzada — LawWare trazabilidad y auditoría v2.1

**Método:** revisión desde tres perspectivas complementarias, con criterios que deben cumplirse simultáneamente:

1. **Reconstrucción funcional:** ante una solicitud ARCO, ¿podemos identificar el aviso mostrado, la decisión exacta, finalidades concedidas/rechazadas, vínculo con cuenta y revocaciones?
2. **Integridad adversarial:** ¿qué pasa si se intenta sellar un consentimiento incompleto, cambiar el hash, vincular un prerregistro ajeno, duplicar una revocación o modificar la bitácora?
3. **Seguridad y consistencia técnica:** ¿el DBML coincide con el SQL, hay referencias válidas, permisos adecuados y una fuente externa de eventos fallidos?

## Hallazgos y resolución

| Hallazgo | Resolución en v2.1 | Estado |
|---|---|---|
| Consentimiento sin finalidades ni sello | Restricción diferida al cierre de transacción, función de sellado con cálculo SHA-256 | Definido en SQL, sin probar en PostgreSQL |
| Revocación sin evento original | FK autoref + verificación de titular, finalidad y no duplicación | Definido en SQL |
| Titular de prerregistro débil | SHA-256 de secreto aleatorio, validación de 24h, vinculación única tras Auth0 | Definido; integración pendiente |
| Correlación entre eventos | `id_correlacion`, `id_peticion`, `servicio_origen` y `rol_sesion_db` | Definido |
| Auditoría insuficiente | Eventos de roles, módulo, contenido, publicaciones, personaje, avisos, consentimiento y solicitudes ARCO | Definido |
| Registro de información personal en logs | Restricción de claves/tamaño de JSONB y datos de auditoría acotados | Definido, requiere revisión de backend |
| El DBML v2 tenía 207 columnas mientras el SQL tenía 208 | Se recuperó `nombre_snapshot` faltante; nuevo DBML con 223 columnas | **Comprobado estáticamente** |

## Restricciones declaradas que no equivale a evidencia operacional

- Una ejecución real de SQL puede detectar errores que un análisis estático no encuentra.
- No se han probado carreras entre sesiones, transacciones o roles de Render.
- La variable `app.user_id` no es identidad fuerte si una conexión SQL está comprometida.
- `SHA-256` ofrece comprobación de integridad, no firma digital, prueba de lectura ni no repudio.
- Los `SELECT` sensibles y los errores revertidos por transacciones requieren logs de Node.js/Auth0/Render externos a las tablas de negocio.
- Un cambio de consentimiento a una nueva versión del aviso todavía requiere definición de **estado efectivo por finalidad**.
- La duración de 24 horas en prerregistro es un **supuesto de diseño provisional**, no una norma jurídica.

## Dictamen

**Aprobado para continuar con integración y pruebas (solo revisión estructural).**

**No aprobado para producción ni como evidencia jurídica suficiente hasta que pasen las pruebas reales** y se documenten las políticas de privacidad y retención.
