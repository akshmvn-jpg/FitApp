<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fitness Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #000000;
            color: #e4e4e7; /* zinc-200 */
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #000; }
        ::-webkit-scrollbar-thumb { background: #3f3f46; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #52525b; }

        /* Studia-like Card Style */
        .studia-card {
            background-color: #18181b; /* zinc-900 */
            border: 1px solid #27272a; /* zinc-800 */
            border-radius: 1rem;
            transition: all 0.2s ease;
        }
        .studia-card:hover {
            border-color: #3f3f46; /* zinc-700 */
            transform: translateY(-1px);
        }

        /* Nav Item Styles */
        .nav-item {
            display: flex;
            align-items: center;
            padding: 0.75rem 1rem;
            border-radius: 0.75rem;
            color: #a1a1aa; /* zinc-400 */
            font-weight: 500;
            transition: all 0.2s;
            cursor: pointer;
        }
        .nav-item:hover {
            background-color: #27272a;
            color: #fff;
        }
        .nav-item.active {
            background-color: #27272a;
            color: #fff;
        }
        
        /* Input Styles */
        .studia-input {
            background-color: #000;
            border: 1px solid #27272a;
            color: white;
            border-radius: 0.5rem;
            padding: 0.75rem;
            width: 100%;
            transition: border-color 0.2s;
        }
        .studia-input:focus {
            outline: none;
            border-color: #10b981; /* emerald-500 */
        }
    </style>
</head>
<body class="flex h-screen overflow-hidden">

    <!-- SIDEBAR -->
    <aside class="w-64 bg-black border-r border-zinc-800 hidden md:flex flex-col p-6">
        <div class="flex items-center gap-3 mb-10 px-2">
            <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-emerald-400 to-green-600"></div>
            <h1 class="text-xl font-bold tracking-tight text-white">FitTrack AI</h1>
        </div>

        <nav class="space-y-1 flex-1">
            <div class="nav-item active" onclick="switchView('dashboard')">
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path></svg>
                Dashboard
            </div>
            <div class="nav-item" onclick="switchView('workouts')">
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                Log Workout
            </div>
            <div class="nav-item" onclick="switchView('nutrition')">
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                Nutrition
            </div>
            <div class="nav-item" onclick="switchView('profile')">
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                Profile & TDEE
            </div>
        </nav>

        <div class="pt-6 border-t border-zinc-800">
            <div class="flex items-center gap-3 px-2">
                <div class="w-8 h-8 rounded-full bg-zinc-800 flex items-center justify-center text-xs font-bold text-zinc-400">ID</div>
                <div class="overflow-hidden">
                    <p class="text-sm font-medium text-white">User</p>
                    <p class="text-xs text-zinc-500 truncate" id="user-id-display">Loading...</p>
                </div>
            </div>
        </div>
    </aside>

    <!-- MOBILE NAV -->
    <div class="md:hidden fixed bottom-0 left-0 right-0 bg-black border-t border-zinc-800 z-50 flex justify-around p-3">
         <button onclick="switchView('dashboard')" class="p-2 text-zinc-400 hover:text-white"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path></svg></button>
         <button onclick="switchView('workouts')" class="p-2 text-zinc-400 hover:text-white"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg></button>
         <button onclick="switchView('nutrition')" class="p-2 text-zinc-400 hover:text-white"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg></button>
         <button onclick="switchView('profile')" class="p-2 text-zinc-400 hover:text-white"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg></button>
    </div>

    <!-- MAIN CONTENT -->
    <main class="flex-1 overflow-y-auto p-4 md:p-8 pb-24 md:pb-8">
        
        <!-- Header Section -->
        <div class="mb-8">
            <h2 class="text-3xl font-bold text-white mb-1">Welcome back</h2>
            <p class="text-zinc-500">Here's your daily fitness breakdown.</p>
        </div>

        <!-- DASHBOARD VIEW -->
        <div id="view-dashboard" class="space-y-6">
            <!-- Top Stats Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="studia-card p-5">
                    <p class="text-xs font-medium text-zinc-400 uppercase tracking-wide">Calories Burned</p>
                    <p id="stat-burned" class="text-3xl font-bold text-white mt-1">0</p>
                    <div class="mt-2 h-1 w-full bg-zinc-800 rounded-full overflow-hidden">
                        <div class="h-full bg-orange-500 w-3/4"></div>
                    </div>
                </div>
                <div class="studia-card p-5">
                    <p class="text-xs font-medium text-zinc-400 uppercase tracking-wide">Calories Consumed</p>
                    <p id="stat-consumed" class="text-3xl font-bold text-white mt-1">0</p>
                    <div class="mt-2 h-1 w-full bg-zinc-800 rounded-full overflow-hidden">
                        <div class="h-full bg-blue-500 w-1/2"></div>
                    </div>
                </div>
                <div class="studia-card p-5">
                    <p class="text-xs font-medium text-zinc-400 uppercase tracking-wide">Net Balance</p>
                    <p id="stat-balance" class="text-3xl font-bold text-white mt-1">0</p>
                    <p id="stat-balance-desc" class="text-xs text-zinc-500 mt-1">Maintenance</p>
                </div>
                <div class="studia-card p-5 border-emerald-900/30 bg-emerald-950/10">
                    <p class="text-xs font-medium text-emerald-400 uppercase tracking-wide">TDEE Goal</p>
                    <p id="stat-tdee" class="text-3xl font-bold text-white mt-1">--</p>
                </div>
            </div>

            <!-- Recent Activity Table -->
            <div class="studia-card p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-lg font-bold text-white">Recent Workouts</h3>
                    <button onclick="switchView('workouts')" class="text-sm text-emerald-400 hover:text-emerald-300">Log New +</button>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm text-zinc-400">
                        <thead class="text-xs uppercase text-zinc-500 border-b border-zinc-800">
                            <tr>
                                <th class="pb-3 pl-2">Date</th>
                                <th class="pb-3">Activity</th>
                                <th class="pb-3">Sets x Reps</th>
                                <th class="pb-3 text-right pr-2">Est. Burn</th>
                            </tr>
                        </thead>
                        <tbody id="dashboard-workout-list" class="divide-y divide-zinc-800">
                            <!-- Injected JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- WORKOUTS VIEW -->
        <div id="view-workouts" class="hidden space-y-6">
            <div class="studia-card p-6 max-w-2xl mx-auto">
                <h3 class="text-xl font-bold text-white mb-6">Log Workout</h3>
                <form id="workout-form" class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <div class="col-span-2">
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Activity Name</label>
                            <input type="text" id="w-activity" class="studia-input" placeholder="e.g. Bench Press" required>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Sets</label>
                            <input type="number" id="w-sets" class="studia-input" placeholder="3" required>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Reps</label>
                            <input type="number" id="w-reps" class="studia-input" placeholder="10" required>
                        </div>
                        <div class="col-span-2">
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Date</label>
                            <input type="date" id="w-date" class="studia-input" required>
                        </div>
                    </div>
                    <button type="submit" class="w-full bg-white text-black font-bold py-3 rounded-lg hover:bg-zinc-200 transition mt-4">Save Entry</button>
                </form>
            </div>
        </div>

        <!-- NUTRITION VIEW -->
        <div id="view-nutrition" class="hidden space-y-6">
            <div class="studia-card p-6 max-w-2xl mx-auto">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-xl font-bold text-white">AI Nutrition Log</h3>
                    <span class="text-xs bg-emerald-900 text-emerald-300 px-2 py-1 rounded">Gemini Powered</span>
                </div>
                
                <form id="food-form" class="space-y-4">
                     <div class="grid grid-cols-2 gap-4">
                        <div class="col-span-2">
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Food Item (Be specific)</label>
                            <input type="text" id="f-item" class="studia-input" placeholder="e.g. 100g Chicken Breast" required>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Weight (g)</label>
                            <input type="number" id="f-weight" class="studia-input" placeholder="100" required>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Date</label>
                            <input type="date" id="f-date" class="studia-input" required>
                        </div>
                    </div>
                    
                    <!-- AI Preview Box -->
                    <div id="ai-preview-box" class="hidden bg-zinc-900 border border-zinc-800 p-4 rounded-lg mt-4">
                        <p class="text-xs text-zinc-500 mb-2 uppercase tracking-wide">AI Estimation</p>
                        <div class="flex justify-between text-sm">
                            <div><span class="block text-xl font-bold text-white" id="disp-cals">0</span><span class="text-zinc-500">kcal</span></div>
                            <div><span class="block text-xl font-bold text-white" id="disp-prot">0</span><span class="text-zinc-500">prot</span></div>
                            <div><span class="block text-xl font-bold text-white" id="disp-fat">0</span><span class="text-zinc-500">fat</span></div>
                            <div><span class="block text-xl font-bold text-white" id="disp-carb">0</span><span class="text-zinc-500">carb</span></div>
                        </div>
                    </div>

                    <button type="submit" id="f-submit-btn" class="w-full bg-white text-black font-bold py-3 rounded-lg hover:bg-zinc-200 transition">
                        Get Estimate & Log
                    </button>
                </form>
            </div>
            
            <div class="studia-card p-6">
                <h3 class="text-lg font-bold text-white mb-4">Today's Meals</h3>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm text-zinc-400">
                        <thead class="text-xs uppercase text-zinc-500 border-b border-zinc-800">
                            <tr>
                                <th class="pb-3 pl-2">Item</th>
                                <th class="pb-3 text-right">Cals</th>
                                <th class="pb-3 text-right pr-2">Action</th>
                            </tr>
                        </thead>
                        <tbody id="nutrition-list" class="divide-y divide-zinc-800"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- PROFILE VIEW -->
        <div id="view-profile" class="hidden space-y-6">
            <div class="studia-card p-6 max-w-2xl mx-auto">
                <h3 class="text-xl font-bold text-white mb-6">TDEE Settings</h3>
                <form id="profile-form" class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Gender</label>
                            <select id="p-gender" class="studia-input">
                                <option value="male">Male</option>
                                <option value="female">Female</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Age</label>
                            <input type="number" id="p-age" class="studia-input" placeholder="25">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Height (cm)</label>
                            <input type="number" id="p-height" class="studia-input" placeholder="175">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Weight (kg)</label>
                            <input type="number" id="p-weight" class="studia-input" placeholder="70">
                        </div>
                        <div class="col-span-2">
                            <label class="block text-xs font-medium text-zinc-400 mb-1">Activity Level</label>
                            <select id="p-activity" class="studia-input">
                                <option value="1.2">Sedentary</option>
                                <option value="1.375">Light Activity</option>
                                <option value="1.55">Moderate Activity</option>
                                <option value="1.725">Very Active</option>
                                <option value="1.9">Extremely Active</option>
                            </select>
                        </div>
                    </div>
                    <button type="submit" class="w-full bg-emerald-600 text-white font-bold py-3 rounded-lg hover:bg-emerald-700 transition">Save & Calculate</button>
                </form>
            </div>
        </div>

    </main>

    <!-- Custom Modal -->
    <div id="custom-modal" class="fixed inset-0 bg-black/80 backdrop-blur-sm hidden items-center justify-center z-50">
        <div class="bg-zinc-900 border border-zinc-800 rounded-xl p-6 w-96 shadow-2xl">
            <h3 id="modal-title" class="text-lg font-bold text-white mb-2">Notification</h3>
            <p id="modal-message" class="text-zinc-400 mb-6 text-sm"></p>
            <div class="flex justify-end gap-3">
                <button id="modal-cancel-btn" class="px-4 py-2 bg-zinc-800 text-zinc-300 rounded-lg hover:bg-zinc-700 text-sm hidden">Cancel</button>
                <button id="modal-confirm-btn" class="px-4 py-2 bg-white text-black rounded-lg hover:bg-zinc-200 text-sm font-bold">OK</button>
            </div>
        </div>
    </div>

    <script>
        const STORAGE_KEY = 'studiaFitnessState';
        const API_KEY = ""; // 
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${API_KEY}`;
        
        let state = { workouts: [], foods: [], profile: {} };

        // --- View Switching ---
        window.switchView = (viewId) => {
            // Hide all views
            ['dashboard', 'workouts', 'nutrition', 'profile'].forEach(id => {
                document.getElementById(`view-${id}`).classList.add('hidden');
            });
            // Show selected
            document.getElementById(`view-${viewId}`).classList.remove('hidden');
            
            // Update Sidebar Active State
            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(el => el.classList.remove('active', 'bg-zinc-800', 'text-white'));
            // Find the clicked element (simplified logic for demo)
            const activeIndex = ['dashboard', 'workouts', 'nutrition', 'profile'].indexOf(viewId);
            if(navItems[activeIndex]) {
                navItems[activeIndex].classList.add('active', 'bg-zinc-800', 'text-white');
            }
        };

        // --- Data Handling ---
        function loadData() {
            const data = localStorage.getItem(STORAGE_KEY);
            if (data) state = JSON.parse(data);
            updateUI();
        }
        function saveData() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
            updateUI();
        }

        // --- UI Updates ---
        function updateUI() {
            // Stats
            const burned = state.workouts.reduce((sum, w) => sum + Math.round(w.sets * w.reps * 1.5), 0);
            const consumed = state.foods.reduce((sum, f) => sum + (f.calories || 0), 0);
            const tdee = state.profile.tdee || 0;
            const balance = (tdee + burned) - consumed;

            document.getElementById('stat-burned').textContent = burned.toLocaleString();
            document.getElementById('stat-consumed').textContent = consumed.toLocaleString();
            document.getElementById('stat-tdee').textContent = tdee ? tdee.toLocaleString() : '--';
            document.getElementById('stat-balance').textContent = tdee ? balance.toLocaleString() : '--';
            
            const balDesc = document.getElementById('stat-balance-desc');
            if(tdee) {
                if(balance < -200) { balDesc.textContent = "Deficit (Losing)"; balDesc.className = "text-xs text-red-400 mt-1"; }
                else if(balance > 200) { balDesc.textContent = "Surplus (Gaining)"; balDesc.className = "text-xs text-emerald-400 mt-1"; }
                else { balDesc.textContent = "Maintenance"; balDesc.className = "text-xs text-zinc-500 mt-1"; }
            }

            // Dashboard Table
            const dbList = document.getElementById('dashboard-workout-list');
            dbList.innerHTML = state.workouts.slice(-5).reverse().map(w => `
                <tr>
                    <td class="py-3 pl-2">${w.date}</td>
                    <td class="py-3 font-medium text-white">${w.activity}</td>
                    <td class="py-3">${w.sets} x ${w.reps}</td>
                    <td class="py-3 text-right pr-2 text-zinc-300">${Math.round(w.sets*w.reps*1.5)}</td>
                </tr>
            `).join('');

            // Nutrition List
            const nutList = document.getElementById('nutrition-list');
            nutList.innerHTML = state.foods.slice(-10).reverse().map(f => `
                <tr>
                    <td class="py-3 pl-2 font-medium text-white">${f.item} <span class="text-xs text-zinc-500">(${f.weight}g)</span></td>
                    <td class="py-3 text-right text-white">${f.calories}</td>
                    <td class="py-3 text-right pr-2"><button onclick="deleteFood('${f.id}')" class="text-xs text-red-500 hover:text-red-400">Del</button></td>
                </tr>
            `).join('');
            
            // Profile Inputs
            if(state.profile.gender) document.getElementById('p-gender').value = state.profile.gender;
            if(state.profile.age) document.getElementById('p-age').value = state.profile.age;
            if(state.profile.height) document.getElementById('p-height').value = state.profile.height;
            if(state.profile.weight) document.getElementById('p-weight').value = state.profile.weight;
            if(state.profile.activity) document.getElementById('p-activity').value = state.profile.activity;
        }

        // --- AI Logic ---
        async function getCaloriesFromLLM(item, weight) {
            const prompt = `Estimate nutrition for ${weight}g of ${item}. Return ONLY JSON: {"calories": int, "protein": int, "fat": int, "carbs": int}. Round to int.`;
            try {
                const res = await fetch(API_URL, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] })
                });
                const data = await res.json();
                const text = data.candidates[0].content.parts[0].text.replace(/```json|```/g, '').trim();
                return JSON.parse(text);
            } catch (e) {
                console.error(e);
                return null;
            }
        }

        // --- Event Listeners ---
        
        // Workout Submit
        document.getElementById('workout-form').addEventListener('submit', (e) => {
            e.preventDefault();
            state.workouts.push({
                id: crypto.randomUUID(),
                date: document.getElementById('w-date').value,
                activity: document.getElementById('w-activity').value,
                sets: parseInt(document.getElementById('w-sets').value),
                reps: parseInt(document.getElementById('w-reps').value)
            });
            saveData();
            e.target.reset();
            document.getElementById('w-date').value = new Date().toISOString().split('T')[0];
            showModal("Workout Logged Successfully!");
        });

        // Food Submit
        document.getElementById('food-form').addEventListener('submit', async (e) => {
            e.preventDefault();
            const btn = document.getElementById('f-submit-btn');
            const item = document.getElementById('f-item').value;
            const weight = document.getElementById('f-weight').value;
            
            btn.textContent = "AI Estimating...";
            btn.disabled = true;

            const macros = await getCaloriesFromLLM(item, weight);
            
            if(macros) {
                // Show Preview
                document.getElementById('ai-preview-box').classList.remove('hidden');
                document.getElementById('disp-cals').textContent = macros.calories;
                document.getElementById('disp-prot').textContent = macros.protein;
                document.getElementById('disp-fat').textContent = macros.fat;
                document.getElementById('disp-carb').textContent = macros.carbs;
                
                // Save
                state.foods.push({
                    id: crypto.randomUUID(),
                    date: document.getElementById('f-date').value,
                    item, weight, ...macros
                });
                saveData();
                e.target.reset();
                document.getElementById('f-date').value = new Date().toISOString().split('T')[0];
                btn.textContent = "Logged! Add Another";
            } else {
                showModal("AI Error. Please try again.", "Error");
            }
            btn.disabled = false;
        });
        
        // Profile Submit
        document.getElementById('profile-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const w = parseFloat(document.getElementById('p-weight').value);
            const h = parseFloat(document.getElementById('p-height').value);
            const a = parseFloat(document.getElementById('p-age').value);
            const g = document.getElementById('p-gender').value;
            const act = parseFloat(document.getElementById('p-activity').value);
            
            let bmr = (10 * w) + (6.25 * h) - (5 * a) + (g === 'male' ? 5 : -161);
            let tdee = Math.round(bmr * act);
            
            state.profile = { gender: g, age: a, height: h, weight: w, activity: act, tdee };
            saveData();
            showModal(`TDEE Updated: ${tdee} kcal/day`);
        });

        // Delete Functions
        window.deleteFood = (id) => {
            if(confirm("Delete this item?")) {
                state.foods = state.foods.filter(f => f.id !== id);
                saveData();
            }
        };

        // Modal Logic
        function showModal(msg, title="Success") {
            const m = document.getElementById('custom-modal');
            document.getElementById('modal-title').textContent = title;
            document.getElementById('modal-message').textContent = msg;
            document.getElementById('modal-cancel-btn').classList.add('hidden');
            document.getElementById('modal-confirm-btn').onclick = () => m.classList.add('hidden');
            m.classList.remove('hidden');
            m.classList.add('flex');
        }

        // Init
        document.getElementById('w-date').value = new Date().toISOString().split('T')[0];
        document.getElementById('f-date').value = new Date().toISOString().split('T')[0];
        loadData();
    </script>
</body>
</html>
