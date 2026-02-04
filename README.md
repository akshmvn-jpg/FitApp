<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DesiFit AI | Indian Diet Tracker</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX" crossorigin="anonymous"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Outfit', sans-serif;
            background-color: #020617; /* Slate 950 */
            color: #e2e8f0;
        }
        .glass-panel {
            background: rgba(15, 23, 42, 0.6);
            border: 1px solid rgba(56, 189, 248, 0.1);
            backdrop-filter: blur(12px);
            border-radius: 1rem;
        }
        .nav-btn.active {
            background: rgba(14, 165, 233, 0.15);
            color: #38bdf8;
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }

        .status-pill {
            padding: 4px 12px;
            border-radius: 99px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
        }
    </style>
</head>
<body class="flex h-screen overflow-hidden selection:bg-blue-500 selection:text-white">

    <aside class="w-64 bg-slate-950 border-r border-slate-900 hidden md:flex flex-col p-6">
        <div class="flex items-center gap-3 mb-8">
            <div class="w-10 h-10 rounded-xl bg-blue-600 flex items-center justify-center shadow-lg shadow-blue-900/50">
                <span class="text-xl font-bold text-white">ॐ</span>
            </div>
            <div>
                <h1 class="font-bold text-lg text-white">DesiFit <span class="text-blue-400">AI</span></h1>
            </div>
        </div>
        <nav class="space-y-2 flex-1">
            <div id="btn-dashboard" class="nav-btn p-3 rounded-xl cursor-pointer flex gap-3 text-slate-400 hover:text-white" onclick="switchTab('dashboard')">📊 Dashboard</div>
            <div id="btn-calculator" class="nav-btn p-3 rounded-xl cursor-pointer flex gap-3 text-slate-400 hover:text-white" onclick="switchTab('calculator')">🧮 TDEE Calc</div>
            <div id="btn-foodlog" class="nav-btn p-3 rounded-xl cursor-pointer flex gap-3 text-slate-400 hover:text-white active" onclick="switchTab('foodlog')">🍛 Food Log</div>
        </nav>
        
        <div class="mt-auto glass-panel p-2 flex items-center justify-center min-h-[150px]">
            <span class="text-[10px] text-slate-600 uppercase">Ad Space</span>
        </div>
    </aside>

    <main class="flex-1 overflow-y-auto p-4 md:p-8 relative">
        
        <div class="w-full h-20 glass-panel mb-6 flex items-center justify-center overflow-hidden">
            <span class="text-[10px] text-slate-600 uppercase">Banner Ad</span>
        </div>

        <div id="tab-dashboard" class="hidden space-y-6">
            <div class="flex justify-between items-end">
                <h2 class="text-3xl font-bold text-white">Your Progress</h2>
                <div id="status-container">
                    <span id="status-badge" class="status-pill bg-slate-800 text-slate-400">Calculate TDEE First</span>
                </div>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="glass-panel p-6">
                    <div class="flex justify-between items-start mb-4">
                        <p class="text-slate-400">Daily Calorie Budget</p>
                        <span id="tdee-val" class="text-xs font-bold text-blue-400 bg-blue-400/10 px-2 py-1 rounded">Target: 0</span>
                    </div>
                    <p id="dash-cals" class="text-5xl font-bold text-white mt-2">0</p>
                    <p id="diff-msg" class="text-sm mt-2 text-slate-500">Log food to see difference</p>
                </div>

                <div class="glass-panel p-6 flex flex-col justify-center">
                    <p class="text-slate-400 mb-4">Macronutrient Breakdown</p>
                    <div class="space-y-4">
                        <div>
                            <div class="flex justify-between text-xs mb-1"><span>Carbs</span><span id="dash-c">0g</span></div>
                            <div class="w-full bg-slate-800 h-1.5 rounded-full"><div id="bar-c" class="bg-orange-400 h-full rounded-full w-0 transition-all"></div></div>
                        </div>
                        <div>
                            <div class="flex justify-between text-xs mb-1"><span>Protein</span><span id="dash-p">0g</span></div>
                            <div class="w-full bg-slate-800 h-1.5 rounded-full"><div id="bar-p" class="bg-blue-400 h-full rounded-full w-0 transition-all"></div></div>
                        </div>
                        <div>
                            <div class="flex justify-between text-xs mb-1"><span>Fats</span><span id="dash-f">0g</span></div>
                            <div class="w-full bg-slate-800 h-1.5 rounded-full"><div id="bar-f" class="bg-yellow-400 h-full rounded-full w-0 transition-all"></div></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="glass-panel p-6">
                <h3 class="font-bold mb-4">Today's Thali (Log)</h3>
                <div id="log-list" class="space-y-2 text-sm text-slate-400">
                    <p class="italic text-slate-600 text-center py-4">No items added today.</p>
                </div>
            </div>
        </div>

        <div id="tab-calculator" class="hidden space-y-6 max-w-xl mx-auto">
            <div class="glass-panel p-8">
                <h2 class="text-2xl font-bold text-white mb-4">TDEE Calculator</h2>
                <form id="tdee-form" class="space-y-4" onsubmit="calculateTDEE(event)">
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs text-slate-500 mb-1 ml-1 uppercase">Gender</label>
                            <select id="gender" class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800">
                                <option value="male">Male</option>
                                <option value="female">Female</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-500 mb-1 ml-1 uppercase">Age</label>
                            <input type="number" id="age" required class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800" placeholder="25">
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs text-slate-500 mb-1 ml-1 uppercase">Height (cm)</label>
                            <input type="number" id="height" required class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800" placeholder="175">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-500 mb-1 ml-1 uppercase">Weight (kg)</label>
                            <input type="number" id="weight" required class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800" placeholder="70">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-500 mb-1 ml-1 uppercase">Activity Level</label>
                        <select id="activity" class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800">
                            <option value="1.2">Sedentary (Office job)</option>
                            <option value="1.375">Light Activity (Exercise 1-3 days/week)</option>
                            <option value="1.55">Moderate Activity (Exercise 3-5 days/week)</option>
                            <option value="1.725">High Activity (Daily intense exercise)</option>
                        </select>
                    </div>
                    <button type="submit" class="w-full bg-blue-600 p-4 rounded-xl font-bold text-white shadow-lg shadow-blue-900/40 hover:bg-blue-500 transition">Save & Calculate Goal</button>
                </form>
            </div>
        </div>

        <div id="tab-foodlog" class="space-y-6 max-w-2xl mx-auto">
            <div class="glass-panel p-8">
                <h2 class="text-2xl font-bold text-white mb-6">Log Indian Meal</h2>

                <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 mb-4">
                    <label class="block text-xs text-blue-400 font-bold mb-2 uppercase tracking-wide">Select Item</label>
                    <select id="quick-food" class="w-full bg-slate-950 p-3 rounded-lg text-white outline-none border border-slate-700 focus:border-blue-500 transition" onchange="updateInputFields()">
                        <option value="">-- Choose Food --</option>
                        <optgroup label="Breads & Staples (Count)">
                            <option value='{"name":"Roti (Whole Wheat)","cals":104,"p":3,"c":20,"f":1,"unit":"pcs"}'>Roti / Chapati</option>
                            <option value='{"name":"Puri","cals":101,"p":2,"c":12,"f":5,"unit":"pcs"}'>Puri (Fried)</option>
                            <option value='{"name":"Idli","cals":39,"p":1,"c":8,"f":0,"unit":"pcs"}'>Idli</option>
                            <option value='{"name":"Samosa","cals":260,"p":4,"c":24,"f":17,"unit":"pcs"}'>Samosa</option>
                        </optgroup>
                        <optgroup label="Curries & Dals (Bowls)">
                            <option value='{"name":"Dal Tadka","cals":150,"p":7,"c":18,"f":6,"unit":"bowl"}'>Dal Tadka (1 Katori)</option>
                            <option value='{"name":"Paneer Butter Masala","cals":350,"p":12,"c":15,"f":25,"unit":"bowl"}'>Paneer Butter Masala</option>
                            <option value='{"name":"Rice (Cooked)","cals":130,"p":2,"c":28,"f":0.5,"unit":"bowl"}'>White Rice (1 Katori)</option>
                        </optgroup>
                        <optgroup label="Cooking Fats (Spoons)">
                            <option value='{"name":"Ghee","cals":45,"p":0,"c":0,"f":5,"unit":"tsp"}'>Desi Ghee (Teaspoon)</option>
                        </optgroup>
                    </select>
                </div>

                <div class="grid grid-cols-5 gap-3 mb-4">
                    <div class="col-span-2">
                        <label id="qty-label" class="block text-xs text-slate-500 mb-1 ml-1">Quantity</label>
                        <div class="relative">
                            <input type="number" id="input-qty" class="w-full bg-slate-900 p-3 rounded-xl text-white outline-none border border-slate-800" value="1" oninput="calculatePreview()">
                            <span id="unit-display" class="absolute right-3 top-3 text-xs text-slate-500 font-bold uppercase">--</span>
                        </div>
                    </div>
                    <div class="col-span-2">
                         <label class="block text-xs text-slate-500 mb-1 ml-1">Est. Weight (g)</label>
                         <input type="number" id="input-grams" class="w-full bg-slate-900 p-3 rounded-xl text-slate-400 border border-slate-800" readonly>
                    </div>
                    <div class="col-span-1 flex items-end">
                        <button onclick="addFood()" class="w-full h-[50px] bg-blue-600 rounded-xl text-white font-bold text-2xl shadow-lg shadow-blue-900/50 hover:bg-blue-500 transition">+</button>
                    </div>
                </div>

                <div class="flex justify-between items-center text-sm p-3 bg-slate-900/50 rounded-lg border border-slate-800/50">
                    <span class="text-slate-400">Preview:</span>
                    <span class="text-white font-mono"><span id="preview-cals" class="text-xl font-bold text-blue-400">0</span> kcal</span>
                </div>

                <div id="ai-box" class="hidden mt-4 p-4 rounded-xl bg-yellow-900/10 border border-yellow-500/20">
                    <p id="ai-msg" class="text-yellow-200 text-xs italic"></p>
                </div>
            </div>
        </div>
    </main>

    <script>
        // Load stored data or default
        let state = JSON.parse(localStorage.getItem('desiFitState')) || { 
            log: [], 
            tdee: 0,
            user: { weight: 0, height: 0, age: 0, gender: 'male', activity: 1.2 }
        };

        let currentSelection = null;

        function switchTab(id) {
            ['dashboard', 'calculator', 'foodlog'].forEach(t => {
                document.getElementById(`tab-${t}`).classList.add('hidden');
                document.getElementById(`btn-${t}`).classList.remove('active');
            });
            document.getElementById(`tab-${id}`).classList.remove('hidden');
            document.getElementById(`btn-${id}`).classList.add('active');
            if (id === 'dashboard') updateDashboard();
        }

        function calculateTDEE(e) {
            e.preventDefault();
            const g = document.getElementById('gender').value;
            const a = parseInt(document.getElementById('age').value);
            const h = parseInt(document.getElementById('height').value);
            const w = parseInt(document.getElementById('weight').value);
            const act = parseFloat(document.getElementById('activity').value);

            // Mifflin-St Jeor Equation
            let bmr = (10 * w) + (6.25 * h) - (5 * a);
            bmr += (g === 'male') ? 5 : -161;
            
            state.tdee = Math.round(bmr * act);
            state.user = { weight: w, height: h, age: a, gender: g, activity: act };
            
            saveAndSync();
            alert("Daily Goal Set to: " + state.tdee + " kcal");
            switchTab('dashboard');
        }

        function updateInputFields() {
            const select = document.getElementById('quick-food');
            if (!select.value) return;

            currentSelection = JSON.parse(select.value);
            document.getElementById('qty-label').textContent = currentSelection.unit === 'pcs' ? "How many pieces?" : "How many bowls?";
            document.getElementById('unit-display').textContent = currentSelection.unit;
            document.getElementById('input-qty').value = 1;
            calculatePreview();
        }

        function calculatePreview() {
            if (!currentSelection) return;
            const qty = parseFloat(document.getElementById('input-qty').value) || 0;
            document.getElementById('preview-cals').textContent = Math.round(currentSelection.cals * qty);
            
            let weightFactor = currentSelection.unit === 'pcs' ? 45 : (currentSelection.unit === 'tsp' ? 5 : 150);
            document.getElementById('input-grams').value = Math.round(qty * weightFactor);
        }

        function addFood() {
            if (!currentSelection) return;
            const qty = parseFloat(document.getElementById('input-qty').value) || 0;
            
            state.log.push({
                name: currentSelection.name,
                qty: qty,
                unit: currentSelection.unit,
                cals: Math.round(currentSelection.cals * qty),
                p: Math.round(currentSelection.p * qty),
                c: Math.round(currentSelection.c * qty),
                f: Math.round(currentSelection.f * qty)
            });

            saveAndSync();
            alert("Added to log!");
            switchTab('dashboard');
        }

        function updateDashboard() {
            const totals = state.log.reduce((acc, curr) => {
                acc.cals += curr.cals; acc.p += curr.p; acc.c += curr.c; acc.f += curr.f;
                return acc;
            }, {cals:0, p:0, c:0, f:0});

            document.getElementById('dash-cals').textContent = totals.cals;
            document.getElementById('dash-p').textContent = totals.p + 'g';
            document.getElementById('dash-c').textContent = totals.c + 'g';
            document.getElementById('dash-f').textContent = totals.f + 'g';
            document.getElementById('tdee-val').textContent = "Goal: " + state.tdee;

            // Update Progress Bars
            if (state.tdee > 0) {
                const targetP = Math.round(state.user.weight * 1.5); // 1.5g per kg
                document.getElementById('bar-p').style.width = Math.min((totals.p / targetP) * 100, 100) + '%';
                document.getElementById('bar-c').style.width = Math.min((totals.c / 250) * 100, 100) + '%';
                document.getElementById('bar-f').style.width = Math.min((totals.f / 70) * 100, 100) + '%';

                // Status Logic
                const badge = document.getElementById('status-badge');
                const diffMsg = document.getElementById('diff-msg');
                const diff = state.tdee - totals.cals;

                if (diff > 50) {
                    badge.className = "status-pill bg-green-500/20 text-green-400 border border-green-500/30";
                    badge.textContent = "Calorie Deficit";
                    diffMsg.textContent = `${diff} kcal remaining for today.`;
                } else if (diff < -50) {
                    badge.className = "status-pill bg-red-500/20 text-red-400 border border-red-500/30";
                    badge.textContent = "Calorie Surplus";
                    diffMsg.textContent = `You are ${Math.abs(diff)} kcal over your goal.`;
                } else {
                    badge.className = "status-pill bg-blue-500/20 text-blue-400 border border-blue-500/30";
                    badge.textContent = "Maintenance";
                    diffMsg.textContent = "Perfectly balanced with your TDEE.";
                }
            }

            // List Update
            const list = document.getElementById('log-list');
            if (state.log.length > 0) {
                list.innerHTML = state.log.map(i => `
                    <div class="flex justify-between border-b border-slate-800 pb-2">
                        <span>${i.name} <span class="text-[10px] text-slate-500 uppercase">x${i.qty}</span></span>
                        <span class="text-white font-bold">${i.cals} kcal</span>
                    </div>
                `).join('');
            }
        }

        function saveAndSync() {
            localStorage.setItem('desiFitState', JSON.stringify(state));
        }

        // Initialize to Food Log Tab for testing
        switchTab('foodlog');
    </script>
</body>
</html>
