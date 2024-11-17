Untuk menginstal **Nginx** menggunakan Docker, Anda dapat mengikuti langkah-langkah berikut:

### 1. **Menarik Nginx Image dari Docker Hub**

Untuk memulai, tarik image resmi Nginx dari Docker Hub dengan perintah berikut:

```bash
docker pull nginx
```

Perintah ini akan mengunduh image Nginx terbaru.

### 2. **Menjalankan Nginx di Docker**

Setelah image berhasil diunduh, Anda dapat menjalankan container Nginx dengan perintah berikut:

```bash
docker run --name nginx-container -d -p 80:80 nginx
```

Penjelasan:
- `--name nginx-container`: Memberikan nama pada container sebagai `nginx-container`.
- `-d`: Menjalankan container di background (detached mode).
- `-p 80:80`: Memetakan port 80 di container ke port 80 di host, sehingga Nginx dapat diakses melalui `http://localhost` atau IP host.
- `nginx`: Menjalankan image Nginx.

### 3. **Verifikasi Nginx Berjalan**

Untuk memastikan Nginx berjalan dengan baik, buka browser dan arahkan ke `http://localhost`. Anda seharusnya melihat halaman default Nginx yang menampilkan pesan seperti "Welcome to nginx!".

Atau, Anda juga dapat memeriksa status container menggunakan perintah berikut:

```bash
docker ps
```

Ini akan menampilkan container yang sedang berjalan, termasuk Nginx.

### 4. **(Opsional) Menjalankan Nginx dengan Konfigurasi Kustom**

Jika Anda ingin menggunakan konfigurasi Nginx kustom atau melayani konten statis, Anda bisa memetakan volume atau file konfigurasi kustom menggunakan perintah berikut:

```bash
docker run --name nginx-container -d -p 80:80 -v /path/to/your/nginx.conf:/etc/nginx/nginx.conf -v /path/to/your/static/files:/usr/share/nginx/html nginx
```

Penjelasan:
- `-v /path/to/your/nginx.conf:/etc/nginx/nginx.conf`: Memetakan file konfigurasi Nginx kustom.
- `-v /path/to/your/static/files:/usr/share/nginx/html`: Memetakan direktori file statis Anda ke direktori root Nginx.

### 5. **Memeriksa Log Nginx**

Jika Anda ingin memeriksa log Nginx di container, Anda dapat menjalankan perintah:

```bash
docker logs nginx-container
```
Untuk melakukan **load test** terhadap Nginx, Anda bisa menggunakan beberapa alat yang dapat mengirimkan permintaan HTTP dalam jumlah besar untuk mengukur bagaimana Nginx menangani beban tinggi. Beberapa alat yang umum digunakan adalah **Apache Benchmark (`ab`)**, **wrk**, dan **JMeter**.

Berikut adalah cara-cara untuk melakukan load test pada Nginx menggunakan alat tersebut.

### 1. **Menggunakan Apache Benchmark (`ab`)**

`ab` adalah alat yang sederhana namun kuat untuk melakukan benchmark HTTP. Jika Anda sudah menginstal `ab` (biasanya sudah terpasang di sistem berbasis Unix), Anda dapat melakukan load test dengan cara berikut.

#### Langkah-langkah:

1. Pastikan Nginx sudah berjalan di Docker seperti yang dijelaskan sebelumnya.
2. Jalankan perintah `ab` untuk mengirimkan sejumlah permintaan HTTP ke server Nginx.

Contoh perintah untuk mengirimkan 1000 permintaan dengan 100 koneksi paralel:

```bash
ab -n 1000 -c 100 http://localhost/
```

Penjelasan:
- `-n 1000`: Jumlah total permintaan yang akan dikirimkan.
- `-c 100`: Jumlah koneksi paralel.
- `http://localhost/`: URL yang akan diuji.

Outputnya akan memberikan informasi seperti waktu rata-rata untuk menanggapi permintaan dan throughput server (permintaan per detik).

#### Hasil Benchmark:
Output yang Anda lihat akan menunjukkan metrik seperti:
- `Requests per second` (permintaan per detik)
- `Connection Times` (waktu koneksi)
- `Transfer Rate` (kecepatan transfer)

### 2. **Menggunakan `wrk`**

`wrk` adalah alat benchmark HTTP yang lebih canggih dan dapat menghasilkan beban yang lebih berat dibandingkan dengan `ab`.

#### Langkah-langkah:

1. Instal `wrk` jika belum terpasang. Di Ubuntu, Anda dapat menginstalnya dengan perintah:
   ```bash
   sudo apt-get install wrk
   ```

2. Jalankan `wrk` untuk melakukan benchmark pada server Nginx. Berikut contoh perintah untuk mengirimkan 1000 permintaan dengan 100 koneksi paralel dan waktu pengujian 30 detik:

   ```bash
   wrk -t4 -c100 -d30s http://localhost/
   ```

   Penjelasan:
   - `-t4`: Menentukan jumlah thread (di sini 4 thread).
   - `-c100`: Menentukan jumlah koneksi paralel (100 koneksi).
   - `-d30s`: Menentukan durasi pengujian (30 detik).
   - `http://localhost/`: URL yang akan diuji.

#### Output:
`wrk` akan memberikan output seperti:
```
Running 30s test @ http://localhost/
  4 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     1.39ms    2.04ms   56.04ms   92.34%
    Req/Sec     7.33k     1.92k    15.14k    66.79%
  877549 requests in 30.03s, 125.26MB read
Requests/sec: 29252.74
Transfer/sec:    4.17MB
```

Di sini, Anda akan mendapatkan statistik tentang throughput (permintaan per detik) dan latensi.

### 3. **Menggunakan JMeter**

**Apache JMeter** adalah alat pengujian beban yang lebih kompleks dan berbasis GUI, yang memungkinkan Anda untuk membuat pengujian berbasis skenario yang lebih mendalam.

#### Langkah-langkah:
1. **Instal JMeter**:
   Anda bisa mengunduh JMeter dari [situs resmi Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi).
   
2. **Buat Tes Baru**:
   - Setelah JMeter diinstal, buka aplikasi dan buat tes baru.
   - Tambahkan **Thread Group** di dalam **Test Plan** untuk mengatur jumlah pengguna virtual.
   - Tambahkan **HTTP Request** untuk menyimulasikan permintaan ke server Nginx.
   - Anda bisa mengatur jumlah permintaan, jumlah pengguna (threads), dan parameter lainnya sesuai dengan kebutuhan tes.

3. **Jalankan Tes**:
   Klik tombol **Start** untuk menjalankan tes dan lihat hasilnya di **View Results**.

JMeter memberikan lebih banyak kontrol terhadap pengujian dan menghasilkan grafik serta statistik yang lebih mendalam.

### 4. **Memantau Kinerja Nginx selama Load Test**

Selama tes beban, Anda juga harus memantau kinerja Nginx menggunakan alat seperti:

- **Docker stats** untuk memantau penggunaan sumber daya container.
  ```bash
  docker stats nginx-container
  ```
- **Redis CLI** atau **Nginx log** untuk memeriksa error atau masalah performa.

### Kesimpulan

- **Apache Benchmark (`ab`)**: Mudah digunakan, cocok untuk pengujian sederhana.
- **wrk**: Lebih kuat dan fleksibel untuk pengujian beban tinggi.
- **JMeter**: Cocok untuk pengujian kompleks dengan banyak skenario dan laporan visual.



Pada **Ubuntu** (dan distribusi Linux lainnya), `host.docker.internal` tidak akan berfungsi langsung seperti pada **macOS** atau **Windows**. Sebagai alternatif, Anda dapat menggunakan **alamat IP lokal** atau metode lain untuk mengonfigurasi **Nginx** di Docker agar dapat mengakses aplikasi **Next.js** yang berjalan di host.

Berikut adalah beberapa metode yang bisa Anda gunakan untuk mengatasi masalah ini:

### 1. **Menggunakan `localhost` atau `127.0.0.1`**

Jika aplikasi **Next.js** Anda berjalan di mesin host di `localhost:3000`, Anda dapat mencoba mengganti `host.docker.internal` dengan `localhost` atau `127.0.0.1`.

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://localhost:3000;  # Mengarahkan ke Next.js yang berjalan di localhost:3000
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Namun, cara ini mungkin **tidak berfungsi** jika Anda menjalankan **Next.js** di host dan **Nginx di container Docker**, karena `localhost` di dalam container mengarah ke container itu sendiri.

### 2. **Menggunakan IP Host Local**

Jika Anda ingin agar Nginx di Docker dapat mengakses aplikasi Next.js yang berjalan di host Ubuntu, Anda bisa menggunakan **IP lokal** dari host Ubuntu. Anda bisa mendapatkan alamat IP lokal host dengan perintah berikut:

```bash
ip addr show
```

Cari alamat IP di bagian `inet` yang terkait dengan interface jaringan Anda, misalnya `192.168.x.x`.

Kemudian, ganti `localhost` atau `host.docker.internal` dengan alamat IP lokal tersebut dalam konfigurasi Nginx:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://192.168.x.x:3000;  # Gunakan alamat IP lokal host Ubuntu
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 3. **Menggunakan `docker0` Bridge Network**

Jika Anda ingin menggunakan jaringan Docker dan mengakses host menggunakan **jaringan bridge**, Anda bisa menggunakan alamat IP **`docker0`**. Untuk mengetahui alamat IP **docker0**, Anda dapat menjalankan:

```bash
ip addr show docker0
```

Alamat IP yang muncul di bagian `inet` biasanya adalah `172.17.x.x`. Gunakan alamat ini dalam konfigurasi Nginx:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://172.17.x.x:3000;  # Gunakan IP docker0
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 4. **Menggunakan Nama Container (Jika Next.js Berjalan di Container Docker Lain)**

Jika aplikasi **Next.js** Anda berjalan dalam container Docker yang sama atau di jaringan Docker yang sama dengan Nginx, Anda bisa menggunakan nama container **Next.js** alih-alih `localhost`. Misalnya, jika nama container untuk Next.js adalah `nextjs-container`, Anda bisa menggunakan:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://nextjs-container:3000;  # Nama container Next.js
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 5. **Menggunakan Host DNS untuk Container Docker**

Jika aplikasi Next.js Anda dijalankan di container Docker, dan Anda ingin Nginx di container Docker mengaksesnya, Anda bisa mengonfigurasi Docker untuk menggunakan DNS atau **jaringan bridge** sehingga nama container dapat digunakan langsung.

---

### Menggunakan `docker-compose` (Opsional)

Jika Anda menggunakan **`docker-compose`**, Anda bisa membuat dua service (Nginx dan Next.js) yang berada di jaringan yang sama dan saling berkomunikasi menggunakan nama container. Misalnya, dalam file `docker-compose.yml`:

```yaml
version: '3'
services:
  nextjs:
    image: your-nextjs-image
    container_name: nextjs-container
    ports:
      - "3000:3000"
    networks:
      - mynetwork

  nginx:
    image: nginx
    container_name: nginx-container
    ports:
      - "80:80"
    volumes:
      - ./nginx-config:/etc/nginx/conf.d
    networks:
      - mynetwork

networks:
  mynetwork:
    driver: bridge
```

Dengan konfigurasi ini, Anda dapat menggunakan nama container `nextjs-container` di dalam file konfigurasi Nginx.

### Kesimpulan

- **Untuk Host Lokal**: Gunakan alamat IP lokal (`192.168.x.x`) atau `localhost` jika aplikasi Next.js berjalan di mesin lokal dan Anda ingin mengaksesnya dari Nginx di Docker.
- **Jika Next.js di Container Docker**: Gunakan nama container atau alamat IP jaringan Docker (`docker0`) untuk menghubungkan Nginx dengan Next.js.


Jika Anda menghadapi masalah dengan **firewall** yang memblokir akses ke aplikasi Next.js yang berjalan di `192.168.x.x:3000` pada Ubuntu, Anda perlu mengonfigurasi firewall untuk mengizinkan lalu lintas masuk ke port tersebut. Di Ubuntu, firewall yang paling umum digunakan adalah **UFW (Uncomplicated Firewall)**.

Berikut adalah langkah-langkah untuk mengonfigurasi UFW agar mengizinkan lalu lintas ke port `3000`:

### 1. **Periksa Status UFW**

Periksa apakah UFW aktif dengan menjalankan perintah berikut:

```bash
sudo ufw status
```

Jika firewall aktif, Anda akan melihat statusnya dan aturan yang diterapkan. Jika tidak aktif, Anda akan melihat pesan yang mengatakan "inactive".

### 2. **Izinkan Lalu Lintas ke Port 3000**

Untuk membuka akses ke port `3000` (tempat aplikasi Next.js Anda berjalan), jalankan perintah berikut:

```bash
sudo ufw allow 3000
```

Ini akan mengizinkan lalu lintas masuk ke port `3000`.

### 3. **Periksa Status Firewall Lagi**

Setelah menambahkan aturan firewall, periksa status firewall untuk memastikan aturan tersebut sudah diterapkan:

```bash
sudo ufw status
```

Anda harus melihat aturan baru yang mengizinkan lalu lintas ke port `3000`.

### 4. **Jika UFW Tidak Digunakan (Firewall Lainnya)**

Jika Anda tidak menggunakan **UFW**, tetapi menggunakan **iptables** atau **firewalld**, Anda perlu menambahkan aturan untuk mengizinkan akses ke port `3000`. Berikut adalah cara untuk mengizinkan akses menggunakan **iptables**:

```bash
sudo iptables -A INPUT -p tcp --dport 3000 -j ACCEPT
```

Jika menggunakan **firewalld** (terutama pada distribusi Fedora/CentOS/RHEL), Anda bisa menambahkan aturan dengan:

```bash
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
```

### 5. **Mengizinkan Semua Lalu Lintas dari IP Tertentu (Opsional)**

Jika Anda ingin hanya mengizinkan akses ke port `3000` dari **alamat IP tertentu** (misalnya, `192.168.1.100`), Anda dapat menambahkan aturan khusus:

```bash
sudo ufw allow from 192.168.1.100 to any port 3000
```

Ini akan membatasi akses ke port `3000` hanya dari IP `192.168.1.100`.

### 6. **Restart UFW (Jika Diperlukan)**

Jika Anda membuat perubahan besar pada pengaturan UFW, restart firewall untuk memastikan aturan diterapkan dengan benar:

```bash
sudo ufw reload
```

### 7. **Pastikan Aplikasi Next.js Dapat Diakses**

Setelah mengizinkan akses ke port 3000, coba akses aplikasi Next.js Anda melalui browser atau alat lain di alamat `http://192.168.x.x:3000`. Jika Anda mengaksesnya dari mesin lain di jaringan yang sama, pastikan tidak ada pembatasan lain (seperti router atau konfigurasi NAT) yang memblokir akses.

### 8. **Mengizinkan Akses ke Port Lain (Jika Diperlukan)**

Jika Anda juga ingin mengizinkan lalu lintas ke port lain, seperti untuk **Nginx** (port `80`), Anda dapat menambahkan aturan serupa:

```bash
sudo ufw allow 80/tcp
```

Ini akan membuka akses ke port `80` untuk aplikasi web yang berjalan di Nginx.

---

Dengan mengikuti langkah-langkah di atas, Anda dapat memastikan bahwa aplikasi Next.js di `192.168.x.x:3000` dapat diakses dari mesin lain di jaringan Anda, meskipun firewall diaktifkan.