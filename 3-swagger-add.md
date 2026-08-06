# Swagger

## Swagger paketini yükleme

```bash
dotnet add package Swashbuckle.AspNetCore
```

## Program.cs konfigürasyon

```c#
builder.Services.AddControllers();

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();


if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```

## /swagger uzatısı ile erişebilirsin