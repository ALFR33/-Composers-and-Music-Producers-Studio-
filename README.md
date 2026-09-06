<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SAFESTAR • Real Studio & Remix Engine</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        slate: { 950: '#020617', 900: '#0f172a', 800: '#1e293b', 700: '#334155', 400: '#94a3b8', 300: '#cbd5e1' },
                        cyan: { 500: '#06b6d4', 400: '#22d3ee' },
                        amber: { 500: '#f59e0b', 400: '#fbbf24' },
                        purple: { 600: '#9333ea', 500: '#a855f7' }
                    }
                }
            }
        }
    </script>
</head>
<body class="min-h-screen w-full bg-slate-950 text-slate-100 font-sans select-none antialiased flex flex-col items-center justify-start p-4">

    <!-- نافذة تسجيل الدخول (مصححة وتعمل 100%) -->
    <div id="auth-modal" class="fixed inset-0 z-50 bg-slate-950/95 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 p-8 rounded-3xl max-w-md w-full space-y-6 shadow-2xl">
            <div class="text-center space-y-2">
                <div class="w-12 h-12 rounded-2xl bg-gradient-to-tr from-cyan-500 to-amber-500 mx-auto flex items-center justify-center font-black text-xl text-slate-950">⭐</div>
                <h2 class="text-2xl font-bold text-white">منصة SAFESTAR الاستوديو الموسيقي</h2>
                <p class="text-xs text-slate-400">سجل دخولك لبدء صناعة الألحان والترانس والريمكسات الحقيقية</p>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">الاسم الكريم</label>
                    <input type="text" id="user-name" placeholder="صالح" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500" />
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">البريد الإلكتروني</label>
                    <input type="email" id="user-email" placeholder="name@example.com" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500" />
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">كلمة المرور</label>
                    <input type="password" id="user-pass" placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500" />
                </div>
                <button type="button" onclick="executeLogin()" class="w-full py-3.5 rounded-xl bg-gradient-to-r from-cyan-500 to-amber-500 text-slate-950 font-black text-sm shadow-lg hover:opacity-90 transition cursor-pointer">دخول الاستوديو</button>
            </div>
        </div>
    </div>

    <!-- واجهة الاستوديو الرئيسية -->
    <div id="app-container" class="w-full max-w-5xl flex flex-col gap-6 hidden">
        
        <header class="w-full bg-slate-900 border border-slate-800 p-4 rounded-2xl flex items-center justify-between shadow-xl">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-cyan-500 to-amber-500 flex items-center justify-center font-black text-slate-950">⭐</div>
                <div>
                    <h1 class="font-bold text-lg text-white">SAFESTAR STUDIO & REMIX ENGINE</h1>
                    <p class="text-[10px] text-cyan-400 font-mono">مرحباً بك: <span id="display-name" class="font-bold text-white"></span> | الاستوديو الحقيقي نشط</p>
                </div>
            </div>
            <button onclick="logout()" class="px-3 py-2 rounded-xl bg-slate-800 text-xs text-slate-300 cursor-pointer">خروج</button>
        </header>

        <!-- 1. استوديو تشغيل ومعالجة ملفات القرآن والصوتيات -->
        <div class="w-full bg-slate-900 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-2xl">
            <h2 class="text-base font-bold text-white">🎙️ معالج وتجويد وتصفية أصوات التلاوة والقرآن</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-3">
                    <label class="text-xs font-bold text-cyan-400">اختر ملف صوتي (تلاوة):</label>
                    <input type="file" id="audio-file-input" accept="audio/*" onchange="loadAudioFile(event)" class="w-full text-xs text-slate-400 file:mr-2 file:py-1.5 file:px-3 file:rounded-lg file:border-0 file:text-xs file:font-semibold file:bg-slate-800 file:text-slate-200" />
                    <button id="play-pause-btn" onclick="togglePlayAudio()" class="w-full py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black text-xs hidden cursor-pointer">▶ تشغيل الصوت المرفوع</button>
                </div>
                <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-3">
                    <label class="text-xs font-bold text-amber-400">التحكم بالصدى (Echo DSP):</label>
                    <input type="range" min="0" max="0.6" step="0.05" value="0" oninput="updateDelay(this.value)" class="w-full accent-amber-500" />
                    <label class="text-xs font-bold text-amber-400">تصفية الترددات (Lowpass):</label>
                    <input type="range" min="500" max="20000" step="100" value="20000" oninput="updateFilter(this.value)" class="w-full accent-amber-500" />
                </div>
            </div>
        </div>

        <!-- 2. محرك ترانس وريمكس إيقاعي حقيقي -->
        <div class="w-full bg-slate-900 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-2xl">
            <div class="flex items-center justify-between">
                <h2 class="text-base font-bold text-white">🎛️ محرك ريمكس وترانس إيقاعي حقيقي</h2>
                <button id="trance-play-btn" onclick="toggleTrancePlay()" class="px-6 py-2.5 rounded-xl bg-purple-600 hover:bg-purple-500 text-white font-bold text-xs shadow-lg cursor-pointer">▶ تشغيل إيقاع الترانس</button>
            </div>
        </div>

        <!-- 3. استوديو الآلات الشرقية الحقيقية -->
        <div class="w-full bg-slate-900 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-2xl">
            <h2 class="text-base font-bold text-white">🪕 العزف الحي على الآلات الشرقية والغربية</h2>
            <div class="flex gap-2 flex-wrap">
                <button onclick="setInstrument('piano')" class="px-4 py-2 rounded-xl bg-cyan-600 text-slate-950 font-bold text-xs cursor-pointer">🎹 بيانو</button>
                <button onclick="setInstrument('oud')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎸 عود شرقي</button>
                <button onclick="setInstrument('mijwiz')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎶 مجوز شعبي</button>
                <button onclick="setInstrument('argol')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎵 رغول صعيدي</button>
                <button onclick="setInstrument('duff')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🥁 دف وإيقاع</button>
            </div>
            
            <div id="current-inst-label" class="text-xs font-mono text-cyan-400 font-bold">الآلة الحالية: بيانو (Grand Piano)</div>

            <div class="p-4 bg-slate-950 rounded-xl flex justify-center gap-2 overflow-x-auto">
                <button onclick="playInstrumentNote(261.63)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Do</button>
                <button onclick="playInstrumentNote(293.66)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Re</button>
                <button onclick="playInstrumentNote(329.63)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Mi</button>
                <button onclick="playInstrumentNote(349.23)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Fa</button>
                <button onclick="playInstrumentNote(392.00)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Sol</button>
                <button onclick="playInstrumentNote(440.00)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">La</button>
                <button onclick="playInstrumentNote(493.88)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Si</button>
            </div>
        </div>

    </div>

    <!-- الكود البرمجي الحقيقي -->
    <script>
        let audioCtx = null;
        let audioElement = null;
        let audioSourceNode = null;
        let delayNode = null;
        let filterNode = null;
        let isPlayingAudio = false;

        let activeWaveform = 'sine';
        let tranceInterval = null;
        let isTrancePlaying = false;

        function initAudioContext() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            if (audioCtx.state === 'suspended') audioCtx.resume();
        }

        // دالة تسجيل الدخول المباشرة والمصححة
        function executeLogin() {
            const name = document.getElementById('user-name').value.trim();
            const email = document.getElementById('user-email').value.trim();
            const pass = document.getElementById('user-pass').value.trim();

            if (!name || !email || !pass) {
                alert("الرجاء تعبئة جميع الحقول (الاسم، البريد، كلمة المرور)");
                return;
            }

            document.getElementById('display-name').innerText = name;
            document.getElementById('auth-modal').classList.add('hidden');
            document.getElementById('app-container').classList.remove('hidden');
        }

        function logout() {
            document.getElementById('app-container').classList.add('hidden');
            document.getElementById('auth-modal').classList.remove('hidden');
        }

        function loadAudioFile(event) {
            initAudioContext();
            const file = event.target.files[0];
            if (!file) return;

            document.getElementById('play-pause-btn').classList.remove('hidden');
            const fileURL = URL.createObjectURL(file);

            if (audioElement) { audioElement.pause(); audioElement = null; }

            audioElement = new Audio(fileURL);
            audioElement.crossOrigin = "anonymous";

            audioSourceNode = audioCtx.createMediaElementSource(audioElement);
            filterNode = audioCtx.createBiquadFilter();
            filterNode.type = 'lowpass';
            filterNode.frequency.value = 20000;

            delayNode = audioCtx.createDelay();
            delayNode.delayTime.value = 0;

            audioSourceNode.connect(filterNode);
            filterNode.connect(delayNode);
            delayNode.connect(audioCtx.destination);
            audioSourceNode.connect(audioCtx.destination);

            audioElement.onended = () => {
                isPlayingAudio = false;
                document.getElementById('play-pause-btn').innerText = "▶ تشغيل الصوت المرفوع";
            };
        }

        function togglePlayAudio() {
            if (!audioElement) return;
            initAudioContext();
            if (isPlayingAudio) {
                audioElement.pause();
                isPlayingAudio = false;
                document.getElementById('play-pause-btn').innerText = "▶ تشغيل الصوت المرفوع";
            } else {
                audioElement.play();
                isPlayingAudio = true;
                document.getElementById('play-pause-btn').innerText = "⏸ إيقاف مؤقت";
            }
        }

        function updateDelay(val) {
            if (delayNode && audioCtx) delayNode.delayTime.setTargetAtTime(Number(val), audioCtx.currentTime, 0.05);
        }

        function updateFilter(val) {
            if (filterNode && audioCtx) filterNode.frequency.setTargetAtTime(Number(val), audioCtx.currentTime, 0.05);
        }

        function setInstrument(type) {
            activeWaveform = type;
            const names = { piano: 'بيانو (Grand Piano)', oud: 'عود شرقي (Oud String)', mijwiz: 'مجوز شعبي (Mijwiz Reed)', argol: 'رغول صعيدي (Argol)', duff: 'دف وإيقاع (Duff)' };
            document.getElementById('current-inst-label').innerText = "الآلة الحالية: " + names[type];
        }

        function playInstrumentNote(freq) {
            initAudioContext();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            if (activeWaveform === 'oud') osc.type = 'sawtooth';
            else if (activeWaveform === 'mijwiz') osc.type = 'square';
            else if (activeWaveform === 'argol') osc.type = 'triangle';
            else if (activeWaveform === 'duff') osc.type = 'sine';
            else osc.type = 'sine';

            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);

            gain.gain.setValueAtTime(0.4, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 1.0);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 1.0);
        }

        function toggleTrancePlay() {
            initAudioContext();
            const btn = document.getElementById('trance-play-btn');

            if (isTrancePlaying) {
                clearInterval(tranceInterval);
                isTrancePlaying = false;
                btn.innerText = "▶ تشغيل إيقاع الترانس";
                btn.className = "px-6 py-2.5 rounded-xl bg-purple-600 hover:bg-purple-500 text-white font-bold text-xs shadow-lg cursor-pointer";
            } else {
                isTrancePlaying = true;
                btn.innerText = "⏹ إيقاف الترانس";
                btn.className = "px-6 py-2.5 rounded-xl bg-rose-600 hover:bg-rose-500 text-white font-bold text-xs shadow-lg cursor-pointer";

                tranceInterval = setInterval(() => {
                    const t = audioCtx.currentTime;
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(120, t);
                    osc.frequency.exponentialRampToValueAtTime(30, t + 0.1);

                    gain.gain.setValueAtTime(0.8, t);
                    gain.gain.exponentialRampToValueAtTime(0.001, t + 0.25);

                    osc.connect(gain);
                    gain.connect(audioCtx.destination);

                    osc.start(t);
                    osc.stop(t + 0.25);
                }, 250);
            }
        }
    </script>
</body>
</html>
