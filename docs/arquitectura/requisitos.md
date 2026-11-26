# Matriz de requisitos de UrbanMove

Este documento relaciona los requisitos del enunciado con los componentes responsables, las pruebas previstas y las evidencias que se recogerán. Describe el diseño previsto; los resultados de ejecución se incorporarán conforme avance la implementación.

## Microservicios obligatorios

El apartado 14 del enunciado exige los diez microservicios siguientes. Cada uno tendrá una responsabilidad principal, un contrato explícito y una unidad de despliegue independiente. Los servicios con estado serán propietarios de sus datos.

| ID | Requisito | Servicio responsable | Prueba prevista | Evidencia prevista |
|---|---|---|---|---|
| MS-01 | Autenticar y distinguir usuarios finales, personal de mantenimiento y administradores. | Identidad | Intentar acceder a una operación de mantenimiento con un usuario final y comprobar el rechazo. | Contrato de la API y resultado de la prueba de autorización. |
| MS-02 | Gestionar los datos y la baja lógica de los usuarios sin administrar sus sesiones. | Perfiles de usuario | Registrar una identidad, crear su perfil y repetir el evento de alta sin generar otro perfil. | Estado persistido y prueba de idempotencia. |
| MS-03 | Registrar bicicletas y patinetes como activos, con sus datos de compra y estado administrativo. | Inventario de recursos | Incorporar un vehículo y consultar sus datos; tramitar su baja definitiva. | Respuestas de la API y evento de incorporación. |
| MS-04 | Mantener la ubicación y el estado operativo de cada vehículo y decidir si puede adjudicarse. | Disponibilidad | Enviar dos solicitudes simultáneas para el mismo vehículo y comprobar que solo una obtiene la adjudicación. | Prueba de concurrencia y estado final del vehículo. |
| MS-05 | Crear, confirmar, cancelar y caducar reservas sin adjudicar dos veces el mismo vehículo. | Reservas | Crear una reserva, confirmarla o dejarla caducar; repetir las operaciones con la misma clave de idempotencia. | Respuestas, eventos y pruebas de concurrencia y caducidad. |
| MS-06 | Gestionar el inicio y fin de los viajes, el origen, el destino, la duración y el coste preliminar. | Viajes | Iniciar un viaje desde una reserva válida, finalizarlo en otra zona y rechazar un destino no permitido. | Respuestas, eventos y prueba del recorrido completo. |
| MS-07 | Ofrecer consultas cronológicas de reservas, viajes e intervenciones mediante un servicio independiente. | Historial | Detener el consumidor, finalizar un viaje y comprobar que recupera el evento al reiniciarse. | API propia, almacenamiento propio y prueba de recuperación. |
| MS-08 | Gestionar la retirada, intervención y reincorporación de vehículos. | Órdenes de mantenimiento | Detectar una batería baja, completar una recarga y comprobar que el vehículo vuelve a estar disponible. | Estados de la orden y del vehículo, eventos y prueba del recorrido. |
| MS-09 | Gestionar órdenes para trasladar vehículos entre zonas. | Órdenes de recolocación | Crear una orden a partir de un déficit detectado, completarla y verificar las nuevas ubicaciones. | Orden, indicador que la justifica y estados antes y después. |
| MS-10 | Exponer indicadores agregados para las decisiones operativas y el dashboard. | Analítica operacional | Consultar cuatro data marts y comprobar que un indicador de demanda produce una orden de recolocación. | Resultados de las consultas, orden generada y dashboard. |

Las pruebas y evidencias de esta tabla están previstas. Su ejecución se registrará en las issues de implementación y en la memoria final.

# Matriz de requisitos de UrbanMove

Este documento relaciona los requisitos del enunciado con los componentes responsables, las pruebas previstas y las evidencias que se recogerán. Describe el diseño previsto; los resultados de ejecución se incorporarán conforme avance la implementación.

## Microservicios obligatorios

El apartado 14 del enunciado exige los diez microservicios siguientes. Cada uno tendrá una responsabilidad principal, un contrato explícito y una unidad de despliegue independiente. Los servicios con estado serán propietarios de sus datos.

| ID | Requisito | Servicio responsable | Prueba prevista | Evidencia prevista |
|---|---|---|---|---|
| MS-01 | Autenticar y distinguir usuarios finales, personal de mantenimiento y administradores. | Identidad | Intentar acceder a una operación de mantenimiento con un usuario final y comprobar el rechazo. | Contrato de la API y resultado de la prueba de autorización. |
| MS-02 | Gestionar los datos y la baja lógica de los usuarios sin administrar sus sesiones. | Perfiles de usuario | Registrar una identidad, crear su perfil y repetir el evento de alta sin generar otro perfil. | Estado persistido y prueba de idempotencia. |
| MS-03 | Registrar bicicletas y patinetes como activos, con sus datos de compra y estado administrativo. | Inventario de recursos | Incorporar un vehículo y consultar sus datos; tramitar su baja definitiva. | Respuestas de la API y evento de incorporación. |
| MS-04 | Mantener la ubicación y el estado operativo de cada vehículo y decidir si puede adjudicarse. | Disponibilidad | Enviar dos solicitudes simultáneas para el mismo vehículo y comprobar que solo una obtiene la adjudicación. | Prueba de concurrencia y estado final del vehículo. |
| MS-05 | Crear, confirmar, cancelar y caducar reservas sin adjudicar dos veces el mismo vehículo. | Reservas | Crear una reserva, confirmarla o dejarla caducar; repetir las operaciones con la misma clave de idempotencia. | Respuestas, eventos y pruebas de concurrencia y caducidad. |
| MS-06 | Gestionar el inicio y fin de los viajes, el origen, el destino, la duración y el coste preliminar. | Viajes | Iniciar un viaje desde una reserva válida, finalizarlo en otra zona y rechazar un destino no permitido. | Respuestas, eventos y prueba del recorrido completo. |
| MS-07 | Ofrecer consultas cronológicas de reservas, viajes e intervenciones mediante un servicio independiente. | Historial | Detener el consumidor, finalizar un viaje y comprobar que recupera el evento al reiniciarse. | API propia, almacenamiento propio y prueba de recuperación. |
| MS-08 | Gestionar la retirada, intervención y reincorporación de vehículos. | Órdenes de mantenimiento | Detectar una batería baja, completar una recarga y comprobar que el vehículo vuelve a estar disponible. | Estados de la orden y del vehículo, eventos y prueba del recorrido. |
| MS-09 | Gestionar órdenes para trasladar vehículos entre zonas. | Órdenes de recolocación | Crear una orden a partir de un déficit detectado, completarla y verificar las nuevas ubicaciones. | Orden, indicador que la justifica y estados antes y después. |
| MS-10 | Exponer indicadores agregados para las decisiones operativas y el dashboard. | Analítica operacional | Consultar cuatro data marts y comprobar que un indicador de demanda produce una orden de recolocación. | Resultados de las consultas, orden generada y dashboard. |

Las pruebas y evidencias de esta tabla están previstas. Su ejecución se registrará en las issues de implementación y en la memoria final.

## Requisitos transversales y entregables

| ID | Requisito del enunciado | Componentes responsables | Comprobación prevista | Evidencia prevista |
|---|---|---|---|---|
| RT-01 | Ofrecer tres interfaces diferenciadas para usuarios, mantenimiento y operaciones, y consulta del estado global. | Cliente de usuarios, cliente de operaciones y dashboard | Ejecutar una operación propia de cada interfaz y comprobar que el dashboard cambia al generar datos. | Colecciones o clientes, capturas y respuestas. |
| RT-02 | Representar los nueve estados exigidos para un vehículo y documentar sus transiciones. | Inventario, Disponibilidad, Reservas, Viajes y Mantenimiento | Recorrer transiciones válidas y solicitar cambios incompatibles con el estado actual. | Matriz de transiciones y resultados de las pruebas. |
| RT-03 | Publicar eventos de dominio con identificador, instante, origen, tipo, versión de esquema y correlación. | Servicios productores y consumidores | Seguir un viaje desde la petición inicial hasta sus consumidores mediante el identificador de correlación. | Contratos de eventos y registros relacionados. |
| RT-04 | Integrar al menos tres funciones Lambda con eventos reales. | Caducidad de reservas, detección de mantenimiento e ingesta analítica | Provocar los eventos de entrada y comprobar el efecto de cada función. | Configuración de las funciones, eventos y registros de ejecución. |
| RT-05 | Utilizar un mecanismo de comunicación asíncrona. | Bus de eventos y colas de los consumidores | Publicar un evento mientras un consumidor está detenido y comprobar su recepción posterior. | Configuración de SNS y SQS y prueba de recuperación. |
| RT-06 | Mantener un Data Lake separado del almacenamiento operacional, con capas raw, trusted y curated. | Ingesta analítica y procesos de los data marts | Seguir un evento original, su versión validada y el agregado resultante. | Objetos, particiones y esquema documentado de cada capa. |
| RT-07 | Construir al menos cuatro data marts asociados a dominios. | Analítica operacional y procesos analíticos | Consultar los marts de reservas, viajes, flota y demanda con datos de resultado conocido. | Consultas, resultados esperados y resultados obtenidos. |
| RT-08 | Generar una orden de recolocación a partir de indicadores de demanda. | Analítica operacional y Órdenes de recolocación | Crear un déficit y un excedente del mismo tipo de vehículo y verificar la orden generada. | Ventana analizada, indicadores, regla aplicada y orden. |
| RT-09 | Incorporar al menos cuatro mecanismos de tolerancia a fallos y probar dos escenarios de fallo. | Servicios operacionales, consumidores y mensajería | Detener Historial y provocar un fallo de ingesta; recuperar ambos flujos. | Pruebas, registros correlacionados, reintentos y mensajes fallidos. |
| RT-10 | Justificar el escalado y analizar al menos dos cuellos de botella. | Reservas, Viajes, consumidores y capa analítica | Comparar una y dos réplicas de Reservas con la misma carga y verificar la adjudicación exclusiva. | Configuración, condiciones de prueba, latencias y conflictos observados. |
| RT-11 | Ofrecer un pipeline reproducible con pruebas, validación y construcción de artefactos. | Repositorio y GitHub Actions | Ejecutar el pipeline sobre una revisión válida y sobre una prueba fallida. | Ejecuciones de CI, resultados de pruebas e imágenes o paquetes generados. |
| RT-12 | Mostrar indicadores funcionales y técnicos actualizados. | Dashboard, Analítica operacional y registros de los servicios | Comparar los indicadores antes y después de viajes, mantenimiento y recolocaciones. | Capturas, consultas a las fuentes y fecha de actualización. |
| RT-13 | Documentar arquitectura, propiedad de datos, contratos y al menos dos decisiones arquitectónicas. | Documentación del repositorio y memoria | Revisar cada diagrama y decisión frente al código y a una ejecución identificable. | Diagramas, contratos, tabla de propiedad y dos ADR. |
| RT-14 | Permitir reproducir la ejecución local y el despliegue. | README, configuración y scripts versionados | Seguir las instrucciones desde una copia limpia del repositorio. | Comandos ejecutados y resultados de la reproducción. |
| RT-15 | Entregar la memoria y realizar una defensa técnica con participación de ambos miembros. | Equipo completo | Demostrar los recorridos funcionales y responder preguntas sobre decisiones y fallos. | Memoria PDF, guion y evidencias de la demostración. |
| RT-16 | Documentar seguridad, coste y limpieza de los recursos cloud. | Scripts de despliegue y memoria | Comparar el inventario de recursos antes y después de la limpieza en cada cuenta utilizada. | Recursos creados, eliminados y activos; consumo observado y medidas de protección de credenciales. |