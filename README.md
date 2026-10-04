<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>متابعة مراجعة القرآن الكريم الأسبوعية</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Tajawal', 'sans-serif'],
          },
          colors: {
            emerald: {
              50: '#ecfdf5',
              100: '#d1fae5',
              500: '#10b981',
              600: '#059669',
              700: '#047857',
            }
          }
        }
      }
    }
  </script>
  <style>
    body {
      font-family: 'Tajawal', sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }
  </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 pb-12 antialiased selection:bg-emerald-500 selection:text-white">

  <header class="sticky top-0 z-20 bg-white/80 backdrop-blur-md border-b border-slate-200/80 px-4 py-4 mb-4 shadow-sm">
    <div class="max-w-md mx-auto flex items-center justify-between">
      <div>
        <h1 class="text-xl font-extrabold text-slate-900 tracking-tight">جدول المراجعة الأسبوعي</h1>
        <p class="text-xs text-slate-500 font-medium mt-0.5">متابعة الـ 15 جزءاً المحددة</p>
      </div>
      <div id="overall-badge" class="bg-emerald-50 text-emerald-700 text-xs font-bold px-3 py-1.5 rounded-full border border-emerald-200 flex items-center gap-1">
        <span id="overall-percentage">0%</span>
        <span>مكتمل</span>
      </div>
    </div>
  </header>

  <main class="max-w-md mx-auto px-4">
    <!-- Weekly Timer & Control Banner -->
    <div class="bg-gradient-to-br from-slate-900 to-slate-800 text-white p-4 rounded-2xl shadow-lg mb-4 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 rounded-xl bg-white/10 flex items-center justify-center text-emerald-400">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
          </svg>
        </div>
        <div>
          <div class="text-[11px] text-slate-300 font-medium">متبقي على إعادة الضبط الأسبوعي</div>
          <div id="reset-countdown" class="text-sm font-bold text-white mt-0.5">7 أيام</div>
        </div>
      </div>
      <button id="manual-reset-btn" class="bg-white/10 hover:bg-white/20 active:scale-95 text-xs font-bold text-slate-200 px-3 py-2 rounded-xl transition-all border border-white/10 flex items-center gap-1.5">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
        </svg>
        إعادة ضبط الآن
      </button>
    </div>

    <!-- Overall Progress Card -->
    <div class="bg-white p-4 rounded-2xl border border-slate-200/80 shadow-sm mb-5">
      <div class="flex justify-between items-center text-xs font-bold text-slate-600 mb-2">
        <span>الإنجاز الأسبوعي الكلي</span>
        <span id="overall-count" class="text-emerald-600">0 من 15 جزءاً</span>
      </div>
      <div class="w-full bg-slate-100 rounded-full h-3 overflow-hidden">
        <div id="overall-progress-bar" class="bg-gradient-to-r from-emerald-500 to-teal-500 h-full rounded-full transition-all duration-500 ease-out" style="width: 0%"></div>
      </div>
    </div>

    <!-- Cards Grid -->
    <div id="juz-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-3.5">
      <!-- Dynamic Juz cards -->
    </div>
  </main>

  <div id="surah-modal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-end sm:items-center justify-center opacity-0 pointer-events-none transition-opacity duration-300">
    <div id="modal-content" class="bg-white w-full max-w-md rounded-t-3xl sm:rounded-3xl max-h-[85vh] flex flex-col shadow-2xl transform translate-y-full sm:translate-y-0 sm:scale-95 transition-transform duration-300">
      
      <!-- Modal Header -->
      <div class="p-5 border-b border-slate-100 flex items-center justify-between bg-slate-50/50 rounded-t-3xl">
        <div>
          <h2 id="modal-title" class="text-lg font-bold text-slate-900">الجزء</h2>
          <p class="text-xs text-slate-500 mt-0.5">حدد السور التي أتممت مراجعتها هذا الأسبوع</p>
        </div>
        <button id="close-modal-btn" class="p-2 rounded-full text-slate-400 hover:text-slate-600 hover:bg-slate-100 transition-colors">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
          </svg>
        </button>
      </div>

      <!-- Quick Actions -->
      <div class="px-5 py-3 border-b border-slate-100 bg-white flex items-center justify-between">
        <span id="modal-progress-text" class="text-xs font-semibold text-slate-500">تم اختيار 0 من 0 سورة</span>
        <button id="toggle-all-btn" class="text-xs font-bold text-emerald-600 hover:text-emerald-700 bg-emerald-50 px-3 py-1.5 rounded-lg border border-emerald-100 active:scale-95 transition-all">
          تحديد الكل
        </button>
      </div>

      <!-- Surah List -->
      <div id="surah-list" class="p-5 overflow-y-auto space-y-2.5 max-h-[50vh] no-scrollbar">
        <!-- Surah items inserted dynamically -->
      </div>

      <!-- Modal Footer -->
      <div class="p-4 border-t border-slate-100 bg-white rounded-b-3xl">
        <button id="save-modal-btn" class="w-full bg-emerald-600 hover:bg-emerald-700 active:bg-emerald-800 text-white font-bold py-3 px-4 rounded-xl shadow-lg shadow-emerald-600/20 transition-all">
          حفظ وإغلاق
        </button>
      </div>

    </div>
  </div>

  <div id="confirm-modal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 opacity-0 pointer-events-none transition-opacity duration-300">
    <div class="bg-white w-full max-w-sm rounded-2xl p-5 shadow-2xl text-center space-y-4">
      <div class="w-12 h-12 rounded-full bg-amber-100 text-amber-600 mx-auto flex items-center justify-center">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path>
        </svg>
      </div>
      <div>
        <h3 class="text-base font-bold text-slate-900">إعادة ضبط الإنجاز؟</h3>
        <p class="text-xs text-slate-500 mt-1">سيتم مسح جميع العلامات الخضراء وبدء أسبوع جديد.</p>
      </div>
      <div class="flex gap-2">
        <button id="cancel-reset-btn" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold py-2.5 rounded-xl text-xs transition-colors">إلغاء</button>
        <button id="confirm-reset-btn" class="flex-1 bg-red-600 hover:bg-red-700 text-white font-bold py-2.5 rounded-xl text-xs transition-colors shadow-md shadow-red-600/20">تأكيد المسح</button>
      </div>
    </div>
  </div>

  <script>
    const ONE_WEEK_MS = 7 * 24 * 60 * 60 * 1000;

    const juzList = [
      { id: 1, range: "من الآية 1 - سورة الفاتحة<br>إلى الآية 141 - سورة البقرة", surahs: ["الفاتحة", "البقرة"] },
      { id: 2, range: "من الآية 142 - سورة البقرة<br>إلى الآية 252 - سورة البقرة", surahs: ["البقرة"] },
      { id: 3, range: "من الآية 253 - سورة البقرة<br>إلى الآية 92 - سورة آل عمران", surahs: ["البقرة", "آل عمران"] },
      { id: 4, range: "من الآية 93 - سورة آل عمران<br>إلى الآية 23 - سورة النساء", surahs: ["آل عمران", "النساء"] },
      { id: 5, range: "من الآية 24 - سورة النساء<br>إلى الآية 147 - سورة النساء", surahs: ["النساء"] },
      { id: 6, range: "من الآية 148 - سورة النساء<br>إلى الآية 81 - سورة المائدة", surahs: ["النساء", "المائدة"] },
      { id: 21, range: "من الآية 46 - سورة العنكبوت<br>إلى الآية 30 - سورة الأحزاب", surahs: ["العنكبوت", "الروم", "لقمان", "السجدة", "الأحزاب"] },
      { id: 22, range: "من الآية 31 - سورة الأحزاب<br>إلى الآية 27 - سورة يس", surahs: ["الأحزاب", "سبأ", "فاطر", "يس"] },
      { id: 23, range: "من الآية 28 - سورة يس<br>إلى الآية 31 - سورة الزمر", surahs: ["يس", "الصافات", "ص", "الزمر"] },
      { id: 24, range: "من الآية 32 - سورة الزمر<br>إلى الآية 46 - سورة فصلت", surahs: ["الزمر", "غافر", "فصلت"] },
      { id: 25, range: "من الآية 47 - سورة فصلت<br>إلى الآية 37 - سورة الجاثية", surahs: ["فصلت", "الشورى", "الزخرف", "الدخان", "الجاثية"] },
      { id: 26, range: "من الآية 1 - سورة الأحقاف<br>إلى الآية 30 - سورة الذاريات", surahs: ["الأحقاف", "محمد", "الفتح", "الحجرات", "ق", "الذاريات"] },
      { id: 27, range: "من الآية 31 - سورة الذاريات<br>إلى الآية 29 - سورة الحديد", surahs: ["الذاريات", "الطور", "النجم", "القمر", "الرحمن", "الواقعة", "الحديد"] },
      { id: 28, range: "من الآية 1 - سورة المجادلة<br>إلى الآية 12 - سورة التحريم", surahs: ["المجادلة", "الحشر", "الممتحنة", "الصف", "الجمعة", "المنافقون", "التغابن", "الطلاق", "التحريم"] },
      { id: 29, range: "من الآية 1 - سورة الملك<br>إلى الآية 50 - سورة المرسلات", surahs: ["الملك", "القلم", "الحاقة", "المعارج", "نوح", "الجن", "المزمل", "المدثر", "القيامة", "الإنسان", "المرسلات"] },
      { id: 30, range: "من الآية 1 - سورة النبأ<br>إلى الآية 6 - سورة الناس", surahs: ["النبأ", "النازعات", "عبس", "التكوير", "الانفطار", "المطففين", "الانشقاق", "البروج", "الطارق", "الأعلى", "الغاشية", "الفجر", "البلد", "الشمس", "الليل", "الضحى", "الشرح", "التين", "العلق", "القدر", "البينة", "الزلزلة", "العاديات", "القارعة", "التكاثر", "العصر", "الهمزة", "الفيل", "قريش", "الماعون", "الكوثر", "الكافرون", "النصر", "المسد", "الإخلاص", "الفلق", "الناس"] }
    ];

    let userState = JSON.parse(localStorage.getItem('quran_revision_state')) || {};
    let lastResetTimestamp = parseInt(localStorage.getItem('quran_last_reset_time')) || Date.now();
    let activeJuzId = null;

    // Verify & Auto-Reset if a week has passed
    function checkWeeklyAutoReset() {
      const now = Date.now();
      if (now - lastResetTimestamp >= ONE_WEEK_MS) {
        performReset();
      } else {
        updateCountdownDisplay();
      }
    }

    function performReset() {
      userState = {};
      juzList.forEach(juz => { userState[juz.id] = {}; });
      lastResetTimestamp = Date.now();
      localStorage.setItem('quran_last_reset_time', lastResetTimestamp.toString());
      saveState();
      renderGrid();
      updateCountdownDisplay();
    }

    function updateCountdownDisplay() {
      const now = Date.now();
      const elapsed = now - lastResetTimestamp;
      const remainingMs = Math.max(0, ONE_WEEK_MS - elapsed);

      const days = Math.floor(remainingMs / (1000 * 60 * 60 * 24));
      const hours = Math.floor((remainingMs % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

      const text = days > 0 ? `${days} أيام و ${hours} ساعة` : `${hours} ساعة`;
      document.getElementById('reset-countdown').textContent = text;
    }

    // Ensure state schema is set up
    juzList.forEach(juz => {
      if (!userState[juz.id]) {
        userState[juz.id] = {};
      }
    });

    function saveState() {
      localStorage.setItem('quran_revision_state', JSON.stringify(userState));
      updateOverallProgress();
    }

    function renderGrid() {
      const grid = document.getElementById('juz-grid');
      grid.innerHTML = '';

      juzList.forEach(juz => {
        const completedSurahs = Object.values(userState[juz.id] || {}).filter(Boolean).length;
        const totalSurahs = juz.surahs.length;
        const percentage = Math.round((completedSurahs / totalSurahs) * 100) || 0;
        const isComplete = percentage === 100;
        const isPartial = percentage > 0 && percentage < 100;

        const card = document.createElement('div');
        card.id = `juz-card-${juz.id}`;
        card.className = `p-4 rounded-2xl border transition-all duration-300 cursor-pointer relative overflow-hidden select-none active:scale-[0.98] ${
          isComplete 
            ? 'bg-gradient-to-br from-emerald-500 to-emerald-600 text-white border-emerald-600 shadow-md shadow-emerald-500/20' 
            : isPartial
            ? 'bg-emerald-50/50 text-slate-800 border-emerald-300 shadow-sm'
            : 'bg-white text-slate-800 border-slate-200/80 hover:border-emerald-300 shadow-sm'
        }`;

        card.innerHTML = `
          <div class="flex justify-between items-start mb-2">
            <span class="font-extrabold text-base ${isComplete ? 'text-white' : 'text-slate-900'}">الجزء ${juz.id}</span>
            <span class="text-xs font-bold px-2 py-0.5 rounded-full ${
              isComplete ? 'bg-white/20 text-white' : isPartial ? 'bg-emerald-100 text-emerald-700' : 'bg-slate-100 text-slate-600'
            }">${percentage}%</span>
          </div>

          <p class="text-xs leading-relaxed mb-3 ${isComplete ? 'text-emerald-50' : 'text-slate-500'}">
            ${juz.range}
          </p>

          <div class="space-y-1">
            <div class="flex justify-between items-center text-[11px] font-medium ${isComplete ? 'text-emerald-100' : 'text-slate-400'}">
              <span>السور: ${completedSurahs}/${totalSurahs}</span>
            </div>
            <div class="w-full ${isComplete ? 'bg-black/10' : 'bg-slate-100'} rounded-full h-1.5 overflow-hidden">
              <div class="${isComplete ? 'bg-white' : 'bg-emerald-500'} h-full rounded-full transition-all duration-300" style="width: ${percentage}%"></div>
            </div>
          </div>
        `;

        card.addEventListener('click', () => openModal(juz));
        grid.appendChild(card);
      });

      updateOverallProgress();
    }

    function updateOverallProgress() {
      let totalPartsCompleted = 0;

      juzList.forEach(juz => {
        const completedSurahs = Object.values(userState[juz.id] || {}).filter(Boolean).length;
        if (completedSurahs === juz.surahs.length && juz.surahs.length > 0) {
          totalPartsCompleted++;
        }
      });

      const overallPercent = Math.round((totalPartsCompleted / juzList.length) * 100);
      document.getElementById('overall-percentage').textContent = `${overallPercent}%`;
      document.getElementById('overall-count').textContent = `${totalPartsCompleted} من ${juzList.length} جزءاً`;
      document.getElementById('overall-progress-bar').style.width = `${overallPercent}%`;
    }

    const modal = document.getElementById('surah-modal');
    const modalContent = document.getElementById('modal-content');

    function openModal(juz) {
      activeJuzId = juz.id;
      document.getElementById('modal-title').textContent = `الجزء ${juz.id}`;
      
      renderSurahList(juz);

      modal.classList.remove('opacity-0', 'pointer-events-none');
      modalContent.classList.remove('translate-y-full', 'sm:scale-95');
      modalContent.classList.add('translate-y-0', 'sm:scale-100');
    }

    function closeModal() {
      modalContent.classList.remove('translate-y-0', 'sm:scale-100');
      modalContent.classList.add('translate-y-full', 'sm:scale-95');
      modal.classList.add('opacity-0', 'pointer-events-none');
      
      activeJuzId = null;
      renderGrid();
    }

    function renderSurahList(juz) {
      const listContainer = document.getElementById('surah-list');
      listContainer.innerHTML = '';

      const juzState = userState[juz.id] || {};

      juz.surahs.forEach(surah => {
        const isChecked = !!juzState[surah];

        const item = document.createElement('div');
        item.className = `p-3 rounded-xl border flex items-center justify-between cursor-pointer transition-all select-none ${
          isChecked 
            ? 'bg-emerald-50/80 border-emerald-300 text-emerald-900 font-bold' 
            : 'bg-white border-slate-200 text-slate-700 hover:border-slate-300 font-medium'
        }`;

        item.innerHTML = `
          <span class="text-sm">سورة ${surah}</span>
          <div class="w-6 h-6 rounded-lg flex items-center justify-center transition-colors ${
            isChecked ? 'bg-emerald-600 text-white' : 'border-2 border-slate-300 bg-white'
          }">
            ${isChecked ? '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7"></path></svg>' : ''}
          </div>
        `;

        item.addEventListener('click', () => {
          userState[juz.id][surah] = !userState[juz.id][surah];
          saveState();
          renderSurahList(juz);
        });

        listContainer.appendChild(item);
      });

      const completedCount = Object.values(userState[juz.id] || {}).filter(Boolean).length;
      document.getElementById('modal-progress-text').textContent = `تم اختيار ${completedCount} من ${juz.surahs.length} سورة`;

      const toggleAllBtn = document.getElementById('toggle-all-btn');
      const allSelected = completedCount === juz.surahs.length;
      toggleAllBtn.textContent = allSelected ? 'إلغاء تحديد الكل' : 'تحديد الكل';
    }

    // Modal Control Listeners
    document.getElementById('close-modal-btn').addEventListener('click', closeModal);
    document.getElementById('save-modal-btn').addEventListener('click', closeModal);
    modal.addEventListener('click', (e) => {
      if (e.target === modal) closeModal();
    });

    // Toggle All Button Listener
    document.getElementById('toggle-all-btn').addEventListener('click', () => {
      if (!activeJuzId) return;
      const juz = juzList.find(j => j.id === activeJuzId);
      if (!juz) return;

      const completedCount = Object.values(userState[juz.id] || {}).filter(Boolean).length;
      const shouldSelectAll = completedCount < juz.surahs.length;

      juz.surahs.forEach(surah => {
        userState[juz.id][surah] = shouldSelectAll;
      });

      saveState();
      renderSurahList(juz);
    });

    // Manual Reset Confirm Modal
    const confirmModal = document.getElementById('confirm-modal');
    document.getElementById('manual-reset-btn').addEventListener('click', () => {
      confirmModal.classList.remove('opacity-0', 'pointer-events-none');
    });

    document.getElementById('cancel-reset-btn').addEventListener('click', () => {
      confirmModal.classList.add('opacity-0', 'pointer-events-none');
    });

    document.getElementById('confirm-reset-btn').addEventListener('click', () => {
      confirmModal.classList.add('opacity-0', 'pointer-events-none');
      performReset();
    });

    // Initialize application state & timers
    checkWeeklyAutoReset();
    renderGrid();
    setInterval(updateCountdownDisplay, 60000); // Update countdown every minute
  </script>
</body>
</html>
