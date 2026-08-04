# .NET Clean Architecture & Solution Cheat Sheet

## 1. Solution (.sln) Nedir ve Neden Kullanılır?
- **Solution (.sln):** Birbiriyle ilişkili projeleri tek bir çatı altında toplayan ana kapsayıcıdır.
- **Sorumlulukların Ayrılması (Separation of Concerns):** Kodları tek bir klasöre yığmak yerine işlevlerine göre bağımsız projelere böleriz.

---

## 2. Katmanların Görevleri (Clean Architecture)

| Katman | Görevi | Bağımlılık Durumu |
| :--- | :--- | :--- |
| **Domain** | Entity'ler (User, Seat, Reservation) ve kurallar bulunur. | **Bağımsızdır.** Hiçbir katmanı görmez. |
| **Application** | İş mantığı (Business Logic), DTO'lar ve Interface'ler durur. | Sadece **Domain**'i görür. |
| **Infrastructure** | Veritabanı (EF Core), Redis, RabbitMQ ve dış servisler bağlanan katmandır. | **Application** ve **Domain**'i görür. |
| **API** | Controller'lar, HTTP endpoint'leri ve Swagger yer alır. | **Application** ve **Infrastructure**'ı görür. |

---

## 3. CLI ile Solution Yapısı Oluşturma Komutları

**Solution dosyasını oluştur**
```bash
dotnet new sln -n ProjeAdi
```

**Katman projelerini oluştur (src klasöründe)**
```bash
dotnet new classlib -o src/Proje.Domain
dotnet new classlib -o src/Proje.Application
dotnet new classlib -o src/Proje.Infrastructure
dotnet new webapi -o src/Proje.Api
```
**Projeleri Solution'a ekle**
```bash
dotnet sln add src/Proje.Domain/Proje.Domain.csproj
dotnet sln add src/Proje.Application/Proje.Application.csproj
dotnet sln add src/Proje.Infrastructure/Proje.Infrastructure.csproj
dotnet sln add src/Proje.Api/Proje.Api.csproj
```

**Katman bağımlılıklarını bağla (Doğru yönler!)**
```bash
dotnet add src/Proje.Application/Proje.Application.csproj reference src/Proje.Domain/Proje.Domain.csproj
dotnet add src/Proje.Infrastructure/Proje.Infrastructure.csproj reference src/Proje.Application/Proje.Application.csproj
dotnet add src/Proje.Api/Proje.Api.csproj reference src/Proje.Application/Proje.Application.csproj
dotnet add src/Proje.Api/Proje.Api.csproj reference src/Proje.Infrastructure/Proje.Infrastructure.csproj
```

**Test derlemesi**
```bash
dotnet build
```
