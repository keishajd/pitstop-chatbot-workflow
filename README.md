pitstop-chatbot-workflow
n8n workflow for Pitstop Gaming Chatbot.

# 🤖 Pitstop Buddy - AI WhatsApp Booking Chatbot
🏁 Overview
Proyek ini mengimplementasikan sistem otomatisasi lengkap untuk rental PS "Pitstop Gaming" menggunakan n8n, AI, dan integrasi pembayaran. Chatbot dapat membedakan niat user (bertanya vs booking) dan mengelola reservasi dari awal hingga akhir pembayaran.

## 🌟 Fitur Canggih
### 🧠 AI-Powered Logic (Inti Kecerdasan)
* **AI Intent Routing (Router):** Menggunakan Google Gemini untuk secara cerdas mengklasifikasikan niat pengguna ke dalam satu dari dua jalur: GENERAL (FAQ/Info) atau BOOKING (Transaksi), memastikan respons yang relevan.
* **Dialogue & Data Extraction:** LLM mengelola dialog multi-turn, secara otomatis mengekstrak (slot-filling) data penting (Nama, Tanggal, Jam, Unit) dari percakapan, dan melakukan validasi awal (misalnya, batas H+2).

### ⚙️ Transaction & Maintenance Engine
* **Slot Checking:** Otomatis memeriksa ketersediaan Unit (Regular/Premium) di Google Sheets berdasarkan tanggal dan jam yang diminta.
* **Price Calculation & Midtrans:** Logika harga (tarif Weekday/Weekend) dihitung di Code Node n8n. Midtrans berfungsi untuk menghasilkan payment link Snap yang siap bayar berdasarkan hasil perhitungan tersebut.
* **Auto-Maintenance:** Menggunakan Cron Job (Schedule Trigger) untuk membatalkan booking yang berstatus "Pending" lebih dari 3 menit (expired payment).
* **Daily Archival & Reporting:** Menggunakan Cron Job yang berjalan setiap malam untuk memindahkan data booking yang sukses ke sheet "Arsip Transaksi" untuk keperluan pelaporan dan maintenance data harian.
* **Centralized Config:** Menggunakan Node CONFIG untuk mempermudah penggantian Sheet ID dan URL API.

## 🛠️ Tech Stack & Integrations
* **Automation Platform:** n8n (Self-hosted) 
* **AI/LLM:** Google Gemini 2.5 Flash
* **Database:** Google Sheets
* **Payment Gateway:** Midtrans (Snap API)
* **Messaging API:** WAHA (WhatsApp HTTP API)

## ⚙️ Panduan Deployment Cepat
Proyek ini adalah workflow berbasis self-hosted webhook dan memerlukan langkah-langkah deployment yang cermat:
**1. Prasyarat & Hosting**
* Wajib Docker: Asumsi deployment menggunakan Docker untuk menjalankan kontainer n8n dan WAHA.
* Public Endpoint: Diperlukan layanan tunneling (Cloudflare Tunnel) untuk membuat Public URL n8n Anda agar dapat menerima webhook dari WAHA dan Midtrans.

**2. Konfigurasi n8n**
1. Import: Impor file My workflow (2).json ke n8n Anda.
2. Credentials: Buat dan hubungkan 3 Credential utama (Google Sheets OAuth, Midtrans Basic Auth, WAHA Header Auth) pada node-node terkait di workflow.

**3. Edit Node CONFIG**
Buka Node CONFIG di awal workflow dan ganti nilai placeholder berikut:
* Sheet_ID: Ganti "YOUR_SHEET_ID" dengan ID Spreadsheet Google Sheets Anda.
* WA_Url: Ganti "YOUR_WAHA_BASE_URL" dengan URL endpoint kirim pesan dari instalasi WAHA Anda.

**4. Finalisasi Webhook**
* Salin Public Endpoint URL n8n Anda (Cloudflare Tunnel) dan daftarkan sebagai webhook di pengaturan WAHA dan Midtrans.

## 📺 Video Demo
[![Tonton Video Demo](https://img.youtube.com/vi/4e8Dr_qfS4c/0.jpg)](https://youtu.be/4e8Dr_qfS4c)

## 🗺️ Workflow Architecture
Untuk melihat alur kerja lengkap, silakan cek dokumen berikut:
[📄 Lihat Workflow Architecture (PDF)](assets/Workflow.pdf)
