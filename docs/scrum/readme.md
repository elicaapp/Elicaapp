# 📖 Guía Scrum — ElicaApp (MVP)

## Objetivo
Organizar el trabajo de ElicaApp en iteraciones cortas para entregar valor temprano y conseguir feedback continuo.

## Roles
- Product Owner: Prioriza el backlog, define objetivo de sprint y acepta entregas.
- Scrum Master: Facilita eventos, remueve impedimentos, mejora del proceso.
- Equipo de Desarrollo: Diseña, implementa, prueba y documenta incrementos.

## Eventos
- Sprint Planning: Selección y objetivo del sprint.
- Daily: 15 minutos para sincronizar y desbloquear.
- Review: Demostración del incremento y feedback.
- Retrospective: Mejoras del proceso para el siguiente sprint.

## Artefactos
- Product Backlog: Lista priorizada de valor.
- Sprint Backlog: Compromiso del sprint actual.
- Incremento: Funcionalidad lista y validada.

## Definición de Hecho (DoD)
- [ ] Criterios de aceptación cumplidos
- [ ] Pruebas funcionales pasadas
- [ ] Documentación de usuario actualizada
- [ ] Registro de cambios documentado
- [ ] Aprobación del PO

## Enlaces rápidos
- [Product Backlog](./Product_Backlog.md)
- [Sprint 01](./Sprints/Sprint-01.md)
- [Plantilla de Historia](./Templates/Story_Template.md)


Vamos a estructurar esto como una **guía práctica para trabajar con Scrum** enfocada en **ElicaApp** y su **MVP**, y vamos a dejar ya listas las **historias de usuario** que podemos convertir en tickets para el tablero (Jira, Trello, ClickUp, etc.).

---

# 📖 Guía Scrum para ElicaApp (MVP)

## 🎯 Objetivo
Organizar el trabajo del equipo para entregar un **Producto Mínimo Viable** de ElicaApp en iteraciones cortas, priorizando valor para el usuario y feedback temprano.

---

## 🧩 Roles en Scrum
- **Product Owner (PO)**: Define la visión, prioriza el backlog y valida entregas.  
- **Scrum Master**: Facilita el proceso, elimina bloqueos y asegura que se sigan las prácticas Scrum.  
- **Equipo de Desarrollo**: Diseña, construye, prueba y entrega incrementos funcionales.

---

## 🔄 Eventos Clave
1. **Sprint Planning**: Seleccionar historias de usuario para el sprint y definir objetivos.
2. **Daily Scrum**: Reunión breve (máx. 15 min) para sincronizar avances y bloqueos.
3. **Sprint Review**: Presentar el incremento al PO y stakeholders.
4. **Sprint Retrospective**: Analizar qué funcionó y qué mejorar para el siguiente sprint.

---

## 📂 Artefactos
- **Product Backlog**: Lista priorizada de historias de usuario.
- **Sprint Backlog**: Historias seleccionadas para el sprint actual.
- **Incremento**: Versión funcional del producto al final de cada sprint.

---

## 📝 Historias de Usuario para el MVP de ElicaApp

Formato:  
**Como** [tipo de usuario]  
**Quiero** [funcionalidad]  
**Para** [beneficio / resultado]

---

### **1. Configuración inicial del negocio**
- **Como** dueño de negocio  
  **Quiero** registrar mi negocio con datos básicos (nombre, tipo, logo, colores)  
  **Para** personalizar la app y que refleje mi marca.

---

### **2. Gestión de servicios**
- **Como** dueño de negocio  
  **Quiero** crear, editar y eliminar servicios con precio y duración  
  **Para** que mis clientes puedan reservarlos fácilmente.

---

### **3. Agenda de citas**
- **Como** cliente  
  **Quiero** ver disponibilidad de horarios y reservar un servicio  
  **Para** agendar mi cita sin tener que llamar.

---

### **4. Confirmación y recordatorio de cita**
- **Como** cliente  
  **Quiero** recibir confirmación y recordatorio de mi cita  
  **Para** no olvidarla y llegar a tiempo.

---

### **5. Panel de control por rol**
- **Como** empleado  
  **Quiero** ver solo mis citas y tareas asignadas  
  **Para** enfocarme en mi trabajo diario.

---

### **6. Historial de citas**
- **Como** cliente  
  **Quiero** ver mi historial de citas pasadas  
  **Para** repetir servicios o con el mismo profesional.

---

### **7. Gestión de inventario básica**
- **Como** dueño de negocio  
  **Quiero** registrar productos y su stock  
  **Para** saber cuándo debo reabastecer.

---

### **8. Reporte básico**
- **Como** dueño de negocio  
  **Quiero** ver un resumen de citas e ingresos del día  
  **Para** tomar decisiones rápidas.

---

### **9. Personalización visual**
- **Como** dueño de negocio  
  **Quiero** cambiar colores, tipografía y logo desde la app  
  **Para** mantener mi identidad visual.

---

### **10. Roles y permisos**
- **Como** dueño de negocio  
  **Quiero** asignar roles y permisos a empleados  
  **Para** controlar el acceso a funciones sensibles.

---

## 📌 Cosas que tomar en cuenta para trabajar estas historias en Scrum
- **Priorizar**: Empezar por las que generan más valor al cliente (configuración inicial, agenda de citas, confirmación).
- **Dividir**: Si una historia es muy grande, crear subtareas técnicas o historias más pequeñas.
- **Definición de Hecho (DoD)**: Cada historia debe estar probada, documentada y lista para producción.
- **Feedback temprano**: Muestrar avances al PO y usuarios clave en cada Sprint Review.

### Estructura de carpetas y archivos del trabajo con Scrum ###

```Plaintext 
docs/
└─ scrum/
   ├─ README.md
   ├─ Product_Backlog.md
   ├─ Templates/
   │  ├─ Story_Template.md
   │  └─ Sprint_Plan_Template.md
   ├─ Sprints/
   │  ├─ Sprint-01.md
   │  └─ Sprint-02.md
   └─ Stories/
      ├─ US-001-configuracion-inicial-negocio.md
      ├─ US-002-gestion-servicios.md
      ├─ US-003-agenda-citas.md
      ├─ US-004-confirmacion-recordatorios.md
      ├─ US-005-dashboard-por-rol.md
      ├─ US-006-historial-citas.md
      ├─ US-007-inventario-basico.md
      ├─ US-008-reporte-basico-dia.md
      ├─ US-009-personalizacion-visual.md
      └─ US-010-roles-y-permisos.md
```