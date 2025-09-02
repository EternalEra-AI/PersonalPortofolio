# Personal Portfolio — gumxlarcw.id

**Wisnu Candra Gumelar** — software engineer yang membangun pengalaman digital yang rapi dan cepat.  
Situs ini dibuat dengan **Gatsby (React)** dan di-deploy sebagai **static site** di server Linux (Nginx).

**Live:** https://gumxlarcw.id  

---

## ✨ Fitur

- ⚡️ **Gatsby (React)** — cepat, SEO-friendly, dan modern
- 🌓 **Dark UI** dengan tipografi custom (Calibre, SF Mono)
- 🧭 Halaman **About / Experience / Work / Contact**
- 📝 Bagian **Pensieve** (tulisan/notes) dengan halaman tags
- 📄 **Resume PDF** (`/resume.pdf`)
- 📈 **Google Analytics** terpasang
- 🗺️ **Sitemap** otomatis (`/sitemap/`)
- 🔍 **Robots.txt** & meta SEO siap produksi
- 📦 **PWA/Offline** via Workbox (service worker)

Struktur build di server (cuplikan):
```
/var/www/gumxlarcw.id
├── index.html
├── archive/
├── pensieve/
├── static/        # aset teroptimasi (webp/avif/woff2)
├── sitemap/
├── icons/         # PWA icons
├── resume.pdf
└── sw.js          # service worker
```

---

## 🛠️ Teknologi

- Node.js (disarankan **14.x atau 16.x LTS**)  
- Gatsby **3.x**  
- React **17**  
- styled-components, Workbox, gatsby-image

> Versi dapat dilihat dari meta build: `Gatsby 3.15.0`.

---

## 🚀 Pengembangan Lokal

1. **Clone** dan masuk ke project:
   ```bash
   git clone https://github.com/<username>/PersonalPortofolio.git
   cd PersonalPortofolio
   ```

2. **Install dependencies**:
   ```bash
   npm install
   # atau:
   yarn
   ```

3. **Jalankan development server**:
   ```bash
   npm run develop
   # atau:
   gatsby develop
   # buka http://localhost:8000
   ```

4. **Build produksi**:
   ```bash
   npm run build
   # atau:
   gatsby build
   # output akan berada di folder ./public
   ```

5. (Opsional) **Preview produksi secara lokal**:
   ```bash
   npm run serve
   # atau:
   gatsby serve
   # buka http://localhost:9000
   ```

---

## 📦 Deploy ke Server (Nginx)

Situs ini adalah **static build**. Setelah `gatsby build`, upload isi folder `public/` ke direktori web server, contoh:

```bash
# dari mesin build
rsync -avz --delete ./public/ root@srv759669:/var/www/gumxlarcw.id/
```

### Contoh Nginx server block

```nginx
server {
  listen 80;
  server_name gumxlarcw.id www.gumxlarcw.id;

  root /var/www/gumxlarcw.id;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }

  # Cache aset statis
  location ~* \.(?:js|css|png|jpg|jpeg|gif|svg|webp|avif|woff2?)$ {
    expires 30d;
    add_header Cache-Control "public, immutable";
  }

  # Service Worker & manifest
  location = /sw.js { add_header Cache-Control "no-cache"; }
  location = /manifest.webmanifest { add_header Cache-Control "no-cache"; }
}
```

Aktifkan HTTPS (disarankan):
```bash
apt-get install -y certbot python3-certbot-nginx
certbot --nginx -d gumxlarcw.id -d www.gumxlarcw.id
```

---

## 📝 Konten & Struktur

- **Halaman utama**: `index.html`
- **Arsip tulisan/notes**: `/archive` & `/pensieve`
- **Tags**: `/pensieve/tags/<tag-name>`
- **404**: `/404/` dan `404.html`
- **OG image**: `og.png` / `og@2x.png`
- **PWA icons**: `/icons/`
- **Sitemap**: `/sitemap/sitemap-index.xml`
- **Robots**: `/robots.txt`

> Konten tulisan biasanya bersumber dari folder konten (Markdown) di repo sumber Gatsby. Tambahkan/ubah tulisan → rebuild → deploy ulang `public/`.

---

## 🔧 Variabel & Integrasi

- **Google Analytics**: sudah tertanam (property UA).
- **Meta SEO / OG / Twitter**: di-setup via `react-helmet`.
- **Manifest PWA**: `manifest.webmanifest`.

Jika ingin mematikan PWA/offline saat debugging, nonaktifkan plugin offline pada konfigurasi Gatsby, rebuild, lalu **hapus `sw.js`** di server atau lakukan:
```js
navigator.serviceWorker.getRegistrations().then(rs => rs.forEach(r => r.unregister()))
```

---

## 🤝 Kontribusi

1. Fork repo ini
2. Buat branch: `feat/nama-fitur`
3. Commit: `git commit -m "feat: deskripsi singkat"`
4. Push: `git push origin feat/nama-fitur`
5. Buka Pull Request

---

## 📄 Lisensi

Copyright © Wisnu Candra Gumelar  
Gunakan, modifikasi, dan distribusikan sesuai lisensi yang kamu pilih untuk repo ini (mis. MIT). Tambahkan file `LICENSE` bila ingin open-source.
