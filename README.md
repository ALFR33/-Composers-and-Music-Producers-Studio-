<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SAFESTAR • Pro Vocal & Audio Recitation Studio</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        slate: { 950: '#020617', 900: '#0f172a', 800: '#1e293b', 700: '#334155', 400: '#94a3b8', 300: '#cbd5e1' },
                        amber: { 500: '#f59e0b', 400: '#fbbf24', 300: '#fcd34d' },
                        rose: { 600: '#e11d48', 500: '#f43f5e' },
                        cyan: { 500: '#06b6d4', 400: '#22d3ee' }
                    }
                }
            }
        }
    </script>
</head>
<body class="min-h-screen w-full bg-slate-950 text-slate-100 font-sans select-none antialiased flex flex-col items-center justify-center p-2 sm:p-4 selection:bg-amber-500 selection:text-slate-950">

    <!-- نافذة تسجيل الدخول -->
    <div id="auth-modal" class="fixed inset-0 z-50 bg-slate-950/95 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 p-8 rounded-3xl max-w-md w-full space-y-6 shadow-2xl">
            <div class="text-center space-y-2">
                <div class="w-12 h-12 rounded-2xl bg-gradient-to-tr from-amber-500 to-rose-600 mx-auto flex items-center justify-center font-black text-xl text-slate-950 shadow-lg">⭐</div>
                <h2 class="text-2xl font-bold text-white">منصة SAFESTAR الاستوديو الاحترافي</h2>
                <p class="text-xs text-slate-400">سجل دخولك للوصول إلى أدوات التلاوة والصدى وتركيب الألحان</p>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">الاسم الكريم</label>
                    <input type="text" id="user-name" placeholder="صالح" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-amber-500" />
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">البريد الإلكتروني</label>
                    <input type="email" id="user-email" placeholder="name@example.com" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-amber-500" />
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-300 mb-1">كلمة المرور</label>
                    <input type="password" id="user-pass" placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-amber-500" />
                </div>
                <button type="button" onclick="executeLogin()" class="w-full py-3.5 rounded-xl bg-gradient-to-r from-amber-500 to-rose-600 text-slate-950 font-black text-sm shadow-lg shadow-amber-500/20 hover:opacity-90 transition cursor-pointer">دخول الاستوديو</button>
            </div>
        </div>
    </div>

    <!-- واجهة الاستوديو الرئيسية -->
    <div id="app-container" class="w-full max-w-5xl bg-slate-900/90 rounded-2xl border border-slate-800 shadow-2xl p-4 sm:p-6 flex flex-col gap-5 backdrop-blur-xl hidden">
        
        <!-- Header Title Bar -->
        <div class="flex flex-col sm:flex-row items-center justify-between gap-3 pb-4 border-b border-slate-800/80">
            <div class="flex items-center gap-3">
                <div class="p-3 bg-gradient-to-tr from-amber-500 to-rose-600 rounded-xl shadow-lg shadow-amber-500/20 text-slate-950">
                    📻
                </div>
                <div>
                    <h1 class="text-lg sm:text-xl font-black bg-gradient-to-r from-amber-200 via-amber-400 to-rose-400 bg-clip-text text-transparent">
                        استديو الصوتيات والتلاوة الاحترافي (SAFESTAR)
                    </h1>
                    <p class="text-xs text-slate-400">مرحباً: <span id="display-name" class="font-bold text-amber-300"></span> | Pro Vocal & Audio Recitation Studio</p>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="logout()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-rose-400 text-xs font-semibold rounded-lg border border-slate-700 transition-all cursor-pointer">تسجيل خروج</button>
            </div>
        </div>

        <!-- Master Transport & Playback -->
        <div class="space-y-3">
            <div class="bg-slate-950/80 p-4 rounded-xl border border-slate-800 flex flex-col gap-3">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-bold text-amber-400">📁 تشغيل وتصفية ملف صوتي أو تلاوة قرآنية:</span>
                    <input type="file" id="audio-file-input" accept="audio/*" onchange="loadAudioFile(event)" class="text-xs text-slate-400 file:py-1 file:px-3 file:rounded-lg file:border-0 file:text-xs file:font-semibold file:bg-slate-800 file:text-slate-200 cursor-pointer" />
                </div>
            </div>

            <div class="flex flex-wrap items-center justify-between gap-3 bg-slate-950/80 p-3 rounded-xl border border-slate-800">
                <div class="flex items-center gap-2">
                    <button
                        onclick="handleTogglePlay()"
                        class="flex items-center gap-2 px-5 py-2.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs rounded-xl shadow-lg shadow-amber-500/20 transition-all cursor-pointer"
                    >
                        <span id="play-btn-text">تشغيل الصوت</span>
                    </button>

                    <button
                        onclick="handleStop()"
                        class="p-2.5 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl transition-all border border-slate-700 cursor-pointer"
                        title="إيقاف"
                    >
                        ⏹
                    </button>
                </div>

                <!-- Master Gain Slider -->
                <div class="flex items-center gap-3">
                    <span class="text-xs text-slate-400">مستوى الصوت العام:</span>
                    <input
                        type="range"
                        min="0"
                        max="2"
                        step="0.05"
                        value="1"
                        oninput="updateMasterGain(this.value)"
                        class="w-28 accent-amber-500 h-1.5 bg-slate-800 rounded-lg cursor-pointer"
                    />
                    <span id="gain-val" class="text-xs font-mono text-amber-400 w-10">100%</span>
                </div>
            </div>
        </div>

        <!-- تبويبات الاستوديو (المؤثرات، الآلات، الريمكس) -->
        <div class="flex border-b border-slate-800 gap-2">
            <button onclick="switchTab('rack')" id="tab-btn-rack" class="flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-amber-500 text-amber-400 cursor-pointer">
                🎚️ وحدة المؤثرات والصدى (FX Rack)
            </button>
            <button onclick="switchTab('instruments')" id="tab-btn-instruments" class="flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-200 cursor-pointer">
                🪕 الآلات الموسيقية (عود، رغول، مجوز)
            </button>
            <button onclick="switchTab('remix')" id="tab-btn-remix" class="flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-200 cursor-pointer">
                🎛️ ريمكس وترانس إيقاعي
            </button>
        </div>

        <!-- Tab 1: Effects Rack (الصدى والفلتر) -->
        <div id="tab-content-rack" class="space-y-4">
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 bg-slate-950 p-4 rounded-xl border border-slate-800">
                <div class="space-y-2">
                    <div class="flex justify-between text-xs text-slate-300">
                        <span>قوة الصدى (Reverb / Delay)</span>
                        <span id="delay-display">0.0 ثانية</span>
                    </div>
                    <input type="range" min="0" max="0.8" step="0.05" value="0" oninput="updateDelayTime(this.value)" class="w-full accent-amber-500 cursor-pointer" />
                </div>
                <div class="space-y-2">
                    <div class="flex justify-between text-xs text-slate-300">
                        <span>فلتر التصفية (Lowpass Cutoff)</span>
                        <span id="filter-display">20000 هرتز</span>
                    </div>
                    <input type="range" min="500" max="20000" step="100" value="20000" oninput="updateFilterCutoff(this.value)" class="w-full accent-amber-500 cursor-pointer" />
                </div>
            </div>
        </div>

        <!-- Tab 2: Instruments (الآلات) -->
        <div id="tab-content-instruments" class="space-y-4 hidden">
            <div class="flex gap-2 flex-wrap">
                <button onclick="setInstrument('sine')" class="px-4 py-2 rounded-xl bg-amber-500 text-slate-950 font-bold text-xs cursor-pointer">🎹 بيانو</button>
                <button onclick="setInstrument('sawtooth')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎸 عود شرقي</button>
                <button onclick="setInstrument('square')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎶 مجوز شعبي</button>
                <button onclick="setInstrument('triangle')" class="px-4 py-2 rounded-xl bg-slate-800 text-slate-300 font-bold text-xs cursor-pointer">🎵 رغول صعيدي</button>
            </div>
            <div id="inst-label" class="text-xs font-mono text-amber-400 font-bold">الآلة الحالية: بيانو</div>
            <div class="p-4 bg-slate-950 rounded-xl flex justify-center gap-2 overflow-x-auto">
                <button onclick="playNote(261.63)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Do</button>
                <button onclick="playNote(293.66)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Re</button>
                <button onclick="playNote(329.63)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Mi</button>
                <button onclick="playNote(349.23)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Fa</button>
                <button onclick="playNote(392.00)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Sol</button>
                <button onclick="playNote(440.00)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">La</button>
                <button onclick="playNote(493.88)" class="w-12 h-28 bg-slate-100 text-slate-950 rounded-b-lg font-bold text-xs flex items-end justify-center pb-2 cursor-pointer">Si</button>
            </div>
        </div>

        <!-- Tab 3: Remix (الريمكس والترانس) -->
        <div id="tab-content-remix" class="space-y-4 hidden">
            <div class="bg-slate-950 p-6 rounded-xl border border-slate-800 text-center space-y-4">
                <h3 class="text-sm font-bold text-white">محرك ريمكس وترانس إيقاعي حقيقي</h3>
                <button id="trance-btn" onclick="toggleTrance()" class="px-6 py-3 rounded-xl bg-rose-600 hover:bg-rose-500 text-white font-bold text-xs shadow-lg cursor-pointer">▶ تشغيل إيقاع الترانس والريمكس</button>
            </div>
        </div>

    </div>

    <!-- جافاسكريبت محرك الصوت والواجهة -->
    <script>
        let audioCtx = null;
        let audioElement = null;
        let sourceNode = null;
        let delayNode = null;
        let filterNode = null;
        let masterGainNode = null;
        let isPlaying = false;

        let activeWaveform = 'sine';
        let tranceTimer = null;
        let isTranceActive = false;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                masterGainNode = audioCtx.createGain();
                masterGainNode.gain.value = 1.0;
                masterGainNode.connect(audioCtx.destination);
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        function executeLogin() {
            const name = document.getElementById('user-name').value.trim();
            const email = document.getElementById('user-email').value.trim();
            const pass = document.getElementById('user-pass').value.trim();

            if (!name || !email || !pass) {
                alert("الرجاء تعبئة الاسم والبريد وكلمة المرور");
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

        function switchTab(tab) {
            ['rack', 'instruments', 'remix'].forEach(t => {
                document.getElementById('tab-content-' + t).classList.add('hidden');
                document.getElementById('tab-btn-' + t).className = "flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-200 cursor-pointer";
            });
            document.getElementById('tab-content-' + tab).classList.remove('hidden');
            document.getElementById('tab-btn-' + tab).className = "flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-amber-500 text-amber-400 cursor-pointer";
        }

        function loadAudioFile(e) {
            initAudio();
            const file = e.target.files[0];
            if (!file) return;

            const url = URL.createObjectURL(file);
            if (audioElement) { audioElement.pause(); }

            audioElement = new Audio(url);
            audioElement.crossOrigin = "anonymous";

            sourceNode = audioCtx.createMediaElementSource(audioElement);
            filterNode = audioCtx.createBiquadFilter();
            filterNode.type = 'lowpass';
            filterNode.frequency.value = 20000;

            delayNode = audioCtx.createDelay();
            delayNode.delayTime.value = 0;

            // التوصيل
            sourceNode.connect(filterNode);
            filterNode.connect(delayNode);
            delayNode.connect(masterGainNode);
            sourceNode.connect(masterGainNode);

            audioElement.onended = () => {
                isPlaying = false;
                document.getElementById('play-btn-text').innerText = "تشغيل الصوت";
            };
        }

        function handleTogglePlay() {
            if (!audioElement) {
                alert("الرجاء اختيار ملف صوتي أولاً");
                return;
            }
            initAudio();

            if (isPlaying) {
                audioElement.pause();
                isPlaying = false;
                document.getElementById('play-btn-text').innerText = "تشغيل الصوت";
            } else {
                audioElement.play();
                isPlaying = true;
                document.getElementById('play-btn-text').innerText = "إيقاف مؤقت";
            }
        }

        function handleStop() {
            if (audioElement) {
                audioElement.pause();
                audioElement.currentTime = 0;
                isPlaying = false;
                document.getElementById('play-btn-text').innerText = "تشغيل الصوت";
            }
        }

        function updateMasterGain(val) {
            document.getElementById('gain-val').innerText = Math.round(val * 100) + '%';
            if (masterGainNode && audioCtx) {
                masterGainNode.gain.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function updateDelayTime(val) {
            document.getElementById('delay-display').innerText = Number(val).toFixed(2) + ' ثانية';
            if (delayNode && audioCtx) {
                delayNode.delayTime.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function updateFilterCutoff(val) {
            document.getElementById('filter-display').innerText = val + ' هرتز';
            if (filterNode && audioCtx) {
                filterNode.frequency.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function setInstrument(type) {
            activeWaveform = type;
            const names = { sine: 'بيانو', sawtooth: 'عود شرقي', square: 'مجوز شعبي', triangle: 'رغول صعيدي' };
            document.getElementById('inst-label').innerText = "الآلة الحالية: " + names[type];
        }

        function playNote(freq) {
            initAudio();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            osc.type = activeWaveform;
            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);

            gain.gain.setValueAtTime(0.4, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 1.0);

            osc.connect(gain);
            gain.connect(masterGainNode);

            osc.start();
            osc.stop(audioCtx.currentTime + 1.0);
        }

        function toggleTrance() {
            initAudio();
            const btn = document.getElementById('trance-btn');

            if (isTranceActive) {
                clearInterval(tranceTimer);
                isTranceActive = false;
                btn.innerText = "▶ تشغيل إيقاع الترانس والريمكس";
                btn.className = "px-6 py-3 rounded-xl bg-rose-600 hover:bg-rose-500 text-white font-bold text-xs shadow-lg cursor-pointer";
            } else {
                isTranceActive = true;
                btn.innerText = "⏹ إيقاف الريمكس";
                btn.className = "px-6 py-3 rounded-xl bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs shadow-lg cursor-pointer";

                tranceTimer = setInterval(() => {
                    const t = audioCtx.currentTime;
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();

                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(130, t);
                    osc.frequency.exponentialRampToValueAtTime(30, t + 0.1);

                    gain.gain.setValueAtTime(0.7, t);
                    gain.gain.exponentialRampToValueAtTime(0.001, t + 0.2);

                    osc.connect(gain);
                    gain.connect(masterGainNode);

                    osc.start(t);
                    osc.stop(t + 0.2);
                }, 250);
            }
        }
    </script>
</body>
</html>
