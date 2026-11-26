# Mapa inicial de las API

**Estado:** Propuesta para revisión conjunta. Las rutas representan las operaciones principales; los payloads, permisos y códigos de respuesta se definirán en los contratos de cada servicio.

| Servicio | Operaciones principales propuestas | Clientes u otros servicios que las utilizan |
|---|---|---|
| Identidad | Registrar una cuenta, iniciar sesión y comprobar el rol de acceso. | Clientes de usuarios y operaciones; servicios protegidos. |
| Perfiles de usuario | Consultar, modificar y dar de baja lógicamente un perfil. | Cliente de usuarios e Identidad durante el alta. |
| Inventario de recursos | Incorporar, consultar y dar de baja administrativamente un vehículo. | Cliente de operaciones y Disponibilidad. |
| Disponibilidad | Consultar vehículos por zona y tipo; adjudicar un vehículo; solicitar un cambio de estado o zona. | Clientes, Reservas, Viajes, Mantenimiento y Recolocación. |
| Reservas | Crear, consultar, confirmar y cancelar una reserva; procesar su vencimiento. | Cliente de usuarios, Viajes y función de caducidad. |
| Viajes | Iniciar, consultar y finalizar un viaje. | Cliente de usuarios y servicios que consultan viajes activos. |
| Historial | Consultar el historial autorizado por usuario o vehículo. | Clientes de usuarios y operaciones. |
| Órdenes de mantenimiento | Consultar y gestionar incidencias, retiradas, intervenciones y reincorporaciones. | Cliente de operaciones y función de detección. |
| Órdenes de recolocación | Crear, consultar, asignar, iniciar y completar órdenes. | Cliente de operaciones y Analítica operacional. |
| Analítica operacional | Consultar indicadores de reservas, viajes, flota y demanda; evaluar déficits y excedentes. | Dashboard y proceso que propone recolocaciones. |

## Reglas comunes previstas

- Cada operación que modifica datos tiene un servicio responsable.
- Las operaciones protegidas comprueban la identidad y el rol del solicitante.
- Las operaciones que pueden repetirse por un reintento conservan un identificador de solicitud.
- Las peticiones y los eventos relacionados comparten un identificador de correlación.
- Los errores de autorización, validación y conflicto se distinguen en las respuestas de las API.