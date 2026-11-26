# Catálogo inicial de eventos

Los eventos describen hechos que ya han ocurrido en UrbanMove. Permiten actualizar el historial y preparar los datos analíticos sin que esas actividades bloqueen las operaciones principales. Esta tabla define responsabilidades iniciales; los esquemas detallados se concretarán durante la implementación.

| Evento | Productor propuesto | Consumidores principales | Hecho que representa |
|---|---|---|---|
| UserRegistered | Identidad | Perfiles, Historial, ingesta analítica | Se ha registrado una identidad. |
| VehiclePurchased | Inventario | Disponibilidad, Historial, ingesta analítica | Se ha incorporado un vehículo. |
| VehicleReserved | Reservas | Historial, ingesta analítica | Se ha creado una reserva para un vehículo. |
| ReservationCancelled | Reservas | Historial, ingesta analítica | Se ha cancelado una reserva. |
| ReservationExpired | Reservas | Historial, ingesta analítica | Una reserva ha caducado. |
| RideStarted | Viajes | Historial, ingesta analítica | Ha comenzado un viaje. |
| RideFinished | Viajes | Historial, ingesta analítica | Ha terminado un viaje. |
| VehicleBatteryLow | Disponibilidad | Mantenimiento, Historial, ingesta analítica | Se ha detectado que un vehículo requiere atención por batería baja. |
| VehicleWithdrawnForMaintenance | Disponibilidad | Mantenimiento, Historial, ingesta analítica | Se ha retirado un vehículo de la oferta pública. |
| VehicleReturnedToService | Disponibilidad | Mantenimiento, Historial, ingesta analítica | Un vehículo vuelve a estar disponible. |
| DemandHotspotDetected | Analítica operacional | Órdenes de recolocación, Historial | Se ha detectado un déficit de vehículos respecto a la demanda. |
| RelocationOrderCreated | Órdenes de recolocación | Historial, ingesta analítica | Se ha creado una orden de traslado. |
| RelocationOrderCompleted | Órdenes de recolocación | Historial, ingesta analítica | Se ha completado una orden de traslado. |

Cada evento deberá incluir un identificador único, el instante en que ocurrió, el servicio productor, su tipo, la versión de su esquema y un identificador de correlación. Esto permitirá seguir un recorrido completo y reconocer un evento recibido más de una vez.

El productor definitivo de los eventos relacionados con el estado del vehículo se confirmará al definir los contratos entre Disponibilidad y Mantenimiento.