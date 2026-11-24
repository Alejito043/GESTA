# Visión y problema

## Visión

Una plataforma web donde cualquier negocio que trabaje con citas o turnos pueda publicar su agenda, recibir reservas de sus clientes y administrar su operación, con una identidad propia y reglas adaptables a su forma de trabajar.

## Problema

Muchos negocios pequeños en Colombia gestionan sus citas por WhatsApp, llamadas o cuadernos. Esto genera dobles agendamientos, olvidos, clientes que no llegan sin avisar, horas perdidas respondiendo mensajes y ninguna visibilidad sobre cuántos clientes se atienden o en qué horarios hay más demanda. Las herramientas existentes suelen estar pensadas para otros mercados, son costosas o no se adaptan a negocios muy distintos entre sí.

## Usuarios

- **Dueño o administrador:** configura el negocio y supervisa la operación.
- **Empleado:** consulta y gestiona su propia agenda.
- **Cliente final:** reserva, reprograma o cancela.

## Personalización por capas

1. Identidad del negocio: nombre, logo, colores y enlace propio.
2. Catálogo propio: servicios, duraciones, precios, empleados y horarios.
3. Reglas propias: políticas de cancelación, anticipación mínima, cupos.
4. Plantillas por tipo de negocio.
5. Campos personalizados al reservar.

## Hoja de ruta

**MVP:** registro de negocios con identidad propia, catálogo de servicios y empleados, horarios, reserva y cancelación, agenda para empleados y dueño, prevención de reservas simultáneas.

**Versión 1:** recordatorios automáticos por correo, reprogramación, políticas de cancelación configurables, plantillas por tipo de negocio, reportes básicos de ocupación.

**Versión 2:** pagos o anticipos en línea, notificaciones por WhatsApp, campos personalizados por negocio, analítica avanzada.

**Versión futura (a evaluar):** aplicación móvil, integraciones con calendarios externos, despliegue en la nube con múltiples regiones.

**Principio de diseño:** cada funcionalidad de versiones posteriores debe tener un punto de extensión previsto en la arquitectura del MVP, sin implementarse todavía.

## Criterios de éxito del MVP

- Un negocio puede configurarse y recibir su primera reserva sin ayuda técnica.
- Dos clientes no pueden reservar el mismo horario aunque lo intenten a la vez.
- Los datos de un negocio nunca son visibles para otro.
