# Historias de usuario

Formato: "Como [actor], quiero [acción], para [beneficio]". Cada criterio de aceptación sigue el esquema Dado / Cuando / Entonces y se convierte después en una prueba automatizada.

## Épica A: Cuenta y negocio

### HU-01: Registrar cuenta y negocio
Cubre RF-01, RF-11.
Como dueño, quiero crear mi cuenta y mi negocio, para empezar a recibir reservas.

- Dado un nombre de negocio, un correo y una contraseña válidos, cuando me registro, entonces se crean mi usuario y mi negocio, y quedo con los roles de administrador y empleado.
- Dado un nombre de negocio, entonces se genera un enlace propio en minúsculas, sin tildes ni espacios (por ejemplo /barberia-el-corte).
- Dado que el enlace ya existe, entonces se rechaza y se sugiere una alternativa.
- Dado un enlace reservado por el sistema (admin, login, api), entonces se rechaza.
- Dado un correo ya registrado, entonces se rechaza sin revelar datos de la otra cuenta.
- Dado una contraseña de menos de 10 caracteres o muy común, entonces se rechaza indicando el motivo.
- Dado que me registro, entonces el negocio queda con zona horaria America/Bogota y una paleta neutra por defecto.
- Dado que me registro, entonces mi perfil de empleado se crea automáticamente.

### HU-02: Iniciar y cerrar sesión
Cubre RF-02, RNF-05.
Como usuario del negocio, quiero entrar y salir de forma segura, para proteger los datos.

- Dado credenciales correctas, cuando inicio sesión, entonces accedo a las pantallas que corresponden a mis roles.
- Dado credenciales incorrectas, entonces el mensaje es genérico y no revela si falló el usuario o la contraseña.
- Dado 5 intentos fallidos seguidos para una misma cuenta, entonces se bloquea el acceso por 15 minutos.
- Dado que cierro sesión, entonces la sesión deja de ser válida de inmediato, también si alguien intenta reutilizarla.
- Dado una sesión con más de 12 horas de inactividad, entonces se cierra y se pide ingresar de nuevo.
- Dado que se guardan contraseñas, entonces se almacenan con hash seguro y nunca aparecen en logs ni en respuestas.
- Dado que el dueño entra con correo y el empleado con nombre de usuario, entonces ambos flujos usan la misma pantalla de ingreso.

### HU-03: Subir el logo con validación
Cubre RF-03, RNF-07, RNF-01.
Como dueño, quiero subir mi logo, para que mis clientes reconozcan mi negocio.

- Dado un PNG, JPG o WebP de hasta 5 MB, cuando lo subo, entonces se guarda y se muestra en una vista previa.
- Dado un archivo con extensión .png cuyo contenido real es otro tipo, entonces se rechaza, porque se valida el contenido y no el nombre.
- Dado un SVG, un archivo de más de 5 MB o un formato no permitido, entonces se rechaza indicando el motivo.
- Dado un logo aceptado, entonces el sistema genera una versión reducida (máximo 512 píxeles de ancho, en WebP) para la página pública y conserva el original aparte.
- Dado que el negocio no tiene logo, entonces la página pública muestra el nombre del negocio como texto.
- Dado que subo un logo nuevo, entonces reemplaza al anterior y la paleta se vuelve a proponer.
- Dado un nombre de archivo malicioso (rutas, caracteres especiales), entonces el sistema lo ignora y guarda con un nombre generado.

### HU-04: Proponer una paleta desde el logo
Cubre RF-04, RNF-16.
Como dueño, quiero que la página adopte los colores de mi logo, para que se vea como mi marca sin diseñar nada.

- Dado un logo recién subido, entonces el sistema extrae su color dominante y propone una paleta (color principal, color de texto sobre el principal y un fondo suave).
- Dado un color dominante muy claro, entonces el sistema lo oscurece lo necesario para que el texto sobre botones tenga un contraste de al menos 4.5:1.
- Dado un logo en tonos grises o casi monocromático, entonces se usa una paleta neutra con un acento de contraste legible.
- Dado un logo con fondo transparente o blanco, entonces el fondo no cuenta como color dominante.
- Dado la paleta propuesta, entonces veo una vista previa de la página pública antes de aceptarla.
- Dado cualquier paleta aplicada, entonces ningún par texto/fondo de la página baja de 4.5:1.

### HU-05: Cambiar el color principal
Cubre RF-05.
Como dueño, quiero corregir el color si no me convence la propuesta, para tener la última palabra sobre mi imagen.

- Dado que elijo otro color principal, cuando lo guardo, entonces se aplica a la página pública.
- Dado un color que no cumple el contraste mínimo, entonces el sistema ajusta el texto o el tono y me muestra el resultado antes de guardar.
- Dado que quiero volver a la propuesta automática, entonces puedo restaurarla.
- Dado que guardo cambios, entonces solo cambia mi negocio.

## Épica B: Catálogo y horarios

### HU-06: Gestionar servicios
Cubre RF-06.
Como dueño, quiero definir mis servicios, para que los clientes vean opciones reales.

- Dado un nombre, una duración (múltiplo de 15 minutos, entre 15 y 480) y un precio mayor o igual a cero, cuando lo guardo, entonces aparece en la página pública.
- Dado una duración que no es múltiplo de 15 o un precio negativo, entonces se rechaza con un mensaje claro.
- Dado un precio igual a cero, entonces se muestra como "Consultar" o "Gratis", según lo que elija el dueño.
- Dado que desactivo un servicio con reservas futuras, entonces deja de aceptar nuevas reservas, las existentes se mantienen y se me avisa cuántas hay.
- Dado que cambio la duración o el precio de un servicio, entonces las reservas existentes conservan los valores con los que se crearon.
- Dado que reactivo un servicio, entonces vuelve a aparecer con su configuración.

### HU-07: Gestionar empleados
Cubre RF-07.
Como dueño, quiero registrar a las personas que atienden, para que los clientes puedan elegirlas.

- Dado un nombre, cuando creo un empleado, entonces queda disponible para asignarle servicios y horario.
- Dado que desactivo un empleado con reservas futuras, entonces deja de aceptar nuevas reservas, las existentes se mantienen y se me avisa cuántas hay.
- Dado un empleado desactivado, entonces no aparece en la página pública ni en la asignación automática.
- Dado que desactivo a un empleado con acceso al sistema, entonces ya no puede iniciar sesión.

### HU-08: Asignar servicios a empleados
Cubre RF-08.
Como dueño, quiero indicar qué servicios presta cada empleado, para que solo se ofrezcan combinaciones reales.

- Dado un empleado y un servicio, cuando los asocio, entonces ese empleado aparece como opción para ese servicio.
- Dado un empleado sin servicios asignados, entonces no aparece como opción para reservar.
- Dado un servicio sin empleados asignados, entonces no se ofrece en la página pública y se me avisa en el panel.
- Dado que quito un servicio a un empleado con reservas futuras de ese servicio, entonces se me advierte y las reservas se mantienen.
- Dado un servicio de otro negocio, entonces no puedo asignarlo.

### HU-09: Definir el horario semanal
Cubre RF-09, RF-23.
Como dueño, quiero indicar cuándo trabaja cada empleado, para no recibir reservas fuera de horario.

- Dado un horario por día (por ejemplo, lunes de 9:00 a 18:00), cuando lo guardo, entonces solo se ofrecen horarios dentro de esos rangos.
- Dado varios rangos en un mismo día (9:00 a 13:00 y 14:00 a 18:00), entonces se respeta el descanso.
- Dado un rango cuya hora de fin es anterior o igual a la de inicio, entonces se rechaza.
- Dado dos rangos que se cruzan en el mismo día, entonces se rechaza.
- Dado que cambio el horario semanal, entonces las reservas existentes se conservan y el cambio solo afecta a las nuevas.
- Dado que alguna reserva existente queda fuera del nuevo horario, entonces se me muestra la lista para que decida.
- Dado un día sin rango, entonces ese empleado no atiende ese día.
- Dado que las horas se definen en la zona horaria del negocio, entonces se guardan sin ambigüedad.

### HU-10: Bloquear fechas y horas
Cubre RF-10, RF-23.
Como dueño, quiero bloquear festivos, vacaciones o permisos, para no recibir reservas cuando no hay atención.

- Dado un bloqueo con fecha y hora de inicio y fin, para un empleado o para todo el negocio, cuando lo creo, entonces ningún horario que lo cruce se ofrece a nuevos clientes.
- Dado un bloqueo de día completo, entonces basta con indicar la fecha.
- Dado que bloqueo un rango donde ya hay reservas, entonces el sistema no las cancela, me muestra la lista de afectadas y yo decido cuáles cancelar.
- Dado que elimino un bloqueo, entonces los horarios vuelven a ofrecerse.
- Dado un bloqueo con fin anterior al inicio, entonces se rechaza.
- Dado un motivo opcional, entonces es solo interno y nunca se muestra al público.

## Épica C: Roles y acceso

### HU-11: Roles múltiples y permisos
Cubre RF-11, RF-12.
Como dueño de un negocio de una sola persona, quiero ser administrador y empleado con una sola cuenta, para no duplicar usuarios.

- Dado un negocio nuevo, entonces el creador queda con los roles de administrador y empleado.
- Dado un usuario con ambos roles, entonces ve el menú de administración y su agenda personal sin cambiar de cuenta.
- Dado un usuario solo con rol de empleado, entonces no ve el catálogo, los horarios ajenos, la configuración ni las fichas de clientes.
- Dado que se le quita un rol a un usuario, entonces pierde esos permisos de inmediato, incluso con la sesión abierta.
- Dado que se intenta dejar un negocio sin administradores, entonces se rechaza.
- Dado cualquier acción protegida, entonces el permiso se verifica en el servidor, no solo ocultando botones en la pantalla.

### HU-12: Aislamiento entre negocios
Cubre RF-24, RNF-06.
Como dueño, quiero tener la certeza de que nadie de otro negocio puede ver mis datos, para proteger a mis clientes.

- Dado un usuario del negocio A, cuando intenta acceder a datos del negocio B cambiando identificadores en la URL o en una solicitud, entonces recibe "no encontrado" y no se revela que el recurso existe.
- Dado cualquier consulta del sistema, entonces está limitada al negocio del usuario autenticado.
- Dado una prueba automatizada que intenta leer, modificar y borrar recursos de otro negocio en cada punto de acceso, entonces todas fallan.
- Dado la página pública, entonces solo expone datos públicos del negocio indicado en el enlace.
- Dado un identificador de reserva o de cliente, entonces no es predecible ni secuencial.

### HU-13: Dar acceso a un empleado
Cubre RF-27, RF-28.
Como dueño, quiero darle acceso al sistema a mi empleado, para que gestione su propia agenda.

- Dado un empleado registrado, cuando le creo acceso con un nombre de usuario, entonces el sistema genera una contraseña temporal que veo una sola vez.
- Dado un nombre de usuario ya usado en el sistema, entonces se rechaza.
- Dado que el empleado inicia sesión con la contraseña temporal, entonces debe cambiarla antes de hacer cualquier otra cosa.
- Dado una contraseña temporal con más de 72 horas, entonces no sirve y el dueño debe generar otra.
- Dado que el empleado olvida su contraseña, entonces el dueño puede generarle una nueva temporal.
- Dado que el dueño genera una nueva temporal, entonces la anterior queda anulada y las sesiones abiertas del empleado se cierran.
- Dado un empleado con acceso, entonces solo tiene el rol de empleado, salvo que el dueño le asigne otro.

### HU-28: Recuperar la contraseña del dueño
Cubre RF-29, RNF-05.
Como dueño, quiero recuperar mi contraseña si la olvido, para no perder el acceso a mi negocio.

- Dado un correo registrado, cuando solicito la recuperación, entonces recibo un enlace de un solo uso válido por 30 minutos.
- Dado un correo no registrado, entonces el mensaje en pantalla es idéntico, para no revelar qué correos existen.
- Dado el enlace válido, cuando defino una contraseña nueva que cumple la política, entonces queda cambiada y el enlace deja de servir.
- Dado un enlace vencido o ya usado, entonces se rechaza y se ofrece solicitar otro.
- Dado que cambio la contraseña, entonces se cierran las demás sesiones abiertas de mi cuenta.
- Dado más de 3 solicitudes por hora para el mismo correo o la misma IP, entonces se bloquean temporalmente.

## Épica D: Reserva del cliente

### HU-14: Ver la página pública del negocio
Cubre RF-13, RNF-14, RNF-01.
Como cliente, quiero abrir el enlace del negocio y ver sus servicios, para saber qué ofrece antes de reservar.

- Dado un enlace válido, cuando lo abro, entonces veo el logo, el nombre y los servicios activos con duración y precio, con la paleta del negocio.
- Dado un enlace que no existe, entonces veo una página de "negocio no encontrado".
- Dado un negocio sin servicios activos, entonces veo un mensaje indicando que por ahora no recibe reservas.
- Dado una pantalla de 360 píxeles de ancho, entonces el contenido se lee sin desplazamiento horizontal.
- Dado que no estoy autenticado, entonces la página no muestra ningún dato interno del negocio (empleados, clientes, agenda).

### HU-15: Ver horarios disponibles
Cubre RF-14, RF-23, RNF-02.
Como cliente, quiero ver los horarios libres de un servicio, para elegir uno sin escribirle al negocio.

- Dado un servicio de 30 minutos y un empleado con horario de 9:00 a 12:00 sin reservas, cuando consulto ese día, entonces los horarios inician cada 15 minutos, desde las 9:00 hasta las 11:30 como último inicio.
- Dado una reserva de 10:00 a 10:30, entonces no se ofrecen 9:45, 10:00 ni 10:15 (se cruzan), pero sí 9:30 y 10:30.
- Dado un bloqueo de 11:00 a 12:00, entonces no se ofrece ningún horario que se cruce con él.
- Dado que no elijo empleado, entonces veo los horarios en que al menos un empleado que presta ese servicio está libre.
- Dado que elijo un empleado, entonces solo veo los horarios de esa persona.
- Dado que consulto hoy, entonces no se ofrecen horas pasadas ni con menos de 30 minutos de anticipación.
- Dado una fecha más allá del horizonte (30 días por defecto), entonces no se ofrecen horarios.
- Dado un día sin horarios libres, entonces se indica claramente y se sugiere el próximo día con disponibilidad.

### HU-16: Reservar un horario
Cubre RF-15, RF-22, RNF-03, RNF-15.
Como cliente, quiero reservar con mis datos básicos, para asegurar mi cita sin crear una cuenta.

- Dado un horario libre, cuando ingreso nombre completo y un celular válido y acepto la política de datos, entonces la reserva queda confirmada.
- Dado que dejo vacío el nombre o el celular, o no acepto la política, entonces no se crea y se indica qué falta.
- Dado un celular que no tiene 10 dígitos o no empieza por 3, entonces se rechaza con un mensaje claro.
- Dado que dejo el correo vacío, entonces la reserva se crea igual. Dado un correo con formato inválido, entonces se rechaza.
- Dado que dos clientes confirman casi a la vez el mismo horario, entonces solo una reserva se confirma y la otra persona ve un aviso y los horarios actualizados.
- Dado que no elegí empleado, cuando confirmo, entonces se asigna al primer empleado disponible según un orden fijo.
- Dado que reservo, entonces se registra un evento "reserva creada" (HU-24) y mi aceptación de la política (HU-27).
- Dado el proceso completo, entonces se completa en un máximo de 5 pasos desde la página del negocio.

### HU-17: Límites anti-abuso
Cubre RNF-08.
Como dueño, quiero que nadie pueda llenar mi agenda con reservas falsas, para no perder clientes reales.

- Dado un celular con 3 reservas creadas hoy en el negocio, cuando intenta una cuarta, entonces se rechaza explicando el límite.
- Dado un celular con 5 reservas activas en el negocio, cuando intenta otra, entonces se rechaza explicando el límite.
- Dado una IP con más de 10 intentos de reserva en una hora, entonces se bloquea temporalmente con un mensaje claro.
- Dado que se alcanza un límite, entonces se muestra el teléfono del negocio para que el cliente pueda comunicarse si es una situación legítima.
- Dado que se cancela una reserva activa, entonces el cupo de reservas activas del celular se libera, pero el contador diario no.

### HU-18: Recibir el código de reserva
Cubre RF-16.
Como cliente, quiero ver mi código de reserva al terminar, para poder cancelar después.

- Dado una reserva confirmada, entonces veo en pantalla el código, el servicio, el empleado, la fecha, la hora y el nombre del negocio.
- Dado esa pantalla, entonces hay un botón para copiar el código y una recomendación de tomar captura.
- Dado que di un correo, entonces recibo ese mismo resumen por correo.
- Dado que no di correo, entonces el sistema no intenta enviar nada y no falla.
- Dado el código, entonces tiene 6 caracteres, sin caracteres confundibles (0/O, 1/I), y no hay dos iguales dentro del mismo negocio.

### HU-19: Cancelar mi reserva
Cubre RF-17.
Como cliente, quiero cancelar mi reserva si no puedo asistir, para liberar el horario.

- Dado una reserva futura, cuando ingreso el código y el celular correctos, entonces se cancela, el horario vuelve a estar libre y se registra el evento.
- Dado un código o celular incorrectos, entonces el mensaje es genérico y no revela cuál falló.
- Dado una reserva ya cancelada o con hora pasada, entonces no se puede cancelar y se informa el motivo.
- Dado 5 intentos fallidos en 15 minutos desde la misma IP, entonces se bloquean los intentos durante 15 minutos.
- Dado que cancelo, entonces veo una confirmación clara y la opción de reservar otro horario.

## Épica E: Agenda

### HU-20: Agenda del empleado
Cubre RF-18.
Como empleado, quiero ver mi agenda del día y de la semana, para saber a quién atiendo y cuándo.

- Dado un empleado con reservas, cuando abre su agenda, entonces ve solo las suyas, ordenadas por hora, con servicio, nombre y celular del cliente.
- Dado que cambio entre vista diaria y semanal, entonces la vista conserva el día seleccionado.
- Dado un día sin reservas, entonces se muestra un mensaje claro y no una pantalla vacía.
- Dado una reserva cancelada, entonces no aparece en la agenda activa, pero se puede ver con un filtro.
- Dado que otro empleado tiene reservas, entonces no las veo, ni cambiando parámetros de la URL.
- Dado que las fechas se guardan en UTC, entonces la agenda muestra todo en la zona horaria del negocio.

### HU-21: Agenda general del dueño
Cubre RF-19.
Como dueño, quiero ver la agenda de todos mis empleados, para tener control de la operación.

- Dado el rol de administrador, cuando abro la agenda general, entonces veo las reservas de todos los empleados, diferenciadas por persona.
- Dado el filtro por empleado, entonces la agenda muestra solo las reservas de esa persona.
- Dado un administrador que también es empleado, entonces ve una sola agenda sin duplicados.
- Dado que busco por nombre o celular del cliente, entonces el sistema lista sus reservas dentro de mi negocio.

### HU-22: Cancelar una reserva como dueño
Cubre RF-20.
Como dueño, quiero cancelar una reserva, para manejar imprevistos.

- Dado una reserva futura, cuando la cancelo eligiendo un motivo de una lista fija (empleado no disponible, negocio cerrado, solicitud del cliente por teléfono, otro), entonces el horario queda libre y se registra el evento con mi usuario como autor.
- Dado que cancelo, entonces el sistema me muestra el celular del cliente con un enlace de contacto (wa.me), porque no hay notificación automática en el MVP.
- Dado una reserva ya cancelada o con hora pasada, entonces no se puede cancelar.
- Dado un empleado sin rol de administrador, cuando intenta cancelar una reserva ajena, entonces se rechaza.
- Dado que cancelo, entonces el cupo de reservas activas del cliente se libera (HU-17).

### HU-23: Marcar atendida o inasistencia
Cubre RF-21.
Como empleado o dueño, quiero registrar si el cliente asistió, para llevar un historial confiable.

- Dado una reserva cuya hora de inicio ya pasó, cuando la marco como atendida o como inasistencia, entonces se registra el evento correspondiente.
- Dado una reserva que aún no empieza, entonces no puedo marcarla.
- Dado un empleado, entonces puede marcar las reservas propias. Dado un administrador, entonces puede marcar cualquiera.
- Dado una reserva ya marcada, cuando necesita corrección, entonces solo un administrador puede corregirla, y se agrega un evento de corrección con su usuario y la fecha. El evento original permanece.
- Dado una reserva cancelada, entonces no se puede marcar.

## Épica F: Historial y privacidad

### HU-24: Registrar eventos inmutables
Cubre RF-25, RNF-11, RNF-13.
Como dueño, quiero que cada cambio en una reserva quede registrado, para tener un historial exacto que nadie pueda alterar.

- Dado cualquiera de estas acciones (reserva creada, cancelada por cliente, cancelada por dueño, atendida, inasistencia, corrección, cliente anonimizado), entonces se guarda un evento con tipo, fecha y hora, autor e identificador interno del cliente y de la reserva.
- Dado un evento, entonces no contiene nombre, celular, correo ni texto libre.
- Dado el historial, entonces la aplicación solo puede insertar eventos. Una prueba automatizada verifica que modificar o borrar falla.
- Dado que una acción falla a medias, entonces no queda un evento sin su cambio correspondiente, ni al revés (el cambio y el evento se guardan juntos o no se guardan).
- Dado el autor de un evento, entonces puede ser un usuario del negocio o el propio cliente, identificado como "cliente".

### HU-25: Ver el historial de un cliente
Cubre RF-26.
Como dueño, quiero ver cuántas veces ha venido un cliente, para conocer a mis clientes frecuentes.

- Dado un cliente con varias visitas, cuando abro su ficha, entonces veo el total de reservas, atenciones, cancelaciones e inasistencias, con fechas.
- Dado un cliente que reservó en dos negocios, entonces cada negocio ve solo su propia ficha e historial.
- Dado un empleado sin rol de administrador, entonces no accede a las fichas de clientes. Solo ve a quién atiende en su agenda.
- Dado un cliente sin visitas atendidas, entonces la ficha muestra contadores en cero.

### HU-26: Anonimizar a un cliente
Cubre RNF-10.
Como dueño, quiero poder eliminar los datos personales de un cliente que lo pida, para cumplir la ley sin perder mis estadísticas.

- Dado un cliente sin reservas activas, cuando uso la opción de anonimizar y confirmo, entonces nombre, celular y correo se reemplazan por "Cliente eliminado".
- Dado un cliente con reservas activas, entonces el sistema me pide cancelarlas primero y no anonimiza.
- Dado un cliente anonimizado, entonces los conteos del negocio no cambian, pero la persona ya no es identificable.
- Dado que anonimizo, entonces se registra un evento "cliente anonimizado" sin datos personales.
- Dado una anonimización, entonces no se puede deshacer, y el sistema lo advierte antes de confirmar.
- Dado un cliente anonimizado que vuelve a reservar con el mismo celular, entonces se crea una ficha nueva, sin vínculo con la anterior.
- Dado un usuario sin rol de administrador, entonces no ve la opción.

### HU-27: Registrar el consentimiento de datos
Cubre RNF-09.
Como cliente, quiero saber para qué se usan mis datos y aceptarlo, para reservar con confianza.

- Dado el formulario de reserva, entonces muestra un enlace a la política de tratamiento de datos y una casilla de aceptación sin marcar por defecto.
- Dado que no marco la casilla, entonces no se puede reservar.
- Dado una reserva confirmada, entonces se guarda la fecha y hora de aceptación y la versión de la política vigente.
- Dado que la política cambia, entonces las aceptaciones anteriores conservan la versión que aceptó cada cliente.
- Dado la política, entonces indica qué datos se recogen, para qué, quién es el responsable (el negocio) y cómo solicitar la supresión.

## Matriz de trazabilidad

### Requisitos funcionales

| Requisito | Historias |
|---|---|
| RF-01 | HU-01 |
| RF-02 | HU-02 |
| RF-03 | HU-03 |
| RF-04 | HU-04 |
| RF-05 | HU-05 |
| RF-06 | HU-06 |
| RF-07 | HU-07 |
| RF-08 | HU-08 |
| RF-09 | HU-09 |
| RF-10 | HU-10 |
| RF-11 | HU-01, HU-11 |
| RF-12 | HU-11 |
| RF-13 | HU-14 |
| RF-14 | HU-15 |
| RF-15 | HU-16, HU-27 |
| RF-16 | HU-18 |
| RF-17 | HU-19 |
| RF-18 | HU-20 |
| RF-19 | HU-21 |
| RF-20 | HU-22 |
| RF-21 | HU-23 |
| RF-22 | HU-16 |
| RF-23 | HU-09, HU-10, HU-15 |
| RF-24 | HU-12 |
| RF-25 | HU-24 |
| RF-26 | HU-25 |
| RF-27 | HU-13 |
| RF-28 | HU-13 |
| RF-29 | HU-28 |

### Requisitos no funcionales

| Requisito | Historias o tratamiento |
|---|---|
| RNF-01 | HU-03, HU-14 |
| RNF-02 | HU-15 |
| RNF-03 | HU-16 |
| RNF-04 | Prueba de carga (estrategia de pruebas) |
| RNF-05 | HU-01, HU-02, HU-13, HU-28 |
| RNF-06 | HU-12 |
| RNF-07 | HU-03 |
| RNF-08 | HU-17 |
| RNF-09 | HU-27 |
| RNF-10 | HU-26 |
| RNF-11 | HU-24 |
| RNF-12 | Infraestructura (copias de seguridad y restauración) |
| RNF-13 | HU-24 |
| RNF-14 | HU-14 |
| RNF-15 | HU-16 |
| RNF-16 | HU-04, HU-05 |
| RNF-17 a RNF-19 | Transversales: se cubren en la arquitectura y el pipeline, no en historias |
| RNF-20 | HU-09, HU-20 (y diseño del modelo de datos) |
