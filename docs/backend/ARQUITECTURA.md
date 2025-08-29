# 🏗️ **ARQUITECTURA DEL BACKEND - ElicaApp**

## 🎯 **Visión General de la Arquitectura**

### **🏛️ Principios Arquitectónicos**
- **Clean Architecture**: Separación clara de responsabilidades
- **SOLID Principles**: Principios de diseño robusto
- **Dependency Inversion**: Inversión de dependencias
- **Separation of Concerns**: Separación de conceptos
- **Testability**: Código fácilmente testeable

### **🔄 Patrón de Arquitectura**
```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │ Controllers │  │ Middleware  │  │   DTOs/ViewModels   │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │   Services  │  │ Validators  │  │   AutoMapper        │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     DOMAIN LAYER                           │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │   Entities  │  │  Interfaces │  │   Domain Services   │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  INFRASTRUCTURE LAYER                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │   Repos     │  │   External  │  │   Configuration     │ │
│  │             │  │   Services  │  │                     │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 **Estructura de Carpetas del Proyecto**

```
ElicaApp.API/
├── 📁 Controllers/           # Controladores de la API
│   ├── AuthController.cs
│   ├── BusinessController.cs
│   ├── ServiceController.cs
│   ├── AppointmentController.cs
│   └── DashboardController.cs
├── 📁 Middleware/            # Middleware personalizado
│   ├── ExceptionMiddleware.cs
│   ├── LoggingMiddleware.cs
│   └── PerformanceMiddleware.cs
├── 📁 DTOs/                  # Data Transfer Objects
│   ├── Auth/
│   ├── Business/
│   ├── Service/
│   └── Appointment/
├── 📁 Services/              # Lógica de negocio
│   ├── Interfaces/
│   ├── AuthService.cs
│   ├── BusinessService.cs
│   ├── ServiceService.cs
│   └── AppointmentService.cs
├── 📁 Validators/            # Validaciones con FluentValidation
│   ├── AuthValidators.cs
│   ├── BusinessValidators.cs
│   └── ServiceValidators.cs
├── 📁 Domain/                # Entidades y lógica de dominio
│   ├── Entities/
│   ├── Interfaces/
│   ├── Enums/
│   └── Exceptions/
├── 📁 Infrastructure/        # Implementaciones de infraestructura
│   ├── Data/
│   ├── Repositories/
│   ├── ExternalServices/
│   └── Configuration/
├── 📁 Tests/                 # Tests unitarios y de integración
│   ├── Unit/
│   ├── Integration/
│   └── Fixtures/
└── 📁 Configuration/         # Configuración de la aplicación
    ├── appsettings.json
    ├── appsettings.Development.json
    └── appsettings.Production.json
```

---

## 🔧 **Componentes Principales**

### **🎮 Controllers (Presentation Layer)**
```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize]
public class BusinessController : ControllerBase
{
    private readonly IBusinessService _businessService;
    private readonly ILogger<BusinessController> _logger;

    public BusinessController(IBusinessService businessService, ILogger<BusinessController> logger)
    {
        _businessService = businessService;
        _logger = logger;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<BusinessDto>>> GetAll()
    {
        var businesses = await _businessService.GetAllAsync();
        return Ok(businesses);
    }
}
```

### **⚙️ Services (Application Layer)**
```csharp
public class BusinessService : IBusinessService
{
    private readonly IBusinessRepository _businessRepository;
    private readonly IMapper _mapper;
    private readonly ILogger<BusinessService> _logger;

    public BusinessService(
        IBusinessRepository businessRepository,
        IMapper mapper,
        ILogger<BusinessService> logger)
    {
        _businessRepository = businessRepository;
        _mapper = mapper;
        _logger = logger;
    }

    public async Task<IEnumerable<BusinessDto>> GetAllAsync()
    {
        var businesses = await _businessRepository.GetAllAsync();
        return _mapper.Map<IEnumerable<BusinessDto>>(businesses);
    }
}
```

### **🏗️ Entities (Domain Layer)**
```csharp
public class Business : BaseEntity
{
    public string Name { get; set; }
    public string Description { get; set; }
    public string Address { get; set; }
    public string Phone { get; set; }
    public string Email { get; set; }
    public BusinessType Type { get; set; }
    public BusinessStatus Status { get; set; }
    
    // Navigation properties
    public Guid OwnerId { get; set; }
    public virtual User Owner { get; set; }
    public virtual ICollection<Service> Services { get; set; }
    public virtual ICollection<Appointment> Appointments { get; set; }
}
```

### **🗄️ Repositories (Infrastructure Layer)**
```csharp
public class BusinessRepository : IBusinessRepository
{
    private readonly ApplicationDbContext _context;

    public BusinessRepository(ApplicationDbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<Business>> GetAllAsync()
    {
        return await _context.Businesses
            .Include(b => b.Owner)
            .Include(b => b.Services)
            .Where(b => b.Status == BusinessStatus.Active)
            .ToListAsync();
    }
}
```

---

## 🔐 **Sistema de Autenticación y Autorización**

### **🔑 JWT Token Structure**
```json
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "user-id",
    "email": "user@example.com",
    "role": "BusinessOwner",
    "businessId": "business-id",
    "iat": 1640995200,
    "exp": 1641081600
  }
}
```

### **🛡️ Roles y Permisos**
```csharp
public enum UserRole
{
    Admin,
    BusinessOwner,
    ServiceProvider,
    Customer
}

public enum Permission
{
    ReadBusiness,
    WriteBusiness,
    ReadServices,
    WriteServices,
    ReadAppointments,
    WriteAppointments,
    ReadReports,
    WriteReports
}
```

---

## 📊 **Patrones de Diseño Implementados**

### **🏭 Factory Pattern**
```csharp
public interface INotificationServiceFactory
{
    INotificationService Create(NotificationType type);
}

public class NotificationServiceFactory : INotificationServiceFactory
{
    public INotificationService Create(NotificationType type)
    {
        return type switch
        {
            NotificationType.Email => new EmailNotificationService(),
            NotificationType.SMS => new SmsNotificationService(),
            NotificationType.Push => new PushNotificationService(),
            _ => throw new ArgumentException("Invalid notification type")
        };
    }
}
```

### **📋 Repository Pattern**
```csharp
public interface IRepository<T> where T : BaseEntity
{
    Task<T> GetByIdAsync(Guid id);
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(Guid id);
}
```

### **🔄 Unit of Work Pattern**
```csharp
public interface IUnitOfWork : IDisposable
{
    IBusinessRepository Businesses { get; }
    IServiceRepository Services { get; }
    IAppointmentRepository Appointments { get; }
    Task<int> SaveChangesAsync();
}
```

---

## 🧪 **Estrategia de Testing**

### **📝 Tests Unitarios**
```csharp
[Fact]
public async Task GetAllAsync_ShouldReturnActiveBusinesses()
{
    // Arrange
    var mockRepository = new Mock<IBusinessRepository>();
    var mockMapper = new Mock<IMapper>();
    var mockLogger = new Mock<ILogger<BusinessService>>();
    
    var expectedBusinesses = new List<Business> { /* test data */ };
    mockRepository.Setup(r => r.GetAllAsync()).ReturnsAsync(expectedBusinesses);
    
    var service = new BusinessService(mockRepository.Object, mockMapper.Object, mockLogger.Object);
    
    // Act
    var result = await service.GetAllAsync();
    
    // Assert
    Assert.NotNull(result);
    Assert.Equal(expectedBusinesses.Count, result.Count());
}
```

### **🔗 Tests de Integración**
```csharp
[Fact]
public async Task GetAllBusinesses_ShouldReturnOkResult()
{
    // Arrange
    var client = _factory.CreateClient();
    
    // Act
    var response = await client.GetAsync("/api/business");
    
    // Assert
    response.EnsureSuccessStatusCode();
    Assert.Equal("application/json; charset=utf-8", response.Content.Headers.ContentType.ToString());
}
```

---

## 🚀 **Configuración y Deployment**

### **⚙️ appsettings.json**
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=your-supabase-host;Database=postgres;Username=postgres;Password=your-password"
  },
  "JwtSettings": {
    "SecretKey": "your-secret-key-here",
    "Issuer": "ElicaApp",
    "Audience": "ElicaAppUsers",
    "ExpirationMinutes": 60
  },
  "Redis": {
    "ConnectionString": "localhost:6379"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

### **🐳 Docker Configuration**
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["ElicaApp.API/ElicaApp.API.csproj", "ElicaApp.API/"]
RUN dotnet restore "ElicaApp.API/ElicaApp.API.csproj"
COPY . .
WORKDIR "/src/ElicaApp.API"
RUN dotnet build "ElicaApp.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ElicaApp.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ElicaApp.API.dll"]
```

---

## 📈 **Métricas y Monitoreo**

### **📊 Health Checks**
```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks()
            .AddDbContextCheck<ApplicationDbContext>()
            .AddRedis(Configuration.GetConnectionString("Redis"))
            .AddUrlGroup(new Uri("https://api.external-service.com"), "External API");
    }
}
```

### **📝 Logging Estructurado**
```csharp
public class BusinessController : ControllerBase
{
    [HttpGet]
    public async Task<ActionResult<IEnumerable<BusinessDto>>> GetAll()
    {
        using var scope = _logger.BeginScope(new Dictionary<string, object>
        {
            ["Action"] = "GetAll",
            ["Controller"] = "Business",
            ["UserId"] = User.GetUserId()
        });
        
        _logger.LogInformation("Fetching all businesses");
        var businesses = await _businessService.GetAllAsync();
        _logger.LogInformation("Retrieved {Count} businesses", businesses.Count());
        
        return Ok(businesses);
    }
}
```

---

## 🔒 **Seguridad y Compliance**

### **🛡️ OWASP Top 10 Compliance**
- **SQL Injection**: Entity Framework Core parameterized queries
- **Authentication**: JWT + Identity con refresh tokens
- **Authorization**: Role-based access control (RBAC)
- **Input Validation**: FluentValidation + model binding
- **Rate Limiting**: Implementado con middleware personalizado
- **CORS**: Configuración restrictiva por origen
- **HTTPS**: Forzado en producción
- **Logging**: Logs de auditoría para acciones sensibles

### **🔐 Data Protection**
```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDataProtection()
            .PersistKeysToDbContext<ApplicationDbContext>();
    }
}
```

---

## 📚 **Referencias y Recursos**

### **🔗 Enlaces Oficiales**
- [.NET 8 Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [ASP.NET Core Documentation](https://docs.microsoft.com/en-us/aspnet/core/)
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

### **📖 Libros Recomendados**
- "Clean Architecture" - Robert C. Martin
- "Domain-Driven Design" - Eric Evans
- "Patterns of Enterprise Application Architecture" - Martin Fowler

---

*Última actualización: Agosto 2025*
*Versión: v1.0.0*
