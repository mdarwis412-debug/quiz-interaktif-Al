<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz AI MTs DDI Kel Baru</title>

    <!-- Meta Tag Open Graph -->
    <meta property="og:title" content="Quiz AI - MTs DDI Kelurahan Baru">
    <meta property="og:description" content="Aplikasi kuis interaktif Informatika & AI untuk siswa MTs DDI Kelurahan Baru. Kerjakan kuis, dapatkan skor, dan lihat pembahasan lengkap!">
    <meta property="og:type" content="website">

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#ecfdf5',
                            100: '#d1fae5',
                            500: '#10b981',
                            600: '#059669',
                            700: '#047857',
                            900: '#064e3b',
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-950 text-slate-100 font-sans min-h-screen flex flex-col justify-between selection:bg-emerald-500 selection:text-white">

    <!-- Top Bar Navigation -->
    <header class="sticky top-0 z-40 bg-slate-900/90 backdrop-blur-md border-b border-slate-800 shadow-md">
        <div class="max-w-4xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-emerald-500 to-teal-400 flex items-center justify-center text-slate-950 font-black text-xl shadow-lg shadow-emerald-500/20">
                    <i class="fa-solid fa-robot text-lg"></i>
                </div>
                <div>
                    <h1 class="font-bold text-sm sm:text-base leading-tight text-white">Quiz AI</h1>
                    <p class="text-[11px] text-emerald-400 font-medium">MTs DDI Kelurahan Baru</p>
                </div>
            </div>

            <!-- Header Controls -->
            <div class="flex items-center space-x-2">
                <span id="userBadge" class="hidden text-xs bg-slate-800 border border-slate-700 text-slate-300 px-3 py-1.5 rounded-lg font-medium">
                    <i class="fa-solid fa-user-graduate text-emerald-400 mr-1.5"></i><span id="badgeName">Siswa</span>
                </span>
                <button id="btnRestartHeader" onclick="confirmRestartQuiz()" class="hidden px-3 py-1.5 text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white border border-slate-700 rounded-lg transition items-center space-x-1">
                    <i class="fa-solid fa-rotate-right text-emerald-400"></i>
                    <span class="hidden sm:inline">Ulangi Kuis</span>
                </button>
                <button id="btnLogoutHeader" onclick="logoutSession()" class="hidden px-3 py-1.5 text-xs bg-red-500/10 hover:bg-red-500/20 text-red-400 border border-red-500/30 rounded-lg transition items-center">
                    <i class="fa-solid fa-right-from-bracket mr-1"></i> Keluar
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-3xl w-full mx-auto px-4 py-6 flex-1 flex flex-col justify-center">

        <!-- 1. LOGIN SCREEN -->
        <section id="loginScreen" class="bg-slate-900/80 border border-slate-800 rounded-2xl p-6 sm:p-8 shadow-2xl text-center backdrop-blur-xl max-w-md mx-auto w-full">
            <div class="w-16 h-16 bg-emerald-500/10 border border-emerald-500/30 rounded-2xl mx-auto flex items-center justify-center text-emerald-400 mb-4 shadow-inner">
                <i class="fa-solid fa-brain text-3xl"></i>
            </div>
            
            <h2 class="text-xl sm:text-2xl font-bold text-white mb-1">Quiz AI</h2>
            <p class="text-xs text-slate-400 mb-6">MTs DDI Kelurahan Baru • Literasi Digital & AI</p>

            <!-- Login Role Switcher -->
            <div class="grid grid-cols-2 gap-2 bg-slate-950 p-1 rounded-xl border border-slate-800 mb-6">
                <button id="tabPeserta" onclick="switchLoginTab('peserta')" class="py-2 text-xs font-bold rounded-lg bg-emerald-500 text-slate-950 transition">
                    <i class="fa-solid fa-user-graduate mr-1"></i> Peserta Siswa
                </button>
                <button id="tabAdmin" onclick="switchLoginTab('admin')" class="py-2 text-xs font-bold rounded-lg text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-user-shield mr-1"></i> Admin / Guru
                </button>
            </div>

            <!-- Form Peserta -->
            <form id="pesertaForm" onsubmit="handlePesertaLogin(event)" class="space-y-4">
                <div class="p-3 bg-emerald-500/10 border border-emerald-500/30 rounded-xl text-left text-[11px] text-emerald-200 mb-2">
                    <i class="fa-solid fa-key mr-1"></i> <strong>Password Masuk Siswa:</strong> <code class="text-white font-mono">1234</code>
                </div>
                <div class="text-left">
                    <label class="text-xs font-bold text-slate-300 block mb-1">Nama Lengkap Siswa</label>
                    <input type="text" id="siswaNama" required placeholder="Contoh: Ahmad Rizki" class="w-full px-4 py-3 rounded-xl bg-slate-950 border border-slate-800 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-emerald-500 transition">
                </div>
                <div class="text-left">
                    <label class="text-xs font-bold text-slate-300 block mb-1">Password Angka Kuis</label>
                    <input type="password" id="siswaPass" required placeholder="Isi dengan angka: 1234" class="w-full px-4 py-3 rounded-xl bg-slate-950 border border-slate-800 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-emerald-500 transition">
                </div>
                <p id="siswaErrorMsg" class="hidden text-xs text-red-400 font-semibold text-left"><i class="fa-solid fa-circle-exclamation"></i> Password angka salah! (Isi: 1234)</p>
                <button type="submit" class="w-full py-3.5 bg-gradient-to-r from-emerald-500 to-teal-400 text-slate-950 font-bold text-xs rounded-xl shadow-lg shadow-emerald-500/20 hover:scale-[1.02] active:scale-95 transition">
                    Masuk & Mulai Kuis <i class="fa-solid fa-arrow-right ml-1"></i>
                </button>
            </form>

            <!-- Form Admin -->
            <form id="adminForm" onsubmit="handleAdminLogin(event)" class="hidden space-y-4">
                <div class="p-3 bg-amber-500/10 border border-amber-500/30 rounded-xl text-left text-[11px] text-amber-200 mb-2">
                    <i class="fa-solid fa-key mr-1"></i> <strong>Kredensial Admin:</strong><br>
                    Username: <code class="text-white font-mono">admin</code> | Password Angka: <code class="text-white font-mono">1234</code>
                </div>
                <div class="text-left">
                    <label class="text-xs font-bold text-amber-300 block mb-1">Username Admin</label>
                    <input type="text" id="adminUser" required placeholder="Isi dengan: admin" class="w-full px-4 py-3 rounded-xl bg-slate-950 border border-slate-800 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-amber-400 transition">
                </div>
                <div class="text-left">
                    <label class="text-xs font-bold text-amber-300 block mb-1">Password Angka</label>
                    <input type="password" id="adminPass" required placeholder="Isi dengan angka: 1234" class="w-full px-4 py-3 rounded-xl bg-slate-950 border border-slate-800 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-amber-400 transition">
                </div>
                <p id="adminErrorMsg" class="hidden text-xs text-red-400 font-semibold text-left"><i class="fa-solid fa-circle-exclamation"></i> Username atau Password angka salah! (Default: admin / 1234)</p>
                <button type="submit" class="w-full py-3.5 bg-gradient-to-r from-amber-500 to-orange-500 text-slate-950 font-bold text-xs rounded-xl shadow-lg hover:scale-[1.02] active:scale-95 transition">
                    Masuk Dashboard Admin <i class="fa-solid fa-lock-open ml-1"></i>
                </button>
            </form>
        </section>

        <!-- 2. ADMIN DASHBOARD -->
        <section id="adminDashboard" class="hidden space-y-6">
            <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <div>
                    <h2 class="text-xl font-bold text-white flex items-center">
                        <i class="fa-solid fa-chart-line text-amber-400 mr-2"></i> Dashboard Admin & Rekap Nilai
                    </h2>
                    <p class="text-xs text-slate-400 mt-1">Sistem Pemantauan Skor Kuis Siswa MTs DDI Kelurahan Baru</p>
                </div>
                <div class="flex items-center space-x-2">
                    <button onclick="exportToCSV()" class="px-3 py-2 bg-slate-800 hover:bg-slate-700 text-slate-200 text-xs rounded-xl border border-slate-700 font-semibold transition">
                        <i class="fa-solid fa-file-csv text-emerald-400 mr-1"></i> Unduh Rekap CSV
                    </button>
                    <button onclick="clearSavedData()" class="px-3 py-2 bg-red-500/10 hover:bg-red-500/20 text-red-400 text-xs rounded-xl border border-red-500/30 font-semibold transition">
                        <i class="fa-solid fa-trash-can mr-1"></i> Hapus Data Rekap
                    </button>
                </div>
            </div>

            <!-- Table Record -->
            <div class="bg-slate-900 border border-slate-800 rounded-2xl overflow-hidden shadow-xl">
                <div class="px-6 py-4 border-b border-slate-800 flex justify-between items-center">
                    <h3 class="text-xs font-bold text-slate-300 uppercase tracking-wider">Daftar Hasil Pengerjaan Siswa (Tersimpan)</h3>
                    <span id="totalParticipantsBadge" class="text-xs font-semibold px-2.5 py-1 bg-slate-800 text-emerald-400 rounded-lg">0 Peserta</span>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-xs text-slate-300">
                        <thead class="bg-slate-950 text-slate-400 uppercase text-[10px] tracking-wider border-b border-slate-800">
                            <tr>
                                <th class="py-3 px-4">Nama Siswa</th>
                                <th class="py-3 px-4 text-center">Benar / Salah</th>
                                <th class="py-3 px-4 text-center">Skor Akhir</th>
                                <th class="py-3 px-4 text-center">Predikat Status</th>
                                <th class="py-3 px-4 text-center">Waktu Simpan</th>
                            </tr>
                        </thead>
                        <tbody id="adminTableBody" class="divide-y divide-slate-800">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- 3. START QUIZ WELCOME SCREEN -->
        <section id="welcomeScreen" class="hidden bg-slate-900/80 border border-slate-800 rounded-2xl p-6 sm:p-8 shadow-2xl text-center backdrop-blur-xl">
            <div class="w-16 h-16 bg-emerald-500/10 border border-emerald-500/30 rounded-2xl mx-auto flex items-center justify-center text-emerald-400 mb-4">
                <i class="fa-solid fa-graduation-cap text-3xl"></i>
            </div>
            
            <h2 class="text-xl sm:text-2xl font-bold text-white mb-2">Quiz AI - MTs DDI Kel Baru</h2>
            <p class="text-xs text-slate-400 max-w-md mx-auto mb-6">Uji pemahaman kamu mengenai konsep Kecerdasan Buatan (AI), Machine Learning, serta etika penggunaan teknologi modern.</p>

            <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 mb-8 text-left max-w-lg mx-auto">
                <div class="bg-slate-950/60 p-3 rounded-xl border border-slate-800/80">
                    <p class="text-[10px] text-slate-400">Total Soal</p>
                    <p class="text-sm font-bold text-white"><i class="fa-solid fa-list-check text-emerald-400 mr-1"></i> 15 Pilihan Ganda</p>
                </div>
                <div class="bg-slate-950/60 p-3 rounded-xl border border-slate-800/80">
                    <p class="text-[10px] text-slate-400">Aturan Jawaban</p>
                    <p class="text-sm font-bold text-amber-400"><i class="fa-solid fa-lock mr-1"></i> Murni di Akhir</p>
                </div>
                <div class="bg-slate-950/60 p-3 rounded-xl border border-slate-800/80 col-span-2 sm:col-span-1">
                    <p class="text-[10px] text-slate-400">Target KKM</p>
                    <p class="text-sm font-bold text-emerald-400"><i class="fa-solid fa-trophy mr-1"></i> Minimal Nilai 70</p>
                </div>
            </div>

            <button onclick="startQuiz()" class="px-8 py-3.5 bg-gradient-to-r from-emerald-500 to-teal-400 text-slate-950 font-extrabold text-sm rounded-xl shadow-lg shadow-emerald-500/20 hover:scale-105 active:scale-95 transition">
                Mulai Kerjakan Soal <i class="fa-solid fa-play ml-1.5"></i>
            </button>
        </section>

        <!-- 4. ACTIVE QUIZ PLAY SCREEN -->
        <section id="quizScreen" class="hidden space-y-4">
            
            <!-- Progress & Question Tracker -->
            <div class="bg-slate-900 border border-slate-800 p-4 rounded-xl shadow-md space-y-3">
                <div class="flex justify-between items-center text-xs font-bold">
                    <span class="text-emerald-400">Soal <span id="currentNum">1</span> dari <span id="totalNum">15</span></span>
                    <span id="answeredCounter" class="text-slate-400 bg-slate-950 px-2.5 py-1 rounded-md border border-slate-800">Dijawab: 0/15</span>
                </div>
                <div class="w-full bg-slate-950 rounded-full h-2 overflow-hidden border border-slate-800">
                    <div id="progressBar" class="bg-gradient-to-r from-emerald-500 to-teal-400 h-full w-0 transition-all duration-300"></div>
                </div>
            </div>

            <!-- Question Card -->
            <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl">
                <h3 id="questionText" class="text-sm sm:text-base font-semibold text-white leading-relaxed mb-6">
                    [Loading Pertanyaan...]
                </h3>

                <!-- Options -->
                <div id="optionsContainer" class="space-y-3">
                    <!-- Dynamic Options -->
                </div>
            </div>

            <!-- Quiz Controls -->
            <div class="flex justify-between items-center pt-2">
                <button id="btnPrev" onclick="navigateQuestion(-1)" class="px-4 py-2.5 bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold rounded-xl border border-slate-700 transition disabled:opacity-40 disabled:cursor-not-allowed">
                    <i class="fa-solid fa-arrow-left mr-1"></i> Sebelumnya
                </button>
                
                <button id="btnNext" onclick="navigateQuestion(1)" class="px-5 py-2.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 text-xs font-extrabold rounded-xl shadow-md transition">
                    Selanjutnya <i class="fa-solid fa-arrow-right ml-1"></i>
                </button>

                <button id="btnFinish" onclick="showFinishConfirmation()" class="hidden px-5 py-2.5 bg-gradient-to-r from-amber-500 to-orange-500 hover:from-amber-400 hover:to-orange-400 text-slate-950 text-xs font-extrabold rounded-xl shadow-md transition">
                    Selesai & Lihat Hasil <i class="fa-solid fa-check-double ml-1"></i>
                </button>
            </div>
        </section>

        <!-- 5. FINAL RESULT SCREEN -->
        <section id="resultScreen" class="hidden space-y-6">
            
            <!-- Result Summary Card -->
            <div id="resultHeaderCard" class="bg-slate-900 border border-slate-800 rounded-2xl p-6 text-center shadow-2xl relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-emerald-500/10 rounded-full blur-3xl"></div>
                
                <div id="resultBadgeIcon" class="w-16 h-16 rounded-2xl mx-auto flex items-center justify-center text-2xl mb-3 shadow-lg">
                    <!-- Icon populated dynamically -->
                </div>

                <h2 id="resultTitle" class="text-xl sm:text-2xl font-bold text-white">Hasil Quiz AI Siswa</h2>
                <p id="resultSubtitle" class="text-xs text-slate-400 mt-1 mb-4">Analisis Penentuan Skor & Rekap Hasil Akhir</p>

                <!-- Detailed Score Grid -->
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 max-w-xl mx-auto mb-6">
                    <div class="bg-emerald-500/10 border border-emerald-500/30 p-3 rounded-xl">
                        <p class="text-[10px] uppercase font-bold text-emerald-400">Jawaban Benar</p>
                        <p id="correctCountText" class="text-xl font-extrabold text-emerald-400">0 Soal</p>
                    </div>
                    <div class="bg-red-500/10 border border-red-500/30 p-3 rounded-xl">
                        <p class="text-[10px] uppercase font-bold text-red-400">Jawaban Salah</p>
                        <p id="wrongCountText" class="text-xl font-extrabold text-red-400">0 Soal</p>
                    </div>
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Skor Akhir</p>
                        <p id="finalScoreText" class="text-xl font-extrabold text-white">0 / 100</p>
                    </div>
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Predikat</p>
                        <p id="gradePredicateText" class="text-xl font-extrabold text-amber-400">-</p>
                    </div>
                </div>

                <!-- Action Retry -->
                <button onclick="confirmRestartQuiz()" class="px-6 py-3 bg-gradient-to-r from-emerald-500 to-teal-400 text-slate-950 text-xs font-bold rounded-xl shadow-lg hover:scale-105 active:scale-95 transition">
                    <i class="fa-solid fa-rotate-right mr-1.5"></i> Coba Ulangi Kuis Lagi
                </button>
            </div>

            <!-- Comprehensive Review Section -->
            <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl space-y-4">
                <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                    <h3 class="text-sm font-bold text-white flex items-center">
                        <i class="fa-solid fa-book-open text-emerald-400 mr-2"></i> Pembahasan & Kunci Jawaban Lengkap
                    </h3>
     
