# Lenovo B360 Hackintosh (macOS Sequoia) - Golden Build 🍎

Repositari ini berisi folder EFI lengkap dan sudah dioptimasi untuk menjalankan macOS Sequoia pada PC Desktop OEM Lenovo dengan prosesor Intel Core i5-8400T (Coffee Lake). 

## 💻 Spesifikasi Hardware
- **PC:** Lenovo Desktop (OEM)
- **Motherboard:** Chipset B360
- **Prosesor:** Intel Core i5-8400T (Generasi ke-8)
- **Grafis Terintegrasi:** Intel UHD Graphics 630
- **Audio:** Realtek ALC233
- **Penyimpanan:** Lexar 128GB SSD
- **Sistem Operasi:** macOS Sequoia 15.x
- **Bootloader:** OpenCore
- **SMBIOS:** iMac19,1

## ✨ Fitur yang Berjalan (100% Working)
- ✅ **QE/CI Graphics Acceleration:** UHD 630 berjalan native. Apple Graphics Power Management (AGPM) aktif untuk kontrol frekuensi dinamis (VRAM: Framebuffer 9MB, Stolen 19MB untuk *bypass limit* 32MB DVMT BIOS Lenovo).
- ✅ **CPU Power Management (SpeedStep):** XCPM aktif via `SSDT-PLUG`. Idle di ~800MHz, Max Turbo 3.3GHz. Suhu sangat stabil.
- ✅ **Sleep / Wake:** Berjalan sempurna (Sleep to RAM murni) tanpa *Kernel Panic* atau *Random Wake*.
- ✅ **USB Mapping:** Bebas limit 15-port via `USBToolBox` + `UTBMap.kext`. (Kext *XHCI-unsupported* sudah dibersihkan karena redundan).
- ✅ **Apple Services (iCloud, App Store, iMessage):** Kartu LAN Ethernet (Intel Mausi) terdeteksi resmi sebagai `en0` Built-in.
- ✅ **NVRAM:** Native NVRAM bekerja baik via `SSDT-PMC`.
- ✅ **Ketahanan BIOS:** `SSDT-RTCAWAC` + *DisableRtcChecksum* aktif untuk mencegah *BIOS Reset Error 0199* khas PC OEM Lenovo.
- ✅ **Audio:** Realtek ALC233 berfungsi sempurna dengan boot-arg `alcid=16`.

---

## 🛠️ Post-Install Optimizations (Catatan Penting)
Jika Anda melakukan instalasi ulang menggunakan EFI ini di masa mendatang, **wajib** melakukan langkah-langkah optimasi via Terminal di bawah ini agar sistem berjalan sempurna:

### 1. Perbaikan Sleep / Wake (Wajib)
Secara bawaan, macOS menggunakan konfigurasi daya yang sering menyebabkan Hackintosh Desktop terbangun sendiri (Random Wake) atau error saat *Sleep* (karena mencoba menulis RAM ke disk). Jalankan perintah ini di Terminal untuk mengamankannya menjadi *Sleep to RAM* murni:
```bash
sudo pmset -a hibernatemode 0 proximitywake 0 standby 0 tcpkeepalive 0 powernap 0 womp 0
```

### 2. Mengaktifkan TRIM untuk SSD (Wajib)
Karena menggunakan SSD pihak ketiga (Lexar) dengan format APFS, fitur TRIM wajib dinyalakan manual agar SSD tidak lambat dan cepat rusak:
```bash
sudo trimforce enable
```
*(Ketik `y` saat diminta konfirmasi, dan sistem akan otomatis reboot).*

### 3. Menonaktifkan Animasi Sistem (Opsional untuk Performa)
Untuk membuat sistem terasa instan dan lebih *snappy* dengan menonaktifkan efek animasi jendela, Dock, dan Launchpad:
```bash
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false
defaults write com.apple.dock expose-animation-duration -float 0.1
defaults write com.apple.dock autohide-delay -float 0
defaults write com.apple.dock autohide-time-modifier -float 0
defaults write com.apple.dock springboard-show-duration -float 0
defaults write com.apple.dock springboard-hide-duration -float 0
defaults write com.apple.finder DisableAllAnimations -bool true
defaults write com.apple.Accessibility ReduceMotionEnabled -bool true
killall Dock; killall Finder
```

### 4. Solusi NTFS (Hardisk Eksternal Windows)
macOS secara *default* tidak bisa menulis (*write*) ke dalam format NTFS. Apple telah mencabut *driver* internal NTFS sejak macOS Sonoma/Sequoia. Solusi gratis terbaik saat ini adalah dengan menginstal aplikasi **Hasleo NTFS for Mac** agar bisa *copy-paste* ke hardisk eksternal.

> **Catatan Troubleshoot Audio:** Jika tiba-tiba suara macOS drop/menghilang (*ducking glitch*) sesaat setelah muncul *pop-up* izin administrator dari sistem, jalankan `sudo killall coreaudiod` di Terminal untuk me-refresh *driver* suara.

---
*Dokumentasi ini dibuat sebagai backup pribadi dan referensi untuk mempermudah proses instalasi di masa mendatang atau bagi pengguna dengan hardware Lenovo B360 serupa.*
