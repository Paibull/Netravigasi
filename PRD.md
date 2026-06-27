# Product Requirements Document
# KampusGuide Access
**Asisten Navigasi Suara untuk Tunanetra & Low Vision — Kampus ITS Surabaya**

---

## 1. Latar Belakang

Mahasiswa penyandang tunanetra atau low vision menghadapi hambatan signifikan saat berorientasi di kampus ITS Surabaya yang luas. Sebagian besar sistem navigasi digital mensyaratkan kemampuan membaca layar secara visual, yang menjadikannya tidak aksesibel bagi kelompok ini. KampusGuide Access hadir sebagai asisten navigasi berbasis suara yang sepenuhnya dapat dioperasikan tanpa melihat layar.

---

## 2. Target Pengguna

| Segmen | Deskripsi |
|---|---|
| **Tunanetra total** | Mahasiswa/civitas ITS yang tidak dapat melihat; mengandalkan TTS dan haptic sepenuhnya |
| **Low vision** | Pengguna dengan penglihatan sebagian; kontras tinggi dan teks besar membantu orientasi |
| **Pengguna baru kampus** | Maba atau tamu yang belum hafal tata letak gedung ITS |

---

## 3. Problem Statement

> *"Bagaimana mahasiswa tunanetra dan low vision dapat menemukan gedung tujuan dan meminta bantuan darurat di kampus ITS secara mandiri, hanya dengan mengandalkan sentuhan dan suara?"*

### Pain points yang diidentifikasi:
- Tidak ada navigasi suara khusus untuk kampus ITS
- Aplikasi peta umum (Google Maps) tidak memberikan umpan balik audio kontekstual yang cukup
- Tidak ada jalur cepat untuk memanggil petugas keamanan kampus saat darurat
- UI konvensional mengharuskan interaksi presisi visual (tap kecil, ikon kecil)

---

## 4. Tujuan Produk

1. Pengguna dapat mencari dan memulai rute ke gedung tujuan hanya menggunakan suara
2. Pengguna dapat memilih lokasi tujuan dari daftar favorit menggunakan swipe dan ketuk ganda tanpa melihat layar
3. Pengguna dapat memanggil petugas keamanan kampus (SKK ITS) dalam dua ketukan
4. Aplikasi dapat digunakan secara offline setelah pertama kali diakses
5. Setiap aksi memberikan konfirmasi audio (TTS) dan taktil (haptic)

---

## 5. Lingkup Produk

### Dalam Lingkup (In Scope)
- PWA single-file, bisa diinstal di homescreen Android/iOS
- Navigasi suara turn-by-turn (simulasi/demo steps)
- Voice input untuk pencarian gedung
- Daftar 13 lokasi favorit kampus ITS
- Tombol SOS dengan auto-kirim koordinat GPS
- Umpan balik audio (TTS Bahasa Indonesia) di setiap transisi layar
- Umpan balik haptic (pola vibrate) per jenis instruksi
- Offline mode via Service Worker

### Luar Lingkup (Out of Scope)
- Peta visual interaktif
- Navigasi GPS real-time berbasis koordinat aktual
- Login / akun pengguna
- Riwayat perjalanan
- Backend server

---

## 6. Fitur & Kebutuhan Fungsional

### F1 — Input Suara (Halaman Tengah / Default)

| ID | Kebutuhan |
|---|---|
| F1.1 | Pengguna menekan tombol mikrofon dua kali untuk memulai perekaman suara |
| F1.2 | Sistem mengaktifkan `SpeechRecognition` dengan bahasa `id-ID` |
| F1.3 | Input suara dibandingkan dengan keyword (`kw`) tiap lokasi di `PLACES` array |
| F1.4 | Jika cocok, langsung masuk ke panduan navigasi lokasi tersebut |
| F1.5 | Jika tidak cocok dalam 9 detik (`LISTEN_TIMEOUT`), otomatis buka Favorit |
| F1.6 | Status mendengarkan dikonfirmasi via TTS: *"Mendengarkan. Sebutkan nama gedung tujuan."* |

### F2 — Navigasi Suara Turn-by-Turn (Panduan)

| ID | Kebutuhan |
|---|---|
| F2.1 | Setelah lokasi dipilih, overlay panduan terbuka dengan rute aktif |
| F2.2 | Setiap langkah menampilkan panah arah, instruksi teks besar, dan peringatan hambatan |
| F2.3 | Instruksi dibacakan via TTS secara otomatis per langkah |
| F2.4 | Langkah berikutnya diputar otomatis 1,5 detik setelah TTS selesai |
| F2.5 | Ketuk satu kali area tengah untuk mengulang instruksi langkah saat ini |
| F2.6 | Ketuk ganda tombol "Batalkan" untuk keluar dari rute |
| F2.7 | Jika langkah memiliki hambatan (tangga, dll.), teks peringatan kuning ditampilkan dan dibacakan |

### F3 — Haptic Feedback

| Pola | Konteks |
|---|---|
| `[15]` | Single tap / konfirmasi |
| `[200]` | Instruksi lurus |
| `[200,100,200]` | Belok kanan |
| `[600]` | Kembali / gagal |
| `[300,150,300,150,300]` | Tiba di tujuan / SOS aktif |

### F4 — Daftar Lokasi Favorit (Halaman Kiri)

| ID | Kebutuhan |
|---|---|
| F4.1 | Menampilkan 13 lokasi ITS sebagai kartu vertikal full-screen |
| F4.2 | Swipe atas/bawah untuk berpindah kartu; scroll-snap per kartu |
| F4.3 | Setiap kartu menampilkan: nama lokasi, jarak dari Asrama, nomor urut |
| F4.4 | Saat kartu settle setelah swipe, TTS otomatis membacakan nama dan jarak |
| F4.5 | Ketuk satu kali: membacakan ulang nama dan instruksi kartu |
| F4.6 | Ketuk dua kali: memulai panduan navigasi ke lokasi tersebut |

**Daftar 13 lokasi (acuan jarak dari Asrama Mahasiswa):**

| No | Nama Lokasi | Jarak |
|---|---|---|
| 1 | Asrama Mahasiswa | ± 10 m |
| 2 | Gedung Teknik Informatika | ± 1.8 km |
| 3 | Tower 1 (Menara MIPA) | ± 400 m |
| 4 | Tower 2 | ± 450 m |
| 5 | Medical Center ITS | ± 520 m |
| 6 | Perpustakaan ITS | ± 560 m |
| 7 | Kantin Pusat | ± 600 m |
| 8 | CCWS | ± 640 m |
| 9 | Masjid Manarul Ilmi | ± 680 m |
| 10 | Gedung Robotika | ± 740 m |
| 11 | Gedung Rektorat | ± 830 m |
| 12 | Graha Sepuluh Nopember | ± 1.0 km |
| 13 | Plaza Dr. Angka | ± 1.1 km |

### F5 — SOS / Panggilan Darurat (Halaman Kanan)

| ID | Kebutuhan |
|---|---|
| F5.1 | Halaman SOS dapat diakses dengan satu kali geser kanan dari halaman utama |
| F5.2 | Tombol SOS memerlukan ketuk ganda untuk mencegah panggilan tidak disengaja |
| F5.3 | Saat diaktifkan, overlay memanggil muncul dan TTS memberitahu status |
| F5.4 | Koordinat GPS dikirim otomatis; jika gagal, teks fallback tampil: *"Lokasi GPS tidak tersedia. Sebutkan posisi Anda saat ditelepon."* |
| F5.5 | Tombol "Telepon SKK ITS sekarang" sebagai `<a href="tel:+62315994251">` untuk panggilan langsung |
| F5.6 | Pola haptic berulang setiap 1,2 detik (7 kali) sebagai alert taktil |

---

## 7. Kebutuhan Non-Fungsional

### Aksesibilitas
- Semua elemen interaktif memiliki `aria-label` deskriptif
- Tidak ada aksi yang bergantung pada warna semata
- Mendukung `prefers-reduced-motion`: semua transisi dan animasi dimatikan
- Live region (`aria-live="assertive"`) sinkron dengan setiap output TTS
- Target sentuh minimal 44×44 px (tombol mic dan SOS menempati >50% lebar layar)
- Pola double-tap mencegah aktivasi tidak sengaja (window 460 ms)

### Performa
- First load < 3 detik pada 4G
- Semua aset (HTML, manifest, ikon) di-cache via Service Worker setelah load pertama
- Navigasi offline penuh setelah install PWA

### Kompatibilitas
- Target utama: Chrome/Android (support `SpeechRecognition` + `vibrate`)
- Fallback: iOS Safari (TTS berfungsi, haptic tidak tersedia, voice input terbatas)
- Responsive 320 px–440 px lebar; tablet ditampilkan dengan border radius dan shadow

### Ketersediaan
- Tidak ada ketergantungan server; seluruh logika di client-side
- Service Worker `kampusguide-v7` dengan strategi network-first untuk HTML, cache-first untuk aset statis

---

## 8. Desain Interaksi

### Gestur Utama

| Gestur | Efek |
|---|---|
| Geser kiri | Pindah ke halaman Favorit |
| Geser kanan | Pindah ke halaman SOS |
| Swipe atas/bawah (di Favorit) | Pindah antar kartu lokasi |
| Ketuk satu kali | Membacakan elemen yang disentuh |
| Ketuk dua kali (< 460 ms) | Mengaktifkan aksi utama elemen |

### Umpan Balik Audio Kontekstual

| Momen | Isi TTS |
|---|---|
| Buka aplikasi | *"Halaman pencarian dengan suara. Ketuk dua kali lingkaran mikrofon untuk mulai."* |
| Geser ke Favorit | *"Halaman lokasi favorit. Geser kanan untuk kembali ke pencarian suara."* |
| Swipe kartu Favorit | *"[Nama lokasi]. Jarak [jarak]."* |
| Geser ke SOS | *"Halaman darurat. Ketuk dua kali untuk menghubungi keamanan kampus."* |
| SOS diaktifkan | *"Menghubungi Pusat Keamanan Kampus ITS. Mengirim lokasi Anda."* |
| Voice input gagal | *"[Pesan]. Ketuk daftar favorit untuk memilih manual."* |

### Prinsip Desain Visual
- **High contrast**: latar hitam (`#000`), teks putih (`#fff`), aksen kuning untuk jarak (`#ff0`)
- **Hierarki ukuran**: tombol utama mendominasi layar (min 58vw diameter)
- **Muscle memory**: posisi vertikal tombol mic dan SOS identik di halaman masing-masing
- **Tidak ada label navigasi bawah**: hanya indikator dot untuk mengurangi kognitif load

---

## 9. Arsitektur Teknis

```
index.html          ← Single-file PWA (HTML + CSS + JS)
sw.js               ← Service Worker (network-first HTML, cache-first assets)
manifest.webmanifest← PWA manifest (installable, standalone, theme hitam)
icon.svg            ← App icon
```

### Stack Teknologi

| Komponen | Teknologi |
|---|---|
| Markup | HTML5 (semantic, ARIA) |
| Gaya | CSS3 (custom properties, clamp, scroll-snap, `@media prefers-reduced-motion`) |
| Logika | Vanilla JavaScript ES2020 (IIFE, tidak ada framework) |
| TTS | Web Speech API — `SpeechSynthesis`, bahasa `id-ID` |
| Voice Input | Web Speech API — `SpeechRecognition` / `webkitSpeechRecognition` |
| Haptic | `navigator.vibrate()` |
| GPS | `navigator.geolocation.getCurrentPosition()` + `watchPosition()` |
| Baterai | `navigator.getBattery()` |
| Offline | Service Worker + Cache API |
| Instalasi | PWA Web App Manifest |

---

## 10. Alur Pengguna Utama

### Alur 1: Cari Lokasi via Suara
```
[Buka App] → [Halaman Input Suara]
  → [Ketuk 2x Mikrofon]
  → [Overlay Mendengarkan terbuka + TTS konfirmasi]
  → [Ucapkan nama gedung]
  → [Cocok] → [Panduan Navigasi]
  → [Tidak cocok] → [Fallback: buka Favorit otomatis]
```

### Alur 2: Pilih Lokasi via Favorit
```
[Halaman Input Suara] → [Geser kiri]
  → [Halaman Favorit: kartu pertama announce otomatis]
  → [Swipe atas/bawah untuk browse + auto-announce tiap kartu]
  → [Ketuk 2x kartu lokasi tujuan]
  → [Panduan Navigasi]
```

### Alur 3: Panduan Navigasi
```
[Lokasi dipilih] → [Overlay Panduan terbuka]
  → [TTS: instruksi langkah 1 + haptic]
  → [Jeda 1,5 detik] → [Langkah 2 otomatis]
  → [Ketuk 1x kapan saja: ulangi instruksi]
  → [Tiba] → [TTS: "Anda tiba di [tujuan]" + haptic arrive]
  → [Ketuk 2x Batalkan: kembali ke Input Suara]
```

### Alur 4: SOS Darurat
```
[Halaman Input Suara] → [Geser kanan]
  → [Halaman SOS]
  → [Ketuk 2x tombol SOS]
  → [Overlay Memanggil: TTS + haptic berulang]
  → [GPS terkirim / fallback teks]
  → [Ketuk "Telepon SKK ITS": panggilan langsung ke +62315994251]
```

---

## 11. Batasan & Asumsi

- Navigasi turn-by-turn menggunakan **langkah demo statis** (bukan rute GPS real-time); implementasi rute nyata membutuhkan data jalur pejalan kaki kampus
- Jarak ke tiap lokasi merupakan **estimasi** dari Asrama Mahasiswa; koordinat akurat dapat diintegrasikan dengan haversine formula di iterasi berikutnya
- `SpeechRecognition` hanya tersedia di browser berbasis Chromium; pengguna Safari/Firefox tidak mendapatkan fitur voice input (fallback ke Favorit)
- `navigator.vibrate()` tidak tersedia di iOS; pengguna iPhone tidak mendapat haptic feedback
- Nomor telepon SKK ITS (`+62315994251`) di-hardcode; perlu diperbarui jika nomor berubah

---

## 12. Metrik Keberhasilan

| Metrik | Target |
|---|---|
| Waktu dari buka app hingga mulai navigasi | < 5 detik |
| Tingkat keberhasilan voice recognition (offline lab test) | ≥ 80% |
| Operasional tanpa koneksi internet | 100% setelah install |
| Semua aksi utama dapat dilakukan tanpa melihat layar | 100% |
| False-positive SOS (panggilan tidak disengaja) | 0 (dijaga oleh double-tap) |

---

*Dokumen ini mencerminkan state implementasi per Juni 2026. Dibuat untuk keperluan laporan Interaksi Manusia Komputer (IMK) — Institut Teknologi Sepuluh Nopember Surabaya.*
