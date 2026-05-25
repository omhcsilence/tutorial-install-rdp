

```markdown
<div align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&duration=3000&pause=1000&color=36BCF7FF&center=true&vCenter=true&width=650&lines=Panduan+Install+XRDP+di+VPS+Ubuntu;Remote+Desktop+Mudah+dan+Cepat!;Dari+Terminal+ke+Desktop+GUI+%F0%9F%96%A5%EF%B8%8F" alt="Typing SVG" />
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu">
  <img src="https://img.shields.io/badge/XRDP-Remote_Desktop-8B0000?style=for-the-badge&logo=microsoft&logoColor=white" alt="XRDP">
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="Bash">
  <img src="https://img.shields.io/badge/Maintained%3F-Yes-brightgreen?style=for-the-badge" alt="Maintenance">
</p>

<br/>

> **⚠️ PENTING:** Pilih GUI yang sesuai dengan kapasitas RAM VPS-mu agar performa tetap lancar. XFCE sangat direkomendasikan untuk VPS dengan RAM kecil.  
> **👤 TIP KEAMANAN:** Jangan gunakan user `root` untuk login RDP! Buat user baru terlebih dahulu.

---

## 👤 Tambah User Baru (Wajib!)

Sebelum melanjutkan instalasi GUI, buatlah user baru untuk login Remote Desktop. **Hindari login langsung sebagai root!**

<table>
  <tr>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/fluency/48/000000/add-user-male.png" width="60"/><br/>
      <strong>1. Buat User Baru</strong><br/>
      <sub>Ganti <code>omhc</code> dengan nama usermu</sub>
    </td>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/color/48/000000/admin-settings-male.png" width="60"/><br/>
      <strong>2. Beri Hak Sudo</strong><br/>
      <sub>Akses administrator untuk user baru</sub>
    </td>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/color/48/000000/key.png" width="60"/><br/>
      <strong>3. Set Password</strong><br/>
      <sub>Password akan diminta saat perintah adduser</sub>
    </td>
  </tr>
  <tr>
    <td align="center">

```bash
sudo adduser omhc
```

</td>
    <td align="center">

```bash
sudo usermod -aG sudo omhc
```

</td>
    <td align="center">

```bash
# Ikuti instruksi yang muncul:
# - Masukkan password baru
# - Konfirmasi password
# - Isi data (bisa dikosongkan dengan ENTER)
```

</td>
  </tr>
</table>

### 🔄 Verifikasi User Baru
Setelah berhasil, cek apakah user sudah memiliki hak sudo dan bisa login.

```bash
# Cek grup user (pastikan ada "sudo")
groups omhc

# Coba login sebagai user baru
su - omhc

# Verifikasi akses sudo
sudo whoami
# Output: root
```

> _Sekarang kamu sudah siap login RDP menggunakan user `omhc` (atau nama user yang kamu buat)!_

---

## ✨ Pilih & Install GUI Favoritmu

<table>
  <tr>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/color/48/000000/xfce.png" width="60"/><br/>
      <strong>XFCE</strong><br/>
      <sub>💡 Ringan (Cocok untuk 1GB RAM)</sub>
    </td>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/color/48/000000/gnome.png" width="60"/><br/>
      <strong>GNOME</strong><br/>
      <sub>🎨 Cukup Ringan (1-4GB RAM)</sub>
    </td>
    <td align="center" width="33%">
      <img src="https://img.icons8.com/color/48/000000/ubuntu--v1.png" width="60"/><br/>
      <strong>Ubuntu Desktop</strong><br/>
      <sub>🚀 Penuh Fitur (>4GB RAM)</sub>
    </td>
  </tr>
  <tr>
    <td align="center">

```bash
sudo apt install -y xfce4 xfce4-goodies
```

</td>
    <td align="center">

```bash
sudo apt install gnome-session -y
```

</td>
    <td align="center">

```bash
sudo apt install ubuntu-desktop -y
```

</td>
  </tr>
</table>

---

## 🔧 Tahapan Instalasi

### 1. 📦 Update Sistem
Pertama, pastikan sistemmu _up-to-date_.
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. 🖥️ Install XRDP
Ini adalah aplikasi utama yang menjembatani koneksi _Remote Desktop Protocol_.
```bash
sudo apt install -y xrdp
```

### 3. ▶️ Jalankan & Cek Status XRDP
Aktifkan XRDP agar berjalan otomatis saat VPS dinyalakan.
```bash
sudo systemctl enable xrdp
sudo systemctl start xrdp
sudo systemctl status xrdp
```
> _Jika muncul status `active (running)` maka XRDP sudah siap!_

### 4. 🔥 Buka Port di Firewall
Biarkan koneksi RDP (port 3389) melewati _firewall_ VPS.
```bash
sudo ufw allow 3389/tcp
```

### 5. 📝 Set Session Default
Agar XRDP tahu GUI mana yang harus dibuka, atur file `.xsession` di direktori _home_ user yang akan login (bukan root!).

**Untuk user `omhc` (XFCE):**
```bash
echo "startxfce4" > /home/omhc/.xsession
chown omhc:omhc /home/omhc/.xsession
```

**Untuk user `omhc` (GNOME / Ubuntu Desktop):**
```bash
echo "gnome-session" > /home/omhc/.xsession
chown omhc:omhc /home/omhc/.xsession
```

> _Jika ada beberapa user, ulangi perintah di atas untuk setiap user dengan menyesuaikan path `/home/namauser/.xsession`._

### 6. 🔄 Restart XRDP
Terapkan konfigurasi baru dengan me-restart servis XRDP.
```bash
sudo systemctl restart xrdp
```

---

## 🔐 Login ke Remote Desktop

Sekarang kamu sudah bisa login menggunakan aplikasi Remote Desktop favoritmu:

| **Platform** | **Aplikasi** |
|---|---|
| 🪟 Windows | **Remote Desktop Connection** (bawaan) |
| 🍎 macOS | **Microsoft Remote Desktop** (App Store) |
| 🐧 Linux | **Remmina** atau **Vinagre** |
| 📱 Android/iOS | **Microsoft Remote Desktop** (Play Store / App Store) |

**Detail Koneksi:**
- **IP Address:** IP VPS kamu
- **Port:** `3389` (default)
- **Username:** `omhc` (atau user yang kamu buat)
- **Password:** Password yang diset saat `adduser`

---

## 🛍️ Bonus: Tools Tambahan (Opsional)

Biar makin nyaman, kamu bisa install _App Store_ dan aplikasi pendukung.

```bash
# Install GNOME Software (GUI App Store)
sudo apt install gnome-software -y

# Install Snap Store alternatif (App Outlet)
sudo snap install app-outlet
```

---

## 🧑‍💻 Credit

<div align="center">
  <br/>
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=500&size=18&pause=1000&color=FFB6C1&center=true&vCenter=true&width=450&lines=Panduan+ini+dipersembahkan+oleh;%E2%9C%A8+OmhcSilence+%E2%9C%A8;Happy+Coding!+%F0%9F%9A%80" alt="Credit Typing SVG" />
  <br/>
  <a href="https://github.com/OmhcSilence">
    <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"/>
  </a>
</div>

---

<p align="center">
  <sub>💡 <strong>Tips:</strong> Gunakan aplikasi <strong>Remote Desktop Connection</strong> bawaan Windows atau <strong>Microsoft Remote Desktop</strong> di macOS/Android untuk terhubung ke IP VPS-mu pada port 3389 dengan user yang sudah dibuat.</sub>
</p>
```
