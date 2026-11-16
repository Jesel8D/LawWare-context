# Decisiones pendientes de LawWare v2.1

Versión legible reconstruida a partir del documento local preservado antes de la reorganización; no añade reglas jurídicas ni plazos de retención.

1. Definir la vigencia por finalidad ante decisiones posteriores y versiones distintas del aviso, y su relación con revocaciones anteriores.
2. Confirmar o sustituir el plazo técnico provisional de 24 horas para decisión y vinculación. No se presenta como plazo legal.
3. Definir conservación, minimización, anonimización y eliminación de IP, hashes de token, evidencia y auditoría; resolver la interacción de `ON DELETE SET NULL` con auditoría append-only.
4. Confirmar las identidades y el alcance autorizado de los servicios de privacidad, administración y auditoría y quién puede realizar consultas forenses.
5. Confirmar los textos legales, las finalidades que requieren consentimiento y el tratamiento de solicitudes de privacidad con las personas responsables.
6. Definir logs externos de fallos y lecturas, control de acceso, credenciales, aislamiento del pool y proxies de confianza.
7. Para datos existentes, diseñar una migración separada con backfill, conciliación y tratamiento de evidencia histórica.
8. Verificar por separado las capacidades reales del proveedor, TLS, respaldo, restauración y auditoría de infraestructura. No se accedió a Render en esta fase.
