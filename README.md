<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Rencana Pembelajaran Mendalam Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        * {
            font-family: 'Inter', sans-serif;
        }
        h1, h2, h3, .font-display {
            font-family: 'Space Grotesk', sans-serif;
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #6366f1;
            border-radius: 4px;
        }
        
        /* Animations */
        @keyframes gradient-x {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }
        .animate-gradient-x {
            background-size: 200% 200%;
            animation: gradient-x 15s ease infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }
        .animate-float {
            animation: float 6s ease-in-out infinite;
        }
        
        @keyframes slide-up {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-slide-up {
            animation: slide-up 0.6s ease-out forwards;
        }
        
        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 20px rgba(99, 102, 241, 0.4); }
            50% { box-shadow: 0 0 40px rgba(99, 102, 241, 0.8); }
        }
        .animate-pulse-glow {
            animation: pulse-glow 2s ease-in-out infinite;
        }
        
        /* Glassmorphism */
        .glass {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        .glass-dark {
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        /* Input focus effects */
        .input-focus-ring:focus-within {
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.3);
            border-color: #6366f1;
        }
        
        /* Typing effect cursor */
        .typing-cursor::after {
            content: '|';
            animation: blink 1s infinite;
        }
        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }
        
        /* Card hover effects */
        .hover-lift {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .hover-lift:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        /* Print styles */
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: white; }
            .glass, .glass-dark { background: white; border: none; }
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 overflow-x-hidden">

    <!-- Background Elements -->
    <div class="fixed inset-0 z-0 overflow-hidden pointer-events-none">
        <div class="absolute top-0 left-0 w-full h-full bg-gradient-to-br from-indigo-50 via-purple-50 to-pink-50 animate-gradient-x"></div>
        <div class="absolute top-20 left-10 w-72 h-72 bg-purple-300 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float"></div>
        <div class="absolute top-40 right-10 w-72 h-72 bg-indigo-300 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float" style="animation-delay: 2s;"></div>
        <div class="absolute -bottom-8 left-20 w-72 h-72 bg-pink-300 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float" style="animation-delay: 4s;"></div>
    </div>

    <!-- Navigation -->
    <nav class="fixed top-0 w-full z-50 glass border-b border-white/20 transition-all duration-300" id="navbar">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-gradient-to-br from-indigo-600 to-purple-600 rounded-xl flex items-center justify-center text-white font-bold text-xl shadow-lg animate-pulse-glow">
                        AI
                    </div>
                    <span class="font-display font-bold text-xl bg-clip-text text-transparent bg-gradient-to-r from-indigo-600 to-purple-600">
                        DeepLearning Plan
                    </span>
                </div>
                <div class="hidden md:flex items-center space-x-6">
                    <button onclick="scrollToSection('generator')" class="text-slate-600 hover:text-indigo-600 font-medium transition-colors">Generator</button>
                    <button onclick="scrollToSection('features')" class="text-slate-600 hover:text-indigo-600 font-medium transition-colors">Fitur</button>
                    <button onclick="scrollToSection('examples')" class="text-slate-600 hover:text-indigo-600 font-medium transition-colors">Contoh</button>
                    <button onclick="toggleTheme()" class="p-2 rounded-lg hover:bg-slate-100 transition-colors">
                        <svg class="w-5 h-5 text-slate-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path></svg>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="relative z-10 pt-32 pb-20 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto">
        <div class="text-center animate-slide-up">
            <div class="inline-flex items-center px-4 py-2 rounded-full bg-indigo-100 text-indigo-700 text-sm font-semibold mb-6 border border-indigo-200">
                <span class="flex h-2 w-2 rounded-full bg-indigo-600 mr-2 animate-pulse"></span>
                Powered by Advanced AI
            </div>
            <h1 class="font-display text-5xl md:text-7xl font-bold text-slate-900 mb-6 leading-tight">
                Rencana Pembelajaran<br>
                <span class="bg-clip-text text-transparent bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 typing-cursor" id="hero-text">Mendalam</span>
            </h1>
            <p class="mt-4 max-w-2xl mx-auto text-xl text-slate-600 leading-relaxed">
                Buat rencana pembelajaran komprehensif, terstruktur, dan sesuai kurikulum dalam hitungan detik. 
                Dari analisis KD hingga penilaian autentik.
            </p>
            <div class="mt-10 flex justify-center gap-4">
                <button onclick="scrollToSection('generator')" class="px-8 py-4 bg-gradient-to-r from-indigo-600 to-purple-600 text-white font-semibold rounded-2xl shadow-lg hover:shadow-xl transform hover:-translate-y-1 transition-all duration-300 flex items-center gap-2">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                    Mulai Generate
                </button>
                <button onclick="showDemo()" class="px-8 py-4 bg-white text-slate-700 font-semibold rounded-2xl shadow-md hover:shadow-lg border border-slate-200 transform hover:-translate-y-1 transition-all duration-300 flex items-center gap-2">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                    Lihat Demo
                </button>
            </div>
        </div>
    </section>

    <!-- Main Generator Section -->
    <section id="generator" class="relative z-10 py-20 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto">
        <div class="grid lg:grid-cols-2 gap-12 items-start">
            
            <!-- Input Form -->
            <div class="glass rounded-3xl p-8 shadow-2xl border border-white/50 sticky top-24">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="font-display text-2xl font-bold text-slate-800">Parameter Pembelajaran</h2>
                    <div class="flex gap-2">
                        <button onclick="resetForm()" class="p-2 text-slate-400 hover:text-red-500 transition-colors" title="Reset">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
                        </button>
                    </div>
                </div>

                <form id="rppForm" class="space-y-6" onsubmit="generateRPP(event)">
                    <!-- Subject & Grade -->
                    <div class="grid grid-cols-2 gap-4">
                        <div class="input-focus-ring rounded-xl bg-white/50 p-1 transition-all duration-300">
                            <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1 ml-3 mt-2">Mata Pelajaran</label>
                            <input type="text" id="subject" required placeholder="Contoh: Matematika" 
                                class="w-full px-3 py-2 bg-transparent border-none focus:ring-0 text-slate-800 placeholder-slate-400 font-medium">
                        </div>
                        <div class="input-focus-ring rounded-xl bg-white/50 p-1 transition-all duration-300">
                            <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1 ml-3 mt-2">Kelas/Semester</label>
                            <input type="text" id="grade" required placeholder="Contoh: VII/Ganjil" 
                                class="w-full px-3 py-2 bg-transparent border-none focus:ring-0 text-slate-800 placeholder-slate-400 font-medium">
                        </div>
                    </div>

                    <!-- Topic & Time -->
                    <div class="space-y-4">
                        <div class="input-focus-ring rounded-xl bg-white/50 p-1 transition-all duration-300">
                            <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1 ml-3 mt-2">Topik/Materi</label>
                            <input type="text" id="topic" required placeholder="Contoh: Persamaan Linear Satu Variabel" 
                                class="w-full px-3 py-2 bg-transparent border-none focus:ring-0 text-slate-800 placeholder-slate-400 font-medium">
                        </div>
                        
                        <div class="grid grid-cols-2 gap-4">
                            <div class="input-focus-ring rounded-xl bg-white/50 p-1 transition-all duration-300">
                                <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1 ml-3 mt-2">Alokasi Waktu</label>
                                <select id="duration" class="w-full px-3 py-2 bg-transparent border-none focus:ring-0 text-slate-800 font-medium">
                                    <option value="2x40 menit">2 x 40 menit</option>
                                    <option value="2x45 menit">2 x 45 menit</option>
                                    <option value="3x45 menit">3 x 45 menit</option>
                                    <option value="4x45 menit">4 x 45 menit</option>
                                </select>
                            </div>
                            <div class="input-focus-ring rounded-xl bg-white/50 p-1 transition-all duration-300">
                                <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1 ml-3 mt-2">Model Pembelajaran</label>
                                <select id="model" class="w-full px-3 py-2 bg-transparent border-none focus:ring-0 text-slate-800 font-medium">
                                    <option value="Problem Based Learning">Problem Based Learning</option>
                                    <option value="Project Based Learning">Project Based Learning</option>
                                    <option value="Discovery Learning">Discovery Learning</option>
                                    <option value="Inquiry Learning">Inquiry Learning</option>
                                    <option value="STEAM">STEAM</option>
                                    <option value="Collaborative Learning">Collaborative Learning</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- Competencies -->
                    <div class="space-y-4">
                        <div class="input-focus-ring rounded-xl bg-white/50 p-4 transition-all duration-300">
                            <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Capaian Pembelajaran (CP)</label>
                            <textarea id="cp" rows="3" placeholder="Masukkan Capaian Pembelajaran..." 
                                class="w-full bg-transparent border-none focus:ring-0 text-slate-800 placeholder-slate-400 text-sm resize-none"></textarea>
                        </div>
                        
                        <div class="input-focus-ring rounded-xl bg-white/50 p-4 transition-all duration-300">
                            <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Tujuan Pembelajaran (TP)</label>
                            <textarea id="tp" rows="3" placeholder="Pisahkan dengan enter untuk multiple TP..." 
                                class="w-full bg-transparent border-none focus:ring-0 text-slate-800 placeholder-slate-400 text-sm resize-none"></textarea>
                        </div>
                    </div>

                    <!-- Profile Pelajar Pancasila -->
                    <div class="bg-gradient-to-r from-indigo-50 to-purple-50 rounded-xl p-4 border border-indigo-100">
                        <label class="block text-sm font-semibold text-indigo-900 mb-3">Profil Pelajar Pancasila</label>
                        <div class="grid grid-cols-2 gap-2">
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Beriman, Bertakwa, dan Berakhlak Mulia" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Beriman & Bertakwa</span>
                            </label>
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Mandiri" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Mandiri</span>
                            </label>
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Bergotong Royong" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Bergotong Royong</span>
                            </label>
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Kreatif" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Kreatif</span>
                            </label>
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Bernalar Kritis" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Bernalar Kritis</span>
                            </label>
                            <label class="flex items-center space-x-2 cursor-pointer hover:bg-white/50 p-2 rounded-lg transition-colors">
                                <input type="checkbox" value="Berkebinekaan Global" class="rounded text-indigo-600 focus:ring-indigo-500">
                                <span class="text-xs text-slate-700">Berkebinekaan Global</span>
                            </label>
                        </div>
                    </div>

                    <!-- Generate Button -->
                    <button type="submit" id="generateBtn" class="w-full py-4 bg-gradient-to-r from-indigo-600 to-purple-600 text-white font-bold rounded-xl shadow-lg hover:shadow-2xl transform hover:scale-[1.02] transition-all duration-300 flex items-center justify-center gap-2 group">
                        <svg class="w-6 h-6 group-hover:rotate-12 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                        <span>Generate RPP Mendalam</span>
                    </button>
                </form>
            </div>

            <!-- Output Preview -->
            <div class="relative">
                <div id="emptyState" class="glass rounded-3xl p-12 text-center border border-white/50 h-full flex flex-col items-center justify-center min-h-[600px]">
                    <div class="w-32 h-32 bg-gradient-to-br from-indigo-100 to-purple-100 rounded-full flex items-center justify-center mb-6 animate-float">
                        <svg class="w-16 h-16 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                    </div>
                    <h3 class="font-display text-2xl font-bold text-slate-800 mb-2">Siap Generate</h3>
                    <p class="text-slate-600 max-w-sm">Isi form di sebelah kiri dan klik tombol generate untuk membuat rencana pembelajaran lengkap dengan struktur Kurikulum Merdeka.</p>
                </div>

                <div id="loadingState" class="hidden glass rounded-3xl p-12 text-center border border-white/50 h-full flex flex-col items-center justify-center min-h-[600px]">
                    <div class="relative w-24 h-24 mb-6">
                        <div class="absolute inset-0 border-4 border-indigo-200 rounded-full"></div>
                        <div class="absolute inset-0 border-4 border-indigo-600 rounded-full border-t-transparent animate-spin"></div>
                        <div class="absolute inset-0 flex items-center justify-center">
                            <svg class="w-10 h-10 text-indigo-600 animate-pulse" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z"></path></svg>
                        </div>
                    </div>
                    <h3 class="font-display text-xl font-bold text-slate-800 mb-2">AI Sedang Menganalisis...</h3>
                    <p class="text-slate-600 text-sm" id="loadingText">Menganalisis kompetensi dasar...</p>
                </div>

                <div id="resultContainer" class="hidden space-y-6">
                    <!-- Action Bar -->
                    <div class="flex justify-between items-center glass rounded-2xl p-4 border border-white/50 sticky top-20 z-30">
                        <div class="flex items-center gap-2">
                            <div class="w-3 h-3 bg-green-500 rounded-full animate-pulse"></div>
                            <span class="text-sm font-semibold text-slate-700">RPP Generated</span>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="copyToClipboard()" class="px-4 py-2 bg-white hover:bg-slate-50 text-slate-700 rounded-lg text-sm font-medium transition-colors border border-slate-200 flex items-center gap-2">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                                Copy
                            </button>
                            <button onclick="downloadPDF()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white rounded-lg text-sm font-medium transition-colors flex items-center gap-2">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
                                Download PDF
                            </button>
                        </div>
                    </div>

                    <!-- RPP Content -->
                    <div id="rppContent" class="glass rounded-3xl p-8 border border-white/50 shadow-xl space-y-8 text-slate-800">
                        <!-- Content will be injected here -->
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Features Section -->
    <section id="features" class="relative z-10 py-20 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto">
        <div class="text-center mb-16">
            <h2 class="font-display text-4xl font-bold text-slate-900 mb-4">Mengapa Menggunakan AI Generator?</h2>
            <p class="text-lg text-slate-600 max-w-2xl mx-auto">Hemat waktu hingga 90% dalam penyusunan RPP dengan tetap mempertahankan kualitas dan kedalaman analisis didaktis.</p>
        </div>

        <div class="grid md:grid-cols-3 gap-8">
            <div class="glass rounded-2xl p-8 hover-lift border border-white/50">
                <div class="w-14 h-14 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-xl flex items-center justify-center text-white mb-6 shadow-lg">
                    <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                </div>
                <h3 class="font-display text-xl font-bold text-slate-900 mb-3">Cepat & Efisien</h3>
                <p class="text-slate-600 leading-relaxed">Generate RPP lengkap dengan analisis komponen dalam hitungan detik, bukan jam. Fokuskan energi Anda pada implementasi, bukan administrasi.</p>
            </div>

            <div class="glass rounded-2xl p-8 hover-lift border border-white/50">
                <div class="w-14 h-14 bg-gradient-to-br from-purple-500 to-pink-600 rounded-xl flex items-center justify-center text-white mb-6 shadow-lg">
                    <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                </div>
                <h3 class="font-display text-xl font-bold text-slate-900 mb-3">Sesuai Standar</h3>
                <p class="text-slate-600 leading-relaxed">Sepenuhnya mengikuti struktur Kurikulum Merdeka dengan Profil Pelajar Pancasila, literasi, dan numerasi terintegrasi.</p>
            </div>

            <div class="glass rounded-2xl p-8 hover-lift border border-white/50">
                <div class="w-14 h-14 bg-gradient-to-br from-orange-500 to-red-600 rounded-xl flex items-center justify-center text-white mb-6 shadow-lg">
                    <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
                </div>
                <h3 class="font-display text-xl font-bold text-slate-900 mb-3">Deep Learning</h3>
                <p class="text-slate-600 leading-relaxed">Bukan sekadar template. AI menganalisis kedalaman materi, membuat essential questions, dan merancang aktivitas higher-order thinking.</p>
            </div>
        </div>
    </section>

    <!-- Examples Section -->
    <section id="examples" class="relative z-10 py-20 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto mb-20">
        <div class="glass rounded-3xl p-8 md:p-12 border border-white/50">
            <h2 class="font-display text-3xl font-bold text-slate-900 mb-8">Contoh Hasil Generate</h2>
            
            <div class="grid md:grid-cols-2 gap-6">
                <div class="bg-white/70 rounded-xl p-6 border border-slate-200 hover:border-indigo-300 transition-colors cursor-pointer group" onclick="loadExample('math')">
                    <div class="flex items-start justify-between mb-4">
                        <div class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center text-blue-600 group-hover:scale-110 transition-transform">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>
                        </div>
                        <span class="text-xs font-semibold text-slate-400">Matematika</span>
                    </div>
                    <h3 class="font-bold text-slate-900 mb-2">Persamaan Linear Dua Variabel</h3>
                    <p class="text-sm text-slate-600">Kelas VIII • Model: PBL • 2x40 menit</p>
                </div>

                <div class="bg-white/70 rounded-xl p-6 border border-slate-200 hover:border-indigo-300 transition-colors cursor-pointer group" onclick="loadExample('science')">
                    <div class="flex items-start justify-between mb-4">
                        <div class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center text-green-600 group-hover:scale-110 transition-transform">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
                        </div>
                        <span class="text-xs font-semibold text-slate-400">IPA</span>
                    </div>
                    <h3 class="font-bold text-slate-900 mb-2">Sistem Pencernaan Manusia</h3>
                    <p class="text-sm text-slate-600">Kelas VIII • Model: Inquiry • 3x45 menit</p>
                </div>

                <div class="bg-white/70 rounded-xl p-6 border border-slate-200 hover:border-indigo-300 transition-colors cursor-pointer group" onclick="loadExample('language')">
                    <div class="flex items-start justify-between mb-4">
                        <div class="w-12 h-12 bg-purple-100 rounded-lg flex items-center justify-center text-purple-600 group-hover:scale-110 transition-transform">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"></path></svg>
                        </div>
                        <span class="text-xs font-semibold text-slate-400">Bahasa Indonesia</span>
                    </div>
                    <h3 class="font-bold text-slate-900 mb-2">Teks Editorial</h3>
                    <p class="text-sm text-slate-600">Kelas XII • Model: Discovery • 2x45 menit</p>
                </div>

                <div class="bg-white/70 rounded-xl p-6 border border-slate-200 hover:border-indigo-300 transition-colors cursor-pointer group" onclick="loadExample('social')">
                    <div class="flex items-start justify-between mb-4">
                        <div class="w-12 h-12 bg-orange-100 rounded-lg flex items-center justify-center text-orange-600 group-hover:scale-110 transition-transform">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3.055 11H5a2 2 0 012 2v1a2 2 0 002 2 2 2 0 012 2v2.945M8 3.935V5.5A2.5 2.5 0 0010.5 8h.5a2 2 0 012 2 2 2 0 104 0 2 2 0 012-2h1.064M15 20.488V18a2 2 0 012-2h3.064M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        </div>
                        <span class="text-xs font-semibold text-slate-400">IPS</span>
                    </div>
                    <h3 class="font-bold text-slate-900 mb-2">Perdagangan Internasional</h3>
                    <p class="text-sm text-slate-600">Kelas IX • Model: Project Based • 4x45 menit</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="relative z-10 border-t border-slate-200 bg-white/50 backdrop-blur-lg">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
            <div class="flex flex-col md:flex-row justify-between items-center">
                <div class="flex items-center space-x-3 mb-4 md:mb-0">
                    <div class="w-8 h-8 bg-gradient-to-br from-indigo-600 to-purple-600 rounded-lg flex items-center justify-center text-white font-bold text-sm">
                        AI
                    </div>
                    <span class="font-display font-bold text-lg text-slate-800">DeepLearning Plan</span>
                </div>
                <div class="text-slate-500 text-sm">
                    © 2024 AI Rencana Pembelajaran Generator. Dibuat untuk pendidik Indonesia.
                </div>
            </div>
        </div>
    </footer>

    <script>
        // Typing effect for hero text
        const texts = ['Mendalam', 'Komprehensif', 'Terstruktur', 'Inovatif'];
        let textIndex = 0;
        let charIndex = 0;
        let isDeleting = false;
        const heroText = document.getElementById('hero-text');

        function typeEffect() {
            const currentText = texts[textIndex];
            
            if (isDeleting) {
                heroText.textContent = currentText.substring(0, charIndex - 1);
                charIndex--;
            } else {
                heroText.textContent = currentText.substring(0, charIndex + 1);
                charIndex++;
            }

            let typeSpeed = isDeleting ? 50 : 100;

            if (!isDeleting && charIndex === currentText.length) {
                typeSpeed = 2000;
                isDeleting = true;
            } else if (isDeleting && charIndex === 0) {
                isDeleting = false;
                textIndex = (textIndex + 1) % texts.length;
                typeSpeed = 500;
            }

            setTimeout(typeEffect, typeSpeed);
        }

        // Initialize typing effect
        setTimeout(typeEffect, 1000);

        // Scroll to section
        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        // Reset form
        function resetForm() {
            document.getElementById('rppForm').reset();
            document.getElementById('emptyState').classList.remove('hidden');
            document.getElementById('resultContainer').classList.add('hidden');
        }

        // Show demo
        function showDemo() {
            loadExample('math');
            scrollToSection('generator');
        }

        // Load example data
        function loadExample(type) {
            const examples = {
                math: {
                    subject: 'Matematika',
                    grade: 'VIII/Ganjil',
                    topic: 'Persamaan Linear Dua Variabel (SPLDV)',
                    duration: '2x40 menit',
                    model: 'Problem Based Learning',
                    cp: 'Peserta didik memahami konsep persamaan linear dua variabel dan sistemnya, serta menyelesaikan masalah kontekstual yang berkaitan dengan SPLDV.',
                    tp: '1. Menjelaskan konsep SPLDV\n2. Menyelesaikan SPLDV dengan metode substitusi\n3. Menyelesaikan masalah nyata menggunakan SPLDV',
                    profiles: ['Bernalar Kritis', 'Kreatif']
                },
                science: {
                    subject: 'Ilmu Pengetahuan Alam',
                    grade: 'VIII/Ganjil',
                    topic: 'Sistem Pencernaan Manusia',
                    duration: '3x45 menit',
                    model: 'Inquiry Learning',
                    cp: 'Peserta didik memahami sistem pencernaan manusia, enzim-enzim yang terlibat, serta gangguan dan penyakit pada sistem pencernaan.',
                    tp: '1. Mengidentifikasi organ pencernaan\n2. Menjelaskan proses pencernaan mekanik dan kimiawi\n3. Menganalisis hubungan pola makan dan kesehatan pencernaan',
                    profiles: ['Mandiri', 'Bernalar Kritis']
                },
                language: {
                    subject: 'Bahasa Indonesia',
                    grade: 'XII/Ganjil',
                    topic: 'Teks Editorial (Tajuk Rencana)',
                    duration: '2x45 menit',
                    model: 'Discovery Learning',
                    cp: 'Peserta didik memahami struktur dan kaidah kebahasaan teks editorial, serta mampu menganalisis isu aktual dalam bentuk tulisan editorial.',
                    tp: '1. Mengidentifikasi struktur teks editorial\n2. Menganalisis penggunaan bahasa persuasif\n3. Menulis teks editorial sederhana tentang isu lokal',
                    profiles: ['Bernalar Kritis', 'Kreatif', 'Berkebinekaan Global']
                },
                social: {
                    subject: 'Ilmu Pengetahuan Sosial',
                    grade: 'IX/Genap',
                    topic: 'Perdagangan Internasional',
                    duration: '4x45 menit',
                    model: 'Project Based Learning',
                    cp: 'Peserta didik memahami konsep perdagangan internasional, kebijakan perdagangan, serta dampak globalisasi terhadap perekonomian nasional.',
                    tp: '1. Menjelaskan motif dan manfaat perdagangan internasional\n2. Menganalisis kebijakan impor dan ekspor\n3. Membuat proposal ekspor produk lokal',
                    profiles: ['Bergotong Royong', 'Berkebinekaan Global', 'Kreatif']
                }
            };

            const data = examples[type];
            if (data) {
                document.getElementById('subject').value = data.subject;
                document.getElementById('grade').value = data.grade;
                document.getElementById('topic').value = data.topic;
                document.getElementById('duration').value = data.duration;
                document.getElementById('model').value = data.model;
                document.getElementById('cp').value = data.cp;
                document.getElementById('tp').value = data.tp;
                
                // Check checkboxes
                document.querySelectorAll('input[type="checkbox"]').forEach(cb => {
                    cb.checked = data.profiles.includes(cb.value);
                });

                // Trigger generation
                generateRPP(new Event('submit'));
            }
        }

        // Generate RPP
        async function generateRPP(e) {
            e.preventDefault();
            
            const formData = {
                subject: document.getElementById('subject').value,
                grade: document.getElementById('grade').value,
                topic: document.getElementById('topic').value,
                duration: document.getElementById('duration').value,
                model: document.getElementById('model').value,
                cp: document.getElementById('cp').value,
                tp: document.getElementById('tp').value,
                profiles: Array.from(document.querySelectorAll('input[type="checkbox"]:checked')).map(cb => cb.value)
            };

            // Show loading
            document.getElementById('emptyState').classList.add('hidden');
            document.getElementById('resultContainer').classList.add('hidden');
            document.getElementById('loadingState').classList.remove('hidden');

            // Simulate AI processing steps
            const steps = [
                'Menganalisis kompetensi dasar...',
                'Merumuskan essential questions...',
                'Mendesain aktivitas pembelajaran...',
                'Menyusun instrumen penilaian...',
                'Memvalidasi alur tujuan pembelajaran...',
                'Finalisasi dokumen RPP...'
            ];

            for (let i = 0; i < steps.length; i++) {
                document.getElementById('loadingText').textContent = steps[i];
                await new Promise(resolve => setTimeout(resolve, 600));
            }

            // Generate content
            const rppHTML = generateRPPContent(formData);
            document.getElementById('rppContent').innerHTML = rppHTML;
            
            // Show result
            document.getElementById('loadingState').classList.add('hidden');
            document.getElementById('resultContainer').classList.remove('hidden');
            
            // Scroll to result
            document.getElementById('resultContainer').scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        function generateRPPContent(data) {
            const date = new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'long', year: 'numeric' });
            
            return `
                <div class="border-b-2 border-indigo-600 pb-6 mb-8">
                    <div class="flex justify-between items-start">
                        <div>
                            <h1 class="font-display text-3xl font-bold text-slate-900 mb-2">RENCANA PELAKSANAAN PEMBELAJARAN (RPP)</h1>
                            <p class="text-slate-600">Satuan Pendidikan: SMP/MTs/SMA/MA/SMK</p>
                            <p class="text-slate-600">Tahun Ajaran: 2024/2025</p>
                        </div>
                        <div class="text-right text-sm text-slate-500">
                            <p>Generated: ${date}</p>
                            <p>Model: ${data.model}</p>
                        </div>
                    </div>
                </div>

                <div class="space-y-6">
                    <!-- Identitas -->
                    <div class="bg-slate-50 rounded-xl p-6 border border-slate-200">
                        <h3 class="font-display text-lg font-bold text-indigo-900 mb-4 flex items-center gap-2">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V8a2 2 0 00-2-2h-5m-4 0V5a2 2 0 114 0v1m-4 0a2 2 0 104 0m-5 8a2 2 0 100-4 2 2 0 000 4zm0 0c1.306 0 2.417.835 2.83 2M9 14a3.001 3.001 0 00-2.83 2M15 11h3m-3 4h2"></path></svg>
                            A. Informasi Umum
                        </h3>
                        <div class="grid grid-cols-2 gap-4 text-sm">
                            <div><span class="font-semibold text-slate-700">Mata Pelajaran:</span> ${data.subject}</div>
                            <div><span class="font-semibold text-slate-700">Kelas/Semester:</span> ${data.grade}</div>
                            <div><span class="font-semibold text-slate-700">Materi:</span> ${data.topic}</div>
                            <div><span class="font-semibold text-slate-700">Alokasi Waktu:</span> ${data.duration}</div>
                        </div>
                    </div>

                    <!-- CP & TP -->
                    <div class="space-y-4">
                        <div class="bg-indigo-50 rounded-xl p-6 border border-indigo-100">
                            <h3 class="font-display text-lg font-bold text-indigo-900 mb-3">B. Capaian Pembelajaran (CP)</h3>
                            <p class="text-slate-700 leading-relaxed">${data.cp}</p>
                        </div>
                        
                        <div class="bg-purple-50 rounded-xl p-6 border border-purple-100">
                            <h3 class="font-display text-lg font-bold text-purple-900 mb-3">C. Tujuan Pembelajaran (TP)</h3>
                            <ul class="list-decimal list-inside space-y-2 text-slate-700">
                                ${data.tp.split('\n').map(tp => tp.trim() ? `<li class="leading-relaxed">${tp.replace(/^\d+\.\s*/, '')}</li>` : '').join('')}
                            </ul>
                        </div>
                    </div>

                    <!-- Profil Pelajar -->
                    <div class="bg-gradient-to-r from-amber-50 to-orange-50 rounded-xl p-6 border border-amber-200">
                        <h3 class="font-display text-lg font-bold text-amber-900 mb-3">D. Profil Pelajar Pancasila yang Dikembangkan</h3>
                        <div class="flex flex-wrap gap-2">
                            ${data.profiles.map(profile => `
                                <span class="px-3 py-1 bg-white rounded-full text-sm font-medium text-amber-800 border border-amber-200 shadow-sm">
                                    ${profile}
                                </span>
                            `).join('')}
                        </div>
                    </div>

                    <!-- Sarana & Prasarana -->
                    <div class="bg-slate-50 rounded-xl p-6 border border-slate-200">
                        <h3 class="font-display text-lg font-bold text-slate-900 mb-3">E. Sarana dan Prasarana</h3>
                        <ul class="list-disc list-inside space-y-1 text-sm text-slate-700">
                            <li>Laptop/ Komputer dan LCD Proyektor</li>
                            <li>Modul/ Bahan Ajar ${data.subject}</li>
                            <li>LKPD (Lembar Kerja Peserta Didik)</li>
                            <li>Media pembelajaran interaktif</li>
                            <li>Akses internet untuk riset</li>
                        </ul>
                    </div>

                    <!-- Target Peserta Didik -->
                    <div class="bg-slate-50 rounded-xl p-6 border border-slate-200">
                        <h3 class="font-display text-lg font-bold text-slate-900 mb-3">F. Target Peserta Didik</h3>
                        <ul class="list-disc list-inside space-y-1 text-sm text-slate-700">
                            <li>Peserta didik reguler dengan gaya belajar visual, auditori, dan kinestetik</li>
                            <li>Peserta didik dengan kebutuhan khusus (disesuaikan dengan kondisi kelas)</li>
                        </ul>
                    </div>

                    <!-- Model Pembelajaran -->
                    <div class="bg-blue-50 rounded-xl p-6 border border-blue-100">
                        <h3 class="font-display text-lg font-bold text-blue-900 mb-3">G. Model Pembelajaran</h3>
                        <p class="text-slate-700 font-medium mb-2">${data.model}</p>
                        <p class="text-sm text-slate-600 leading-relaxed">
                            ${getModelDescription(data.model)}
                        </p>
                    </div>

                    <!-- Langkah-Langkah -->
                    <div class="space-y-6">
                        <h3 class="font-display text-xl font-bold text-slate-900 border-b pb-2">H. Langkah-Langkah Pembelajaran</h3>
                        
                        ${generateLearningSteps(data.model, data.topic)}
                    </div>

                    <!-- Penilaian -->
                    <div class="space-y-4">
                        <h3 class="font-display text-xl font-bold text-slate-900 border-b pb-2">I. Penilaian</h3>
                        
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="bg-green-50 rounded-xl p-4 border border-green-200">
                                <h4 class="font-bold text-green-900 mb-2">Penilaian Sikap</h4>
                                <ul class="text-sm text-slate-700 space-y-1">
                                    <li>• Observasi aktivitas kelompok</li>
                                    <li>• Penilaian diri (self-assessment)</li>
                                    <li>• Jurnal catatan guru</li>
                                </ul>
                            </div>
                            
                            <div class="bg-blue-50 rounded-xl p-4 border border-blue-200">
                                <h4 class="font-bold text-blue-900 mb-2">Penilaian Pengetahuan</h4>
                                <ul class="text-sm text-slate-700 space-y-1">
                                    <li>• Tes formatif (kuis digital)</li>
                                    <li>• Pertanyaan esensial</li>
                                    <li>• Diskusi kelas</li>
                                </ul>
                            </div>
                            
                            <div class="bg-purple-50 rounded-xl p-4 border border-purple-200">
                                <h4 class="font-bold text-purple-900 mb-2">Penilaian Keterampilan</h4>
                                <ul class="text-sm text-slate-700 space-y-1">
                                    <li>• Praktik/ Proyek</li>
                                    <li>• Presentasi hasil kerja</li>
                                    <li>• Portofolio</li>
                                </ul>
                            </div>
                        </div>

                        <div class="bg-slate-50 rounded-xl p-6 border border-slate-200">
                            <h4 class="font-bold text-slate-900 mb-3">Instrumen Penilaian</h4>
                            <div class="overflow-x-auto">
                                <table class="w-full text-sm">
                                    <thead class="bg-slate-100">
                                        <tr>
                                            <th class="px-4 py-2 text-left rounded-tl-lg">Teknik</th>
                                            <th class="px-4 py-2 text-left">Bentuk Instrumen</th>
                                            <th class="px-4 py-2 text-left rounded-tr-lg">Kriteria</th>
                                        </tr>
                                    </thead>
                                    <tbody class="divide-y divide-slate-200">
                                        <tr>
                                            <td class="px-4 py-3">Tes Tertulis</td>
                                            <td class="px-4 py-3">Essay, Pilihan Ganda</td>
                                            <td class="px-4 py-3">Ketepatan konsep dan analisis</td>
                                        </tr>
                                        <tr>
                                            <td class="px-4 py-3">Penugasan</td>
                                            <td class="px-4 py-3">LKPD, Proyek</td>
                                            <td class="px-4 py-3">Kelengkapan, kreativitas, kerjasama</td>
                                        </tr>
                                        <tr>
                                            <td class="px-4 py-3">Praktik</td>
                                            <td class="px-4 py-3">Demonstrasi, Presentasi</td>
                                            <td class="px-4 py-3">Proses dan hasil kerja</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- Refleksi -->
                    <div class="bg-amber-50 rounded-xl p-6 border border-amber-200">
                        <h3 class="font-display text-lg font-bold text-amber-900 mb-3">J. Refleksi Guru</h3>
                        <div class="space-y-3 text-sm text-slate-700">
                            <p><strong>1. Ketercapaian TP:</strong> Peserta didik mampu mencapai tujuan pembelajaran melalui aktivitas...</p>
                            <p><strong>2. Hambatan:</strong> Beberapa siswa masih kesulitan dalam...</p>
                            <p><strong>3. Solusi:</strong> Perlu pendekatan diferensiasi dengan...</p>
                            <p><strong>4. Tindak Lanjut:</strong> Remedial untuk siswa yang belum tuntas, pengayaan untuk yang sudah...</p>
                        </div>
                    </div>

                    <!-- Daftar Pustaka -->
                    <div class="border-t pt-6">
                        <h3 class="font-display text-lg font-bold text-slate-900 mb-3">Daftar Pustaka</h3>
                        <ul class="text-sm text-slate-600 space-y-1">
                            <li>Kementerian Pendidikan dan Kebudayaan. (2022). <em>Capaian Pembelajaran dan Tujuan Pembelajaran dalam Kurikulum Merdeka</em>. Jakarta: Kemendikbud.</li>
                            <li>Buku Paket ${data.subject} Kelas ${data.grade.split('/')[0]} Kurikulum Merdeka.</li>
                        </ul>
                    </div>

                    <!-- Lampiran -->
                    <div class="border-t pt-6">
                        <h3 class="font-display text-lg font-bold text-slate-900 mb-3">Lampiran</h3>
                        <ul class="list-disc list-inside text-sm text-slate-600 space-y-1">
                            <li>Lampiran 1: LKPD (Lembar Kerja Peserta Didik)</li>
                            <li>Lampiran 2: Instrumen Penilaian Sikap</li>
                            <li>Lampiran 3: Kisi-kisi Soal dan Rubrik Penilaian</li>
                            <li>Lampiran 4: Bahan Bacaan/ Handout</li>
                        </ul>
                    </div>
                </div>
            `;
        }

        function getModelDescription(model) {
            const descriptions = {
                'Problem Based Learning': 'Pembelajaran yang berorientasi pada penyelesaian masalah autentik untuk mengembangkan kemampuan berpikir kritis dan keterampilan kolaborasi.',
                'Project Based Learning': 'Pembelajaran berbasis proyek yang menghasilkan produk/ presentasi nyata, mengintegrasikan multiple disiplin ilmu.',
                'Discovery Learning': 'Peserta didik menemukan konsep sendiri melalui pengamatan, analisis data, dan penyimpulan.',
                'Inquiry Learning': 'Pembelajaran berbasis inkuiri dengan tahapan orientasi, merumuskan masalah, mengumpulkan data, menyimpulkan, dan mengkomunikasikan.',
                'STEAM': 'Integrasi Science, Technology, Engineering, Arts, dan Mathematics dalam satu aktivitas pembelajaran terpadu.',
                'Collaborative Learning': 'Pembelajaran kolaboratif dengan struktur kerja kelompok yang terorganisir untuk mencapai tujuan bersama.'
            };
            return descriptions[model] || 'Model pembelajaran modern yang mengutamakan partisipasi aktif peserta didik.';
        }

        function generateLearningSteps(model, topic) {
            const steps = {
                'Problem Based Learning': [
                    { phase: 'Orientasi', activity: `Guru menyajikan fenomena/ masalah nyata terkait ${topic} melalui video/ gambar. Peserta didik mengamati dan mengidentifikasi masalah.` },
                    { phase: 'Mengorganisasikan Siswa', activity: 'Peserta didik dibagi dalam kelompok kooperatif (4-5 orang heterogen). Distribusi LKPD dan penjelasan tugas.' },
                    { phase: 'Membimbing Investigasi', activity: `Kelompok menganalisis masalah, merumuskan hipotesis, dan mencari informasi tentang ${topic} dari berbagai sumber (buku, internet, wawancara).` },
                    { phase: 'Mengembangkan dan Menyajikan Hasil', activity: 'Kelompok menyusun solusi/ jawaban dalam bentuk poster, infografis, atau presentasi digital.' },
                    { phase: 'Menganalisis dan Mengevaluasi', activity: 'Presentasi kelompok, tanya jawab, dan diskusi kelas. Guru memfasilitasi refleksi proses berpikir.' }
                ],
                'Project Based Learning': [
                    { phase: 'Start with the Essential Question', activity: `Guru mengajukan pertanyaan esensial: "Bagaimana kita bisa menerapkan konsep ${topic} dalam kehidupan sehari-hari?"` },
                    { phase: 'Design a Plan for the Project', activity: 'Perencanaan proyek: menentukan tujuan, timeline, peran anggota kelompok, dan kriteria penilaian (rubrik).' },
                    { phase: 'Create a Schedule', activity: 'Penyusunan jadwal aktivitas: riset, desain, produksi, dan persiapan presentasi.' },
                    { phase: 'Monitor the Students', activity: 'Guru memantau progress, memberikan scaffolding, dan melakukan konferensi kelompok.' },
                    { phase: 'Assess the Outcome', activity: 'Presentasi produk/ proyek, penilaian diri, penilaian antar teman, dan refleksi proses.' }
                ],
                'Discovery Learning': [
                    { phase: 'Stimulation', activity: `Guru memberikan stimulus berupa fenomena/ pertanyaan menantang terkait ${topic} untuk membangkitkan rasa ingin tahu.` },
                    { phase: 'Problem Statement', activity: 'Peserta didik mengidentifikasi masalah, merumuskan pertanyaan investigasi, dan membuat prediksi.' },
                    { phase: 'Data Collection', activity: 'Pengumpulan data melalui eksperimen, observasi, wawancara, atau studi literatur.' },
                    { phase: 'Data Processing', activity: 'Mengolah dan menganalisis data yang telah dikumpulkan menggunakan grafik, tabel, atau diagram.' },
                    { phase: 'Verification', activity: 'Memeriksa kebenaran hasil analisis dengan teori/ referensi. Menyimpulkan konsep.' },
                    { phase: 'Generalization', activity: 'Menarik kesimpulan umum dan mengkomunikasikan hasil discovery dalam bentuk laporan atau presentasi.' }
                ],
                'Inquiry Learning': [
                    { phase: 'Orientasi', activity: 'Apersepsi, motivasi, dan penyampaian tujuan pembelajaran. Guru menyajikan fenomena yang menarik.' },
                    { phase: 'Merumuskan Masalah', activity: 'Peserta didik merumuskan pertanyaan inquiry yang terukur dan dapat diinvestigasi.' },
                    { phase: 'Mengajukan Hipotesis', activity: 'Membuat dugaan sementara atau prediksi jawaban atas pertanyaan yang dirumuskan.' },
                    { phase: 'Mengumpulkan Data', activity: 'Melakukan eksperimen, observasi, atau riset untuk menguji hipotesis.' },
                    { phase: 'Menguji Hipotesis', activity: 'Menganalisis data dan menentukan apakah hipotesis diterima atau ditolak.' },
                    { phase: 'Menarik Kesimpulan', activity: 'Menyusun kesimpulan berdasarkan hasil pengujian dan mengkomunikasikannya.' }
                ],
                'STEAM': [
                    { phase: 'Science', activity: `Eksplorasi konsep ilmiah dasar ${topic} melalui pengamatan dan eksperimen sederhana.` },
                    { phase: 'Technology', activity: 'Pemanfaatan aplikasi digital, simulasi, atau software untuk menganalisis data dan membuat desain.' },
                    { phase: 'Engineering', activity: 'Merancang dan membuat prototipe/ model yang mengintegrasikan konsep yang dipelajari.' },
                    { phase: 'Arts', activity: 'Desain estetika produk, presentasi kreatif, atau integrasi seni dalam penyajian hasil.' },
                    { phase: 'Mathematics', activity: 'Analisis kuantitatif, pengukuran, kalkulasi, dan interpretasi data matematis.' }
                ],
                'Collaborative Learning': [
                    { phase: 'Pre-test/ Konsep Dasar', activity: 'Penilaian awal dan penjelasan konsep dasar yang diperlukan.' },
                    { phase: 'Pembentukan Kelompok', activity: 'Pembagian kelompok heterogen dengan peran spesifik (fasilitator, recorder, reporter, time keeper).' },
                    { phase: 'Team Learning', activity: 'Diskusi kelompok, peer tutoring, dan kolaborasi menyelesaikan tugas kompleks.' },
                    { phase: 'Team Assistance', activity: 'Guru memberikan bantuan spesifik pada kelompok yang mengalami kesulitan.' },
                    { phase: 'Test/ Presentasi', activity: 'Evaluasi individual atau presentasi kelompok. Penghargaan tim (team recognition).' }
                ]
            };

            const selectedSteps = steps[model] || steps['Problem Based Learning'];
            
            return selectedSteps.map((step, index) => `
                <div class="flex gap-4 p-4 bg-white rounded-xl border border-slate-200 hover:border-indigo-300 transition-colors">
                    <div class="flex-shrink-0 w-10 h-10 bg-indigo-100 rounded-full flex items-center justify-center text-indigo-700 font-bold">
                        ${index + 1}
                    </div>
                    <div class="flex-1">
                        <h4 class="font-bold text-slate-900 mb-1">${step.phase}</h4>
                        <p class="text-slate-600 text-sm leading-relaxed">${step.activity}</p>
                    </div>
                </div>
            `).join('');
        }

        function copyToClipboard() {
            const content = document.getElementById('rppContent').innerText;
            navigator.clipboard.writeText(content).then(() => {
                alert('RPP berhasil disalin ke clipboard!');
            });
        }

        function downloadPDF() {
            alert('Fitur download PDF akan membuka dialog print. Silakan pilih "Save as PDF" sebagai destination printer.');
            window.print();
        }

        function toggleTheme() {
            // Simple theme toggle demonstration
            document.body.classList.toggle('bg-slate-900');
            document.body.classList.toggle('text-white');
            document.body.classList.toggle('bg-slate-50');
            document.body.classList.toggle('text-slate-800');
        }

        // Navbar scroll effect
        window.addEventListener('scroll', () => {
            const navbar = document.getElementById('navbar');
            if (window.scrollY > 50) {
                navbar.classList.add('shadow-md');
                navbar.classList.remove('glass');
                navbar.classList.add('bg-white/90');
            } else {
                navbar.classList.remove('shadow-md');
                navbar.classList.add('glass');
                navbar.classList.remove('bg-white/90');
            }
        });
    </script>
</body>
</html>

