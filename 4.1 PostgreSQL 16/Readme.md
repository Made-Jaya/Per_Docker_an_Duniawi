Saya akan membantu Anda membuat container Docker untuk PostgreSQL. Berikut ini langkah-langkahnya:

Pertama, mari buat file `docker-compose.yml` yang akan menyederhanakan proses pembuatan dan konfigurasi container PostgreSQL:

```yaml
version: '3'

services:
  postgres:
    image: postgres:16
    container_name: my-postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
    restart: unless-stopped

volumes:
  postgres-data:
```

Langkah-langkah penggunaan:

1. Simpan kode di atas ke dalam file bernama `docker-compose.yml`
2. Buka terminal dan navigasi ke direktori tempat file tersebut berada
3. Jalankan perintah: `docker-compose up -d`

Ini akan:
- Menggunakan image PostgreSQL versi 16
- Membuat container bernama "my-postgres"
- Mengatur username, password, dan nama database
- Memetakan port 5432 dari container ke port 5432 pada host
- Membuat volume untuk menyimpan data secara permanen
- Mengatur container untuk restart otomatis kecuali jika dihentikan secara manual

Untuk menghubungkan ke database PostgreSQL:
```
psql -h localhost -p 5432 -U postgres
```
Kemudian masukkan password: `password`

Saya akan membantu Anda membuat database baru bernama "mydatabase" di dalam PostgreSQL container Anda. Ada beberapa cara untuk melakukannya:

### Metode 1: Menggunakan bash di dalam container

1. Akses bash di dalam container:
   ```bash
   docker exec -it my-postgres bash
   ```

2. Jalankan psql sebagai user postgres:
   ```bash
   psql -U postgres
   ```

3. Buat database:
   ```sql
   CREATE DATABASE mydatabase;
   ```

### Metode 2: Perintah langsung melalui docker exec

```bash
docker exec -it my-postgres psql -U postgres -c "CREATE DATABASE mydatabase;"
```

### Metode 3: Tambahkan ke docker-compose (jika Anda belum menjalankan container)

Jika Anda belum menjalankan container, Anda bisa memodifikasi file `docker-compose.yml`:

```yaml
version: '3'

services:
  postgres:
    image: postgres:16
    container_name: my-postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init-db.sql:/docker-entrypoint-initdb.d/init-db.sql
    restart: unless-stopped

volumes:
  postgres-data:
```

Lalu buat file `init-db.sql`:
```sql
CREATE DATABASE mydatabase;
```

Apakah Anda ingin saya menjelaskan salah satu metode ini lebih detail?