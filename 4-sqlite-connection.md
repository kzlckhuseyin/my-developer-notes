## Sqlite

## SQLite Sağlayıcısı ile EF Core tasarım araçlarını yüklemek için bu komutları çalıştırın.

```bash
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

## appsettings.json dosyanızı açın ve SQLite veritabanı kaynağını ekleyin. Belirtilen dosya adı, proje dizininizde otomatik olarak oluşturulacaktır.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localdata.db"
  }
}
```

## Modellerinizi veritabanı tablolarıyla eşleştirmek için (genellikle Data veya Context klasörü içinde) AppDbContext.cs adında yeni bir dosya oluşturun.

```c#

using Microsoft.EntityFrameworkCore;

namespace YourProjectNamespace.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
        }

        // Add your database tables here, for example:
        // public DbSet<Product> Products { get; set; }
    }
}
```

## Program.cs dosyanızı açın ve .UseSqlite() kullanarak bağlamınızı (context) bağımlılık enjeksiyonu kapsayıcısına enjekte edin.

```C#
using Microsoft.EntityFrameworkCore;
using YourProjectNamespace.Data; // Replace with your actual namespace

var builder = WebApplication.CreateBuilder(args);

// 1. Fetch connection string from appsettings.json
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");

// 2. Register AppDbContext with SQLite provider
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlite(connectionString));

builder.Services.AddControllers();
// Learn more about configuring OpenAPI at https://aka.ms
builder.Services.AddOpenApi();

var app = builder.Build();
```

## SQLite dosyasını fiziksel olarak oluşturmak ve bağlamınıza (context) dayalı şemayı meydana getirmek için, CLI kullanarak şu komutları çalıştırın.

```bash
# Öncelikle küresel EF aracının yüklü olduğundan emin olun.
dotnet tool install --global dotnet-ef

# İlk migrasyonu oluşturun
dotnet ef migrations add InitialCreate

# Veritabanı şemasını ve dosyasını oluşturun.
dotnet ef database update
```