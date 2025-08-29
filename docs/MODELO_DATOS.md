# 🗄️ Modelo de Datos y Relaciones - ElicaApp

## 🎯 Objetivo
Definir la estructura de datos que soporta la lógica de negocio, garantizando integridad, escalabilidad y flexibilidad.

---

## 📌 Entidades Principales
- **Business**: Información del negocio.
- **Theme**: Configuración visual (colores, fuentes, logotipos).
- **User**: Datos de usuarios con roles y permisos.
- **Service**: Servicios ofrecidos por el negocio.
- **Appointment**: Citas y reservas.
- **InventoryItem**: Productos en inventario.
- **BusinessSettings**: Configuraciones generales.

---

## 🔗 Relaciones Clave
- Un **Business** tiene un **Theme**.
- Un **Business** tiene muchos **Users**.
- Un **User** puede tener varios **Roles**.
- Un **Service** pertenece a un **Business** y puede estar en muchas **Appointments**.
- Un **InventoryItem** pertenece a un **Business** y tiene muchos movimientos.

---

## 📊 Normalización
- Datos separados en tablas específicas para evitar redundancia.
- Uso de claves foráneas para mantener integridad referencial.
- Configuración visual desacoplada para permitir cambios sin afectar la lógica operativa.

---

## 📂 Relación con otros documentos
- [Lógica de Negocio](LOGICA_NEGOCIO.md)
- [Hoja de Ruta de Funcionalidades](ROADMAP.md)
