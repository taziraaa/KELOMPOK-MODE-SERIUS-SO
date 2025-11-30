# Implementasi Mail Server Menggunakan Postfix dan Dovecot

Repository ini berisi dokumentasi dan konfigurasi untuk proyek akhir mata kuliah Sistem Operasi tentang implementasi mail server berbasis Ubuntu menggunakan Postfix dan Dovecot.

## 📋 Deskripsi Proyek

Proyek ini mengimplementasikan infrastruktur mail server mandiri pada sistem operasi Ubuntu Server untuk layanan SMTP dan IMAP di jaringan lokal. Mail server dibangun menggunakan:
- **Postfix** sebagai Mail Transfer Agent (MTA) untuk mengelola pengiriman email via protokol SMTP
- **Dovecot** sebagai IMAP Server untuk mengelola penyimpanan dan akses email pengguna

## 🎯 Tujuan

1. Mengimplementasikan layanan mail server pada Ubuntu Server menggunakan Postfix dan Dovecot
2. Menjelaskan dan mempraktikkan alur kerja sistem email dari pengiriman hingga penerimaan pesan
3. Mengonfigurasi SMTP dan IMAP agar email dapat dikirim serta diakses oleh user melalui mail client
4. Membangun sistem email dalam jaringan lokal yang dapat berfungsi sebagai sarana komunikasi internal
5. Melatih kemampuan administrasi server, konfigurasi jaringan, serta troubleshooting pada sistem berbasis Linux

## 🔧 Teknologi yang Digunakan

- **OS**: Ubuntu Server (20.04/22.04)
- **MTA**: Postfix
- **IMAP Server**: Dovecot (dovecot-core, dovecot-imapd)
- **Security**: OpenSSL, UFW/Iptables
- **Mail Client**: Thunderbird/Evolution
- **Tools**: net-tools, telnet, openssl s_client

## 📦 Komponen Sistem

### Protokol Email
- **SMTP** (Simple Mail Transfer Protocol) - Port 25, 587
- **IMAP** (Internet Message Access Protocol) - Port 143, 993
- **POP3** (Post Office Protocol v3) - Port 110, 995 (opsional)

### Arsitektur Mail Server
1. **Mail Transfer Agent (MTA)**: Postfix - mengirim dan menerima email antarserver
2. **Mail Delivery Agent (MDA)**: Dovecot - mengantarkan email lokal ke mailbox pengguna
3. **Mail Access Server**: Dovecot IMAP/POP3 - menyediakan layanan akses email

## 🚀 Tahapan Implementasi

### 1. Persiapan Lingkungan Server
```bash
# Update repository
sudo apt update && sudo apt upgrade -y

# Install tools pendukung
sudo apt install net-tools openssl vim -y

# Konfigurasi hostname
sudo hostnamectl set-hostname mail.localdomain

# Set IP statis (edit /etc/netplan/)
```

### 2. Instalasi Postfix
```bash
# Install Postfix
sudo apt install postfix -y

# Pilih "Internet Site" pada konfigurasi
# Set domain: mail.localdomain
```

**Konfigurasi `/etc/postfix/main.cf`:**
- Set hostname dan domain lokal
- Aktifkan format Maildir
- Konfigurasi relay dan routing

### 3. Instalasi Dovecot
```bash
# Install Dovecot
sudo apt install dovecot-core dovecot-imapd dovecot-pop3d -y
```

**Konfigurasi Dovecot:**
- `/etc/dovecot/conf.d/10-mail.conf` - Set direktori Maildir
- `/etc/dovecot/conf.d/10-auth.conf` - Konfigurasi autentikasi
- `/etc/dovecot/conf.d/10-master.conf` - Setup layanan IMAP

### 4. Konfigurasi Keamanan
```bash
# Setup firewall
sudo ufw enable
sudo ufw allow 25/tcp    # SMTP
sudo ufw allow 587/tcp   # SMTP Submission
sudo ufw allow 143/tcp   # IMAP
sudo ufw allow 993/tcp   # IMAPS

# Setup SSL/TLS
sudo openssl req -new -x509 -days 365 -nodes \
  -out /etc/ssl/certs/mailserver.pem \
  -keyout /etc/ssl/private/mailserver.key
```

## 🧪 Pengujian

### Test SMTP (Pengiriman Email)
```bash
# Kirim email via command line
echo "Test email body" | mail -s "Test Subject" user@mail.localdomain

# Test SMTP dengan telnet
telnet localhost 25
```

### Test IMAP (Penerimaan Email)
```bash
# Test IMAP dengan telnet
telnet localhost 143

# Atau gunakan openssl untuk IMAPS
openssl s_client -connect localhost:993
```

### Verifikasi Log
```bash
# Cek log Postfix dan Dovecot
sudo tail -f /var/log/mail.log
```

## 📊 Struktur Penyimpanan

Format penyimpanan email menggunakan **Maildir**:
```
/home/user/Maildir/
├── new/     # Email baru
├── cur/     # Email yang sudah dibaca
└── tmp/     # Temporary files
```

## 📁 File Konfigurasi Penting

### Postfix
- `/etc/postfix/main.cf` - Konfigurasi utama
- `/etc/postfix/master.cf` - Konfigurasi daemon

### Dovecot
- `/etc/dovecot/conf.d/10-mail.conf` - Mailbox storage
- `/etc/dovecot/conf.d/10-auth.conf` - Authentication
- `/etc/dovecot/conf.d/10-master.conf` - Service configuration

## 🎓 Tim Pengembang

**Kelompok Mode Serius**
- 2401020005 - Juan Prattycha Tazira (Ketua)
- 2401020014 - Naurah Nadhif Shahada
- 2401020026 - Pitria
# Implementasi Mail Server Menggunakan Postfix dan Dovecot

Repository ini berisi dokumentasi dan konfigurasi untuk proyek akhir mata kuliah Sistem Operasi tentang implementasi mail server berbasis Ubuntu menggunakan Postfix dan Dovecot.

## 📋 Deskripsi Proyek

Proyek ini mengimplementasikan infrastruktur mail server mandiri pada sistem operasi Ubuntu Server untuk layanan SMTP dan IMAP di jaringan lokal. Mail server dibangun menggunakan:
- **Postfix** sebagai Mail Transfer Agent (MTA) untuk mengelola pengiriman email via protokol SMTP
- **Dovecot** sebagai IMAP Server untuk mengelola penyimpanan dan akses email pengguna

## 🎯 Tujuan

1. Mengimplementasikan layanan mail server pada Ubuntu Server menggunakan Postfix dan Dovecot
2. Menjelaskan dan mempraktikkan alur kerja sistem email dari pengiriman hingga penerimaan pesan
3. Mengonfigurasi SMTP dan IMAP agar email dapat dikirim serta diakses oleh user melalui mail client
4. Membangun sistem email dalam jaringan lokal yang dapat berfungsi sebagai sarana komunikasi internal
5. Melatih kemampuan administrasi server, konfigurasi jaringan, serta troubleshooting pada sistem berbasis Linux

## 🔧 Teknologi yang Digunakan

- **OS**: Ubuntu Server (20.04/22.04)
- **MTA**: Postfix
- **IMAP Server**: Dovecot (dovecot-core, dovecot-imapd)
- **Security**: OpenSSL, UFW/Iptables
- **Mail Client**: Thunderbird/Evolution
- **Tools**: net-tools, telnet, openssl s_client

## 📦 Komponen Sistem

### Protokol Email
- **SMTP** (Simple Mail Transfer Protocol) - Port 25, 587
- **IMAP** (Internet Message Access Protocol) - Port 143, 993
- **POP3** (Post Office Protocol v3) - Port 110, 995 (opsional)

### Arsitektur Mail Server
1. **Mail Transfer Agent (MTA)**: Postfix - mengirim dan menerima email antarserver
2. **Mail Delivery Agent (MDA)**: Dovecot - mengantarkan email lokal ke mailbox pengguna
3. **Mail Access Server**: Dovecot IMAP/POP3 - menyediakan layanan akses email

## 🚀 Tahapan Implementasi

### 1. Persiapan Lingkungan Server
```bash
# Update repository
sudo apt update && sudo apt upgrade -y

# Install tools pendukung
sudo apt install net-tools openssl vim -y

# Konfigurasi hostname
sudo hostnamectl set-hostname mail.localdomain

# Set IP statis (edit /etc/netplan/)
```

### 2. Instalasi Postfix
```bash
# Install Postfix
sudo apt install postfix -y

# Pilih "Internet Site" pada konfigurasi
# Set domain: mail.localdomain
```

**Konfigurasi `/etc/postfix/main.cf`:**
- Set hostname dan domain lokal
- Aktifkan format Maildir
- Konfigurasi relay dan routing

### 3. Instalasi Dovecot
```bash
# Install Dovecot
sudo apt install dovecot-core dovecot-imapd dovecot-pop3d -y
```

**Konfigurasi Dovecot:**
- `/etc/dovecot/conf.d/10-mail.conf` - Set direktori Maildir
- `/etc/dovecot/conf.d/10-auth.conf` - Konfigurasi autentikasi
- `/etc/dovecot/conf.d/10-master.conf` - Setup layanan IMAP

### 4. Konfigurasi Keamanan
```bash
# Setup firewall
sudo ufw enable
sudo ufw allow 25/tcp    # SMTP
sudo ufw allow 587/tcp   # SMTP Submission
sudo ufw allow 143/tcp   # IMAP
sudo ufw allow 993/tcp   # IMAPS

# Setup SSL/TLS
sudo openssl req -new -x509 -days 365 -nodes \
  -out /etc/ssl/certs/mailserver.pem \
  -keyout /etc/ssl/private/mailserver.key
```

## 🧪 Pengujian

### Test SMTP (Pengiriman Email)
```bash
# Kirim email via command line
echo "Test email body" | mail -s "Test Subject" user@mail.localdomain

# Test SMTP dengan telnet
telnet localhost 25
```

### Test IMAP (Penerimaan Email)
```bash
# Test IMAP dengan telnet
telnet localhost 143

# Atau gunakan openssl untuk IMAPS
openssl s_client -connect localhost:993
```

### Verifikasi Log
```bash
# Cek log Postfix dan Dovecot
sudo tail -f /var/log/mail.log
```

## 📊 Struktur Penyimpanan

Format penyimpanan email menggunakan **Maildir**:
```
/home/user/Maildir/
├── new/     # Email baru
├── cur/     # Email yang sudah dibaca
└── tmp/     # Temporary files
```

## 📁 File Konfigurasi Penting

### Postfix
- `/etc/postfix/main.cf` - Konfigurasi utama
- `/etc/postfix/master.cf` - Konfigurasi daemon

### Dovecot
- `/etc/dovecot/conf.d/10-mail.conf` - Mailbox storage
- `/etc/dovecot/conf.d/10-auth.conf` - Authentication
- `/etc/dovecot/conf.d/10-master.conf` - Service configuration

## 🎓 Tim Pengembang

**Kelompok Mode Serius**
- 2401020005 - Juan Prattycha Tazira (Ketua)
- 2401020014 - Naurah Nadhif Shahada
- 2401020026 - Pitria
- 2401020037 - Gio Argama Sitohang

**Dosen Pengampu:**  
Ferdi Chahyadi, S.Kom., M.Cs

**Program Studi:**  
Teknik Informatika  
Fakultas Teknik dan Teknologi Kemaritiman  
Universitas Maritim Raja Ali Haji

**Tahun Akademik:** 2025/2026

## 📝 Batasan Proyek

1. Sistem mail server hanya dibangun menggunakan Ubuntu Server
2. Mail server hanya menggunakan Postfix dan Dovecot
3. Implementasi dilakukan dalam jaringan lokal (LAN)
4. Tidak mencakup konfigurasi spam filtering, DKIM, SPF, atau DMARC
5. Pengujian terbatas pada pengiriman dan penerimaan pesan lokal
6. Tidak membahas integrasi webmail (Roundcube/RainLoop)

## ✅ Indikator Keberhasilan

- [x] Server dapat mengirim email melalui SMTP (Postfix)
- [x] Server dapat menerima dan menampilkan email melalui IMAP (Dovecot)
- [x] Mailbox pengguna tersimpan dalam format Maildir
- [x] Firewall berfungsi dan hanya membuka port layanan mail
- [x] Semua pengujian berjalan tanpa error pada log
- [x] Dokumentasi lengkap tersusun sistematis

## 📚 Referensi

- Dokumentasi Resmi Postfix: http://www.postfix.org/documentation.html
- Dokumentasi Resmi Dovecot: https://doc.dovecot.org/
- RFC 5321 - Simple Mail Transfer Protocol
- RFC 3501 - Internet Message Access Protocol

## 📄 Lisensi

Proyek ini dibuat untuk keperluan akademik sebagai tugas akhir mata kuliah Sistem Operasi.

---

**Catatan**: Implementasi teknis mengikuti proposal yang telah disetujui. Dokumentasi lengkap dan hasil pengujian akan diperbarui setelah tahap implementasi selesai.

**Dosen Pengampu:**  
Ferdi Chahyadi, S.Kom., M.Cs

**Program Studi:**  
Teknik Informatika  
Fakultas Teknik dan Teknologi Kemaritiman  
Universitas Maritim Raja Ali Haji

**Tahun Akademik:** 2025/2026

## 📝 Batasan Proyek

1. Sistem mail server hanya dibangun menggunakan Ubuntu Server
2. Mail server hanya menggunakan Postfix dan Dovecot
3. Implementasi dilakukan dalam jaringan lokal (LAN)
4. Tidak mencakup konfigurasi spam filtering, DKIM, SPF, atau DMARC
5. Pengujian terbatas pada pengiriman dan penerimaan pesan lokal
6. Tidak membahas integrasi webmail (Roundcube/RainLoop)

## ✅ Indikator Keberhasilan

- [x] Server dapat mengirim email melalui SMTP (Postfix)
- [x] Server dapat menerima dan menampilkan email melalui IMAP (Dovecot)
- [x] Mailbox pengguna tersimpan dalam format Maildir
- [x] Firewall berfungsi dan hanya membuka port layanan mail
- [x] Semua pengujian berjalan tanpa error pada log
- [x] Dokumentasi lengkap tersusun sistematis

## 📚 Referensi

- Dokumentasi Resmi Postfix: http://www.postfix.org/documentation.html
- Dokumentasi Resmi Dovecot: https://doc.dovecot.org/
- RFC 5321 - Simple Mail Transfer Protocol
- RFC 3501 - Internet Message Access Protocol

## 📄 Lisensi

Proyek ini dibuat untuk keperluan akademik sebagai tugas akhir mata kuliah Sistem Operasi.

---

**Catatan**: Implementasi teknis mengikuti proposal yang telah disetujui. Dokumentasi lengkap dan hasil pengujian akan diperbarui setelah tahap implementasi selesai.