# QRIS Dinamis Generator

Aplikasi web sederhana untuk generate QRIS dinamis dari template QRIS statis menggunakan Nuxt.js + Vue.js.

## 🚀 Fitur Utama

- ✅ Upload QRIS statis (template)
- ✅ Extract data merchant dari QRIS statis
- ✅ Generate QRIS dinamis dengan nominal
- ✅ History QR yang pernah di-generate
- ✅ Download QR Code
- ✅ Preview QR dengan detail informasi

## 🛠️ Teknologi

- **Framework**: Nuxt.js 3 + Vue.js 3 (CDN)
- **Styling**: TailwindCSS (CDN)
- **QR Generator**: QRCode.js (CDN)
- **Storage**: localStorage
- **UI**: Responsive design

## 📁 Struktur Project

```
qris-dinamis/
├── app.vue                 # Main app component
├── nuxt.config.ts         # Nuxt configuration
├── pages/
│   └── index.vue          # Main page dengan semua fitur
└── public/
    └── favicon.ico
```

## 🚀 Cara Menjalankan

### Opsi 1: Development Server (Recommended)
```bash
npm run dev
```

### Opsi 2: Static Build
```bash
npm run build
npm run preview
```

### Opsi 3: Serve Static Files
Setelah build, file di folder `dist/` dapat di-serve dengan web server apapun.

## 🎯 Cara Penggunaan

1. **Upload Template QRIS** - Upload gambar QRIS statis atau file text QRIS
2. **Extract Data** - Klik "Extract Data" untuk mengekstrak Merchant ID dan info lain
3. **Generate QR Dinamis** - Masukkan nominal dan keterangan, lalu generate QR
4. **Download/Preview** - Lihat atau download QR yang sudah dibuat

## 🔧 Format yang Didukung

- **Gambar QR**: PNG, JPG dengan QR code
- **File Text**: File .txt berisi data QRIS
- **Format EMV**: Standar QRIS Indonesia

## 🔧 Konfigurasi CDN

Semua library sudah dikonfigurasi untuk menggunakan CDN:

- **Vue.js**: `https://unpkg.com/vue@3/dist/vue.global.js`
- **Nuxt.js**: `https://unpkg.com/nuxt@3/dist/nuxt.js`
- **TailwindCSS**: `https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css`
- **QRCode.js**: `https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js`

## 💾 Data Storage

Aplikasi menggunakan localStorage untuk menyimpan:
- Template QRIS yang di-upload
- History QR yang di-generate
- Data merchant yang diekstrak

## 🎨 UI/UX Features

- ✅ Responsive design (mobile-friendly)
- ✅ Modern UI dengan TailwindCSS
- ✅ Notification system
- ✅ Modal dialogs
- ✅ Loading states
- ✅ Form validation
- ✅ File upload dengan preview

## 🔒 Features

- ✅ Format QRIS standar EMV
- ✅ CRC16 checksum yang benar
- ✅ Template-based generation
- ✅ Real-time preview

## 📊 Fitur Tambahan

- ✅ Export/Download QR Code
- ✅ File upload dengan drag & drop
- ✅ Real-time notifications
- ✅ Responsive design
- ✅ History tracking

## 🚀 Deployment

Aplikasi dapat di-deploy ke:
- **Vercel**: `vercel --prod`
- **Netlify**: Upload folder `dist/`
- **GitHub Pages**: Push ke branch `gh-pages`
- **Static Hosting**: Upload folder `dist/` ke hosting apapun

## 📝 Catatan

- Aplikasi sederhana tanpa login/authentication
- Data disimpan di localStorage browser
- UI sudah responsive dan mobile-friendly
- Menggunakan algoritma CRC16 yang benar untuk QRIS
- Semua fitur sudah siap pakai

## 🤝 Kontribusi

Silakan fork dan submit pull request untuk perbaikan atau fitur tambahan.

## 📄 Lisensi

MIT License