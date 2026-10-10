# ⚡ CodeStash — Modern Neubrutalism Code Snippet Manager

<p align="center">
  <img src="https://img.shields.io/badge/Design-Neubrutalism-FFE600?style=for-the-badge&logoColor=000&labelColor=000" alt="Neubrutalism Design" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
  <img src="https://img.shields.io/badge/Prism.js-2A2B2E?style=for-the-badge&logo=codeigniter&logoColor=white" alt="Prism.js" />
  <img src="https://img.shields.io/badge/License-MIT-black?style=for-the-badge" alt="MIT License" />
</p>

---

## 📌 Tentang CodeStash

**CodeStash** adalah aplikasi web *Single Page Application* (SPA) untuk manajemen cuplikan kode (*code snippet manager*) yang mengusung gaya desain **Neubrutalism** — estetika visual modern yang berani dengan garis tepi tebal hitam, bayangan tajam (*hard offset shadows*), palet warna pop kontras tinggi, dan animasi tombol taktil.

Dibangun dengan pendekatan arsitektur ringan tanpa build tool rumit, CodeStash memadukan kesederhanaan **Vanilla HTML/JavaScript** dengan keandalan backend **Supabase** (Autentikasi & Database PostgreSQL Realtime).

---

## ✨ Fitur Unggulan

- 🎨 **Estetika Neubrutalism Premium**
  - Border tebal 2px/3px hitam solid, bayangan tajam (*offset hard shadows*).
  - Palet warna kontras tinggi (*Electric Yellow, Cyan Accent, Bubblegum Pink*).
  - Micro-interaction taktil pada tombol dan kartu saat hover dan diklik.

- ⚡ **Kategori Cepat (Quick Language Navigation)**
  - Akses cepat filter bahasa dari Sidebar Desktop dan Drawer Mobile:
    - 🌐 **HTML & CSS** (mendukung filter cuplikan HTML dan CSS sekaligus)
    - 🟨 **JavaScript** (JS & TypeScript)
    - 🎯 **Dart / Flutter**
    - 🐘 **PHP / Laravel**
    - 🐍 **Python**
    - 🗄️ **SQL Database**

- 🔍 **Pencarian Realtime Multi-Kriteria**
  - Cari cuplikan kode secara instan berdasarkan judul, tag, atau isi kode tanpa memuat ulang halaman.
  - Filter bahasa dan pencarian teks bekerja sinkron secara simultan.

- 🔐 **Manajemen Privasi (Public & Private)**
  - **Public**: Cuplikan dapat dilihat oleh semua developer di komunitas.
  - **Private**: Cuplikan bersifat rahasia dan hanya dapat diakses oleh pemilik akun.

- 🌈 **Syntax Highlighting Otomatis**
  - Ditenagai oleh **Prism.js** dengan tampilan dark editor yang nyaman di mata untuk berbagai bahasa pemrograman.

- 📋 **One-Click Code Copy**
  - Salin baris kode ke clipboard hanya dengan satu kali klik, dilengkapi status indikator visual (*"Tersalin!"*).

- 📱 **Desain Sepenuhnya Responsif**
  - Optimal di layar smartphone, tablet, maupun monitor desktop dengan navigasi Drawer mobile yang mulus.

---

## 🛠️ Tech Stack

| Komponen | Teknologi |
| :--- | :--- |
| **Frontend Core** | HTML5, JavaScript Modern (ES6+ SPA Hash Router) |
| **Styling & CSS** | Tailwind CSS (CDN) + Custom Neubrutalism Tokens |
| **Typography** | Google Fonts (*Plus Jakarta Sans* & *Space Mono*) |
| **Icons** | Font Awesome 6 Free CDN |
| **Syntax Highlighter**| Prism.js (Core + Autoloader & Components) |
| **Backend & Auth** | Supabase (User Auth & Realtime Database) |
| **Database** | PostgreSQL (Supabase Database) |

---

## 📁 Struktur Berkas

```text
CodeStash/
├── .gitignore          # File pengabaian git
├── README.md           # Dokumentasi resmi proyek
└── index.html          # Aplikasi utama (Frontend SPA, Router, Styles, & Logic)
```

---

## 🚀 Cara Menjalankan Proyek

Tidak diperlukan instalasi package node atau build tools tambahan. Proyek ini siap dijalankan secara langsung:

### 1. Klon Repositori
```bash
git clone https://github.com/Qaidy/codestash.git
cd codestash
```

### 2. Jalankan dengan Local Server

Anda dapat membuka `index.html` langsung di browser, atau menggunakan server lokal untuk pengalaman terbaik:

- **Menggunakan Python 3:**
  ```bash
  python3 -m http.server 8000
  ```
- **Menggunakan Node.js (`npx serve`):**
  ```bash
  npx serve .
  ```
- **Menggunakan VS Code Live Server:**
  Klik kanan pada berkas `index.html` dan pilih **"Open with Live Server"**.

Akses aplikasi di browser melalui URL: `http://localhost:8000`

---

## 🗄️ Setup Database Supabase (Opsional)

Jika Anda ingin menghubungkan dengan instance Supabase milik sendiri:

1. Buat proyek baru di [Supabase Dashboard](https://supabase.com).
2. Eksekusi skrip SQL berikut di menu **SQL Editor**:

```sql
-- Buat tabel snippets
CREATE TABLE public.snippets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    language TEXT NOT NULL,
    code_content TEXT NOT NULL,
    tags TEXT[] DEFAULT '{}',
    is_public BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Aktifkan Row Level Security (RLS)
ALTER TABLE public.snippets ENABLE ROW LEVEL SECURITY;

-- Policy: Siapa saja dapat melihat snippet yang berstatus public
CREATE POLICY "Public snippets are viewable by everyone" 
ON public.snippets FOR SELECT 
USING (is_public = true);

-- Policy: Pengguna dapat melihat semua snippet miliknya (termasuk private)
CREATE POLICY "Users can view own snippets" 
ON public.snippets FOR SELECT 
USING (auth.uid() = user_id);

-- Policy: Pengguna terautentikasi dapat menambahkan snippet baru
CREATE POLICY "Authenticated users can insert snippets" 
ON public.snippets FOR INSERT 
WITH CHECK (auth.uid() = user_id);

-- Policy: Pengguna dapat menghapus snippet miliknya sendiri
CREATE POLICY "Users can delete own snippets" 
ON public.snippets FOR DELETE 
USING (auth.uid() = user_id);
```

3. Ganti konfigurasi `SUPABASE_URL` dan `SUPABASE_ANON_KEY` pada awal tag `<script>` di `index.html`:
```javascript
const SUPABASE_URL = "https://YOUR_PROJECT_ID.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_ANON_PUBLIC_KEY";
```

---

## 📄 Lisensi

Didistribusikan di bawah lisensi **MIT**. Silakan gunakan, modifikasi, dan kembangkan sesuai kebutuhan Anda.

---

<p align="center">
  Dibuat dengan ❤️ & estetika <b>Neubrutalism</b> untuk developer.
</p>
