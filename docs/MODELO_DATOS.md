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
- **AuditLog**: Registro de auditoría para cumplimiento.
- **Notification**: Sistema de notificaciones inteligente.

---

## 🔗 Relaciones Clave
- Un **Business** tiene un **Theme**.
- Un **Business** tiene muchos **Users**.
- Un **User** puede tener varios **Roles**.
- Un **Service** pertenece a un **Business** y puede estar en muchas **Appointments**.
- Un **InventoryItem** pertenece a un **Business** y tiene muchos movimientos.
- Todas las operaciones críticas se registran en **AuditLog**.

---

## 📊 Normalización
- Datos separados en tablas específicas para evitar redundancia.
- Uso de claves foráneas para mantener integridad referencial.
- Configuración visual desacoplada para permitir cambios sin afectar la lógica operativa.
- Normalización hasta 3FN (Tercera Forma Normal) con casos específicos en BCNF.

---

## 🚀 Optimización de Consultas

### **Índices Estratégicos**
```sql
-- Índices para búsquedas frecuentes
CREATE INDEX idx_appointments_date_status ON appointments(date, status);
CREATE INDEX idx_appointments_business_employee ON appointments(business_id, employee_id);
CREATE INDEX idx_services_business_active ON services(business_id, is_active);
CREATE INDEX idx_users_business_role ON users(business_id, role);
CREATE INDEX idx_inventory_business_stock ON inventory_items(business_id, stock_quantity);
```

### **Particionamiento de Tablas**
- **Appointments**: Particionado por fecha (mensual)
- **AuditLog**: Particionado por fecha (semanal)
- **BusinessReports**: Particionado por negocio y fecha

### **Cache Inteligente**
- **Configuraciones visuales**: Cache en Redis con TTL de 1 hora
- **Datos estáticos**: Cache en memoria para consultas frecuentes
- **Métricas en tiempo real**: Cache con invalidación automática

---

## 🔒 Seguridad y Privacidad

### **Encriptación de Datos**
```sql
-- Campos sensibles encriptados
ALTER TABLE users ADD COLUMN personal_data_encrypted TEXT;
ALTER TABLE businesses ADD COLUMN financial_info_encrypted TEXT;
```

### **Manejo de Contraseñas**
- Hash con bcrypt y salt único
- Política de complejidad configurable
- Rotación automática de contraseñas

### **Auditoría Completa**
```sql
CREATE TABLE audit_log (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    action VARCHAR(100) NOT NULL,
    entity_type VARCHAR(50) NOT NULL,
    entity_id UUID NOT NULL,
    old_values JSONB,
    new_values JSONB,
    ip_address INET,
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 📈 Escalabilidad y Performance

### **Estrategias de Crecimiento**
- **Sharding horizontal** por business_id para negocios grandes
- **Read replicas** para reportes y consultas de solo lectura
- **Connection pooling** para manejar múltiples conexiones concurrentes

### **Monitoreo de Performance**
- **Query performance**: Logging de consultas lentas (>100ms)
- **Connection metrics**: Monitoreo de conexiones activas y pool
- **Storage growth**: Alertas automáticas de crecimiento de base de datos

---

## 🔄 Migración y Versionado

### **Estrategia de Migración**
- **Versionado de esquemas** con herramientas como Flyway o Liquibase
- **Rollback automático** en caso de errores de migración
- **Backup automático** antes de cada migración

### **Compatibilidad hacia Atrás**
- **Campos opcionales** para nuevas funcionalidades
- **Default values** para mantener compatibilidad
- **Deprecation warnings** para campos obsoletos

---

## 📂 Relación con otros documentos
- [Lógica de Negocio](LOGICA_NEGOCIO.md)
- [Hoja de Ruta de Funcionalidades](ROADMAP.md)
- [Guía de Implementación](../guia_de_implementacion/RutadeImplementación.md)
- [Estructura de Base de Datos](db/db_structure.md)
- [Normalización Avanzada](db/db_normalizer.md)
