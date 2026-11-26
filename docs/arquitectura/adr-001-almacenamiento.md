# ADR-001 — Almacenamiento operacional

**Estado:** Propuesta pendiente de consulta con el docente y de pruebas en AWS Academy.

## Problema

UrbanMove necesita conservar los datos de diez microservicios independientes. Disponibilidad debe impedir que dos solicitudes concurrentes adjudiquen el mismo vehículo. Reservas y Viajes deben poder recuperar una operación interrumpida entre servicios. El equipo dispone de dos cuentas Learner Lab con presupuesto limitado y realiza pruebas en sesiones intermitentes.

## Alternativa A — DynamoDB bajo demanda

Cada microservicio con estado dispone de sus propias tablas. Disponibilidad adjudica un vehículo mediante una escritura condicionada a que su estado siga siendo AVAILABLE. Las operaciones del mismo servicio que deban persistirse juntas se agrupan mediante una transacción cuando corresponda.

**Ventajas:** el consumo depende principalmente de lecturas y escrituras realizadas; una escritura condicional permite probar directamente la adjudicación exclusiva; no hay una instancia de base de datos que mantener encendida durante las pausas.

**Costes y límites:** deben definirse los patrones de consulta y sus índices antes de implementar cada servicio. Las transacciones, índices y operaciones adicionales aumentan el consumo. La transacción de un servicio no resuelve por sí sola una operación que atraviesa Reservas y Disponibilidad.

Como referencia para us-east-1, 100 000 escrituras de hasta 1 KB y 100 000 lecturas fuertemente consistentes representan aproximadamente 0,075 USD de peticiones, antes de almacenamiento, transacciones, índices y otros servicios.

## Alternativa B — Amazon RDS para PostgreSQL

Una instancia PostgreSQL aloja bases de datos lógicas separadas. Cada microservicio utiliza exclusivamente su base de datos. Las restricciones y transacciones SQL protegen las modificaciones dentro de cada servicio.

**Ventajas:** facilita las consultas relacionales y permite registrar en una misma transacción local el cambio de datos y el evento pendiente de publicación.

**Costes y límites:** se paga por la instancia mientras funciona y por el almacenamiento aprovisionado. Detenerla reduce el coste de cómputo, pero el almacenamiento sigue generando cargos y la instancia se inicia automáticamente después de siete días detenida. Una transacción SQL tampoco abarca de forma segura las llamadas entre microservicios.

Para ocho horas de prueba, una instancia pequeña representa aproximadamente 0,15 USD de cómputo, más almacenamiento y posibles cargos adicionales. La clase disponible y el precio exacto se comprobarán antes de crearla en el Learner Lab.

## Propuesta y criterio de validación

Se propone empezar por DynamoDB bajo demanda por el patrón de uso intermitente y porque permite demostrar la adjudicación exclusiva mediante una escritura condicional. La elección final depende de las consultas necesarias, de la prueba con dos solicitudes concurrentes y del parecer del docente.

Con cualquiera de las alternativas, Disponibilidad conserva la autoridad sobre el estado operativo. Reservas registra la operación y debe reintentar o compensar un fallo producido entre ambos servicios. Se probará expresamente que una compensación tardía no libera una adjudicación posterior.

## Fuentes para revisar las estimaciones

- https://aws.amazon.com/dynamodb/pricing/
- https://aws.amazon.com/rds/postgresql/pricing/
- https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html
- https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_StopInstance.html