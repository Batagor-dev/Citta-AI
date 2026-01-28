# 🎓 Sistem Rekomendasi Jurusan SMK Berbasis AI

## 📌 Tentang Repository Ini

Repository ini berisi source code **Sistem Rekomendasi Jurusan SMK Berbasis AI** yang dirancang untuk membantu siswa SMP menentukan jurusan SMK yang paling sesuai berdasarkan **minat, kebiasaan, dan kecenderungan kepribadian**.

Sistem ini menggunakan **Artificial Intelligence (LLM)** sebagai *decision support system*, bukan sebagai penentu keputusan mutlak. Semua hasil AI **divalidasi oleh backend** dan dicocokkan dengan data jurusan serta sekolah yang tersedia di database.

---

## 🎯 Tujuan Pengembangan

Tujuan utama pengembangan sistem ini adalah:

* Membantu siswa SMP mengenali potensi dan minat diri
* Memberikan rekomendasi jurusan SMK yang rasional dan terukur
* Menyediakan daftar sekolah yang memiliki jurusan terkait
* Menerapkan AI secara **terkontrol, akademis, dan bertanggung jawab**
* Menjadi studi kasus implementasi AI di bidang pendidikan

---

## 🧠 Konsep Sistem

Alur kerja sistem secara umum:

1. Siswa mengisi kuisioner (minat, kebiasaan, preferensi)
2. Data dikirim ke backend (NestJS)
3. Backend memproses dan menyusun prompt
4. AI menganalisis kecenderungan siswa
5. AI menghasilkan rekomendasi jurusan + skor kecocokan (JSON)
6. Backend memvalidasi hasil AI dengan database
7. Sistem mengambil data sekolah terkait
8. Hasil akhir ditampilkan ke pengguna

AI **tidak pernah langsung menentukan hasil akhir**, seluruh keputusan tetap dikontrol oleh sistem.

---

## 🛠️ Teknologi yang Digunakan

### Frontend

* Next.js 14 (App Router)
* TypeScript
* React
* (Opsional) Tailwind CSS

### Backend

* NestJS
* TypeScript
* REST API

### Artificial Intelligence

* OpenRouter API
* Model LLM (GPT / Claude / LLaMA)

### Database

* PostgreSQL / MySQL
* ORM: Prisma

---

## 🧩 Arsitektur Sistem

```mermaid
flowchart LR
    A[User / Siswa] --> B[Next.js Frontend]
    B --> C[NestJS Backend API]

    C --> D[AI Service - OpenRouter]
    C --> E[Database Jurusan & Sekolah]

    D --> C
    E --> C

    C --> B
    B --> A
```

---

## 🗂️ Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    STUDENT ||--o{ ANALYSIS_RESULT : has
    ANALYSIS_RESULT ||--o{ ANALYSIS_RECOMMENDATION : generates
    MAJOR ||--o{ ANALYSIS_RECOMMENDATION : recommended
    SCHOOL ||--o{ SCHOOL_MAJOR : has
    MAJOR ||--o{ SCHOOL_MAJOR : offered_by

    STUDENT {
        int id PK
        string name
        string gender
        date birth_date
        datetime created_at
    }

    ANALYSIS_RESULT {
        int id PK
        int student_id FK
        text personality_summary
        datetime created_at
    }

    ANALYSIS_RECOMMENDATION {
        int id PK
        int analysis_result_id FK
        int major_id FK
        int score
    }

    MAJOR {
        int id PK
        string name
        string description
    }

    SCHOOL {
        int id PK
        string name
        string address
        string city
    }

    SCHOOL_MAJOR {
        int id PK
        int school_id FK
        int major_id FK
    }
```

---

## 🔄 Flowchart Proses Sistem

```mermaid
flowchart TD
    A[Mulai] --> B[Siswa Mengisi Kuisioner]
    B --> C{Input Valid?}

    C -- Tidak --> B
    C -- Ya --> D[Frontend Kirim Data ke Backend]

    D --> E[NestJS Backend]
    E --> F[Preprocessing Jawaban]

    F --> G[Kirim Prompt ke AI via OpenRouter]
    G --> H[AI Analisis Kepribadian]

    H --> I[AI Rekomendasi Jurusan + Skor]
    I --> J[Output JSON]

    J --> K[Validasi Backend]
    K --> L{Jurusan Valid?}

    L -- Tidak --> K
    L -- Ya --> M[Query Database Sekolah]

    M --> N[Susun Rekomendasi Final]
    N --> O[Tampilkan Hasil]
    O --> P[Selesai]
```

---

## 📂 Struktur Folder (Garis Besar)

```bash
root
├─ frontend/            # Next.js Frontend
│  ├─ app/
│  └─ components/
│
├─ backend/             # NestJS Backend
│  ├─ src/
│  │  ├─ ai/            # Integrasi OpenRouter
│  │  ├─ major/         # Master Jurusan
│  │  ├─ school/        # Master Sekolah
│  │  ├─ school-major/  # Relasi sekolah & jurusan
│  │  └─ analysis/      # Analisis & rekomendasi
│  └─ main.ts
│
└─ README.md
```

---

## 🔰 Tahapan Pengembangan Sistem

### ⚙️ Tahap 1 — Setup Backend Core

* Init NestJS Project
* ENV Configuration (Database & OpenRouter)
* Setup Prisma ORM
* Prisma Service & Module

**Output:** Backend siap query database

---

### 🗃️ Tahap 2 — Data Master

* Seeder Jurusan (Major)
* Seeder Sekolah (School)
* Seeder Relasi School-Major
* CRUD Master Data

Endpoint minimal:

* `GET /majors`
* `GET /schools?majorId=`

**Output:** Data master siap digunakan

---

### 🧠 Tahap 3 — Analysis System (Tanpa AI)

* Module analysis
* Simpan data siswa & jawaban
* Simpan hasil analisis dummy
* Validasi DTO & error handling

**Output:** Flow sistem berjalan stabil

---

### 🤖 Tahap 4 — Integrasi AI

* Integrasi OpenRouter
* Prompt terkontrol
* Output wajib JSON
* Validasi hasil AI

**Output:** AI aman dan akademis

---

### 🎨 Tahap 5 — Frontend

* Setup Next.js App Router
* Form kuisioner siswa
* Halaman hasil rekomendasi

**Output:** Sistem bisa digunakan user

---

### 🚀 Tahap 6 — Polishing & Akademis

* Penjelasan hasil rekomendasi
* Disclaimer AI
* Dokumentasi & README

**Output:** Layak presentasi dan akademik

---

## 🧾 Ringkasan Pegangan

> **Konsep → ENV → DB → Seeder → CRUD → Analysis → AI → Frontend**

---

## 📌 Catatan Penting

* Sistem ini **tidak menggantikan konselor pendidikan**
* AI bersifat pendukung analisis
* Semua hasil AI divalidasi backend

---

## 🚀 Pengembangan Lanjutan

* Akun siswa & histori analisis
* Dashboard admin
* Visualisasi skor
* Perbandingan multi-model AI
* Export hasil (PDF)

---

## 📄 Lisensi

Project ini dikembangkan untuk keperluan **edukasi dan akademik**.

---

## 🙌 Penutup

Project ini diharapkan menjadi contoh penerapan AI yang **etis, terkontrol, dan bermanfaat** dalam membantu siswa menentukan jurusan pendidikan yang sesuai dengan potensi mereka.
