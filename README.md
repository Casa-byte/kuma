# 🐻 KUMA – Kukis Manis

> **Freshly Baked Happiness in Every Bite**
> Website bisnis cookies premium homemade dari Jember.

---

## 📋 Daftar Isi

- [Tentang Project](#tentang-project)
- [Fitur Website](#fitur-website)
- [Cara Deploy (Langsung HTML)](#cara-deploy-langsung-html)
- [Struktur Project Next.js](#struktur-project-nextjs)
- [Instalasi Next.js Fullstack](#instalasi-nextjs-fullstack)
- [Konfigurasi Environment](#konfigurasi-environment)
- [Setup Database](#setup-database)
- [Cara Ganti Foto Produk](#cara-ganti-foto-produk)
- [Deployment Guide](#deployment-guide)
- [Kontak](#kontak)

---

## Tentang Project

KUMA adalah website bisnis cookies homemade premium yang dibangun untuk keperluan:
- ✅ Identitas brand digital
- ✅ Katalog produk interaktif
- ✅ Sistem Open PO otomatis dengan countdown
- ✅ Informasi nutrisi dengan visualisasi
- ✅ Smart Packaging dengan simulasi QR Code
- ✅ Media promosi & pemesanan via WhatsApp / Instagram

---

## Fitur Website

| Fitur | Deskripsi |
|---|---|
| 🎨 Dark Mode | Toggle otomatis, menyimpan preferensi |
| 📱 Responsive | Mobile, tablet, desktop ready |
| 🍪 Loading Animation | Animasi cookies saat halaman dimuat |
| ✨ Floating Particles | Partikel cookies yang bergerak pelan |
| 🍔 Side Menu | Hamburger menu dengan animasi smooth |
| ⏰ Countdown PO | Status open/close otomatis berdasarkan hari & jam |
| 📊 Nutrition Infographic | Bar animasi kandungan gizi |
| 📱 QR Code Modal | Simulasi scan kemasan produk |
| 💬 FAQ Accordion | Pertanyaan umum interaktif |
| 🟢 Floating WhatsApp | Tombol WA yang selalu terlihat |
| 🔍 SEO Ready | Meta tags lengkap |
| 🌀 Scroll Animation | Reveal animation saat scroll |

---

## Cara Deploy (Langsung HTML)

File `index.html` adalah **single-file website** yang bisa langsung digunakan tanpa instalasi apapun.

### Option 1 – Netlify Drop (Paling Mudah)
1. Buka [netlify.com/drop](https://netlify.com/drop)
2. Drag & drop folder `kuma-website/`
3. Selesai! Website langsung online ✅

### Option 2 – GitHub Pages
```bash
# 1. Buat repo baru di GitHub
# 2. Upload folder kuma-website/
# 3. Settings > Pages > Deploy from branch main
# 4. Pilih folder / (root)
```

### Option 3 – Vercel
```bash
npm install -g vercel
cd kuma-website/
vercel --prod
```

---

## Struktur Project Next.js

Jika ingin mengembangkan ke fullstack Next.js:

```
kuma-nextjs/
├── app/
│   ├── layout.tsx           # Root layout
│   ├── page.tsx             # Homepage
│   ├── globals.css          # Global styles
│   └── api/
│       ├── products/
│       │   └── route.ts     # GET /api/products
│       ├── po-status/
│       │   └── route.ts     # GET /api/po-status
│       └── contact/
│           └── route.ts     # POST /api/contact
├── components/
│   ├── Navbar.tsx
│   ├── Hero.tsx
│   ├── Products.tsx
│   ├── OpenPO.tsx
│   ├── Nutrition.tsx
│   ├── Sustainability.tsx
│   ├── SmartPackaging.tsx
│   ├── Testimonials.tsx
│   ├── Contact.tsx
│   ├── Footer.tsx
│   └── ui/
│       ├── Button.tsx
│       ├── Card.tsx
│       └── Badge.tsx
├── lib/
│   ├── db.ts               # Prisma client
│   └── utils.ts            # Helper functions
├── prisma/
│   ├── schema.prisma       # Database schema
│   └── seed.ts             # Data seeding
├── public/
│   └── images/
│       └── products/
│           ├── choco-cookies.jpg       # ← Taruh foto di sini
│           ├── red-velvet-cookies.jpg
│           ├── matcha-cookies.jpg
│           └── triple-choco-cookies.jpg
├── .env                    # Environment variables
├── .env.example
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## Instalasi Next.js Fullstack

```bash
# 1. Buat project Next.js baru
npx create-next-app@latest kuma-nextjs \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --src-dir=false

cd kuma-nextjs

# 2. Install dependencies
npm install prisma @prisma/client
npm install --save-dev @types/node

# 3. Init Prisma
npx prisma init --datasource-provider postgresql

# 4. Install optional dependencies
npm install next-themes    # Dark mode
npm install framer-motion  # Animations
npm install react-countup  # Number counters
npm install react-hot-toast # Toast notifications

# 5. Jalankan development
npm run dev
```

---

## Konfigurasi Environment

Buat file `.env` di root project:

```env
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/kuma_db?schema=public"

# App
NEXT_PUBLIC_APP_URL="http://localhost:3000"
NEXT_PUBLIC_WA_NUMBER="6289515796458"
NEXT_PUBLIC_IG_URL="https://www.instagram.com/albar_aaziz"

# Optional: Jika pakai Supabase
NEXT_PUBLIC_SUPABASE_URL="your-supabase-url"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-supabase-key"
```

---

## Setup Database

### Prisma Schema (`prisma/schema.prisma`)

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Product {
  id          String   @id @default(cuid())
  name        String
  slug        String   @unique
  description String
  price       Int
  calories    Int
  imageUrl    String?
  status      ProductStatus @default(COMING_SOON)
  category    String
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

enum ProductStatus {
  AVAILABLE
  COMING_SOON
  OUT_OF_STOCK
}

model POSchedule {
  id         String   @id @default(cuid())
  startDate  DateTime
  endDate    DateTime
  isActive   Boolean  @default(true)
  note       String?
  createdAt  DateTime @default(now())
}

model Testimonial {
  id        String   @id @default(cuid())
  name      String
  location  String?
  rating    Int      @default(5)
  message   String
  avatar    String?
  isActive  Boolean  @default(true)
  createdAt DateTime @default(now())
}
```

### Jalankan Migration

```bash
# Generate client
npx prisma generate

# Buat migration
npx prisma migrate dev --name init

# Seed data
npx prisma db seed
```

### Seed Data (`prisma/seed.ts`)

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

async function main() {
  // Products
  await prisma.product.createMany({
    data: [
      {
        name: 'Choco Cookies',
        slug: 'choco-cookies',
        description: 'Cookies coklat premium dengan dark chocolate chip pilihan.',
        price: 35000,
        calories: 180,
        status: 'AVAILABLE',
        category: 'classic',
      },
      {
        name: 'Red Velvet Cookies',
        slug: 'red-velvet-cookies',
        description: 'Perpaduan warna merah elegan dengan cream cheese.',
        price: 40000,
        calories: 190,
        status: 'COMING_SOON',
        category: 'premium',
      },
      {
        name: 'Matcha Cookies',
        slug: 'matcha-cookies',
        description: 'Kesegaran matcha premium bertemu cookies renyah.',
        price: 38000,
        calories: 165,
        status: 'COMING_SOON',
        category: 'premium',
      },
      {
        name: 'Triple Choco Cookies',
        slug: 'triple-choco-cookies',
        description: 'Tiga lapis dark, milk, dan white chocolate.',
        price: 45000,
        calories: 220,
        status: 'COMING_SOON',
        category: 'premium',
      },
    ],
  })

  // Testimonials
  await prisma.testimonial.createMany({
    data: [
      {
        name: 'Sari Dewi',
        location: 'Jember',
        rating: 5,
        message: 'Cookiesnya enak banget! Crispy di luar, chewy di dalam.',
      },
      {
        name: 'Rizky Pratama',
        location: 'Surabaya',
        rating: 5,
        message: 'Buat hampers ulang tahun, semua orang suka!',
      },
      {
        name: 'Nanda Putri',
        location: 'Malang',
        rating: 5,
        message: 'QR Code di kemasannya keren banget! KUMA beneran inovatif.',
      },
    ],
  })

  console.log('✅ Seed data berhasil!')
}

main()
  .catch(console.error)
  .finally(() => prisma.$disconnect())
```

---

## Cara Ganti Foto Produk

### Untuk versi HTML (index.html):

Cari komentar ini di dalam kode:
```html
<!-- Replace this emoji with:
  <img src="images/products/choco-cookies.jpg" alt="Choco Cookies KUMA">
  Place image in: public/images/products/choco-cookies.jpg
-->
```

Ganti emoji 🍪 dengan:
```html
<img src="images/products/choco-cookies.jpg" alt="Choco Cookies KUMA" loading="lazy">
```

Lalu upload foto ke folder `images/products/` dengan nama:
- `choco-cookies.jpg` ← Foto cookies coklat premium
- `red-velvet-cookies.jpg`
- `matcha-cookies.jpg`
- `triple-choco-cookies.jpg`

**Tips foto produk:**
- Ukuran ideal: 800×600 px atau 1:1 ratio
- Format: JPG atau WebP (lebih ringan)
- Maksimal ukuran file: 500KB per foto
- Background putih atau kraft paper sangat cocok

---

## Deployment Guide

### Netlify

```toml
# netlify.toml (untuk static HTML)
[build]
  publish = "."

[[headers]]
  for = "/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000"
```

### Vercel (untuk Next.js)

```json
// vercel.json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install"
}
```

### Railway (Database PostgreSQL)

1. Buat project baru di [railway.app](https://railway.app)
2. Add PostgreSQL service
3. Copy `DATABASE_URL` dari Railway
4. Set sebagai environment variable di Vercel

---

## Kontak

**KUMA – Kukis Manis**

- 💬 WhatsApp: [+62 895-1579-6458](https://wa.me/6289515796458)
- 📸 Instagram: [@albar_aaziz](https://www.instagram.com/albar_aaziz)
- 📍 Lokasi: Jl. Slamet Riyadi III, No. 2, Patrang, Jember

---

*Made with 🤎 for KUMA – Kukis Manis | © 2024*
