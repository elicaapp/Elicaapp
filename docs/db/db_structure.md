## 🧠 Visión General del Modelo de Datos

Esta plataforma debe estar dividida en **módulos funcionales** y **módulos de personalización**, todos interrelacionados. Aquí te presento una estructura lógica dividida en secciones clave:

### 1. 🏢 Entidad Principal: Negocio.

```plaintext
Business
- id (PK)
- name
- type (salón, restaurante, etc.)
- owner_id (FK → User)
- created_at
- updated_at
```

---

### 2. 🎨 Personalización Visual.

```plaintext
BusinessTheme
- id (PK)
- business_id (FK → Business)
- primary_color
- secondary_color
- accent_color
- font_family
- button_style (rounded, square, etc.)
- layout_style (sidebar, topbar, etc.)
- logo_url
- favicon_url
- background_image_url
- custom_css (opcional)
```

> 💡 **Lógica recomendada**: Al iniciar la app, hacemos un query a `BusinessTheme` según el `business_id`, lo guardamos en local (por ejemplo, en `localStorage` o SQLite) y lo usamos para renderizar la interfaz. Si el usuario cambia algo, actualizamos la base de datos y refrescamos el local.

---

### 3. 👤 Usuarios y Roles

```plaintext
User
- id (PK)
- name
- email
- password_hash
- role (admin, empleado, cliente)
- business_id (FK → Business)
- profile_picture_url
- is_active
```

```plaintext
RolePermissions
- id (PK)
- role
- can_edit_theme
- can_manage_appointments
- can_view_reports
- can_manage_inventory
```

---

### 4. 📅 Citas / Reservas

```plaintext
Appointment
- id (PK)
- business_id (FK → Business)
- client_id (FK → User)
- employee_id (FK → User)
- service_id (FK → Service)
- date
- time
- status (pendiente, confirmada, cancelada)
- notes
```

---

### 5. 💇 Servicios Ofrecidos

```plaintext
Service
- id (PK)
- business_id (FK → Business)
- name
- description
- duration_minutes
- price
- category (corte, color, comida, etc.)
- is_active
```

---

### 6. 📦 Inventario (opcional para restaurantes o salones)

```plaintext
InventoryItem
- id (PK)
- business_id (FK → Business)
- name
- description
- quantity
- unit (ml, unidades, etc.)
- price
- supplier
- last_updated
```

---

### 7. 📊 Reportes y Métricas

```plaintext
BusinessReport
- id (PK)
- business_id (FK → Business)
- report_type (ventas, citas, inventario)
- generated_at
- data_json (estructura flexible)
```

---

### 8. ⚙️ Configuraciones Generales

```plaintext
BusinessSettings
- id (PK)
- business_id (FK → Business)
- timezone
- currency
- language
- opening_hours_json
- enable_notifications
- enable_online_booking
```

---

## 🔄 Relación entre Módulos

| Módulo            | Relacionado con...                |
|-------------------|-----------------------------------|
| `BusinessTheme`   | `Business`                        |
| `User`            | `Business`, `RolePermissions`     |
| `Appointment`     | `User`, `Service`, `Business`     |
| `Service`         | `Business`                        |
| `InventoryItem`   | `Business`                        |
| `BusinessReport`  | `Business`                        |
| `BusinessSettings`| `Business`                        |
