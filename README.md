<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Wean Diet - Modern Guide</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <!-- Google Fonts: Inter for UI -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Mukta+Malar:wght@400;500;600;700&display=swap" rel="stylesheet">

    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        tamil: ['Mukta Malar', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        },
                        slate: {
                            50: '#f8fafc', // Main BG
                            100: '#f1f5f9',
                            200: '#e2e8f0',
                            800: '#1e293b',
                            900: '#0f172a',
                        }
                    },
                    boxShadow: {
                        'soft': '0 4px 20px -2px rgba(0, 0, 0, 0.05)',
                        'nav': '0 -4px 20px -2px rgba(0, 0, 0, 0.03)',
                        'float': '0 8px 30px -4px rgba(37, 99, 235, 0.3)',
                    },
                    animation: {
                        'slide-up-fade': 'slideUpFade 0.4s cubic-bezier(0.16, 1, 0.3, 1)',
                        'scale-in': 'scaleIn 0.2s ease-out',
                        'modal-slide': 'modalSlide 0.4s cubic-bezier(0.16, 1, 0.3, 1)',
                    },
                    keyframes: {
                        slideUpFade: {
                            '0%': { transform: 'translateY(15px)', opacity: '0' },
                            '100%': { transform: 'translateY(0)', opacity: '1' },
                        },
                        scaleIn: {
                            '0%': { transform: 'scale(0.95)', opacity: '0' },
                            '100%': { transform: 'scale(1)', opacity: '1' },
                        },
                        modalSlide: {
                            '0%': { transform: 'translateY(100%)' },
                            '100%': { transform: 'translateY(0)' },
                        }
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #f8fafc; /* slate-50 */
            -webkit-tap-highlight-color: transparent;
            overscroll-behavior-y: none; /* Prevent bounce on pull-to-refresh style */
        }
        
        /* Hide Scrollbar but allow scrolling */
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* Safe Area Insets for Mobile */
        .safe-pb { padding-bottom: calc(90px + env(safe-area-inset-bottom)); }
        .safe-pt { padding-top: calc(12px + env(safe-area-inset-top)); }

        /* Floating Center Button Logic */
        .nav-center-btn {
            transform: translateY(-20px);
            box-shadow: 0 8px 25px -5px rgba(37, 99, 235, 0.4);
            border: 4px solid #ffffff;
        }

        /* Glassmorphism Header */
        .glass-header {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }

        /* Card Hover/Active States */
        .touch-card {
            transition: transform 0.15s ease, background-color 0.2s ease;
        }
        .touch-card:active {
            transform: scale(0.97);
            background-color: #f1f5f9; /* slate-100 */
        }

        /* Bottom Sheet Modal */
        .modal-sheet {
            max-height: 92vh;
            border-radius: 28px 28px 0 0;
            box-shadow: 0 -10px 40px rgba(0,0,0,0.1);
        }

        .guide-tab-active {
            background-color: #ffffff;
            color: #2563eb;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }
    </style>
</head>
<body class="text-slate-800 h-screen w-full flex justify-center bg-slate-100 font-sans">

    <!-- App Container -->
    <div class="w-full h-full max-w-md bg-slate-50 relative flex flex-col shadow-2xl sm:rounded-[30px] sm:h-[95vh] sm:my-auto sm:border-[6px] sm:border-slate-800 overflow-hidden">
        
        <!-- Fixed Header -->
        <header class="glass-header absolute top-0 w-full z-40 px-6 py-4 flex justify-between items-center border-b border-slate-100/50 safe-pt transition-all duration-300">
            <div class="flex flex-col">
                <span id="header-eyebrow" class="text-[10px] uppercase tracking-wider font-bold text-slate-400 mb-0.5">Welcome Back</span>
                <h1 id="header-title" class="text-xl font-bold text-slate-900 leading-tight">Home</h1>
            </div>
            
            <div class="flex items-center gap-3">
                <button onclick="toggleLanguage()" id="lang-btn" class="h-9 px-3 rounded-full bg-slate-100 border border-slate-200 text-xs font-bold text-slate-600 active:scale-95 transition-transform">
                    TA
                </button>
                <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-blue-500 to-indigo-600 p-[2px] shadow-md">
                    <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Baby&backgroundColor=c0aede" class="w-full h-full rounded-full bg-white" alt="Profile">
                </div>
            </div>
        </header>

        <!-- Main Scrollable Content -->
        <main id="main-content" class="flex-1 overflow-y-auto hide-scrollbar pt-24 safe-pb px-5 relative">
            <!-- Dynamic Views Injected Here -->
        </main>

        <!-- Bottom Navigation -->
        <nav class="absolute bottom-0 w-full bg-white border-t border-slate-100 z-50 shadow-nav pb-[env(safe-area-inset-bottom)]">
            <div class="flex justify-between items-end px-4 h-[70px] pb-3 relative">
                
                <button onclick="switchTab('home')" class="nav-btn flex-1 flex flex-col items-center gap-1 group" data-tab="home">
                    <i data-lucide="home" class="w-6 h-6 text-slate-400 group-[.active]:text-brand-600 transition-colors"></i>
                    <span class="text-[10px] font-medium text-slate-400 group-[.active]:text-brand-600 transition-colors">Home</span>
                </button>

                <button onclick="switchTab('guide')" class="nav-btn flex-1 flex flex-col items-center gap-1 group" data-tab="guide">
                    <i data-lucide="book-open" class="w-6 h-6 text-slate-400 group-[.active]:text-brand-600 transition-colors"></i>
                    <span class="text-[10px] font-medium text-slate-400 group-[.active]:text-brand-600 transition-colors">Guide</span>
                </button>

                <!-- Center FAB -->
                <div class="w-16 relative flex justify-center z-10">
                    <button onclick="switchTab('tracker')" class="nav-btn nav-center-btn absolute bottom-3 w-14 h-14 bg-brand-600 rounded-2xl flex items-center justify-center text-white active:scale-90 transition-transform" data-tab="tracker">
                        <i data-lucide="plus" class="w-7 h-7"></i>
                    </button>
                    <!-- Label for screen readers/visual clarity if needed, mostly hidden by design -->
                    <span class="text-[10px] font-medium text-brand-600 absolute bottom-0 opacity-0 transition-opacity">Track</span>
                </div>

                <button onclick="switchTab('diet')" class="nav-btn flex-1 flex flex-col items-center gap-1 group" data-tab="diet">
                    <i data-lucide="utensils" class="w-6 h-6 text-slate-400 group-[.active]:text-brand-600 transition-colors"></i>
                    <span class="text-[10px] font-medium text-slate-400 group-[.active]:text-brand-600 transition-colors">Recipes</span>
                </button>

                <button onclick="switchTab('chat')" class="nav-btn flex-1 flex flex-col items-center gap-1 group" data-tab="chat">
                    <i data-lucide="message-circle" class="w-6 h-6 text-slate-400 group-[.active]:text-brand-600 transition-colors"></i>
                    <span class="text-[10px] font-medium text-slate-400 group-[.active]:text-brand-600 transition-colors">Chat</span>
                </button>

            </div>
        </nav>

        <!-- Bottom Sheet Modal (Recipes & Details) -->
        <div id="modal-overlay" class="absolute inset-0 bg-slate-900/40 backdrop-blur-sm z-[60] hidden opacity-0 transition-opacity duration-300" onclick="closeModal()">
            <div onclick="event.stopPropagation()" class="absolute bottom-0 w-full bg-white modal-sheet transform translate-y-full transition-transform duration-300 flex flex-col">
                <!-- Drag Handle -->
                <div class="w-full flex justify-center pt-3 pb-1" onclick="closeModal()">
                    <div class="w-12 h-1.5 bg-slate-200 rounded-full"></div>
                </div>
                
                <!-- Modal Content -->
                <div id="modal-content" class="flex-1 overflow-y-auto p-6 pb-12">
                    <!-- Dynamic Content -->
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- DATA STORE ---
        const STORE = {
            lang: 'en',
            translations: {
                en: {
                    home_title: "Dashboard",
                    guide_title: "Care Guide",
                    tracker_title: "Daily Tracker",
                    diet_title: "Recipes",
                    chat_title: "Expert Chat",
                    welcome_card_title: "Start Weaning Journey",
                    welcome_card_desc: "Your complete guide to introducing solid foods safely at 6 months.",
                    action_track: "Log Meal",
                    action_quiz: "Take Quiz",
                    section_popular: "Popular Today",
                    section_recent: "Recent Activity",
                    tab_start: "Start", tab_foods: "Foods", tab_schedule: "Routine", tab_care: "Care",
                    btn_save: "Save Entry",
                    empty_state: "No logs yet today.",
                    recipes: [
                        { title: "Ragi Porridge", subtitle: "Iron-rich first food", icon: "🥣", time: "15m", calories: "120 kcal" },
                        { title: "Mashed Banana", subtitle: "Energy & Potassium", icon: "🍌", time: "2m", calories: "90 kcal" },
                        { title: "Dal Water", subtitle: "Protein starter", icon: "🍲", time: "20m", calories: "50 kcal" },
                        { title: "Apple Puree", subtitle: "Easy digestion", icon: "🍎", time: "10m", calories: "60 kcal" }
                    ]
                },
                ta: {
                    home_title: "முகப்பு",
                    guide_title: "வழிகாட்டி",
                    tracker_title: "பதிவு",
                    diet_title: "சமையல்",
                    chat_title: "உதவி",
                    welcome_card_title: "இணை உணவு பயணம்",
                    welcome_card_desc: "6 மாத குழந்தைகளுக்கான முழுமையான உணவு வழிகாட்டி.",
                    action_track: "உணவு பதிவு",
                    action_quiz: "வினாடி வினா",
                    section_popular: "பிரபலமானவை",
                    section_recent: "சமீபத்தியவை",
                    tab_start: "துவக்கம்", tab_foods: "உணவு", tab_schedule: "நேரம்", tab_care: "பராமரிப்பு",
                    btn_save: "சேமிக்கவும்",
                    empty_state: "பதிவுகள் இல்லை.",
                    recipes: [
                        { title: "ராகி கூழ்", subtitle: "இரும்புச்சத்து நிறைந்தது", icon: "🥣", time: "15m", calories: "120 kcal" },
                        { title: "வாழைப்பழ மசியல்", subtitle: "ஆற்றல் நிறைந்தது", icon: "🍌", time: "2m", calories: "90 kcal" },
                        { title: "பருப்பு தண்ணீர்", subtitle: "புரதம்", icon: "🍲", time: "20m", calories: "50 kcal" },
                        { title: "ஆப்பிள் கூழ்", subtitle: "எளிதில் ஜீரணமாகும்", icon: "🍎", time: "10m", calories: "60 kcal" }
                    ]
                }
            },
            // Using same recipe structure as before but simplified for demo
            recipeDetails: [
                 {
                    id: 0,
                    title_en: "Ragi Koozh", title_ta: "ராகி கூழ்",
                    desc_en: "High Calcium, Iron, and Fiber. Best for 6+ Months.", desc_ta: "கால்சியம், இரும்புச்சத்து மற்றும் நார்ச்சத்து நிறைந்தது.",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["2 tbsp Sprouted Ragi Flour", "1 cup Water", "1/2 tsp Ghee"],
                    ing_ta: ["2 மேசைக்கரண்டி ராகி மாவு", "1 கப் தண்ணீர்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Mix ragi flour with water to make paste.", "Cook on low flame until thick.", "Add ghee and serve lukewarm."],
                    steps_ta: ["ராகி மாவை தண்ணீரில் கரைக்கவும்.", "கெட்டியாகும் வரை காய்ச்சவும்.", "நெய் சேர்த்து பரிமாறவும்."]
                },
                {
                    id: 1,
                    title_en: "Mashed Banana", title_ta: "வாழைப்பழம் மசியல்",
                    desc_en: "Instant energy boost. Good for weight gain.", desc_ta: "உடனடி ஆற்றல். எடை கூட உதவும்.",
                    prep: "2 min", cook: "0 min",
                    ing_en: ["1 Ripe Banana", "1/2 tsp Ghee (Optional)"],
                    ing_ta: ["1 பழுத்த வாழைப்பழம்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Peel the banana.", "Mash well with a fork.", "Serve immediately."],
                    steps_ta: ["வாழைப்பழத்தை உரித்து மசிக்கவும்.", "உடனே பரிமாறவும்."]
                },
                {
                    id: 2,
                    title_en: "Dal Water", title_ta: "பருப்பு தண்ணீர்",
                    desc_en: "Simple protein introduction.", desc_ta: "எளிய புரத உணவு.",
                    prep: "5 min", cook: "20 min",
                    ing_en: ["2 tbsp Moong Dal", "1 cup Water", "Turmeric"],
                    ing_ta: ["2 மேசைக்கரண்டி பாசிப்பருப்பு", "1 கப் தண்ணீர்", "மஞ்சள்"],
                    steps_en: ["Boil dal with turmeric.", "Strain the water.", "Add a drop of ghee."],
                    steps_ta: ["பருப்பை மஞ்சளுடன் வேகவைக்கவும்.", "தண்ணீரை வடிகட்டவும்.", "நெய் சேர்க்கவும்."]
                },
                {
                    id: 3,
                    title_en: "Apple Puree", title_ta: "ஆப்பிள் கூழ்",
                    desc_en: "Sweet and easily digestible.", desc_ta: "இனிப்பான மற்றும் எளிதில் ஜீரணமாகும்.",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["1 Apple", "Water for steaming"],
                    ing_ta: ["1 ஆப்பிள்", "வேகவைக்க தண்ணீர்"],
                    steps_en: ["Peel and chop apple.", "Steam until soft.", "Blend to smooth puree."],
                    steps_ta: ["ஆப்பிளை தோல் சீவி நறுக்கவும்.", "வேகவைத்து கூழாக்கவும்."]
                }
            ],
            logs: []
        };

        // --- CORE FUNCTIONS ---
        function t(key) {
            return STORE.translations[STORE.lang][key] || key;
        }

        function toggleLanguage() {
            STORE.lang = STORE.lang === 'en' ? 'ta' : 'en';
            document.getElementById('lang-btn').innerText = STORE.lang === 'en' ? 'TA' : 'EN';
            document.body.classList.toggle('font-tamil', STORE.lang === 'ta');
            
            // Refresh current view
            const activeBtn = document.querySelector('.nav-btn.active');
            if(activeBtn) switchTab(activeBtn.dataset.tab);
        }

        function switchTab(tabId) {
            // Update Navigation UI
            document.querySelectorAll('.nav-btn').forEach(btn => {
                const label = btn.querySelector('span');
                const icon = btn.querySelector('i') || btn.querySelector('svg');
                const isCenterBtn = btn.classList.contains('nav-center-btn');

                if(btn.dataset.tab === tabId) {
                    btn.classList.add('active');
                    if (label && !isCenterBtn) {
                        label.classList.remove('text-slate-400');
                        label.classList.add('text-brand-600');
                    }
                    if (icon && !isCenterBtn) {
                        icon.classList.remove('text-slate-400');
                        icon.classList.add('text-brand-600');
                    }
                } else {
                    btn.classList.remove('active');
                    if (label && !isCenterBtn) {
                        label.classList.add('text-slate-400');
                        label.classList.remove('text-brand-600');
                    }
                    if (icon && !isCenterBtn) {
                        icon.classList.add('text-slate-400');
                        icon.classList.remove('text-brand-600');
                    }
                }
            });

            // Update Header Title
            const headerTitle = document.getElementById('header-title');
            const headerEye = document.getElementById('header-eyebrow');
            headerTitle.innerText = t(tabId + '_title');
            
            // Contextual Eyebrow Text
            if(tabId === 'home') headerEye.innerText = "Wednesday, 12 Oct";
            else if(tabId === 'tracker') headerEye.innerText = "Log & Monitor";
            else if(tabId === 'diet') headerEye.innerText = "Nutritious Meals";
            else headerEye.innerText = "Wean Diet App";

            // Render View
            const main = document.getElementById('main-content');
            main.innerHTML = ''; // Clear current
            
            // Scroll to top
            main.scrollTop = 0;

            if(tabId === 'home') renderHome(main);
            if(tabId === 'guide') renderGuide(main);
            if(tabId === 'tracker') renderTracker(main);
            if(tabId === 'diet') renderDiet(main);
            if(tabId === 'chat') renderChat(main);

            lucide.createIcons();
        }

        // --- RENDERERS ---

        function renderHome(container) {
            const data = STORE.translations[STORE.lang];
            const html = `
                <div class="space-y-6 animate-slide-up-fade">
                    
                    <!-- Hero Card -->
                    <div class="w-full bg-gradient-to-br from-brand-600 to-indigo-700 rounded-3xl p-6 shadow-float text-white relative overflow-hidden">
                        <div class="absolute -right-10 -top-10 w-40 h-40 bg-white opacity-10 rounded-full blur-2xl"></div>
                        <div class="relative z-10">
                            <span class="inline-block px-2 py-1 bg-white/20 rounded-md text-[10px] font-bold uppercase tracking-wide mb-2">Phase 1: 6-8 Months</span>
                            <h2 class="text-2xl font-bold mb-2 leading-tight">${data.welcome_card_title}</h2>
                            <p class="text-sm text-brand-100 mb-6 leading-relaxed opacity-90">${data.welcome_card_desc}</p>
                            <button onclick="switchTab('guide')" class="bg-white text-brand-600 px-5 py-2.5 rounded-xl text-xs font-bold shadow-sm active:scale-95 transition-transform">Start Reading</button>
                        </div>
                    </div>

                    <!-- Quick Actions Grid -->
                    <div class="grid grid-cols-2 gap-4">
                        <button onclick="switchTab('tracker')" class="touch-card bg-white p-4 rounded-2xl shadow-soft flex items-center gap-3">
                            <div class="w-10 h-10 rounded-full bg-emerald-50 text-emerald-600 flex items-center justify-center">
                                <i data-lucide="plus-circle" class="w-5 h-5"></i>
                            </div>
                            <div class="text-left">
                                <span class="block text-xs font-bold text-slate-800">${data.action_track}</span>
                                <span class="block text-[10px] text-slate-500">Add meal</span>
                            </div>
                        </button>
                        <button onclick="openLink('quiz')" class="touch-card bg-white p-4 rounded-2xl shadow-soft flex items-center gap-3">
                            <div class="w-10 h-10 rounded-full bg-amber-50 text-amber-600 flex items-center justify-center">
                                <i data-lucide="help-circle" class="w-5 h-5"></i>
                            </div>
                            <div class="text-left">
                                <span class="block text-xs font-bold text-slate-800">${data.action_quiz}</span>
                                <span class="block text-[10px] text-slate-500">Check readiness</span>
                            </div>
                        </button>
                    </div>

                    <!-- Popular Section -->
                    <div>
                        <div class="flex justify-between items-center mb-3 px-1">
                            <h3 class="font-bold text-slate-800 text-sm">${data.section_popular}</h3>
                            <button onclick="switchTab('diet')" class="text-xs text-brand-600 font-medium">View All</button>
                        </div>
                        <div class="space-y-3">
                            ${data.recipes.slice(0, 2).map((r, i) => `
                                <div onclick="openRecipeModal(${i})" class="touch-card bg-white p-3 rounded-2xl shadow-soft flex items-center gap-4 cursor-pointer">
                                    <div class="w-14 h-14 bg-slate-50 rounded-xl flex items-center justify-center text-2xl shadow-inner">${r.icon}</div>
                                    <div class="flex-1">
                                        <h4 class="font-bold text-slate-800 text-sm">${r.title}</h4>
                                        <p class="text-[11px] text-slate-500">${r.subtitle}</p>
                                    </div>
                                    <div class="pr-2">
                                        <div class="w-8 h-8 rounded-full bg-slate-50 flex items-center justify-center text-slate-400">
                                            <i data-lucide="chevron-right" class="w-4 h-4"></i>
                                        </div>
                                    </div>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        function renderGuide(container) {
            const t_dat = STORE.translations[STORE.lang];
            container.innerHTML = `
                <div class="animate-slide-up-fade h-full flex flex-col">
                    <!-- Tabs -->
                    <div class="bg-slate-200/50 p-1 rounded-xl flex mb-6 relative">
                        <button onclick="showGuideSection('basics', this)" class="flex-1 py-2 rounded-lg text-xs font-bold text-slate-500 transition-all guide-tab-active" id="default-guide-tab">${t_dat.tab_start}</button>
                        <button onclick="showGuideSection('foods', this)" class="flex-1 py-2 rounded-lg text-xs font-bold text-slate-500 transition-all">${t_dat.tab_foods}</button>
                        <button onclick="showGuideSection('routine', this)" class="flex-1 py-2 rounded-lg text-xs font-bold text-slate-500 transition-all">${t_dat.tab_schedule}</button>
                    </div>

                    <!-- Content Area -->
                    <div id="guide-content" class="flex-1 space-y-4 pb-4">
                        <!-- Basics Content (Default) -->
                        <div class="bg-white p-5 rounded-3xl shadow-soft">
                            <div class="flex items-start gap-4 mb-4">
                                <div class="w-10 h-10 rounded-full bg-blue-50 text-brand-600 flex items-center justify-center shrink-0">
                                    <i data-lucide="baby" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <h3 class="font-bold text-slate-800 mb-1">Why Start at 6 Months?</h3>
                                    <p class="text-xs text-slate-500 leading-relaxed">Breast milk alone is not enough for the growing energy needs. The digestive system is ready for solids.</p>
                                </div>
                            </div>
                            <div class="h-px bg-slate-100 w-full my-4"></div>
                            <div class="flex items-start gap-4">
                                <div class="w-10 h-10 rounded-full bg-green-50 text-green-600 flex items-center justify-center shrink-0">
                                    <i data-lucide="check" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <h3 class="font-bold text-slate-800 mb-1">Signs of Readiness</h3>
                                    <ul class="text-xs text-slate-500 space-y-1 mt-1">
                                        <li>• Steady head control</li>
                                        <li>• Sitting with support</li>
                                        <li>• Lost tongue-thrust reflex</li>
                                    </ul>
                                </div>
                            </div>
                        </div>

                        <div class="bg-indigo-50 p-5 rounded-3xl border border-indigo-100">
                             <h3 class="font-bold text-indigo-900 mb-2 text-sm flex items-center gap-2"><i data-lucide="alert-circle" class="w-4 h-4"></i> Hygiene is Key</h3>
                             <p class="text-xs text-indigo-800/80 leading-relaxed">Always wash hands. Use clean cups and spoons. Avoid feeding bottles as they cause infections.</p>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderTracker(container) {
            const t_dat = STORE.translations[STORE.lang];
            
            // Calendar Generation
            const today = new Date();
            let calendarHTML = '<div class="grid grid-cols-7 gap-2 mb-6">';
            const days = ['S','M','T','W','T','F','S'];
            
            // Header Row
            days.forEach(d => calendarHTML += `<div class="text-center text-[10px] font-bold text-slate-400 uppercase">${d}</div>`);
            
            // Days Row (Mocking current week)
            for(let i=6; i>=0; i--) {
                const date = new Date();
                date.setDate(today.getDate() - i);
                const dayNum = date.getDate();
                const isToday = i === 0;
                
                calendarHTML += `
                    <div class="aspect-square flex flex-col items-center justify-center rounded-xl text-xs font-semibold ${isToday ? 'bg-brand-600 text-white shadow-lg shadow-brand-200' : 'bg-white text-slate-600 border border-slate-100'}">
                        ${dayNum}
                        ${isToday ? '<div class="w-1 h-1 bg-white rounded-full mt-1"></div>' : ''}
                    </div>
                `;
            }
            calendarHTML += '</div>';

            container.innerHTML = `
                <div class="animate-slide-up-fade">
                    <!-- Calendar Strip -->
                    ${calendarHTML}

                    <!-- Stats Card -->
                    <div class="bg-white p-5 rounded-3xl shadow-soft mb-6 relative overflow-hidden">
                        <div class="flex justify-between items-end mb-4 relative z-10">
                            <div>
                                <h3 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-1">Weekly Summary</h3>
                                <p class="text-2xl font-bold text-slate-800">12 Meals</p>
                            </div>
                            <div class="text-right">
                                <span class="text-xs font-bold text-green-500 bg-green-50 px-2 py-1 rounded-lg">+2 from last week</span>
                            </div>
                        </div>
                        <!-- Chart Area -->
                        <div class="h-32 w-full">
                             <canvas id="trackerChart"></canvas>
                        </div>
                    </div>

                    <!-- Input Card -->
                    <div class="bg-white p-5 rounded-3xl shadow-soft">
                        <h3 class="font-bold text-slate-800 mb-4">Log Meal</h3>
                        
                        <div class="space-y-4">
                            <div>
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-2">Food Item</label>
                                <select id="food-select" class="w-full bg-slate-50 border border-slate-200 text-sm font-semibold text-slate-700 rounded-xl px-4 py-3 outline-none focus:ring-2 focus:ring-brand-500 focus:border-transparent transition-all appearance-none">
                                    <option>Ragi Porridge</option>
                                    <option>Mashed Banana</option>
                                    <option>Breast Milk</option>
                                    <option>Formula</option>
                                    <option>Dal Water</option>
                                </select>
                            </div>
                            
                            <div>
                                <label class="block text-[10px] font-bold text-slate-400 uppercase mb-2">Reaction</label>
                                <div class="grid grid-cols-3 gap-3">
                                    <button onclick="selectReaction(this)" class="reaction-btn p-3 rounded-xl border border-slate-200 bg-slate-50 flex flex-col items-center gap-1 hover:bg-emerald-50 hover:border-emerald-200 transition-colors group">
                                        <span class="text-xl group-hover:scale-110 transition-transform">😋</span>
                                        <span class="text-[9px] font-bold text-slate-400 group-hover:text-emerald-600">Loved</span>
                                    </button>
                                    <button onclick="selectReaction(this)" class="reaction-btn p-3 rounded-xl border border-slate-200 bg-slate-50 flex flex-col items-center gap-1 hover:bg-amber-50 hover:border-amber-200 transition-colors group">
                                        <span class="text-xl group-hover:scale-110 transition-transform">😐</span>
                                        <span class="text-[9px] font-bold text-slate-400 group-hover:text-amber-600">Okay</span>
                                    </button>
                                    <button onclick="selectReaction(this)" class="reaction-btn p-3 rounded-xl border border-slate-200 bg-slate-50 flex flex-col items-center gap-1 hover:bg-rose-50 hover:border-rose-200 transition-colors group">
                                        <span class="text-xl group-hover:scale-110 transition-transform">🤢</span>
                                        <span class="text-[9px] font-bold text-slate-400 group-hover:text-rose-600">Refused</span>
                                    </button>
                                </div>
                            </div>

                            <button onclick="saveLog()" class="w-full bg-slate-900 text-white py-4 rounded-xl font-bold shadow-lg shadow-slate-200 active:scale-95 transition-transform flex justify-center items-center gap-2">
                                <i data-lucide="check" class="w-4 h-4"></i> ${t_dat.btn_save}
                            </button>
                        </div>
                    </div>
                </div>
            `;
            
            setTimeout(initChart, 100);
        }

        function renderDiet(container) {
            const t_dat = STORE.translations[STORE.lang];
            const details = STORE.recipeDetails;
            
            container.innerHTML = `
                <div class="animate-slide-up-fade pb-6">
                    <div class="grid grid-cols-1 gap-4">
                        ${details.map((r, i) => `
                            <div onclick="openRecipeModal(${i})" class="touch-card bg-white p-4 rounded-3xl shadow-soft flex items-start gap-4 cursor-pointer relative overflow-hidden">
                                <!-- Deco BG -->
                                <div class="absolute right-0 top-0 w-24 h-full bg-gradient-to-l from-slate-50 to-transparent"></div>
                                
                                <div class="w-20 h-20 bg-brand-50 rounded-2xl flex items-center justify-center text-4xl shadow-inner relative z-10">
                                    ${i === 0 ? '🥣' : i === 1 ? '🍌' : i === 2 ? '🍲' : '🍎'}
                                </div>
                                <div class="flex-1 relative z-10 py-1">
                                    <h3 class="font-bold text-slate-800 text-lg mb-1">${STORE.lang === 'ta' ? r.title_ta : r.title_en}</h3>
                                    <p class="text-xs text-slate-500 line-clamp-2 mb-3 leading-relaxed">${STORE.lang === 'ta' ? r.desc_ta : r.desc_en}</p>
                                    <div class="flex gap-3">
                                        <span class="inline-flex items-center gap-1 text-[10px] font-bold text-slate-400 bg-slate-50 px-2 py-1 rounded-lg">
                                            <i data-lucide="clock" class="w-3 h-3"></i> ${r.prep}
                                        </span>
                                        <span class="inline-flex items-center gap-1 text-[10px] font-bold text-slate-400 bg-slate-50 px-2 py-1 rounded-lg">
                                            <i data-lucide="flame" class="w-3 h-3"></i> ${r.cook}
                                        </span>
                                    </div>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
        }
        
        function renderChat(container) {
             container.innerHTML = `
                <div class="flex flex-col h-full animate-slide-up-fade">
                    <div class="bg-white p-4 rounded-t-3xl shadow-sm border-b border-slate-100 flex items-center gap-3">
                        <div class="relative">
                            <div class="w-10 h-10 bg-blue-100 rounded-full overflow-hidden">
                                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Nurse" class="w-full h-full">
                            </div>
                            <div class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-500 border-2 border-white rounded-full"></div>
                        </div>
                        <div>
                            <h3 class="font-bold text-sm text-slate-800">Nurse Sharon</h3>
                            <p class="text-[10px] text-green-600 font-bold uppercase">Online Now</p>
                        </div>
                    </div>
                    
                    <div class="flex-1 p-4 space-y-4 overflow-y-auto">
                        <div class="flex justify-start">
                            <div class="bg-white border border-slate-100 text-slate-600 text-sm p-3 rounded-2xl rounded-tl-none max-w-[85%] shadow-sm">
                                Hello! How can I help you with weaning today?
                            </div>
                        </div>
                        <div class="flex justify-end">
                            <div class="bg-brand-600 text-white text-sm p-3 rounded-2xl rounded-tr-none max-w-[85%] shadow-md">
                                Can I give cow's milk at 7 months?
                            </div>
                        </div>
                         <div class="flex justify-start">
                            <div class="bg-white border border-slate-100 text-slate-600 text-sm p-3 rounded-2xl rounded-tl-none max-w-[85%] shadow-sm">
                                No, cow's milk should not be given as a main drink before 1 year. It can cause iron deficiency. You can use small amounts in cooking (like mashed potatoes), but stick to breast milk or formula for drinking.
                            </div>
                        </div>
                    </div>

                    <div class="p-4 bg-white border-t border-slate-100">
                        <div class="flex gap-2">
                            <input type="text" placeholder="Type your question..." class="flex-1 bg-slate-100 border-none rounded-xl px-4 py-3 text-sm focus:ring-2 focus:ring-brand-500 outline-none">
                            <button class="bg-brand-600 text-white p-3 rounded-xl shadow-lg active:scale-95 transition-transform"><i data-lucide="send" class="w-5 h-5"></i></button>
                        </div>
                    </div>
                </div>
             `;
        }

        // --- INTERACTION LOGIC ---
        
        function showGuideSection(type, btn) {
            // Update Tab UI
            document.querySelectorAll('.guide-tab-active').forEach(el => {
                el.classList.remove('guide-tab-active', 'bg-white', 'text-brand-600', 'shadow-sm');
                el.classList.add('text-slate-500');
            });
            btn.classList.add('guide-tab-active', 'bg-white', 'text-brand-600', 'shadow-sm');
            btn.classList.remove('text-slate-500');

            // Render Content (Mocking content switch)
            const contentDiv = document.getElementById('guide-content');
            
            // Just basic HTML swap for demo logic
            let html = '';
            if(type === 'basics') {
                 html = `
                    <div class="bg-white p-5 rounded-3xl shadow-soft animate-scale-in">
                        <div class="flex items-start gap-4 mb-4">
                            <div class="w-10 h-10 rounded-full bg-blue-50 text-brand-600 flex items-center justify-center shrink-0"><i data-lucide="baby" class="w-5 h-5"></i></div>
                            <div><h3 class="font-bold text-slate-800 mb-1">Basics</h3><p class="text-xs text-slate-500 leading-relaxed">Start at 6 months completed. Continue breastfeeding on demand.</p></div>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-3xl shadow-soft animate-scale-in">
                        <h3 class="font-bold text-slate-800 mb-2">Rule of Thumb</h3>
                        <p class="text-xs text-slate-500">1 New Food every 3-4 days to check for allergies.</p>
                    </div>`;
            } else if (type === 'foods') {
                html = `
                    <div class="grid grid-cols-2 gap-3 animate-scale-in">
                        <div class="bg-white p-4 rounded-2xl shadow-soft text-center"><span class="text-2xl block mb-2">🥣</span><p class="text-xs font-bold text-slate-700">Porridge</p></div>
                        <div class="bg-white p-4 rounded-2xl shadow-soft text-center"><span class="text-2xl block mb-2">🥔</span><p class="text-xs font-bold text-slate-700">Mashed Veg</p></div>
                        <div class="bg-white p-4 rounded-2xl shadow-soft text-center"><span class="text-2xl block mb-2">🍎</span><p class="text-xs font-bold text-slate-700">Stewed Fruit</p></div>
                        <div class="bg-white p-4 rounded-2xl shadow-soft text-center"><span class="text-2xl block mb-2">🥚</span><p class="text-xs font-bold text-slate-700">Egg Yolk</p></div>
                    </div>
                    <div class="bg-red-50 p-4 rounded-2xl border border-red-100 mt-2 animate-scale-in">
                        <p class="text-xs font-bold text-red-700 flex items-center gap-2"><i data-lucide="x-circle" class="w-4 h-4"></i> Avoid Honey, Sugar, Salt</p>
                    </div>`;
            } else {
                html = `
                    <div class="bg-white p-5 rounded-3xl shadow-soft animate-scale-in">
                        <div class="flex justify-between items-center mb-4">
                            <h3 class="font-bold text-slate-800">6-8 Months</h3>
                            <span class="bg-brand-50 text-brand-700 px-2 py-1 rounded text-[10px] font-bold">2 Meals/Day</span>
                        </div>
                        <div class="space-y-3">
                            <div class="flex items-center gap-3 text-xs text-slate-600"><div class="w-2 h-2 rounded-full bg-slate-300"></div>Breakfast: Porridge</div>
                            <div class="flex items-center gap-3 text-xs text-slate-600"><div class="w-2 h-2 rounded-full bg-slate-300"></div>Lunch: Mashed Veg/Dal</div>
                        </div>
                    </div>`;
            }
            contentDiv.innerHTML = html;
            lucide.createIcons();
        }

        function openRecipeModal(index) {
            const recipe = STORE.recipeDetails[index];
            const isTa = STORE.lang === 'ta';
            
            const content = document.getElementById('modal-content');
            content.innerHTML = `
                <div class="text-center mb-6">
                    <div class="w-20 h-20 bg-brand-50 rounded-2xl mx-auto flex items-center justify-center text-4xl mb-4 shadow-inner">
                        ${index === 0 ? '🥣' : index === 1 ? '🍌' : index === 2 ? '🍲' : '🍎'}
                    </div>
                    <h2 class="text-2xl font-bold text-slate-900 mb-1">${isTa ? recipe.title_ta : recipe.title_en}</h2>
                    <p class="text-sm text-slate-500 px-6">${isTa ? recipe.desc_ta : recipe.desc_en}</p>
                </div>

                <div class="grid grid-cols-2 gap-4 mb-8">
                    <div class="bg-slate-50 p-3 rounded-xl text-center">
                        <span class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider">Prep Time</span>
                        <span class="font-bold text-slate-800">${recipe.prep}</span>
                    </div>
                    <div class="bg-slate-50 p-3 rounded-xl text-center">
                        <span class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider">Cook Time</span>
                        <span class="font-bold text-slate-800">${recipe.cook}</span>
                    </div>
                </div>

                <div class="mb-8">
                    <h3 class="text-sm font-bold text-slate-900 mb-4 border-l-4 border-brand-500 pl-3">Ingredients</h3>
                    <ul class="space-y-3">
                        ${(isTa ? recipe.ing_ta : recipe.ing_en).map(item => `
                            <li class="flex items-center gap-3 text-sm text-slate-600 bg-white border border-slate-100 p-3 rounded-xl shadow-sm">
                                <div class="w-1.5 h-1.5 rounded-full bg-brand-400"></div> ${item}
                            </li>
                        `).join('')}
                    </ul>
                </div>

                <div class="mb-8">
                    <h3 class="text-sm font-bold text-slate-900 mb-4 border-l-4 border-brand-500 pl-3">Instructions</h3>
                    <div class="space-y-4">
                        ${(isTa ? recipe.steps_ta : recipe.steps_en).map((step, i) => `
                            <div class="flex gap-4">
                                <div class="w-6 h-6 rounded-full bg-slate-900 text-white flex items-center justify-center text-xs font-bold shrink-0 shadow-lg shadow-slate-200 mt-0.5">${i+1}</div>
                                <p class="text-sm text-slate-600 leading-relaxed bg-slate-50 p-3 rounded-xl rounded-tl-none w-full">${step}</p>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
            
            const overlay = document.getElementById('modal-overlay');
            const sheet = overlay.querySelector('.modal-sheet');
            
            overlay.classList.remove('hidden');
            // Small delay to allow display:block to apply before transition
            setTimeout(() => {
                overlay.classList.remove('opacity-0');
                sheet.classList.remove('translate-y-full');
            }, 10);
        }

        function closeModal() {
            const overlay = document.getElementById('modal-overlay');
            const sheet = overlay.querySelector('.modal-sheet');
            
            sheet.classList.add('translate-y-full');
            overlay.classList.add('opacity-0');
            
            setTimeout(() => {
                overlay.classList.add('hidden');
            }, 300);
        }

        function selectReaction(btn) {
            document.querySelectorAll('.reaction-btn').forEach(b => {
                b.classList.remove('ring-2', 'ring-brand-500');
                b.classList.add('border-slate-200');
            });
            btn.classList.add('ring-2', 'ring-brand-500');
            btn.classList.remove('border-slate-200');
        }

        function saveLog() {
            const btn = document.querySelector('button[onclick="saveLog()"]');
            const originalHTML = btn.innerHTML;
            
            btn.innerHTML = `<i data-lucide="check-circle" class="w-5 h-5"></i> Saved!`;
            btn.classList.remove('bg-slate-900');
            btn.classList.add('bg-green-600');
            
            lucide.createIcons();
            
            setTimeout(() => {
                btn.innerHTML = originalHTML;
                btn.classList.add('bg-slate-900');
                btn.classList.remove('bg-green-600');
                lucide.createIcons();
                // In real app, update chart/history here
            }, 1500);
        }

        function openLink(type) {
             if(type === 'quiz') window.open("https://forms.gle/EJhej4Tfivpa7vM19", "_blank");
        }

        function initChart() {
            const ctx = document.getElementById('trackerChart');
            if(!ctx) return;
            
            // Mock Data
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'],
                    datasets: [{
                        label: 'Solids',
                        data: [2, 3, 2, 4, 3, 3, 2],
                        backgroundColor: '#2563eb',
                        borderRadius: 4,
                        barThickness: 8
                    },
                    {
                        label: 'Milk',
                        data: [4, 3, 4, 3, 3, 4, 3],
                        backgroundColor: '#e2e8f0',
                        borderRadius: 4,
                        barThickness: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        x: { grid: { display: false }, border: { display: false } },
                        y: { display: false }
                    },
                    animation: {
                        duration: 1000,
                        easing: 'easeOutQuart'
                    }
                }
            });
        }

        // --- INIT ---
        document.addEventListener('DOMContentLoaded', () => {
            switchTab('home');
            lucide.createIcons();
        });

    </script>
</body>
</html>
