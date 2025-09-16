# QRIS Dinamis

Aplikasi web untuk generate QRIS dinamis menggunakan Nuxt.js + Vue.js yang di-load dari CDN tanpa perlu npm install.

## 🚀 Fitur Utama

### Admin
- ✅ Login & Logout
- ✅ CRUD Store (Create, Read, Update, Delete)
- ✅ Dashboard dengan statistik
- ✅ Kelola data store

### Store
- ✅ Login & Logout
- ✅ Upload QRIS statis (template)
- ✅ Generate QRIS dinamis dengan nominal
- ✅ History QR yang pernah di-generate
- ✅ Download QR Code

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
├── components/
│   ├── Navbar.vue         # Navigation component
│   └── Notification.vue   # Notification component
├── pages/
│   ├── index.vue          # Landing page
│   ├── login.vue          # Login page
│   ├── admin/
│   │   └── dashboard.vue  # Admin dashboard
│   └── store/
│       └── dashboard.vue  # Store dashboard
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

## 👤 Demo Credentials

### Admin
- **Email**: admin@qris.com
- **Password**: admin123
- **Fitur**: Kelola store, lihat statistik

### Store
- **Email**: store@qris.com
- **Password**: store123
- **Fitur**: Upload template, generate QR, lihat history

## 📱 Cara Penggunaan

### Untuk Admin:
1. Login dengan kredensial admin
2. Dashboard menampilkan statistik store
3. Klik "Tambah Store" untuk menambah store baru
4. Edit atau hapus store yang sudah ada

### Untuk Store:
1. Login dengan kredensial store
2. Upload template QRIS statis (PNG/JPG)
3. Masukkan nominal dan keterangan
4. Klik "Generate QR" untuk membuat QR dinamis
5. Lihat history QR yang sudah dibuat
6. Download QR Code yang diinginkan

## 🔧 Konfigurasi CDN

Semua library sudah dikonfigurasi untuk menggunakan CDN:

- **Vue.js**: `https://unpkg.com/vue@3/dist/vue.global.js`
- **Nuxt.js**: `https://unpkg.com/nuxt@3/dist/nuxt.js`
- **TailwindCSS**: `https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css`
- **QRCode.js**: `https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js`

## 💾 Data Storage

Aplikasi menggunakan localStorage untuk menyimpan:
- Data user yang login
- Data store (admin)
- Template QRIS (store)
- History QR generated (store)

## 🎨 UI/UX Features

- ✅ Responsive design (mobile-friendly)
- ✅ Modern UI dengan TailwindCSS
- ✅ Notification system
- ✅ Modal dialogs
- ✅ Loading states
- ✅ Form validation
- ✅ File upload dengan preview

## 🔒 Security Features

- ✅ Route protection (redirect ke login jika belum auth)
- ✅ Role-based access (admin vs store)
- ✅ Session management dengan localStorage

## 📊 Fitur Tambahan

- ✅ Statistik dashboard
- ✅ Export/Download QR Code
- ✅ File upload dengan drag & drop
- ✅ Real-time notifications
- ✅ Responsive tables
- ✅ Search dan filter (dapat dikembangkan)

## 🚀 Deployment

Aplikasi dapat di-deploy ke:
- **Vercel**: `vercel --prod`
- **Netlify**: Upload folder `dist/`
- **GitHub Pages**: Push ke branch `gh-pages`
- **Static Hosting**: Upload folder `dist/` ke hosting apapun

## 📝 Catatan

- Aplikasi ini menggunakan CDN untuk semua dependencies
- Tidak perlu npm install untuk menjalankan
- Data disimpan di localStorage browser
- UI sudah responsive dan mobile-friendly
- Semua fitur sudah siap pakai

## 🤝 Kontribusi

Silakan fork dan submit pull request untuk perbaikan atau fitur tambahan.

## 📄 Lisensi

MIT License