# 🛡️ Otomatisasi Hash SHA-256 dengan PowerShell

> **Laporan Praktikum & Repositori Alat Digital Evidence Handling**  
> Proyek ini dibuat untuk memenuhi tugas praktikum penanganan barang bukti digital menggunakan otomatisasi skrip Windows PowerShell dan visualisasi dasbor interaktif berbasis web.

---

## 👤 Identitas Praktikan
* **Nama:** Eva Riyanti Prasetia
* **NIM:** 24020260018
* **Laboratorium:** EVA Forensic Lab
* **Mata Kuliah:** Penanganan Barang Bukti Digital (Digital Evidence Handling)

---

## 📌 Tentang Proyek
Proyek ini mengintegrasikan kekuatan **skrip otomatisasi shell (PowerShell)** dengan **dasbor analitik modern (HTML & Vanilla CSS)** untuk merekam dan memverifikasi integritas barang bukti digital secara efisien. Dalam dunia digital forensik, menjaga keaslian bukti melalui nilai *cryptographic hash* adalah langkah krusial dalam prosedur *Chain of Custody*.

Dasbor interaktif yang disertakan memungkinkan investigator/dosen pemeriksa untuk mencari, menyalin hash, mengomparasikan metode, dan mengekspor kembali data hasil forensik secara instan.

---

## ⚡ Fitur Utama
1. **Otomatisasi Kalkulasi SHA-256:** Memproses seluruh berkas dalam direktori target sekaligus tanpa input manual.
2. **Ekspor CSV Terstandarisasi:** Menyimpan hasil pencatatan hash langsung ke format CSV yang kompatibel dengan alat audit forensik lainnya.
3. **Deteksi Sabotase & Verifikasi (Skrip Bonus):** Skrip verifikator yang secara otomatis membandingkan hash terekam dengan kondisi file saat ini untuk mendeteksi perubahan sekecil apa pun.
4. **Dasbor Interaktif & Modern:**
   * Desain premium dengan **Glassmorphism** dan dukungan tema gelap/terang.
   * Pencarian langsung (*real-time search*) pada data tabel bukti.
   * Tombol salin instan (*Copy to Clipboard*) untuk skrip dan hash.
   * Ekspor interaktif dasbor ke file CSV secara langsung.

---

## 📁 Struktur Repositori
* `index.html` — Dasbor utama laporan praktikum & visualisasi interaktif.
* `styles.css` — Sistem desain dasbor (responsif, tema dinamis, mikro-animasi).
* `README.md` — Dokumentasi proyek (berkas ini).

---

## 💻 Skrip PowerShell yang Digunakan

### 1. Skrip Hashing Utama
Digunakan untuk menavigasi ke folder barang bukti, menghitung tanda tangan digital SHA-256 dari seluruh file, dan mengekspor hasilnya ke laporan CSV.

```powershell
# Navigasi ke folder barang bukti
cd "C:\BarangBukti_Mahasiswa"

# Hitung hash SHA-256 semua file sekaligus
Get-ChildItem "C:\BarangBukti_Mahasiswa" -File | Get-FileHash -Algorithm SHA256

# Ekspor hasil ke file CSV
Get-ChildItem "C:\BarangBukti_Mahasiswa" -File | Get-FileHash -Algorithm SHA256 | Export-Csv -Path "hash_report.csv" -NoTypeInformation
```

### 2. Skrip Verifikator (Bonus)
Digunakan untuk mendeteksi integritas barang bukti secara berkala dengan membandingkan nilai hash saat ini dengan nilai hash awal yang tercatat dalam `hash_report.csv`.

```powershell
# Membaca laporan hash CSV yang ada
$recordedHashes = Import-Csv -Path "hash_report.csv"

# Melakukan verifikasi untuk setiap file
foreach ($row in $recordedHashes) {
    $currentHash = (Get-FileHash -Path $row.Path -Algorithm SHA256).Hash
    
    if ($currentHash -eq $row.Hash) {
        Write-Host "MATCH: Berkas [$(Split-Path $row.Path -Leaf)] utuh dan tidak dimodifikasi." -ForegroundColor Green
    } else {
        Write-Warning "MISMATCH: Berkas [$(Split-Path $row.Path -Leaf)] telah termodifikasi atau rusak!"
    }
}
```

---

## ⚙️ Petunjuk Penggunaan

### Menjalankan Skrip PowerShell
1. Buka Windows PowerShell sebagai Administrator.
2. Buat folder barang bukti di C: dengan nama `C:\BarangBukti_Mahasiswa` dan masukkan file bukti Anda ke dalamnya.
3. Jalankan skrip di atas untuk menghasilkan laporan `hash_report.csv`.
4. Untuk memverifikasi keaslian barang bukti di kemudian hari, jalankan Skrip Verifikator di direktori yang sama dengan berkas `hash_report.csv`.

### Menjalankan Dasbor Web
1. Cukup buka berkas `index.html` langsung di peramban (browser) web favorit Anda (Chrome, Edge, Firefox, dll).
2. Dasbor akan menampilkan ringkasan data, visualisasi alur, komparasi metode, serta memberikan opsi penyalinan skrip secara instan.

---

> [!NOTE]
> Proyek ini dirancang sebagai pemenuhan kebutuhan akademis praktikum kelas forensik dengan tetap mengadopsi standar industri best-practice seperti SHA-256 dan standarisasi ekspor laporan.