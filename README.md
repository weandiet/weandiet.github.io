<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>WeanWise - App</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    <script src="https://unpkg.com/lucide@latest"></script>

    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Mukta+Malar:wght@400;500;600;700&display=swap" rel="stylesheet">

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                        tamil: ['"Mukta Malar"', 'sans-serif'],
                    },
                    colors: {
                        primary: { 50: '#eef2ff', 100: '#e0e7ff', 500: '#6366f1', 600: '#4f46e5', 700: '#4338ca' },
                        surface: '#f8fafc',
                    }
                }
            }
        }
    </script>

    <style>
        /* CRITICAL FIX: Lock the body to viewport height and prevent rubber-banding */
        html, body {
            height: 100dvh; /* Dynamic Viewport Height */
            width: 100vw;
            overflow: hidden; /* Stop body scroll */
            overscroll-behavior: none; /* Stop bounce */
            background-color: #f1f5f9;
            -webkit-tap-highlight-color: transparent;
            touch-action: pan-y;
        }

        /* Scrollable Area Styling */
        .scroll-area {
            overflow-y: auto;
            -webkit-overflow-scrolling: touch; /* Smooth scroll on iOS */
            overscroll-behavior: contain; /* Traps scroll inside element */
        }
        
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* Animations & Visuals */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-fade-in { animation: fadeIn 0.4s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
        .card-hover { transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); }
        .card-hover:active { transform: scale(0.97); }
        .nav-item-active { color: #4f46e5; background-color: #eef2ff; }
        .nav-item-inactive { color: #64748b; }
        .tab-pill { transition: all 0.3s ease; }
        .tab-pill.active { background-color: white; color: #4f46e5; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1); }
        
        /* Safe Area for iPhone X+ */
        .pb-safe { padding-bottom: env(safe-area-inset-bottom, 20px); }
        .pt-safe { padding-top: env(safe-area-inset-top, 20px); }
    </style>
</head>
<body class="flex items-center justify-center bg-slate-200">

    <div class="w-full h-full sm:max-w-[420px] sm:h-[95dvh] bg-surface sm:rounded-[2.5rem] shadow-2xl flex flex-col relative overflow-hidden sm:border-[8px] sm:border-slate-900">
        
        <header class="bg-white/95 backdrop-blur-md border-b border-slate-100 px-6 py-4 z-20 flex justify-between items-center shrink-0 pt-safe">
            <div class="flex items-center gap-3" onclick="switchTab('home')">
                <div class="w-10 h-10 bg-gradient-to-br from-primary-500 to-primary-700 rounded-xl flex items-center justify-center text-white shadow-lg shadow-primary-500/30 cursor-pointer">
                    <i data-lucide="utensils-crossed" class="w-5 h-5"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold text-slate-900 leading-tight">WeanWise</h1>
                    <p id="header-subtitle" class="text-[10px] text-slate-500 font-semibold tracking-wide">Complete Weaning Guide</p>
                </div>
            </div>
            <button onclick="toggleLanguage()" id="lang-btn" class="h-9 px-3 rounded-full bg-slate-100 hover:bg-slate-200 border border-slate-200 text-xs font-bold text-slate-700 transition-all flex items-center gap-1">
                <i data-lucide="languages" class="w-3 h-3"></i> <span>TA</span>
            </button>
        </header>

        <main id="main-content" class="flex-1 scroll-area hide-scrollbar bg-slate-50/50 relative w-full pb-6">
            </main>

        <nav class="bg-white border-t border-slate-100 px-4 py-2 shrink-0 z-30 pb-safe shadow-[0_-4px_6px_-1px_rgba(0,0,0,0.05)] relative">
            <div class="flex justify-between items-end h-14">
                <button onclick="switchTab('home')" class="nav-btn flex flex-col items-center justify-center w-14 h-12 rounded-xl transition-all" data-tab="home">
                    <i data-lucide="layout-grid" class="w-6 h-6 mb-0.5"></i>
                </button>
                <button onclick="switchTab('guide')" class="nav-btn flex flex-col items-center justify-center w-14 h-12 rounded-xl transition-all" data-tab="guide">
                    <i data-lucide="book-open" class="w-6 h-6 mb-0.5"></i>
                </button>
                
                <div class="relative -top-5">
                    <button onclick="switchTab('diet')" class="nav-btn bg-primary-600 text-white shadow-xl shadow-primary-600/40 w-14 h-14 rounded-full flex items-center justify-center transition-transform active:scale-95 border-4 border-white" data-tab="diet">
                        <i data-lucide="chef-hat" class="w-6 h-6"></i>
                    </button>
                </div>

                <button onclick="switchTab('tracker')" class="nav-btn flex flex-col items-center justify-center w-14 h-12 rounded-xl transition-all" data-tab="tracker">
                    <i data-lucide="activity" class="w-6 h-6 mb-0.5"></i>
                </button>
                <button onclick="switchTab('quiz')" class="nav-btn flex flex-col items-center justify-center w-14 h-12 rounded-xl transition-all" data-tab="quiz">
                    <i data-lucide="clipboard-check" class="w-6 h-6 mb-0.5"></i>
                </button>
            </div>
        </nav>

        <div id="modal-overlay" class="absolute inset-0 z-50 hidden">
            <div class="absolute inset-0 bg-slate-900/60 backdrop-blur-sm transition-opacity opacity-0" id="modal-backdrop" onclick="closeModal()"></div>
            <div class="absolute bottom-0 left-0 right-0 bg-white rounded-t-[2.5rem] h-[90%] transform translate-y-full transition-transform duration-300 ease-out flex flex-col shadow-2xl" id="modal-panel">
                <div class="p-6 border-b border-slate-100 flex justify-between items-center shrink-0">
                    <h3 id="modal-title" class="font-bold text-xl text-slate-800 line-clamp-1">Details</h3>
                    <button onclick="closeModal()" class="w-8 h-8 bg-slate-100 rounded-full flex items-center justify-center hover:bg-slate-200 transition-colors">
                        <i data-lucide="x" class="w-4 h-4 text-slate-600"></i>
                    </button>
                </div>
                <div id="modal-body" class="flex-1 overflow-y-auto p-6 space-y-6 scroll-area"></div>
            </div>
        </div>

    </div>

    <script>
        // --- DATA STORE ---
        const STORE = {
            lang: 'en',
            translations: {
                en: {
                    subtitle: "Your Complete Weaning Food Guide",
                    nav_home: "Home", nav_tracker: "Tracker", nav_diet: "Recipes", nav_guide: "Guide", nav_chat: "Chat", nav_quiz: "Quiz",
                    welcome_title: "Start Weaning Today",
                    welcome_desc: "A step-by-step guide to introduce solids to your baby (6m+) with confidence and care.",
                    quick_tracker: "Log Meal", quick_tracker_desc: "Track solids & milk",
                    quick_chat: "Ask Expert", quick_chat_desc: "Chat with Nurse",
                    pop_recipe: "Popular Recipe", pop_recipe_desc: "Nutritious & easy to make",
                    feedback_card_title: "Research Feedback", feedback_card_desc: "Help our study by rating this app.",
                    guide_tab1: "Start", guide_tab2: "Foods", guide_tab3: "Routine", guide_tab4: "Care",
                    red_flag_title: "Foods to Avoid",
                    schedule_6m: "6-8 Months", schedule_9m: "9-11 Months", schedule_1y: "1-2 Years",
                    track_food_label: "Food Item", track_reaction: "Reaction", track_save: "Save Log",
                    chat_placeholder: "Type keyword (e.g., Milk, Avoid)...",
                    qa_title: "Common Questions",
                    quiz_title: "Quiz", 
                    feedback_title: "Study Feedback", feedback_q1: "Was the app easy to use?", feedback_q2: "Did you feel more confident?",
                    feedback_submit: "Submit Survey", feedback_thanks: "Thank you for participating!",
                    chart_label_solid: "Solid Meals", chart_label_milk: "Milk Feeds",
                    reaction_loved: "Loved", reaction_ok: "Okay", reaction_refused: "Refused",
                    food_options: ["Breast Milk", "Formula", "Ragi Porridge", "Mashed Idli", "Dal Water", "Khichdi", "Mashed Banana", "Boiled Egg", "Family Pot Food"],
                    recipe_title: "Healthy Recipes", recipe_prep: "Prep time", recipe_cook: "Cook time", recipe_ing: "Ingredients", recipe_steps: "Instructions"
                },
                ta: {
                    subtitle: "உங்கள் முழுமையான இணை உணவு வழிகாட்டி",
                    nav_home: "முகப்பு", nav_tracker: "பதிவு", nav_diet: "சமையல்", nav_guide: "வழிகாட்டி", nav_chat: "உதவி", nav_quiz: "வினாடி",
                    welcome_title: "இணை உணவு ஆரம்பம்",
                    welcome_desc: "6 மாத குழந்தைகளுக்கான முழுமையான உணவு மற்றும் ஊட்டச்சத்து வழிகாட்டி.",
                    quick_tracker: "உணவு பதிவு", quick_tracker_desc: "உணவு விபரம்",
                    quick_chat: "நிபுணரிடம் கேள்", quick_chat_desc: "செவிலியர் உதவி",
                    pop_recipe: "பிரபலமான உணவு", pop_recipe_desc: "சத்தான மற்றும் எளிமையானது",
                    feedback_card_title: "ஆய்வு கருத்துக்கணிப்பு", feedback_card_desc: "உங்கள் கருத்துக்கள் எங்களுக்கு தேவை.",
                    guide_tab1: "துவக்கம்", guide_tab2: "உணவுகள்", guide_tab3: "முறை", guide_tab4: "பராமரிப்பு",
                    red_flag_title: "தவிர்க்க வேண்டியவை",
                    schedule_6m: "6-8 மாதங்கள்", schedule_9m: "9-11 மாதங்கள்", schedule_1y: "1-2 ஆண்டுகள்",
                    track_food_label: "உணவு வகை", track_reaction: "எதிர்வினை", track_save: "சேமிக்கவும்",
                    chat_placeholder: "தேடு (உதாரணம்: பால், தவிர்க்க)...",
                    qa_title: "அடிக்கடி கேட்கப்படும் கேள்விகள்",
                    quiz_title: "வினாடி வினா",
                    feedback_title: "கருத்துக்கணிப்பு படிவம்", feedback_q1: "செயலி பயன்படுத்த எளிதாக இருந்ததா?", feedback_q2: "உணவு கொடுப்பதில் நம்பிக்கை உள்ளதா?",
                    feedback_submit: "சமர்ப்பிக்கவும்", feedback_thanks: "பங்கேற்றமைக்கு நன்றி!",
                    chart_label_solid: "திட உணவுகள்", chart_label_milk: "பால்",
                    reaction_loved: "பிடித்தது", reaction_ok: "பரவாயில்லை", reaction_refused: "மறுத்தது",
                    food_options: ["தாய்ப்பால்", "ஃபார்முலா பால்", "ராகி கூழ்", "இட்லி மசியல்", "பருப்பு தண்ணீர்", "கிச்சடி", "வாழைப்பழம்", "வேகவைத்த முட்டை", "வீட்டு உணவு"],
                    recipe_title: "சத்தான சமையல்", recipe_prep: "தயாரிப்பு", recipe_cook: "சமையல்", recipe_ing: "தேவையானவை", recipe_steps: "செய்முறை"
                }
            },
            recipes: [
                {
                    id: 1,
                    title_en: "Ragi Koozh (Finger Millet Porridge)", title_ta: "ராகி கூழ்",
                    desc_en: "High Calcium, Iron, and Fiber. Best for 6+ Months (First Food).", desc_ta: "கால்சியம், இரும்புச்சத்து மற்றும் நார்ச்சத்து நிறைந்தது. 6+ மாதங்களுக்கு ஏற்றது (முதல் உணவு).",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["2 tbsp Sprouted Ragi Flour", "1 cup Water", "1/2 tsp Ghee", "Jaggery/Cumin (Optional)"],
                    ing_ta: ["2 மேசைக்கரண்டி முளைக்கட்டிய ராகி மாவு", "1 கப் தண்ணீர்", "1/2 தேக்கரண்டி நெய்", "வெல்லம்/சீரகத்தூள் (விருப்பப்பட்டால்)"],
                    steps_en: ["Mix ragi flour with a little water to make a smooth paste.", "Boil remaining water. Add paste slowly.", "Cook on low flame until thick (8-10 mins).", "Add ghee. Serve lukewarm."],
                    steps_ta: ["ராகி மாவை சிறிது தண்ணீரில் கட்டிகள் இல்லாமல் கரைக்கவும்.", "மீதமுள்ள தண்ணீரை கொதிக்க வைத்து அதில் மெதுவாக சேர்க்கவும்.", "மிதமான தீயில் கெட்டியாகும் வரை (8-10 நிமிடம்) காய்ச்சவும்.", "நெய் சேர்த்து மிதமான சூட்டில் பரிமாறவும்."]
                },
                {
                    id: 2,
                    title_en: "Paruppu Sadam (Lentil Mash)", title_ta: "பருப்பு சாதம்",
                    desc_en: "High Protein, Carbohydrates, and Energy. Best for 6-8 Months.", desc_ta: "புரதம், கார்போஹைட்ரேட் மற்றும் ஆற்றல் நிறைந்தது. 6-8 மாதங்களுக்கு ஏற்றது.",
                    prep: "5 min", cook: "15 min",
                    ing_en: ["2 tbsp Rice", "1 tbsp Moong Dal", "1 pinch Turmeric", "1/2 tsp Ghee", "1.5 cups Water"],
                    ing_ta: ["2 மேசைக்கரண்டி அரிசி", "1 மேசைக்கரண்டி பாசிப்பருப்பு", "1 சிட்டிகை மஞ்சள் தூள்", "1/2 தேக்கரண்டி நெய்", "1.5 கப் தண்ணீர்"],
                    steps_en: ["Wash rice and dal.", "Pressure cook with turmeric and water for 4-5 whistles.", "Mash well while hot.", "Drizzle ghee and mix."],
                    steps_ta: ["அரிசி மற்றும் பருப்பை கழுவவும்.", "மஞ்சள் தூள் மற்றும் தண்ணீர் சேர்த்து குக்கரில் 4-5 விசில் விடவும்.", "சூடாக இருக்கும்போதே நன்றாக மசிக்கவும்.", "நெய் சேர்த்து கலக்கவும்."]
                },
                {
                    id: 3,
                    title_en: "Idli Mash", title_ta: "இட்லி மசியல்",
                    desc_en: "Probiotics, easy digestibility. Best for 6-8 Months.", desc_ta: "புரோபயாட்டிக்ஸ் நிறைந்தது, எளிதில் ஜீரணமாகும். 6-8 மாதங்களுக்கு ஏற்றது.",
                    prep: "15 min", cook: "0 min",
                    ing_en: ["1 Idli (Steam cooked)", "Warm Water or Breast Milk", "Drop of Ghee (Optional)"],
                    ing_ta: ["1 இட்லி (ஆவியில் வெந்தது)", "வெதுவெதுப்பான நீர் அல்லது தாய்ப்பால்", "சிறிது நெய் (விருப்பப்பட்டால்)"],
                    steps_en: ["Steam fresh, soft idlis.", "Crumble one warm idli into a bowl.", "Add warm water or breast milk.", "Mash to a smooth consistency."],
                    steps_ta: ["இட்லியை ஆவியில் வேகவைக்கவும்.", "இட்லியை கிண்ணத்தில் உதிர்த்து போடவும்.", "வெதுவெதுப்பான நீர்/பால் சேர்க்கவும்.", "கட்டிகள் இல்லாமல் நன்றாக மசிக்கவும்."]
                },
                {
                    id: 4,
                    title_en: "Apple Suji Kheer (Rava Porridge)", title_ta: "ஆப்பிள் ரவை கஞ்சி",
                    desc_en: "Vitamin C, Fiber, and Carbohydrates. Best for 7+ Months.", desc_ta: "வைட்டமின் சி, நார்ச்சத்து மற்றும் கார்போஹைட்ரேட். 7+ மாதங்களுக்கு ஏற்றது.",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["1 tbsp Roasted Sooji (Rava)", "1/2 Apple (grated/pureed)", "1 cup Water", "1/2 tsp Ghee"],
                    ing_ta: ["1 மேசைக்கரண்டி வறுத்த ரவை", "1/2 ஆப்பிள் (துருவியது)", "1 கப் தண்ணீர்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Roast sooji in ghee until aromatic.", "Add water and cook until soft.", "Add grated apple and cook for 2-3 mins.", "Serve warm."],
                    steps_ta: ["ரவையை நெய்யில் வாசனை வரும் வரை வறுக்கவும்.", "தண்ணீர் சேர்த்து ரவையை வேகவைக்கவும்.", "துருவிய ஆப்பிள் சேர்த்து 2-3 நிமிடம் வேகவைக்கவும்.", "சூடாக பரிமாறவும்."]
                },
                {
                    id: 5,
                    title_en: "Carrot-Moong Dal Khichdi", title_ta: "கேரட் பாசிப்பருப்பு கிச்சடி",
                    desc_en: "Vitamin A (Eyes), Protein, and Fiber. Best for 8+ Months.", desc_ta: "வைட்டமின் ஏ (கண்), புரதம் மற்றும் நார்ச்சத்து. 8+ மாதங்களுக்கு ஏற்றது.",
                    prep: "10 min", cook: "15 min",
                    ing_en: ["2 tbsp Rice", "1 tbsp Moong Dal", "1 small Carrot (chopped)", "Turmeric & Cumin powder", "2 cups Water"],
                    ing_ta: ["2 மேசைக்கரண்டி அரிசி", "1 மேசைக்கரண்டி பாசிப்பருப்பு", "1 சிறிய கேரட் (நறுக்கியது)", "மஞ்சள் & சீரகத்தூள்", "2 கப் தண்ணீர்"],
                    steps_en: ["Wash rice and dal.", "Pressure cook rice, dal, carrot, turmeric, water (4 whistles).", "Mash everything together.", "Add cumin powder for digestion."],
                    steps_ta: ["அரிசி மற்றும் பருப்பை கழுவவும்.", "அரிசி, பருப்பு, கேரட், மஞ்சள், தண்ணீர் சேர்த்து 4 விசில் விடவும்.", "அனைத்தையும் ஒன்றாக மசிக்கவும்.", "செரிமானத்திற்கு சீரகத்தூள் சேர்க்கவும்."]
                },
                {
                    id: 6,
                    title_en: "Curd Rice (Thayir Sadam) - Mash", title_ta: "தயிர் சாதம் மசியல்",
                    desc_en: "Probiotics (Gut health), Calcium, cooling. Best for 8+ Months.", desc_ta: "புரோபயாட்டிக்ஸ் (குடல் நலம்), கால்சியம், குளிர்ச்சி. 8+ மாதங்களுக்கு ஏற்றது.",
                    prep: "10 min", cook: "0 min",
                    ing_en: ["1/2 cup Soft Cooked Rice", "2 tbsp Fresh Homemade Curd", "Coriander leaves (Optional)"],
                    ing_ta: ["1/2 கப் குழைவான சாதம்", "2 மேசைக்கரண்டி புதிய தயிர்", "கொத்தமல்லி இலைகள் (விருப்பப்பட்டால்)"],
                    steps_en: ["Mash the cooked rice thoroughly to a paste.", "Mix in the fresh curd.", "Note: No salt for babies under 1 year."],
                    steps_ta: ["சாதத்தை நன்றாக பேஸ்ட் போல மசிக்கவும்.", "புதிய தயிர் சேர்த்து கலக்கவும்.", "குறிப்பு: 1 வயதுக்கு கீழ் உப்பு சேர்க்க வேண்டாம்."]
                },
                {
                    id: 7,
                    title_en: "Poha (Aval) Porridge", title_ta: "அவல் கஞ்சி",
                    desc_en: "Easily digestible Carbohydrates, Iron. Best for 6-8 Months.", desc_ta: "எளிதில் ஜீரணமாகும் கார்போஹைட்ரேட், இரும்புச்சத்து. 6-8 மாதங்களுக்கு ஏற்றது.",
                    prep: "5 min", cook: "5 min",
                    ing_en: ["2 tbsp Poha (Flattened Rice)", "Warm Water or Milk", "Jaggery (Optional)", "Ghee"],
                    ing_ta: ["2 மேசைக்கரண்டி அவல்", "வெதுவெதுப்பான நீர் அல்லது பால்", "வெல்லம் (விருப்பப்பட்டால்)", "நெய்"],
                    steps_en: ["Dry roast poha and powder it.", "Mix powder with water, cook on low flame (3-4 mins).", "Add a little ghee."],
                    steps_ta: ["அவலை வறுத்து பொடித்துக் கொள்ளவும்.", "பொடியை தண்ணீரில் கலந்து மிதமான தீயில் (3-4 நிமிடம்) வேகவைக்கவும்.", "சிறிது நெய் சேர்க்கவும்."]
                },
                {
                    id: 8,
                    title_en: "Mashed Banana & Ghee", title_ta: "வாழைப்பழம் நெய் மசியல்",
                    desc_en: "Potassium, Energy, Fiber. Good for weight gain. Best for 6+ Months.", desc_ta: "பொட்டாசியம், ஆற்றல், நார்ச்சத்து. எடை கூட உதவும். 6+ மாதங்களுக்கு ஏற்றது.",
                    prep: "2 min", cook: "0 min",
                    ing_en: ["1 Small Ripe Banana", "1/2 tsp Ghee"],
                    ing_ta: ["1 சிறிய பழுத்த வாழைப்பழம்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Peel the banana.", "Mash it well with a fork.", "Mix in the ghee."],
                    steps_ta: ["வாழைப்பழத்தை உரித்துக்கொள்ளவும்.", "ஸ்பூன் கொண்டு நன்றாக மசிக்கவும்.", "நெய் சேர்த்து கலக்கவும்."]
                },
                {
                    id: 9,
                    title_en: "Rasam Sadam (Pepper Water Rice)", title_ta: "ரசம் சாதம்",
                    desc_en: "Digestion aid, immunity boosting. Best for 9+ Months.", desc_ta: "செரிமானம், நோய் எதிர்ப்பு சக்தி. 9+ மாதங்களுக்கு ஏற்றது.",
                    prep: "5 min", cook: "20 min",
                    ing_en: ["1/2 cup Soft Cooked Rice", "1/2 cup Mild Rasam (No chili)", "1 tsp Ghee"],
                    ing_ta: ["1/2 கப் குழைவான சாதம்", "1/2 கப் மிதமான ரசம் (மிளகாய் இல்லாமல்)", "1 தேக்கரண்டி நெய்"],
                    steps_en: ["Take soft cooked rice in a bowl.", "Pour mild rasam over it.", "Mash well so rice absorbs rasam.", "Add ghee."],
                    steps_ta: ["குழைவான சாதத்தை கிண்ணத்தில் எடுக்கவும்.", "ரசம் ஊற்றவும்.", "சாதம் ரசத்தை உறிஞ்சும் வரை மசிக்கவும்.", "நெய் சேர்க்கவும்."]
                },
                {
                    id: 10,
                    title_en: "Sathu Maavu Kanji (Health Mix)", title_ta: "சத்துமாவு கஞ்சி",
                    desc_en: "Balanced meal - Protein, Fats, Minerals. Best for 8+ Months.", desc_ta: "சமச்சீர் உணவு - புரதம், கொழுப்பு, தாதுக்கள். 8+ மாதங்களுக்கு ஏற்றது.",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["2 tbsp Sathu Maavu Powder", "1 cup Water", "1/2 tsp Ghee"],
                    ing_ta: ["2 மேசைக்கரண்டி சத்துமாவு", "1 கப் தண்ணீர்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Mix powder with water ensuring no lumps.", "Cook on medium flame until thick.", "Add ghee."],
                    steps_ta: ["சத்துமாவை தண்ணீரில் கட்டிகள் இல்லாமல் கரைக்கவும்.", "மிதமான தீயில் கெட்டியாகும் வரை காய்ச்சவும்.", "நெய் சேர்க்கவும்."]
                }
            ],
            chatHistory: [
                { sender: 'bot', text_en: 'Hello! I am Nurse Sharon. Tap a category or type a keyword like "Milk" or "Schedule".', text_ta: 'வணக்கம்! நான் செவிலியர் ஷரோன். கீழே உள்ள வகையைத் தேர்ந்தெடுக்கவும் அல்லது "பால்" போன்ற சொல்லைத் தட்டச்சு செய்யவும்.' }
            ],
            qa_db: [
                { id: "q1", cat: "Definitions", q: "What is the modern term used for \"Weaning\"?", a: "The term \"Weaning\" is now largely replaced by \"Complementary Feeding.\"", keys: ["weaning", "term", "definition"] },
                { id: "q2", cat: "Definitions", q: "What is the definition of Complementary Feeding?", a: "It is the process of introducing suitable semi-solid and solid foods to an infant at a developmentally appropriate age (6 months) to meet nutritional requirements while continuing breastfeeding.", keys: ["complementary", "feeding", "definition"] },
                { id: "q3", cat: "Definitions", q: "What is the primary goal of weaning?", a: "To provide a nutritional balance for proper growth and development and to gradually accustom the child to the \"family pot\" (normal family food) by the second year of life.", keys: ["goal", "aim", "why"] },
                { id: "q4", cat: "Definitions", q: "Does weaning mean stopping breastfeeding?", a: "No. Weaning is not the sudden withdrawal of breast milk. It is a gradual process where breast milk is supplemented with other foods. Breastfeeding should continue up to 2 years or beyond.", keys: ["stop", "breastfeeding", "milk", "withdrawal"] },
                { id: "q5", cat: "Definitions", q: "What is \"Baby-Led Weaning\" (BLW)?", a: "A method where infants (6 months+) are encouraged to self-feed \"whole\" soft pieces of food (finger foods) instead of being spoon-fed purees by a parent.", keys: ["blw", "baby-led", "self-feed"] },
                { id: "q6", cat: "Timing", q: "At what age should complementary feeding strictly begin?", a: "It should begin at 6 completed months (180 days) of age.", keys: ["age", "start", "begin", "when"] },
                { id: "q7", cat: "Timing", q: "Why is exclusive breastfeeding recommended for the first 0–6 months?", a: "Breast milk is nutritionally complete, aids the immune system, and the infant's digestive organs are not mature enough to tolerate other foods yet.", keys: ["exclusive", "0-6", "breastfeeding", "why"] },
                { id: "q8", cat: "Timing", q: "Why must complementary foods be introduced after 6 months?", a: "Breast milk alone is no longer sufficient to meet the growing baby's needs for energy, protein, iron, and zinc. The child’s body mass and activity levels increase significantly.", keys: ["after 6 months", "why", "sufficient"] },
                { id: "q9", cat: "Timing", q: "What are the risks of starting complementary feeding too late?", a: "Delayed introduction can lead to growth faltering (malnutrition), micronutrient deficiencies (iron/zinc), and the child may enter a \"critical period\" where they struggle to learn chewing and swallowing solids later.", keys: ["late", "delayed", "risk"] },
                { id: "q10", cat: "Timing", q: "What developmental milestones indicate a child is ready for solids?", a: "5 months: Biting movements appear.<br>6–7 months: Ability to swallow solids; good head/neck control.<br>8–12 months: Side-to-side tongue movements.", keys: ["milestone", "ready", "signs"] },
                { id: "q11", cat: "Principles", q: "What are the \"Guiding Principles\" of complementary feeding?", a: "<b>Timely:</b> Start at 6 months.<br><b>Adequate:</b> Sufficient energy/nutrients.<br><b>Safe:</b> Hygienically prepared.<br><b>Properly Fed:</b> Responsive feeding (no force-feeding).", keys: ["principles", "guide"] },
                { id: "q12", cat: "Principles", q: "How should new foods be introduced?", a: "Introduce only one food at a time (interval of 4–5 days) to easily detect allergies. Start with small amounts (1–2 teaspoons).", keys: ["new", "introduce", "allergy"] },
                { id: "q13", cat: "Principles", q: "What is the correct progression of food consistency?", a: "Liquids → Semi-solids (pureed/mashed) → Solids (chopped/finger foods).", keys: ["consistency", "texture", "progression"] },
                { id: "q14", cat: "Principles", q: "Should water be given before 6 months?", a: "No. Even in hot climates, breast milk provides sufficient hydration. Water can be introduced in small amounts after 6 months.", keys: ["water", "drink"] },
                { id: "q15", cat: "Principles", q: "What is \"Responsive Feeding\"?", a: "Feeding with patience and love (Bahlaa ke, manaa ke khilao). It involves talking to the child, maintaining eye contact, encouraging them to eat, and never forcing them.", keys: ["responsive", "force", "love"] },
                { id: "q16", cat: "Diet Plan", q: "Why is \"Energy Density\" important for infant food?", a: "Children have small stomach capacities but high energy needs. They cannot eat large bulky quantities, so food must be calorie-rich in small volumes.", keys: ["energy", "density", "calorie"] },
                { id: "q17", cat: "Diet Plan", q: "How can you increase the energy density of a child's meal?", a: "<b>Add Fats:</b> Oil/ghee/butter.<br><b>Add Sweeteners:</b> Sugar/jaggery (moderation).<br><b>Thicken It:</b> Avoid watery gruels.<br><b>Amylase-Rich Foods:</b> Malted grains.", keys: ["increase", "energy", "fat", "ghee"] },
                { id: "q18", cat: "Diet Plan", q: "What is the ideal calorie distribution for a young child?", a: "Carbohydrates (55–60%), Proteins (10–12%), Fats (25–30%).", keys: ["calorie", "distribution", "fat", "protein"] },
                { id: "q19", cat: "Diet Plan", q: "Which nutrients are most critical to supplement after 6 months?", a: "Iron, Zinc, Vitamin A, and Calcium.", keys: ["nutrient", "iron", "zinc", "vitamin"] },
                { id: "q20", cat: "Selection", q: "What are good \"First Foods\" to introduce at 6 months?", a: "Soft porridge (rice, wheat, ragi, suji), mashed potatoes, mashed banana/papaya, and thick dal with added ghee.", keys: ["first", "food", "start", "milk"] },
                { id: "q21", cat: "Selection", q: "Which foods should be strictly AVOIDED under 1 year?", a: "<b>Honey:</b> Risk of Botulism.<br><b>Cow’s Milk (as main drink):</b> Bleeding risk.<br><b>Salt/Sugar (Excess):</b> Kidney stress.", keys: ["avoid", "honey", "cow", "milk", "salt", "sugar"] },
                { id: "q22", cat: "Selection", q: "Which foods are choking hazards (avoid until 3 years)?", a: "Whole nuts, seeds, popcorn, hard candies, raw carrots, and whole grapes. (Nuts allowed if finely powdered).", keys: ["choking", "hazard", "avoid", "nut"] },
                { id: "q23", cat: "Selection", q: "Is it safe to give tea or coffee to infants?", a: "No. They inhibit iron absorption and offer no nutritional value.", keys: ["tea", "coffee", "caffeine", "avoid"] },
                { id: "q24", cat: "Selection", q: "Can I give fruit juices?", a: "Diluted fruit juice (1:10) is mentioned, but generally, whole mashed fruit is preferred. Avoid commercial sugary juices.", keys: ["juice", "fruit"] },
                { id: "q25", cat: "Schedule", q: "What is the feeding schedule for a 6–9 month old?", a: "<b>Freq:</b> Breastfeeding + 2–3 meals + 1 snack.<br><b>Amount:</b> 2–3 tbsp to ½ cup (125 ml).<br><b>Texture:</b> Mashed/semi-solid.", keys: ["schedule", "6-9"] },
                { id: "q26", cat: "Schedule", q: "What is the feeding schedule for a 9–12 month old?", a: "<b>Freq:</b> Breastfeeding + 3 meals + 1 snack.<br><b>Amount:</b> ½ to ¾ cup (at least 1 katori).<br><b>Texture:</b> Finely chopped, finger foods.", keys: ["schedule", "9-12"] },
                { id: "q27", cat: "Schedule", q: "What is the feeding schedule for a 12–24 month old?", a: "<b>Freq:</b> Breastfeeding + 3–4 meals + 2 snacks.<br><b>Amount:</b> ¾ to 1 full cup (250 ml).<br><b>Texture:</b> Family food.", keys: ["schedule", "12-24"] },
                { id: "q28", cat: "Special", q: "How should I feed a child during illness (e.g., Diarrhea)?", a: "Never stop feeding. Continue breastfeeding frequently. Offer soft, favorite foods in small amounts. Increase fluid intake.", keys: ["illness", "sick", "diarrhea"] },
                { id: "q29", cat: "Special", q: "What is the feeding advice after an illness?", a: "Give one extra meal per day for a week to help the child catch up on lost weight (\"Catch-up growth\").", keys: ["after illness", "sick", "catch-up"] },
                { id: "q30", cat: "Special", q: "How do you feed a Low Birth Weight (LBW) baby (<1.5 kg)?", a: "They may require tube feeding or spoon-feeding of expressed breast milk.", keys: ["lbw", "low birth", "milk"] },
                { id: "q31", cat: "Special", q: "How do you feed an LBW baby (1.5–1.8 kg)?", a: "Cup and spoon feeding is preferred over bottles.", keys: ["lbw"] },
                { id: "q32", cat: "Special", q: "How is feeding managed in Severe Acute Malnutrition (SAM)?", a: "<b>Stabilization:</b> F-75 diet via cup/spoon.<br><b>Rehabilitation:</b> Switch to F-100 diet or energy-dense home foods.", keys: ["sam", "malnutrition", "milk", "illness", "sick"] },
                { id: "q33", cat: "Hygiene", q: "Why are feeding bottles discouraged?", a: "They are difficult to clean and are a major source of infection (diarrhea). Cup and spoon feeding is safer.", keys: ["bottle", "hygiene"] },
                { id: "q34", cat: "Hygiene", q: "How long can cooked food be kept before feeding?", a: "In hot climates, fresh food should be consumed within 1–2 hours of preparation unless refrigerated.", keys: ["storage", "fresh", "cooked"] },
                { id: "q35", cat: "Hygiene", q: "What is a potential medical risk of weaning involving the intestines?", a: "Higher chance of Intussusception because Peyer’s patches enlarge and can act as a lead point.", keys: ["intussusception", "risk", "intestine"] },
                { id: "q36", cat: "Hygiene", q: "What are the risks of using commercial/processed baby foods?", a: "Often high in salt, sugar, preservatives. Predispose child to obesity and poor eating habits.", keys: ["commercial", "processed", "avoid"] }
            ],
            feedLogs: [
                { timestamp: new Date().getTime(), food: "Ragi Porridge", type: "solid", reaction: "liked" },
                { timestamp: new Date().getTime() - 86400000, food: "Breast Milk", type: "milk", reaction: "loved" }
            ],
            chartInstance: null
        };

        // --- CORE LOGIC ---
        function t(key) {
            return STORE.translations[STORE.lang][key] || STORE.translations['en'][key] || key;
        }

        function toggleLanguage() {
            STORE.lang = STORE.lang === 'en' ? 'ta' : 'en';
            
            // Update button UI
            const btn = document.getElementById('lang-btn');
            if(STORE.lang === 'ta') {
                btn.innerHTML = `<i data-lucide="languages" class="w-3 h-3"></i> <span>EN</span>`;
                btn.classList.add('bg-primary-50', 'text-primary-700', 'border-primary-100');
            } else {
                btn.innerHTML = `<i data-lucide="languages" class="w-3 h-3"></i> <span>TA</span>`;
                btn.classList.remove('bg-primary-50', 'text-primary-700', 'border-primary-100');
            }
            lucide.createIcons();

            // Re-render current tab
            const activeBtn = document.querySelector('.nav-btn.nav-item-active');
            const activeTab = activeBtn ? activeBtn.dataset.tab : 'home';
            switchTab(activeTab);
        }

        function switchTab(tabId) {
            // Update Nav UI
            document.querySelectorAll('.nav-btn').forEach(btn => {
                // Diet button is special
                if (btn.dataset.tab === 'diet') return; 

                if(btn.dataset.tab === tabId) {
                    btn.classList.add('nav-item-active');
                    btn.classList.remove('nav-item-inactive');
                } else {
                    btn.classList.remove('nav-item-active');
                    btn.classList.add('nav-item-inactive');
                }
            });

            // Update Diet Button State
            const dietBtn = document.querySelector('[data-tab="diet"]');
            if (tabId === 'diet') {
                dietBtn.classList.add('scale-110');
            } else {
                dietBtn.classList.remove('scale-110');
            }

            // Update Header & Translations
            document.getElementById('header-subtitle').innerText = t('subtitle');
            
            // Main Content Container
            const main = document.getElementById('main-content');
            main.className = "flex-1 scroll-area hide-scrollbar bg-slate-50/50 relative w-full pb-6 animate-fade-in";
            main.scrollTop = 0;

            // Route Logic
            if (tabId === 'home') renderHome(main);
            else if (tabId === 'guide') renderGuide(main);
            else if (tabId === 'chat') renderChat(main);
            else if (tabId === 'tracker') renderTracker(main);
            else if (tabId === 'diet') renderDiet(main);
            else if (tabId === 'quiz') renderQuiz(main);
            else if (tabId === 'feedback') renderFeedback(main);
            
            lucide.createIcons();
        }

        // --- RENDERERS ---
        function renderHome(container) {
            container.innerHTML = `
                <div class="px-6 py-6 space-y-6">
                    <div class="bg-gradient-to-br from-primary-600 to-indigo-800 rounded-[2rem] p-8 text-white shadow-xl shadow-primary-900/20 relative overflow-hidden group cursor-pointer" onclick="switchTab('guide')">
                        <div class="absolute top-0 right-0 w-32 h-32 bg-white/10 rounded-full blur-2xl transform translate-x-10 -translate-y-10"></div>
                        <div class="absolute bottom-0 left-0 w-24 h-24 bg-primary-400/20 rounded-full blur-xl transform -translate-x-5 translate-y-5"></div>
                        
                        <div class="relative z-10">
                            <span class="inline-block px-3 py-1 bg-white/20 backdrop-blur-md rounded-full text-[10px] font-bold uppercase tracking-wider mb-3 border border-white/10">Start Here</span>
                            <h2 class="text-2xl font-bold mb-2 leading-tight">${t('welcome_title')}</h2>
                            <p class="text-sm text-primary-100 leading-relaxed mb-6 opacity-90 font-medium max-w-[90%]">${t('welcome_desc')}</p>
                            <button class="bg-white text-primary-700 px-6 py-3 rounded-full text-xs font-extrabold hover:bg-slate-50 transition-colors shadow-sm flex items-center gap-2">
                                Start Reading <i data-lucide="arrow-right" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div onclick="switchTab('tracker')" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 card-hover cursor-pointer group">
                            <div class="w-12 h-12 bg-emerald-50 rounded-2xl flex items-center justify-center text-emerald-600 mb-4 group-hover:scale-110 transition-transform"><i data-lucide="plus" class="w-6 h-6"></i></div>
                            <h3 class="font-bold text-slate-800 text-sm">${t('quick_tracker')}</h3>
                            <p class="text-xs text-slate-400 mt-1 font-medium">${t('quick_tracker_desc')}</p>
                        </div>
                        <div onclick="switchTab('chat')" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 card-hover cursor-pointer group">
                            <div class="w-12 h-12 bg-violet-50 rounded-2xl flex items-center justify-center text-violet-600 mb-4 group-hover:scale-110 transition-transform"><i data-lucide="message-square" class="w-6 h-6"></i></div>
                            <h3 class="font-bold text-slate-800 text-sm">${t('quick_chat')}</h3>
                            <p class="text-xs text-slate-400 mt-1 font-medium">${t('quick_chat_desc')}</p>
                        </div>
                    </div>

                    <div class="flex items-center justify-between mt-2">
                         <h3 class="font-bold text-slate-800 text-lg">${t('pop_recipe')}</h3>
                         <button onclick="switchTab('diet')" class="text-xs font-bold text-primary-600 hover:text-primary-700 bg-primary-50 px-3 py-1 rounded-full transition-colors">View All</button>
                    </div>

                    <div onclick="openRecipe(0)" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 flex gap-5 items-center card-hover cursor-pointer">
                         <div class="w-16 h-16 bg-orange-100 rounded-2xl flex items-center justify-center text-3xl shadow-inner">🥣</div>
                         <div class="flex-1">
                             <h4 class="font-bold text-slate-800 text-base mb-1">${STORE.lang === 'ta' ? 'ராகி கூழ்' : 'Ragi Porridge'}</h4>
                             <p class="text-xs text-slate-500 line-clamp-1 font-medium">${STORE.lang === 'ta' ? 'இரும்புச்சத்து நிறைந்த முதல் உணவு' : 'Iron-rich first food'}</p>
                         </div>
                         <div class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i data-lucide="chevron-right" class="w-5 h-5 text-slate-400"></i></div>
                    </div>

                    <div onclick="switchTab('feedback')" class="bg-amber-50 border border-amber-100 rounded-3xl p-5 flex gap-4 items-center card-hover cursor-pointer">
                        <div class="bg-amber-100 p-2 rounded-xl"><i data-lucide="star" class="text-amber-600 w-5 h-5 fill-current"></i></div>
                        <div>
                            <h3 class="font-bold text-amber-900 text-sm">${t('feedback_card_title')}</h3>
                            <p class="text-xs text-amber-700/80 font-bold mt-0.5">${t('feedback_card_desc')}</p>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderTracker(container) {
            const options = t('food_options').map(f => `<option>${f}</option>`).join('');
            container.innerHTML = `
                <div class="px-6 py-6">
                    <h2 class="text-2xl font-extrabold mb-6 text-slate-800 tracking-tight">Feeding Tracker</h2>
                    
                    <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100 h-72 relative mb-6">
                        <div class="flex justify-between items-center mb-4">
                            <h3 class="text-sm font-bold text-slate-500 uppercase tracking-wide">Weekly Overview</h3>
                            <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                        </div>
                        <div class="h-48">
                             <canvas id="trackerChart"></canvas>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-3xl shadow-lg shadow-slate-200/50 border border-slate-100">
                        <div class="mb-5">
                            <label class="block text-xs font-bold text-slate-400 uppercase tracking-wider mb-2 ml-1">${t('track_food_label')}</label>
                            <div class="relative">
                                <select id="food-select" class="w-full bg-slate-50 border-0 rounded-2xl px-5 py-4 text-sm font-bold text-slate-700 outline-none focus:ring-2 focus:ring-primary-500 focus:bg-white transition-all appearance-none">${options}</select>
                                <div class="absolute right-4 top-4 text-slate-400 pointer-events-none"><i data-lucide="chevron-down" class="w-5 h-5"></i></div>
                            </div>
                        </div>

                        <div class="mb-6">
                            <label class="block text-xs font-bold text-slate-400 uppercase tracking-wider mb-2 ml-1">${t('track_reaction')}</label>
                            <div class="grid grid-cols-3 gap-3">
                                <button onclick="selectReaction(this, 'loved')" class="reaction-btn group p-3 bg-slate-50 rounded-2xl border border-transparent hover:bg-emerald-50 hover:border-emerald-200 transition-all">
                                    <div class="text-2xl mb-1 group-hover:scale-110 transition-transform">😋</div>
                                    <span class="text-[10px] font-bold text-slate-500 group-hover:text-emerald-600 uppercase">${t('reaction_loved')}</span>
                                </button>
                                <button onclick="selectReaction(this, 'ok')" class="reaction-btn group p-3 bg-slate-50 rounded-2xl border border-transparent hover:bg-amber-50 hover:border-amber-200 transition-all">
                                    <div class="text-2xl mb-1 group-hover:scale-110 transition-transform">😐</div>
                                    <span class="text-[10px] font-bold text-slate-500 group-hover:text-amber-600 uppercase">${t('reaction_ok')}</span>
                                </button>
                                <button onclick="selectReaction(this, 'refused')" class="reaction-btn group p-3 bg-slate-50 rounded-2xl border border-transparent hover:bg-rose-50 hover:border-rose-200 transition-all">
                                    <div class="text-2xl mb-1 group-hover:scale-110 transition-transform">🤢</div>
                                    <span class="text-[10px] font-bold text-slate-500 group-hover:text-rose-600 uppercase">${t('reaction_refused')}</span>
                                </button>
                            </div>
                        </div>
                        
                        <button onclick="addLog()" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-bold text-sm shadow-xl shadow-slate-900/20 active:scale-[0.98] transition-all hover:bg-slate-800 flex items-center justify-center gap-2">
                            <span>${t('track_save')}</span> <i data-lucide="check" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>
            `;
            setTimeout(renderChart, 100);
        }

        // --- CHART LOGIC ---
        function renderChart() {
            const ctx = document.getElementById('trackerChart');
            if(!ctx) return;
            if(STORE.chartInstance) STORE.chartInstance.destroy();

            const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
            const today = new Date();
            const labels = [];
            const solidData = [0,0,0,0,0,0,0];
            const milkData = [0,0,0,0,0,0,0];
            
            for(let i=6; i>=0; i--) {
                const d = new Date();
                d.setDate(today.getDate() - i);
                labels.push(days[d.getDay()]);
            }

            STORE.feedLogs.forEach(log => {
                const logDate = new Date(log.timestamp);
                const diffTime = Math.abs(today - logDate);
                const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24)); 
                if(diffDays < 7) {
                    const idx = 6 - diffDays;
                    if(idx >= 0) {
                        if(log.type === 'solid') solidData[idx]++;
                        else milkData[idx]++;
                    }
                }
            });

            STORE.chartInstance = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        {label: t('chart_label_solid'), data: solidData, backgroundColor: '#4f46e5', borderRadius: 4, barThickness: 12}, 
                        {label: t('chart_label_milk'), data: milkData, backgroundColor: '#e0e7ff', borderRadius: 4, barThickness: 12}
                    ]
                },
                options: { 
                    responsive: true, 
                    maintainAspectRatio: false, 
                    plugins: { 
                        legend: { position: 'bottom', labels: { usePointStyle: true, boxWidth: 6, font: {family: "'Plus Jakarta Sans'", size: 10} } } 
                    }, 
                    scales: { 
                        x: { grid: { display: false }, ticks: { font: {family: "'Plus Jakarta Sans'", size: 10} } }, 
                        y: { display: false, beginAtZero: true, suggestedMax: 5 } 
                    } 
                }
            });
        }

        let selectedReactionVal = 'ok';
        window.selectReaction = function(btn, val) {
            document.querySelectorAll('.reaction-btn').forEach(b => {
                b.classList.remove('ring-2', 'ring-primary-500', 'bg-primary-50', 'border-primary-200');
                b.classList.add('bg-slate-50', 'border-transparent');
            });
            btn.classList.remove('bg-slate-50', 'border-transparent');
            btn.classList.add('ring-2', 'ring-primary-500', 'bg-primary-50', 'border-primary-200');
            selectedReactionVal = val;
        }

        window.addLog = function() {
            const food = document.getElementById('food-select').value;
            const type = (food.includes('Milk') || food.includes('Formula') || food.includes('பால்')) ? 'milk' : 'solid';
            STORE.feedLogs.push({ timestamp: new Date().getTime(), food: food, type: type, reaction: selectedReactionVal });
            
            const btn = document.querySelector('button[onclick="addLog()"]');
            const originalHTML = btn.innerHTML;
            btn.innerHTML = `<span>Saved!</span>`;
            btn.classList.remove('bg-slate-900', 'text-white');
            btn.classList.add('bg-emerald-500', 'text-white');
            
            setTimeout(() => {
                btn.innerHTML = originalHTML;
                btn.classList.add('bg-slate-900', 'text-white');
                btn.classList.remove('bg-emerald-500');
                renderTracker(document.getElementById('main-content'));
            }, 800);
        }

        // --- GUIDE LOGIC ---
        function renderGuide(container) {
             container.innerHTML = `
                <div class="px-6 py-6">
                    <h2 class="text-2xl font-extrabold mb-6 text-slate-800 tracking-tight">Parent Guide</h2>
                    
                    <div class="flex p-1.5 bg-slate-200/60 rounded-2xl mb-8 overflow-x-auto relative">
                        <button onclick="showGuideSection('basics')" class="guide-tab flex-1 py-2.5 px-3 text-[11px] font-bold rounded-xl text-slate-600 transition-all tab-pill" id="gt-basics">${t('guide_tab1')}</button>
                        <button onclick="showGuideSection('foods')" class="guide-tab flex-1 py-2.5 px-3 text-[11px] font-bold rounded-xl text-slate-600 transition-all tab-pill" id="gt-foods">${t('guide_tab2')}</button>
                        <button onclick="showGuideSection('routine')" class="guide-tab flex-1 py-2.5 px-3 text-[11px] font-bold rounded-xl text-slate-600 transition-all tab-pill" id="gt-routine">${t('guide_tab3')}</button>
                        <button onclick="showGuideSection('care')" class="guide-tab flex-1 py-2.5 px-3 text-[11px] font-bold rounded-xl text-slate-600 transition-all tab-pill" id="gt-care">${t('guide_tab4')}</button>
                    </div>

                    <div id="gs-basics" class="guide-section space-y-5 animate-fade-in">
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <div class="flex items-center gap-4 mb-4">
                                <div class="p-3 bg-primary-100 rounded-2xl text-primary-600"><i data-lucide="baby" class="w-6 h-6"></i></div>
                                <h3 class="font-bold text-slate-800 text-lg">What is Weaning?</h3>
                            </div>
                            <p class="text-sm text-slate-600 leading-7 font-medium">
                                Weaning (Complementary Feeding) is starting solid foods at 6 months while continuing breastfeeding. It meets the growing baby's needs for energy, iron, and zinc.
                            </p>
                        </div>
                        
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <div class="flex items-center gap-4 mb-4">
                                <div class="p-3 bg-emerald-100 rounded-2xl text-emerald-600"><i data-lucide="check-circle-2" class="w-6 h-6"></i></div>
                                <h3 class="font-bold text-slate-800 text-lg">Signs of Readiness</h3>
                            </div>
                            <ul class="space-y-3 text-sm text-slate-600 font-medium">
                                <li class="flex gap-3 items-center"><span class="w-5 h-5 bg-emerald-100 rounded-full flex items-center justify-center text-emerald-600 text-[10px] font-bold">✓</span> Head & Neck control (steady head).</li>
                                <li class="flex gap-3 items-center"><span class="w-5 h-5 bg-emerald-100 rounded-full flex items-center justify-center text-emerald-600 text-[10px] font-bold">✓</span> Sitting with little or no support.</li>
                                <li class="flex gap-3 items-center"><span class="w-5 h-5 bg-emerald-100 rounded-full flex items-center justify-center text-emerald-600 text-[10px] font-bold">✓</span> Opens mouth when food is offered.</li>
                                <li class="flex gap-3 items-center"><span class="w-5 h-5 bg-emerald-100 rounded-full flex items-center justify-center text-emerald-600 text-[10px] font-bold">✓</span> Swallows food rather than pushing it out.</li>
                            </ul>
                        </div>

                        <div class="bg-violet-50 p-6 rounded-3xl border border-violet-100">
                            <h3 class="font-bold text-violet-800 mb-3 text-lg">Hygiene Rules</h3>
                            <div class="space-y-2">
                                <p class="text-sm text-violet-700/80 font-medium flex gap-2"><span>1.</span> Wash hands with soap before feeding.</p>
                                <p class="text-sm text-violet-700/80 font-medium flex gap-2"><span>2.</span> Use clean cups/spoons. <b>NO BOTTLES.</b></p>
                                <p class="text-sm text-violet-700/80 font-medium flex gap-2"><span>3.</span> Cook fresh food. Use within 1-2 hours.</p>
                            </div>
                        </div>
                    </div>

                    <div id="gs-foods" class="guide-section hidden space-y-5 animate-fade-in">
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <h3 class="font-bold text-slate-800 mb-4 flex items-center gap-2"><i data-lucide="apple" class="w-5 h-5 text-rose-500"></i> Best First Foods</h3>
                            <div class="grid grid-cols-2 gap-3">
                                <div class="bg-orange-50 p-4 rounded-2xl text-center border border-orange-100"><span class="text-2xl block mb-2">🥣</span><span class="text-xs font-bold text-slate-700 uppercase tracking-wide">Thick Porridge</span></div>
                                <div class="bg-amber-50 p-4 rounded-2xl text-center border border-amber-100"><span class="text-2xl block mb-2">🥔</span><span class="text-xs font-bold text-slate-700 uppercase tracking-wide">Mashed Veg</span></div>
                                <div class="bg-emerald-50 p-4 rounded-2xl text-center border border-emerald-100"><span class="text-2xl block mb-2">🍌</span><span class="text-xs font-bold text-slate-700 uppercase tracking-wide">Mashed Fruit</span></div>
                                <div class="bg-blue-50 p-4 rounded-2xl text-center border border-blue-100"><span class="text-2xl block mb-2">🥚</span><span class="text-xs font-bold text-slate-700 uppercase tracking-wide">Egg Yolk</span></div>
                            </div>
                        </div>

                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <h3 class="font-bold text-slate-800 mb-2 flex gap-2 items-center"><i data-lucide="zap" class="w-5 h-5 text-amber-500 fill-current"></i> Energy Density</h3>
                            <p class="text-xs text-slate-400 font-bold uppercase tracking-wide mb-4">Babies have small stomachs. Food must be rich!</p>
                            <ul class="space-y-3 text-sm text-slate-600 font-medium">
                                <li class="bg-slate-50 p-3 rounded-xl border border-slate-100"><b>Thicken it:</b> Don't make watery soups. It should stay on the spoon.</li>
                                <li class="bg-slate-50 p-3 rounded-xl border border-slate-100"><b>Add Fat:</b> Mix 1 tsp Ghee, Oil, or Butter in every meal.</li>
                                <li class="bg-slate-50 p-3 rounded-xl border border-slate-100"><b>Add Sweet:</b> Jaggery or mashed fruit (in moderation).</li>
                            </ul>
                        </div>

                        <div class="bg-rose-50 p-6 rounded-3xl border border-rose-100">
                            <h3 class="font-bold text-rose-800 flex items-center gap-2 mb-4"><i data-lucide="alert-triangle" class="w-5 h-5"></i> Foods to Avoid (< 1 Year)</h3>
                             <div class="space-y-3">
                                <div class="flex gap-4 items-center bg-white p-3 rounded-2xl shadow-sm border border-rose-100"><span class="text-2xl">🍯</span><div><p class="font-bold text-sm text-slate-800">Honey</p><p class="text-[10px] font-bold text-slate-400 uppercase">Botulism risk</p></div></div>
                                <div class="flex gap-4 items-center bg-white p-3 rounded-2xl shadow-sm border border-rose-100"><span class="text-2xl">🐄</span><div><p class="font-bold text-sm text-slate-800">Cow's Milk</p><p class="text-[10px] font-bold text-slate-400 uppercase">Not as main drink</p></div></div>
                                <div class="flex gap-4 items-center bg-white p-3 rounded-2xl shadow-sm border border-rose-100"><span class="text-2xl">🧂</span><div><p class="font-bold text-sm text-slate-800">Salt & Sugar</p><p class="text-[10px] font-bold text-slate-400 uppercase">Bad for kidneys</p></div></div>
                                <div class="flex gap-4 items-center bg-white p-3 rounded-2xl shadow-sm border border-rose-100"><span class="text-2xl">🍇</span><div><p class="font-bold text-sm text-slate-800">Choking Hazards</p><p class="text-[10px] font-bold text-slate-400 uppercase">Nuts, popcorn, grapes</p></div></div>
                            </div>
                        </div>
                    </div>

                    <div id="gs-routine" class="guide-section hidden space-y-6 animate-fade-in pl-2">
                        <div class="flex gap-4">
                            <div class="flex flex-col items-center">
                                <div class="w-8 h-8 rounded-full bg-primary-100 border-4 border-white shadow-sm z-10 flex items-center justify-center text-[10px] font-bold text-primary-600">6m</div>
                                <div class="w-0.5 h-full bg-slate-200 -my-2"></div>
                            </div>
                            <div class="pb-8">
                                <h4 class="font-bold text-lg text-primary-700">6 - 8 Months</h4>
                                <div class="bg-white p-5 rounded-2xl mt-3 border border-slate-100 shadow-sm w-full">
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Frequency</p>
                                    <p class="text-sm text-slate-700 font-bold mb-3">Breastfeed + 2-3 Meals</p>
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Texture</p>
                                    <p class="text-sm text-slate-600">Thick porridge, well-mashed foods.</p>
                                </div>
                            </div>
                        </div>

                        <div class="flex gap-4">
                            <div class="flex flex-col items-center">
                                <div class="w-8 h-8 rounded-full bg-emerald-100 border-4 border-white shadow-sm z-10 flex items-center justify-center text-[10px] font-bold text-emerald-600">9m</div>
                                <div class="w-0.5 h-full bg-slate-200 -my-2"></div>
                            </div>
                            <div class="pb-8">
                                <h4 class="font-bold text-lg text-emerald-700">9 - 11 Months</h4>
                                <div class="bg-white p-5 rounded-2xl mt-3 border border-slate-100 shadow-sm w-full">
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Frequency</p>
                                    <p class="text-sm text-slate-700 font-bold mb-3">Breastfeed + 3 Meals + 1 Snack</p>
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Texture</p>
                                    <p class="text-sm text-slate-600">Finely chopped, lumpy, finger foods.</p>
                                </div>
                            </div>
                        </div>

                        <div class="flex gap-4">
                            <div class="flex flex-col items-center">
                                <div class="w-8 h-8 rounded-full bg-violet-100 border-4 border-white shadow-sm z-10 flex items-center justify-center text-[10px] font-bold text-violet-600">1y</div>
                            </div>
                            <div>
                                <h4 class="font-bold text-lg text-violet-700">12 - 24 Months</h4>
                                <div class="bg-white p-5 rounded-2xl mt-3 border border-slate-100 shadow-sm w-full">
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Frequency</p>
                                    <p class="text-sm text-slate-700 font-bold mb-3">Breastfeed + 3-4 Meals + 2 Snacks</p>
                                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Texture</p>
                                    <p class="text-sm text-slate-600">Family pot food (mashed/chopped).</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div id="gs-care" class="guide-section hidden space-y-5 animate-fade-in">
                        <div class="bg-gradient-to-br from-pink-500 to-rose-600 p-6 rounded-3xl text-white shadow-lg shadow-pink-500/20">
                            <h3 class="font-bold text-xl mb-3 flex items-center gap-2"><i data-lucide="heart-handshake" class="w-6 h-6"></i> Responsive Feeding</h3>
                            <p class="text-sm text-pink-50 leading-relaxed mb-4 font-medium opacity-90">
                                Feeding is not just about food, it's about love.
                            </p>
                            <div class="bg-white/10 p-4 rounded-2xl border border-white/10 backdrop-blur-sm">
                                <ul class="text-sm space-y-2 font-medium">
                                    <li class="flex gap-2"><span>•</span> Feed slowly and patiently.</li>
                                    <li class="flex gap-2"><span>•</span> Encourage, but <b>never force</b>.</li>
                                    <li class="flex gap-2"><span>•</span> Maintain eye contact.</li>
                                </ul>
                            </div>
                        </div>

                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <h3 class="font-bold text-slate-800 mb-3 flex items-center gap-2"><i data-lucide="thermometer" class="w-5 h-5 text-rose-500"></i> During Illness</h3>
                            <p class="text-xs font-bold text-slate-400 uppercase tracking-wide mb-3">Never stop feeding</p>
                            <ul class="text-sm text-slate-600 space-y-3 font-medium">
                                <li class="bg-slate-50 p-3 rounded-xl">Breastfeed more frequently.</li>
                                <li class="bg-slate-50 p-3 rounded-xl">Offer soft, favorite foods in small amounts.</li>
                                <li class="bg-slate-50 p-3 rounded-xl"><b>Recovery:</b> Give one extra meal daily for 1 week after illness for catch-up growth.</li>
                            </ul>
                        </div>
                    </div>
                </div>
            `;
            
            // Re-initialize icons for the new HTML
            lucide.createIcons();
            
            // Tab Logic
            window.showGuideSection = function(id) {
                document.querySelectorAll('.guide-section').forEach(el => el.classList.add('hidden'));
                document.getElementById('gs-' + id).classList.remove('hidden');
                
                document.querySelectorAll('.guide-tab').forEach(el => {
                    el.classList.remove('active', 'bg-white', 'text-primary-600', 'shadow-sm');
                    el.classList.add('text-slate-500');
                });
                
                const activeBtn = document.getElementById('gt-' + id);
                activeBtn.classList.add('active', 'bg-white', 'text-primary-600', 'shadow-sm');
                activeBtn.classList.remove('text-slate-500');
            }
            // Trigger default tab style
            showGuideSection('basics');
        }

        // --- DIET / RECIPE LOGIC ---
        function renderDiet(container) {
            container.innerHTML = `
                <div class="px-6 py-6">
                    <h2 class="text-2xl font-extrabold mb-6 text-slate-800 tracking-tight">${t('recipe_title')}</h2>
                    <div class="grid gap-4">
                        ${STORE.recipes.map((r, i) => `
                            <div onclick="openRecipe(${i})" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 flex gap-4 cursor-pointer card-hover group">
                                <div class="w-20 h-20 bg-orange-50 rounded-2xl flex items-center justify-center text-3xl shrink-0 group-hover:scale-105 transition-transform duration-300">🥣</div>
                                <div class="flex-1 py-1">
                                    <h3 class="font-bold text-slate-800 text-base mb-1">${STORE.lang === 'ta' ? r.title_ta : r.title_en}</h3>
                                    <p class="text-xs text-slate-500 mb-3 line-clamp-2 font-medium leading-relaxed">${STORE.lang === 'ta' ? r.desc_ta : r.desc_en}</p>
                                    <div class="flex items-center gap-3 text-[10px] font-bold text-slate-400 uppercase tracking-wide">
                                        <span class="flex items-center gap-1 bg-slate-50 px-2 py-1 rounded-md"><i data-lucide="clock" class="w-3 h-3"></i> ${r.prep}</span>
                                        <span class="flex items-center gap-1 bg-slate-50 px-2 py-1 rounded-md"><i data-lucide="flame" class="w-3 h-3"></i> ${r.cook}</span>
                                    </div>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
        }

        window.openRecipe = function(index) {
            const r = STORE.recipes[index];
            const title = STORE.lang === 'ta' ? r.title_ta : r.title_en;
            const ing = STORE.lang === 'ta' ? r.ing_ta : r.ing_en;
            const steps = STORE.lang === 'ta' ? r.steps_ta : r.steps_en;

            const html = `
                <div class="text-center mb-8">
                    <div class="w-24 h-24 bg-orange-100 rounded-3xl mx-auto flex items-center justify-center text-4xl mb-5 shadow-inner">🥣</div>
                    <h2 class="text-2xl font-extrabold text-slate-900 leading-tight mb-3">${title}</h2>
                    <div class="flex justify-center gap-3">
                        <span class="bg-slate-100 text-slate-600 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wide">${t('recipe_prep')}: ${r.prep}</span>
                        <span class="bg-slate-100 text-slate-600 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wide">${t('recipe_cook')}: ${r.cook}</span>
                    </div>
                </div>
                
                <div class="mb-8 bg-slate-50 p-6 rounded-3xl border border-slate-100">
                    <h3 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-4 border-b border-slate-200 pb-2">${t('recipe_ing')}</h3>
                    <ul class="space-y-3">
                        ${ing.map(item => `<li class="flex items-start gap-3 text-sm text-slate-700 font-medium"><div class="mt-1.5 w-1.5 h-1.5 bg-primary-500 rounded-full shrink-0"></div>${item}</li>`).join('')}
                    </ul>
                </div>

                <div>
                    <h3 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-4 border-b border-slate-200 pb-2">${t('recipe_steps')}</h3>
                    <div class="space-y-5">
                        ${steps.map((step, i) => `
                            <div class="flex gap-4">
                                <div class="w-8 h-8 rounded-xl bg-primary-600 text-white flex items-center justify-center text-xs font-bold shrink-0 shadow-lg shadow-primary-500/30">${i+1}</div>
                                <p class="text-sm text-slate-600 leading-relaxed mt-1 font-medium">${step}</p>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
            
            document.getElementById('modal-title').innerText = title;
            document.getElementById('modal-body').innerHTML = html;
            
            const overlay = document.getElementById('modal-overlay');
            const backdrop = document.getElementById('modal-backdrop');
            const panel = document.getElementById('modal-panel');
            
            overlay.classList.remove('hidden');
            // Trigger animation
            setTimeout(() => {
                backdrop.classList.remove('opacity-0');
                panel.classList.remove('translate-y-full');
            }, 10);
        }

        window.closeModal = function() {
            const overlay = document.getElementById('modal-overlay');
            const backdrop = document.getElementById('modal-backdrop');
            const panel = document.getElementById('modal-panel');
            
            backdrop.classList.add('opacity-0');
            panel.classList.add('translate-y-full');
            
            setTimeout(() => {
                overlay.classList.add('hidden');
            }, 300);
        }

        // --- CHAT LOGIC ---
        function renderChat(container) {
            const categories = ["Definitions", "Timing", "Principles", "Diet Plan", "Selection", "Schedule", "Special", "Hygiene"];
            
            container.innerHTML = `
                <div class="flex flex-col h-full bg-slate-50">
                    <div class="px-6 py-4 bg-white/90 backdrop-blur border-b border-slate-100 flex items-center gap-4 shadow-sm z-10 sticky top-0">
                        <div class="relative">
                            <div class="w-12 h-12 bg-primary-100 rounded-2xl flex items-center justify-center text-primary-600 overflow-hidden border-2 border-white shadow-sm">
                                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Nurse&backgroundColor=e0e7ff" alt="Nurse" class="w-full h-full">
                            </div>
                            <div class="absolute -bottom-1 -right-1 w-4 h-4 bg-emerald-500 rounded-full border-[3px] border-white"></div>
                        </div>
                        <div>
                            <h3 class="font-bold text-slate-800 text-base">Nurse Sharon</h3>
                            <p class="text-[10px] text-emerald-600 font-extrabold uppercase tracking-wide bg-emerald-50 px-2 py-0.5 rounded-md inline-block mt-0.5">Online</p>
                        </div>
                    </div>
                    
                    <div class="bg-white py-3 px-6 border-b border-slate-100 overflow-x-auto whitespace-nowrap hide-scrollbar shrink-0">
                        <div class="flex gap-2">
                           ${categories.map(c => `<button onclick="filterChatCategory('${c}')" class="category-chip px-5 py-2 bg-slate-50 border border-slate-200 rounded-full text-xs font-bold text-slate-600 hover:bg-primary-50 hover:text-primary-600 hover:border-primary-100 transition-all active:scale-95">${c}</button>`).join('')}
                        </div>
                    </div>

                    <div class="flex-1 overflow-y-auto p-6 space-y-6 scroll-area" id="chat-scroller">
                        ${STORE.chatHistory.map(msg => `
                            <div class="flex w-full ${msg.sender === 'user' ? 'justify-end' : 'justify-start'} animate-fade-in">
                                <div class="max-w-[85%] px-5 py-3.5 rounded-2xl text-sm font-medium leading-relaxed shadow-sm ${msg.sender === 'user' ? 'bg-primary-600 text-white rounded-tr-sm' : 'bg-white text-slate-700 border border-slate-100 rounded-tl-sm'}">
                                    ${msg.text}
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    
                    <div class="p-4 bg-white border-t border-slate-100 shrink-0 pb-6">
                        <div class="flex gap-3 bg-slate-100 p-1.5 rounded-[2rem]">
                            <input type="text" id="chat-input" placeholder="${t('chat_placeholder')}" class="flex-1 bg-transparent border-none px-4 py-3 text-sm font-medium text-slate-800 placeholder:text-slate-400 focus:ring-0 outline-none" onkeypress="handleEnter(event)">
                            <button onclick="sendUserMsg()" class="w-11 h-11 bg-primary-600 rounded-full flex items-center justify-center text-white shadow-md hover:bg-primary-700 transition-colors active:scale-95"><i data-lucide="send" class="w-5 h-5 ml-0.5"></i></button>
                        </div>
                    </div>
                </div>
            `;
            setTimeout(() => {
                const scroller = document.getElementById('chat-scroller');
                if(scroller) scroller.scrollTop = scroller.scrollHeight;
            }, 50);
        }

        window.handleEnter = function(e) {
            if(e.key === 'Enter') sendUserMsg();
        }

        window.filterChatCategory = function(cat) {
            const relevant = STORE.qa_db.filter(item => item.cat === cat);
            const listHtml = relevant.map(item => `
                <div onclick="askQuestion('${item.id}')" class="group p-4 bg-white border border-slate-100 rounded-2xl mb-3 cursor-pointer shadow-sm hover:border-primary-200 hover:shadow-md transition-all">
                    <p class="text-[10px] font-extrabold text-primary-500 mb-1 tracking-wider uppercase">${item.id.toUpperCase()}</p>
                    <p class="text-sm text-slate-700 font-bold group-hover:text-primary-700 transition-colors">${item.q}</p>
                </div>
            `).join('');
            
            STORE.chatHistory.push({ sender: 'bot', text: `<p class="font-bold text-slate-800 mb-3 text-sm">Category: ${cat}</p>${listHtml}` });
            renderChat(document.getElementById('main-content'));
        }

        window.askQuestion = function(id) {
            const item = STORE.qa_db.find(q => q.id === id);
            if(!item) return;

            STORE.chatHistory.push({ sender: 'user', text: item.q });
            renderChat(document.getElementById('main-content'));

            setTimeout(() => {
                STORE.chatHistory.push({ sender: 'bot', text: item.a });
                renderChat(document.getElementById('main-content'));
            }, 600);
        }

        window.sendUserMsg = function() {
            const input = document.getElementById('chat-input');
            const txt = input.value.trim().toLowerCase();
            if(!txt) return;

            STORE.chatHistory.push({ sender: 'user', text: input.value });
            input.value = "";
            renderChat(document.getElementById('main-content'));
            
            const matches = STORE.qa_db.filter(item => {
                return item.keys.some(k => txt.includes(k)) || item.q.toLowerCase().includes(txt);
            });

            setTimeout(() => {
                if(matches.length > 0) {
                    const listHtml = matches.map(item => `
                        <div onclick="askQuestion('${item.id}')" class="group p-4 bg-white border border-slate-100 rounded-2xl mb-3 cursor-pointer shadow-sm hover:border-primary-200 hover:shadow-md transition-all">
                            <p class="text-[10px] font-extrabold text-primary-500 mb-1 tracking-wider uppercase">${item.id.toUpperCase()}</p>
                            <p class="text-sm text-slate-700 font-bold group-hover:text-primary-700 transition-colors">${item.q}</p>
                        </div>
                    `).join('');
                    STORE.chatHistory.push({ sender: 'bot', text: `<p class="font-bold text-slate-800 mb-3 text-sm">Here is what I found:</p>${listHtml}` });
                } else {
                    STORE.chatHistory.push({ sender: 'bot', text: "I couldn't find a specific answer for that. Try keywords like 'Milk', 'Schedule', or tap a category above." });
                }
                renderChat(document.getElementById('main-content'));
            }, 600);
        }

        // --- IFRAMES (Quiz & Feedback) ---
        function renderQuiz(container) {
             container.innerHTML = `
                <div class="flex flex-col h-full bg-white">
                    <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-white z-10 sticky top-0 shrink-0">
                        <h2 class="text-xl font-bold text-slate-800">${t('quiz_title')}</h2>
                        <span class="text-[10px] font-bold text-slate-500 bg-slate-100 px-3 py-1 rounded-full uppercase tracking-widest">Google Form</span>
                    </div>
                    <div class="flex-1 relative w-full bg-slate-50 scroll-area">
                        <iframe src="https://forms.gle/EJhej4Tfivpa7vM19" class="absolute inset-0 w-full h-full border-0" frameborder="0" marginheight="0" marginwidth="0">Loading...</iframe>
                    </div>
                </div>
            `;
        }

        function renderFeedback(container) {
             container.innerHTML = `
                <div class="flex flex-col h-full bg-white">
                    <div class="px-6 py-4 border-b border-slate-100 flex items-center gap-4 bg-white z-10 sticky top-0 shrink-0">
                        <button onclick="switchTab('home')" class="p-2 bg-slate-100 rounded-full hover:bg-slate-200"><i data-lucide="arrow-left" class="w-5 h-5 text-slate-600"></i></button>
                        <h2 class="text-xl font-bold text-slate-800">${t('feedback_title')}</h2>
                    </div>
                    <div class="flex-1 relative w-full bg-slate-50 scroll-area">
                        <iframe src="https://forms.gle/dL4b5tVfx6A7T6vj9" class="absolute inset-0 w-full h-full border-0" frameborder="0" marginheight="0" marginwidth="0">Loading...</iframe>
                    </div>
                </div>
            `;
            lucide.createIcons();
        }

        // --- INIT ---
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            switchTab('home');
        });
    </script>
</body>
</html>
