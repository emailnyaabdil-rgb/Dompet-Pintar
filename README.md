# DompetPintar - Aplikasi Pencatat Keuangan Otomatis Android (Kotlin & Jetpack Compose)

Aplikasi Android Native (**Kotlin + Jetpack Compose + Room SQLite + Google ML Kit OCR + Gemini AI Vision + NotificationListenerService**) untuk mencatat pengeluaran dan pemasukan keuangan secara otomatis maupun manual.

---

## Fitur Utama

1. **Pencatatan dari Foto Struk / Nota (Hybrid OCR & AI)**
   - **Google ML Kit On-Device OCR (100% Gratis & Offline):** Memindai foto nota langsung di perangkat tanpa kuota internet, dilengkapi algoritma parser struk Indonesia ([`IndonesianReceiptParser`](app/src/main/java/id/aplikasi/dompetpintar/service/ocr/IndonesianReceiptParser.kt)) untuk mengenali nama toko (Indomaret, Alfamart, SPBU Pertamina, Restoran, Apotek, PLN, dll.), rincian item, tanggal, kategori, serta total bayar.
   - **Google Gemini AI Vision (Akurasi Tinggi per Item):** Opsi integrasi API Key Gemini ([`GeminiReceiptAnalyzer`](app/src/main/java/id/aplikasi/dompetpintar/service/ocr/GeminiReceiptAnalyzer.kt)) untuk mengekstrak nama toko, daftar barang beserta kuantitas & harga satuan, kategori, dan total secara cerdas.
   - **Mode Hybrid Otomatis ([`HybridReceiptScanner`](app/src/main/java/id/aplikasi/dompetpintar/service/ocr/HybridReceiptScanner.kt)):** Menggunakan Gemini AI saat API Key tersedia & terhubung ke internet, dan otomatis *fallback* ke ML Kit Offline jika tanpa internet atau tanpa API Key.

2. **Pencatatan Otomatis dari Notifikasi Transaksi Bank & E-Wallet**
   - Menggunakan [`FinanceNotificationListenerService`](app/src/main/java/id/aplikasi/dompetpintar/service/notification/FinanceNotificationListenerService.kt) & [`BankNotificationParser`](app/src/main/java/id/aplikasi/dompetpintar/service/notification/BankNotificationParser.kt).
   - Otomatis mendeteksi uang masuk (transfer/gaji) dan uang keluar (pembayaran QRIS, transfer, debit) dari notifikasi **BCA mobile, myBCA, Livin' by Mandiri, BRImo, BNI Mobile / wondr, Bank Jago, SeaBank, GoPay, OVO, DANA, dan ShopeePay**.
   - Dilengkapi tombol **Simulasi Notifikasi** di tab **Otomatis** untuk menguji fitur secara instan.

3. **Ringkasan Saldo, Grafik Bulanan, & Filter Kategori**
   - Kartu Total Saldo Gabungan dan ringkasan Pemasukan vs Pengeluaran bulanan di [`DashboardScreen`](app/src/main/java/id/aplikasi/dompetpintar/ui/screens/DashboardScreen.kt).
   - Grafik Batang tren 6 bulan terakhir & Grafik Donat distribusi pengeluaran per kategori di [`AnalyticsScreen`](app/src/main/java/id/aplikasi/dompetpintar/ui/screens/AnalyticsScreen.kt).

4. **Multi-Dompet / Rekening & Batas Anggaran (Budgeting)**
   - Kelola saldo terpisah untuk **Tunai**, **Rekening Bank**, dan **E-Wallet / QRIS** (serta tambah dompet baru) di [`WalletAndBudgetScreen`](app/src/main/java/id/aplikasi/dompetpintar/ui/screens/WalletAndBudgetScreen.kt).
   - Atur batas anggaran (*budget*) bulanan per kategori dengan indikator warna otomatis (Hijau = aman, Kuning = >75%, Merah = *over-budget*).

5. **Database Lokal (Offline-First) & Ekspor Laporan CSV / Excel**
   - Seluruh data tersimpan aman di perangkat menggunakan Room SQLite ([`AppDatabase`](app/src/main/java/id/aplikasi/dompetpintar/data/local/AppDatabase.kt)).
   - Ekspor laporan transaksi bulanan ke file `.csv` (kompatibel penuh dengan Microsoft Excel & Google Sheets) melalui [`CsvExporter`](app/src/main/java/id/aplikasi/dompetpintar/util/CsvExporter.kt).

---

## Cara Membuka & Build APK

### Opsi 1: Menggunakan Android Studio (Lokal)
1. Buka **Android Studio** (Ladybug / Koala atau versi terbaru).
2. Pilih **Open** lalu arahkan ke folder `d:\Aplikasi`.
3. Tunggu proses *Gradle Sync* selesai (Android Studio akan otomatis mengunduh dependensi).
4. Hubungkan HP Android (aktifkan *USB Debugging*) atau jalankan Emulator, lalu klik tombol **Run (▶)**, atau pilih menu **Build > Build Bundle(s) / APK(s) > Build APK(s)** untuk menghasilkan file `.apk`.

### Opsi 2: Build APK Otomatis via GitHub Actions (Tanpa Install Android Studio)
Proyek ini sudah dilengkapi konfigurasi [`.github/workflows/build-apk.yml`](.github/workflows/build-apk.yml):
1. Upload/push folder `d:\Aplikasi` ke repositori GitHub Anda.
2. Buka tab **Actions** di GitHub, jalankan workflow **Build Android APK**.
3. Unduh file `DompetPintar-debug-apk` dari bagian *Artifacts* dan install langsung di HP Android Anda.
