<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FitTrack AI | Midnight Edition</title>

<script src="https://cdn.tailwindcss.com"></script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700&display=swap');

body {
    font-family: 'Plus Jakarta Sans', sans-serif;
    background:#020617;
    color:#f1f5f9;
}

.glass-card {
    background:rgba(15,23,42,.85);
    border:1px solid rgba(56,189,248,.15);
    border-radius:1.25rem;
    backdrop-filter:blur(12px);
}

.nav-item {
    padding:.8rem 1rem;
    border-radius:.75rem;
    color:#94a3b8;
    cursor:pointer;
}

.nav-item.active, .nav-item:hover {
    background:rgba(56,189,248,.15);
    color:#38bdf8;
}

.input-field {
    width:100%;
    padding:.75rem;
    border-radius:.75rem;
    background:#0f172a;
    border:1px solid #1e293b;
    color:white;
}

.input-field:focus {
    outline:none;
    border-color:#38bdf8;
}

.blue-gradient {
    background:linear-gradient(135deg,#38bdf8,#1d4ed8);
}
</style>
</head>

<body class="flex h-screen overflow-hidden">

<!-- SIDEBAR -->
<aside class="w-72 bg-slate-950 border-r border-slate-900 hidden md:flex flex-col p-6">
    <h1 class="text-xl font-bold mb-8">FitTrack <span class="text-blue-400">AI</span></h1>

    <nav class="space-y-2 flex-1">
        <div class="nav-item active" onclick="switchView('dashboard',this)">Dashboard</div>
        <div class="nav-item" onclick="switchView('workouts',this)">Workouts</div>
        <div class="nav-item" onclick="switchView('nutrition',this)">Nutrition</div>
        <div class="nav-item" onclick="switchView('profile',this)">TDEE Calc</div>
    </nav>
</aside>

<!-- MAIN -->
<main class="flex-1 overflow-y-auto p-6 md:p-10">

<!-- DASHBOARD -->
<section id="view-dashboard" class="space-y-8">
    <h2 class="text-3xl font-bold">Your Stats</h2>

    <div class="grid md:grid-cols-3 gap-6">
        <div class="glass-card p-6">
            <p class="text-sm text-slate-400">Burned</p>
            <p id="stat-burned" class="text-4xl font-bold">0</p>
        </div>

        <div class="glass-card p-6">
            <p class="text-sm text-slate-400">Consumed</p>
            <p id="stat-consumed" class="text-4xl font-bold">0</p>
        </div>

        <div class="glass-card p-6 border-blue-400/40">
            <p class="text-sm text-blue-400">Net Balance</p>
            <p id="stat-balance" class="text-4xl font-bold">--</p>
            <p id="stat-balance-desc" class="text-xs text-slate-500"></p>
        </div>
    </div>

    <div class="glass-card p-6">
        <h3 class="font-bold mb-4">Recent Activity</h3>
        <div id="activity-list" class="space-y-3 text-sm text-slate-400">
            No activity yet.
        </div>
    </div>
</section>

<!-- WORKOUTS -->
<section id="view-workouts" class="hidden max-w-xl mx-auto">
    <div class="glass-card p-8">
        <h3 class="text-xl font-bold mb-4">Log Workout</h3>
        <form id="workout-form" class="space-y-4">
            <input id="w-name" class="input-field" placeholder="Exercise" required>
            <div class="grid grid-cols-2 gap-4">
                <input id="w-sets" type="number" class="input-field" placeholder="Sets" required>
                <input id="w-reps" type="number" class="input-field" placeholder="Reps" required>
            </div>
            <button class="w-full blue-gradient py-3 rounded-xl font-bold">Save Workout</button>
        </form>
    </div>
</section>

<!-- NUTRITION -->
<section id="view-nutrition" class="hidden max-w-xl mx-auto">
    <div class="glass-card p-8">
        <h3 class="text-xl font-bold mb-4">AI Nutrition Log</h3>
        <form id="food-form" class="space-y-4">
            <input id="f-item" class="input-field" placeholder="What did you eat?" required>
            <button class="w-full bg-cyan-400 text-slate-950 py-3 rounded-xl font-bold">Analyze</button>
        </form>
        <div id="ai-result" class="hidden mt-4 p-4 bg-slate-950 rounded-xl border border-slate-800"></div>
    </div>
</section>

<!-- TDEE -->
<section id="view-profile" class="hidden max-w-xl mx-auto">
    <div class="glass-card p-8">
        <h3 class="text-xl font-bold mb-4">TDEE Calculator</h3>
        <form id="tdee-form" class="space-y-4">
            <select id="t-gender" class="input-field">
                <option value="male">Male</option>
                <option value="female">Female</option>
            </select>
            <div class="grid grid-cols-3 gap-4">
                <input id="t-age" type="number" class="input-field" placeholder="Age" required>
                <input id="t-height" type="number" class="input-field" placeholder="Height cm" required>
                <input id="t-weight" type="number" class="input-field" placeholder="Weight kg" required>
            </div>
            <select id="t-activity" class="input-field">
                <option value="1.2">Sedentary</option>
                <option value="1.55">Moderate</option>
                <option value="1.9">Very Active</option>
            </select>
            <button class="w-full blue-gradient py-3 rounded-xl font-bold">Calculate</button>
        </form>
    </div>
</section>

</main>

<script>
const foodDB={
 egg:{cals:70,alt:"Paneer / Tofu"},
 rice:{cals:130,alt:"Brown rice / Millets"},
 paratha:{cals:220,alt:"Roti without oil"},
 milk:{cals:60,alt:"Low-fat milk / curd"}
};

let state=JSON.parse(localStorage.getItem("fittrack"))||{workouts:[],foods:[],tdee:0};

function switchView(view,el){
["dashboard","workouts","nutrition","profile"].forEach(v=>{
document.getElementById("view-"+v).classList.add("hidden");
});
document.getElementById("view-"+view).classList.remove("hidden");
document.querySelectorAll(".nav-item").forEach(n=>n.classList.remove("active"));
el.classList.add("active");
}

function updateDashboard(){
const burned=state.workouts.length*120;
const consumed=state.foods.reduce((s,f)=>s+f.cals,0);

stat-burned.textContent=burned;
stat-consumed.textContent=consumed;

if(state.tdee){
stat-balance.textContent=(state.tdee+burned)-consumed;
stat-balance-desc.textContent=`Target ${state.tdee} kcal`;
}

activity-list.innerHTML=state.workouts.concat(state.foods).slice(-5).reverse().map(i=>
`<div class="flex justify-between">
<span>${i.name||i.item}</span>
<span class="text-blue-400">${i.sets?`${i.sets}x${i.reps}`:`${i.cals} kcal`}</span>
</div>`
).join("")||"No activity yet.";

localStorage.setItem("fittrack",JSON.stringify(state));
}

workout-form.onsubmit=e=>{
e.preventDefault();
state.workouts.push({name:w-name.value,sets:w-sets.value,reps:w-reps.value});
updateDashboard();
e.target.reset();
};

food-form.onsubmit=e=>{
e.preventDefault();
let text=f-item.value.toLowerCase();
let found=Object.keys(foodDB).find(f=>text.includes(f));
let cals=found?foodDB[found].cals:200;
let alt=found?foodDB[found].alt:"Whole foods";

state.foods.push({item:f-item.value,cals});
ai-result.classList.remove("hidden");
ai-result.innerHTML=`<p class="font-bold">${cals} kcal</p><p class="text-slate-400">Better: <span class="text-cyan-400">${alt}</span></p>`;
updateDashboard();
e.target.reset();
};

tdee-form.onsubmit=e=>{
e.preventDefault();
let w=+t-weight.value,h=+t-height.value,a=+t-age.value;
let g=t-gender.value,act=+t-activity.value;
let bmr=g==="male"?(10*w+6.25*h-5*a+5):(10*w+6.25*h-5*a-161);
state.tdee=Math.round(bmr*act);
updateDashboard();
};

updateDashboard();
</script>

</body>
</html>
