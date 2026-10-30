# CONCEPT BLUEPRINT: E-HEALTH PLATFORM
**Perancangan Konseptual Web Platform E-Health & Tele-Counseling Terpadu**  
*Dokumen Arsitektur Sistem (Versi 1.4: Professional Production & Expo Ready)*

---

## 1. Eksekutif Ringkasan & Gambaran Sistem

Platform Web E-Health & Tele-Counseling ini dirancang sebagai ekosistem digital terpadu untuk menghubungkan pasien, psikolog swasta, dan administrator lembaga dalam satu sistem layanan kesehatan mental yang aman, efisien, dan *data-driven*. 

Sistem ini mengintegrasikan fungsi pemantauan harian interaktif, penjadwalan terstruktur 4-minggu, ruang konseling *real-time*, hingga catatan klinis rahasia. Layanan konsultasi tersedia dalam **empat bentuk praktik**: **Chat**, **Audio Call**, **Video Call**, dan **Tatap Muka (*Face-to-Face*)**. Pembayaran konsultasi diproses melalui *Payment Gateway* terintegrasi serta opsi Transfer Bank Manual, yang dilengkapi Mode Pengujian / Simulasi yang dapat disetel via Portal Admin untuk keperluan demonstrasi Expo.

---

## 2. Pembagian Peran Pengguna (3-Portal Architecture)

### 1. Portal Admin *(System Control & Support)*
- **Monitoring & Audit Log:** Pemantauan kesehatan operasional sistem dasar dan pengelolaan log audit.
- **CMS Artikel:** Publikasi artikel edukasi kesehatan mental terakreditasi.
- **Manajemen Transaksi & Konfigurasi Mode:** Pengelolaan status transaksi serta sakelar konfigurasi Mode Simulasi / Testing untuk demonstrasi Expo.
- **Customer Support:** Menangani keluhan umum pengguna dasar (*customer support*).

### 2. Portal Psikolog *(Desktop-Ready)*
- **Akun & Akses Masuk Psikolog:** Akses pendaftaran jalur khusus psikolog untuk pembuatan akun dan *login*.
- **Matriks Penjadwalan:** Pengaturan ketersediaan slot jadwal konseling berbasis matriks 4-minggu.
- **Dashboard Pemantauan:** Analisis tren *mood* harian pasien (hanya jika pasien mengaktifkan opsi *Shared*).
- **Ruang Konseling Multi-Layanan:** Sesi interaktif *live* via *Chat*, *Audio Call*, dan *Video Call* berbasis web (WebRTC).
- **Private SOAP Notes:** Pengisian rekam medis terenkripsi dan terisolasi khusus psikolog (*Subjective, Objective, Assessment, Plan*).
- **Jadwal Tatap Muka:** Pengaturan lokasi dan jam praktik untuk sesi tatap muka (*face-to-face*).
- **Ringkasan Pendapatan:** Melihat riwayat sesi dan estimasi akumulasi pendapatan.
- **Perlindungan Akses Jurnal:** Sistem menampilkan peringatan edukatif privasi kepada pasien sebelum diizinkan membagikan jurnal pribadi ke psikolog untuk mencegah potensi penyalahgunaan.

### 3. Portal Pasien *(User-Friendly & Interactive)*
- **Akun & Akses Masuk Customer:** Pendaftaran portal khusus *customer* dan masuk menggunakan email, nomor HP, atau akun Google OAuth.
- **Reservasi Sesi Matriks 4-Minggu:** Pencarian psikolog dan pemesanan slot sesi (pilihan 1 sesi satuan atau paket kombo 4-minggu).
- **Modul Mood Tracker & Jurnal:** Pencatatan kondisi emosional harian dengan *toggle* privasi (*Private / Shared*) dan *tag* pemicu emosi otomatis (`#Pekerjaan`, `#Keluarga`, `#Keuangan`).
- **Tugas Terapi & PHR:** Modul penugasan instruksional personal (*Homework*) dan unggah berkas riwayat kesehatan (*Personal Health Record* - PDF/Gambar).
- **Bantuan Darurat (Emergency Safety Net):** Tombol manual SOS yang selalu terlihat di semua halaman portal pasien dan ruang *chat*.
- **Pilihan Layanan & Pembayaran:** Memilih 1 dari 4 bentuk layanan (*chat*, *audio call*, *video call*, atau tatap muka) lalu melakukan transaksi pembayaran (*Non-refundable*).

---

## 3. Diagram Topologi & Use Case Diagram (UC1 - UC7)

```text
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│                    BATAS SISTEM: PLATFORM WEB E-HEALTH & TELE-COUNSELING                        │
├──────────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                                  │
│   [ Pasien ] ──────┬──────> ( UC1: Booking Sesi 4 Modul & Matriks 4-Minggu ) [MANFAAT UTAMA]     │
│                    ├──────> ( UC2: Kelola Mood Tracker & Tag Pemicu Privasi )                   │
│                    ├──────> ( UC4: Transaksi Payment Gateway / Transfer Bank ) <─ [ Payment Gateway ]│
│                    ├──────> ( UC5: Bantuan SOS Manual & Kontak Darurat )                         │
│                    └──────> ( UC6: Sesi Konseling Live - Chat, Audio, Video, Tatap Muka )        │
│                                                                                                  │
│   [ Psikolog ] ────┬──────> ( UC1: Kelola Matriks Jadwal 4-Minggu )                             │
│                    ├──────> ( UC2: Lihat Mood Tracker Pasien [jika Dibagikan] )                 │
│                    ├──────> ( UC3: Input Private SOAP Notes [Rahasia] )                         │
│                    └──────> ( UC6: Sesi Konseling Live - Chat, Audio, Video, Tatap Muka )        │
│                                                                                                  │
│   [ Admin ] ───────┴──────> ( UC7: Monitoring Operasional & Setel Mode Simulasi Pembayaran )    │
│                                                                                                  │
└──────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Rincian Pemetaan Use Case:
- **UC1:** Booking Sesi 4 Modul & Matriks 4-Minggu `[MANFAAT UTAMA]` *(Pasien & Psikolog)*
- **UC2:** Kelola Mood Tracker & Tag Pemicu Privasi *(Pasien & Psikolog jika Shared)*
- **UC3:** Input Private SOAP Notes Rahasia *(Psikolog)*
- **UC4:** Transaksi Payment Gateway / Transfer Bank *(Pasien & Payment Gateway)*
- **UC5:** Bantuan SOS Manual & Kontak Darurat *(Pasien)*
- **UC6:** Sesi Konseling Live - Chat, Audio, Video, Tatap Muka *(Pasien & Psikolog)*
- **UC7:** Monitoring Operasional Sistem & Setel Mode Simulasi Pembayaran *(Admin)*

---

## 4. Detail Modul Utama & Fitur Unggulan

### A. Modul Jurnal & Mood Tracker
- **Input Multimodal:** Mengakomodasi entri teks serta Perekam Suara (*Voice Note*) untuk kemudahan ekspresi pasien.
- **Pemicu Emosi (*Trigger Tags*):** Muncul secara otomatis ketika pasien memilih skala *mood* (1–5) untuk menanyakan pemicu utama (misal: `#Pekerjaan`, `#Keluarga`, `#Keuangan`).
- **Privasi Dua Arah (*Toggle*):** Pasien mengontrol penuh jurnal miliknya dengan opsi *Private* (hanya untuk pasien) atau *Shared with Psychologist*. Jurnal adalah milik khusus pasien, sedangkan Psikolog memiliki lembar tersendiri yaitu *Private SOAP Notes*.
- **Tes Skrining Sederhana:** Tes singkat dari psikolog yang ditentukan per sesi (opsional/boleh tidak ada tergantung kebutuhan klinis).
- **Grafik Riwayat Mood:** Visualisasi grafik tren *mood* mingguan/bulanan bagi pasien.

### B. Penjadwalan Matriks 4-Minggu
- **Penetapan Slot Fleksibel:** Psikolog mengonfigurasi ketersediaan jadwal untuk 4 minggu ke depan.
- **Reservasi Terstruktur:** Pasien dapat memilih membeli 1 sesi tunggal (satuan) atau paket kombo 4 minggu dengan alur yang terstruktur.
- **Sinkronisasi Kalender:** Notifikasi web, surel, dan WhatsApp otomatis bagi pasien dan psikolog.
- **Jenis Layanan per Slot:** Setiap slot jadwal ditandai bentuk layanannya (*chat*, *audio call*, *video call*, atau tatap muka).
- **Aturan Pembatalan (Non-Refundable):** Tidak ada mekanisme pengembalian dana (*Non-Refundable*). Pasien yang membatalkan sesi tidak dapat menarik kembali pembayaran, baik untuk reservasi satuan maupun paket 4 minggu.

### C. Modul Tugas Terapi & PHR 
- **Tugas Terapi (*Homework*):** Penugasan instruksional yang dipersonalisasi oleh psikolog untuk masing-masing pasien (misal: *"Latihan pernapasan 4-7-8"*). Dilengkapi *checklist* penyelesaian harian.
- **Rekam Medis Pribadi (*PHR*):** Pasien dapat mengunggah berkas riwayat medis/lab (PDF/Gambar) yang diizinkan untuk ditinjau oleh psikolog pendamping.

### D. Pustaka Artikel Edukasi
- **Pustaka Artikel:** Artikel edukasi kesehatan mental terakreditasi disajikan melalui CMS Admin. (Modul Katalog Obat dihapus sepenuhnya karena platform diperuntukkan khusus bagi psikolog yang tidak berwenang meresepkan obat).

### E. Modul Konsultasi Multi-Layanan (4 Tipe Praktik)
- **Empat Bentuk Praktik:** *Chat real-time*, *Audio Call*, *Video Call*, dan **Tatap Muka** (di lokasi praktik psikolog).
- **Ruang Sesi Live:** Dilengkapi pengatur waktu sesi (*timer*), opsi beralih dari *chat* ke *audio/video call*, dan tombol SOS manual yang selalu terlihat.

### F. Modul Pembayaran Terintegrasi
- **Metode Pembayaran:** Mendukung QRIS, Virtual Account, E-Wallet (via Payment Gateway Midtrans/Xendit) serta opsi Transfer Bank Manual. Disertai fitur sakelar Admin untuk mengaktifkan Mode Simulasi / Sandbox saat demonstrasi.
- **Status Transaksi:** Otomatis diperbarui via integrasi gateway (`Pending`, `Paid`, `Failed`).
- **Skema Sesi:** Pembayaran dapat dilakukan per 1 sesi satuan atau paket kombo 4 minggu (*Non-Refundable*).

---

## 5. Fitur Keamanan & Privasi

- **Private SOAP Notes:** Setiap selesai sesi konseling, psikolog menginput lembar rekam medis berformat standar SOAP (*Subjective, Objective, Assessment, Plan*). Catatan ini diisolasi secara ketat dan **TIDAK DAPAT DIBACA** oleh pasien maupun admin demi menjaga etika profesi dan kerahasiaan medis.
- **Protokol Keselamatan Darurat (SOS Manual):** Pemindaian otomatis teks jurnal pribadi ditiadakan untuk menghormati privasi penuh pengguna (UU PDP). Bantuan krisis diakomodasi melalui **Tombol SOS Manual** yang selalu tersedia di setiap halaman portal pasien dan di dalam ruang *chat*.
- **Keamanan Teknis:** Enkripsi data saat transit (HTTPS) dan *at rest* (AES-256). Kontrol akses berbasis peran (*Role-Based Access Control*) serta log audit mendata setiap akses sensitif.

---

## 6. Logika Basis Data (*Flagging Matrix*)

| Tabel Database | Kolom Kunci (*Key Column*) | Tipe Data | Fungsi & Logika Operasional |
| :--- | :--- | :--- | :--- |
| `users` | `role` | `ENUM` | `Pasien`, `Psikolog`, `Admin`. Dasar kontrol akses. |
| `journals` | `is_shared` | `BOOLEAN` | `TRUE`: Tampil pada *dashboard* psikolog.<br/>`FALSE`: Terkunci hanya untuk pasien. |
| `journals` | `trigger_tags` | `ARRAY` | Label pemicu yang dipilih pengguna setelah memilih skor *mood*. |
| `journals` | `voice_note_url` | `TEXT` | Lokasi berkas rekaman suara (opsional). |
| `mood_entries` | `score` | `INT (1-5)` | Skor *mood* harian pasien. |
| `therapy_tasks` | `status` | `ENUM` | Status tugas personal: `Pending`, `Completed`. |
| `clinical_notes` | `psychologist_id` | `FOREIGN KEY` | Terkunci khusus ID psikolog bersangkutan (*Private SOAP Notes*). |
| `schedules` | `week_number` | `INT (1-4)` | Mengelompokkan ketersediaan waktu matriks 4-minggu. |
| `appointments` | `service_type` | `ENUM` | `Chat`, `Audio`, `Video`, `TatapMuka`. Menandai 4 tipe praktik. |
| `payments` | `status` | `ENUM` | `Pending`, `Paid`, `Failed`. Diperbarui via *callback/webhook* gateway. |
| `reviews` | `rating` | `INT (1-5)` | Ulasan pasien pasca-sesi. |
| `audit_logs` | `actor_id` | `FOREIGN KEY` | Jejak rekam akses data sensitif. |

---

## 7. Tahapan Perilisan (*Phasing*)

- **Fase 1 (Rilis Utama / Core Release):** Autentikasi portal terpisah, Matriks Penjadwalan 4-Minggu, Booking 4 Modul Layanan (Chat, Audio, Video, Tatap Muka), Payment Gateway & Transfer Bank (Non-refundable) dengan Sakelar Mode Pengujian Admin, Jurnal (Toggle Privacy & Trigger Tags), Private SOAP Notes, Tombol SOS Manual.
- **Fase 2 (Pengembangan Lanjutan):** Penugasan Terapi kustom, PHR, Perekam Suara (Voice Note), Tes Skrining Sesi.
- **Fase 3 (Ekspansi Skala Besar):** Aplikasi Mobile Native, Komunitas Pasien, Integrasi Asuransi Kesehatan/BPJS.

---

## 8. Persetujuan Final & Status VRD

Dokumen ini difokuskan sebagai rancangan arsitektur sistem *(Visual Requirement Document / VRD)* yang disetujui untuk pengembangan aplikasi web PABW serta sinkronisasi proyek MPTI. Platform khusus untuk Psikolog (tanpa resep obat/katalog obat) dan siap untuk demonstrasi Expo maupun penggunaan klien riil.
