# US-002 — Gestión de servicios
- Épica: Servicios
- Prioridad: P0
- Puntos: 5
- Estado: To Do

## Historia
Como dueño de negocio quiero crear, editar y eliminar servicios con precio y duración para que mis clientes puedan reservarlos.

## Criterios de aceptación
- [ ] Crear servicio con nombre, precio, duración y categoría
- [ ] Editar servicio existente y reflejar cambios
- [ ] Desactivar servicio sin borrarlo
- [ ] Validaciones de precio y duración
- [ ] Listado filtrable por estado/categoría

## Reglas de negocio
- No se puede borrar un servicio usado en citas futuras; solo desactivar
- Duración expresada en minutos enteros

## Tareas
- [ ] UX: formularios y listado con filtros
- [ ] Reglas: validaciones y estados
- [ ] Implementación: persistencia y versionado simple
- [ ] QA: CRUD completo
- [ ] Docs: guía de servicios
