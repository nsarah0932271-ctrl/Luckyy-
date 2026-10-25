<!DOCTYPE html>
<html lang="fr" data-theme="girly-pink">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>N_Sarah Prep Hub - EPGE</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- Chart.js pour les courbes de progression -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Poppins', sans-serif; }
        
        :root {
            --primary: #ec4899;
            --primary-hover: #db2777;
            --bg-gradient: linear-gradient(135deg, #fdf2f8 0%, #fce7f3 100%);
            --card-bg: #ffffff;
            --text-main: #1f2937;
        }

        [data-theme="lavender"] { --primary: #8b5cf6; --bg-gradient: linear-gradient(135deg, #f5f3ff 0%, #ede9fe 100%); }
        [data-theme="peach"] { --primary: #f97316; --bg-gradient: linear-gradient(135deg, #fff7ed 0%, #ffedd5 100%); }
        [data-theme="mint"] { --primary: #10b981; --bg-gradient: linear-gradient(135deg, #ecfdf5 0%, #d1fae5 100%); }
        [data-mode="dark"] { --bg-gradient: linear-gradient(135deg, #111827 0%, #1f2937 100%); --card-bg: #1f2937; --text-main: #f9fafb; }

        body { background: var(--bg-gradient); color: var(--text-main); min-height: 100vh; }
        .custom-card { background-color: var(--card-bg); color: var(--text-main); }
    </style>
</head>
<body class="transition-colors duration-300">

    <!-- ÉCRAN D'ACCÈS / ESPACE PERSONNEL (GARANTI SANS BUG) -->
    <div id="authModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/60 backdrop-blur-sm p-4">
        <div class="custom-card max-w-md w-full p-8 rounded-3xl shadow-2xl border border-pink-100 dark:border-gray-700 text-center space-y-6">
            <span class="text-5xl">💖</span>
            <div>
                <h1 class="text-2xl font-bold bg-gradient-to-r from-pink-500 to-purple-500 bg-clip-text text-transparent">N_Sarah Prep Hub</h1>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Entre ton pseudo pour accéder à ton espace personnel EPGE</p>
            </div>
            
            <div class="space-y-4 text-left">
                <div>
                    <label class="block text-sm font-medium mb-1">Ton Pseudo / Prénom</label>
                    <input type="text" id="usernameInput" placeholder="Ex: N_Sarah" class="w-full p-3.5 rounded-2xl border border-gray-300 dark:border-gray-600 dark:bg-gray-700 focus:outline-none focus:border-pink-500 font-medium">
                </div>
                <button onclick="loginUser()" class="w-full bg-pink-500 hover:bg-pink-600 text-white font-semibold py-3.5 rounded-2xl transition shadow-lg shadow-pink-500/25">
                    Entrer dans mon espace ➔
                </button>
            </div>
        </div>
    </div>

    <!-- SITE PRINCIPAL (Masqué tant que le pseudo n'est pas entré) -->
    <div id="appContainer" class="hidden">
        <!-- BARRE DE NAVIGATION -->
        <header class="bg-white/80 dark:bg-gray-800/80 backdrop-blur-md shadow-sm sticky top-0 z-40 px-4 py-3 border-b border-pink-100 dark:border-gray-700">
            <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center space-x-3">
                    <span class="text-2xl">💖</span>
                    <h1 class="text-xl font-bold bg-gradient-to-r from-pink-500 to-purple-500 bg-clip-text text-transparent">N_Sarah Prep Hub (EPGE)</h1>
                </div>

                <div class="flex items-center flex-wrap gap-3">
                    <span class="text-sm bg-pink-50 dark:bg-gray-700 px-3 py-1.5 rounded-full">👤 <strong id="headerUsername" class="text-pink-600">N_Sarah</strong></span>
                    <button onclick="logoutUser()" class="text-xs text-gray-500 hover:text-red-500 font-medium">[Changer de profil]</button>
                    
                    <select id="colorThemeSelect" onchange="changeColorTheme(this.value)" class="text-xs px-2 py-1.5 rounded border dark:bg-gray-700">
                        <option value="girly-pink">🌸 Rose Girly</option>
                        <option value="lavender">💜 Lavande Douce</option>
                        <option value="peach">🍑 Pêche Lumineuse</option>
                        <option value="mint">🌿 Menthe Fraîche</option>
                    </select>

                    <button onclick="toggleDarkMode()" class="p-2 rounded-full bg-gray-200 dark:bg-gray-700 text-sm">🌙/☀️</button>
                </div>
            </div>
        </header>

        <!-- CONTENU PRINCIPAL -->
        <main class="max-w-6xl mx-auto p-4 md:p-6 space-y-6">

            <!-- TABLEAU DE BORD -->
            <div id="dashboardView" class="space-y-6">
                <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700">
                    <h2 class="text-2xl font-bold mb-2">Bienvenue sur ton espace, <span class="text-pink-500" id="welcomeUsername">N_Sarah</span> ! ✨</h2>
                    <p class="text-gray-600 dark:text-gray-300 text-sm mb-6">Prépare tes concours EPGE efficacement : QCM avec malus/partiel, examens complets à ton rythme, et tests mix multi-leçons.</p>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div class="p-4 rounded-2xl bg-pink-50 dark:bg-gray-700/50 border border-pink-100">
                            <h3 class="font-semibold text-pink-600">📊 Moyenne générale</h3>
                            <p class="text-2xl font-bold mt-1">15.4 / 20</p>
                        </div>
                        <div class="p-4 rounded-2xl bg-purple-50 dark:bg-gray-700/50 border border-purple-100">
                            <h3 class="font-semibold text-purple-600">📝 Sessions validées</h3>
                            <p class="text-2xl font-bold mt-1">8</p>
                        </div>
                        <div class="p-4 rounded-2xl bg-amber-50 dark:bg-gray-700/50 border border-amber-100">
                            <h3 class="font-semibold text-amber-600">⚡ Fréquence de révision</h3>
                            <p class="text-lg font-bold mt-1">Tous les 3 jours</p>
                        </div>
                    </div>
                </div>

                <!-- MODULES DE CHOIX -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="custom-card p-6 rounded-3xl shadow-lg border border-pink-100 dark:border-gray-700 flex flex-col justify-between">
                        <div>
                            <span class="text-xs bg-pink-100 text-pink-700 font-semibold px-3 py-1 rounded-full">Mode QCM</span>
                            <h3 class="text-xl font-bold mt-3 mb-2">⚡ QCM Standard (Chrono 30 min)</h3>
                            <p class="text-gray-600 dark:text-gray-300 text-sm mb-4">Questions à choix uniques ou multiples (+1 point, -0.5 malus, partage partiel, correction détaillée).</p>
                        </div>
                        # Luckyy-
