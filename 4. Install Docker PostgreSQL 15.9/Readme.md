Untuk menginstal PostgreSQL di Docker dan menggunakannya dengan konfigurasi yang Anda berikan, berikut langkah-langkahnya:

---

### 1. **Siapkan Docker Compose File**
Buat file `docker-compose.yml` untuk mendefinisikan PostgreSQL.

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15
    container_name: postgres_container
    restart: always
    environment:
      POSTGRES_USER: your_username
      POSTGRES_PASSWORD: your_password
      POSTGRES_DB: your_database
    ports:
      - "5400:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

### 2. **Menjalankan PostgreSQL**
Jalankan perintah berikut di terminal Anda untuk memulai PostgreSQL:

```bash
docker-compose up -d
```

PostgreSQL akan berjalan di port `5400`.

---

### 3. **Mengatur Variabel Lingkungan**
Buat file `.env` untuk menyimpan konfigurasi:

```env
POSTGRES_URL=postgresql://your_username:your_password@localhost:5400/your_database
STRIPE_SECRET_KEY=sk_test_***
STRIPE_WEBHOOK_SECRET=whsec_***
BASE_URL=http://localhost:3000
AUTH_SECRET=your_auth_secret
```

Pastikan mengganti `your_username`, `your_password`, dan `your_database` dengan kredensial yang sesuai.

---

### 4. **Akses PostgreSQL**
Anda dapat menggunakan klien PostgreSQL seperti `psql`, DBeaver, atau pgAdmin untuk mengakses database Anda.

#### Contoh Menggunakan `psql`:
```bash
docker exec -it postgres_container psql -U your_username -d your_database
```

---

### 5. **Gunakan URL PostgreSQL di Aplikasi**
Konfigurasi `POSTGRES_URL` dari file `.env` dapat digunakan oleh aplikasi Anda untuk menghubungkan ke PostgreSQL.

- Format:  
  ```plaintext
  postgresql://username:password@host:port/database
  ```
  Contoh:
  ```plaintext
  postgresql://your_username:your_password@localhost:5432/your_database
  ```
