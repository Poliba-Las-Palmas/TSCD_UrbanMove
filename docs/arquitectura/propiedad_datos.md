# Propiedad de los datos

Esta tabla identifica la fuente autorizada de cada dato principal. Un servicio puede consultar datos de otro mediante su API o recibir sus eventos, pero solo el propietario modifica el dato original. La tecnología de almacenamiento se decidirá después de comparar las alternativas arquitectónicas.

| Servicio propietario | Datos bajo su responsabilidad | Otros servicios que los utilizan |
|---|---|---|
| Identidad | Credenciales, roles y sesiones de acceso. | Clientes y servicios que comprueban permisos. |
| Perfiles de usuario | Datos personales, preferencias y baja lógica del perfil. | Reservas, Viajes e Historial reciben el identificador del usuario. |
| Inventario de recursos | Identificador del vehículo, tipo, modelo, fecha de compra y baja administrativa. | Disponibilidad y Mantenimiento consultan el activo; Analítica recibe sus eventos. |
| Disponibilidad | Estado operativo, zona actual y adjudicación vigente del vehículo. | Reservas, Viajes, Mantenimiento y Recolocación solicitan cambios mediante su API. |
| Reservas | Titular, vehículo, estado y vencimiento de cada reserva. | Viajes comprueba la reserva; Historial y Analítica reciben sus eventos. |
| Viajes | Usuario, vehículo, origen, destino, inicio, fin, duración y coste preliminar del viaje. | Historial y Analítica reciben sus eventos. |
| Historial | Proyección cronológica de los eventos relevantes para cada consulta. | Los clientes consultan la proyección mediante su API. |
| Órdenes de mantenimiento | Incidencia, motivo, intervención, responsable y estado de cada orden. | Disponibilidad recibe las solicitudes de retirada y reincorporación; Historial y Analítica reciben eventos. |
| Órdenes de recolocación | Origen, destino, cantidad, asignación y estado de cada orden. | Disponibilidad recibe las solicitudes de traslado; Historial y Analítica reciben eventos. |
| Analítica operacional | Agregados publicados, indicadores de demanda y resultados de las reglas de decisión. | El dashboard consulta los indicadores; Recolocación recibe las decisiones que generan órdenes. |

## Regla de actualización

Cuando termina un viaje, Viajes registra su resultado y solicita a Disponibilidad que cambie la zona y el estado operativo del vehículo. Viajes no escribe directamente los datos de Disponibilidad. La operación debe poder recuperarse si alguno de los dos servicios falla durante el proceso.

Historial y Analítica mantienen datos derivados de los eventos. Si reciben dos veces el mismo evento, utilizan su identificador para evitar duplicarlo en el resultado lógico.