# Requisitos

## Decisiones tomadas

### Clientes y datos
- El cliente final reserva sin crear cuenta.
- El único dato de contacto obligatorio es el celular (10 dígitos, empieza por 3). El correo es opcional.
- No se pide cédula al cliente.
- La ficha del cliente es por negocio y se identifica por celular y nombre normalizado (sin tildes, en minúsculas y sin espacios repetidos). El mismo celular con nombres distintos son fichas distintas, y el mismo celular en dos negocios también.
- Los límites anti-abuso se cuentan por celular, no por ficha.
- El historial de eventos es inmutable. Los datos personales de la ficha se pueden anonimizar a solicitud del titular, y el historial se conserva sin datos identificables.
- Los eventos del historial no contienen texto libre. Los motivos de cancelación del dueño son una lista fija: empleado no disponible, negocio cerrado, solicitud del cliente por teléfono y otro.
- Los cambios de estado de una reserva atendida o marcada solo los corrige un administrador, agregando un evento de corrección.
- No se puede anonimizar a un cliente con reservas activas.

### Reservas
- Los horarios se ofrecen en bloques de 15 minutos. Las duraciones de los servicios son múltiplos de 15.
- Sin preferencia de empleado, la reserva se asigna al primer empleado disponible según un orden fijo.
- El horario no se retiene mientras el cliente llena el formulario. El conflicto se resuelve al confirmar.
- El cliente puede cancelar en cualquier momento antes de la cita, con el código y el celular.
- Las reservas del mismo día requieren 30 minutos de anticipación.
- El código de reserva tiene 6 caracteres, sin caracteres confundibles (0/O, 1/I).
- Límites anti-abuso: 3 reservas por día y 5 activas por celular y negocio, horizonte máximo de 30 días, 10 intentos de reserva por hora por IP.

### Cuentas, roles y acceso
- Un usuario puede tener varios roles dentro de un negocio (por ejemplo, administrador y empleado a la vez).
- El dueño inicia sesión con correo. Los empleados inician sesión con nombre de usuario.
- El dueño da acceso a un empleado creándole un usuario con contraseña temporal. La contraseña temporal vence a las 72 horas y debe cambiarse en el primer ingreso.
- La sesión expira tras 12 horas de inactividad.
- Las contraseñas tienen mínimo 10 caracteres, sin reglas de composición, y se rechazan las muy comunes.

### Logo y diseño
- El logo admite PNG, JPG o WebP con un máximo de 5 MB. No se aceptan SVG.
- Se genera una versión reducida del logo para la página pública y se conserva el original.
- La paleta se propone automáticamente desde el logo, con ajuste para cumplir el contraste mínimo, y el dueño puede cambiar el color principal.

### Bloqueos y desactivaciones
- Desactivar un servicio o empleado con reservas futuras impide nuevas reservas y conserva las existentes.
- Los bloqueos y cambios de horario no cancelan reservas existentes. El sistema muestra la lista de afectadas y el dueño decide.
- Solo se puede marcar una reserva como atendida o inasistencia después de su hora de inicio.

## Requisitos funcionales del MVP

### Negocio y cuenta
- RF-01: El dueño puede registrarse y crear un negocio con nombre y enlace propio único.
- RF-02: El dueño y los empleados pueden iniciar y cerrar sesión.
- RF-03: El dueño puede subir el logo de su negocio (PNG, JPG o WebP, máximo 5 MB).
- RF-04: El sistema propone una paleta de colores a partir del logo y la aplica a la página pública del negocio, ajustándola para garantizar legibilidad.
- RF-05: El dueño puede cambiar manualmente el color principal propuesto.

### Catálogo y horarios
- RF-06: El dueño puede crear, editar y desactivar servicios con nombre, duración y precio.
- RF-07: El dueño puede crear, editar y desactivar empleados.
- RF-08: El dueño puede asignar a cada empleado los servicios que presta.
- RF-09: El dueño puede definir el horario semanal de atención de cada empleado.
- RF-10: El dueño puede bloquear fechas u horas específicas (festivos, vacaciones, permisos).

### Roles, permisos y acceso
- RF-11: Un usuario puede tener varios roles dentro de un mismo negocio.
- RF-12: Los permisos se determinan por los roles asignados, no por un tipo fijo de cuenta.
- RF-27: El dueño puede dar acceso a un empleado creándole un usuario con contraseña temporal.
- RF-28: El empleado debe cambiar la contraseña temporal en su primer ingreso.
- RF-29: El dueño puede recuperar su contraseña mediante un enlace enviado a su correo.

### Reserva (cliente final)
- RF-13: El cliente puede ver la página pública del negocio con sus servicios.
- RF-14: El cliente puede elegir un servicio y ver los horarios disponibles, con o sin preferencia de empleado.
- RF-15: El cliente puede reservar un horario dejando nombre completo y celular (correo opcional) y aceptando la política de tratamiento de datos.
- RF-16: El cliente recibe un código de reserva en pantalla, y por correo si lo proporcionó.
- RF-17: El cliente puede cancelar su reserva con el código y el celular con que reservó.

### Agenda
- RF-18: El empleado puede ver su propia agenda por día y por semana.
- RF-19: El dueño puede ver la agenda de todos los empleados.
- RF-20: El dueño puede cancelar una reserva.
- RF-21: El dueño y los empleados pueden marcar una reserva como atendida o como inasistencia.

### Reglas del sistema
- RF-22: El sistema no permite dos reservas sobre el mismo horario del mismo empleado.
- RF-23: El sistema solo ofrece horarios dentro del horario del empleado y que no estén bloqueados.
- RF-24: Un negocio solo puede ver y modificar sus propios datos.

### Historial
- RF-25: Cada reserva, cancelación, atención, inasistencia, corrección y anonimización queda registrada como un evento inmutable con fecha y autor.
- RF-26: El dueño puede ver el historial de visitas de cada cliente de su negocio.

## Requisitos no funcionales

### Rendimiento
- RNF-01: La página pública de un negocio carga en menos de 2 segundos en una conexión móvil 4G.
- RNF-02: La consulta de horarios disponibles responde en menos de 1 segundo para el 95 % de las solicitudes.

### Concurrencia e integridad
- RNF-03: Dos intentos simultáneos de reservar el mismo horario nunca resultan en dos reservas confirmadas.
- RNF-04: El sistema soporta 50 usuarios concurrentes sin degradar los tiempos de RNF-02.

### Seguridad
- RNF-05: Todo el tráfico va cifrado (HTTPS). Las contraseñas se guardan con un algoritmo de hash seguro (Argon2 o bcrypt), tienen mínimo 10 caracteres y se rechazan las contraseñas muy comunes.
- RNF-06: Ningún negocio puede acceder a datos de otro, ni manipulando URLs o solicitudes. Se verifica con pruebas automatizadas.
- RNF-07: La subida de logos valida el tipo real del archivo y el tamaño máximo.
- RNF-08: Límites anti-abuso: 3 reservas por día y 5 activas por celular y negocio, horizonte máximo de reserva configurable (30 días por defecto) y límite de intentos por IP.

### Privacidad (Ley 1581 de 2012)
- RNF-09: Se recolectan solo los datos estrictamente necesarios, con autorización expresa registrada (fecha y versión de la política aceptada).
- RNF-10: A solicitud del titular, el dueño puede anonimizar los datos personales de un cliente. El historial de eventos se conserva sin datos identificables.
- RNF-11: Los eventos del historial no contienen texto libre ni datos personales: solo tipo, fecha, autor e identificador interno.

### Disponibilidad y datos
- RNF-12: Meta de disponibilidad del 99 % mensual, con copias de seguridad diarias de la base de datos y una restauración probada.
- RNF-13: El historial de eventos solo admite inserciones: la aplicación no tiene permisos de modificar ni borrar sobre esa tabla.

### Usabilidad
- RNF-14: Diseño pensado primero para celular.
- RNF-15: Reservar toma como máximo 5 pasos desde la página del negocio.
- RNF-16: Contraste de colores conforme a WCAG AA (4.5:1 para texto normal).

### Mantenibilidad y operación
- RNF-17: Todo el sistema arranca en un entorno local con un solo comando (contenedores).
- RNF-18: La configuración sensible vive en variables de entorno, nunca en el código.
- RNF-19: Los eventos importantes se registran en logs estructurados.
- RNF-20: Las fechas se guardan en UTC y se muestran en la zona horaria del negocio (America/Bogota por defecto).

## Supuestos de escala

Diseño pensado para unos 100 negocios y 5.000 reservas al mes. Es una referencia para decidir con criterio y evitar sobreingeniería, no una meta de construcción.

## Previsto para versiones posteriores

- Verificación del celular con código y notificaciones por WhatsApp.
- Beneficios de fidelidad, con autorización opcional y separada del cliente.
- Política automática de retención de datos.
- Crear reservas desde el panel en nombre de un cliente (reserva telefónica).
- Anticipación mínima, horizonte de reserva y política de cancelación configurables por negocio.
- Retención temporal del horario mientras el cliente completa la reserva.
- Reparto equitativo de la carga entre empleados en la asignación automática.
