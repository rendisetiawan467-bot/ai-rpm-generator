<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Rencana Pembelajaran Mendalam Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .glass-effect {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
        }
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .input-field {
            transition: all 0.3s ease;
        }
        .input-field:focus {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .step-indicator {
            transition: all 0.3s ease;
        }
        .step-active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .print-area {
            background: white;
            box-shadow: 0 0 50px rgba(0,0,0,0.1);
        }
        @media print {
            .no-print { display: none !important; }
            .print-area { box-shadow: none; margin: 0; padding: 20px; }
            body { background: white; }
        }
        .animate-fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .hover-scale {
            transition: transform 0.2s;
        }
        .hover-scale:hover {
            transform: scale(1.02);
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">

    <!-- Header -->
    <header class="gradient-bg text-white shadow-lg sticky top-0 z-50 no-print">
        <div class="container mx-auto px-4 py-4">
            <div class="flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div class="bg-white/20 p-2 rounded-lg">
                        <i class="fas fa-brain text-2xl"></i>
                    </div>
                    <div>
                        <h1 class="text-xl font-bold">AI RPP Mendalam Generator</h1>
                        <p class="text-xs opacity-90">Rencana Pembelajaran Berbasis AI & Deep Learning</p>
                    </div>
                </div>
                <div class="flex space-x-2">
                    <button onclick="resetForm()" class="px-4 py-2 bg-white/20 rounded-lg hover:bg-white/30 transition text-sm">
                        <i class="fas fa-redo mr-2"></i>Reset
                    </button>
                    <button onclick="generatePreview()" class="px-4 py-2 bg-white text-purple-600 rounded-lg hover:bg-gray-100 transition font-semibold text-sm">
                        <i class="fas fa-eye mr-2"></i>Preview
                    </button>
                    <button onclick="exportPDF()" class="px-4 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 transition font-semibold text-sm">
                        <i class="fas fa-download mr-2"></i>Export PDF
                    </button>
                </div>
            </div>
        </div>
    </header>

    <div class="container mx-auto px-4 py-8 flex flex-col lg:flex-row gap-6">
        
        <!-- Sidebar Navigation -->
        <nav class="lg:w-64 no-print flex-shrink-0">
            <div class="bg-white rounded-2xl shadow-sm p-4 sticky top-24">
                <h3 class="font-semibold text-gray-700 mb-4 px-2">Langkah Pengisian</h3>
                <div class="space-y-2">
                    <button onclick="scrollToSection('identitas')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">1</span>
                        <span>Identitas Modul</span>
                    </button>
                    <button onclick="scrollToSection('karakteristik')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">2</span>
                        <span>Karakteristik & Profil</span>
                    </button>
                    <button onclick="scrollToSection('desain')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">3</span>
                        <span>Desain Pembelajaran</span>
                    </button>
                    <button onclick="scrollToSection('langkah')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">4</span>
                        <span>Langkah & Asesmen</span>
                    </button>
                    <button onclick="scrollToSection('lkpd')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">5</span>
                        <span>LKPD & Rubrik</span>
                    </button>
                    <button onclick="scrollToSection('pelengkap')" class="w-full text-left px-4 py-3 rounded-xl hover:bg-purple-50 transition flex items-center space-x-3 text-sm font-medium text-gray-600 hover:text-purple-600">
                        <span class="w-6 h-6 rounded-full bg-purple-100 text-purple-600 flex items-center justify-center text-xs">6</span>
                        <span>Bahan & Glosarium</span>
                    </button>
                </div>
            </div>
        </nav>

        <!-- Main Form -->
        <main class="flex-1 space-y-6 pb-24 no-print">
            
            <!-- AI Assistant Banner -->
            <div class="bg-gradient-to-r from-blue-500 to-purple-600 rounded-2xl p-6 text-white shadow-lg">
                <div class="flex items-start justify-between">
                    <div>
                        <h2 class="text-lg font-bold mb-2"><i class="fas fa-robot mr-2"></i>Asisten AI Aktif</h2>
                        <p class="text-sm opacity-90 mb-4">Saya akan membantu Anda membuat RPP Mendalam yang komprehensif. Isi data dasar, dan AI akan menyarankan konten yang sesuai dengan kurikulum.</p>
                        <div class="flex space-x-2">
                            <span class="px-3 py-1 bg-white/20 rounded-full text-xs">Kurikulum Merdeka</span>
                            <span class="px-3 py-1 bg-white/20 rounded-full text-xs">Deep Learning</span>
                            <span class="px-3 py-1 bg-white/20 rounded-full text-xs">P5</span>
                        </div>
                    </div>
                    <div class="text-4xl opacity-50">
                        <i class="fas fa-magic"></i>
                    </div>
                </div>
            </div>

            <form id="rppForm" class="space-y-6">
                
                <!-- Section 1: Identitas Modul -->
                <section id="identitas" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-purple-100 flex items-center justify-center text-purple-600 mr-3">
                            <i class="fas fa-id-card"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Identitas Modul</h2>
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Satuan Pendidikan</label>
                            <input type="text" id="sekolah" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Nama Sekolah">
                        </div>
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Fase / Kelas</label>
                            <select id="fase" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" onchange="updateFase()">
                                <option value="">Pilih Fase</option>
                                <option value="A (Kelas 1-2)">Fase A (Kelas 1-2)</option>
                                <option value="B (Kelas 3-4)">Fase B (Kelas 3-4)</option>
                                <option value="C (Kelas 5-6)">Fase C (Kelas 5-6)</option>
                                <option value="D (Kelas 7-8)">Fase D (Kelas 7-8)</option>
                                <option value="E (Kelas 9-10)">Fase E (Kelas 9-10)</option>
                                <option value="F (Kelas 11-12)">Fase F (Kelas 11-12)</option>
                            </select>
                        </div>
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Mata Pelajaran</label>
                            <input type="text" id="mapel" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Contoh: Matematika">
                        </div>
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Materi/Topik</label>
                            <input type="text" id="materi" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Topik pembelajaran">
                        </div>
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Alokasi Waktu</label>
                            <div class="flex space-x-2">
                                <input type="number" id="jam" class="input-field flex-1 px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="JP">
                                <select id="semester" class="input-field w-32 px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none">
                                    <option value="Ganjil">Ganjil</option>
                                    <option value="Genap">Genap</option>
                                </select>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <label class="text-sm font-semibold text-gray-700">Tahun Ajaran</label>
                            <input type="text" id="tahun" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="2024/2025">
                        </div>
                    </div>

                    <!-- Identitas Guru -->
                    <div class="mt-6 pt-6 border-t border-gray-100">
                        <h3 class="font-semibold text-gray-700 mb-4">Identitas Guru</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                            <input type="text" id="namaGuru" class="input-field px-4 py-3 rounded-xl border border-gray-200" placeholder="Nama Guru">
                            <input type="text" id="nipGuru" class="input-field px-4 py-3 rounded-xl border border-gray-200" placeholder="NIP">
                            <input type="text" id="golongan" class="input-field px-4 py-3 rounded-xl border border-gray-200" placeholder="Golongan">
                        </div>
                    </div>
                </section>

                <!-- Section 2: Identifikasi Murid & Karakteristik -->
                <section id="karakteristik" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-blue-600 mr-3">
                            <i class="fas fa-users"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Identifikasi Murid & Karakteristik Materi</h2>
                    </div>

                    <div class="space-y-6">
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Profil Kelas & Karakteristik Murid</label>
                            <textarea id="profilMurid" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Deskripsikan kondisi awal belajar murid, gaya belajar dominan, kebutuhan khusus, dll..."></textarea>
                            <button type="button" onclick="suggest('profilMurid', 'profil')" class="mt-2 text-sm text-purple-600 hover:text-purple-800 font-medium">
                                <i class="fas fa-magic mr-1"></i>Generate dengan AI
                            </button>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Sumber Belajar yang Tersedia</label>
                                <div class="space-y-2">
                                    <label class="flex items-center space-x-2 cursor-pointer">
                                        <input type="checkbox" class="rounded text-purple-600" value="Buku Teks">
                                        <span class="text-sm">Buku Teks Pelajaran</span>
                                    </label>
                                    <label class="flex items-center space-x-2 cursor-pointer">
                                        <input type="checkbox" class="rounded text-purple-600" value="Video Pembelajaran">
                                        <span class="text-sm">Video Pembelajaran</span>
                                    </label>
                                    <label class="flex items-center space-x-2 cursor-pointer">
                                        <input type="checkbox" class="rounded text-purple-600" value="Internet">
                                        <span class="text-sm">Akses Internet</span>
                                    </label>
                                    <label class="flex items-center space-x-2 cursor-pointer">
                                        <input type="checkbox" class="rounded text-purple-600" value="Laboratorium">
                                        <span class="text-sm">Ruang Laboratorium</span>
                                    </label>
                                </div>
                            </div>
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Karakteristik Materi Pelajaran</label>
                                <textarea id="karakterMateri" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Konsep abstrak/konkret, praktikum, teori, dll..."></textarea>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Section 3: Dimensi Profil Lulusan -->
                <section class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-green-100 flex items-center justify-center text-green-600 mr-3">
                            <i class="fas fa-graduation-cap"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Dimensi Profil Pelajar Pancasila</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Beriman, Bertakwa</span>
                            </div>
                            <p class="text-xs text-gray-500">kepada Tuhan YME dan Berakhlak Mulia</p>
                        </div>
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Mandiri</span>
                            </div>
                            <p class="text-xs text-gray-500">Belajar secara mandiri dan bertanggung jawab</p>
                        </div>
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Bergotong Royong</span>
                            </div>
                            <p class="text-xs text-gray-500">Bekerja sama dan kolaboratif</p>
                        </div>
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Kreatif</span>
                            </div>
                            <p class="text-xs text-gray-500">Menghasilkan ide dan solusi baru</p>
                        </div>
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Bernalar Kritis</span>
                            </div>
                            <p class="text-xs text-gray-500">Berpikir logis dan analitis</p>
                        </div>
                        <div class="p-4 border-2 border-gray-100 rounded-xl hover:border-purple-300 transition cursor-pointer hover-scale" onclick="toggleDimensi(this)">
                            <div class="flex items-center mb-2">
                                <input type="checkbox" class="dimensi-check w-5 h-5 text-purple-600 rounded mr-2">
                                <span class="font-semibold text-gray-700">Kebhinekaan Global</span>
                            </div>
                            <p class="text-xs text-gray-500">Menghargai keberagaman</p>
                        </div>
                    </div>
                </section>

                <!-- Section 4: Desain Pembelajaran -->
                <section id="desain" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-orange-100 flex items-center justify-center text-orange-600 mr-3">
                            <i class="fas fa-drafting-compass"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Desain Pembelajaran Mendalam</h2>
                    </div>

                    <div class="space-y-6">
                        <div class="bg-purple-50 rounded-xl p-4 border-l-4 border-purple-500">
                            <h3 class="font-semibold text-purple-900 mb-2">Tujuan Pembelajaran (Deep Learning)</h3>
                            <textarea id="tujuan" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-purple-200 focus:border-purple-500 focus:outline-none bg-white" placeholder="Murid mampu... (gunakan kata kerja operasional taksonomi bloom revisi)"></textarea>
                            <button type="button" onclick="suggest('tujuan', 'tujuan')" class="mt-2 text-sm text-purple-600 hover:text-purple-800 font-medium">
                                <i class="fas fa-magic mr-1"></i>Generate Tujuan dengan AI
                            </button>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Pemantik/Pertanyaan Pendahuluan</label>
                                <textarea id="pemantik" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Pertanyaan yang memancing rasa ingin tahu..."></textarea>
                            </div>
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Apersepsi</label>
                                <textarea id="apersepsi" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Mengaitkan dengan pengetahuan sebelumnya..."></textarea>
                            </div>
                        </div>

                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Model/Metode Pembelajaran</label>
                            <div class="flex flex-wrap gap-2 mb-3">
                                <button type="button" onclick="addMetode('Project Based Learning')" class="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm hover:bg-blue-200 transition">PjBL</button>
                                <button type="button" onclick="addMetode('Problem Based Learning')" class="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm hover:bg-blue-200 transition">PBL</button>
                                <button type="button" onclick="addMetode('Discovery Learning')" class="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm hover:bg-blue-200 transition">Discovery</button>
                                <button type="button" onclick="addMetode('Inquiry Learning')" class="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm hover:bg-blue-200 transition">Inquiry</button>
                                <button type="button" onclick="addMetode('Collaborative Learning')" class="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm hover:bg-blue-200 transition">Collaborative</button>
                            </div>
                            <input type="text" id="metode" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Metode yang dipilih...">
                        </div>
                    </div>
                </section>

                <!-- Section 5: Langkah Pembelajaran -->
                <section id="langkah" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-red-100 flex items-center justify-center text-red-600 mr-3">
                            <i class="fas fa-list-ol"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Langkah-Langkah Pembelajaran</h2>
                    </div>

                    <div class="space-y-4" id="langkahContainer">
                        <!-- Kegiatan Pendahuluan -->
                        <div class="border-2 border-gray-100 rounded-xl p-4">
                            <div class="flex justify-between items-center mb-3">
                                <h3 class="font-semibold text-gray-700">1. Kegiatan Pendahuluan (15 menit)</h3>
                                <button type="button" onclick="suggestLangkah(0)" class="text-xs text-purple-600 hover:text-purple-800">
                                    <i class="fas fa-magic"></i> AI Suggest
                                </button>
                            </div>
                            <textarea id="langkah0" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Salam, doa, presensi, apersepsi, motivasi..."></textarea>
                        </div>

                        <!-- Kegiatan Inti -->
                        <div class="border-2 border-gray-100 rounded-xl p-4">
                            <div class="flex justify-between items-center mb-3">
                                <h3 class="font-semibold text-gray-700">2. Kegiatan Inti (50-60 menit)</h3>
                                <span class="text-xs bg-purple-100 text-purple-700 px-2 py-1 rounded">Fase Deep Learning</span>
                            </div>
                            
                            <div class="space-y-3 ml-4">
                                <div>
                                    <label class="text-xs font-semibold text-gray-500">Observasi/Mengamati</label>
                                    <textarea id="observasi" rows="2" class="input-field w-full px-4 py-2 rounded-lg border border-gray-200 text-sm" placeholder="Murid mengamati fenomena/masalah..."></textarea>
                                </div>
                                <div>
                                    <label class="text-xs font-semibold text-gray-500">Mempertanyakan/Menanya</label>
                                    <textarea id="menanya" rows="2" class="input-field w-full px-4 py-2 rounded-lg border border-gray-200 text-sm" placeholder="Murid mengajukan pertanyaan kritis..."></textarea>
                                </div>
                                <div>
                                    <label class="text-xs font-semibold text-gray-500">Mengeksplorasi/Mencoba</label>
                                    <textarea id="mencoba" rows="2" class="input-field w-full px-4 py-2 rounded-lg border border-gray-200 text-sm" placeholder="Eksperimen, diskusi, pengumpulan data..."></textarea>
                                </div>
                                <div>
                                    <label class="text-xs font-semibold text-gray-500">Mengasosiasi/Menalar</label>
                                    <textarea id="menalar" rows="2" class="input-field w-full px-4 py-2 rounded-lg border border-gray-200 text-sm" placeholder="Analisis, sintesis, menarik kesimpulan..."></textarea>
                                </div>
                                <div>
                                    <label class="text-xs font-semibold text-gray-500">Mengkomunikasikan</label>
                                    <textarea id="komunikasi" rows="2" class="input-field w-full px-4 py-2 rounded-lg border border-gray-200 text-sm" placeholder="Presentasi, diskusi kelas, publikasi..."></textarea>
                                </div>
                            </div>
                        </div>

                        <!-- Kegiatan Penutup -->
                        <div class="border-2 border-gray-100 rounded-xl p-4">
                            <div class="flex justify-between items-center mb-3">
                                <h3 class="font-semibold text-gray-700">3. Kegiatan Penutup (15 menit)</h3>
                                <button type="button" onclick="suggestLangkah(2)" class="text-xs text-purple-600 hover:text-purple-800">
                                    <i class="fas fa-magic"></i> AI Suggest
                                </button>
                            </div>
                            <textarea id="langkah2" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Refleksi, simpulan, evaluasi, tindak lanjut..."></textarea>
                        </div>
                    </div>
                </section>

                <!-- Section 6: Asesmen -->
                <section class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-yellow-100 flex items-center justify-center text-yellow-600 mr-3">
                            <i class="fas fa-clipboard-check"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Asesmen Pembelajaran</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div class="bg-blue-50 rounded-xl p-4">
                            <h3 class="font-semibold text-blue-900 mb-2">Diagnostik</h3>
                            <textarea id="asesmenDiag" rows="3" class="w-full px-3 py-2 rounded-lg border border-blue-200 text-sm" placeholder="Pra-asesmen, gaya belajar, kemampuan awal..."></textarea>
                        </div>
                        <div class="bg-green-50 rounded-xl p-4">
                            <h3 class="font-semibold text-green-900 mb-2">Formatif</h3>
                            <textarea id="asesmenFor" rows="3" class="w-full px-3 py-2 rounded-lg border border-green-200 text-sm" placeholder="Selama proses, observasi, unjuk kerja..."></textarea>
                        </div>
                        <div class="bg-purple-50 rounded-xl p-4">
                            <h3 class="font-semibold text-purple-900 mb-2">Sumatif</h3>
                            <textarea id="asesmenSum" rows="3" class="w-full px-3 py-2 rounded-lg border border-purple-200 text-sm" placeholder="Tes, proyek, portofolio akhir..."></textarea>
                        </div>
                    </div>

                    <div class="mt-6">
                        <label class="text-sm font-semibold text-gray-700 mb-2 block">Teknik dan Instrumen Penilaian</label>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
                            <label class="flex items-center space-x-2 p-3 border rounded-lg cursor-pointer hover:bg-gray-50">
                                <input type="checkbox" class="rounded text-purple-600">
                                <span class="text-sm">Tes Tertulis</span>
                            </label>
                            <label class="flex items-center space-x-2 p-3 border rounded-lg cursor-pointer hover:bg-gray-50">
                                <input type="checkbox" class="rounded text-purple-600">
                                <span class="text-sm">Praktik/Unjuk Kerja</span>
                            </label>
                            <label class="flex items-center space-x-2 p-3 border rounded-lg cursor-pointer hover:bg-gray-50">
                                <input type="checkbox" class="rounded text-purple-600">
                                <span class="text-sm">Penugasan</span>
                            </label>
                            <label class="flex items-center space-x-2 p-3 border rounded-lg cursor-pointer hover:bg-gray-50">
                                <input type="checkbox" class="rounded text-purple-600">
                                <span class="text-sm">Portofolio</span>
                            </label>
                        </div>
                    </div>
                </section>

                <!-- Section 7: Refleksi & Pengayaan -->
                <section class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-indigo-100 flex items-center justify-center text-indigo-600 mr-3">
                            <i class="fas fa-lightbulb"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Refleksi dan Pengayaan</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Refleksi Guru</label>
                            <textarea id="refleksi" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Kendala, solusi, dan perbaikan untuk pertemuan berikutnya..."></textarea>
                        </div>
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Kegiatan Pengayaan (Remedial & Pengayaan)</label>
                            <textarea id="pengayaan" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Program untuk murid yang belum tuntas dan yang sudah mencapai kompetensi..."></textarea>
                        </div>
                    </div>
                </section>

                <!-- Section 8: LKPD -->
                <section id="lkpd" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-pink-100 flex items-center justify-center text-pink-600 mr-3">
                            <i class="fas fa-file-alt"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Lembar Kerja Peserta Didik (LKPD)</h2>
                    </div>

                    <div class="space-y-4">
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Judul LKPD</label>
                            <input type="text" id="judulLkpd" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Judul kegiatan LKPD">
                        </div>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Tujuan LKPD</label>
                                <textarea id="tujuanLkpd" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Tujuan kegiatan..."></textarea>
                            </div>
                            <div>
                                <label class="text-sm font-semibold text-gray-700 mb-2 block">Petunjuk Pengerjaan</label>
                                <textarea id="petunjukLkpd" rows="3" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Langkah-langkah pengerjaan..."></textarea>
                            </div>
                        </div>

                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Isi/Kegiatan LKPD</label>
                            <textarea id="isiLkpd" rows="6" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none font-mono text-sm" placeholder="1. &#10;2. &#10;3. &#10;..."></textarea>
                            <button type="button" onclick="generateLKPD()" class="mt-2 px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition text-sm">
                                <i class="fas fa-magic mr-2"></i>Generate Template LKPD
                            </button>
                        </div>
                    </div>
                </section>

                <!-- Section 9: Rubrik Penilaian -->
                <section class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-teal-100 flex items-center justify-center text-teal-600 mr-3">
                            <i class="fas fa-star"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Rubrik Penilaian</h2>
                    </div>

                    <div id="rubrikContainer" class="space-y-4">
                        <div class="overflow-x-auto">
                            <table class="w-full text-sm">
                                <thead>
                                    <tr class="bg-gray-50">
                                        <th class="px-4 py-2 text-left font-semibold text-gray-700">Kriteria</th>
                                        <th class="px-4 py-2 text-center font-semibold text-gray-700">Sangat Baik (4)</th>
                                        <th class="px-4 py-2 text-center font-semibold text-gray-700">Baik (3)</th>
                                        <th class="px-4 py-2 text-center font-semibold text-gray-700">Cukup (2)</th>
                                        <th class="px-4 py-2 text-center font-semibold text-gray-700">Kurang (1)</th>
                                    </tr>
                                </thead>
                                <tbody id="rubrikBody">
                                    <tr class="border-b">
                                        <td class="p-2"><input type="text" class="w-full px-2 py-1 border rounded" placeholder="Kriteria 1"></td>
                                        <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                                        <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                                        <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                                        <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <button type="button" onclick="addRubrikRow()" class="px-4 py-2 border-2 border-dashed border-gray-300 rounded-lg text-gray-500 hover:border-purple-500 hover:text-purple-600 transition w-full text-sm">
                            <i class="fas fa-plus mr-2"></i>Tambah Kriteria
                        </button>
                    </div>
                </section>

                <!-- Section 10: Bahan Bacaan & Glosarium -->
                <section id="pelengkap" class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center text-gray-600 mr-3">
                            <i class="fas fa-book"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Bahan Bacaan & Glosarium</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Bahan Bacaan untuk Murid</label>
                            <textarea id="bahanMurid" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Buku, artikel, video, website untuk murid..."></textarea>
                        </div>
                        <div>
                            <label class="text-sm font-semibold text-gray-700 mb-2 block">Bahan Bacaan untuk Guru</label>
                            <textarea id="bahanGuru" rows="4" class="input-field w-full px-4 py-3 rounded-xl border border-gray-200 focus:border-purple-500 focus:outline-none" placeholder="Referensi akademik, jurnal, panduan kurikulum..."></textarea>
                        </div>
                    </div>

                    <div class="mt-6">
                        <label class="text-sm font-semibold text-gray-700 mb-2 block">Glosarium (Istilah Penting)</label>
                        <div id="glosariumContainer" class="space-y-2">
                            <div class="flex space-x-2">
                                <input type="text" class="flex-1 px-4 py-2 rounded-lg border border-gray-200" placeholder="Istilah">
                                <input type="text" class="flex-2 px-4 py-2 rounded-lg border border-gray-200" placeholder="Definisi/Penjelasan">
                            </div>
                        </div>
                        <button type="button" onclick="addGlosarium()" class="mt-2 text-sm text-purple-600 hover:text-purple-800 font-medium">
                            <i class="fas fa-plus mr-1"></i>Tambah Istilah
                        </button>
                    </div>
                </section>

                <!-- Section 11: Tanda Tangan -->
                <section class="bg-white rounded-2xl shadow-sm p-6 animate-fade-in">
                    <div class="flex items-center mb-6">
                        <div class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center text-gray-600 mr-3">
                            <i class="fas fa-signature"></i>
                        </div>
                        <h2 class="text-xl font-bold text-gray-800">Tanda Tangan</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="border-2 border-dashed border-gray-300 rounded-xl p-6 text-center">
                            <p class="text-sm font-semibold text-gray-700 mb-4">Guru Mata Pelajaran</p>
                            <div class="h-20 border-b border-gray-400 mb-2"></div>
                            <input type="text" id="ttdGuru" class="w-full text-center font-semibold border-b border-gray-300 focus:border-purple-500 outline-none" placeholder="Nama Lengkap">
                            <p class="text-xs text-gray-500 mt-1">NIP. <input type="text" class="border-b border-gray-300 focus:border-purple-500 outline-none w-32" placeholder="..............."></p>
                        </div>
                        <div class="border-2 border-dashed border-gray-300 rounded-xl p-6 text-center">
                            <p class="text-sm font-semibold text-gray-700 mb-4">Kepala Sekolah</p>
                            <div class="h-20 border-b border-gray-400 mb-2"></div>
                            <input type="text" id="ttdKepsek" class="w-full text-center font-semibold border-b border-gray-300 focus:border-purple-500 outline-none" placeholder="Nama Lengkap">
                            <p class="text-xs text-gray-500 mt-1">NIP. <input type="text" class="border-b border-gray-300 focus:border-purple-500 outline-none w-32" placeholder="..............."></p>
                        </div>
                    </div>
                </section>

            </form>
        </main>

        <!-- Preview Panel (Hidden by default on mobile, shown on desktop) -->
        <aside class="lg:w-96 no-print">
            <div class="sticky top-24 space-y-4">
                <div class="bg-white rounded-2xl shadow-sm p-4">
                    <h3 class="font-semibold text-gray-700 mb-4">Preview Cepat</h3>
                    <div id="quickPreview" class="text-sm text-gray-600 space-y-2">
                        <p class="italic text-gray-400">Isi formulir untuk melihat preview...</p>
                    </div>
                </div>
                
                <div class="bg-gradient-to-br from-purple-500 to-pink-500 rounded-2xl p-6 text-white">
                    <h3 class="font-bold mb-2">Tips Deep Learning</h3>
                    <ul class="text-sm space-y-2 opacity-90">
                        <li class="flex items-start"><i class="fas fa-check mt-1 mr-2"></i>Fokus pada pemahaman konsep, bukan hafalan</li>
                        <li class="flex items-start"><i class="fas fa-check mt-1 mr-2"></i>Gunakan masalah otentik dari kehidupan nyata</li>
                        <li class="flex items-start"><i class="fas fa-check mt-1 mr-2"></i>Fasilitasi kolaborasi dan diskusi</li>
                        <li class="flex items-start"><i class="fas fa-check mt-1 mr-2"></i>Refleksi metakognitif di akhir pembelajaran</li>
                    </ul>
                </div>
            </div>
        </aside>
    </div>

    <!-- Print Preview Modal -->
    <div id="printModal" class="fixed inset-0 bg-black/50 z-50 hidden overflow-y-auto">
        <div class="min-h-screen px-4 text-center">
            <div class="fixed inset-0 transition-opacity" onclick="closePreview()"></div>
            
            <span class="inline-block h-screen align-middle" aria-hidden="true">&#8203;</span>
            
            <div class="inline-block w-full max-w-4xl my-8 text-left align-middle transition-all transform bg-white shadow-xl rounded-lg overflow-hidden">
                <div class="bg-gray-50 px-4 py-3 border-b flex justify-between items-center sticky top-0 z-10">
                    <h3 class="text-lg font-semibold">Preview Dokumen</h3>
                    <div class="space-x-2">
                        <button onclick="closePreview()" class="px-4 py-2 text-gray-600 hover:text-gray-800">Tutup</button>
                        <button onclick="window.print()" class="px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                            <i class="fas fa-print mr-2"></i>Cetak
                        </button>
                    </div>
                </div>
                
                <div id="printContent" class="p-8 bg-white">
                    <!-- Content will be generated here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        // AI Suggestions Database
        const aiSuggestions = {
            profil: {
                'A': 'Murid kelas awal memiliki karakteristik belajar melalui bermain dan pengalaman langsung. Perhatian mereka pendek, memerlukan variasi aktivitas. Beberapa murid masih dalam tahap membaca permulaan.',
                'B': 'Murid mulai mampu bekerja mandiri dalam waktu singkat. Memiliki rasa ingin tahu tinggi tentang lingkungan sekitar. Kemampuan literasi dasar mulai berkembang pesat.',
                'C': 'Murid mampu berpikir logis konkret. Mulai mampu menerima tanggung jawab kelompok. Terdapat variasi gaya belajar: visual, auditori, dan kinestetik.',
                'D': 'Murid memasuki tahap berpikir abstrak awal. Mampu menganalisis hubungan sebab-akibat. Perlu penguatan soft skills dan manajemen emosi.',
                'E': 'Murid mampu berpikir kritis dan analitis. Memiliki minat yang mulai spesifik. Mampu bekerja dalam proyek kompleks dengan supervisi minimal.',
                'F': 'Murid siap untuk pembelajaran mandiri tingkat lanjut. Mampu melakukan riset sederhana. Memerlukan persiapan untuk jenjang pendidikan tinggi.'
            },
            tujuan: {
                'Matematika': 'Melalui kegiatan eksplorasi dan diskusi, murid mampu: 1) Menganalisis pola dan struktur matematis dalam konteks nyata; 2) Mengembangkan strategi pemecahan masalah non-rutin; 3) Mengkomunikasikan argumen matematis secara logis; 4) Menghargai kegunaan matematika dalam kehidupan.',
                'IPA': 'Melalui inkuiri ilmiah, murid mampu: 1) Merumuskan pertanyaan penelitian sederhana; 2) Merancang dan melakukan penyelidikan; 3) Menganalisis data dan menarik kesimpulan; 4) Mengkomunikasikan hasil penyelidikan; 5) Menumbuhkan sikap ilmiah dan peduli lingkungan.',
                'Bahasa Indonesia': 'Melalui pembelajaran berbasis teks, murid mampu: 1) Memahami struktur dan kebahasaan teks sesuai jenisnya; 2) Menyajikan informasi secara kreatif; 3) Menggunakan bahasa Indonesia secara tepat dan kreatif; 4) Mengapresiasi keberagaman budaya melalui teks.'
            }
        };

        function updateFase() {
            const fase = document.getElementById('fase').value;
            // Auto-suggest based on fase
        }

        function suggest(fieldId, type) {
            const field = document.getElementById(fieldId);
            const fase = document.getElementById('fase').value.charAt(0);
            const mapel = document.getElementById('mapel').value;
            
            let suggestion = '';
            
            if (type === 'profil' && aiSuggestions.profil[fase]) {
                suggestion = aiSuggestions.profil[fase];
            } else if (type === 'tujuan') {
                if (aiSuggestions.tujuan[mapel]) {
                    suggestion = aiSuggestions.tujuan[mapel];
                } else {
                    suggestion = `Melalui pendekatan pembelajaran mendalam, murid mampu: 1) Memahami konsep ${mapel} secara komprehensif; 2) Mengaplikasikan pengetahuan dalam konteks nyata; 3) Mengembangkan keterampilan berpikir kritis dan kreatif; 4) Bekerja sama dalam menyelesaikan masalah kompleks.`;
                }
            }
            
            if (suggestion) {
                field.value = suggestion;
                field.classList.add('ring-2', 'ring-purple-500');
                setTimeout(() => field.classList.remove('ring-2', 'ring-purple-500'), 1000);
            }
        }

        function suggestLangkah(index) {
            const suggestions = [
                '1) Guru menyampaikan salam dan doa; 2) Melakukan presensi dan pemeriksaan kesiapan belajar; 3) Mengaitkan materi dengan pengalaman sehari-hari murid; 4) Menyampaikan tujuan pembelajaran dan memotivasi murid.',
                'Lihat bagian Kegiatan Inti (5M)',
                '1) Guru bersama murid menyimpulkan materi; 2) Melakukan refleksi proses pembelajaran; 3) Menyampaikan tugas/penugasan rumah; 4) Menutup dengan doa dan salam.'
            ];
            
            const field = document.getElementById(`langkah${index}`);
            field.value = suggestions[index];
        }

        function addMetode(metode) {
            const input = document.getElementById('metode');
            const current = input.value;
            input.value = current ? current + ', ' + metode : metode;
        }

        function toggleDimensi(element) {
            const checkbox = element.querySelector('input[type="checkbox"]');
            checkbox.checked = !checkbox.checked;
            element.classList.toggle('border-purple-500', checkbox.checked);
            element.classList.toggle('bg-purple-50', checkbox.checked);
        }

        function addRubrikRow() {
            const tbody = document.getElementById('rubrikBody');
            const row = document.createElement('tr');
            row.className = 'border-b';
            row.innerHTML = `
                <td class="p-2"><input type="text" class="w-full px-2 py-1 border rounded" placeholder="Kriteria"></td>
                <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
                <td class="p-2"><textarea class="w-full px-2 py-1 border rounded text-xs" rows="2"></textarea></td>
            `;
            tbody.appendChild(row);
        }

        function addGlosarium() {
            const container = document.getElementById('glosariumContainer');
            const div = document.createElement('div');
            div.className = 'flex space-x-2';
            div.innerHTML = `
                <input type="text" class="flex-1 px-4 py-2 rounded-lg border border-gray-200" placeholder="Istilah">
                <input type="text" class="flex-2 px-4 py-2 rounded-lg border border-gray-200" placeholder="Definisi/Penjelasan">
            `;
            container.appendChild(div);
        }

        function generateLKPD() {
            const materi = document.getElementById('materi').value || 'Materi Pembelajaran';
            const isi = `LEMBAR KERJA PESERTA DIDIK

Judul: Eksplorasi ${materi} dalam Kehidupan Sehari-hari

Petunjuk:
1. Baca dengan cermat setiap instruksi
2. Diskusikan dengan kelompokmu
3. Catat hasil pengamatan dengan jelas

Kegiatan 1: Mengamati
Amati fenomena berikut di lingkungan sekitarmu...
[Tempat gambar/tabel pengamatan]

Kegiatan 2: Menanya
Tuliskan 3 pertanyaan kritis tentang fenomena yang kamu amati:
1. 
2. 
3. 

Kegiatan 3: Mengeksplorasi
Lakukan kegiatan berikut secara berkelompok...
[Langkah-langkah praktik]

Kegiatan 4: Mengasosiasi
Analisis data yang telah dikumpulkan:
- Pola yang ditemukan:
- Hubungan antar variabel:
- Kesimpulan sementara:

Kegiatan 5: Mengkomunikasikan
Presentasikan hasil kerjamu dalam format:
[Poster/PPT/Laporan tertulis]`;
            
            document.getElementById('isiLkpd').value = isi;
        }

        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        function resetForm() {
            if (confirm('Apakah Anda yakin ingin mengosongkan semua form?')) {
                document.getElementById('rppForm').reset();
            }
        }

        function generatePreview() {
            const modal = document.getElementById('printModal');
            const content = document.getElementById('printContent');
            
            // Gather all data
            const data = {
                sekolah: document.getElementById('sekolah').value,
                fase: document.getElementById('fase').value,
                mapel: document.getElementById('mapel').value,
                materi: document.getElementById('materi').value,
                guru: document.getElementById('namaGuru').value,
                tujuan: document.getElementById('tujuan').value,
                // ... gather all other fields
            };
            
            // Generate HTML content for printing
            content.innerHTML = `
                <div class="print-area">
                    <div class="text-center mb-8 border-b-2 border-black pb-4">
                        <h1 class="text-2xl font-bold uppercase">RENCANA PEMBELAJARAN MENDALAM</h1>
                        <h2 class="text-xl font-semibold mt-2">${data.sekolah || 'Nama Sekolah'}</h2>
                        <p class="text-sm mt-1">Tahun Ajaran ${document.getElementById('tahun').value || '2024/2025'}</p>
                    </div>
                    
                    <table class="w-full mb-6 text-sm">
                        <tr class="border-b">
                            <td class="py-2 font-semibold w-1/3">Mata Pelajaran</td>
                            <td class="py-2">${data.mapel || '-'}</td>
                        </tr>
                        <tr class="border-b">
                            <td class="py-2 font-semibold">Materi/Topik</td>
                            <td class="py-2">${data.materi || '-'}</td>
                        </tr>
                        <tr class="border-b">
                            <td class="py-2 font-semibold">Fase/Kelas</td>
                            <td class="py-2">${data.fase || '-'}</td>
                        </tr>
                        <tr class="border-b">
                            <td class="py-2 font-semibold">Alokasi Waktu</td>
                            <td class="py-2">${document.getElementById('jam').value || '-'} JP (${document.getElementById('semester').value || ''})</td>
                        </tr>
                        <tr class="border-b">
                            <td class="py-2 font-semibold">Guru</td>
                            <td class="py-2">${data.guru || '-'}</td>
                        </tr>
                    </table>

                    <div class="mb-6">
                        <h3 class="font-bold text-lg mb-2 bg-gray-100 p-2">A. TUJUAN PEMBELAJARAN</h3>
                        <p class="text-sm whitespace-pre-line">${data.tujuan || '-'}</p>
                    </div>

                    <div class="mb-6">
                        <h3 class="font-bold text-lg mb-2 bg-gray-100 p-2">B. PROFIL MURID</h3>
                        <p class="text-sm whitespace-pre-line">${document.getElementById('profilMurid').value || '-'}</p>
                    </div>

                    <div class="mb-6">
                        <h3 class="font-bold text-lg mb-2 bg-gray-100 p-2">C. LANGKAH PEMBELAJARAN</h3>
                        <div class="text-sm space-y-4">
                            <div>
                                <strong>1. Pendahuluan</strong>
                                <p class="whitespace-pre-line ml-4">${document.getElementById('langkah0').value || '-'}</p>
                            </div>
                            <div>
                                <strong>2. Kegiatan Inti (5M)</strong>
                                <div class="ml-4 space-y-2 mt-2">
                                    <p><em>Observasi:</em> ${document.getElementById('observasi').value || '-'}</p>
                                    <p><em>Menanya:</em> ${document.getElementById('menanya').value || '-'}</p>
                                    <p><em>Mencoba:</em> ${document.getElementById('mencoba').value || '-'}</p>
                                    <p><em>Menalar:</em> ${document.getElementById('menalar').value || '-'}</p>
                                    <p><em>Komunikasi:</em> ${document.getElementById('komunikasi').value || '-'}</p>
                                </div>
                            </div>
                            <div>
                                <strong>3. Penutup</strong>
                                <p class="whitespace-pre-line ml-4">${document.getElementById('langkah2').value || '-'}</p>
                            </div>
                        </div>
                    </div>

                    <div class="mb-6">
                        <h3 class="font-bold text-lg mb-2 bg-gray-100 p-2">D. ASESMEN</h3>
                        <div class="grid grid-cols-3 gap-4 text-sm">
                            <div>
                                <strong>Diagnostik:</strong>
                                <p>${document.getElementById('asesmenDiag').value || '-'}</p>
                            </div>
                            <div>
                                <strong>Formatif:</strong>
                                <p>${document.getElementById('asesmenFor').value || '-'}</p>
                            </div>
                            <div>
                                <strong>Sumatif:</strong>
                                <p>${document.getElementById('asesmenSum').value || '-'}</p>
                            </div>
                        </div>
                    </div>

                    <div class="mb-6">
                        <h3 class="font-bold text-lg mb-2 bg-gray-100 p-2">E. LEMBAR KERJA PESERTA DIDIK</h3>
                        <p class="text-sm whitespace-pre-line">${document.getElementById('isiLkpd').value || '-'}</p>
                    </div>

                    <div class="mt-12 grid grid-cols-2 gap-12 text-center">
                        <div>
                            <p class="mb-8">Guru Mata Pelajaran,</p>
                            <p class="font-bold underline">${document.getElementById('ttdGuru').value || '_________________'}</p>
                            <p class="text-sm">NIP. ${document.getElementById('nipGuru').value || '................'}</p>
                        </div>
                        <div>
                            <p class="mb-8">Kepala Sekolah,</p>
                            <p class="font-bold underline">${document.getElementById('ttdKepsek').value || '_________________'}</p>
                            <p class="text-sm">NIP. ................</p>
                        </div>
                    </div>
                </div>
            `;
            
            modal.classList.remove('hidden');
        }

        function closePreview() {
            document.getElementById('printModal').classList.add('hidden');
        }

        function exportPDF() {
            window.print();
        }

        // Live preview update
        document.querySelectorAll('input, textarea, select').forEach(input => {
            input.addEventListener('input', updateQuickPreview);
        });

        function updateQuickPreview() {
            const preview = document.getElementById('quickPreview');
            const mapel = document.getElementById('mapel').value || 'Mata Pelajaran';
            const materi = document.getElementById('materi').value || 'Topik';
            const fase = document.getElementById('fase').value || 'Fase';
            
            preview.innerHTML = `
                <div class="space-y-2">
                    <div class="flex justify-between border-b pb-1">
                        <span class="text-gray-500">Mapel:</span>
                        <span class="font-medium">${mapel}</span>
                    </div>
                    <div class="flex justify-between border-b pb-1">
                        <span class="text-gray-500">Materi:</span>
                        <span class="font-medium">${materi}</span>
                    </div>
                    <div class="flex justify-between border-b pb-1">
                        <span class="text-gray-500">Fase:</span>
                        <span class="font-medium">${fase}</span>
                    </div>
                    <div class="mt-3 p-2 bg-purple-50 rounded text-xs text-purple-700">
                        <i class="fas fa-info-circle mr-1"></i>
                        Lengkapi semua bagian untuk generate dokumen lengkap
                    </div>
                </div>
            `;
        }
    </script>
</body>
</html>
