Berikut adalah langkah-langkah untuk menginstal Redis di Docker:

### 1. **Pastikan Docker Sudah Terinstal**
Pastikan Docker sudah terinstal dan berjalan di sistem Anda. Anda dapat memeriksa status Docker dengan menjalankan:
```bash
docker --version
```

### 2. **Menarik Redis Image dari Docker Hub**
Gunakan perintah berikut untuk menarik image Redis dari Docker Hub:
```bash
docker pull redis
```

### 3. **Menjalankan Redis Container**
Setelah image berhasil diunduh, jalankan container Redis dengan perintah berikut:
```bash
docker run --name redis-server -d redis
```

Penjelasan:
- `--name redis-server`: Memberi nama container sebagai `redis-server`.
- `-d`: Menjalankan container dalam mode detached (di background).

### 4. **Memverifikasi Redis Container**
Periksa apakah Redis container berjalan dengan baik:
```bash
docker ps
```

Anda akan melihat container `redis-server` di daftar container yang sedang berjalan.

### 5. **Menghubungkan ke Redis**
Untuk menghubungkan ke Redis, gunakan perintah berikut:
```bash
docker exec -it redis-server redis-cli
```

Setelah masuk ke `redis-cli`, Anda dapat menjalankan perintah Redis seperti:
```bash
PING
```
Jika Redis berfungsi dengan baik, balasan yang Anda dapatkan adalah `PONG`.

---

### 6. **(Opsional) Menjalankan Redis dengan Port dan Volume**
Jika Anda ingin memetakan port dan menyimpan data Redis di luar container, gunakan:
```bash
docker run --name redis-server -d -p 6379:6379 -v redis_data:/data redis
```

Penjelasan:
- `-p 6379:6379`: Memetakan port Redis (6379) di container ke port host.
- `-v redis_data:/data`: Menyimpan data Redis di volume bernama `redis_data`.


Untuk melakukan **load testing** Redis, Anda bisa menggunakan berbagai alat dan pendekatan untuk mengukur kinerja Redis di bawah beban. Salah satu alat yang umum digunakan untuk ini adalah **Redis-benchmark**, yang sudah tersedia di Redis dan dapat dijalankan di dalam container Redis Anda.

### Langkah-langkah untuk melakukan load test Redis:

#### 1. **Menggunakan `redis-benchmark` dari Docker**

Redis sudah dilengkapi dengan alat benchmark yang disebut `redis-benchmark`, yang dapat digunakan untuk melakukan load testing terhadap Redis.

Anda dapat menjalankan perintah benchmark langsung di container Redis dengan perintah berikut:

```bash
docker exec -it redis-server redis-benchmark
```

Perintah ini akan menjalankan serangkaian tes default untuk mengukur kinerja Redis pada operasi dasar seperti `SET`, `GET`, dan `INCR`.

#### 2. **Menentukan Jumlah Koneksi dan Iterasi**

Anda dapat menyesuaikan pengujian dengan berbagai parameter, seperti jumlah koneksi (`-c`), jumlah iterasi (`-n`), dan jenis operasi yang dilakukan.

Contoh perintah untuk menguji kinerja Redis dengan 100 koneksi dan 1 juta iterasi:

```bash
docker exec -it redis-server redis-benchmark -c 100 -n 1000000
```

Penjelasan:
- `-c 100`: Menggunakan 100 koneksi paralel.
- `-n 1000000`: Melakukan 1 juta operasi.
  
#### 3. **Menguji Jenis Perintah Redis Tertentu**

Jika Anda ingin menguji perintah tertentu, misalnya `SET` dan `GET`, Anda dapat menentukan perintah yang ingin diuji:

```bash
docker exec -it redis-server redis-benchmark -t set,get -c 100 -n 1000000
```

#### 4. **Menggunakan Redis-benchmark dengan Parameter Lain**

Berikut beberapa parameter yang bisa digunakan untuk mengubah beban pengujian:
- `-q`: Output hanya menunjukkan hasil rata-rata per operasi.
- `-P`: Membatasi jumlah pipeline yang digunakan dalam pengujian (default 1).
- `-d`: Menentukan ukuran data yang digunakan dalam setiap perintah (misalnya, `-d 1024` untuk 1 KB per perintah).

Contoh lengkap dengan parameter ini:

```bash
docker exec -it redis-server redis-benchmark -t set,get -c 100 -n 1000000 -d 1024 -P 16
```

#### 5. **Analisis Hasil Benchmark**
Setelah menjalankan benchmark, Anda akan mendapatkan output yang menunjukkan berapa banyak operasi yang dapat dilakukan per detik untuk masing-masing perintah yang diuji.

Misalnya, output benchmark bisa terlihat seperti ini:

```
SET: 1000000 requests completed in 1.23 seconds
GET: 1000000 requests completed in 0.45 seconds
```

Anda bisa memeriksa throughput (jumlah operasi per detik) untuk masing-masing operasi yang diuji.

#### 6. **Memantau Kinerja Sistem**
Selain menggunakan `redis-benchmark`, Anda bisa menggunakan alat monitoring lainnya seperti **`htop`**, **`docker stats`**, atau **`redis-cli info`** untuk memantau penggunaan sumber daya sistem (CPU, memori, dll.) saat pengujian beban berlangsung.

- **Docker stats**:
  ```bash
  docker stats redis-server
  ```

- **Redis CLI info**:
  ```bash
  docker exec -it redis-server redis-cli info
  ```