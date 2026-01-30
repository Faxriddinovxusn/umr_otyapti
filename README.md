<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Life & 2026 Progress Tracker</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap');
        
        * {
            box-sizing: border-box;
        }

        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.5);
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(0, 242, 255, 0.5);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: rgba(0, 242, 255, 0.7);
        }

        body {
            background: linear-gradient(135deg, #020617 0%, #0f172a 50%, #030b19 100%);
            color: #f8fafc;
            font-family: 'Plus Jakarta Sans', sans-serif;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            padding: 20px;
            width: 100%;
            overflow-x: hidden;
            background-attachment: fixed;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(circle at 20% 50%, rgba(0, 242, 255, 0.03) 0%, transparent 50%),
                        radial-gradient(circle at 80% 80%, rgba(59, 130, 246, 0.03) 0%, transparent 50%);
            pointer-events: none;
            z-index: 0;
        }

        .content-wrapper {
            position: relative;
            z-index: 1;
            width: 100%;
            max-width: 600px;
            padding: 0;
        }

        .glass-panel {
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 24px;
            width: 100%;
            transition: all 0.3s ease;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3),
                        inset 0 1px 1px rgba(255, 255, 255, 0.1);
        }

        .glass-panel:hover {
            background: rgba(15, 23, 42, 0.8);
            border-color: rgba(0, 242, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0, 242, 255, 0.15),
                        inset 0 1px 1px rgba(255, 255, 255, 0.15);
        }

        /* Canvas konteyneri */
        #canvas-container {
            width: 100%;
            background: linear-gradient(135deg, rgba(0, 0, 0, 0.5) 0%, rgba(15, 23, 42, 0.3) 100%);
            border-radius: 16px;
            padding: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: auto;
            border: 1px solid rgba(0, 242, 255, 0.1);
            transition: all 0.3s ease;
        }

        #canvas-container:hover {
            border-color: rgba(0, 242, 255, 0.2);
            box-shadow: inset 0 0 20px rgba(0, 242, 255, 0.05);
        }

        canvas {
            max-width: 100%;
            height: auto;
            display: block;
            filter: drop-shadow(0 0 10px rgba(0, 242, 255, 0.1));
            transition: filter 0.3s ease;
        }

        canvas:hover {
            filter: drop-shadow(0 0 20px rgba(0, 242, 255, 0.2));
        }

        .progress-glow {
            box-shadow: 0 0 20px rgba(0, 242, 255, 0.4);
            animation: pulse-glow 2s ease-in-out infinite;
        }

        @keyframes pulse-glow {
            0%, 100% {
                box-shadow: 0 0 20px rgba(0, 242, 255, 0.4);
            }
            50% {
                box-shadow: 0 0 30px rgba(0, 242, 255, 0.6);
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .glass-panel {
            animation: fadeInUp 0.6s ease-out forwards;
        }

        .glass-panel:nth-child(1) { animation-delay: 0s; }
        .glass-panel:nth-child(2) { animation-delay: 0.1s; }
        .glass-panel:nth-child(3) { animation-delay: 0.2s; }
        .glass-panel:nth-child(4) { animation-delay: 0.3s; }

        #setup-modal {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.95);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            padding: 20px;
            backdrop-filter: blur(5px);
            animation: fadeIn 0.3s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        input[type="date"] {
            transition: all 0.3s ease;
        }

        input[type="date"]:focus {
            box-shadow: 0 0 0 3px rgba(0, 242, 255, 0.2);
        }

        input[type="date"]::-webkit-calendar-picker-indicator {
            filter: invert(1);
            cursor: pointer;
        }

        button {
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        button:active {
            transform: scale(0.98);
        }

        button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.2);
            transition: left 0.5s ease;
            z-index: -1;
        }

        button:hover::before {
            left: 100%;
        }

        .stat-card {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.5) 0%, rgba(15, 23, 42, 0.5) 100%);
            padding: 12px;
            border-radius: 16px;
            border: 1px solid rgba(0, 242, 255, 0.1);
            transition: all 0.3s ease;
        }

        .stat-card:hover {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.7) 0%, rgba(15, 23, 42, 0.7) 100%);
            border-color: rgba(0, 242, 255, 0.3);
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0, 242, 255, 0.15);
        }

        #notification-trigger {
            background: linear-gradient(135deg, rgba(0, 242, 255, 0.05) 0%, rgba(59, 130, 246, 0.05) 100%);
            border: 1px dashed rgba(0, 242, 255, 0.3);
            transition: all 0.3s ease;
        }

        #notification-trigger:hover {
            background: linear-gradient(135deg, rgba(0, 242, 255, 0.1) 0%, rgba(59, 130, 246, 0.1) 100%);
            border-color: rgba(0, 242, 255, 0.6);
            box-shadow: 0 0 20px rgba(0, 242, 255, 0.2);
            transform: scale(1.02);
        }

        .header-title {
            background: linear-gradient(135deg, #f8fafc 0%, #0ff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: shimmer 3s ease-in-out infinite;
        }

        @keyframes shimmer {
            0%, 100% { filter: brightness(1); }
            50% { filter: brightness(1.1); }
        }

        .reset-btn {
            opacity: 0.7;
            transition: all 0.3s ease;
        }

        .reset-btn:hover {
            opacity: 1;
            text-shadow: 0 0 10px rgba(239, 68, 68, 0.5);
        }

        /* Telefon uchun (0px - 640px) */
        @media (max-width: 640px) {
            body {
                padding: 12px;
            }

            .glass-panel {
                border-radius: 16px;
                padding: 16px !important;
            }

            h1 {
                font-size: 1.75rem !important;
            }

            h2 {
                font-size: 1rem !important;
            }

            #canvas-container {
                padding: 8px;
            }

            .grid-cols-3 {
                gap: 8px !important;
            }

            .stat-card {
                padding: 10px !important;
                font-size: 0.8rem;
            }

            .text-2xl {
                font-size: 1.25rem !important;
            }

            .text-xl {
                font-size: 1rem !important;
            }

            input[type="date"] {
                font-size: 16px;
                padding: 12px !important;
            }

            button {
                padding: 12px !important;
                font-size: 0.95rem;
            }
        }

        /* Tablet uchun (641px - 1024px) */
        @media (min-width: 641px) and (max-width: 1024px) {
            body {
                padding: 16px;
            }

            .glass-panel {
                border-radius: 20px;
                padding: 20px !important;
            }

            h1 {
                font-size: 2.25rem !important;
            }

            h2 {
                font-size: 1.25rem !important;
            }

            .grid-cols-3 {
                gap: 12px !important;
            }

            .stat-card {
                padding: 16px !important;
            }
        }

        /* Kompyuter uchun (1025px va undan ko'p) */
        @media (min-width: 1025px) {
            body {
                padding: 24px;
            }

            .glass-panel {
                border-radius: 24px;
                padding: 24px !important;
            }
        }

        /* Landscape mode uchun */
        @media (max-height: 600px) {
            body {
                padding: 8px;
            }

            .glass-panel {
                padding: 12px !important;
            }

            .space-y-6 {
                gap: 12px !important;
            }
        }
    </style>
</head>
<body>

<!-- Setup Modal -->
<div id="setup-modal" class="hidden">
    <div class="glass-panel max-w-sm w-[95%] mx-auto text-center">
        <h2 class="text-2xl font-bold mb-4 text-cyan-400">Xush kelibsiz!</h2>
        <p class="text-gray-400 mb-6">Tug'ilgan sanangizni kiriting:</p>
        <input type="date" id="birth-date-input" class="w-full bg-slate-800 border border-slate-700 rounded-xl p-3 mb-6 outline-none focus:border-cyan-500 transition-all text-white">
        <button id="save-btn" class="w-full bg-cyan-600 hover:bg-cyan-500 text-white font-bold py-3 rounded-xl transition-all">Boshlash</button>
    </div>
</div>

<div class="content-wrapper space-y-6">
    <!-- Header Card -->
    <div class="glass-panel p-6">
        <div class="flex justify-between items-start">
            <div>
                <h1 class="text-3xl font-black tracking-tight header-title" id="user-age-title">Salom!</h1>
                <p class="text-cyan-400 font-semibold text-sm mt-2" id="current-time-display">00:00:00</p>
            </div>
            <button onclick="resetData()" class="reset-btn text-xs text-gray-500 hover:text-red-400 uppercase tracking-tighter font-semibold px-3 py-2 rounded-lg hover:bg-red-500/10 transition-all duration-300">⟲ Ma'lumotlarni to'chirish</button>
        </div>
    </div>

    <!-- Life Progress Card -->
    <div class="glass-panel p-6">
        <div class="flex justify-between items-end mb-4">
            <div>
                <h2 class="text-sm font-bold text-gray-400 uppercase tracking-widest">Umr Progressi (75 yil)</h2>
                <p class="text-2xl font-black" id="life-percent">0.000000%</p>
            </div>
            <div class="text-right">
                <p class="text-xs text-gray-500" id="life-days-info">... kun yashaldi</p>
            </div>
        </div>
        
        <div id="canvas-container">
            <canvas id="lifeCanvas"></canvas>
        </div>
        
        <p class="text-[10px] text-gray-500 text-center italic mt-4">Har bir nuqta hayotingizning bir kunini anglatadi. Ko'k - yashalgan, xira - kelajak.</p>
    </div>

    <!-- 2026 Year Progress Card -->
    <div class="glass-panel p-6 relative overflow-hidden">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-xl font-extrabold">2026-yil Progressi</h2>
            <span id="year-percent" class="text-2xl font-bold text-cyan-400">0.0%</span>
        </div>

        <div class="w-full bg-slate-800 h-3 rounded-full overflow-hidden mb-6">
            <div id="year-bar" class="h-full bg-gradient-to-r from-cyan-500 to-blue-600 progress-glow transition-all duration-1000" style="width: 0%"></div>
        </div>

        <div class="grid grid-cols-3 gap-4 text-center">
            <div class="stat-card">
                <p class="text-[10px] text-gray-500 uppercase font-semibold tracking-wider">Qoldi</p>
                <p class="text-lg font-bold text-blue-400 mt-2" id="days-left">0</p>
                <p class="text-[10px] text-gray-400 mt-1">kun</p>
            </div>
            <div class="stat-card">
                <p class="text-[10px] text-gray-500 uppercase font-semibold tracking-wider">Bugun</p>
                <p class="text-lg font-bold text-cyan-400 mt-2" id="today-percent">0%</p>
                <p class="text-[10px] text-gray-400 mt-1">o'tdi</p>
            </div>
            <div class="stat-card">
                <p class="text-[10px] text-gray-500 uppercase font-semibold tracking-wider">Shu oy</p>
                <p class="text-lg font-bold text-purple-400 mt-2" id="month-percent">0%</p>
                <p class="text-[10px] text-gray-400 mt-1">tugadi</p>
            </div>
        </div>
    </div>

    <!-- Notifications -->
    <div id="notification-trigger" class="p-6 cursor-pointer rounded-2xl transition-all duration-300">
        <div class="flex flex-col items-center justify-center">
            <p class="text-sm text-gray-300 italic text-center leading-relaxed">"Vaqt — bu sizga berilgan yagona va eng qimmatli xazina."</p>
            <p id="notify-status-text" class="text-xs mt-4 text-cyan-400 font-semibold flex items-center gap-2">
                <span id="notify-icon">🔔</span>
                <span>Kunlik eslatmalarni yoqish</span>
            </p>
        </div>
    </div>
</div>

<script>
    const STORAGE_KEY = 'life_tracker_birthdate';
    const AVERAGE_LIFE_EXPECTANCY = 75;
    const TOTAL_LIFE_DAYS = Math.floor(AVERAGE_LIFE_EXPECTANCY * 365.25);

    const modal = document.getElementById('setup-modal');
    const birthInput = document.getElementById('birth-date-input');
    const saveBtn = document.getElementById('save-btn');
    const canvas = document.getElementById('lifeCanvas');
    const ctx = canvas.getContext('2d');
    
    const lifePercentText = document.getElementById('life-percent');
    const lifeDaysInfo = document.getElementById('life-days-info');
    const userAgeTitle = document.getElementById('user-age-title');
    const currentTimeDisplay = document.getElementById('current-time-display');
    const yearPercentText = document.getElementById('year-percent');
    const yearBar = document.getElementById('year-bar');
    const daysLeftText = document.getElementById('days-left');
    const todayPercentText = document.getElementById('today-percent');
    const monthPercentText = document.getElementById('month-percent');

    let birthDate = localStorage.getItem(STORAGE_KEY);

    function init() {
        if (!birthDate) {
            modal.classList.remove('hidden');
        } else {
            modal.classList.add('hidden');
            setupCanvas();
            startLoops();
        }
    }

    saveBtn.addEventListener('click', () => {
        if (birthInput.value) {
            localStorage.setItem(STORAGE_KEY, birthInput.value);
            birthDate = birthInput.value;
            saveBtn.innerText = '⏳ Yuklanmoqda...';
            saveBtn.disabled = true;
            setTimeout(() => {
                modal.classList.add('hidden');
                setupCanvas();
                startLoops();
            }, 500);
        } else {
            birthInput.style.animation = 'pulse 0.5s ease-out';
            setTimeout(() => birthInput.style.animation = '', 500);
        }
    });

    birthInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter' && birthInput.value) {
            saveBtn.click();
        }
    });

    function setupCanvas() {
        // Canvas hajmini moslashtirish
        const container = document.getElementById('canvas-container');
        const containerWidth = container.offsetWidth - 20; // padding hisobiga olgan
        
        const cols = window.innerWidth < 640 ? 40 : window.innerWidth < 1024 ? 50 : 60;
        const rows = Math.ceil(TOTAL_LIFE_DAYS / cols);
        const dotSize = window.innerWidth < 640 ? 3 : window.innerWidth < 1024 ? 4 : 4;
        const gap = 2;
        
        const maxWidth = cols * (dotSize + gap);
        canvas.width = Math.min(maxWidth, containerWidth) || maxWidth;
        canvas.height = rows * (dotSize + gap);
        
        drawLifeDots();
    }

    function drawLifeDots() {
        const now = new Date();
        const birth = new Date(birthDate);
        const livedMs = now - birth;
        const livedDays = Math.floor(livedMs / (1000 * 60 * 60 * 24));
        
        const cols = window.innerWidth < 640 ? 40 : window.innerWidth < 1024 ? 50 : 60;
        const dotSize = window.innerWidth < 640 ? 3 : window.innerWidth < 1024 ? 4 : 4;
        const gap = 2;
        
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        for (let i = 0; i < TOTAL_LIFE_DAYS; i++) {
            const x = (i % cols) * (dotSize + gap);
            const y = Math.floor(i / cols) * (dotSize + gap);
            
            if (i < livedDays) {
                ctx.fillStyle = '#1e293b'; // Yashalgan kunlar
            } else if (i === livedDays) {
                ctx.fillStyle = '#ef4444'; // Bugun
            } else {
                ctx.fillStyle = '#00f2ff33'; // Kelajak
            }
            
            ctx.beginPath();
            ctx.arc(x + dotSize/2, y + dotSize/2, dotSize/2, 0, Math.PI * 2);
            ctx.fill();
        }
    }

    function updateCounters() {
        const now = new Date();
        const birth = new Date(birthDate);
        
        // Hayot progressi
        const livedMs = now - birth;
        const totalMs = TOTAL_LIFE_DAYS * 24 * 60 * 60 * 1000;
        const lifePercent = Math.max(0, (livedMs / totalMs) * 100);
        
        // Sonni animasiya qilish
        animateValue(lifePercentText, parseFloat(lifePercentText.innerText), lifePercent, 500);
        lifePercentText.innerText = lifePercent.toFixed(7) + '%';
        
        const livedDays = Math.floor(livedMs / (1000 * 60 * 60 * 24));
        lifeDaysInfo.innerText = `${livedDays.toLocaleString()} kun yashaldi`;
        
        const age = Math.floor(livedDays / 365.25);
        userAgeTitle.innerText = `Siz ${age} yoshdasiz`;

        // 2026-yil progressi
        const year = 2026;
        const start = new Date(year, 0, 1);
        const end = new Date(year, 11, 31, 23, 59, 59);
        const yearTotal = end - start;
        const yearPassed = now - start;
        const yearPercent = Math.max(0, Math.min(100, (yearPassed / yearTotal) * 100));
        
        yearPercentText.innerText = yearPercent.toFixed(1) + '%';
        yearBar.style.width = yearPercent + '%';
        
        const msLeft = end - now;
        const daysLeft = Math.ceil(msLeft / (1000 * 60 * 60 * 24));
        daysLeftText.innerText = daysLeft > 0 ? daysLeft : 0;

        // Kunlik progress
        const dayPassed = (now.getHours() * 3600 + now.getMinutes() * 60 + now.getSeconds()) / 86400 * 100;
        todayPercentText.innerText = Math.floor(dayPassed) + '%';

        // Oylik progress
        const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
        const endOfMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0);
        const monthPercent = ((now - startOfMonth) / (endOfMonth - startOfMonth)) * 100;
        monthPercentText.innerText = Math.floor(monthPercent) + '%';

        currentTimeDisplay.innerText = now.toLocaleTimeString('uz-UZ', { hour12: false });
    }

    function animateValue(element, start, end, duration) {
        let startTime = null;
        const animate = (currentTime) => {
            if (startTime === null) startTime = currentTime;
            const progress = (currentTime - startTime) / duration;
            if (progress < 1) {
                requestAnimationFrame(animate);
            }
        };
        requestAnimationFrame(animate);
    }

    function startLoops() {
        updateCounters();
        // Har soniyada raqamlar yangilanadi
        setInterval(updateCounters, 1000);
        // Har soatda nuqtalar qayta chiziladi (chunki kun o'zgarishi mumkin)
        setInterval(drawLifeDots, 3600000);
        // Oynani o'zgartirganda canvas qayta chiziladi
        window.addEventListener('resize', () => {
            setupCanvas();
        });
    }

    function resetData() {
        if(confirm("Barcha ma'lumotlarni o'chirib, qaytadan sozlashni xohlaysizmi?")) {
            localStorage.removeItem(STORAGE_KEY);
            location.reload();
        }
    }

    // Push Notifications
    document.getElementById('notification-trigger').addEventListener('click', async () => {
        if (!("Notification" in window)) {
            alert("Brauzeringiz bildirishnomalarni qo'llab-quvvatlamaydi.");
            return;
        }

        const notifyIcon = document.getElementById('notify-icon');
        const notifyText = document.getElementById('notify-status-text');
        const permission = await Notification.requestPermission();
        if (permission === "granted") {
            notifyIcon.innerText = "✅";
            notifyText.innerHTML = '<span>✓ Eslatmalar yoqildi</span>';
            alert("Har kuni 09:00 da yil progressi haqida xabar olasiz.");
            
            setInterval(() => {
                const d = new Date();
                if(d.getHours() === 9 && d.getMinutes() === 0 && d.getSeconds() === 0) {
                    new Notification("📊 Vaqt qadri", {
                        body: `Bugun yilning ${yearPercentText.innerText} qismi o'tdi. Uni mazmunli o'tkazing!`,
                        icon: "https://cdn-icons-png.flaticon.com/512/2997/2997300.png"
                    });
                }
            }, 1000);
        }
    });

    init();
</script>

</body>
</html>
