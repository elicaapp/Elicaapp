# US-004 — Confirmación y recordatorios
- Épica: Citas
- Prioridad: P0
- Puntos: 5
- Estado: To Do

## Historia
Como cliente quiero recibir confirmación y recordatorios de mi cita para no olvidarla.

## Criterios de aceptación
- [ ] Confirmación inmediata tras reservar
- [ ] Recordatorio previo configurable (ej. 24h)
- [ ] Mensajes claros con fecha/hora/servicio
- [ ] Registro del estado del envío
- [ ] Opt-out por parte del cliente

## Reglas de negocio
- Los recordatorios siguen el huso horario del negocio
- Evitar envíos duplicados por cita

## Tareas
- [ ] UX: preferencias de notificación
- [ ] Reglas: programación y ventanas de envío
- [ ] Implementación: disparadores de confirmación/recordatorio
- [ ] QA: verificación de tiempos
- [ ] Docs: política de recordatorios
