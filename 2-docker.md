# Docker

## Docker Nedir ve Neden Kullanılır?
- **Docker**, uygulamaları ve onların ihtiyaç duyduğu tüm bağımlılıkları (kütüphaneler, ayarlar, konfigürasyonlar) "Konteyner" (Container) adı verilen hafif ve izole paketlere dönüştürerek çalıştırmaya yarayan açık kaynaklı bir platformdur.

**docker-compose.yml Dosyasını oluştur**

```bash

services:
  # PostgreSQL Veritabanı Servisi
  postgres:
    image: postgres:16-alpine
    container_name: smartqueue_postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: mysecretpassword
      POSTGRES_DB: SmartQueueDb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  # Redis (Distributed Lock & Cache Servisi)
  redis:
    image: redis:7-alpine
    container_name: smartqueue_redis
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```
**Docker Desktop Uygulamasını Aç**

**Komutu çalıştır**

```bash
docker compose up -d
```
