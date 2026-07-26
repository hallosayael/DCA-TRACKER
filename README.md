# 🚀 Nabung Crypto Pro — DCA Tracker

Web app sederhana untuk melacak portfolio investasi crypto dengan strategi **DCA (Dollar Cost Averaging)**. Cukup satu file `index.html`, tanpa build tool, data tersimpan aman di cloud per akun Google.

🔗 **Live:** [nabungcrypto.vercel.app](https://nabungcrypto.vercel.app)

## ✨ Fitur

- 🔐 **Login Google** — tiap user punya data sendiri yang terisolasi
- ☁️ **Cloud sync** — transaksi tersimpan di Firebase Firestore, otomatis ke-load di perangkat mana pun
- 📊 **Multi-aset** — BTC, ETH, SOL, BNB, SUI, dan PAXG (emas)
- 💰 **Hitung otomatis** — average buy price, floating & realized P/L, quantity, nilai pasar
- 📈 **Live price** dari CoinGecko API (refresh tiap 60 detik)
- 🧮 **DCA Calculator** — simulasikan harga rata-rata baru sebelum nambah beli
- 📸 **Share P/L** — export kartu performa aset jadi gambar PNG
- 📄 **Export CSV** — unduh riwayat transaksi & ringkasan posisi
- 🔒 **Mode privasi** — sembunyikan angka nominal saat screenshot
- 📱 **Responsif** — tampilan mobile dengan bottom navigation

## 🛠️ Tech Stack

| Bagian | Teknologi |
|---|---|
| Frontend | HTML + Vanilla JS (single file) |
| Database | Firebase Firestore |
| Autentikasi | Firebase Authentication (Google) |
| Chart | Chart.js |
| Harga live | CoinGecko API |
| Hosting | Vercel |

## 🗄️ Struktur Data

Transaksi disimpan sebagai subcollection per user:

```
users/{uid}/transaksi/{docId}
  ├─ asset_id   : "BTC" | "ETH" | "SOL" | "BNB" | "SUI" | "PAXG"
  ├─ tipe       : "BUY" | "SELL"
  ├─ tanggal    : "YYYY-MM-DD"
  ├─ harga      : number  (harga aset saat transaksi, USD)
  ├─ nominal    : number  (nilai investasi, USD)
  ├─ qty        : number  (jumlah koin; negatif untuk SELL)
  └─ created_at : timestamp
```

## 🔐 Security Rules

Setiap user hanya bisa membaca & menulis datanya sendiri:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/transaksi/{doc} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## ⚙️ Setup (jika ingin deploy sendiri)

1. Buat project di [Firebase Console](https://console.firebase.google.com)
2. Aktifkan **Authentication → Google** dan **Firestore Database** (mode production)
3. Pasang **Security Rules** seperti di atas
4. Salin konfigurasi web app (`firebaseConfig`) ke dalam `index.html`
5. Tambahkan domain hosting kamu di **Authentication → Settings → Authorized domains**
6. Deploy `index.html` ke Vercel / hosting statik apa pun

> Firebase plan **Spark (gratis)** sudah lebih dari cukup untuk pemakaian pribadi dan **tidak auto-pause** saat aplikasi sepi.

## 📝 Catatan

- `firebaseConfig` memang dirancang untuk dipasang di sisi client (bersifat publik). Keamanan sesungguhnya ada di **Security Rules**, bukan di menyembunyikan config.
- Aplikasi ini murni untuk pencatatan pribadi — bukan nasihat keuangan/investasi.
