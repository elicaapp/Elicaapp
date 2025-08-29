# 🗄️ **Stack de Supabase para ElicaApp**

## 📋 **Resumen Ejecutivo**

**Supabase** es la base de datos elegida para ElicaApp, proporcionando una solución PostgreSQL completamente gestionada con características adicionales como autenticación, real-time subscriptions, y una interfaz de administración intuitiva.

---

## 🏗️ **Arquitectura de Supabase**

### **Componentes Principales**
- **PostgreSQL 15+**: Motor de base de datos principal
- **Supabase Auth**: Sistema de autenticación integrado
- **Supabase Storage**: Almacenamiento de archivos
- **Supabase Edge Functions**: Funciones serverless
- **Supabase Dashboard**: Interfaz de administración web
- **Supabase CLI**: Herramientas de línea de comandos

### **Ventajas para ElicaApp**
- **Escalabilidad**: Crecimiento automático según demanda
- **Seguridad**: Certificaciones SOC2, GDPR, HIPAA
- **Performance**: Optimizaciones automáticas de PostgreSQL
- **Desarrollo Rápido**: APIs automáticas y herramientas integradas
- **Costos**: Plan gratuito generoso, precios predecibles

---

## 🔧 **Configuración Técnica**

### **Requisitos del Sistema**
- **Versión**: Supabase Cloud (PostgreSQL 15+)
- **Región**: Closest to target users (Latam)
- **Plan**: Pro plan para producción
- **Backup**: Automático cada 24 horas
- **Retención**: 7 días de backups

### **Variables de Entorno**
```bash
# .env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
SUPABASE_DB_PASSWORD=your-db-password
```

### **Connection String para .NET Core**
```csharp
// appsettings.json
"ConnectionStrings": {
  "DefaultConnection": "Host=db.your-project.supabase.co;Database=postgres;Username=postgres;Password=your-password;Port=5432;SSL Mode=Require;Trust Server Certificate=true;"
}
```

---

## 🚀 **Integración con .NET Core**

### **Entity Framework Core**
```csharp
// Program.cs
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));

// Migrations
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### **Supabase Client (Opcional)**
```csharp
// Para funcionalidades adicionales de Supabase
builder.Services.AddScoped<ISupabaseClient>(provider =>
{
    var url = builder.Configuration["Supabase:Url"];
    var key = builder.Configuration["Supabase:AnonKey"];
    return new SupabaseClient(url, key);
});
```

### **Configuración de Migraciones**
```csharp
// Migrations/InitialCreate.cs
public partial class InitialCreate : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        // Esquemas base de ElicaApp
        migrationBuilder.CreateTable(
            name: "users",
            columns: table => new
            {
                id = table.Column<Guid>(type: "uuid", nullable: false),
                email = table.Column<string>(type: "text", nullable: false),
                created_at = table.Column<DateTime>(type: "timestamp with time zone", nullable: false),
                // ... más campos
            });
    }
}
```

---

## 📊 **Esquemas de Base de Datos**

### **Tablas Principales**
```sql
-- Usuarios del sistema
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email TEXT UNIQUE NOT NULL,
    full_name TEXT NOT NULL,
    phone TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Negocios
CREATE TABLE businesses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    name TEXT NOT NULL,
    description TEXT,
    address TEXT,
    phone TEXT,
    email TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Servicios
CREATE TABLE services (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    business_id UUID REFERENCES businesses(id),
    name TEXT NOT NULL,
    description TEXT,
    duration_minutes INTEGER NOT NULL,
    price DECIMAL(10,2),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Citas
CREATE TABLE appointments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    service_id UUID REFERENCES services(id),
    user_id UUID REFERENCES users(id),
    business_id UUID REFERENCES businesses(id),
    appointment_date TIMESTAMPTZ NOT NULL,
    status TEXT DEFAULT 'pending',
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### **Índices de Performance**
```sql
-- Índices para consultas frecuentes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_businesses_user_id ON businesses(user_id);
CREATE INDEX idx_services_business_id ON services(business_id);
CREATE INDEX idx_appointments_date ON appointments(appointment_date);
CREATE INDEX idx_appointments_business_date ON appointments(business_id, appointment_date);
```

---

## 🔐 **Seguridad y Autenticación**

### **Row Level Security (RLS)**
```sql
-- Habilitar RLS en todas las tablas
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE businesses ENABLE ROW LEVEL SECURITY;
ALTER TABLE services ENABLE ROW LEVEL SECURITY;
ALTER TABLE appointments ENABLE ROW LEVEL SECURITY;

-- Políticas de seguridad
CREATE POLICY "Users can view own data" ON users
    FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Business owners can manage their business" ON businesses
    FOR ALL USING (auth.uid() = user_id);

CREATE POLICY "Users can view public business info" ON businesses
    FOR SELECT USING (true);
```

### **Autenticación con Supabase Auth**
```csharp
// AuthController.cs
[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] LoginRequest request)
{
    // Integración con Supabase Auth
    var authResponse = await _supabaseClient.Auth.SignIn(request.Email, request.Password);
    
    if (authResponse.User != null)
    {
        var token = authResponse.AccessToken;
        return Ok(new { token, user = authResponse.User });
    }
    
    return Unauthorized();
}
```

---

## 📈 **Performance y Optimización**

### **Configuración de Pool de Conexiones**
```csharp
// Program.cs
builder.Services.AddDbContext<AppDbContext>(options =>
{
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection"), 
        npgsqlOptions =>
        {
            npgsqlOptions.MaxPoolSize(20);
            npgsqlOptions.MinPoolSize(5);
            npgsqlOptions.ConnectionLifetime(300);
        });
});
```

### **Query Optimization**
```csharp
// Repository pattern con queries optimizadas
public async Task<List<Appointment>> GetAppointmentsByBusinessAsync(Guid businessId, DateTime date)
{
    return await _context.Appointments
        .Include(a => a.Service)
        .Include(a => a.User)
        .Where(a => a.BusinessId == businessId && 
                   a.AppointmentDate.Date == date.Date)
        .OrderBy(a => a.AppointmentDate)
        .AsNoTracking()
        .ToListAsync();
}
```

### **Caching Strategy**
```csharp
// Redis + Supabase para cache
public async Task<Business> GetBusinessAsync(Guid id)
{
    var cacheKey = $"business:{id}";
    var cached = await _cache.GetAsync<Business>(cacheKey);
    
    if (cached != null) return cached;
    
    var business = await _context.Businesses
        .Include(b => b.Services)
        .FirstOrDefaultAsync(b => b.Id == id);
    
    if (business != null)
    {
        await _cache.SetAsync(cacheKey, business, TimeSpan.FromMinutes(30));
    }
    
    return business;
}
```

---

## 🔄 **Migraciones y Deployment**

### **Supabase Migrations**
```bash
# Instalar Supabase CLI
npm install -g supabase

# Login
supabase login

# Inicializar proyecto
supabase init

# Crear migración
supabase migration new create_initial_schema

# Aplicar migraciones
supabase db push

# Reset local
supabase db reset
```

### **Entity Framework Migrations**
```bash
# Crear migración
dotnet ef migrations add InitialSchema

# Aplicar migración
dotnet ef database update

# Generar script SQL
dotnet ef migrations script
```

### **CI/CD Integration**
```yaml
# .github/workflows/database.yml
name: Database Migration
on:
  push:
    branches: [main]
jobs:
  migrate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      - name: Run migrations
        run: |
          dotnet ef database update
        env:
          CONNECTION_STRING: ${{ secrets.SUPABASE_CONNECTION_STRING }}
```

---

## 📊 **Monitoreo y Observabilidad**

### **Supabase Dashboard**
- **Database**: Monitoreo de queries, performance, conexiones
- **Auth**: Logs de autenticación, usuarios activos
- **Storage**: Uso de almacenamiento, archivos
- **Edge Functions**: Logs y métricas de funciones

### **Logs y Métricas**
```csharp
// Program.cs
builder.Services.AddLogging(logging =>
{
    logging.AddConsole();
    logging.AddDebug();
    logging.AddSerilog(new LoggerConfiguration()
        .WriteTo.Console()
        .WriteTo.File("logs/supabase-.txt", rollingInterval: RollingInterval.Day)
        .CreateLogger());
});
```

### **Health Checks**
```csharp
// Program.cs
builder.Services.AddHealthChecks()
    .AddNpgSql(builder.Configuration.GetConnectionString("DefaultConnection"))
    .AddRedis(builder.Configuration.GetConnectionString("Redis"));

app.MapHealthChecks("/health");
```

---

## 🚨 **Backup y Recovery**

### **Backup Automático**
- **Frecuencia**: Cada 24 horas
- **Retención**: 7 días
- **Tipo**: Point-in-time recovery
- **Región**: Multi-region backup

### **Recovery Procedures**
```sql
-- Restaurar desde backup
-- Usar Supabase Dashboard o CLI

-- Verificar integridad
SELECT schemaname, tablename, attname, n_distinct, correlation 
FROM pg_stats 
WHERE schemaname NOT IN ('information_schema', 'pg_catalog');
```

---

## 🔮 **Roadmap de Supabase para ElicaApp**

### **MVP (Etapa 1)**
- [x] Configuración básica de PostgreSQL
- [x] Esquemas base de datos
- [x] Autenticación básica
- [x] Migraciones EF Core

### **Optimización (Etapa 2)**
- [ ] Row Level Security avanzado
- [ ] Índices optimizados
- [ ] Particionamiento de tablas
- [ ] Backup automatizado

### **Escalabilidad (Etapa 3)**
- [ ] Read replicas
- [ ] Sharding horizontal
- [ ] Cache distribuido
- [ ] Monitoreo avanzado

### **Innovación (Etapa 4)**
- [ ] Machine Learning con pgvector
- [ ] GraphQL con PostGraphile
- [ ] Real-time subscriptions
- [ ] Edge Functions

---

## 📚 **Recursos y Documentación**

### **Documentación Oficial**
- [Supabase Documentation](https://supabase.com/docs)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)

### **Herramientas Recomendadas**
- **Supabase CLI**: Para migraciones y gestión
- **pgAdmin**: Para administración avanzada
- **DBeaver**: Cliente universal de base de datos
- **Postman**: Para testing de APIs

### **Comunidad y Soporte**
- [Supabase Discord](https://discord.supabase.com/)
- [GitHub Issues](https://github.com/supabase/supabase/issues)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/supabase)

---

## ✅ **Checklist de Implementación**

### **Configuración Inicial**
- [ ] Crear proyecto en Supabase Cloud
- [ ] Configurar variables de entorno
- [ ] Configurar connection string en .NET Core
- [ ] Probar conexión a base de datos

### **Desarrollo**
- [ ] Crear modelos EF Core
- [ ] Implementar migraciones iniciales
- [ ] Configurar RLS policies
- [ ] Implementar autenticación

### **Testing**
- [ ] Tests de conexión a base de datos
- [ ] Tests de migraciones
- [ ] Tests de performance
- [ ] Tests de seguridad

### **Producción**
- [ ] Configurar backup automático
- [ ] Configurar monitoreo
- [ ] Configurar alertas
- [ ] Documentar procedimientos de recovery

---

*Última actualización: Diciembre 2024*
*Versión: v1.0.0*
