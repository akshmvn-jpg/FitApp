<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fitness Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX"
     crossorigin="anonymous"></script>

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
        
        /* Ad Placeholder Styling */
        .ad-label {
            font-size: 10px;
            color: #3f3f46;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 4px;
            text-align: center;
        }
    </style>
</head>
<body class="flex h-screen overflow-hidden">

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
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4(rest of path)"></path></svg>
                Nutrition
            </div>
            <div class="nav-item" onclick="switchView('profile')">
                <svg class="w-5 h-5 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                Profile & TDEE
            </div>
        </nav>

        <div class="mt-6 mb-6">
            <p class="ad-label">Advertisement</p>
            <div class="studia-card p-1 bg-zinc-900/50">
                <ins class="adsbygoogle"
                     style="display:block"
                     data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
                     data-ad-slot="YYYYYYYYYY"
                     data-ad-format="auto"
                     data-full-width-responsive="true"></ins>
                <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
            </div>
        </div>

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

    <main class="flex-1 overflow-y-auto p-4 md:p-8 pb-24 md:pb-8">
        
        <div class="mb-8">
            <p class="ad-label">Advertisement</p>
            <div class="min-h-[90px] w-full bg-zinc-900/30 rounded-xl flex items-center justify-center border border-zinc-800/50">
                <ins class="adsbygoogle"
                     style="display:block"
                     data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
                     data-ad-slot="ZZZZZZZZZZ"
                     data-ad-format="horizontal"
                     data-full-width-responsive="true"></ins>
                <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
            </div>
        </div>

        <div class="mb-8">
            <h2 class="text-3xl font-bold text-white mb-1">Welcome back</h2>
            <p class="text-zinc-500">Here's your daily fitness breakdown.</p>
        </div>

        </main>

    </body>
</html>
