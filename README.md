<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>WeanWise - Professional Pediatric Nutrition</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&family=Inter:wght@400;500;600&family=Mukta+Malar:wght@400;500;600;700&display=swap" rel="stylesheet">

    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
            -webkit-tap-highlight-color: transparent;
        }
        body.lang-ta {
            font-family: 'Mukta Malar', sans-serif;
        }
        h1, h2, h3, h4, .font-heading {
            font-family: 'Poppins', sans-serif;
        }
        body.lang-ta h1, body.lang-ta h2, body.lang-ta .font-heading {
            font-family: 'Mukta Malar', sans-serif;
        }
        .hide-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .hide-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        .fade-in {
            animation: fadeIn 0.3s ease-out forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        @keyframes popIn {
            0% { transform: scale(0.9); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .chat-bubble {
            max-width: 85%;
            padding: 12px 16px;
            border-radius: 18px;
            font-size: 14px;
            line-height: 1.5;
            position: relative;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }
        .chat-user {
            background-color: #2563eb;
            color: white;
            border-bottom-right-radius: 4px;
            margin-left: auto;
        }
        .chat-bot {
            background-color: #ffffff;
            color: #1f2937;
            border-bottom-left-radius: 4px;
            border: 1px solid #e5e7eb;
        }
        .recipe-card {
            transition: transform 0.2s;
        }
        .recipe-card:active {
            transform: scale(0.98);
        }
        .safe-bottom {
            padding-bottom: env(safe-area-inset-bottom);
        }
        /* Chat specific enhancements */
        .category-chip {
            white-space: nowrap;
            transition: all 0.2s;
        }
        .suggestion-btn {
            text-align: left;
            transition: background-color 0.2s;
        }
        .suggestion-btn:active {
            background-color: #f3f4f6;
        }
    </style>
</head>
<body class="text-gray-800 h-screen flex justify-center overflow-hidden bg-gray-100">

    <!-- Mobile Container -->
    <div class="w-full max-w-md h-full bg-white relative shadow-2xl flex flex-col overflow-hidden sm:rounded-xl sm:my-4 sm:h-[95vh] sm:border border-gray-200">
        
        <!-- Header -->
        <header class="bg-white/95 backdrop-blur-sm pt-8 pb-3 px-6 shadow-sm z-20 flex justify-between items-center sticky top-0 border-b border-gray-100">
            <div class="flex items-center gap-3" onclick="switchTab('home')">
                <div class="w-10 h-10 bg-blue-600 rounded-xl flex items-center justify-center text-white shadow-lg shadow-blue-200 cursor-pointer">
                    <i data-lucide="baby" class="w-6 h-6"></i>
                </div>
                <div class="cursor-pointer">
                    <h1 class="text-lg font-bold text-gray-900 leading-none tracking-tight">WeanWise</h1>
                    <p id="header-subtitle" class="text-[10px] text-gray-500 font-semibold tracking-wide mt-0.5">PEDIATRIC NUTRITION</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="toggleLanguage()" id="lang-btn" class="w-9 h-9 rounded-full bg-gray-50 flex items-center justify-center text-xs font-bold text-gray-700 border border-gray-200 hover:bg-gray-100 active:scale-95 transition-transform">
                    TA
                </button>
            </div>
        </header>

        <!-- Main Content Area -->
        <main id="main-content" class="flex-1 overflow-y-auto hide-scrollbar pb-24 relative bg-gray-50/50">
            <!-- Dynamic Content -->
        </main>

        <!-- Bottom Navigation -->
        <nav class="absolute bottom-0 w-full bg-white/95 backdrop-blur-md border-t border-gray-200 px-1 py-2 pb-6 safe-bottom z-30 flex justify-around items-center shadow-[0_-4px_6px_-1px_rgba(0,0,0,0.05)]">
            <button onclick="switchTab('home')" class="nav-btn flex flex-col items-center gap-1 w-14 p-1 rounded-xl transition-all" data-tab="home">
                <i data-lucide="layout-grid" class="w-5 h-5"></i>
                <span class="text-[9px] font-bold mt-0.5" data-key="nav_home">Home</span>
            </button>
            <button onclick="switchTab('guide')" class="nav-btn flex flex-col items-center gap-1 w-14 p-1 rounded-xl transition-all" data-tab="guide">
                <i data-lucide="book-open" class="w-5 h-5"></i>
                <span class="text-[9px] font-bold mt-0.5" data-key="nav_guide">Guide</span>
            </button>
            <button onclick="switchTab('diet')" class="nav-btn flex flex-col items-center gap-1 w-14 p-1 rounded-xl transition-all" data-tab="diet">
                <i data-lucide="utensils" class="w-5 h-5"></i>
                <span class="text-[9px] font-bold mt-0.5" data-key="nav_diet">Diet</span>
            </button>
            <button onclick="switchTab('tracker')" class="nav-btn flex flex-col items-center gap-1 w-14 p-1 rounded-xl transition-all" data-tab="tracker">
                <i data-lucide="activity" class="w-5 h-5"></i>
                <span class="text-[9px] font-bold mt-0.5" data-key="nav_tracker">Tracker</span>
            </button>
            <button onclick="switchTab('quiz')" class="nav-btn flex flex-col items-center gap-1 w-14 p-1 rounded-xl transition-all" data-tab="quiz">
                <i data-lucide="clipboard-check" class="w-5 h-5"></i>
                <span class="text-[9px] font-bold mt-0.5" data-key="nav_quiz">Quiz</span>
            </button>
        </nav>

        <!-- Recipe Modal Overlay -->
        <div id="modal-overlay" class="fixed inset-0 bg-black/60 z-50 hidden flex-col justify-end backdrop-blur-sm transition-opacity opacity-0">
            <div class="bg-white rounded-t-[2rem] h-[85%] w-full transform translate-y-full transition-transform duration-300 ease-out flex flex-col shadow-2xl sm:max-w-md sm:mx-auto">
                <div class="p-5 border-b flex justify-between items-center bg-gray-50 rounded-t-[2rem]">
                    <h3 id="modal-title" class="font-bold text-lg text-gray-800 line-clamp-1">Details</h3>
                    <button onclick="closeModal()" class="p-2 bg-gray-200 rounded-full hover:bg-gray-300 transition-colors"><i data-lucide="x" class="w-5 h-5"></i></button>
                </div>
                <div id="modal-body" class="flex-1 overflow-y-auto p-6 space-y-4"></div>
            </div>
        </div>

    </div>

    <!-- Application Logic -->
    <script>
        // --- DATA STORE ---
        const STORE = {
            lang: 'en',
            translations: {
                en: {
                    subtitle: "PEDIATRIC NUTRITION",
                    nav_home: "Home", nav_tracker: "Tracker", nav_diet: "Recipes", nav_guide: "Guide", nav_chat: "Chat", nav_quiz: "Quiz",
                    welcome_title: "Baby-Led Weaning",
                    welcome_desc: "An alternative method where infants (6m+) self-feed whole foods instead of spoon-feeding.",
                    quick_tracker: "Log Meal", quick_tracker_desc: "Track solids & milk",
                    quick_chat: "Ask Expert", quick_chat_desc: "Chat with Nurse",
                    pop_recipe: "Popular Recipe", pop_recipe_desc: "Nutritious & easy to make",
                    feedback_card_title: "Research Feedback", feedback_card_desc: "Help our study by rating this app.",
                    guide_tab1: "Basics", guide_tab2: "Schedule", guide_tab3: "Principles", guide_tab4: "Red Flags",
                    red_flag_title: "Foods to Avoid",
                    schedule_6m: "6-8 Months", schedule_9m: "9-11 Months", schedule_1y: "1-2 Years",
                    track_food_label: "Food Item", track_reaction: "Reaction", track_save: "Save Log",
                    chat_placeholder: "Type keyword (e.g., Milk, Avoid)...",
                    qa_title: "Common Questions",
                    quiz_title: "Quiz", // Updated title
                    feedback_title: "Study Feedback", feedback_q1: "Was the app easy to use?", feedback_q2: "Did you feel more confident?",
                    feedback_submit: "Submit Survey", feedback_thanks: "Thank you for participating!",
                    chart_label_solid: "Solid Meals", chart_label_milk: "Milk Feeds",
                    reaction_loved: "Loved", reaction_ok: "Okay", reaction_refused: "Refused",
                    food_options: ["Breast Milk", "Formula", "Ragi Porridge", "Mashed Idli", "Dal Water", "Khichdi", "Mashed Banana", "Boiled Egg", "Family Pot Food"],
                    recipe_title: "Healthy Recipes", recipe_prep: "Prep time", recipe_cook: "Cook time", recipe_ing: "Ingredients", recipe_steps: "Instructions"
                },
                ta: {
                    subtitle: "குழந்தை ஊட்டச்சத்து",
                    nav_home: "முகப்பு", nav_tracker: "பதிவு", nav_diet: "சமையல்", nav_guide: "வழிகாட்டி", nav_chat: "உதவி", nav_quiz: "வினாடி",
                    welcome_title: "சுய உணவு முறை (BLW)",
                    welcome_desc: "குழந்தைகள் (6+ மாதம்) தாங்களாகவே கையில் எடுத்து உண்ணும் நவீன முறை.",
                    quick_tracker: "உணவு பதிவு", quick_tracker_desc: "உணவு விபரம்",
                    quick_chat: "நிபுணரிடம் கேள்", quick_chat_desc: "செவிலியர் உதவி",
                    pop_recipe: "பிரபலமான உணவு", pop_recipe_desc: "சத்தான மற்றும் எளிமையானது",
                    feedback_card_title: "ஆய்வு கருத்துக்கணிப்பு", feedback_card_desc: "உங்கள் கருத்துக்கள் எங்களுக்கு தேவை.",
                    guide_tab1: "அடிப்படை", guide_tab2: "அட்டவணை", guide_tab3: "விதிமுறைகள்", guide_tab4: "எச்சரிக்கை",
                    red_flag_title: "தவிர்க்க வேண்டியவை",
                    schedule_6m: "6-8 மாதங்கள்", schedule_9m: "9-11 மாதங்கள்", schedule_1y: "1-2 ஆண்டுகள்",
                    track_food_label: "உணவு வகை", track_reaction: "எதிர்வினை", track_save: "சேமிக்கவும்",
                    chat_placeholder: "தேடு (உதாரணம்: பால், தவிர்க்க)...",
                    qa_title: "அடிக்கடி கேட்கப்படும் கேள்விகள்",
                    quiz_title: "வினாடி வினா", // Updated title
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
                    title_en: "Ragi Porridge", title_ta: "ராகி கூழ்",
                    desc_en: "Iron-rich first food for babies.", desc_ta: "இரும்புச்சத்து நிறைந்த முதல் உணவு.",
                    prep: "5 min", cook: "10 min",
                    ing_en: ["2 tbsp Sprouted Ragi Flour", "1 cup Water", "1/2 tsp Ghee"],
                    ing_ta: ["2 மேசைக்கரண்டி ராகி மாவு", "1 கப் தண்ணீர்", "1/2 தேக்கரண்டி நெய்"],
                    steps_en: ["Mix ragi flour with water without lumps.", "Cook on low flame until it thickens.", "Add ghee and serve warm."],
                    steps_ta: ["ராகி மாவை தண்ணீரில் கட்டிகள் இல்லாமல் கலக்கவும்.", "குறைந்த தீயில் கெட்டியாகும் வரை சமைக்கவும்.", "நெய் சேர்த்து மிதமான சூட்டில் பரிமாறவும்."]
                },
                {
                    id: 2,
                    title_en: "Mashed Banana", title_ta: "வாழைப்பழம் மசியல்",
                    desc_en: "Easy to digest natural sweetness.", desc_ta: "எளிதில் ஜீரணமாகும் இயற்கை இனிப்பு.",
                    prep: "2 min", cook: "0 min",
                    ing_en: ["1 Ripe Banana", "Breast milk (optional)"],
                    ing_ta: ["1 பழுத்த வாழைப்பழம்", "தாய்ப்பால் (தேவைப்பட்டால்)"],
                    steps_en: ["Peel the banana.", "Mash it well with a fork.", "Add milk for thinner consistency if needed."],
                    steps_ta: ["வாழைப்பழத்தை உரித்துக்கொள்ளவும்.", "ஸ்பூன் கொண்டு நன்றாக மசிக்கவும்.", "தேவைப்பட்டால் பால் சேர்க்கவும்."]
                },
                {
                    id: 3,
                    title_en: "Dal Khichdi", title_ta: "பருப்பு கிச்சடி",
                    desc_en: "Complete meal with protein & carbs.", desc_ta: "புரதம் மற்றும் கார்போஹைட்ரேட் நிறைந்த உணவு.",
                    prep: "10 min", cook: "20 min",
                    ing_en: ["2 tbsp Rice", "1 tbsp Moong Dal", "Pinch of Turmeric", "1 cup Water"],
                    ing_ta: ["2 மேசைக்கரண்டி அரிசி", "1 மேசைக்கரண்டி பாசிப்பருப்பு", "சிட்டிகை மஞ்சள் தூள்", "1 கப் தண்ணீர்"],
                    steps_en: ["Wash rice and dal.", "Pressure cook with water and turmeric for 3-4 whistles.", "Mash well and add ghee."],
                    steps_ta: ["அரிசி மற்றும் பருப்பை கழுவவும்.", "தண்ணீர் மற்றும் மஞ்சள் தூள் சேர்த்து 3-4 விசிலுக்கு வேகவைக்கவும்.", "நன்றாக மசித்து நெய் சேர்க்கவும்."]
                }
            ],
            chatHistory: [
                { sender: 'bot', text_en: 'Hello! I am Nurse Sharon. Tap a category or type a keyword like "Milk" or "Schedule".', text_ta: 'வணக்கம்! நான் செவிலியர் ஷரோன். கீழே உள்ள வகையைத் தேர்ந்தெடுக்கவும் அல்லது "பால்" போன்ற சொல்லைத் தட்டச்சு செய்யவும்.' }
            ],
            // Comprehensive Q&A Data
            qa_db: [
                // Category 1: Definitions & Basics
                { id: "q1", cat: "Definitions", q: "What is the modern term used for \"Weaning\"?", a: "The term \"Weaning\" is now largely replaced by \"Complementary Feeding.\"", keys: ["weaning", "term", "definition"] },
                { id: "q2", cat: "Definitions", q: "What is the definition of Complementary Feeding?", a: "It is the process of introducing suitable semi-solid and solid foods to an infant at a developmentally appropriate age (6 months) to meet nutritional requirements while continuing breastfeeding.", keys: ["complementary", "feeding", "definition"] },
                { id: "q3", cat: "Definitions", q: "What is the primary goal of weaning?", a: "To provide a nutritional balance for proper growth and development and to gradually accustom the child to the \"family pot\" (normal family food) by the second year of life.", keys: ["goal", "aim", "why"] },
                { id: "q4", cat: "Definitions", q: "Does weaning mean stopping breastfeeding?", a: "No. Weaning is not the sudden withdrawal of breast milk. It is a gradual process where breast milk is supplemented with other foods. Breastfeeding should continue up to 2 years or beyond.", keys: ["stop", "breastfeeding", "milk", "withdrawal"] },
                { id: "q5", cat: "Definitions", q: "What is \"Baby-Led Weaning\" (BLW)?", a: "A method where infants (6 months+) are encouraged to self-feed \"whole\" soft pieces of food (finger foods) instead of being spoon-fed purees by a parent.", keys: ["blw", "baby-led", "self-feed"] },
                
                // Category 2: Timing & Rationale
                { id: "q6", cat: "Timing", q: "At what age should complementary feeding strictly begin?", a: "It should begin at 6 completed months (180 days) of age.", keys: ["age", "start", "begin", "when"] },
                { id: "q7", cat: "Timing", q: "Why is exclusive breastfeeding recommended for the first 0–6 months?", a: "Breast milk is nutritionally complete, aids the immune system, and the infant's digestive organs are not mature enough to tolerate other foods yet.", keys: ["exclusive", "0-6", "breastfeeding", "why"] },
                { id: "q8", cat: "Timing", q: "Why must complementary foods be introduced after 6 months?", a: "Breast milk alone is no longer sufficient to meet the growing baby's needs for energy, protein, iron, and zinc. The child’s body mass and activity levels increase significantly.", keys: ["after 6 months", "why", "sufficient"] },
                { id: "q9", cat: "Timing", q: "What are the risks of starting complementary feeding too late?", a: "Delayed introduction can lead to growth faltering (malnutrition), micronutrient deficiencies (iron/zinc), and the child may enter a \"critical period\" where they struggle to learn chewing and swallowing solids later.", keys: ["late", "delayed", "risk"] },
                { id: "q10", cat: "Timing", q: "What developmental milestones indicate a child is ready for solids?", a: "5 months: Biting movements appear.<br>6–7 months: Ability to swallow solids; good head/neck control.<br>8–12 months: Side-to-side tongue movements.", keys: ["milestone", "ready", "signs"] },

                // Category 3: Principles & Guidelines
                { id: "q11", cat: "Principles", q: "What are the \"Guiding Principles\" of complementary feeding?", a: "<b>Timely:</b> Start at 6 months.<br><b>Adequate:</b> Sufficient energy/nutrients.<br><b>Safe:</b> Hygienically prepared.<br><b>Properly Fed:</b> Responsive feeding (no force-feeding).", keys: ["principles", "guide"] },
                { id: "q12", cat: "Principles", q: "How should new foods be introduced?", a: "Introduce only one food at a time (interval of 4–5 days) to easily detect allergies. Start with small amounts (1–2 teaspoons).", keys: ["new", "introduce", "allergy"] },
                { id: "q13", cat: "Principles", q: "What is the correct progression of food consistency?", a: "Liquids → Semi-solids (pureed/mashed) → Solids (chopped/finger foods).", keys: ["consistency", "texture", "progression"] },
                { id: "q14", cat: "Principles", q: "Should water be given before 6 months?", a: "No. Even in hot climates, breast milk provides sufficient hydration. Water can be introduced in small amounts after 6 months.", keys: ["water", "drink"] },
                { id: "q15", cat: "Principles", q: "What is \"Responsive Feeding\"?", a: "Feeding with patience and love (Bahlaa ke, manaa ke khilao). It involves talking to the child, maintaining eye contact, encouraging them to eat, and never forcing them.", keys: ["responsive", "force", "love"] },

                // Category 4: Diet Planning
                { id: "q16", cat: "Diet Plan", q: "Why is \"Energy Density\" important for infant food?", a: "Children have small stomach capacities but high energy needs. They cannot eat large bulky quantities, so food must be calorie-rich in small volumes.", keys: ["energy", "density", "calorie"] },
                { id: "q17", cat: "Diet Plan", q: "How can you increase the energy density of a child's meal?", a: "<b>Add Fats:</b> Oil/ghee/butter.<br><b>Add Sweeteners:</b> Sugar/jaggery (moderation).<br><b>Thicken It:</b> Avoid watery gruels.<br><b>Amylase-Rich Foods:</b> Malted grains.", keys: ["increase", "energy", "fat", "ghee"] },
                { id: "q18", cat: "Diet Plan", q: "What is the ideal calorie distribution for a young child?", a: "Carbohydrates (55–60%), Proteins (10–12%), Fats (25–30%).", keys: ["calorie", "distribution", "fat", "protein"] },
                { id: "q19", cat: "Diet Plan", q: "Which nutrients are most critical to supplement after 6 months?", a: "Iron, Zinc, Vitamin A, and Calcium.", keys: ["nutrient", "iron", "zinc", "vitamin"] },

                // Category 5: Food Selection
                { id: "q20", cat: "Selection", q: "What are good \"First Foods\" to introduce at 6 months?", a: "Soft porridge (rice, wheat, ragi, suji), mashed potatoes, mashed banana/papaya, and thick dal with added ghee.", keys: ["first", "food", "start", "milk"] },
                { id: "q21", cat: "Selection", q: "Which foods should be strictly AVOIDED under 1 year?", a: "<b>Honey:</b> Risk of Botulism.<br><b>Cow’s Milk (as main drink):</b> Bleeding risk.<br><b>Salt/Sugar (Excess):</b> Kidney stress.", keys: ["avoid", "honey", "cow", "milk", "salt", "sugar"] },
                { id: "q22", cat: "Selection", q: "Which foods are choking hazards (avoid until 3 years)?", a: "Whole nuts, seeds, popcorn, hard candies, raw carrots, and whole grapes. (Nuts allowed if finely powdered).", keys: ["choking", "hazard", "avoid", "nut"] },
                { id: "q23", cat: "Selection", q: "Is it safe to give tea or coffee to infants?", a: "No. They inhibit iron absorption and offer no nutritional value.", keys: ["tea", "coffee", "caffeine", "avoid"] },
                { id: "q24", cat: "Selection", q: "Can I give fruit juices?", a: "Diluted fruit juice (1:10) is mentioned, but generally, whole mashed fruit is preferred. Avoid commercial sugary juices.", keys: ["juice", "fruit"] },

                // Category 6: Schedule
                { id: "q25", cat: "Schedule", q: "What is the feeding schedule for a 6–9 month old?", a: "<b>Freq:</b> Breastfeeding + 2–3 meals + 1 snack.<br><b>Amount:</b> 2–3 tbsp to ½ cup (125 ml).<br><b>Texture:</b> Mashed/semi-solid.", keys: ["schedule", "6-9"] },
                { id: "q26", cat: "Schedule", q: "What is the feeding schedule for a 9–12 month old?", a: "<b>Freq:</b> Breastfeeding + 3 meals + 1 snack.<br><b>Amount:</b> ½ to ¾ cup (at least 1 katori).<br><b>Texture:</b> Finely chopped, finger foods.", keys: ["schedule", "9-12"] },
                { id: "q27", cat: "Schedule", q: "What is the feeding schedule for a 12–24 month old?", a: "<b>Freq:</b> Breastfeeding + 3–4 meals + 2 snacks.<br><b>Amount:</b> ¾ to 1 full cup (250 ml).<br><b>Texture:</b> Family food.", keys: ["schedule", "12-24"] },

                // Category 7: Special Situations
                { id: "q28", cat: "Special", q: "How should I feed a child during illness (e.g., Diarrhea)?", a: "Never stop feeding. Continue breastfeeding frequently. Offer soft, favorite foods in small amounts. Increase fluid intake.", keys: ["illness", "sick", "diarrhea"] },
                { id: "q29", cat: "Special", q: "What is the feeding advice after an illness?", a: "Give one extra meal per day for a week to help the child catch up on lost weight (\"Catch-up growth\").", keys: ["after illness", "sick", "catch-up"] },
                { id: "q30", cat: "Special", q: "How do you feed a Low Birth Weight (LBW) baby (<1.5 kg)?", a: "They may require tube feeding or spoon-feeding of expressed breast milk.", keys: ["lbw", "low birth", "milk"] },
                { id: "q31", cat: "Special", q: "How do you feed an LBW baby (1.5–1.8 kg)?", a: "Cup and spoon feeding is preferred over bottles.", keys: ["lbw"] },
                { id: "q32", cat: "Special", q: "How is feeding managed in Severe Acute Malnutrition (SAM)?", a: "<b>Stabilization:</b> F-75 diet via cup/spoon.<br><b>Rehabilitation:</b> Switch to F-100 diet or energy-dense home foods.", keys: ["sam", "malnutrition", "milk", "illness", "sick"] },

                // Category 8: Hygiene & Safety
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
            document.body.classList.toggle('lang-ta', STORE.lang === 'ta');
            document.getElementById('lang-btn').innerText = STORE.lang === 'en' ? 'TA' : 'EN';
            
            // Re-render current tab
            const activeBtn = document.querySelector('.nav-btn.text-blue-600');
            const activeTab = activeBtn ? activeBtn.dataset.tab : 'home';
            switchTab(activeTab);
        }

        function switchTab(tabId) {
            // Update Nav UI
            document.querySelectorAll('.nav-btn').forEach(btn => {
                const icon = btn.querySelector('svg');
                if(btn.dataset.tab === tabId) {
                    btn.classList.add('text-blue-600');
                    btn.classList.remove('text-gray-400');
                    if(icon) icon.setAttribute('stroke-width', 2.5);
                } else {
                    btn.classList.remove('text-blue-600');
                    btn.classList.add('text-gray-400');
                    if(icon) icon.setAttribute('stroke-width', 1.5);
                }
            });

            // Update Header & Translations
            document.getElementById('header-subtitle').innerText = t('subtitle');
            document.querySelectorAll('[data-key]').forEach(el => el.innerText = t(el.dataset.key));

            // Main Content Container
            const main = document.getElementById('main-content');
            main.className = "flex-1 overflow-y-auto hide-scrollbar pb-24 relative bg-gray-50/50 fade-in";
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
                <div class="px-5 py-6 space-y-6">
                    <div class="bg-gradient-to-br from-blue-600 to-indigo-700 rounded-3xl p-6 text-white shadow-xl shadow-blue-200 relative overflow-hidden">
                        <i data-lucide="info" class="absolute top-4 right-4 text-white/20 w-12 h-12"></i>
                        <h2 class="text-xl font-bold mb-2 pr-8">${t('welcome_title')}</h2>
                        <p class="text-sm text-blue-50 leading-relaxed mb-4 opacity-90">${t('welcome_desc')}</p>
                        <button onclick="switchTab('guide')" class="bg-white/10 backdrop-blur-md border border-white/20 text-white px-4 py-2 rounded-full text-xs font-bold hover:bg-white/20 transition-colors">Start Reading</button>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div onclick="switchTab('tracker')" class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 active:scale-95 transition-transform cursor-pointer">
                            <div class="w-10 h-10 bg-green-50 rounded-full flex items-center justify-center text-green-600 mb-3"><i data-lucide="plus" class="w-5 h-5"></i></div>
                            <h3 class="font-bold text-gray-800 text-sm">${t('quick_tracker')}</h3>
                            <p class="text-xs text-gray-500 mt-1">${t('quick_tracker_desc')}</p>
                        </div>
                        <div onclick="switchTab('chat')" class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 active:scale-95 transition-transform cursor-pointer">
                            <div class="w-10 h-10 bg-purple-50 rounded-full flex items-center justify-center text-purple-600 mb-3"><i data-lucide="message-square" class="w-5 h-5"></i></div>
                            <h3 class="font-bold text-gray-800 text-sm">${t('quick_chat')}</h3>
                            <p class="text-xs text-gray-500 mt-1">${t('quick_chat_desc')}</p>
                        </div>
                    </div>

                    <div class="flex items-center justify-between mt-2 mb-2">
                         <h3 class="font-bold text-gray-800">${t('pop_recipe')}</h3>
                         <button onclick="switchTab('diet')" class="text-xs font-bold text-blue-600">View All</button>
                    </div>

                    <div onclick="openRecipe(0)" class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 flex gap-4 items-center active:scale-95 transition-transform cursor-pointer">
                         <div class="w-16 h-16 bg-orange-100 rounded-xl flex items-center justify-center text-2xl">🥣</div>
                         <div>
                             <h4 class="font-bold text-gray-800">${STORE.lang === 'ta' ? 'ராகி கூழ்' : 'Ragi Porridge'}</h4>
                             <p class="text-xs text-gray-500 mt-1 line-clamp-1">${STORE.lang === 'ta' ? 'இரும்புச்சத்து நிறைந்த முதல் உணவு' : 'Iron-rich first food'}</p>
                         </div>
                         <div class="ml-auto bg-gray-50 p-2 rounded-full"><i data-lucide="chevron-right" class="w-4 h-4 text-gray-400"></i></div>
                    </div>

                    <div onclick="switchTab('feedback')" class="bg-yellow-50 border border-yellow-100 rounded-xl p-4 flex gap-3 items-center active:scale-95 transition-transform cursor-pointer">
                        <i data-lucide="star" class="text-yellow-600 w-5 h-5 fill-current"></i>
                        <div>
                            <h3 class="font-bold text-yellow-800 text-sm">${t('feedback_card_title')}</h3>
                            <p class="text-xs text-yellow-700 font-medium">${t('feedback_card_desc')}</p>
                        </div>
                    </div>
                </div>
            `;
        }

        // --- GOOGLE FORM EMBED LOGIC (QUIZ) ---
        function renderQuiz(container) {
            const formUrl = "https://docs.google.com/forms/d/e/1FAIpQLSfD-T1u-h1lB8fT_vM5mJ-5y4q6r-9m-4z-3/viewform?embedded=true";
            // Note: I'm using the short link provided by you, but standard iframes prefer full embedded links.
            // Using direct link: https://forms.gle/EJhej4Tfivpa7vM19 
            
            container.innerHTML = `
                <div class="flex flex-col h-full bg-white">
                    <div class="px-5 py-4 border-b border-gray-100 flex justify-between items-center bg-white z-10 sticky top-0">
                        <h2 class="text-2xl font-bold text-gray-800 font-heading">${t('quiz_title')}</h2>
                        <span class="text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-1 rounded border border-gray-200 uppercase tracking-widest">Google Form</span>
                    </div>
                    <div class="flex-1 relative w-full bg-gray-50">
                        <iframe 
                            src="https://forms.gle/EJhej4Tfivpa7vM19" 
                            class="absolute inset-0 w-full h-full border-0" 
                            frameborder="0" 
                            marginheight="0" 
                            marginwidth="0">
                            Loading Form...
                        </iframe>
                    </div>
                </div>
            `;
            lucide.createIcons();
        }

        // --- GOOGLE FORM EMBED LOGIC (FEEDBACK) ---
        function renderFeedback(container) {
            const formUrl = "https://forms.gle/dL4b5tVfx6A7T6vj9";
            
            container.innerHTML = `
                <div class="flex flex-col h-full bg-white">
                    <div class="px-5 py-4 border-b border-gray-100 flex items-center gap-4 bg-white z-10 sticky top-0">
                        <button onclick="switchTab('home')" class="p-2 bg-gray-100 rounded-full hover:bg-gray-200"><i data-lucide="arrow-left" class="w-5 h-5 text-gray-600"></i></button>
                        <h2 class="text-xl font-bold text-gray-800 font-heading">${t('feedback_title')}</h2>
                    </div>
                    <div class="flex-1 relative w-full bg-gray-50">
                        <iframe 
                            src="${formUrl}" 
                            class="absolute inset-0 w-full h-full border-0" 
                            frameborder="0" 
                            marginheight="0" 
                            marginwidth="0">
                            Loading Feedback Form...
                        </iframe>
                    </div>
                </div>
            `;
            lucide.createIcons();
        }

        // --- DIET / RECIPE LOGIC ---
        function renderDiet(container) {
            container.innerHTML = `
                <div class="px-5 py-6">
                    <h2 class="text-2xl font-bold mb-6 text-gray-800 font-heading">${t('recipe_title')}</h2>
                    <div class="space-y-4">
                        ${STORE.recipes.map((r, i) => `
                            <div onclick="openRecipe(${i})" class="recipe-card bg-white p-4 rounded-2xl shadow-sm border border-gray-100 flex gap-4 cursor-pointer">
                                <div class="w-20 h-20 bg-orange-50 rounded-xl flex items-center justify-center text-3xl shrink-0">🥣</div>
                                <div class="flex-1">
                                    <h3 class="font-bold text-gray-800">${STORE.lang === 'ta' ? r.title_ta : r.title_en}</h3>
                                    <p class="text-xs text-gray-500 mt-1 mb-2 line-clamp-2">${STORE.lang === 'ta' ? r.desc_ta : r.desc_en}</p>
                                    <div class="flex items-center gap-3 text-[10px] font-bold text-gray-400">
                                        <span class="flex items-center gap-1"><i data-lucide="clock" class="w-3 h-3"></i> ${r.prep}</span>
                                        <span class="flex items-center gap-1"><i data-lucide="flame" class="w-3 h-3"></i> ${r.cook}</span>
                                    </div>
                                </div>
                                <div class="self-center"><i data-lucide="chevron-right" class="text-gray-300 w-5 h-5"></i></div>
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
                <div class="text-center mb-6">
                    <div class="w-24 h-24 bg-orange-100 rounded-full mx-auto flex items-center justify-center text-4xl mb-4 shadow-inner">🥣</div>
                    <h2 class="text-xl font-bold text-gray-900">${title}</h2>
                    <div class="flex justify-center gap-4 mt-3 text-xs font-bold text-gray-500">
                        <span class="bg-gray-100 px-3 py-1 rounded-full">${t('recipe_prep')}: ${r.prep}</span>
                        <span class="bg-gray-100 px-3 py-1 rounded-full">${t('recipe_cook')}: ${r.cook}</span>
                    </div>
                </div>
                
                <div class="mb-6">
                    <h3 class="text-sm font-bold text-gray-800 uppercase tracking-wide mb-3 border-b pb-2">${t('recipe_ing')}</h3>
                    <ul class="space-y-2">
                        ${ing.map(item => `<li class="flex items-start gap-2 text-sm text-gray-600"><span class="mt-1.5 w-1.5 h-1.5 bg-blue-400 rounded-full"></span>${item}</li>`).join('')}
                    </ul>
                </div>

                <div>
                    <h3 class="text-sm font-bold text-gray-800 uppercase tracking-wide mb-3 border-b pb-2">${t('recipe_steps')}</h3>
                    <div class="space-y-4">
                        ${steps.map((step, i) => `
                            <div class="flex gap-3">
                                <div class="w-6 h-6 rounded-full bg-blue-600 text-white flex items-center justify-center text-xs font-bold shrink-0 shadow-sm">${i+1}</div>
                                <p class="text-sm text-gray-600 leading-relaxed mt-0.5">${step}</p>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
            
            document.getElementById('modal-title').innerText = title;
            document.getElementById('modal-body').innerHTML = html;
            
            const overlay = document.getElementById('modal-overlay');
            const panel = overlay.querySelector('div');
            
            overlay.classList.remove('hidden');
            // Small timeout to allow display:block to apply before opacity transition
            setTimeout(() => {
                overlay.classList.remove('opacity-0');
                panel.classList.remove('translate-y-full');
            }, 10);
        }

        window.closeModal = function() {
            const overlay = document.getElementById('modal-overlay');
            const panel = overlay.querySelector('div');
            
            overlay.classList.add('opacity-0');
            panel.classList.add('translate-y-full');
            
            setTimeout(() => {
                overlay.classList.add('hidden');
            }, 300);
        }

        // --- TRACKER LOGIC ---
        function renderTracker(container) {
            const options = t('food_options').map(f => `<option>${f}</option>`).join('');
            container.innerHTML = `
                <div class="px-5 py-6">
                    <h2 class="text-2xl font-bold mb-6 text-gray-800 font-heading">Feeding Tracker</h2>
                    <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100 mb-6">
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-2">${t('track_food_label')}</label>
                        <select id="food-select" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-4 py-3 mb-4 text-sm font-semibold outline-none focus:ring-2 focus:ring-blue-500">${options}</select>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-2">${t('track_reaction')}</label>
                        <div class="grid grid-cols-3 gap-3 mb-6">
                            <button onclick="selectReaction(this, 'loved')" class="reaction-btn p-3 bg-gray-50 rounded-xl border border-gray-200 text-center hover:bg-green-50 transition-colors">😋<br><span class="text-[10px] font-bold">${t('reaction_loved')}</span></button>
                            <button onclick="selectReaction(this, 'ok')" class="reaction-btn p-3 bg-gray-50 rounded-xl border border-gray-200 text-center hover:bg-yellow-50 transition-colors">😐<br><span class="text-[10px] font-bold">${t('reaction_ok')}</span></button>
                            <button onclick="selectReaction(this, 'refused')" class="reaction-btn p-3 bg-gray-50 rounded-xl border border-gray-200 text-center hover:bg-red-50 transition-colors">🤢<br><span class="text-[10px] font-bold">${t('reaction_refused')}</span></button>
                        </div>
                        <button onclick="addLog()" class="w-full bg-gray-900 text-white py-3.5 rounded-xl font-bold shadow-lg active:scale-95 transition-transform">${t('track_save')}</button>
                    </div>
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 h-64 relative">
                        <canvas id="trackerChart"></canvas>
                    </div>
                </div>
            `;
            setTimeout(renderChart, 100);
        }

        function renderChart() {
            const ctx = document.getElementById('trackerChart');
            if(!ctx) return;

            // Destroy previous instance to prevent glitches
            if(STORE.chartInstance) {
                STORE.chartInstance.destroy();
            }

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
                        {label: t('chart_label_solid'), data: solidData, backgroundColor: '#2563eb', borderRadius: 4}, 
                        {label: t('chart_label_milk'), data: milkData, backgroundColor: '#dbeafe', borderRadius: 4}
                    ]
                },
                options: { 
                    responsive: true, 
                    maintainAspectRatio: false, 
                    plugins: { 
                        legend: { position: 'bottom', labels: { usePointStyle: true, boxWidth: 8 } } 
                    }, 
                    scales: { 
                        x: { grid: { display: false } }, 
                        y: { display: false, beginAtZero: true, suggestedMax: 5 } 
                    } 
                }
            });
        }

        let selectedReactionVal = 'ok';
        window.selectReaction = function(btn, val) {
            document.querySelectorAll('.reaction-btn').forEach(b => b.classList.remove('ring-2', 'ring-blue-500', 'bg-blue-50'));
            btn.classList.add('ring-2', 'ring-blue-500', 'bg-blue-50');
            selectedReactionVal = val;
        }

        window.addLog = function() {
            const food = document.getElementById('food-select').value;
            const type = (food.includes('Milk') || food.includes('Formula') || food.includes('பால்')) ? 'milk' : 'solid';
            STORE.feedLogs.push({ timestamp: new Date().getTime(), food: food, type: type, reaction: selectedReactionVal });
            
            // Animation for feedback
            const btn = document.querySelector('button[onclick="addLog()"]');
            const originalText = btn.innerText;
            btn.innerText = "✓ Saved";
            btn.classList.add('bg-green-600');
            setTimeout(() => {
                btn.innerText = originalText;
                btn.classList.remove('bg-green-600');
                renderTracker(document.getElementById('main-content'));
            }, 800);
        }

        // --- GUIDE LOGIC ---
        function renderGuide(container) {
             container.innerHTML = `
                <div class="px-5 py-6">
                    <h2 class="text-2xl font-bold mb-4 text-gray-800 font-heading">Complete Guide</h2>
                    <div class="flex p-1 bg-gray-200 rounded-xl mb-6">
                        <button onclick="showGuideSection('basics')" class="guide-tab flex-1 py-2 text-xs font-bold rounded-lg bg-white shadow-sm text-gray-800 transition-all" id="gt-basics">${t('guide_tab1')}</button>
                        <button onclick="showGuideSection('schedule')" class="guide-tab flex-1 py-2 text-xs font-bold rounded-lg text-gray-500 transition-all" id="gt-schedule">${t('guide_tab2')}</button>
                        <button onclick="showGuideSection('avoid')" class="guide-tab flex-1 py-2 text-xs font-bold rounded-lg text-gray-500 transition-all" id="gt-avoid">${t('guide_tab4')}</button>
                    </div>
                    <div id="gs-basics" class="guide-section space-y-4 animate-[fadeIn_0.3s_ease-out]">
                        <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100">
                            <h3 class="font-bold text-blue-600 mb-2">What is BLW?</h3>
                            <p class="text-sm text-gray-600 leading-relaxed">${STORE.lang === 'ta' ? 'சுய உணவு முறை (BLW) என்பது குழந்தைகள் 6 மாதத்திலிருந்து திட உணவுகளை தாங்களாகவே எடுத்து உண்ணும் முறையாகும்.' : 'Baby-led weaning (BLW) allows infants to feed themselves finger foods from the start of weaning (6 months).'}</p>
                        </div>
                        <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100">
                             <h3 class="font-bold text-gray-800 mb-3">Core Principles</h3>
                            <div class="space-y-3">
                                <div class="flex gap-3"><div class="w-8 h-8 bg-blue-50 rounded-full flex items-center justify-center text-blue-600 font-bold text-xs">1</div><div><h4 class="font-bold text-sm">Timely</h4><p class="text-xs text-gray-500">Start at 6 months.</p></div></div>
                                <div class="flex gap-3"><div class="w-8 h-8 bg-green-50 rounded-full flex items-center justify-center text-green-600 font-bold text-xs">2</div><div><h4 class="font-bold text-sm">Adequate</h4><p class="text-xs text-gray-500">Energy & Protein rich.</p></div></div>
                                <div class="flex gap-3"><div class="w-8 h-8 bg-red-50 rounded-full flex items-center justify-center text-red-600 font-bold text-xs">3</div><div><h4 class="font-bold text-sm">Safe</h4><p class="text-xs text-gray-500">Hygienic & No choking hazards.</p></div></div>
                            </div>
                        </div>
                    </div>
                    <div id="gs-schedule" class="guide-section hidden space-y-4 animate-[fadeIn_0.3s_ease-out]">
                        <div class="relative border-l-2 border-blue-100 ml-3 space-y-8 py-2">
                            <div class="relative pl-6"><span class="absolute -left-[9px] top-1 w-4 h-4 rounded-full bg-blue-500 border-2 border-white"></span><h4 class="font-bold text-gray-800">${t('schedule_6m')}</h4><div class="bg-white p-4 rounded-xl mt-2 border border-gray-100 shadow-sm"><p class="text-sm">Thick porridge / Mashed foods.</p><p class="text-xs text-gray-500 mt-1">2-3 meals/day</p></div></div>
                            <div class="relative pl-6"><span class="absolute -left-[9px] top-1 w-4 h-4 rounded-full bg-green-500 border-2 border-white"></span><h4 class="font-bold text-gray-800">${t('schedule_9m')}</h4><div class="bg-white p-4 rounded-xl mt-2 border border-gray-100 shadow-sm"><p class="text-sm">Finely chopped / Bite-sized.</p><p class="text-xs text-gray-500 mt-1">3 meals + 1 snack</p></div></div>
                        </div>
                    </div>
                    <div id="gs-avoid" class="guide-section hidden space-y-4 animate-[fadeIn_0.3s_ease-out]">
                        <div class="bg-red-50 p-5 rounded-2xl border border-red-100">
                            <h3 class="font-bold text-red-700 flex items-center gap-2 mb-4"><i data-lucide="alert-octagon" class="w-5 h-5"></i> ${t('red_flag_title')}</h3>
                             <ul class="space-y-3">
                                <li class="flex gap-3 items-start bg-white p-3 rounded-lg"><span class="text-xl">🍯</span><div><p class="font-bold text-sm text-gray-800">Honey / தேன்</p><p class="text-xs text-gray-500">Risk of Botulism.</p></div></li>
                                <li class="flex gap-3 items-start bg-white p-3 rounded-lg"><span class="text-xl">🍪</span><div><p class="font-bold text-sm text-gray-800">Biscuits / பிஸ்கட்</p><p class="text-xs text-gray-500">High sugar & preservatives.</p></div></li>
                                <li class="flex gap-3 items-start bg-white p-3 rounded-lg"><span class="text-xl">🍇</span><div><p class="font-bold text-sm text-gray-800">Whole Grapes / திராட்சை</p><p class="text-xs text-gray-500">Choking hazard.</p></div></li>
                            </ul>
                        </div>
                    </div>
                </div>
            `;
            window.showGuideSection = function(id) {
                document.querySelectorAll('.guide-section').forEach(el => el.classList.add('hidden'));
                document.getElementById('gs-' + id).classList.remove('hidden');
                document.querySelectorAll('.guide-tab').forEach(el => {
                    el.classList.remove('bg-white', 'shadow-sm', 'text-gray-800');
                    el.classList.add('text-gray-500');
                });
                document.getElementById('gt-' + id).classList.add('bg-white', 'shadow-sm', 'text-gray-800');
                document.getElementById('gt-' + id).classList.remove('text-gray-500');
            }
        }

        // --- CHAT LOGIC ---
        function renderChat(container) {
            const categories = ["Definitions", "Timing", "Principles", "Diet Plan", "Selection", "Schedule", "Special", "Hygiene"];
            
            container.innerHTML = `
                <div class="flex flex-col h-full">
                    <div class="px-5 py-4 bg-white border-b border-gray-100 flex items-center gap-3 shadow-sm z-10 sticky top-0">
                        <div class="relative"><div class="w-10 h-10 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 overflow-hidden"><img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Nurse" alt="Nurse" class="w-full h-full"></div><div class="absolute bottom-0 right-0 w-3 h-3 bg-green-500 rounded-full border-2 border-white"></div></div>
                        <div><h3 class="font-bold text-gray-800 text-sm">Nurse Sharon</h3><p class="text-[10px] text-green-600 font-bold uppercase tracking-wide">Online</p></div>
                    </div>
                    
                    <!-- Categories Scroll -->
                    <div class="bg-gray-50 py-3 px-5 border-b border-gray-100 overflow-x-auto whitespace-nowrap hide-scrollbar">
                        <div class="flex gap-2">
                           ${categories.map(c => `<button onclick="filterChatCategory('${c}')" class="category-chip px-4 py-1.5 bg-white border border-gray-200 rounded-full text-xs font-bold text-gray-600 shadow-sm hover:border-blue-400 hover:text-blue-600 active:scale-95">${c}</button>`).join('')}
                        </div>
                    </div>

                    <div class="flex-1 overflow-y-auto p-4 space-y-4 bg-gray-50/50" id="chat-scroller">
                        ${STORE.chatHistory.map(msg => `
                            <div class="flex w-full ${msg.sender === 'user' ? 'justify-end' : 'justify-start'} animate-[fadeIn_0.2s_ease-out]">
                                <div class="chat-bubble ${msg.sender === 'user' ? 'chat-user' : 'chat-bot'}">
                                    ${msg.text}
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    
                    <div class="p-4 bg-white border-t border-gray-100 safe-bottom">
                        <div class="flex gap-2">
                            <input type="text" id="chat-input" placeholder="${t('chat_placeholder')}" class="flex-1 bg-gray-100 border-none rounded-full px-5 py-3 text-sm focus:ring-2 focus:ring-blue-500 outline-none transition-all" onkeypress="handleEnter(event)">
                            <button onclick="sendUserMsg()" class="w-11 h-11 bg-blue-600 rounded-full flex items-center justify-center text-white shadow-md active:scale-95 transition-transform"><i data-lucide="send" class="w-5 h-5 ml-0.5"></i></button>
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
            const listHtml = relevant.map(item => `<div onclick="askQuestion('${item.id}')" class="suggestion-btn p-3 bg-white border border-gray-100 rounded-xl mb-2 cursor-pointer shadow-sm hover:border-blue-200"><p class="text-xs font-bold text-blue-600 mb-1">${item.id.toUpperCase()}</p><p class="text-sm text-gray-700">${item.q}</p></div>`).join('');
            
            STORE.chatHistory.push({ sender: 'bot', text: `<p class="font-bold text-gray-800 mb-2">Category: ${cat}</p>${listHtml}` });
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
            
            // Search Logic
            const matches = STORE.qa_db.filter(item => {
                return item.keys.some(k => txt.includes(k)) || item.q.toLowerCase().includes(txt);
            });

            setTimeout(() => {
                if(matches.length > 0) {
                    const listHtml = matches.map(item => `<div onclick="askQuestion('${item.id}')" class="suggestion-btn p-3 bg-white border border-gray-100 rounded-xl mb-2 cursor-pointer shadow-sm hover:border-blue-200"><p class="text-xs font-bold text-blue-600 mb-1">${item.id.toUpperCase()}</p><p class="text-sm text-gray-700">${item.q}</p></div>`).join('');
                    STORE.chatHistory.push({ sender: 'bot', text: `<p class="font-bold text-gray-800 mb-2">Found ${matches.length} related questions:</p>${listHtml}` });
                } else {
                    STORE.chatHistory.push({ sender: 'bot', text: "I couldn't find a specific answer for that. Try keywords like 'Milk', 'Schedule', or tap a category above." });
                }
                renderChat(document.getElementById('main-content'));
            }, 600);
        }

        // --- INIT ---
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            switchTab('home');
        });

    </script>
</body> 
</html>
