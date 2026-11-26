# Ciclo de vida de los vehículos

## Propuesta de responsabilidad

Inventario es propietario de los datos administrativos del vehículo, incluida su baja definitiva. Disponibilidad es propietario del estado operativo y de la zona actual. Reservas, Viajes y Mantenimiento solicitan cambios mediante operaciones explícitas de Disponibilidad.

Disponibilidad comprueba el estado actual, el identificador de la operación y la versión del vehículo antes de aceptar una transición. Si la condición ya no se cumple, rechaza el cambio. Una petición repetida con el mismo identificador conserva el resultado de la primera ejecución.

Esta distribución permite comprobar en un único lugar si un vehículo puede reservarse, utilizarse o retirarse. La coordinación de una operación que afecta a dos servicios requiere además mecanismos de recuperación e idempotencia.

## Estados exigidos

| Estado | Significado |
|---|---|
| AVAILABLE | El vehículo está en una zona permitida y puede adjudicarse a un usuario. |
| RESERVED | Una reserva vigente tiene adjudicado temporalmente el vehículo. |
| IN_USE | Un usuario está realizando un viaje con el vehículo. |
| LOW_BATTERY | El vehículo necesita atención por batería baja y queda fuera de la oferta pública. |
| WITHDRAWN | El personal ha retirado el vehículo de la vía pública. |
| CHARGING | Se está recargando el vehículo. |
| REPAIRING | Se está reparando el vehículo. |
| READY_FOR_SERVICE | La intervención ha terminado y se espera la reincorporación. |
| RETIRED | El vehículo ha sido dado de baja definitivamente. |

## Transiciones principales propuestas

| Estado inicial | Acción y condición necesaria | Servicio que solicita el cambio | Estado final | Evento de dominio |
|---|---|---|---|---|
| AVAILABLE | Adjudicar a una reserva vigente; el vehículo sigue libre. | Reservas | RESERVED | VehicleReserved |
| AVAILABLE | Iniciar un viaje directo; el vehículo sigue libre. | Viajes | IN_USE | RideStarted |
| RESERVED | Iniciar el viaje del titular de la reserva vigente. | Viajes | IN_USE | RideStarted |
| RESERVED | Cancelar la reserva correspondiente. | Reservas | AVAILABLE | ReservationCancelled |
| RESERVED | Caducar una reserva sin confirmar y vencida. | Reservas, tras el aviso de la Lambda | AVAILABLE | ReservationExpired |
| IN_USE | Finalizar el viaje en una zona permitida; el vehículo puede seguir en servicio. | Viajes | AVAILABLE | RideFinished |
| IN_USE | Finalizar el viaje; la batería está por debajo del umbral. | Viajes | LOW_BATTERY | RideFinished y VehicleBatteryLow |
| AVAILABLE | Recibir una medición válida por debajo del umbral de batería. | Detección de mantenimiento | LOW_BATTERY | VehicleBatteryLow |
| AVAILABLE | Retirar un vehículo por avería o revisión. | Órdenes de mantenimiento | WITHDRAWN | VehicleWithdrawnForMaintenance |
| LOW_BATTERY | Registrar la retirada física. | Órdenes de mantenimiento | WITHDRAWN | VehicleWithdrawnForMaintenance |
| WITHDRAWN | Iniciar una recarga registrada en la orden. | Órdenes de mantenimiento | CHARGING | MaintenanceChargingStarted* |
| WITHDRAWN | Iniciar una reparación registrada en la orden. | Órdenes de mantenimiento | REPAIRING | MaintenanceRepairStarted* |
| WITHDRAWN | Finalizar una revisión que no requiere intervención adicional. | Órdenes de mantenimiento | READY_FOR_SERVICE | MaintenanceReviewFinished* |
| CHARGING | Completar la recarga y verificar el nivel de batería. | Órdenes de mantenimiento | READY_FOR_SERVICE | MaintenanceChargingFinished* |
| REPAIRING | Completar la reparación y verificar la ausencia de avería. | Órdenes de mantenimiento | READY_FOR_SERVICE | MaintenanceRepairFinished* |
| READY_FOR_SERVICE | Reincorporar el vehículo en una zona permitida. | Órdenes de mantenimiento | AVAILABLE | VehicleReturnedToService |
| WITHDRAWN | Dar de baja definitivamente el activo. | Inventario | RETIRED | VehicleRetired* |

Los eventos marcados con * son ampliaciones propuestas para poder reconstruir los cambios de estado. Se documentarán sus productores y consumidores antes de implementarlos.

## Reglas que deben probarse

- Solo un usuario puede obtener la transición de AVAILABLE a RESERVED o IN_USE para un vehículo concreto.
- La cancelación o caducidad libera únicamente la adjudicación de la reserva correspondiente.
- Un vehículo en IN_USE no cambia de estado operativo durante el viaje. Las incidencias recibidas durante su uso quedan registradas y se evalúan al finalizarlo.
- Un vehículo retirado, en recarga, en reparación o dado de baja no aparece en la consulta de vehículos disponibles.
- RETIRED es un estado terminal. Su aplicación requiere que no exista una reserva o un viaje activo.
- Cada transición aceptada conserva el identificador de correlación del flujo que la originó.