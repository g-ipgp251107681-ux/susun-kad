<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Susun Perbezaan: Azan vs Iqamat</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Plus Jakarta Sans & Amiri -->
    <link href="https://fonts.googleapis.com/css2?family=Amiri:wght@700&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Font Awesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --cream-bg: #fdfbf7;
            --parchment: #fffdf9;
            --emerald-dark: #064e3b;
            --emerald-main: #059669;
            --gold-accent: #d97706;
            --royal-dark: #1e3a8a;
            --royal-main: #0284c7;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: var(--cream-bg);
            color: #1e293b;
            overflow-x: hidden;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        .font-arabic {
            font-family: 'Amiri', serif;
        }

        /* Corak Geometri Islamik SVG Halus pada Latar Belakang */
        .islamic-geo-bg {
            background-color: #fdfbf7;
            background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg stroke='%23059669' stroke-opacity='0.07' stroke-width='1'%3E%3Cpath d='M30 0l8.66 15H21.34L30 0zm0 60l8.66-15H21.34L30 60zM0 30l15-8.66v17.32L0 30zm60 0L45 21.34v17.32L60 30z'/%3E%3Ccircle cx='30' cy='30' r='12'/%3E%3Crect x='21.5' y='21.5' width='17' height='17' transform='rotate(45 30 30)'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
        }

        /* Kad Pernyataan Utama (Parchment Card) */
        .statement-card {
            background: linear-gradient(145deg, #ffffff 0%, #fef9ee 100%);
            box-shadow: 0 12px 25px -5px rgba(15, 23, 42, 0.12), 0 4px 0 #d97706;
            transition: transform 0.25s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.2s ease, opacity 0.2s ease;
            touch-action: none;
        }

        .statement-card.dragging {
            opacity: 0.75;
            transform: scale(1.04) rotate(2deg);
            box-shadow: 0 20px 35px -5px rgba(15, 23, 42, 0.22);
            cursor: grabbing;
        }

        .statement-card.selected-card {
            ring: 4px;
            box-shadow: 0 0 0 4px #f59e0b, 0 15px 30px rgba(245, 158, 11, 0.25);
            transform: translateY(-4px);
        }

        /* Zon Sasaran (Drop Buckets) */
        .drop-zone {
            transition: all 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
        }

        .drop-zone-azan {
            background: linear-gradient(180deg, rgba(236, 253, 245, 0.92) 0%, rgba(209, 250, 229, 0.95) 100%);
            border: 2.5px dashed #059669;
            box-shadow: inset 0 2px 10px rgba(6, 78, 59, 0.06);
        }
        .drop-zone-azan.drag-over, .drop-zone-azan.ready-target:hover {
            background: linear-gradient(180deg, #d1fae5 0%, #a7f3d0 100%);
            border-style: solid;
            border-color: #d97706;
            transform: scale(1.02);
            box-shadow: 0 10px 25px rgba(5, 150, 105, 0.25);
        }

        .drop-zone-iqamat {
            background: linear-gradient(180deg, rgba(240, 249, 255, 0.92) 0%, rgba(224, 242, 254, 0.95) 100%);
            border: 2.5px dashed #0284c7;
            box-shadow: inset 0 2px 10px rgba(30, 58, 138, 0.06);
        }
        .drop-zone-iqamat.drag-over, .drop-zone-iqamat.ready-target:hover {
            background: linear-gradient(180deg, #e0f2fe 0%, #bae6fd 100%);
            border-style: solid;
            border-color: #0284c7;
            transform: scale(1.02);
            box-shadow: 0 10px 25px rgba(2, 132, 199, 0.25);
        }

        /* Butang Gaya 3D */
        .btn-3d {
            transition: all 0.15s ease;
        }
        .btn-3d:active:not(:disabled) {
            transform: translateY(3px);
        }

        .btn-azan {
            background: linear-gradient(180deg, #10b981 0%, #059669 100%);
            box-shadow: 0 4px 0 #065f46, 0 6px 12px rgba(5, 150, 105, 0.25);
        }
        .btn-azan:active { box-shadow: 0 1px 0 #065f46; }

        .btn-iqamat {
            background: linear-gradient(180deg, #0ea5e9 0%, #0284c7 100%);
            box-shadow: 0 4px 0 #1e3a8a, 0 6px 12px rgba(2, 132, 199, 0.25);
        }
        .btn-iqamat:active { box-shadow: 0 1px 0 #1e3a8a; }

        .btn-gold {
            background: linear-gradient(180deg, #f59e0b 0%, #d97706 100%);
            box-shadow: 0 4px 0 #92400e, 0 6px 12px rgba(217, 119, 6, 0.25);
        }
        .btn-gold:active { box-shadow: 0 1px 0 #92400e; }

        /* Animasi Khas */
        @keyframes popIn {
            0% { transform: scale(0.85); opacity: 0; }
            75% { transform: scale(1.03); opacity: 1; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop { animation: popIn 0.3s cubic-bezier(0.34, 1.56, 0.64, 1) forwards; }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-9px); }
            40%, 80% { transform: translateX(9px); }
        }
        .animate-shake { animation: shake 0.4s ease-in-out; }

        @keyframes floatUp {
            0% { transform: translateY(0) scale(0.8); opacity: 1; }
            100% { transform: translateY(-55px) scale(1.15); opacity: 0; }
        }
        .floating-score {
            position: fixed;
            pointer-events: none;
            z-index: 100;
            animation: floatUp 0.95s ease-out forwards;
        }

        @keyframes flyLeft {
            0% { transform: translate(0, 0) scale(1); opacity: 1; }
            100% { transform: translate(-140px, 60px) scale(0.35); opacity: 0; }
        }
        @keyframes flyRight {
            0% { transform: translate(0, 0) scale(1); opacity: 1; }
            100% { transform: translate(140px, 60px) scale(0.35); opacity: 0; }
        }
        .fly-to-azan { animation: flyLeft 0.32s ease-in forwards; }
        .fly-to-iqamat { animation: flyRight 0.32s ease-in forwards; }

        /* Scrollbar halus untuk senarai kad tersusun */
        .custom-scroll::-webkit-scrollbar { width: 5px; }
        .custom-scroll::-webkit-scrollbar-track { background: transparent; }
        .custom-scroll::-webkit-scrollbar-thumb { background: rgba(100, 116, 139, 0.25); border-radius: 10px; }
    </style>
</head>
<body class="islamic-geo-bg min-h-screen flex flex-col justify-between">

    <!-- HEADER / BAR NAVIGASI & STATUS -->
    <header class="bg-gradient-to-r from-emerald-950 via-emerald-900 to-sky-950 text-white shadow-lg border-b-4 border-amber-500 px-4 py-3 sm:py-4 rounded-b-3xl sticky top-0 z-30">
        <div class="max-w-5xl mx-auto flex flex-wrap items-center justify-between gap-2">
            <!-- Logo & Tajuk -->
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 sm:w-11 sm:h-11 rounded-2xl bg-gradient-to-br from-amber-400 to-amber-600 flex items-center justify-center text-emerald-950 text-xl shadow-md border-2 border-amber-200">
                    <i class="fa-solid fa-scale-balanced"></i>
                </div>
                <div>
                    <h1 class="font-extrabold text-base sm:text-xl tracking-tight text-amber-100 leading-tight">
                        Susun Perbezaan: <span class="text-emerald-300">Azan</span> vs <span class="text-sky-300">Iqamat</span>
                    </h1>
                    <p class="text-[11px] sm:text-xs text-slate-300">Fiqh Ibadah • Mazhab Syafi'i</p>
                </div>
            </div>

            <!-- HUD Statistik Semasa Bermain -->
            <div id="game-hud" class="hidden flex items-center gap-2 sm:gap-3">
                <!-- Pemasa (Hanya untuk Mod Cabaran Masa) -->
                <div id="timer-badge" class="hidden bg-rose-950/80 border border-rose-500/60 px-3 py-1.5 rounded-xl flex items-center gap-1.5 text-rose-200 font-extrabold text-sm sm:text-base">
                    <i class="fa-solid fa-stopwatch text-rose-400 animate-pulse"></i>
                    <span id="timer-text">60s</span>
                </div>

                <!-- Pengganda Kombo (Streak) -->
                <div id="combo-badge" class="hidden bg-amber-500/20 border border-amber-400/50 px-2.5 py-1.5 rounded-xl flex items-center gap-1 text-amber-300 font-extrabold text-xs sm:text-sm">
                    <i class="fa-solid fa-fire text-amber-400"></i>
                    <span id="combo-text">x1</span>
                </div>

                <!-- Kemajuan Kad (Mod Belajar) -->
                <div id="progress-badge" class="bg-white/10 border border-white/20 px-3 py-1.5 rounded-xl text-xs sm:text-sm font-bold text-emerald-100">
                    <span id="card-progress-text">1 / 12</span>
                </div>

                <!-- Skor -->
                <div class="bg-black/30 border border-amber-400/40 px-3 py-1.5 rounded-xl flex items-center gap-1.5">
                    <i class="fa-solid fa-star text-amber-400 text-sm"></i>
                    <span id="score-text" class="font-extrabold text-amber-300 text-sm sm:text-base">0</span>
                </div>

                <!-- Butang Keluar ke Menu -->
                <button onclick="returnToMenu()" class="w-9 h-9 rounded-xl bg-white/10 hover:bg-white/20 flex items-center justify-center text-slate-200 border border-white/15 transition" title="Kembali ke Menu Utama">
                    <i class="fa-solid fa-house text-sm"></i>
                </button>
            </div>

            <!-- Butang Kawalan Audio (Sentiasa Ada) -->
            <button id="mute-btn" onclick="toggleAudio()" class="w-9 h-9 rounded-xl bg-white/10 hover:bg-white/20 flex items-center justify-center text-amber-200 border border-white/20 transition" title="Tutup/Buka Bunyi">
                <i id="mute-icon" class="fa-solid fa-volume-high text-sm"></i>
            </button>
        </div>
    </header>

    <!-- KAWASAN UTAMA -->
    <main class="flex-grow flex items-center justify-center p-3 sm:p-6">
        <div class="w-full max-w-5xl">

            <!-- ========================================== -->
            <!-- 1. SKRIN MULA (PILIH MOD PERMAINAN)        -->
            <!-- ========================================== -->
            <section id="start-screen" class="bg-white/95 backdrop-blur-sm rounded-3xl shadow-xl border-2 border-emerald-900/10 p-6 sm:p-10 text-center max-w-3xl mx-auto animate-pop">
                <!-- Hiasan Kaligrafi -->
                <div class="flex items-center justify-center gap-4 mb-4">
                    <span class="px-3 py-1 rounded-full bg-emerald-100 text-emerald-800 font-arabic text-xl sm:text-2xl border border-emerald-300">الأذان</span>
                    <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-gradient-to-br from-amber-100 to-amber-200 border-2 border-amber-400 flex items-center justify-center text-2xl sm:text-3xl text-amber-700 shadow-inner">
                        <i class="fa-solid fa-mosque"></i>
                    </div>
                    <span class="px-3 py-1 rounded-full bg-sky-100 text-sky-800 font-arabic text-xl sm:text-2xl border border-sky-300">الإقامة</span>
                </div>

                <h2 class="text-2xl sm:text-4xl font-extrabold text-slate-900 mb-2">
                    Susun Perbezaan: <span class="text-emerald-700">Azan</span> vs <span class="text-sky-700">Iqamat</span>
                </h2>
                <p class="text-slate-600 text-sm sm:text-base max-w-xl mx-auto mb-6 leading-relaxed">
                    Kategorikan setiap pernyataan kepada ruangan <strong>AZAN</strong> atau <strong>IQAMAT</strong> yang tepat. Anda boleh <strong>seret & lepas (drag & drop)</strong> kad atau klik terus butang pilihan!
                </p>

                <!-- Rekod Skor Tertinggi Cabaran Masa -->
                <div class="inline-flex items-center gap-2 bg-amber-50 border border-amber-300 px-4 py-1.5 rounded-full text-xs sm:text-sm font-bold text-amber-900 mb-6">
                    <i class="fa-solid fa-crown text-amber-500"></i>
                    <span>Rekod Terbaik Cabaran Masa: <span id="high-score-display" class="text-amber-700 font-extrabold">0</span> mata</span>
                </div>

                <!-- Pilihan 2 Mod Permainan -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-left mb-4">
                    <!-- Mod 1: Mod Belajar (Santai) -->
                    <div class="bg-gradient-to-br from-emerald-50 to-teal-50/70 border-2 border-emerald-300 rounded-2xl p-5 flex flex-col justify-between hover:shadow-lg transition">
                        <div>
                            <div class="flex items-center justify-between mb-2">
                                <span class="w-10 h-10 rounded-xl bg-emerald-600 text-white flex items-center justify-center text-lg shadow">
                                    <i class="fa-solid fa-book-open"></i>
                                </span>
                                <span class="text-xs font-extrabold uppercase tracking-wider bg-emerald-200/80 text-emerald-900 px-2.5 py-1 rounded-full">Tanpa Had Masa</span>
                            </div>
                            <h3 class="text-lg sm:text-xl font-extrabold text-emerald-950 mb-1">Mod Belajar (Santai)</h3>
                            <p class="text-xs sm:text-sm text-emerald-900/80 mb-4 leading-relaxed">
                                Susun <strong>12 kad perbezaan asas</strong> satu persatu dengan penerangan ringkas serta-merta bagi setiap kad. Sesuai untuk ulang kaji!
                            </p>
                        </div>
                        <button onclick="startGame('learn')" class="btn-3d btn-azan text-white font-extrabold py-3 px-4 rounded-xl w-full flex items-center justify-center gap-2 text-sm sm:text-base">
                            <i class="fa-solid fa-play"></i>
                            <span>Mula Mod Belajar</span>
                        </button>
                    </div>

                    <!-- Mod 2: Mod Cabaran Masa (60 Saat) -->
                    <div class="bg-gradient-to-br from-sky-50 to-indigo-50/70 border-2 border-sky-300 rounded-2xl p-5 flex flex-col justify-between hover:shadow-lg transition">
                        <div>
                            <div class="flex items-center justify-between mb-2">
                                <span class="w-10 h-10 rounded-xl bg-sky-600 text-white flex items-center justify-center text-lg shadow">
                                    <i class="fa-solid fa-bolt"></i>
                                </span>
                                <span class="text-xs font-extrabold uppercase tracking-wider bg-amber-200 text-amber-900 px-2.5 py-1 rounded-full">60 Saat • Kombo x3</span>
                            </div>
                            <h3 class="text-lg sm:text-xl font-extrabold text-sky-950 mb-1">Mod Cabaran Masa</h3>
                            <p class="text-xs sm:text-sm text-sky-900/80 mb-4 leading-relaxed">
                                Susun seberapa banyak kad dengan pantas dalam <strong>60 saat</strong>! Bina rentetan jawapan betul untuk pengganda mata <strong>x2</strong> dan <strong>x3</strong>.
                            </p>
                        </div>
                        <button onclick="startGame('timed')" class="btn-3d btn-iqamat text-white font-extrabold py-3 px-4 rounded-xl w-full flex items-center justify-center gap-2 text-sm sm:text-base">
                            <i class="fa-solid fa-stopwatch"></i>
                            <span>Mula Cabaran 60s</span>
                        </button>
                    </div>
                </div>
            </section>

            <!-- ========================================== -->
            <!-- 2. SKRIN PERMAINAN UTAMA (ARENA SUSUNAN)   -->
            <!-- ========================================== -->
            <section id="play-screen" class="hidden space-y-4">
                
                <!-- Bar Kombo Visual (Untuk Cabaran Masa) -->
                <div id="combo-bar-container" class="hidden max-w-md mx-auto bg-white/80 rounded-full p-1 border border-amber-300 shadow-xs">
                    <div class="flex items-center justify-between px-3 text-[11px] font-bold text-amber-900 mb-0.5">
                        <span>Rentetan Tepat: <strong id="streak-count">0</strong></span>
                        <span id="multiplier-label" class="text-amber-600">Pengganda: x1</span>
                    </div>
                    <div class="w-full bg-slate-200 h-2 rounded-full overflow-hidden">
                        <div id="combo-fill" class="bg-gradient-to-r from-amber-400 via-orange-500 to-rose-500 h-full rounded-full transition-all duration-300" style="width: 0%;"></div>
                    </div>
                </div>

                <!-- DEK KAD PERNYATAAN AKTIF (TENGAH) -->
                <div class="flex flex-col items-center justify-center my-1">
                    <div class="text-xs font-bold text-slate-500 mb-2 flex items-center gap-1.5">
                        <i class="fa-solid fa-hand-pointer text-amber-600"></i>
                        <span>Seret kad ke kotak kiri/kanan ATAU tekan butang di bawah kad:</span>
                    </div>

                    <!-- Kad Aktif Boleh Seret & Klik -->
                    <div id="active-card" draggable="true" onclick="toggleSelectCard()" class="statement-card w-full max-w-xl rounded-3xl border-2 border-amber-400/80 p-5 sm:p-6 text-center cursor-grab relative">
                        <!-- Lencana Kategori Aspek -->
                        <div class="inline-flex items-center gap-1.5 bg-amber-100/90 text-amber-900 text-xs font-extrabold px-3 py-1 rounded-full mb-3 border border-amber-300">
                            <i class="fa-solid fa-tag text-amber-600"></i>
                            <span id="card-aspect">Aspek: Tujuan</span>
                        </div>

                        <!-- Teks Pernyataan -->
                        <h3 id="card-statement" class="text-lg sm:text-2xl font-extrabold text-slate-800 leading-snug min-h-[64px] flex items-center justify-center px-2">
                            Memberitahu masuknya waktu solat fardu
                        </h3>

                        <!-- Butang Tindakan Pantas pada Kad (Mesra Telefon Pintar) -->
                        <div class="grid grid-cols-2 gap-3 mt-5 pt-4 border-t border-amber-200/70">
                            <button type="button" onclick="event.stopPropagation(); submitAnswer('AZAN')" class="btn-3d btn-azan text-white font-extrabold py-3 px-3 rounded-2xl flex items-center justify-center gap-2 text-sm sm:text-base">
                                <i class="fa-solid fa-arrow-left"></i>
                                <span>AZAN</span>
                            </button>
                            <button type="button" onclick="event.stopPropagation(); submitAnswer('IQAMAT')" class="btn-3d btn-iqamat text-white font-extrabold py-3 px-3 rounded-2xl flex items-center justify-center gap-2 text-sm sm:text-base">
                                <span>IQAMAT</span>
                                <i class="fa-solid fa-arrow-right"></i>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- DUA KOLUM / ZON SASARAN (AZAN vs IQAMAT) -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-1">
                    
                    <!-- ZON 1: AZAN (Emerald Green / Gold) -->
                    <div id="zone-azan" onclick="handleZoneClick('AZAN')" class="drop-zone drop-zone-azan rounded-3xl p-4 sm:p-5 flex flex-col min-h-[190px] sm:min-h-[230px] cursor-pointer">
                        <div class="flex items-center justify-between border-b-2 border-emerald-600/20 pb-2.5 mb-3">
                            <div class="flex items-center gap-2.5">
                                <div class="w-9 h-9 rounded-xl bg-emerald-700 text-amber-300 flex items-center justify-center font-bold shadow-xs border border-amber-400/50">
                                    <i class="fa-solid fa-bullhorn"></i>
                                </div>
                                <div>
                                    <h4 class="font-extrabold text-emerald-950 text-base sm:text-lg leading-none">AZAN</h4>
                                    <span class="text-[11px] font-semibold text-emerald-700">Seruan Masuk Waktu</span>
                                </div>
                            </div>
                            <span id="count-azan" class="bg-emerald-700 text-white text-xs font-extrabold px-2.5 py-1 rounded-full">0 kad</span>
                        </div>

                        <!-- Senarai Kad Yang Berjaya Disusun ke AZAN -->
                        <div id="list-azan" class="space-y-2 overflow-y-auto max-h-[180px] pr-1 custom-scroll flex-grow">
                            <div id="empty-azan" class="h-full flex flex-col items-center justify-center text-emerald-800/45 text-xs italic py-6 text-center">
                                <i class="fa-solid fa-inbox text-2xl mb-1 opacity-50"></i>
                                <span>Lepaskan atau klik di sini untuk jawapan AZAN</span>
                            </div>
                        </div>
                    </div>

                    <!-- ZON 2: IQAMAT (Royal Blue / Teal) -->
                    <div id="zone-iqamat" onclick="handleZoneClick('IQAMAT')" class="drop-zone drop-zone-iqamat rounded-3xl p-4 sm:p-5 flex flex-col min-h-[190px] sm:min-h-[230px] cursor-pointer">
                        <div class="flex items-center justify-between border-b-2 border-sky-600/20 pb-2.5 mb-3">
                            <div class="flex items-center gap-2.5">
                                <div class="w-9 h-9 rounded-xl bg-sky-700 text-white flex items-center justify-center font-bold shadow-xs border border-sky-300/50">
                                    <i class="fa-solid fa-people-group"></i>
                                </div>
                                <div>
                                    <h4 class="font-extrabold text-sky-950 text-base sm:text-lg leading-none">IQAMAT</h4>
                                    <span class="text-[11px] font-semibold text-sky-700">Seruan Dirikan Solat</span>
                                </div>
                            </div>
                            <span id="count-iqamat" class="bg-sky-700 text-white text-xs font-extrabold px-2.5 py-1 rounded-full">0 kad</span>
                        </div>

                        <!-- Senarai Kad Yang Berjaya Disusun ke IQAMAT -->
                        <div id="list-iqamat" class="space-y-2 overflow-y-auto max-h-[180px] pr-1 custom-scroll flex-grow">
                            <div id="empty-iqamat" class="h-full flex flex-col items-center justify-center text-sky-800/45 text-xs italic py-6 text-center">
                                <i class="fa-solid fa-inbox text-2xl mb-1 opacity-50"></i>
                                <span>Lepaskan atau klik di sini untuk jawapan IQAMAT</span>
                            </div>
                        </div>
                    </div>

                </div>
            </section>

            <!-- ========================================== -->
            <!-- 3. SKRIN TAMAT & JADUAL PERBANDINGAN       -->
            <!-- ========================================== -->
            <section id="end-screen" class="hidden bg-white/95 backdrop-blur-sm rounded-3xl shadow-xl border-2 border-emerald-900/15 p-5 sm:p-8 animate-pop">
                <div class="text-center mb-6">
                    <div class="w-16 h-16 sm:w-20 sm:h-20 mx-auto rounded-full bg-amber-100 border-4 border-amber-400 flex items-center justify-center text-3xl sm:text-4xl text-amber-600 mb-3 shadow-md">
                        <i class="fa-solid fa-trophy"></i>
                    </div>
                    <h2 id="end-heading" class="text-2xl sm:text-3xl font-extrabold text-slate-900">Syabas! Susunan Selesai</h2>
                    <p id="end-subheading" class="text-slate-600 text-xs sm:text-sm mt-1">Berikut ialah prestasi anda dan jadual perbandingan lengkap untuk ulang kaji.</p>

                    <!-- Kad Statistik Ringkas -->
                    <div class="grid grid-cols-3 gap-2 sm:gap-4 max-w-lg mx-auto mt-4">
                        <div class="bg-emerald-50 border border-emerald-200 rounded-2xl p-3">
                            <span class="text-[11px] font-bold text-emerald-700 block">Jumlah Skor</span>
                            <span id="final-score-val" class="text-xl sm:text-2xl font-extrabold text-emerald-950">0</span>
                        </div>
                        <div class="bg-sky-50 border border-sky-200 rounded-2xl p-3">
                            <span class="text-[11px] font-bold text-sky-700 block">Ketepatan</span>
                            <span id="final-accuracy-val" class="text-xl sm:text-2xl font-extrabold text-sky-950">100%</span>
                        </div>
                        <div class="bg-amber-50 border border-amber-200 rounded-2xl p-3">
                            <span class="text-[11px] font-bold text-amber-700 block">Kad Betul</span>
                            <span id="final-correct-val" class="text-xl sm:text-2xl font-extrabold text-amber-900">12</span>
                        </div>
                    </div>
                </div>

                <!-- JADUAL PERBANDINGAN LENGKAP AZAN & IQAMAT -->
                <div class="bg-slate-50 rounded-2xl border-2 border-slate-200/80 p-4 sm:p-5 mb-6">
                    <div class="flex flex-wrap items-center justify-between gap-2 mb-3 pb-2 border-b border-slate-200">
                        <h3 class="font-extrabold text-slate-900 text-base sm:text-lg flex items-center gap-2">
                            <i class="fa-solid fa-table-columns text-amber-600"></i>
                            <span>Jadual Perbandingan Lengkap: Azan & Iqamat</span>
                        </h3>
                        <span class="text-xs font-semibold text-slate-500">Rujukan Fiqh Mazhab Syafi'i</span>
                    </div>

                    <div class="overflow-x-auto rounded-xl border border-slate-200 shadow-xs">
                        <table class="w-full text-left border-collapse bg-white text-xs sm:text-sm">
                            <thead>
                                <tr>
                                    <th class="p-3 bg-slate-800 text-white font-bold w-1/5">Aspek</th>
                                    <th class="p-3 bg-emerald-800 text-amber-200 font-extrabold w-2/5">
                                        <i class="fa-solid fa-bullhorn mr-1"></i> AZAN
                                    </th>
                                    <th class="p-3 bg-sky-800 text-sky-100 font-extrabold w-2/5">
                                        <i class="fa-solid fa-people-group mr-1"></i> IQAMAT
                                    </th>
                                </tr>
                            </thead>
                            <tbody id="comparison-table-body" class="divide-y divide-slate-200">
                                <!-- Diisi secara dinamik oleh JavaScript -->
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Butang Main Semula -->
                <div class="flex flex-wrap items-center justify-center gap-3">
                    <button onclick="returnToMenu()" class="btn-3d bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold py-3 px-6 rounded-2xl text-sm sm:text-base flex items-center gap-2">
                        <i class="fa-solid fa-house"></i>
                        <span>Menu Utama</span>
                    </button>
                    <button onclick="restartCurrentMode()" class="btn-3d btn-gold text-white font-extrabold py-3 px-8 rounded-2xl text-sm sm:text-base flex items-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i>
                        <span>Main Semula</span>
                    </button>
                </div>
            </section>

        </div>
    </main>

    <!-- TOAST MAKLUM BALAS SEGERA (MINI-EXPLANATION) -->
    <div id="feedback-toast" class="fixed bottom-4 left-1/2 -translate-x-1/2 w-[92%] max-w-lg z-50 hidden">
        <div id="toast-inner" class="rounded-2xl p-4 shadow-2xl border-2 flex items-start gap-3 animate-pop">
            <div id="toast-icon" class="w-9 h-9 rounded-xl flex items-center justify-center flex-shrink-0 text-lg mt-0.5"></div>
            <div class="flex-grow">
                <h4 id="toast-title" class="font-extrabold text-sm">Tajuk</h4>
                <p id="toast-desc" class="text-xs leading-relaxed mt-0.5">Penerangan ringkas.</p>
            </div>
            <button id="toast-next-btn" onclick="dismissToastAndContinue()" class="hidden bg-emerald-700 hover:bg-emerald-800 text-white text-xs font-extrabold px-3 py-2 rounded-xl flex-shrink-0 self-center shadow">
                Seterusnya <i class="fa-solid fa-arrow-right ml-1"></i>
            </button>
        </div>
    </div>

    <!-- FOOTER -->
    <footer class="text-center py-2.5 text-[11px] text-slate-500 font-medium">
        Permainan Pendidikan Islam Interaktif • Susun Perbezaan: Azan vs Iqamat
    </footer>

    <!-- SKRIP PERMAINAN LENGKAP -->
    <script>
        // ==========================================
        // 1. PANGKALAN DATA PERBEZAAN AZAN & IQAMAT (14 KAD)
        // ==========================================
        const cardsDatabase = [
            {
                id: 1,
                aspect: "Tujuan Seruan",
                statement: "Memberitahu masuknya waktu solat fardu kepada orang ramai",
                category: "AZAN",
                explanation: "Azan dilaungkan sebagai tanda masuknya waktu solat fardu bagi menyeru umat Islam berkumpul menunaikan solat."
            },
            {
                id: 2,
                aspect: "Tujuan Seruan",
                statement: "Menyeru jemaah yang sudah hadir bangun untuk mendirikan solat",
                category: "IQAMAT",
                explanation: "Iqamat adalah isyarat bahawa solat fardu berjemaah akan dimulakan serta-merta."
            },
            {
                id: 3,
                aspect: "Nada & Tempo",
                statement: "Dibaca dengan nada perlahan, nyaring dan tartil (panjang)",
                category: "AZAN",
                explanation: "Bacaan azan disunatkan secara tartil (perlahan dan jelas) supaya suara sampai kepada mereka yang berada jauh."
            },
            {
                id: 4,
                aspect: "Nada & Tempo",
                statement: "Dibaca dengan nada cepat dan lancar (hadar / ringkas)",
                category: "IQAMAT",
                explanation: "Iqamat disunatkan dibaca secara hadar (cepat) kerana jemaah sudah berada di dalam masjid dan bersedia untuk solat."
            },
            {
                id: 5,
                aspect: "Lafaz Khusus",
                statement: "Mengandungi lafaz 'Qad qamatis solah' sebanyak 2 kali",
                category: "IQAMAT",
                explanation: "Lafaz 'Qad qamatis solah' (Sesungguhnya solat telah didirikan) hanya terdapat dalam Iqamat selepas 'Hayya alal falah'."
            },
            {
                id: 6,
                aspect: "Lafaz Khusus Subuh",
                statement: "Ditambah lafaz 'As-solatu khairum minan naum' pada waktu Subuh",
                category: "AZAN",
                explanation: "Tambahan ini (Tathwib) khusus pada Azan Subuh yang bermaksud 'Solat itu lebih baik daripada tidur'."
            },
            {
                id: 7,
                aspect: "Bilangan Kalimah",
                statement: "Kebanyakan lafaznya diulang sebanyak dua kali (15 kalimah)",
                category: "AZAN",
                explanation: "Dalam Azan, takbir awal 4 kali, kalimah syahadah dan hayya'alatain masing-masing diulang 2 kali (jumlah 15 kalimah)."
            },
            {
                id: 8,
                aspect: "Bilangan Kalimah",
                statement: "Kebanyakan lafaznya disebut sekali sahaja (11 kalimah)",
                category: "IQAMAT",
                explanation: "Dalam Iqamat, kebanyakan lafaz disebut sekali sahaja kecuali takbir dan 'Qad qamatis solah' (jumlah 11 kalimah)."
            },
            {
                id: 9,
                aspect: "Cara Pendengar Menjawab",
                statement: "Disunatkan mengulang lafaz-lafaznya satu per satu oleh pendengar",
                category: "AZAN",
                explanation: "Pendengar Azan sunat menjawab seperti ucapan muazzin kecuali pada 'Hayya alas solah/falah' dijawab dengan Hauqalah."
            },
            {
                id: 10,
                aspect: "Cara Pendengar Menjawab",
                statement: "Disunatkan menjawab 'Aqamahallahu wa adamaha' ketika 'Qad qamatis solah'",
                category: "IQAMAT",
                explanation: "Ketika muazzin menyebut 'Qad qamatis solah', jemaah sunat menjawab 'Aqamahallahu wa adamaha' (Semoga Allah mendirikan dan mengekalkannya)."
            },
            {
                id: 11,
                aspect: "Tempat & Kelantangan",
                statement: "Disunatkan melaungkannya di tempat tinggi dan suara yang lantang",
                category: "AZAN",
                explanation: "Muazzin disunatkan berdiri di tempat tinggi (seperti menara) dan menguatkan suara agar didengari penduduk sekitar."
            },
            {
                id: 12,
                aspect: "Tempat & Kelantangan",
                statement: "Dilaungkan dengan suara lebih rendah (sekadar didengari jemaah hadir)",
                category: "IQAMAT",
                explanation: "Suara Iqamat hendaklah lebih perlahan daripada Azan kerana sasarannya hanyalah jemaah yang berada di dalam ruang solat."
            },
            {
                id: 13,
                aspect: "Sunat Ketika Melaungkan",
                statement: "Disunatkan memasukkan hujung jari telunjuk ke dalam kedua telinga",
                category: "AZAN",
                explanation: "Memasukkan hujung jari telunjuk ke dalam telinga membantu melantangkan suara muazzin dan menjadi tanda visual dari jauh."
            },
            {
                id: 14,
                aspect: "Sunat Ketika Melaungkan",
                statement: "Disunatkan berpaling muka ke kanan dan ke kiri ketika 'Hayya alas solah/falah'",
                category: "AZAN",
                explanation: "Muazzin disunatkan memalingkan muka ke kanan ketika 'Hayya alas solah' dan ke kiri ketika 'Hayya alal falah' tanpa mengalihkan dada dari kiblat."
            }
        ];

        // Jadual Perbandingan Tersusun untuk Skrin Akhir
        const comparisonSummaryRows = [
            {
                aspect: "Tujuan Utama",
                azan: "Memberitahu masuknya waktu solat fardu kepada orang ramai.",
                iqamat: "Menyeru jemaah yang sudah hadir bangun memulakan solat."
            },
            {
                aspect: "Nada & Tempo",
                azan: "Perlahan, nyaring dan bertartil (dipanjangkan).",
                iqamat: "Cepat dan lancar (hadar / ringkas)."
            },
            {
                aspect: "Bilangan Lafaz",
                azan: "Kebanyakan diulang 2 kali (15 kalimah).",
                iqamat: "Kebanyakan disebut 1 kali sahaja (11 kalimah)."
            },
            {
                aspect: "Lafaz Tambahan",
                azan: "<em>'As-solatu khairum minan naum'</em> (Azan Subuh).",
                iqamat: "<em>'Qad qamatis solah'</em> (disebut 2 kali)."
            },
            {
                aspect: "Kelantangan Suara",
                azan: "Sangat lantang & disunatkan di tempat tinggi.",
                iqamat: "Lebih rendah (sekadar didengari jemaah hadir)."
            },
            {
                aspect: "Perbuatan Sunat",
                azan: "Jari telunjuk di telinga & paling muka kanan/kiri.",
                iqamat: "Berdiri lurus menghadap kiblat tanpa berpaling."
            },
            {
                aspect: "Jawapan Pendengar",
                azan: "Mengikut lafaz muazzin (dan Hauqalah pada Hayya'alatain).",
                iqamat: "Menjawab <em>'Aqamahallahu wa adamaha'</em> pada Qad qamatis solah."
            }
        ];

        // ==========================================
        // 2. PEMBOLEHUBAH STATUS PERMAINAN
        // ==========================================
        let gameMode = 'learn'; // 'learn' | 'timed'
        let deck = [];
        let currentCard = null;
        let currentIndex = 0;
        let score = 0;
        let streak = 0;
        let multiplier = 1;
        let totalAttempts = 0;
        let correctCount = 0;
        let timeLeft = 60;
        let timerInterval = null;
        let isCardSelected = false;
        let isWaitingNextLearn = false;
        let soundEnabled = true;
        let highScoreTimed = parseInt(localStorage.getItem('azan_iqamat_highscore') || '0', 10);

        // ==========================================
        // 3. SISTEM AUDIO SINTETIK (WEB AUDIO API)
        // ==========================================
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) audioCtx = new AudioContext();
            if (audioCtx.state === 'suspended') audioCtx.resume();
        }

        function playTone(type) {
            if (!soundEnabled) return;
            try {
                initAudio();
                const now = audioCtx.currentTime;

                if (type === 'pick') {
                    // Bunyi sentuhan kad lembut
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(420, now);
                    osc.frequency.exponentialRampToValueAtTime(620, now + 0.06);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.06);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.06);
                } else if (type === 'correct') {
                    // Loceng harmonik ceria (naik mengikut kombo)
                    const baseFreq = 523.25 * (1 + (multiplier - 1) * 0.12);
                    [baseFreq, baseFreq * 1.25, baseFreq * 1.5].forEach((freq, i) => {
                        const osc = audioCtx.createOscillator();
                        const gain = audioCtx.createGain();
                        osc.type = 'triangle';
                        osc.frequency.setValueAtTime(freq, now + i * 0.055);
                        gain.gain.setValueAtTime(0.18, now + i * 0.055);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + i * 0.055 + 0.28);
                        osc.connect(gain);
                        gain.connect(audioCtx.destination);
                        osc.start(now + i * 0.055);
                        osc.stop(now + i * 0.055 + 0.28);
                    });
                } else if (type === 'wrong') {
                    // Bunyi getaran lembut apabila salah
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(175, now);
                    osc.frequency.linearRampToValueAtTime(130, now + 0.24);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.24);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.24);
                } else if (type === 'finish') {
                    // Fanfare tamat
                    const notes = [523.25, 659.25, 783.99, 1046.50];
                    notes.forEach((freq, idx) => {
                        const osc = audioCtx.createOscillator();
                        const gain = audioCtx.createGain();
                        osc.type = 'triangle';
                        osc.frequency.setValueAtTime(freq, now + idx * 0.1);
                        gain.gain.setValueAtTime(0.2, now + idx * 0.1);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.1 + 0.45);
                        osc.connect(gain);
                        gain.connect(audioCtx.destination);
                        osc.start(now + idx * 0.1);
                        osc.stop(now + idx * 0.1 + 0.45);
                    });
                }
            } catch (err) {
                console.warn("Audio disekat pelayar:", err);
            }
        }

        function toggleAudio() {
            soundEnabled = !soundEnabled;
            const icon = document.getElementById('mute-icon');
            icon.className = soundEnabled ? 'fa-solid fa-volume-high text-sm' : 'fa-solid fa-volume-xmark text-sm text-rose-400';
            if (soundEnabled) playTone('pick');
        }

        // ==========================================
        // 4. INISIALISASI & KAWALAN ALIRAN PERMAINAN
        // ==========================================
        window.onload = function() {
            document.getElementById('high-score-display').textContent = highScoreTimed;
            setupDragAndDrop();
            setupKeyboardShortcuts();
        };

        function shuffle(arr) {
            const copy = [...arr];
            for (let i = copy.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [copy[i], copy[j]] = [copy[j], copy[i]];
            }
            return copy;
        }

        function startGame(mode) {
            playTone('pick');
            gameMode = mode;
            score = 0;
            streak = 0;
            multiplier = 1;
            totalAttempts = 0;
            correctCount = 0;
            currentIndex = 0;
            isCardSelected = false;
            isWaitingNextLearn = false;

            clearInterval(timerInterval);
            hideToast();

            // Sediakan dek kad
            if (mode === 'learn') {
                deck = shuffle(cardsDatabase).slice(0, 12); // 12 kad utama dalam Mod Belajar
            } else {
                deck = shuffle(cardsDatabase); // Semua 14 kad dikitar semula dalam Cabaran Masa
            }

            // Reset paparan senarai zon Azan & Iqamat
            document.getElementById('list-azan').innerHTML = `
                <div id="empty-azan" class="h-full flex flex-col items-center justify-center text-emerald-800/45 text-xs italic py-6 text-center">
                    <i class="fa-solid fa-inbox text-2xl mb-1 opacity-50"></i>
                    <span>Lepaskan atau klik di sini untuk jawapan AZAN</span>
                </div>`;
            document.getElementById('list-iqamat').innerHTML = `
                <div id="empty-iqamat" class="h-full flex flex-col items-center justify-center text-sky-800/45 text-xs italic py-6 text-center">
                    <i class="fa-solid fa-inbox text-2xl mb-1 opacity-50"></i>
                    <span>Lepaskan atau klik di sini untuk jawapan IQAMAT</span>
                </div>`;
            document.getElementById('count-azan').textContent = '0 kad';
            document.getElementById('count-iqamat').textContent = '0 kad';

            // Tukar skrin
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('end-screen').classList.add('hidden');
            document.getElementById('play-screen').classList.remove('hidden');
            document.getElementById('game-hud').classList.remove('hidden');

            // Konfigurasi UI mengikut Mod
            if (mode === 'timed') {
                timeLeft = 60;
                document.getElementById('timer-badge').classList.remove('hidden');
                document.getElementById('combo-badge').classList.remove('hidden');
                document.getElementById('combo-bar-container').classList.remove('hidden');
                document.getElementById('progress-badge').classList.add('hidden');
                updateTimerUI();
                startTimer();
            } else {
                document.getElementById('timer-badge').classList.add('hidden');
                document.getElementById('combo-badge').classList.add('hidden');
                document.getElementById('combo-bar-container').classList.add('hidden');
                document.getElementById('progress-badge').classList.remove('hidden');
            }

            updateScoreAndComboUI();
            loadNextCard();
        }

        function startTimer() {
            timerInterval = setInterval(() => {
                timeLeft--;
                updateTimerUI();
                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    endGame();
                }
            }, 1000);
        }

        function updateTimerUI() {
            const timerEl = document.getElementById('timer-text');
            const badgeEl = document.getElementById('timer-badge');
            timerEl.textContent = `${timeLeft}s`;
            if (timeLeft <= 10) {
                badgeEl.classList.add('bg-rose-600', 'text-white', 'border-rose-300');
            } else {
                badgeEl.classList.remove('bg-rose-600', 'text-white', 'border-rose-300');
            }
        }

        function updateScoreAndComboUI() {
            document.getElementById('score-text').textContent = score;
            document.getElementById('combo-text').textContent = `x${multiplier}`;
            document.getElementById('streak-count').textContent = streak;
            document.getElementById('multiplier-label').textContent = `Pengganda: x${multiplier}`;

            // Kemas kini bar kemajuan kombo (0 hingga 6 rentetan untuk x3)
            const fillPercent = Math.min(100, (streak / 6) * 100);
            document.getElementById('combo-fill').style.width = `${fillPercent}%`;

            if (gameMode === 'learn') {
                const displayIdx = Math.min(currentIndex + 1, deck.length);
                document.getElementById('card-progress-text').textContent = `${displayIdx} / ${deck.length}`;
            }
        }

        function loadNextCard() {
            if (gameMode === 'learn' && currentIndex >= deck.length) {
                endGame();
                return;
            }

            // Dalam mod cabaran masa, kitar semula kad secara rawak apabila habis satu pusingan
            if (gameMode === 'timed' && currentIndex >= deck.length) {
                deck = shuffle(cardsDatabase);
                currentIndex = 0;
            }

            currentCard = deck[currentIndex];
            isCardSelected = false;
            isWaitingNextLearn = false;

            const cardEl = document.getElementById('active-card');
            cardEl.classList.remove('fly-to-azan', 'fly-to-iqamat', 'animate-shake', 'selected-card');
            cardEl.classList.add('animate-pop');
            setTimeout(() => cardEl.classList.remove('animate-pop'), 300);

            document.getElementById('zone-azan').classList.remove('ready-target');
            document.getElementById('zone-iqamat').classList.remove('ready-target');

            document.getElementById('card-aspect').textContent = `Aspek: ${currentCard.aspect}`;
            document.getElementById('card-statement').textContent = currentCard.statement;

            updateScoreAndComboUI();
        }

        // ==========================================
        // 5. INTERAKSI: SERET & LEPAS (DRAG & DROP) + KLIK
        // ==========================================
        function setupDragAndDrop() {
            const cardEl = document.getElementById('active-card');
            const zoneAzan = document.getElementById('zone-azan');
            const zoneIqamat = document.getElementById('zone-iqamat');

            cardEl.addEventListener('dragstart', (e) => {
                if (isWaitingNextLearn) {
                    e.preventDefault();
                    return;
                }
                playTone('pick');
                cardEl.classList.add('dragging');
                e.dataTransfer.setData('text/plain', 'active-card');
                e.dataTransfer.effectAllowed = 'move';
            });

            cardEl.addEventListener('dragend', () => {
                cardEl.classList.remove('dragging');
                zoneAzan.classList.remove('drag-over');
                zoneIqamat.classList.remove('drag-over');
            });

            [
                { el: zoneAzan, target: 'AZAN' },
                { el: zoneIqamat, target: 'IQAMAT' }
            ].forEach(({ el, target }) => {
                el.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    e.dataTransfer.dropEffect = 'move';
                    el.classList.add('drag-over');
                });

                el.addEventListener('dragleave', () => {
                    el.classList.remove('drag-over');
                });

                el.addEventListener('drop', (e) => {
                    e.preventDefault();
                    el.classList.remove('drag-over');
                    submitAnswer(target);
                });
            });
        }

        function toggleSelectCard() {
            if (isWaitingNextLearn) return;
            playTone('pick');
            isCardSelected = !isCardSelected;
            const cardEl = document.getElementById('active-card');
            const zoneAzan = document.getElementById('zone-azan');
            const zoneIqamat = document.getElementById('zone-iqamat');

            if (isCardSelected) {
                cardEl.classList.add('selected-card');
                zoneAzan.classList.add('ready-target');
                zoneIqamat.classList.add('ready-target');
            } else {
                cardEl.classList.remove('selected-card');
                zoneAzan.classList.remove('ready-target');
                zoneIqamat.classList.remove('ready-target');
            }
        }

        function handleZoneClick(chosenCategory) {
            if (isWaitingNextLearn) return;
            submitAnswer(chosenCategory);
        }

        function setupKeyboardShortcuts() {
            document.addEventListener('keydown', (e) => {
                if (document.getElementById('play-screen').classList.contains('hidden')) return;
                if (isWaitingNextLearn) {
                    if (e.key === 'Enter' || e.key === ' ') {
                        e.preventDefault();
                        dismissToastAndContinue();
                    }
                    return;
                }
                if (e.key === 'ArrowLeft') {
                    submitAnswer('AZAN');
                } else if (e.key === 'ArrowRight') {
                    submitAnswer('IQAMAT');
                }
            });
        }

        // ==========================================
        // 6. PENILAIAN JAWAPAN & MAKLUM BALAS VISUAL
        // ==========================================
        function submitAnswer(chosenCategory) {
            if (!currentCard || isWaitingNextLearn) return;

            totalAttempts++;
            const cardEl = document.getElementById('active-card');

            if (chosenCategory === currentCard.category) {
                // JAWAPAN BETUL
                playTone('correct');
                correctCount++;
                streak++;

                // Kira pengganda kombo (x1 -> x2 selepas 3 betul, x3 selepas 6 betul)
                if (streak >= 6) multiplier = 3;
                else if (streak >= 3) multiplier = 2;
                else multiplier = 1;

                const pointsEarned = 100 * (gameMode === 'timed' ? multiplier : 1);
                score += pointsEarned;
                updateScoreAndComboUI();

                // Animasi skor terapung (+100)
                spawnFloatingScore(pointsEarned, chosenCategory);

                // Animasi kad terbang ke zon sasaran
                cardEl.classList.add(chosenCategory === 'AZAN' ? 'fly-to-azan' : 'fly-to-iqamat');

                // Masukkan kad ke dalam senarai zon tersebut
                appendSortedCardToZone(currentCard, chosenCategory);

                if (gameMode === 'learn') {
                    // Dalam Mod Belajar, papar penerangan pendidikan ringkas
                    isWaitingNextLearn = true;
                    showToast(
                        true,
                        `Tepat! Jawapan ialah ${chosenCategory}`,
                        currentCard.explanation,
                        true
                    );
                } else {
                    // Dalam Mod Cabaran Masa, terus ke kad seterusnya dengan pantas
                    showToast(true, `Betul! (+${pointsEarned})`, currentCard.explanation, false);
                    setTimeout(() => {
                        currentIndex++;
                        loadNextCard();
                    }, 300);
                }
            } else {
                // JAWAPAN SALAH
                playTone('wrong');
                streak = 0;
                multiplier = 1;
                updateScoreAndComboUI();

                // Gegarkan kad
                cardEl.classList.remove('animate-shake');
                void cardEl.offsetWidth; // paksa reflow
                cardEl.classList.add('animate-shake');

                const correctLabel = currentCard.category;
                const hintMsg = `Bukan ${chosenCategory}. Petunjuk: ${currentCard.explanation}`;
                showToast(false, `Kurang Tepat! Cuba lagi`, hintMsg, false);
            }
        }

        function appendSortedCardToZone(cardObj, targetZone) {
            const isAzan = targetZone === 'AZAN';
            const listEl = document.getElementById(isAzan ? 'list-azan' : 'list-iqamat');
            const emptyEl = document.getElementById(isAzan ? 'empty-azan' : 'empty-iqamat');
            const countEl = document.getElementById(isAzan ? 'count-azan' : 'count-iqamat');

            if (emptyEl) emptyEl.remove();

            const itemDiv = document.createElement('div');
            itemDiv.className = isAzan
                ? 'bg-white/95 border-l-4 border-emerald-600 rounded-xl p-2.5 text-xs text-slate-800 shadow-xs flex items-start gap-2 animate-pop'
                : 'bg-white/95 border-l-4 border-sky-600 rounded-xl p-2.5 text-xs text-slate-800 shadow-xs flex items-start gap-2 animate-pop';

            itemDiv.innerHTML = `
                <i class="fa-solid fa-circle-check ${isAzan ? 'text-emerald-600' : 'text-sky-600'} mt-0.5 flex-shrink-0"></i>
                <span class="font-medium leading-snug">${cardObj.statement}</span>
            `;

            listEl.prepend(itemDiv);
            const totalInZone = listEl.children.length;
            countEl.textContent = `${totalInZone} kad`;
        }

        function spawnFloatingScore(points, targetZone) {
            const zoneEl = document.getElementById(targetZone === 'AZAN' ? 'zone-azan' : 'zone-iqamat');
            const rect = zoneEl.getBoundingClientRect();

            const floater = document.createElement('div');
            floater.className = 'floating-score font-extrabold text-xl sm:text-2xl text-amber-500 drop-shadow-md';
            floater.style.left = `${rect.left + rect.width / 2 - 30}px`;
            floater.style.top = `${rect.top + 30}px`;
            floater.textContent = `+${points}`;

            document.body.appendChild(floater);
            setTimeout(() => floater.remove(), 950);
        }

        // ==========================================
        // 7. KAWALAN TOAST & SKRIN TAMAT
        // ==========================================
        let toastAutoTimeout = null;

        function showToast(isSuccess, title, description, requireButton) {
            clearTimeout(toastAutoTimeout);
            const toast = document.getElementById('feedback-toast');
            const inner = document.getElementById('toast-inner');
            const icon = document.getElementById('toast-icon');
            const titleEl = document.getElementById('toast-title');
            const descEl = document.getElementById('toast-desc');
            const nextBtn = document.getElementById('toast-next-btn');

            titleEl.textContent = title;
            descEl.textContent = description;

            if (isSuccess) {
                inner.className = 'rounded-2xl p-4 shadow-2xl border-2 bg-emerald-50 border-emerald-400 text-emerald-950 flex items-start gap-3 animate-pop';
                icon.className = 'w-9 h-9 rounded-xl bg-emerald-600 text-white flex items-center justify-center flex-shrink-0 text-lg mt-0.5';
                icon.innerHTML = '<i class="fa-solid fa-check"></i>';
            } else {
                inner.className = 'rounded-2xl p-4 shadow-2xl border-2 bg-rose-50 border-rose-400 text-rose-950 flex items-start gap-3 animate-pop';
                icon.className = 'w-9 h-9 rounded-xl bg-rose-600 text-white flex items-center justify-center flex-shrink-0 text-lg mt-0.5';
                icon.innerHTML = '<i class="fa-solid fa-xmark"></i>';
            }

            if (requireButton) {
                nextBtn.classList.remove('hidden');
            } else {
                nextBtn.classList.add('hidden');
                toastAutoTimeout = setTimeout(hideToast, 2600);
            }

            toast.classList.remove('hidden');
        }

        function hideToast() {
            document.getElementById('feedback-toast').classList.add('hidden');
        }

        function dismissToastAndContinue() {
            if (!isWaitingNextLearn) return;
            playTone('pick');
            hideToast();
            currentIndex++;
            loadNextCard();
        }

        function endGame() {
            clearInterval(timerInterval);
            hideToast();
            playTone('finish');

            // Kemas kini rekod tertinggi bagi Cabaran Masa
            if (gameMode === 'timed' && score > highScoreTimed) {
                highScoreTimed = score;
                localStorage.setItem('azan_iqamat_highscore', highScoreTimed.toString());
                document.getElementById('high-score-display').textContent = highScoreTimed;
            }

            document.getElementById('play-screen').classList.add('hidden');
            document.getElementById('game-hud').classList.add('hidden');
            document.getElementById('end-screen').classList.remove('hidden');

            const accuracy = totalAttempts > 0 ? Math.round((correctCount / totalAttempts) * 100) : 0;
            document.getElementById('final-score-val').textContent = score;
            document.getElementById('final-accuracy-val').textContent = `${accuracy}%`;
            document.getElementById('final-correct-val').textContent = `${correctCount} kad`;

            if (gameMode === 'timed') {
                document.getElementById('end-heading').textContent = 'Masa Tamat! Cabaran Selesai';
                document.getElementById('end-subheading').textContent = `Anda berjaya menyusun ${correctCount} kad dalam masa 60 saat!`;
            } else {
                document.getElementById('end-heading').textContent = 'Tahniah! Semua 12 Kad Berjaya Disusun';
                document.getElementById('end-subheading').textContent = 'Sila semak jadual perbandingan lengkap di bawah untuk mengukuhkan ingatan.';
            }

            renderComparisonTable();
        }

        function renderComparisonTable() {
            const tbody = document.getElementById('comparison-table-body');
            tbody.innerHTML = '';

            comparisonSummaryRows.forEach((row, idx) => {
                const tr = document.createElement('tr');
                tr.className = idx % 2 === 0 ? 'bg-white' : 'bg-slate-50/80';
                tr.innerHTML = `
                    <td class="p-3 font-bold text-slate-800 align-top">${row.aspect}</td>
                    <td class="p-3 text-emerald-950 bg-emerald-50/30 align-top">${row.azan}</td>
                    <td class="p-3 text-sky-950 bg-sky-50/30 align-top">${row.iqamat}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function returnToMenu() {
            clearInterval(timerInterval);
            hideToast();
            playTone('pick');
            document.getElementById('play-screen').classList.add('hidden');
            document.getElementById('end-screen').classList.add('hidden');
            document.getElementById('game-hud').classList.add('hidden');
            document.getElementById('start-screen').classList.remove('hidden');
        }

        function restartCurrentMode() {
            startGame(gameMode);
        }
    </script>
</body>
</html>
