# Arquitectura prevista

LawWare es una plataforma educativa sobre ética y legislación en tecnologías de la información. La arquitectura prevista usa Next.js y TypeScript en frontend, Node.js y TypeScript en backend, Auth0 para autenticación y PostgreSQL para persistencia. Vercel está previsto para frontend y Render para backend y PostgreSQL administrado. Estas son decisiones de arquitectura, no evidencia de integración o despliegue.

El esquema v2.1 está definido por el SQL de `LawWare-database`: 31 tablas, 223 columnas y 50 claves foráneas comprobadas localmente. PostgreSQL 17.11 es la versión probada en desarrollo local; la versión y características de un servicio administrado se verificarán por separado.

El backend deberá validar credenciales Auth0, resolver la identidad interna y establecer el contexto transaccional de PostgreSQL; el frontend consumirá contratos de backend. El GUC `app.user_id` puede ser alterado por cualquier cliente con SQL arbitrario y no es por sí mismo una identidad confiable. Las operaciones fallidas y lecturas sensibles necesitan registro externo a la transacción de negocio.

Repositorios: [database](https://github.com/Jesel8D/LawWare-database), [backend](https://github.com/Jesel8D/LawWare-backend), [frontend](https://github.com/Jesel8D/LawWare-frontend) y [context](https://github.com/Jesel8D/LawWare-context).
