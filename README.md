# ai-rpm-generator
AI-powered lesson plan generator for Indonesian curriculum
┌─────────────────────────────────────────────────────────────┐
│                     FRONTEND (Next.js 14)                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │   React     │  │ TailwindCSS │  │   shadcn/ui         │  │
│  │   Server    │  │   + Framer  │  │   (Components)      │  │
│  │  Components │  │   Motion    │  │                     │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
└──────────────────────────┬──────────────────────────────────┘
                           │ API Routes / tRPC
┌──────────────────────────▼──────────────────────────────────┐
│                    BACKEND (Next.js API)                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │  NextAuth   │  │   OpenAI    │  │   Puppeteer/        │  │
│  │    Auth     │  │    SDK      │  │   docx-templates    │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
└──────────────────────────┬──────────────────────────────────┘
                           │ Prisma ORM
┌──────────────────────────▼──────────────────────────────────┐
│              DATABASE (PostgreSQL via Supabase)             │
│              + Redis (Cache) + Storage (PDF/DOCX)           │
└─────────────────────────────────────────────────────────────┘
ai-rpm-generator/
├── apps/
│   └── web/                          # Next.js 14 App Router
│       ├── app/
│       │   ├── (auth)/               # Route group: Autentikasi
│       │   │   ├── login/
│       │   │   └── register/
│       │   ├── (dashboard)/          # Route group: Dashboard
│       │   │   ├── dashboard/
│       │   │   ├── rpm/
│       │   │   │   ├── new/          # Buat RPM baru
│       │   │   │   ├── [id]/         # Detail RPM
│       │   │   │   └── edit/[id]/    # Edit RPM
│       │   │   ├── history/          # Riwayat RPM
│       │   │   └── templates/        # Template RPM
│       │   ├── api/                  # API Routes
│       │   │   ├── auth/             # NextAuth config
│       │   │   ├── rpm/              # CRUD RPM
│       │   │   ├── ai/               # AI Generation endpoints
│       │   │   ├── export/           # PDF/DOCX export
│       │   │   └── share/            # Share link generation
│       │   ├── layout.tsx
│       │   └── page.tsx
│       ├── components/
│       │   ├── ui/                   # shadcn/ui components
│       │   ├── rpm/                  # Komponen spesifik RPM
│       │   │   ├── GeneratorForm.tsx
│       │   │   ├── PreviewPanel.tsx
│       │   │   ├── CPSelector.tsx
│       │   │   ├── LearningSteps.tsx
│       │   │   ├── AssessmentRubric.tsx
│       │   │   └── LKPDViewer.tsx
│       │   ├── ai/                   # Komponen AI Chat
│       │   │   ├── AIAssistant.tsx
│       │   │   ├── ChatInterface.tsx
│       │   │   └── SuggestionChips.tsx
│       │   └── export/
│       │       ├── ExportButton.tsx
│       │       ├── SignaturePad.tsx
│       │       └── ShareModal.tsx
│       ├── lib/
│       │   ├── prisma.ts             # Database client
│       │   ├── openai.ts             # AI configuration
│       │   ├── gemini.ts             # Alternative AI
│       │   ├── prompts/              # System prompts
│       │   │   ├── rpm-generator.ts
│       │   │   └── rpm-assistant.ts
│       │   └── utils/
│       │       ├── cp-data.ts        # Data CP Kemendikbud
│       │       └── validators.ts
│       ├── hooks/
│       │   ├── useRPM.ts
│       │   ├── useAIChat.ts
│       │   └── useExport.ts
│       ├── types/
│       │   └── index.ts              # TypeScript definitions
│       ├── public/
│       │   └── templates/            # Template dokumen
│       └── package.json
│
├── packages/
│   ├── shared-types/                 # Shared TypeScript types
│   ├── eslint-config/
│   └── typescript-config/
│
├── prisma/
│   └── schema.prisma                 # Database schema
│
├── docker-compose.yml
├── turbo.json                        # Monorepo config
└── README.md
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// ==================== USER MANAGEMENT ====================
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  role          UserRole  @default(TEACHER)
  institution   String?   // Nama sekolah
  nip           String?   // Nomor Induk Pegawai
  subjects      String[]  // Mata pelajaran yang diampu
  
  // Relations
  rpmHistory    RPMHistory[]
  sharedLinks   SharedLink[]
  chatSessions  ChatSession[]
  
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

enum UserRole {
  TEACHER
  SCHOOL_ADMIN
  SUPER_ADMIN
}

// ==================== RPM CORE ====================
model RPMHistory {
  id              String   @id @default(cuid())
  userId          String
  user            User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  // Metadata Pembelajaran
  educationLevel  EducationLevel    // PAUD, SD, SMP, SMA, SMK
  grade           String            // Kelas (1, 2, 3, dst)
  subject         String            // Mata pelajaran
  topic           String            // Topik/Materi
  duration        Int               // Durasi (JP)
  
  // Capaian Pembelajaran (CP)
  cpElement       String            // Elemen CP
  cpCode          String            // Kode CP (e.g., "3.1")
  cpDescription   String            // Deskripsi CP
  
  // Konten RPM (JSON untuk fleksibilitas)
  content         Json              // Struktur lengkap RPM
  
  // Status & Sharing
  status          RPMStatus @default(DRAFT)
  shareToken      String?   @unique
  shareExpiry     DateTime?
  
  // Digital Signatures
  teacherSignature    String?       // URL/Hash tanda tangan guru
  principalSignature  String?       // URL/Hash tanda tangan kepala sekolah
  signedAt            DateTime?
  
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt
  
  @@index([userId])
  @@index([shareToken])
}

enum EducationLevel {
  PAUD
  SD
  SMP
  SMA
  SMK
}

enum RPMStatus {
  DRAFT
  COMPLETED
  SHARED
  ARCHIVED
}

// ==================== AI CHAT SESSIONS ====================
model ChatSession {
  id          String   @id @default(cuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  rpmId       String?  // Optional: terkait dengan RPM tertentu
  context     Json     // Konteks chat (elemen CP, topik, dll)
  
  messages    ChatMessage[]
  
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

model ChatMessage {
  id              String   @id @default(cuid())
  sessionId       String
  session         ChatSession @relation(fields: [sessionId], references: [id], onDelete: Cascade)
  
  role            MessageRole
  content         String
  metadata        Json?    // Token usage, model used, etc.
  
  createdAt       DateTime @default(now())
}

enum MessageRole {
  SYSTEM
  USER
  ASSISTANT
}

// ==================== SHARED LINKS ====================
model SharedLink {
  id          String   @id @default(cuid())
  token       String   @unique
  rpmId       String
  rpm         RPMHistory @relation(fields: [rpmId], references: [id], onDelete: Cascade)
  
  createdBy   String
  creator     User     @relation(fields: [createdBy], references: [id])
  
  expiresAt   DateTime?
  viewCount   Int      @default(0)
  lastViewed  DateTime?
  
  createdAt   DateTime @default(now())
}

// ==================== TEMPLATES & REFERENCES ====================
model CurriculumReference {
  id          String   @id @default(cuid())
  level       EducationLevel
  subject     String
  element     String   // Elemen
  cpCode      String   // Kode CP
  description String   // Deskripsi lengkap
  
  // Struktur Kurikulum
  learningOutcomes String[] // Tujuan Pembelajaran (TP)
  achievementIndicators String[] // IPK
  
  @@unique([level, subject, cpCode])
}
// lib/prompts/rpm-generator.ts

export const RPM_SYSTEM_PROMPT = `Anda adalah **AI Curriculum Designer** yang berspesialisasi dalam membuat Rancangan Pembelajaran Mendalam (RPM) sesuai dengan standar Kementerian Pendidikan, Kebudayaan, Riset, dan Teknologi (Kemendikbudristek) Republik Indonesia.

## KONTEKS PROFESIONAL
Anda bekerja sama dengan guru-guru Indonesia untuk merancang pembelajaran yang:
- Mengacu pada **Kurikulum Merdeka** terbaru
- Menerapkan **pendekatan pembelajaran mendalam** (Deep Learning)
- Mengintegrasikan **profil pelajar Pancasila**
- Menggunakan **pendekatan saintifik** (5M: Mengamati, Menanya, Mencoba, Menalar, Mengkomunikasikan)

## STRUKTUR OUTPUT WAJIB

Buatlah RPM dengan struktur JSON berikut:

\`\`\`json
{
  "metadata": {
    "satuan_pendidikan": "[string]",
    "fase": "[string: A, B, C, D, E, atau F]",
    "kelas": "[string]",
    "mata_pelajaran": "[string]",
    "materi": "[string]",
    "alokasi_waktu": "[string: X JP (@Y menit)]",
    "capaian_pembelajaran": {
      "kode": "[string]",
      "elemen": "[string]",
      "deskripsi": "[string]"
    }
  },
  "profil_pelajar_pancasila": [
    "Beriman, Bertakwa kepada Tuhan YME, dan Berakhlak Mulia",
    "Mandiri",
    "Bergotong Royong",
    "Kreatif",
    "Bernalar Kritis"
  ],
  "tujuan_pembelajaran": [
    {
      "kode": "TP 1",
      "deskripsi": "[Deskripsi spesifik, terukur, mencakup aspek pengetahuan]",
      "indikator_pencapaian": ["[IPK 1.1]", "[IPK 1.2]"]
    },
    {
      "kode": "TP 2",
      "deskripsi": "[Deskripsi spesifik, terukur, mencakup aspek keterampilan]",
      "indikator_pencapaian": ["[IPK 2.1]", "[IPK 2.2]"]
    },
    {
      "kode": "TP 3",
      "deskripsi": "[Deskripsi spesifik, terukur, mencakup aspek sikap/afektif]",
      "indikator_pencapaian": ["[IPK 3.1]"]
    }
  ],
  "pemetaan_materi": {
    "konsep_utama": ["[Konsep 1]", "[Konsep 2]"],
    "prasyarat": ["[Prasyarat 1]", "[Prasyarat 2]"],
    "pengayaan": ["[Topik pengayaan]"]
  },
  "model_pembelajaran": "Project-Based Learning (PjBL) / Problem-Based Learning (PBL) / Discovery Learning / Inquiry Learning",
  "langkah_pembelajaran": {
    "pendahuluan": {
      "durasi": "10-15 menit",
      "kegiatan": [
        "Guru menyampaikan tujuan pembelajaran dan IPK",
        "Guru melakukan apersepsi/mengaitkan dengan pengetahuan sebelumnya",
        "Guru memotivasi peserta didik melalui [contekstualisasi spesifik]",
        "Guru menjelaskan rangkaian kegiatan pembelajaran"
      ]
    },
    "inti": {
      "durasi": "50-60 menit",
      "fase_1_mengamati": {
        "deskripsi": "[Kegiatan mengamati fenomena/konteks nyata]",
        "alamat_materi": "[Detail aktivitas]",
        "estimasi_waktu": "10 menit"
      },
      "fase_2_menanya": {
        "deskripsi": "[Kegiatan menanya/menghipotesis]",
        "alamat_materi": "[Pertanyaan pemantik]",
        "estimasi_waktu": "10 menit"
      },
      "fase_3_mencoba": {
        "deskripsi": "[Kegiatan eksplorasi/eksperimen/praktik]",
        "alamat_materi": "[Langkah-langkah praktik]",
        "estimasi_waktu": "20 menit"
      },
      "fase_4_menalar": {
        "deskripsi": "[Kegiatan analisis, asosiasi, diskusi kelompok]",
        "alamat_materi": "[Panduan analisis]",
        "estimasi_waktu": "15 menit"
      },
      "fase_5_mengkomunikasikan": {
        "deskripsi": "[Presentasi hasil kerja/refleksi]",
        "alamat_materi": "[Format presentasi]",
        "estimasi_waktu": "10 menit"
      }
    },
    "penutup": {
      "durasi": "10-15 menit",
      "kegiatan": [
        "Guru bersama peserta didik menyimpulkan materi pembelajaran",
        "Guru melakukan refleksi proses dan hasil pembelajaran",
        "Guru memberikan umpan balik dan penghargaan",
        "Guru menyampaikan rencana tindak lanjut (pembelajaran selanjutnya)"
      ]
    }
  },
  "asesmen": {
    "formatif": {
      "teknik": "Observasi, Tes Lisan, Tes Tertulis, Praktik",
      "instrumen": [
        {
          "nama": "Lembar Observasi Sikap",
          "deskripsi": "[Deskripsi penggunaan]",
          "kriteria": ["[Kriteria 1]", "[Kriteria 2]"]
        },
        {
          "nama": "Kuis/Pertanyaan Pendek",
          "deskripsi": "[3-5 pertanyaan singkat]",
          "kriteria": ["[Kunci jawaban singkat]"]
        }
      ]
    },
    "sumatif": {
      "teknik": "Tes Tertulis, Proyek, Praktik",
      "instrumen": [
        {
          "nama": "Rubrik Penilaian Proyek",
          "aspek": [
            {"nama": "[Aspek 1]", "bobot": 30, "kriteria": "[Deskripsi]"},
            {"nama": "[Aspek 2]", "bobot": 40, "kriteria": "[Deskripsi]"},
            {"nama": "[Aspek 3]", "bobot": 30, "kriteria": "[Deskripsi]"}
          ]
        }
      ]
    }
  },
  "lkpd": {
    "judul": "[Judul Lembar Kerja]",
    "kompetensi_dasar": "[KD terkait]",
    "tujuan": "[Tujuan LKPD]",
    "langkah_kerja": [
      "[Langkah 1: Instruksi detail]",
      "[Langkah 2: Instruksi detail]",
      "[Langkah 3: Instruksi detail]"
    ],
    "pertanyaan_diskusi": [
      "[Pertanyaan reflektif 1]",
      "[Pertanyaan reflektif 2]"
    ],
    "penilaian_mandiri": {
      "skala": "1-4 (Belum → Sangat Baik)",
      "aspek": ["[Aspek penilaian diri 1]", "[Aspek penilaian diri 2]"]
    }
  },
  "media_alat_bahan": [
    "[Media/alat/bahan spesifik]",
    "[Media/alat/bahan spesifik]"
  ],
  "sumber_belajar": [
    "[Buku teks Kemendikbud]",
    "[Sumber digital/website]",
    "[Bahan lokal/lingkungan sekitar]"
  ],
  "refleksi_guru": {
    "keberhasilan": "[Indikator keberhasilan]",
    "kendala": "[Kendala yang mungkin dihadapi]",
    "solusi": "[Solusi alternatif]",
    "tindak_lanjut": "[Rencana perbaikan]"
  }
}
\`\`\`

## ATURAN PENTING

1. **Kesesuaian CP**: Pastikan semua elemen merujuk pada CP yang diberikan dengan tepat
2. **Kontekstualisasi**: Sesuaikan contoh dan konteks dengan jenjang pendidikan (PAUD/SD/SMP/SMA/SMK)
3. **Kebahasaan**: Gunakan bahasa Indonesia baku, istilah pendidikan yang tepat
4. **Kelengkapan**: Jangan ada bagian yang kosong atau placeholder generik
5. **Aplikatif**: Langkah pembelajaran harus realistis dan dapat diimplementasikan
6. **Diferensiasi**: Sertakan opsi untuk peserta didik dengan kebutuhan khusus

## PROSES KERJA
1. Analisis CP yang diberikan
2. Identifikasi karakteristik peserta didik berdasarkan jenjang
3. Rancang pembelajaran yang mengintegrasikan 5M
4. Pastikan asesmen mencakup ranah sikap, pengetahuan, dan keterampilan
5. Generate output JSON lengkap`;

export const RPM_USER_PROMPT_TEMPLATE = (
  level: string,
  subject: string,
  topic: string,
  cpCode: string,
  cpDescription: string,
  duration: number
) => `Buatkan Rancangan Pembelajaran Mendalam (RPM) dengan spesifikasi:

**Identitas Pembelajaran:**
- Jenjang: ${level}
- Mata Pelajaran: ${subject}
- Topik/Materi: ${topic}
- Alokasi Waktu: ${duration} JP
- Capaian Pembelajaran: ${cpCode} - ${cpDescription}

**Instruksi Khusus:**
1. Sesuaikan kedalaman materi dengan jenjang ${level}
2. Gunakan model pembelajaran yang paling relevan dengan materi "${topic}"
3. Pastikan langkah-langkah pembelajaran detail dan operasional
4. Sertakan LKPD yang menarik dan sesuai karakteristik peserta didik ${level}
5. Rubrik penilaian harus objektif dan terukur

Generate output dalam format JSON sesuai struktur yang telah ditentukan.`;
// lib/prompts/rpm-assistant.ts

export const ASSISTANT_SYSTEM_PROMPT = `Anda adalah **AI Teaching Assistant** bernama "Guru AI" yang membantu para pendidik di Indonesia dalam mengembangkan dan menyempurnakan Rancangan Pembelajaran Mendalam (RPM).

## KARAKTER PROFESIONAL
- **Nama**: Guru AI
- **Peran**: Konsultan Pendidikan & Kurikulum
- **Gaya Komunikasi**: Profesional namun ramah, menggunakan bahasa Indonesia yang baik dan benar, sesekali menggunakan istilah pedagogis yang tepat
- **Pendekatan**: Kolaboratif, memberikan saran konstruktif, menghargai konteks lokal pendidikan Indonesia

## KEMAMPUAN UTAMA

1. **Konsultasi RPM**: 
   - Menjelaskan elemen-elemen RPM
   - Memberikan saran perbaikan untuk TP, langkah pembelajaran, atau asesmen
   - Membantu menyesuaikan RPM untuk peserta didik berkebutuhan khusus

2. **Strategi Pembelajaran**:
   - Menjelaskan berbagai model pembelajaran (PjBL, PBL, Inquiry, dll)
   - Memberikan contoh aktivitas pembelajaran yang engaging
   - Saran diferensiasi pembelajaran

3. **Asesmen & Evaluasi**:
   - Membantu merancang instrumen penilaian
   - Menjelaskan teknik asesmen formatif dan sumatif
   - Memberikan contoh rubrik penilaian

4. **Analisis Kurikulum**:
   - Membantu memahami Capaian Pembelajaran (CP) dan Tujuan Pembelajaran (TP)
   - Menjelaskan alur tujuan pembelajaran
   - Memetakan CP ke dalam pembelajaran konkret

## BATASAN

- Fokus pada pendidikan formal jenjang PAUD, SD, SMP, SMA, SMK
- Selalu merujuk pada Kurikulum Merdeka Kemendikbudristek
- Tidak memberikan saran yang bertentangan dengan nilai-nilai Pancasila dan Kode Etik Guru Indonesia
- Tidak membuat konten yang bersifat politis, SARA, atau tidak mendidik

## KONTEKS SEKARANG

Anda sedang berdiskusi dengan seorang guru yang sedang menyusun atau menyempurnakan RPM. Gunakan konteks berikut untuk memberikan respons yang relevan:

Konteks RPM Saat Ini:
- Jenjang: {educationLevel}
- Mata Pelajaran: {subject}
- Topik: {topic}
- CP: {cpCode} - {cpDescription}

Riwayat Percakapan:
{chatHistory}

Bantu guru dengan memberikan respons yang:
1. **Spesifik**: Jangan terlalu umum, berikan contoh konkret
2. **Aplikatif**: Langsung bisa diimplementasikan
3. **Berdasarkan Bukti**: Rujuk pada teori pembelajaran atau kurikulum yang relevan
4. **Konstruktif**: Jika ada yang perlu diperbaiki, sampaikan dengan solusi

Respons Anda:`;
// components/rpm/GeneratorForm.tsx
'use client';

import { useState } from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Textarea } from '@/components/ui/textarea';
import { Loader2, Sparkles } from 'lucide-react';
import { CP_SELECTOR_DATA } from '@/lib/utils/cp-data';

const generatorSchema = z.object({
  educationLevel: z.enum(['PAUD', 'SD', 'SMP', 'SMA', 'SMK']),
  grade: z.string(),
  subject: z.string(),
  topic: z.string().min(3, 'Topik harus diisi'),
  duration: z.number().min(1).max(20),
  cpElement: z.string(),
  cpCode: z.string(),
  cpDescription: z.string(),
  additionalContext: z.string().optional(),
});

type GeneratorFormData = z.infer<typeof generatorSchema>;

interface GeneratorFormProps {
  onGenerate: (data: GeneratorFormData) => Promise<void>;
  isGenerating: boolean;
}

export function GeneratorForm({ onGenerate, isGenerating }: GeneratorFormProps) {
  const [selectedLevel, setSelectedLevel] = useState<string>('');
  
  const { register, handleSubmit, watch, setValue, formState: { errors } } = useForm<GeneratorFormData>({
    resolver: zodResolver(generatorSchema),
  });

  const watchLevel = watch('educationLevel');
  const watchSubject = watch('subject');

  // Filter CP berdasarkan jenjang dan mata pelajaran
  const availableCPs = selectedLevel && watchSubject 
    ? CP_SELECTOR_DATA[selectedLevel]?.[watchSubject] || []
    : [];

  return (
    <Card className="w-full max-w-4xl mx-auto border-2 border-blue-100 shadow-lg">
      <CardHeader className="bg-gradient-to-r from-blue-600 to-indigo-600 text-white rounded-t-lg">
        <CardTitle className="text-2xl flex items-center gap-2">
          <Sparkles className="w-6 h-6" />
          Generator RPM AI
        </CardTitle>
        <p className="text-blue-100 mt-2">
          Isi detail pembelajaran untuk menghasilkan Rancangan Pembelajaran Mendalam yang komprehensif
        </p>
      </CardHeader>
      
      <CardContent className="p-6 space-y-6">
        <form onSubmit={handleSubmit(onGenerate)} className="space-y-6">
          
          {/* Bagian 1: Identitas Pembelajaran */}
          <div className="space-y-4">
            <h3 className="text-lg font-semibold text-gray-800 border-b pb-2">
              1. Identitas Pembelajaran
            </h3>
            
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div className="space-y-2">
                <Label htmlFor="educationLevel">Jenjang Pendidikan</Label>
                <Select 
                  onValueChange={(val) => {
                    setValue('educationLevel', val as any);
                    setSelectedLevel(val);
                  }}
                >
                  <SelectTrigger>
                    <SelectValue placeholder="Pilih jenjang" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="PAUD">PAUD (Fase A)</SelectItem>
                    <SelectItem value="SD">SD (Fase B-C)</SelectItem>
                    <SelectItem value="SMP">SMP (Fase D)</SelectItem>
                    <SelectItem value="SMA">SMA (Fase E)</SelectItem>
                    <SelectItem value="SMK">SMK (Fase F)</SelectItem>
                  </SelectContent>
                </Select>
              </div>

              <div className="space-y-2">
                <Label htmlFor="grade">Kelas</Label>
                <Input 
                  {...register('grade')} 
                  placeholder="Contoh: Kelas 5 / XI / 2 SMK" 
                />
              </div>

              <div className="space-y-2">
                <Label htmlFor="subject">Mata Pelajaran</Label>
                <Select onValueChange={(val) => setValue('subject', val)}>
                  <SelectTrigger>
                    <SelectValue placeholder="Pilih mapel" />
                  </SelectTrigger>
                  <SelectContent>
                    {/* Dynamic based on level */}
                    <SelectItem value="Matematika">Matematika</SelectItem>
                    <SelectItem value="Bahasa Indonesia">Bahasa Indonesia</SelectItem>
                    <SelectItem value="IPA">IPA</SelectItem>
                    <SelectItem value="IPS">IPS</SelectItem>
                    <SelectItem value="Pendidikan Agama">Pendidikan Agama</SelectItem>
                    {/* ... more subjects */}
                  </SelectContent>
                </Select>
              </div>

              <div className="space-y-2">
                <Label htmlFor="duration">Alokasi Waktu (JP)</Label>
                <Input 
                  type="number" 
                  {...register('duration', { valueAsNumber: true })} 
                  placeholder="Contoh: 4" 
                />
              </div>
            </div>

            <div className="space-y-2">
              <Label htmlFor="topic">Topik/Materi Pembelajaran</Label>
              <Textarea 
                {...register('topic')} 
                placeholder="Deskripsikan topik pembelajaran secara spesifik..."
                className="min-h-[80px]"
              />
            </div>
          </div>

          {/* Bagian 2: Capaian Pembelajaran */}
          <div className="space-y-4">
            <h3 className="text-lg font-semibold text-gray-800 border-b pb-2">
              2. Capaian Pembelajaran (CP)
            </h3>
            
            <div className="bg-amber-50 border border-amber-200 rounded-lg p-4">
              <p className="text-sm text-amber-800 mb-2">
                <strong>💡 Tips:</strong> Pilih CP sesuai dengan dokumen Kurikulum Operasional Satuan Pendidikan (KOSP) sekolah Anda.
              </p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div className="space-y-2">
                <Label>Elemen CP</Label>
                <Select onValueChange={(val) => setValue('cpElement', val)}>
                  <SelectTrigger>
                    <SelectValue placeholder="Pilih elemen" />
                  </SelectTrigger>
                  <SelectContent>
                    {availableCPs.map((cp) => (
                      <SelectItem key={cp.element} value={cp.element}>
                        {cp.element}
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>

              <div className="space-y-2">
                <Label>Kode CP</Label>
                <Input {...register('cpCode')} placeholder="Contoh: 3.1" />
              </div>
            </div>

            <div className="space-y-2">
              <Label>Deskripsi CP</Label>
              <Textarea 
                {...register('cpDescription')} 
                placeholder="Tempelkan deskripsi CP lengkap di sini..."
                className="min-h-[100px]"
              />
            </div>
          </div>

          {/* Bagian 3: Konteks Tambahan */}
          <div className="space-y-4">
            <h3 className="text-lg font-semibold text-gray-800 border-b pb-2">
              3. Konteks Tambahan (Opsional)
            </h3>
            <Textarea 
              {...register('additionalContext')} 
              placeholder="Tambahkan informasi khusus: karakteristik peserta didik, ketersediaan sarana, budaya lokal, dll."
              className="min-h-[80px]"
            />
          </div>

          <Button 
            type="submit" 
            className="w-full h-12 text-lg bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700"
            disabled={isGenerating}
          >
            {isGenerating ? (
              <>
                <Loader2 className="mr-2 h-5 w-5 animate-spin" />
                Menghasilkan RPM...
              </>
            ) : (
              <>
                <Sparkles className="mr-2 h-5 w-5" />
                Generate RPM dengan AI
              </>
            )}
          </Button>
        </form>
      </CardContent>
    </Card>
  );
}
// components/ai/AIAssistant.tsx
'use client';

import { useState, useRef, useEffect } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { ScrollArea } from '@/components/ui/scroll-area';
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import { Bot, User, Send, Lightbulb, BookOpen, HelpCircle } from 'lucide-react';
import { useAIChat } from '@/hooks/useAIChat';

interface Message {
  id: string;
  role: 'user' | 'assistant';
  content: string;
  timestamp: Date;
  suggestions?: string[];
}

interface AIAssistantProps {
  rpmContext: {
    level: string;
    subject: string;
    topic: string;
    cpCode: string;
    currentContent?: any;
  };
}

const SUGGESTION_CHIPS = [
  { icon: Lightbulb, label: "Optimalkan Tujuan Pembelajaran", prompt: "Bantu saya menyempurnakan Tujuan Pembelajaran agar lebih spesifik dan terukur" },
  { icon: BookOpen, label: "Saran Aktivitas Menarik", prompt: "Berikan saran aktivitas pembelajaran yang lebih engaging untuk topik ini" },
  { icon: HelpCircle, label: "Penilaian Formatif", prompt: "Bagaimana cara melakukan asesmen formatif yang efektif untuk pembelajaran ini?" },
];

export function AIAssistant({ rpmContext }: AIAssistantProps) {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState<Message[]>([
    {
      id: 'welcome',
      role: 'assistant',
      content: `Halo! Saya Guru AI, asisten Anda dalam menyusun RPM. Saya melihat Anda sedang menyusun pembelajaran **${rpmContext.subject}** tentang **${rpmContext.topic}** untuk jenjang **${rpmContext.level}**.\n\nAda yang bisa saya bantu? Anda bisa bertanya tentang:\n• Penyempurnaan tujuan pembelajaran\n• Strategi pembelajaran yang sesuai\n• Teknik penilaian\n• Diferensiasi untuk peserta didik berkebutuhan khusus`,
      timestamp: new Date(),
      suggestions: SUGGESTION_CHIPS.map(s => s.label)
    }
  ]);
  
  const { sendMessage, isLoading } = useAIChat();
  const scrollRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    scrollRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  const handleSend = async (content: string) => {
    if (!content.trim()) return;

    const userMessage: Message = {
      id: Date.now().toString(),
      role: 'user',
      content,
      timestamp: new Date(),
    };

    setMessages(prev => [...prev, userMessage]);
    setInput('');

    try {
      const response = await sendMessage({
        message: content,
        context: rpmContext,
        history: messages.map(m => ({ role: m.role, content: m.content }))
      });

      const assistantMessage: Message = {
        id: (Date.now() + 1).toString(),
        role: 'assistant',
        content: response.content,
        timestamp: new Date(),
        suggestions: response.suggestions || [],
      };

      setMessages(prev => [...prev, assistantMessage]);
    } catch (error) {
      // Handle error
    }
  };

  return (
    <Card className="w-full h-[600px] flex flex-col border-2 border-purple-100">
      <CardHeader className="bg-gradient-to-r from-purple-600 to-pink-600 text-white rounded-t-lg shrink-0">
        <CardTitle className="text-lg flex items-center gap-2">
          <Bot className="w-5 h-5" />
          Guru AI Assistant
        </CardTitle>
        <p className="text-xs text-purple-100">
          Konteks: {rpmContext.subject} - {rpmContext.topic}
        </p>
      </CardHeader>

      <CardContent className="flex-1 flex flex-col p-0 overflow-hidden">
        <ScrollArea className="flex-1 p-4">
          <div className="space-y-4">
            {messages.map((message) => (
              <div
                key={message.id}
                className={`flex gap-3 ${message.role === 'user' ? 'flex-row-reverse' : ''}`}
              >
                <Avatar className={message.role === 'assistant' ? 'bg-purple-100' : 'bg-blue-100'}>
                  <AvatarFallback>
                    {message.role === 'assistant' ? <Bot className="w-4 h-4" /> : <User className="w-4 h-4" />}
                  </AvatarFallback>
                </Avatar>
                
                <div className={`flex-1 space-y-2 ${message.role === 'user' ? 'items-end' : 'items-start'}`}>
                  <div className={`rounded-lg p-3 max-w-[85%] ${
                    message.role === 'assistant' 
                      ? 'bg-gray-100 text-gray-800' 
                      : 'bg-blue-600 text-white'
                  }`}>
                    <div className="text-sm whitespace-pre-wrap">{message.content}</div>
                  </div>
                  
                  {message.suggestions && message.suggestions.length > 0 && (
                    <div className="flex flex-wrap gap-2 mt-2">
                      {SUGGESTION_CHIPS.filter(s => message.suggestions?.includes(s.label)).map((suggestion) => (
                        <Button
                          key={suggestion.label}
                          variant="outline"
                          size="sm"
                          className="text-xs border-purple-200 hover:bg-purple-50"
                          onClick={() => handleSend(suggestion.prompt)}
                        >
                          <suggestion.icon className="w-3 h-3 mr-1" />
                          {suggestion.label}
                        </Button>
                      ))}
                    </div>
                  )}
                </div>
              </div>
            ))}
            <div ref={scrollRef} />
          </div>
        </ScrollArea>

        <div className="p-4 border-t bg-gray-50 shrink-0">
          <form 
            onSubmit={(e) => {
              e.preventDefault();
              handleSend(input);
            }}
            className="flex gap-2"
          >
            <Input
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Tanyakan tentang strategi pembelajaran, penilaian, atau penyempurnaan RPM..."
              className="flex-1"
              disabled={isLoading}
            />
            <Button type="submit" size="icon" disabled={isLoading || !input.trim()}>
              <Send className="w-4 h-4" />
            </Button>
          </form>
        </div>
      </CardContent>
    </Card>
  );
}
// app/api/export/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { PDFDocument, rgb, StandardFonts } from 'pdf-lib';
import { Document, Packer, Paragraph, TextRun, HeadingLevel, AlignmentType } from 'docx';
import { getRPMById } from '@/lib/db';

export async function POST(req: NextRequest) {
  const { rpmId, format, includeSignatures } = await req.json();
  
  const rpm = await getRPMById(rpmId);
  if (!rpm) return NextResponse.json({ error: 'Not found' }, { status: 404 });

  if (format === 'pdf') {
    const pdfBuffer = await generatePDF(rpm, includeSignatures);
    return new NextResponse(pdfBuffer, {
      headers: {
        'Content-Type': 'application/pdf',
        'Content-Disposition': `attachment; filename="RPM_${rpm.subject}_${rpm.topic}.pdf"`,
      },
    });
  }

  if (format === 'docx') {
    const docBuffer = await generateDOCX(rpm, includeSignatures);
    return new NextResponse(docBuffer, {
      headers: {
        'Content-Type': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
        'Content-Disposition': `attachment; filename="RPM_${rpm.subject}_${rpm.topic}.docx"`,
      },
    });
  }

  return NextResponse.json({ error: 'Invalid format' }, { status: 400 });
}

async function generatePDF(rpm: any, includeSignatures: boolean) {
  const pdfDoc = await PDFDocument.create();
  const page = pdfDoc.addPage([595.28, 841.89]); // A4
  const { width, height } = page.getSize();
  const font = await pdfDoc.embedFont(StandardFonts.Helvetica);
  const boldFont = await pdfDoc.embedFont(StandardFonts.HelveticaBold);

  let y = height - 50;

  // Header
  page.drawText('RANCANGAN PEMBELAJARAN MENDALAM (RPM)', {
    x: 50,
    y,
    size: 16,
    font: boldFont,
    color: rgb(0, 0, 0.5),
  });
  
  y -= 30;

  // Metadata
  const drawSection = (title: string, content: string) => {
    page.drawText(title, { x: 50, y, size: 10, font: boldFont });
    y -= 15;
    page.drawText(content, { x: 70, y, size: 9, font, maxWidth: width - 120 });
    y -= 25;
  };

  drawSection('Satuan Pendidikan:', rpm.content.metadata.satuan_pendidikan);
  drawSection('Mata Pelajaran:', rpm.content.metadata.mata_pelajaran);
  drawSection('Kelas/Fase:', `${rpm.content.metadata.kelas} / ${rpm.content.metadata.fase}`);
  drawSection('Materi:', rpm.content.metadata.materi);
  drawSection('Alokasi Waktu:', rpm.content.metadata.alokasi_waktu);

  // Tujuan Pembelajaran
  y -= 10;
  page.drawText('A. TUJUAN PEMBELAJARAN', { x: 50, y, size: 12, font: boldFont });
  y -= 20;

  rpm.content.tujuan_pembelajaran.forEach((tp: any, idx: number) => {
    page.drawText(`${tp.kode}: ${tp.deskripsi}`, { x: 70, y, size: 9, font, maxWidth: width - 120 });
    y -= 30;
  });

  // Langkah Pembelajaran
  y -= 10;
  page.drawText('B. LANGKAH-LANGKAH PEMBELAJARAN', { x: 50, y, size: 12, font: boldFont });
  y -= 20;

  const steps = rpm.content.langkah_pembelajaran;
  
  // Pendahuluan
  page.drawText('1. Pendahuluan (10-15 menit)', { x: 70, y, size: 10, font: boldFont });
  y -= 15;
  steps.pendahuluan.kegiatan.forEach((k: string) => {
    page.drawText(`• ${k}`, { x: 90, y, size: 9, font, maxWidth: width - 140 });
    y -= 20;
  });

  // Kegiatan Inti
  y -= 5;
  page.drawText('2. Kegiatan Inti', { x: 70, y, size: 10, font: boldFont });
  y -= 15;
  
  const phases = ['fase_1_mengamati', 'fase_2_menanya', 'fase_3_mencoba', 'fase_4_menalar', 'fase_5_mengkomunikasikan'];
  const phaseNames = ['Mengamati', 'Menanya', 'Mencoba', 'Menalar', 'Mengkomunikasikan'];
  
  phases.forEach((phase, idx) => {
    if (steps.inti[phase]) {
      page.drawText(`   ${idx + 1}. ${phaseNames[idx]} (${steps.inti[phase].estimasi_waktu})`, { 
        x: 90, y, size: 9, font: boldFont 
      });
      y -= 15;
      page.drawText(steps.inti[phase].deskripsi, { x: 110, y, size: 8, font, maxWidth: width - 160 });
      y -= 25;
    }
  });

  // Penutup
  y -= 5;
  page.drawText('3. Penutup (10-15 menit)', { x: 70, y, size: 10, font: boldFont });
  y -= 15;
  steps.penutup.kegiatan.forEach((k: string) => {
    page.drawText(`• ${k}`, { x: 90, y, size: 9, font, maxWidth: width - 140 });
    y -= 20;
  });

  // Asesmen
  if (y < 200) {
    page = pdfDoc.addPage([595.28, 841.89]);
    y = height - 50;
  }

  y -= 10;
  page.drawText('C. ASESMEN', { x: 50, y, size: 12, font: boldFont });
  y -= 20;
  
  // ... continue with assessment details

  // Signatures
  if (includeSignatures && y > 150) {
    y -= 40;
    page.drawText('Mengetahui,', { x: 100, y, size: 10, font });
    page.drawText('Guru Mata Pelajaran,', { x: width - 200, y, size: 10, font });
    
    y -= 60;
    
    // Signature lines
    page.drawLine({ start: { x: 80, y }, end: { x: 200, y }, thickness: 1 });
    page.drawLine({ start: { x: width - 220, y }, end: { x: width - 100, y }, thickness: 1 });
    
    y -= 20;
    page.drawText('(_______________________)', { x: 85, y, size: 9, font });
    page.drawText('(_______________________)', { x: width - 215, y, size: 9, font });
    
    y -= 15;
    page.drawText('Kepala Sekolah', { x: 100, y, size: 9, font });
    page.drawText('NIP. ....................', { x: width - 210, y, size: 9, font });
  }

  return await pdfDoc.save();
}

async function generateDOCX(rpm: any, includeSignatures: boolean) {
  const doc = new Document({
    sections: [{
      properties: {},
      children: [
        new Paragraph({
          text: 'RANCANGAN PEMBELAJARAN MENDALAM (RPM)',
          heading: HeadingLevel.TITLE,
          alignment: AlignmentType.CENTER,
          spacing: { after: 400 },
        }),
        
        // Metadata Table
        new Paragraph({
          children: [
            new TextRun({ text: 'Satuan Pendidikan: ', bold: true }),
            new TextRun(rpm.content.metadata.satuan_pendidikan),
          ],
          spacing: { after: 100 },
        }),
        new Paragraph({
          children: [
            new TextRun({ text: 'Mata Pelajaran: ', bold: true }),
            new TextRun(rpm.content.metadata.mata_pelajaran),
          ],
          spacing: { after: 100 },
        }),
        // ... more metadata

        new Paragraph({
          text: 'A. TUJUAN PEMBELAJARAN',
          heading: HeadingLevel.HEADING_1,
          spacing: { before: 400, after: 200 },
        }),
        
        ...rpm.content.tujuan_pembelajaran.map((tp: any) => 
          new Paragraph({
            children: [
              new TextRun({ text: `${tp.kode}: `, bold: true }),
              new TextRun(tp.deskripsi),
            ],
            spacing: { after: 100 },
          })
        ),

        new Paragraph({
          text: 'B. LANGKAH-LANGKAH PEMBELAJARAN',
          heading: HeadingLevel.HEADING_1,
          spacing: { before: 400, after: 200 },
        }),

        // ... continue with all sections

        // LKPD
        new Paragraph({
          text: 'D. LEMBAR KERJA PESERTA DIDIK (LKPD)',
          heading: HeadingLevel.HEADING_1,
          spacing: { before: 400, after: 200 },
        }),
        
        new Paragraph({
          text: rpm.content.lkpd.judul,
          heading: HeadingLevel.HEADING_2,
        }),
        
        ...rpm.content.lkpd.langkah_kerja.map((step: string, idx: number) =>
          new Paragraph({
            text: `${idx + 1}. ${step}`,
            spacing: { after: 100 },
          })
        ),

        // Signatures
        ...(includeSignatures ? [
          new Paragraph({ text: '', spacing: { before: 600 } }),
          new Table({
            rows: [
              new TableRow({
                children: [
                  new TableCell({
                    children: [new Paragraph('Mengetahui,')],
                    width: { size: 50, type: WidthType.PERCENTAGE },
                  }),
                  new TableCell({
                    children: [new Paragraph('Guru Mata Pelajaran,')],
                    width: { size: 50, type: WidthType.PERCENTAGE },
                  }),
                ],
              }),
              // ... signature rows
            ],
          }),
        ] : []),
      ],
    }],
  });

  return await Packer.toBuffer(doc);
}
// app/api/rpm/generate/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { OpenAI } from 'openai';
import { RPM_SYSTEM_PROMPT, RPM_USER_PROMPT_TEMPLATE } from '@/lib/prompts/rpm-generator';
import { prisma } from '@/lib/prisma';
import { getServerSession } from 'next-auth';

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

export async function POST(req: NextRequest) {
  try {
    const session = await getServerSession();
    if (!session) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });

    const body = await req.json();
    const { educationLevel, subject, topic, cpCode, cpDescription, duration, grade } = body;

    // Generate RPM using AI
    const completion = await openai.chat.completions.create({
      model: 'gpt-4-turbo-preview',
      messages: [
        { role: 'system', content: RPM_SYSTEM_PROMPT },
        { 
          role: 'user', 
          content: RPM_USER_PROMPT_TEMPLATE(
            educationLevel, 
            subject, 
            topic, 
            cpCode, 
            cpDescription, 
            duration
          ) 
        },
      ],
      temperature: 0.7,
      response_format: { type: 'json_object' },
    });

    const generatedContent = JSON.parse(completion.choices[0].message.content || '{}');

    // Save to database
    const rpm = await prisma.rPMHistory.create({
      data: {
        userId: session.user.id,
        educationLevel,
        grade,
        subject,
        topic,
        duration,
        cpElement: body.cpElement,
        cpCode,
        cpDescription,
        content: generatedContent,
        status: 'DRAFT',
      },
    });

    return NextResponse.json({ 
      success: true, 
      data: rpm,
      content: generatedContent 
    });

  } catch (error) {
    console.error('RPM Generation Error:', error);
    return NextResponse.json(
      { error: 'Failed to generate RPM' }, 
      { status: 500 }
    );
  }
}
// components/export/SignaturePad.tsx
'use client';

import { useRef, useState } from 'react';
import SignatureCanvas from 'react-signature-canvas';
import { Button } from '@/components/ui/button';
import { Dialog, DialogContent, DialogHeader, DialogTitle } from '@/components/ui/dialog';

interface SignaturePadProps {
  onSave: (signatureData: string) => void;
  role: 'teacher' | 'principal';
  label: string;
}

export function SignaturePad({ onSave, role, label }: SignaturePadProps) {
  const sigRef = useRef<SignatureCanvas>(null);
  const [isOpen, setIsOpen] = useState(false);

  const handleSave = () => {
    if (sigRef.current) {
      const dataUrl = sigRef.current.toDataURL('image/png');
      onSave(dataUrl);
      setIsOpen(false);
    }
  };

  const handleClear = () => {
    sigRef.current?.clear();
  };

  return (
    <Dialog open={isOpen} onOpenChange={setIsOpen}>
      <Button variant="outline" onClick={() => setIsOpen(true)} className="w-full">
        {label}
      </Button>
      
      <DialogContent className="max-w-2xl">
        <DialogHeader>
          <DialogTitle>Tanda Tangan Digital - {label}</DialogTitle>
        </DialogHeader>
        
        <div className="border-2 border-dashed border-gray-300 rounded-lg bg-gray-50">
          <SignatureCanvas
            ref={sigRef}
            canvasProps={{
              width: 500,
              height: 200,
              className: 'signature-canvas',
            }}
            backgroundColor="rgba(255, 255, 255, 0)"
          />
        </div>
        
        <div className="flex justify-between mt-4">
          <Button variant="outline" onClick={handleClear}>
            Hapus
          </Button>
          <div className="space-x-2">
            <Button variant="outline" onClick={() => setIsOpen(false)}>
              Batal
            </Button>
            <Button onClick={handleSave}>
              Simpan Tanda Tangan
            </Button>
          </div>
        </div>
      </DialogContent>
    </Dialog>
  );
}
# .env.local

# Database
DATABASE_URL="postgresql://postgres:[password]@db.[project].supabase.co:5432/postgres"
DIRECT_URL="postgresql://postgres:[password]@db.[project].supabase.co:5432/postgres"

# Authentication
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key"
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# AI APIs
OPENAI_API_KEY="sk-..."
GEMINI_API_KEY="..." # Alternative/Backup

# Storage (Supabase)
NEXT_PUBLIC_SUPABASE_URL="https://[project].supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="..."
SUPABASE_SERVICE_ROLE_KEY="..."

# Redis (Upstash) - for caching
UPSTASH_REDIS_REST_URL="..."
UPSTASH_REDIS_REST_TOKEN="..."
┌─────────────────────────────────────────────────────────────────┐
│                         USER FLOW                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. INPUT DATA                                                  │
│     ├─ Pilih Jenjang (PAUD-SMK)                                 │
│     ├─ Pilih Mapel & Topik                                      │
│     ├─ Input CP (Kode + Deskripsi)                              │
│     └─ Tentukan Durasi (JP)                                     │
│                           ↓                                     │
│  2. AI GENERATION                                               │
│     ├─ System Prompt (RPM Standards)                            │
│     ├─ User Prompt (Context)                                    │
│     ├─ GPT-4 Turbo / Gemini Pro                                 │
│     └─ Parse JSON Response                                      │
│                           ↓                                     │
│  3. PREVIEW & EDIT                                              │
│     ├─ Tampilkan Struktur RPM                                   │
│     ├─ AI Chat Assistant (Fine-tuning)                          │
│     ├─ Edit Manual jika perlu                                   │
│     └─ Real-time Preview                                        │
│                           ↓                                     │
│  4. EXPORT & SHARE                                              │
│     ├─ Export PDF (Layout resmi)                                │
│     ├─ Export DOCX (Editable)                                   │
│     ├─ Digital Signatures (Guru & Kepsek)                       │
│     └─ Generate Share Link                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
