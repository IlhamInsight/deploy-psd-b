# Panduan Deploy Server - Klasifikasi Suara Knalpot

Folder ini berisi file-file esensial yang sudah siap dideploy ke server (VPS / Cloud VM) tanpa membawa dataset audio mentah.

## Struktur File
```text
DEPLOY_SERVER/
├── templates/
│   └── index.html          # Antarmuka web dashboard & animasi portal
├── .dockerignore           # Daftar file yang diabaikan saat build
├── app.py                  # Backend Flask & API prediksi
├── docker-compose.yml      # Konfigurasi container Docker (Port 5001)
├── Dockerfile              # Spesifikasi image container (Python 3.10-slim + ffmpeg + Gunicorn)
├── model_knalpot.pkl       # Model Machine Learning siap pakai
├── requirements.txt        # Dependensi Python
└── README.md               # Panduan deploy
```

## Langkah Deploy ke Server

### 1. Upload Folder ke Server
Kompres folder ini menjadi `.zip` atau kirim langsung ke server via SCP/SFTP:
```bash
scp -r DEPLOY_SERVER user@ip-server-anda:/home/user/
```

### 2. Jalankan Aplikasi dengan Docker
Masuk ke direktori project di server dan jalankan:
```bash
cd DEPLOY_SERVER
docker compose up -d --build
```

### 3. Buka Port di Firewall Server
Pastikan port `5001` terbuka di firewall server Anda:
* **Ubuntu/Debian (UFW):**
  ```bash
  sudo ufw allow 5001/tcp
  sudo ufw reload
  ```
* **AWS / GCP / Cloud Provider:**
  Tambahkan Inbound Rule di Security Group / Firewall untuk Port `5001` (Custom TCP).

### 4. Akses Aplikasi
Buka browser dan akses melalui IP Publik server Anda:
```
http://<IP_SERVER>:5001
```

---

## Catatan Pengujian di Server Publik
- **Akses Mikrofon:** Browser modern (Chrome/Edge/Safari) secara default **memblokir akses mikrofon jika situs tidak menggunakan HTTPS**.
- Saat melakukan demo di server publik melalui HTTP (`http://<IP_SERVER>:5001`), gunakan fitur **"Pilih File Audio"** (Upload) yang ada di halaman web untuk menguji file audio knalpot.

