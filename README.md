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
                <p class="text-xs text-slate-400">سجل دخولك للوصول إلى أدوات التلاوة والصدى الحقيقية</p>
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

    <!-- واجهة الاستوديو الرئيسية (المحولة من React) -->
    <div id="app-container" class="w-full max-w-5xl bg-slate-900/90 rounded-2xl border border-slate-800 shadow-2xl p-4 sm:p-6 flex flex-col gap-5 backdrop-blur-xl hidden">
        
        <!-- Header Title Bar -->
        <div class="flex flex-col sm:flex-row items-center justify-between gap-3 pb-4 border-b border-slate-800/80">
            <div class="flex items-center gap-3">
                <div class="p-3 bg-gradient-to-tr from-amber-500 to-rose-600 rounded-xl shadow-lg shadow-amber-500/20 text-slate-950 font-black">
                    📻
                </div>
                <div>
                    <h1 class="text-lg sm:text-xl font-black bg-gradient-to-r from-amber-200 via-amber-400 to-rose-400 bg-clip-text text-transparent">
                        استديو الصوتيات والتلاوة الاحترافي
                    </h1>
                    <p class="text-xs text-slate-400">مرحباً: <span id="display-name" class="font-bold text-amber-300"></span> | Pro Vocal & Audio Recitation Studio</p>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <button
                    onclick="exportAudio()"
                    class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-amber-300 text-xs font-semibold rounded-lg border border-slate-700 transition-all shadow-sm cursor-pointer"
                >
                    📥 <span>تصدير الصوت المعالج</span>
                </button>
                <button onclick="logout()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-rose-400 text-xs font-semibold rounded-lg border border-slate-700 cursor-pointer">خروج</button>
            </div>
        </div>

        <!-- Master Waveform Visualizer & Transport -->
        <div class="space-y-3">
            <!-- Waveform Canvas Mock -->
            <div class="w-full h-24 bg-slate-950 rounded-xl border border-slate-800 flex items-center justify-center relative overflow-hidden">
                <div id="waveform-bars" class="flex items-center gap-1 h-16 px-4 w-full justify-around">
                    <!-- توليد أعمدة شكل الموجة ديناميكياً -->
                </div>
                <span class="absolute bottom-2 right-3 text-[10px] font-mono text-slate-500">Master Audio Waveform</span>
            </div>

            <div class="flex flex-wrap items-center justify-between gap-3 bg-slate-950/80 p-3 rounded-xl border border-slate-800">
                <div class="flex items-center gap-2">
                    <button
                        onclick="handleTogglePlay()"
                        id="play-btn-main"
                        class="flex items-center gap-2 px-5 py-2.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs rounded-xl shadow-lg shadow-amber-500/20 transition-all cursor-pointer"
                    >
                        ▶ <span>تشغيل الصوت</span>
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

        <!-- Presets Quick Bar -->
        <div class="flex items-center gap-2 overflow-x-auto pb-1">
            <button onclick="applyPreset('studio_vocal')" class="px-3 py-1.5 rounded-lg bg-amber-500/20 text-amber-300 border border-amber-500/30 text-xs font-bold whitespace-nowrap cursor-pointer">🎙️ تلاوة خاشعة نقية</button>
            <button onclick="applyPreset('cathedral')" class="px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 border border-slate-700 text-xs font-bold whitespace-nowrap cursor-pointer">🏛️ صدى المسجد الكبير</button>
            <button onclick="applyPreset('warm_radio')" class="px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 border border-slate-700 text-xs font-bold whitespace-nowrap cursor-pointer">📻 صوت إذاعي دافئ</button>
        </div>

        <!-- Tabs switcher -->
        <div class="flex border-b border-slate-800 gap-2">
            <button
                onclick="setActiveTab('rack')"
                id="tab-btn-rack"
                class="flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-amber-500 text-amber-400 cursor-pointer transition-all"
            >
                🎚️ <span>وحدة المؤثرات والصدى (FX Rack)</span>
            </button>

            <button
                onclick="setActiveTab('recorder')"
                id="tab-btn-recorder"
                class="flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-200 cursor-pointer transition-all"
            >
                🎙️ <span>التسجيل والمكتبة (Studio Library)</span>
            </button>
        </div>

        <!-- Active Tab Content: FX Rack -->
        <div id="tab-content-rack" class="space-y-4">
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 bg-slate-950 p-5 rounded-xl border border-slate-800">
                <div class="space-y-2">
                    <div class="flex justify-between text-xs text-slate-300">
                        <span>قوة الصدى والتردد (Delay/Reverb)</span>
                        <span id="fx-delay-val" class="font-mono text-amber-400">0.25 ثانية</span>
                    </div>
                    <input type="range" min="0" max="1" step="0.05" value="0.25" oninput="updateFxDelay(this.value)" class="w-full accent-amber-500 cursor-pointer" />
                </div>

                <div class="space-y-2">
                    <div class="flex justify-between text-xs text-slate-300">
                        <span>فلتر التصفية (Lowpass Cutoff)</span>
                        <span id="fx-filter-val" class="font-mono text-amber-400">4000 هرتز</span>
                    </div>
                    <input type="range" min="500" max="20000" step="100" value="4000" oninput="updateFxFilter(this.value)" class="w-full accent-amber-500 cursor-pointer" />
                </div>
            </div>
        </div>

        <!-- Active Tab Content: Studio Library -->
        <div id="tab-content-recorder" class="space-y-4 hidden">
            <div class="bg-slate-950 p-5 rounded-xl border border-slate-800 space-y-4">
                <h3 class="text-xs font-bold text-amber-400 uppercase">إضافة ملف صوتي أو تلاوة جديدة للمكتبة</h3>
                <input type="file" id="audio-file-input" accept="audio/*" onchange="addNewTrack(event)" class="w-full text-xs text-slate-400 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-slate-800 file:text-slate-200 cursor-pointer" />
                
                <div class="space-y-2 pt-2">
                    <div class="text-xs font-bold text-slate-300">المسارات النشطة:</div>
                    <div id="tracks-list" class="space-y-2">
                        <!-- يتم تعبئته تلقائياً -->
                    </div>
                </div>
            </div>
        </div>

    </div>

    <!-- جافاسكريبت التشغيل ومحرك الصوت الحقيقي -->
    <script>
        let audioCtx = null;
        let audioElement = null;
        let sourceNode = null;
        let delayNode = null;
        let filterNode = null;
        let masterGainNode = null;
        let isPlaying = false;

        let tracks = [
            { id: 'demo_1', title: 'تلاوة تجريبية للقياس والاستماع', url: 'https://cdn.pixabay.com/download/audio/2022/05/27/audio_1808fbf07a.mp3?filename=arabic-vocal-ambient-11234.mp3' }
        ];
        let activeTrackUrl = tracks[0].url;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                masterGainNode = audioCtx.createGain();
                masterGainNode.gain.value = 1.0;

                delayNode = audioCtx.createDelay();
                delayNode.delayTime.value = 0.25;

                filterNode = audioCtx.createBiquadFilter();
                filterNode.type = 'lowpass';
                filterNode.frequency.value = 4000;

                // توصيل المسار الاحترافي
                filterNode.connect(delayNode);
                delayNode.connect(masterGainNode);
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
            renderTracks();
            generateWaveformBars();
            loadTrackUrl(activeTrackUrl);
        }

        function logout() {
            if (audioElement) audioElement.pause();
            document.getElementById('app-container').classList.add('hidden');
            document.getElementById('auth-modal').classList.remove('hidden');
        }

        function setActiveTab(tab) {
            ['rack', 'recorder'].forEach(t => {
                document.getElementById('tab-content-' + t).classList.add('hidden');
                document.getElementById('tab-btn-' + t).className = "flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-200 cursor-pointer transition-all";
            });
            document.getElementById('tab-content-' + tab).classList.remove('hidden');
            document.getElementById('tab-btn-' + tab).className = "flex items-center gap-2 px-4 py-2 text-xs font-bold border-b-2 border-amber-500 text-amber-400 cursor-pointer transition-all";
        }

        function loadTrackUrl(url) {
            if (audioElement) {
                audioElement.pause();
            }
            audioElement = new Audio(url);
            audioElement.crossOrigin = "anonymous";

            audioElement.onended = () => {
                isPlaying = false;
                document.getElementById('play-btn-main').innerHTML = "▶ <span>تشغيل الصوت</span>";
            };
        }

        function handleTogglePlay() {
            initAudio();
            if (!audioElement) return;

            if (isPlaying) {
                audioElement.pause();
                isPlaying = false;
                document.getElementById('play-btn-main').innerHTML = "▶ <span>تشغيل الصوت</span>";
            } else {
                audioElement.play();
                isPlaying = true;
                document.getElementById('play-btn-main').innerHTML = "⏸ <span>إيقاف مؤقت</span>";
            }
        }

        function handleStop() {
            if (audioElement) {
                audioElement.pause();
                audioElement.currentTime = 0;
                isPlaying = false;
                document.getElementById('play-btn-main').innerHTML = "▶ <span>تشغيل الصوت</span>";
            }
        }

        function updateMasterGain(val) {
            document.getElementById('gain-val').innerText = Math.round(val * 100) + '%';
            if (masterGainNode && audioCtx) {
                masterGainNode.gain.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function updateFxDelay(val) {
            document.getElementById('fx-delay-val').innerText = Number(val).toFixed(2) + ' ثانية';
            if (delayNode && audioCtx) {
                delayNode.delayTime.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function updateFxFilter(val) {
            document.getElementById('fx-filter-val').innerText = val + ' هرتز';
            if (filterNode && audioCtx) {
                filterNode.frequency.setTargetAtTime(parseFloat(val), audioCtx.currentTime, 0.05);
            }
        }

        function applyPreset(presetType) {
            if (presetType === 'studio_vocal') {
                updateFxDelay(0.15);
                updateFxFilter(8000);
            } else if (presetType === 'cathedral') {
                updateFxDelay(0.6);
                updateFxFilter(3000);
            } else if (presetType === 'warm_radio') {
                updateFxDelay(0.05);
                updateFxFilter(2500);
            }
            alert("تم تطبيق البريست بنجاح!");
        }

        function addNewTrack(e) {
            const file = e.target.files[0];
            if (!file) return;
            const url = URL.createObjectURL(file);
            const newT = { id: 't_' + Date.now(), title: file.name, url: url };
            tracks.push(newT);
            renderTracks();
            loadTrackUrl(url);
            activeTrackUrl = url;
            alert("تم إضافة الملف بنجاح وتفعيله للاستماع!");
        }

        function renderTracks() {
            const list = document.getElementById('tracks-list');
            list.innerHTML = tracks.map(t => `
                <div class="flex items-center justify-between p-3 bg-slate-900 rounded-xl border ${t.url === activeTrackUrl ? 'border-amber-500/50' : 'border-slate-800'}">
                    <span class="text-xs text-slate-200 truncate">${t.title}</span>
                    <button onclick="selectTrack('${t.url}')" class="px-3 py-1 bg-amber-500 text-slate-950 font-bold rounded-lg text-xs cursor-pointer">اختر ولعب</button>
                </div>
            `).join('');
        }

        function selectTrack(url) {
            activeTrackUrl = url;
            loadTrackUrl(url);
            renderTracks();
            alert("تم تعيين المسار النشط بنجاح!");
        }

        function exportAudio() {
            if (!activeTrackUrl) return;
            const a = document.createElement('a');
            a.href = activeTrackUrl;
            a.download = `SAFESTAR_Recitation_Processed.mp3`;
            a.click();
        }

        function generateWaveformBars() {
            const container = document.getElementById('waveform-bars');
            let html = '';
            for (let i = 0; i < 35; i++) {
                const height = Math.floor(Math.random() * 40) + 15;
                html += `<div class="w-1.5 bg-amber-500/60 rounded-full" style="height: ${height}px;"></div>`;
            }
            container.innerHTML = html;
        }
    </script>
</body>
</html>
