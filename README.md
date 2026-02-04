<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FitTrack AI | Midnight Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX" crossorigin="anonymous"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #020617; /* Slate 950 */
            color: #f1f5f9;
        }

        .glass-card {
            background: rgba(15, 23, 42, 0.8); /* Slate 900 */
            border: 1px solid rgba(56, 189, 248, 0.1); /* Sky 400 */
            border-radius: 1.25rem;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
        }

        .glass-card:hover {
            border-color: rgba(56, 189, 248, 0.4);
            box-shadow: 0 0 20px rgba(56, 189, 248, 0.1);
        }

        .nav-item {
            display: flex;
            align-items: center;
            padding: 0.85rem 1rem;
            border-radius: 0.75rem;
            color: #94a3b8;
            transition: all 0.2s;
            cursor: pointer;
        }

        .nav-item:hover, .nav-item.active {
            background: rgba(56, 189, 248, 0.1);
            color: #38bdf8;
        }

        .blue-gradient {
            background: linear-gradient(135deg, #38bdf8 0%, #1d4ed8 100%);
        }

        .input-field {
            background: #0f172a;
            border: 1px solid #1e293b;
            color: white;
            border-radius: 0.75rem;
            padding: 0.75rem;
            width: 100%;
            outline: none;
        }

        .input-field:focus {
            border-color: #38bdf8;
            box-shadow: 0 0 0 2px rgba(56, 189, 248, 0.2);
        }

        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 10px; }
    </style>
</head>
<body class="flex h-screen overflow-hidden">

    <aside class="w-72 bg-slate-950 border-r border-slate-900 hidden md:flex flex-col p-6">
        <div class="flex items-center gap-3 mb-10 px-2">
            <div class="w-10 h-10 rounded-xl blue-gradient flex items-center justify-center shadow-lg shadow-blue-500/20">
                <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
            </div>
            <h1 class="text-xl font-bold tracking-tight text-white">FitTrack <span class="text-blue-400">AI</span></h1>
        </div>

        <nav class="space-y-2 flex-1">
            <div class="nav-item active" onclick="switchView('dashboard')">Dashboard</div>
            <div class="nav-item" onclick="switchView('workouts')">Workouts</div>
            <div class="nav-item" onclick="switchView('nutrition')">Nutrition</div>
            <div class="nav-item" onclick="switchView('profile')">TDEE Calc</div>
        </nav>

        <div class="mt-4 p-2 glass-card text-center overflow-hidden">
            <p class="text-[9px] text-slate-500 mb-2 uppercase">Sponsored</p>
            <ins class="adsbygoogle" style="display:block" data-ad-client="ca-pub-XXXXXXXXXXXXXXXX" data-ad-slot="YYYYYYYYYY" data-ad-format="auto"></ins>
            <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
        </div>
    </aside>

    <main class="flex-1 overflow-y-auto p-4 md:p-10 pb-24">
        
        <div class="mb-8 overflow-hidden rounded-xl border border-slate-900 bg-slate-900/50">
            <ins class="adsbygoogle" style="display:block" data-ad-client="ca-pub-XXXXXXXXXXXXXXXX" data-ad-slot="ZZZZZZZZZZ" data-ad-format="horizontal"></ins>
            <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
        </div>

        <div id="view-dashboard" class="space-y-8">
            <header>
                <h2 class="text-4xl font-bold text-white">Your Stats</h2>
                <p class="text-slate-400">Keep pushing towards your goals.</p>
            </header>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="glass-card p-6">
                    <p class="text-sm text-slate-400 uppercase">Burned Today</p>
                    <p id="stat-burned" class="text-4xl font-bold text-white mt-2">0</p>
                    <div class="mt-4 h-1.5 w-full bg-slate-800 rounded-full"><div class="h-full bg-blue-500 w-2/3 rounded-full"></div></div>
                </div>
                <div class="glass-card p-6">
                    <p class="text-sm text-slate-400 uppercase">Consumed Today</p>
                    <p id="stat-consumed" class="text-4xl font-bold text-white mt-2">0</p>
                    <div class="mt-4 h-1.5 w-full bg-slate-800 rounded-full"><div class="h-full bg-cyan-400 w-1/3 rounded-full"></div></div>
                </div>
                <div class="glass-card p-6 border-blue-500/30">
                    <p class="text-sm text-blue-400 uppercase font-bold">Net Balance</p>
                    <p id="stat-balance" class="text-4xl font-bold text-white mt-2">--</p>
                    <p id="stat-balance-desc" class="text-xs mt-2 text-slate-500">Calculate TDEE to see balance</p>
                </div>
            </div>

            <div class="glass-card p-8">
                <h3 class="text-xl font-bold mb-6">Recent Activity</h3>
                <div id="activity-list" class="space-y-4">
                    <p class="text-slate-500 text-sm">No activity logged yet.</p>
                </div>
            </div>
        </div>

        <div id="view-workouts" class="hidden max-w-2xl mx-auto space-y-6">
            <div class="glass-card p-8">
                <h3 class="text-2xl font-bold mb-6">Log Workout</h3>
                <form id="workout-form" class="space-y-4">
                    <input type="text" id="w-name" class="input-field" placeholder="Exercise (e.g., Squats)" required>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="number" id="w-sets" class="input-field" placeholder="Sets" required>
                        <input type="number" id="w-reps" class="input-field" placeholder="Reps" required>
                    </div>
                    <button type="submit" class="w-full blue-gradient text-white font-bold py-3 rounded-xl">Save Workout</button>
                </form>
            </div>
        </div>

        <div id="view-nutrition" class="hidden max-w-2xl mx-auto space-y-6">
            <div class="glass-card p-8 border-cyan-500/20">
                <h3 class="text-2xl font-bold mb-6">AI Nutrition Log</h3>
                <form id="food-form" class="space-y-4">
                    <input type="text" id="f-item" class="input-field" placeholder="What did you eat? (e.g., 2 Eggs)" required>
                    <input type="number" id="f-weight" class="input-field" placeholder="Weight in grams (optional)">
                    <button type="submit" id="f-btn" class="w-full bg-cyan-500 text-slate-950 font-bold py-3 rounded-xl">Analyze with AI</button>
                </form>
                <div id="ai-result" class="hidden mt-6 p-4 bg-slate-950 rounded-xl border border-slate-800">
                    </div>
            </div>
        </div>

        <div id="view-profile" class="hidden max-w-2xl mx-auto space-y-6">
            <div class="glass-card p-8">
                <h3 class="text-2xl font-bold mb-6">TDEE Calculator</h3>
                <form id="tdee-form" class="space-y-4">
                    <select id="t-gender" class="input-field"><option value="male">Male</option><option value="female">Female</option></select>
                    <div class="grid grid-cols-3 gap-4">
                        <input type="number" id="t-age" class="input-field" placeholder="Age" required>
                        <input type="number" id="t-height" class="input-field" placeholder="Height (cm)" required>
                        <input type="number" id="t-weight" class="input-field" placeholder="Weight (kg)" required>
                    </div>
                    <select id="t-activity" class="input-field">
                        <option value="1.2">Sedentary</option>
                        <option value="1.55">Moderate</option>
                        <option value="1.9">Extreme</option>
                    </select>
                    <button type="submit" class="w-full blue-gradient text-white font-bold py-3 rounded-xl">Calculate Daily Goal</button>
                </form>
            </div>
        </div>

    </main>

    <script>
        let state = JSON.parse(localStorage.getItem('fittrack_data')) || { workouts: [], foods: [], tdee: 0 };

        function switchView(view) {
            ['dashboard', 'workouts', 'nutrition', 'profile'].forEach(v => document.getElementById(`view-${v}`).classList.add('hidden'));
            document.getElementById(`view-${view}`).classList.remove('hidden');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            event.currentTarget.classList.add('active');
        }

        function updateDashboard() {
            const burned = state.workouts.length * 150; // Simple estimate
            const consumed = state.foods.reduce((s, f) => s + f.cals, 0);
            document.getElementById('stat-burned').textContent = burned;
            document.getElementById('stat-consumed').textContent = consumed;
            
            if(state.tdee > 0) {
                const balance = (state.tdee + burned) - consumed;
                document.getElementById('stat-balance').textContent = balance;
                document.getElementById('stat-balance-desc').textContent = `Target: ${state.tdee} kcal/day`;
            }

            const list = document.getElementById('activity-list');
            const recent = [...state.workouts, ...state.foods].slice(-5).reverse();
            list.innerHTML = recent.map(item => `
                <div class="flex justify-between items-center p-3 bg-slate-900/50 rounded-lg border border-slate-800">
                    <span>${item.name || item.item}</span>
                    <span class="text-blue-400 font-bold">${item.sets ? item.sets+' sets' : item.cals+' kcal'}</span>
                </div>
            `).join('');
            
            localStorage.setItem('fittrack_data', JSON.stringify(state));
        }

        document.getElementById('workout-form').onsubmit = (e) => {
            e.preventDefault();
            state.workouts.push({ name: document.getElementById('w-name').value, sets: document.getElementById('w-sets').value });
            updateDashboard();
            e.target.reset();
            alert("Workout Logged!");
        };

        document.getElementById('tdee-form').onsubmit = (e) => {
            e.preventDefault();
            const w = document.getElementById('t-weight').value;
            const h = document.getElementById('t-height').value;
            const a = document.getElementById('t-age').value;
            const bmr = (10 * w) + (6.25 * h) - (5 * a) + 5;
            state.tdee = Math.round(bmr * document.getElementById('t-activity').value);
            updateDashboard();
            alert("Goal Updated: " + state.tdee + " kcal");
        };

        // Simplified AI Simulation (Since you need an API key for real Gemini)
        document.getElementById('food-form').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('f-btn');
            btn.textContent = "Analyzing...";
            
            // Simulating API response for demo - In production, use your fetch call here
            setTimeout(() => {
                const cals = Math.floor(Math.random() * 500) + 100;
                state.foods.push({ item: document.getElementById('f-item').value, cals });
                updateDashboard();
                btn.textContent = "Analyzed & Logged!";
                setTimeout(() => btn.textContent = "Analyze with AI", 2000);
                e.target.reset();
            }, 1000);
        };

        updateDashboard();
    </script>
</body>
</html>
