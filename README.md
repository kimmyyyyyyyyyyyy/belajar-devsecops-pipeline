# 🛡️ DevSecOps CI/CD Pipeline Implementation

Proyek implementasi pipeline CI/CD otomatis berbasis prinsip **Shift-Left Security** menggunakan GitHub Actions, Semgrep SAST, Pylint Code Quality Gate, pip-audit (SCA), dan deployment otomatis ke VirtualBox VM (Docker).

![DevSecOps Pipeline](https://github.com/kimmyyyyyyyyyyyy/belajar-devsecops-pipeline/actions/workflows/devsecops.yml/badge.svg)

---

## 📌 Alur Pipeline DevSecOps

Pipeline ini mengimplementasikan tahapan otomatisasi keamanan dan deployment:
1. **SAST Scan (Semgrep)**: Memindai celah keamanan kode secara statis (seperti SQL Injection, XSS, dan miskonfigurasi) sebelum kode masuk tahap build.
2. **Code Quality Gate (Pylint)**: Memeriksa kepatuhan penulisan kode sesuai standar PEP 8 dengan ambang batas minimal skor **9.0 / 10.0**.
3. **Dependency Security Scan (pip-audit)**: Memeriksa database CVE publik untuk memastikan tidak ada library Python yang rentan.
4. **Automated CD (Self-Hosted Runner)**: Jika seluruh pemeriksaan lolos dan kode di-merge ke branch `main`, runner di Ubuntu VM akan otomatis mem-build image Docker dan me-restart container `web-flask`.

---

## 🚀 Endpoint Aplikasi

Aplikasi dibangun menggunakan Flask dan menyediakan beberapa endpoint:
* `GET /`: Halaman antarmuka login aman.
* `POST /`: Proses autentikasi pengguna menggunakan parameterized query (aman dari SQLi).
* `GET /about`: Halaman informasi versi aplikasi (hasil penambahan route pada tugas CI/CD).
* `GET /health`: Endpoint healthcheck untuk memantau status keaktifan aplikasi.

---

## 🧪 Riwayat Simulasi Pengujian Keamanan

| Skenario Pengujian | Hasil Pipeline | Keterangan |
| :--- | :---: | :--- |
| **Simulasi Broken Build** (Kode Rentan SQLi) | ❌ **Ditolak (Blocked)** | Semgrep mendeteksi celah SQL Injection & Pylint gagal (skor < 9.0). Deploy dibatalkan otomatis. |
| **Remediasi Kode** (Parameterized Query + PEP 8) | ✅ **Lolos (Passed)** | Skor Pylint 10/10, Semgrep bebas celah, container berhasil di-deploy ke VM. |
| **Branch Protection & Feature Route** | ✅ **Lolos & Ter-deploy** | Route `/about` dan `/health` di-merge via Pull Request dan otomatis aktif di server. |