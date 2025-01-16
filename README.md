# **Laporan UAS - Sistem Operasi Ubuntu**

---

## **Daftar Isi**
1. Folder dan File Management
2. User dan Group Management
3. File Permissions
4. Bash Script Automation
5. Monitoring dan Log Analysis
6. Web Server Installation
7. Secure Remote Access
8. Firewall Setup

---

## **1. Folder dan File Management**

### **Langkah-langkah:**
1. Buat struktur folder yang diminta:

```bash
sudo mkdir -p /srv/startup_data/reports /srv/startup_data/temp /srv/startup_data/archive
```

2. Buat file **README.txt** dengan deskripsi folder:

```bash
echo "This directory structure is used to manage startup data." | sudo tee /srv/startup_data/README.txt
```

### **Verifikasi Keberhasilan:**
1. Cek struktur folder:

```bash
ls -l /srv/startup_data
```

2. Cek isi file **README.txt**:

```bash
cat /srv/startup_data/README.txt
```

---

## **2. User dan Group Management**

### **Langkah-langkah:**
1. Buat user **John_NIM** dan **emma_NIM**:

```bash
sudo adduser John_NIM
sudo adduser emma_NIM
```

2. Buat grup **startup_team** dan tambahkan kedua user ke grup tersebut:

```bash
sudo groupadd startup_team
sudo usermod -aG startup_team John_NIM
sudo usermod -aG startup_team emma_NIM
```

### **Verifikasi Keberhasilan:**
1. Cek apakah user sudah dibuat:

```bash
cat /etc/passwd | grep John_NIM
cat /etc/passwd | grep emma_NIM
```

2. Cek apakah grup **startup_team** sudah dibuat:

```bash
getent group startup_team
```

---

## **3. File Permissions**

### **Langkah-langkah:**
1. Atur hak akses pada folder:

```bash
sudo chown John_NIM /srv/startup_data/reports/
sudo chmod 660 /srv/startup_data/reports/

sudo chown emma_NIM /srv/startup_data/temp/
sudo chmod 770 /srv/startup_data/temp/

sudo chmod 640 /srv/startup_data/archive/*
sudo chown -R :startup_team /srv/startup_data/
sudo chmod 770 /srv/startup_data/
```

### **Verifikasi Keberhasilan:**
1. Cek kepemilikan dan hak akses folder:

```bash
ls -l /srv/startup_data
```

---

## **4. Bash Script Automation**

### **Langkah-langkah:**
1. Buat script **archive_cleanup.sh**:

```bash
sudo nano /srv/startup_data/archive_cleanup.sh
```

Isi script:

```bash
#!/bin/bash

mkdir -p /srv/startup_data/logs
find /srv/startup_data/temp/ -type f -mtime +7 -exec mv {} /srv/startup_data/archive/ \;
timestamp=$(date "+%Y-%m-%d at %H:%M:%S")
echo "Files moved on $timestamp" >> /srv/startup_data/logs/cleanup.log
```

2. Berikan izin eksekusi:

```bash
sudo chmod +x /srv/startup_data/archive_cleanup.sh
```

### **Verifikasi Keberhasilan:**
1. Jalankan script:

```bash
sudo /srv/startup_data/archive_cleanup.sh
```

2. Cek log aktivitas:

```bash
cat /srv/startup_data/logs/cleanup.log
```

---

## **5. Monitoring dan Log Analysis**

### **Langkah-langkah:**
1. Pantau log autentikasi terakhir:

```bash
sudo tail -n 50 /var/log/auth.log | grep "Failed password"
```

2. Gunakan **awk** untuk menampilkan timestamp dan username:

```bash
sudo tail -n 50 /var/log/auth.log | grep "Failed password" | awk '{print $1, $2, $3, $11}'
```

### **Verifikasi Keberhasilan:**
1. Output akan menampilkan hasil seperti:

```
Jan 9 12:34:56 username
```

---

## **6. Web Server Installation**

### **Langkah-langkah:**
1. Instal dan konfigurasi Apache:

```bash
sudo apt update
sudo apt install apache2 -y
```

2. Buat file HTML:

```bash
echo '<h1>Welcome to the Startup Company!</h1>' | sudo tee /var/www/html/home.html
```

### **Verifikasi Keberhasilan:**
1. Cek status Apache:

```bash
sudo systemctl status apache2
```

2. Akses halaman di browser:

```
http://localhost/home.html
```

---

## **7. Secure Remote Access**

### **Langkah-langkah:**
1. Edit konfigurasi SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

2. Tambahkan baris berikut:

```conf
AllowUsers emma_NIM
```

3. Restart layanan SSH:

```bash
sudo systemctl restart ssh
```

### **Verifikasi Keberhasilan:**
1. Coba login dengan user **emma_NIM**:

```bash
ssh emma_NIM@localhost
```

2. Pastikan user lain ditolak:

```bash
ssh John_NIM@localhost
```

---

## **8. Firewall Setup**

### **Langkah-langkah:**
1. Konfigurasi firewall UFW:

```bash
sudo ufw allow 80
sudo ufw allow 25
sudo ufw allow 22
sudo ufw enable
```

### **Verifikasi Keberhasilan:**
1. Cek status UFW:

```bash
sudo ufw status
```

**Output yang diharapkan:**

```
To                         Action      From
--                         ------      ----
80/tcp                     ALLOW       Anywhere
25/tcp                     ALLOW       Anywhere
22/tcp                     ALLOW       Anywhere
```

