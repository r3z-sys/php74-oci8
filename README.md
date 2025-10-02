# 🚀 PHP 7.4 + Oracle Instant Client + Redis Docker Environment

![Docker](https://img.shields.io/badge/Docker-Yes-blue)

Docker environment siap pakai untuk project PHP:

- PHP 7.4-FPM  
- Oracle Instant Client 11.2 + OCI8  
- Redis 5.3.7  
- Nginx sebagai web server  

Cocok untuk aplikasi PHP tradisional atau framework (CodeIgniter, Laravel, dsb).

---

## 📂 Struktur Project



## ⚡ Quick Start

1. Build image:

```bash
docker-compose build php```

2. Jalankan semua service:
```docker-compose up -d```

3. Akses aplikasi:
```http://localhost:8080```

🔧 Konfigurasi PHP

Override setting PHP via php/upload.ini, misal:
```ini
upload_max_filesize = 20M
post_max_size = 25M
memory_limit = 512M```