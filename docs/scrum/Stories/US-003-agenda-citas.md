# US-003 — Agenda de citas
- Épica: Citas
- Prioridad: P0
- Puntos: 13
- Estado: To Do

## Historia
Como cliente quiero ver disponibilidad y reservar un servicio para agendar mi cita sin llamar.

## Criterios de aceptación
- [ ] Selección de servicio, empleado y horario disponible
- [ ] Prevención de solapamientos de empleado/recurso
- [ ] Confirmación de reserva con resumen
- [ ] Estado inicial: pendiente/confirmada según política
- [ ] Reprogramación/cancelación con reglas de tiempo

## Reglas de negocio
- La disponibilidad respeta horarios y bloqueos del negocio
- Antelación mínima configurable para reservar o cancelar

## Tareas
- [ ] UX: selector de fecha/empleado con estados
- [ ] Reglas: motor mínimo de disponibilidad
- [ ] Implementación: creación/actualización de cita
- [ ] QA: pruebas de solapamiento y bordes
- [ ] Docs: flujo de reserva
