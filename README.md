# Lenovo B360 Hackintosh (macOS Sequoia) - Golden Build 🍎

Repositari ini berisi folder EFI lengkap dan sudah dioptimasi untuk menjalankan macOS Sequoia pada PC Desktop OEM Lenovo dengan prosesor Intel Core i5-8400T (Coffee Lake). 

## 💻 Spesifikasi Hardware
- **PC:** Lenovo Desktop (OEM)
- **Motherboard:** Chipset B360
- **Prosesor:** Intel Core i5-8400T (Generasi ke-8)
- **Grafis Terintegrasi:** Intel UHD Graphics 630
- **Audio:** Realtek ALC233
- **Penyimpanan:** Lexar 128GB SSD SATA
- **Sistem Operasi:** macOS Sequoia 15.x
- **Bootloader:** OpenCore
- **SMBIOS:** iMac19,1

## ✨ Fitur yang Berjalan (100% Working)
- ✅ **QE/CI Graphics Acceleration:** UHD 630 berjalan native. Apple Graphics Power Management (AGPM) aktif untuk kontrol frekuensi dinamis (VRAM: Framebuffer 9MB, Stolen 19MB untuk *bypass limit* 32MB DVMT BIOS Lenovo).
- ✅ **CPU Power Management (SpeedStep):** Sangat stabil via `SSDT-PLUG` + **`CPUFriend` kustom**. Profil telah dikonfigurasi khusus untuk TDP 35W (Low Frequency Mode 800MHz, EPP: Balanced Performance) agar Turbo Boost maksimal tanpa *overheating*.
- ✅ **Sleep / Wake:** Berjalan sempurna (Sleep to RAM murni) tanpa *Kernel Panic* atau *Random Wake*.
- ✅ **USB Mapping:** Bebas limit 15-port via `USBToolBox` + `UTBMap.kext`. Menggunakan konfigurasi 6 port persis sesuai hardware PC (2 Depan, 4 Belakang).
- ✅ **Apple Services (iCloud, App Store, iMessage):** Kartu LAN Ethernet (Intel I219-V) menggunakan `IntelMausi.kext` terbaru dari Acidanthera (anti *Kernel Panic* di Sequoia).
- ✅ **NVRAM:** Native NVRAM bekerja baik via `SSDT-PMC`.
- ✅ **Ketahanan BIOS:** `SSDT-RTCAWAC` + *DisableRtcChecksum* aktif untuk mencegah *BIOS Reset Error 0199* khas PC OEM Lenovo.
- ✅ **Audio:** Realtek ALC233 berfungsi sempurna dengan boot-arg `alcid=11`.
- ✅ **Aesthetic Boot:** Mode Verbose (`-v`) telah dimatikan untuk tampilan boot Logo Apple yang elegan dan bersih.

---

## 🛠️ Post-Install Optimizations (Catatan Penting)
Jika Anda melakukan instalasi ulang menggunakan EFI ini di masa mendatang, **wajib** melakukan langkah-langkah optimasi via Terminal di bawah ini agar sistem berjalan sempurna:

### 1. Perbaikan Sleep / Wake (Wajib)
Secara bawaan, macOS menggunakan konfigurasi daya yang sering menyebabkan Hackintosh Desktop terbangun sendiri (Random Wake) atau error saat *Sleep* (karena mencoba menulis RAM ke disk). Jalankan perintah ini di Terminal untuk mengamankannya menjadi *Sleep to RAM* murni dan mematikan Hibernasi untuk menjaga umur SSD:
```bash
sudo pmset -a hibernatemode 0 proximitywake 0 standby 0 tcpkeepalive 0 powernap 0 womp 0
sudo rm -f /var/vm/sleepimage
```

### 2. Mengaktifkan TRIM untuk SSD (Wajib)
Karena menggunakan SSD pihak ketiga (Lexar) dengan format APFS, fitur TRIM wajib dinyalakan manual agar SSD tidak lambat dan cepat rusak:
```bash
sudo trimforce enable
```
*(Ketik `y` saat diminta konfirmasi, dan sistem akan otomatis reboot).*

### 3. Solusi "Hantu Bluetooth" (Untuk Barebone tanpa Wi-Fi Card)
Jika PC M720q Anda adalah versi barebone tanpa chip M.2 Intel Wi-Fi/Bluetooth, `bluetoothd` macOS akan sering *error* dan menahan `Package C-State` di angka 0%. Solusi paling bersih:
1. Masuk ke BIOS Lenovo.
2. Ke menu **Devices** -> **Network Setup** (atau I/O Port Access).
3. Ubah pengaturan **Bluetooth** menjadi `Disabled`.

### 4. Solusi NTFS (Hardisk Eksternal Windows)
macOS secara *default* tidak bisa menulis (*write*) ke dalam format NTFS. Apple telah mencabut *driver* internal NTFS sejak macOS Sonoma/Sequoia. Solusi gratis terbaik saat ini adalah dengan menginstal aplikasi **Hasleo NTFS for Mac** agar bisa *copy-paste* ke hardisk eksternal.

> **Catatan Troubleshoot Audio:** Jika tiba-tiba suara macOS drop/menghilang (*ducking glitch*) sesaat setelah muncul *pop-up* izin administrator dari sistem, Anda cukup menjalankan aplikasi **Reset Audio.app** yang telah disediakan di folder `Tools` dalam repositori ini untuk me-refresh *driver* suara.

---
*Dokumentasi ini diupdate secara otomatis paska optimasi stabilitas (CPUFriend, Jaringan, dan Estetika) untuk mempermudah proses instalasi di masa mendatang bagi hardware Lenovo B360.*
