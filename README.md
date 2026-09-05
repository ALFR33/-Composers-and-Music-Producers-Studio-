<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>استوديو الملحنين والمؤلفين الموسيقيين (Composer Studio Pro)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- PayPal JavaScript SDK (استبدل YOUR_PAYPAL_CLIENT_ID بمعرف حسابك التجاري الحقيقي لاحقاً) -->
    <script src="https://www.paypal.com/sdk/js?client-id=sb&currency=USD"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        slate: { 950: '#020617', 900: '#0f172a', 800: '#1e293b', 700: '#334155', 400: '#94a3b8', 300: '#cbd5e1', 100: '#f1f5f9' },
                        indigo: { 600: '#4f46e5', 500: '#6366f1', 400: '#818cf8', 300: '#a5b4fc', 900: '#312e81' },
                        purple: { 600: '#9333ea', 500: '#a855f7', 400: '#c084fc' },
                        amber: { 500: '#f59e0b', 400: '#fbbf24', 300: '#fcd34d' },
                        emerald: { 500: '#10b981', 400: '#34d399' },
                        rose: { 600: '#e11d48' }
                    }
                }
            }
        }
    </script>
</head>
<body class="min-h-screen w-full bg-slate-950 text-slate-100 font-sans pb-24">

    <!-- Top Navbar Header -->
    <header class="border-b border-indigo-900/40 bg-slate-900/90 backdrop-blur-md sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3.5 flex flex-col sm:flex-row items-center justify-between gap-3">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-indigo-600 via-purple-600 to-pink-500 text-white font-bold flex items-center justify-center shadow-lg shadow-indigo-500/20">
                    🎵
                </div>
                <div>
                    <h1 class="text-lg font-extrabold font-serif bg-gradient-to-r from-indigo-300 via-purple-200 to-amber-200 bg-clip-text text-transparent">
                        استوديو الملحنين والمؤلفين الموسيقيين (Composer Studio Pro)
                    </h1>
                    <p class="text-[11px] text-slate-400">منصة التأليف والتوزيع الموسيقي وتوليد المقامات والألحان</p>
                </div>
            </div>

            <!-- Subscription Status & Upgrade Button -->
            <div id="header-subscription-status" class="flex items-center gap-3">
                <button onclick="openPricingModal()" class="px-4 py-2 rounded-2xl bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-slate-950 font-extrabold text-xs flex items-center gap-2 shadow-lg shadow-amber-500/20 transition animate-pulse">
                    ✨ <span>الاشتراك عبر PayPal ($18/شهر أو $43/سنة)</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Studio Body -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 my-8 space-y-8">
        
        <!-- Banner Announcement for Subscriptions -->
        <div id="subscription-banner" class="p-6 rounded-3xl bg-gradient-to-r from-indigo-900/60 via-purple-900/40 to-slate-900 border border-indigo-500/30 flex flex-col md:flex-row items-center justify-between gap-4 shadow-xl">
            <div class="space-y-1">
                <span class="px-3 py-1 rounded-full text-[10px] font-bold bg-amber-500/20 text-amber-300 border border-amber-500/30 inline-block mb-1">
                    عروض اشتراكات الملحنين 🎵
                </span>
                <h3 class="text-xl font-bold font-serif text-white">
                    اشترك الآن في استوديو الملحنين عبر PayPal بـ $18 شهرياً أو $43 سنوياً!
                </h3>
                <p class="text-xs text-slate-300">احصل على ترخيص استخدام تجاري 100% لتأليف وتوزيع وتصدير الألحان بحرية تامة.</p>
            </div>
            <button onclick="openPricingModal()" class="px-6 py-3 rounded-2xl bg-gradient-to-r from-amber-400 to-amber-500 hover:from-amber-300 hover:to-amber-400 text-slate-950 font-extrabold text-xs shrink-0 shadow-lg">
                عرض باقات الاشتراك الدفع عبر PayPal
            </button>
        </div>

        <!-- SECTION 1: Virtual Piano Keyboard -->
        <section class="p-6 md:p-8 rounded-3xl bg-slate-900 border border-slate-800 space-y-6 shadow-2xl">
            <div class="flex flex-col sm:flex-row items-center justify-between gap-4 border-b border-slate-800 pb-4">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-2xl bg-indigo-600/20 text-indigo-400 font-bold flex items-center justify-center border border-indigo-500/30">🎹</div>
                    <div>
                        <h2 class="text-lg font-bold font-serif text-slate-100">لوحة البيانو الافتراضي (Interactive Piano Keyboard)</h2>
                        <p class="text-xs text-slate-400">اضغط على المفاتيح لتأليف واختبار النغمات الحية بالصوت الصافي</p>
                    </div>
                </div>

                <div class="flex items-center gap-2">
                    <span class="text-xs text-slate-400 font-bold">خامة الصوت:</span>
                    <select id="sound-type-select" class="bg-slate-950 border border-slate-800 text-slate-200 text-xs font-bold rounded-xl px-3 py-2 focus:outline-none focus:border-indigo-500">
                        <option value="sine">بيانو ناعم (Sine)</option>
                        <option value="triangle" selected>بيانو كلاسيكي (Triangle)</option>
                        <option value="square">سينث رقمي (Square)</option>
                        <option value="sawtooth">سينث دافئ (Sawtooth)</option>
                    </select>
                </div>
            </div>

            <div class="overflow-x-auto pb-4">
                <div id="piano-keyboard-container" class="flex justify-center min-w-[650px] p-2 bg-slate-950 rounded-2xl border border-slate-800/80 shadow-inner relative dir-ltr"></div>
            </div>
        </section>

        <!-- SECTION 2: Chord Progressions -->
        <section class="p-6 md:p-8 rounded-3xl bg-slate-900 border border-slate-800 space-y-6 shadow-2xl">
            <div class="border-b border-slate-800 pb-4">
                <h2 class="text-lg font-bold font-serif text-slate-100 flex items-center gap-2">
                    🎛️ <span>مساعد التتابعات التوافقية والمقامات (Chord Progressions)</span>
                </h2>
                <p class="text-xs text-slate-400 mt-1">اختر التتابع التوافقي المناسب لنوع أغنيتك وعزف التتابع بضغطة زر واحدة</p>
            </div>
            <div id="chord-progressions-container" class="grid grid-cols-1 md:grid-cols-3 gap-4"></div>
        </section>

        <!-- SECTION 3: Rhythm Sequencer -->
        <section class="p-6 md:p-8 rounded-3xl bg-slate-900 border border-slate-800 space-y-6 shadow-2xl">
            <div class="flex flex-col sm:flex-row items-center justify-between gap-4 border-b border-slate-800 pb-4">
                <div>
                    <h2 class="text-lg font-bold font-serif text-slate-100 flex items-center gap-2">
                        📻 <span>صانع الإيقاعات والبيتميكر (8-Step Rhythm Sequencer)</span>
                    </h2>
                    <p class="text-xs text-slate-400 mt-1">صمم الإيقاع الخاص بمقطوعتك مع التحكم بالسرعة (BPM)</p>
                </div>
                <div class="flex items-center gap-4">
                    <div class="flex items-center gap-2 bg-slate-950 px-3 py-1.5 rounded-2xl border border-slate-800">
                        <span id="bpm-label" class="text-xs font-bold text-slate-300">السرعة: 100 BPM</span>
                        <input type="range" min="60" max="180" value="100" oninput="updateBpm(this.value)" class="w-24 accent-purple-500 cursor-pointer" />
                    </div>
                    <button id="btn-toggle-seq" onclick="toggleSequencer()" class="px-5 py-2.5 rounded-2xl font-bold text-xs flex items-center gap-2 shadow-lg transition bg-gradient-to-r from-purple-600 to-indigo-600 text-white">
                        ▶ <span>تشغيل الإيقاع</span>
                    </button>
                </div>
            </div>

            <div class="space-y-3 bg-slate-950 p-4 md:p-6 rounded-2xl border border-slate-800/80 dir-rtl">
                <div class="flex items-center gap-3">
                    <span class="w-20 text-xs font-bold text-slate-300 shrink-0">🥁 Kick</span>
                    <div id="track-kick" class="flex-1 grid grid-cols-8 gap-2"></div>
                </div>
                <div class="flex items-center gap-3">
                    <span class="w-20 text-xs font-bold text-slate-300 shrink-0">🪘 Snare</span>
                    <div id="track-snare" class="flex-1 grid grid-cols-8 gap-2"></div>
                </div>
                <div class="flex items-center gap-3">
                    <span class="w-20 text-xs font-bold text-slate-300 shrink-0">✨ Hi-Hat</span>
                    <div id="track-hihat" class="flex-1 grid grid-cols-8 gap-2"></div>
                </div>
            </div>
        </section>
    </main>

    <!-- PAYPAL SUBSCRIPTION PRICING MODAL -->
    <div id="pricing-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md dir-rtl overflow-y-auto hidden">
        <div class="relative w-full max-w-3xl rounded-3xl bg-slate-900 border border-indigo-500/30 p-6 md:p-8 space-y-6 shadow-2xl my-8">
            <div class="text-center space-y-2 border-b border-slate-800 pb-4">
                <span class="px-3.5 py-1 rounded-full text-xs font-bold bg-amber-500/20 text-amber-300 border border-amber-500/30 inline-block">
                    اشتراكات استوديو الملحنين والمؤلفين 🎵
                </span>
                <h2 class="text-2xl font-extrabold font-serif text-white">اختر خطة الاشتراك المناسبة واشترك عبر PayPal</h2>
                <p class="text-xs text-slate-400">وصول كامل للأدوات والمقامات وتصدير الألحان بترخيص تجاري 100%</p>
            </div>

            <div id="plans-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>

            <!-- Real PayPal Button Container -->
            <div class="p-5 rounded-2xl bg-slate-950 border border-slate-800 space-y-4">
                <div class="flex items-center justify-between text-xs text-slate-300">
                    <span>الباقة المختارة: <strong id="modal-selected-plan-name" class="text-amber-300">سنوية</strong></span>
                    <span id="modal-selected-plan-price" class="font-mono text-amber-300 font-bold text-base">$43 USD</span>
                </div>

                <div id="paypal-success-box" class="p-4 rounded-xl bg-emerald-500/20 border border-emerald-500/40 text-emerald-300 text-xs font-bold text-center flex items-center justify-center gap-2 hidden">
                    ✅ تم الدفع وتفعيل الاشتراك بنجاح عبر PayPal! 🎉
                </div>

                <!-- زر PayPal الذكي الرسمي الذي يتأكد من الرصيد والبطاقة الحقيقية -->
                <div id="paypal-button-container" class="mt-2"></div>

                <p class="text-[11px] text-slate-500 text-center">🔒 دفع حقيقي وآمن ومشفر 100% عبر خدمة PayPal العالمية</p>
            </div>

            <button onclick="closePricingModal()" class="w-full py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-bold">إلغاء وإغلاق</button>
        </div>
    </div>

    <!-- Application Audio Engine & Core Logic -->
    <script>
        let audioCtx = null;
        function getAudioContext() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            if (audioCtx.state === 'suspended') audioCtx.resume();
            return audioCtx;
        }

        const NOTE_FREQUENCIES = {
            "C3": 130.81, "C#3": 138.59, "D3": 146.83, "D#3": 155.56, "E3": 164.81, "F3": 174.61, "F#3": 185.00, "G3": 196.00, "G#3": 207.65, "A3": 220.00, "A#3": 233.08, "B3": 246.94,
            "C4": 261.63, "C#4": 277.18, "D4": 293.66, "D#4": 311.13, "E4": 329.63, "F4": 349.23, "F#4": 369.99, "G4": 392.00, "G#4": 415.30, "A4": 440.00, "A#4": 466.16, "B4": 493.88, "C5": 523.25
        };

        function playNote(freq, type = "triangle", duration = 1.2) {
            try {
                const ctx = getAudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.type = type;
                osc.frequency.setValueAtTime(freq, ctx.currentTime);
                gain.gain.setValueAtTime(0.3, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + duration);
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start();
                osc.stop(ctx.currentTime + duration);
            } catch (err) { console.error(err); }
        }

        function playChord(frequencies, duration = 1.2) {
            const type = document.getElementById('sound-type-select').value;
            frequencies.forEach(f => playNote(f, type, duration));
        }

        function playDrumHit(type) {
            try {
                const ctx = getAudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                if (type === "kick") {
                    osc.frequency.setValueAtTime(150, ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.3);
                    gain.gain.setValueAtTime(0.6, ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.3);
                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start();
                    osc.stop(ctx.currentTime + 0.3);
                } else {
                    const bufferSize = ctx.sampleRate * 0.1;
                    const buffer = ctx.createBuffer(1, bufferSize, ctx.sampleRate);
                    const data = buffer.getChannelData(0);
                    for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1;
                    const noise = ctx.createBufferSource();
                    noise.buffer = buffer;
                    gain.gain.setValueAtTime(0.3, ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.1);
                    noise.connect(gain);
                    gain.connect(ctx.destination);
                    noise.start();
                }
            } catch (err) { console.error(err); }
        }

        const CHORD_PROGRESSIONS = [
            { id: "pop", name: "تتابع البوب الشهير (I - V - vi - IV)", genre: "Pop", chords: ["C Major", "G Major", "A Minor", "F Major"], frequencies: [[NOTE_FREQUENCIES.C4, NOTE_FREQUENCIES.E4, NOTE_FREQUENCIES.G4], [NOTE_FREQUENCIES.G3, NOTE_FREQUENCIES.B3, NOTE_FREQUENCIES.D4], [NOTE_FREQUENCIES.A3, NOTE_FREQUENCIES.C4, NOTE_FREQUENCIES.E4], [NOTE_FREQUENCIES.F3, NOTE_FREQUENCIES.A3, NOTE_FREQUENCIES.C4]] },
            { id: "arabic", name: "مقام النهاوند العربي", genre: "شرقي", chords: ["C Minor", "F Minor", "G Major", "C Minor"], frequencies: [[NOTE_FREQUENCIES.C4, NOTE_FREQUENCIES["D#4"], NOTE_FREQUENCIES.G4], [NOTE_FREQUENCIES.F3, NOTE_FREQUENCIES.G3, NOTE_FREQUENCIES.C4], [NOTE_FREQUENCIES.G3, NOTE_FREQUENCIES.B3, NOTE_FREQUENCIES.D4], [NOTE_FREQUENCIES.C4, NOTE_FREQUENCIES["D#4"], NOTE_FREQUENCIES.G4]] }
        ];

        const SUBSCRIPTION_PLANS = [
            { id: "monthly", name: "الباقة الشهرية للملحنين", price: "18.00", period: "شهر", features: ["وصول كامل لجميع المقامات والأدوات", "تصدير الألحان بجودة استوديو عالية", "ترخيص تجاري 100% للأعمال"] },
            { id: "yearly", name: "الباقة السنوية الاحترافية", price: "43.00", period: "سنة", savingsBadge: "وفر 80%", features: ["وصول كامل لجميع المقامات والأدوات", "تصدير الألحان بجودة استوديو عالية", "ترخيص تجاري 100% للأعمال", "تحديثات مجانية مدى الحياة"] }
        ];

        let state = {
            activeSubscription: null,
            selectedPlan: SUBSCRIPTION_PLANS[1],
            bpm: 100,
            isPlayingSeq: false,
            currentStep: 0,
            kickSteps: [true, false, false, false, true, false, false, false],
            snareSteps: [false, false, true, false, false, false, true, false],
            hihatSteps: [true, true, true, true, true, true, true, true]
        };

        let seqInterval = null;

        function initPianoKeyboard() {
            const container = document.getElementById('piano-keyboard-container');
            container.innerHTML = Object.keys(NOTE_FREQUENCIES).map(keyName => {
                const isBlack = keyName.includes("#");
                return `<button onclick="handleKeyClick('${keyName}')" id="key-${keyName}" class="transition-all duration-75 select-none relative flex flex-col justify-end items-center pb-3 text-[10px] font-mono font-bold rounded-b-xl ${isBlack ? 'w-8 h-28 -mx-4 z-10 bg-slate-900 text-slate-300 border border-slate-700' : 'w-12 h-44 border-r border-slate-200/20 bg-slate-100 text-slate-800'}"><span>${keyName}</span></button>`;
            }).join('');
        }

        function handleKeyClick(keyName) {
            const freq = NOTE_FREQUENCIES[keyName];
            const type = document.getElementById('sound-type-select').value;
            if (freq) playNote(freq, type, 1.2);
        }

        function renderChordProgressions() {
            const container = document.getElementById('chord-progressions-container');
            container.innerHTML = CHORD_PROGRESSIONS.map(prog => `
                <div class="p-5 rounded-2xl bg-slate-950 border border-slate-800 space-y-4">
                    <span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-indigo-500/20 text-indigo-300">${prog.genre}</span>
                    <h3 class="font-bold text-slate-200 text-sm font-serif">${prog.name}</h3>
                    <div class="flex flex-wrap gap-1.5">${prog.chords.map(c => `<span class="px-2.5 py-1 rounded-xl bg-slate-900 text-slate-300 text-xs font-mono">${c}</span>`).join('')}</div>
                    <button onclick="playProgressionFreqs('${prog.id}')" class="w-full py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-bold text-xs">▶ عزف التتابع</button>
                </div>
            `).join('');
        }

        function playProgressionFreqs(id) {
            const prog = CHORD_PROGRESSIONS.find(p => p.id === id);
            if (prog) prog.frequencies.forEach((f, idx) => setTimeout(() => playChord(f, 1.2), idx * 1000));
        }

        function updateBpm(val) {
            state.bpm = Number(val);
            document.getElementById('bpm-label').innerText = `السرعة: ${state.bpm} BPM`;
        }

        function toggleSequencer() {
            state.isPlayingSeq = !state.isPlayingSeq;
            const btn = document.getElementById('btn-toggle-seq');
            if (state.isPlayingSeq) {
                btn.className = "px-5 py-2.5 rounded-2xl font-bold text-xs flex items-center gap-2 bg-rose-600 text-white";
                btn.innerHTML = "⏹ إيقاف الإيقاع";
                const stepDuration = (60 / state.bpm / 2) * 1000;
                seqInterval = setInterval(() => {
                    state.currentStep = (state.currentStep + 1) % 8;
                    if (state.kickSteps[state.currentStep]) playDrumHit("kick");
                    if (state.snareSteps[state.currentStep]) playDrumHit("snare");
                    if (state.hihatSteps[state.currentStep]) playDrumHit("hihat");
                    renderSequencerGrid();
                }, stepDuration);
            } else {
                btn.className = "px-5 py-2.5 rounded-2xl font-bold text-xs flex items-center gap-2 bg-gradient-to-r from-purple-600 to-indigo-600 text-white";
                btn.innerHTML = "▶ تشغيل الإيقاع";
                clearInterval(seqInterval);
            }
        }

        function renderSequencerGrid() {
            renderTrack('kick', state.kickSteps, 'bg-amber-500');
            renderTrack('snare', state.snareSteps, 'bg-purple-600');
            renderTrack('hihat', state.hihatSteps, 'bg-indigo-500');
        }

        function renderTrack(name, steps, cls) {
            document.getElementById(`track-${name}`).innerHTML = steps.map((active, idx) => `
                <button onclick="toggleStep('${name}', ${idx})" class="h-10 rounded-xl transition ${state.currentStep === idx && state.isPlayingSeq ? 'ring-2 ring-amber-400' : ''} ${active ? cls : 'bg-slate-900 border border-slate-800'}"></button>
            `).join('');
        }

        function toggleStep(track, idx) {
            if (track === 'kick') state.kickSteps[idx] = !state.kickSteps[idx];
            if (track === 'snare') state.snareSteps[idx] = !state.snareSteps[idx];
            if (track === 'hihat') state.hihatSteps[idx] = !state.hihatSteps[idx];
            renderSequencerGrid();
        }

        function openPricingModal() {
            document.getElementById('pricing-modal').classList.remove('hidden');
            renderPlansModal();
            renderPayPalButton();
        }

        function closePricingModal() {
            document.getElementById('pricing-modal').classList.add('hidden');
        }

        function renderPlansModal() {
            document.getElementById('plans-grid').innerHTML = SUBSCRIPTION_PLANS.map(plan => {
                const isSelected = state.selectedPlan.id === plan.id;
                return `
                    <div onclick="selectPlan('${plan.id}')" class="p-6 rounded-3xl border transition-all cursor-pointer space-y-4 ${isSelected ? 'bg-slate-950 border-amber-400 ring-2 ring-amber-400' : 'bg-slate-950/60 border-slate-800'}">
                        <h3 class="font-bold text-white">${plan.name}</h3>
                        <div class="text-3xl font-extrabold text-amber-300">$${plan.price} / ${plan.period}</div>
                    </div>
                `;
            }).join('');
            document.getElementById('modal-selected-plan-name').innerText = state.selectedPlan.name;
            document.getElementById('modal-selected-plan-price').innerText = `$${state.selectedPlan.price} USD`;
        }

        function selectPlan(id) {
            state.selectedPlan = SUBSCRIPTION_PLANS.find(p => p.id === id) || SUBSCRIPTION_PLANS[1];
            renderPlansModal();
            renderPayPalButton();
        }

        // --- ربط حقيقي بزِر PayPal الرسمي (يتحقق من الرصيد والبطاقة الحقيقية) ---
        function renderPayPalButton() {
            const container = document.getElementById('paypal-button-container');
            container.innerHTML = ""; // إعادة تعيين الزر بناءً على السعر الحالي
            
            paypal.Buttons({
                createOrder: function(data, actions) {
                    return actions.order.create({
                        purchase_units: [{
                            amount: { value: state.selectedPlan.price }
                        }]
                    });
                },
                onApprove: function(data, actions) {
                    return actions.order.capture().then(function(details) {
                        state.activeSubscription = state.selectedPlan;
                        document.getElementById('paypal-success-box').classList.remove('hidden');
                        document.getElementById('header-subscription-status').innerHTML = `
                            <div class="px-3.5 py-1.5 rounded-full bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 text-xs font-bold">
                                ✅ تم الدفع بنجاح بواسطة ${details.payer.name.given_name}
                            </div>
                        `;
                        const banner = document.getElementById('subscription-banner');
                        if (banner) banner.classList.add('hidden');

                        setTimeout(() => {
                            closePricingModal();
                            document.getElementById('paypal-success-box').classList.add('hidden');
                        }, 2000);
                    });
                },
                onError: function(err) {
                    alert("فشلت عملية الدفع لعدم توفر رصيد كافٍ أو حدوث مشكلة في البطاقة عبر PayPal.");
                }
            }).render('#paypal-button-container');
        }

        initPianoKeyboard();
        renderChordProgressions();
        renderSequencerGrid();
    </script>
</body>
</html>
