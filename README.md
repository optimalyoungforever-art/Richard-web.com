<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>richardweb.com</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #05070f;
            --surface: rgba(12,18,34,0.78);
            --surface2: rgba(20,28,50,0.5);
            --border: rgba(255,255,255,0.06);
            --border-accent: rgba(99,224,255,0.2);
            --text: #e8edf8;
            --muted: #5a6a8a;
            --dim: #3a4a6a;
            --accent: #1de9ff;
            --accent2: #a78bfa;
            --accent3: #34eba8;
            --danger: #ff6b9d;
            --glow: rgba(29,233,255,0.12);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body, html {
            width: 100%; height: 100%;
            background: var(--bg);
            font-family: 'DM Mono', monospace;
            color: var(--text);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        canvas#bg { position:fixed; inset:0; z-index:0; opacity:0.4; }

        body::before {
            content:''; position:fixed; inset:0;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
            opacity:0.025; z-index:0; pointer-events:none;
        }

        .glow-orb { position:fixed; border-radius:50%; filter:blur(120px); pointer-events:none; z-index:0; }
        .glow-orb.a { width:700px;height:700px;top:-200px;left:-200px;background:radial-gradient(circle,rgba(99,224,255,0.07),transparent 70%);animation:driftA 18s ease-in-out infinite alternate; }
        .glow-orb.b { width:500px;height:500px;bottom:-150px;right:-100px;background:radial-gradient(circle,rgba(167,139,250,0.09),transparent 70%);animation:driftB 14s ease-in-out infinite alternate; }
        @keyframes driftA { 0%{transform:translate(0,0)} 100%{transform:translate(80px,60px)} }
        @keyframes driftB { 0%{transform:translate(0,0)} 100%{transform:translate(-60px,-80px)} }

        /* ─── CARD ── */
        .card {
            position:relative; z-index:1;
            width:min(92vw,680px);
            max-height:92vh;
            background:var(--surface);
            backdrop-filter:blur(32px) saturate(1.4);
            border:1px solid var(--border);
            border-radius:28px;
            overflow:hidden;
            display:flex; flex-direction:column;
            box-shadow: 0 0 0 1px rgba(255,255,255,0.03) inset,0 40px 80px -16px rgba(0,0,0,0.7),0 0 80px -20px var(--glow);
            animation:cardIn 0.7s cubic-bezier(0.16,1,0.3,1) both;
        }
        @keyframes cardIn { from{opacity:0;transform:translateY(24px) scale(0.97)} to{opacity:1;transform:translateY(0) scale(1)} }
        .card::after { content:'';position:absolute;inset:0;background:linear-gradient(180deg,transparent 0%,rgba(255,255,255,0.012) 50%,transparent 100%);background-size:100% 4px;pointer-events:none;z-index:99;opacity:0.5; }

        /* ─── HEADER ── */
        .header {
            display:flex; align-items:center; justify-content:space-between;
            padding:1.1rem 1.75rem;
            border-bottom:1px solid var(--border);
            background:rgba(8,12,24,0.5);
            gap:12px; flex-shrink:0;
        }
        .logo-group { display:flex; align-items:center; gap:10px; }
        .status-dot { width:7px;height:7px;border-radius:50%;background:var(--accent3);box-shadow:0 0 8px var(--accent3),0 0 18px rgba(52,235,168,0.4);animation:blink 2.5s ease-in-out infinite;flex-shrink:0; }
        @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.45} }
        .logo-text { font-family:'Syne',sans-serif;font-size:0.875rem;font-weight:700;color:var(--text);letter-spacing:-0.02em; }
        .tag { background:rgba(29,233,255,0.08);border:1px solid rgba(29,233,255,0.2);color:var(--accent);font-size:0.6rem;font-weight:500;padding:0.18rem 0.6rem;border-radius:20px;letter-spacing:0.08em;text-transform:uppercase; }
        .datetime { display:flex;align-items:center;gap:10px;font-size:0.68rem;color:var(--muted);background:rgba(255,255,255,0.02);border:1px solid var(--border);padding:0.3rem 0.85rem;border-radius:20px; }
        .datetime .sep { width:1px;height:10px;background:var(--dim); }
        .datetime .t { color:var(--accent);font-weight:500; }

        /* ─── SCROLLABLE BODY ── */
        .body {
            padding:1.5rem 1.75rem;
            display:flex; flex-direction:column; gap:1.1rem;
            overflow-y:auto; flex:1;
        }
        .body::-webkit-scrollbar { width:4px; }
        .body::-webkit-scrollbar-track { background:transparent; }
        .body::-webkit-scrollbar-thumb { background:var(--dim); border-radius:4px; }

        /* ─── HERO ── */
        .hero { text-align:center; }
        .hero h1 { font-family:'Syne',sans-serif;font-size:clamp(1.8rem,6vw,2.5rem);font-weight:800;letter-spacing:-0.04em;line-height:1;background:linear-gradient(135deg,#ffffff 10%,#a8d8ff 50%,var(--accent) 90%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:0.35rem; }
        .hero p { font-size:0.72rem;color:var(--muted);line-height:1.6; }

        /* ─── SECTION LABEL ── */
        .section-label { font-size:0.6rem;text-transform:uppercase;letter-spacing:0.15em;color:var(--dim);margin-bottom:0.6rem;display:flex;align-items:center;gap:8px; }
        .section-label::after { content:'';flex:1;height:1px;background:var(--border); }

        /* ─── TASK ROW ── */
        .tasks { display:flex;flex-direction:column;gap:0.5rem; }

        .task {
            display:flex; align-items:center; gap:12px;
            padding:0.7rem 1rem;
            background:var(--surface2);
            border:1px solid var(--border);
            border-radius:14px;
            transition:border-color 0.2s, box-shadow 0.2s, opacity 0.3s, transform 0.3s;
            animation:taskIn 0.35s cubic-bezier(0.16,1,0.3,1) both;
        }
        @keyframes taskIn { from{opacity:0;transform:translateY(8px) scale(0.98)} to{opacity:1;transform:translateY(0) scale(1)} }
        .task:hover { border-color:var(--border-accent); box-shadow:0 0 20px -4px rgba(29,233,255,0.1); }
        .task.removing { opacity:0; transform:translateX(20px) scale(0.95); }

        .task-check {
            width:18px; height:18px; border-radius:5px;
            border:1.5px solid var(--dim);
            cursor:pointer; flex-shrink:0;
            display:flex; align-items:center; justify-content:center;
            transition:border-color 0.2s, background 0.2s;
            font-size:0.65rem;
        }
        .task-check:hover { border-color:var(--accent3); }
        .task-check.done { background:rgba(52,235,168,0.15); border-color:var(--accent3); color:var(--accent3); }

        .task-icon { width:30px;height:30px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:0.8rem;flex-shrink:0; }
        .task-icon.blue  { background:rgba(99,102,241,0.15);border:1px solid rgba(99,102,241,0.25); }
        .task-icon.teal  { background:rgba(29,233,255,0.1);border:1px solid rgba(29,233,255,0.2); }
        .task-icon.purple{ background:rgba(167,139,250,0.12);border:1px solid rgba(167,139,250,0.25); }
        .task-icon.green { background:rgba(52,235,168,0.1);border:1px solid rgba(52,235,168,0.2); }
        .task-icon.pink  { background:rgba(255,107,157,0.1);border:1px solid rgba(255,107,157,0.2); }

        .task-info { flex:1; min-width:0; text-align:left; }
        .task-title { font-size:0.78rem;font-weight:500;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis; }
        .task-title.done-text { text-decoration:line-through; color:var(--muted); }
        .task-meta { font-size:0.62rem;color:var(--muted);margin-top:2px; }

        .pill { font-size:0.57rem;font-weight:600;padding:0.2rem 0.6rem;border-radius:20px;white-space:nowrap;flex-shrink:0; }
        .pill.soon     { background:rgba(255,107,157,0.12);color:var(--danger);border:1px solid rgba(255,107,157,0.28); }
        .pill.upcoming { background:rgba(29,233,255,0.1);color:var(--accent);border:1px solid rgba(29,233,255,0.25); }
        .pill.done-pill{ background:rgba(52,235,168,0.1);color:var(--accent3);border:1px solid rgba(52,235,168,0.25); }

        .btn-delete {
            width:24px;height:24px;border-radius:7px;border:none;
            background:transparent;color:var(--dim);cursor:pointer;
            display:flex;align-items:center;justify-content:center;font-size:0.85rem;
            flex-shrink:0;transition:background 0.2s,color 0.2s;
        }
        .btn-delete:hover { background:rgba(255,107,157,0.12);color:var(--danger); }

        /* ─── ADD TASK BUTTON ── */
        .btn-add-task {
            display:flex; align-items:center; justify-content:center; gap:8px;
            width:100%; padding:0.65rem;
            background:rgba(29,233,255,0.05);
            border:1px dashed rgba(29,233,255,0.2);
            border-radius:14px;
            color:var(--accent);
            font-family:'DM Mono',monospace;
            font-size:0.72rem;
            cursor:pointer;
            transition:background 0.2s, border-color 0.2s, transform 0.15s;
            margin-top:0.2rem;
        }
        .btn-add-task:hover { background:rgba(29,233,255,0.09);border-color:rgba(29,233,255,0.4);transform:translateY(-1px); }
        .btn-add-task:active { transform:scale(0.98); }

        /* ─── SEARCH BAR SECTION ── */
        .search-section {
            display:flex; flex-direction:column; gap:8px;
            background:var(--surface2); padding:1rem;
            border:1px solid var(--border); border-radius:18px;
        }
        .search-row { display:flex; gap:8px; }
        .search-select {
            background:rgba(5,7,15,0.6); border:1px solid var(--border);
            border-radius:10px; padding:0.4rem 0.6rem; color:var(--text);
            font-family:'DM Mono',monospace; font-size:0.68rem; outline:none;
            cursor:pointer;
        }
        .search-select option { background:#05070f; color:var(--text); }
        .search-input {
            flex:1; background:rgba(5,7,15,0.6); border:1px solid var(--border);
            border-radius:10px; padding:0.55rem 0.75rem; color:var(--text);
            font-family:'DM Mono',monospace; font-size:0.68rem; outline:none;
            transition:border-color 0.2s, box-shadow 0.2s;
        }
        .search-input:focus { border-color:var(--accent); box-shadow:0 0 12px -4px rgba(29,233,255,0.2); }
        .search-btn {
            background:linear-gradient(135deg, rgba(29,233,255,0.15), rgba(52,235,168,0.15));
            border:1px solid rgba(29,233,255,0.3); color:var(--accent);
            border-radius:10px; padding:0.55rem 1rem; font-family:'DM Mono',monospace;
            font-size:0.68rem; cursor:pointer; font-weight:600;
            transition:background 0.2s, border-color 0.2s;
        }
        .search-btn:hover { border-color:var(--accent); background:rgba(29,233,255,0.25); }

        /* ─── URL SECTION ── */
        .url-section {
            display:flex; flex-direction:column; gap:8px;
            background:var(--surface2);
            padding:1rem; border:1px solid var(--border); border-radius:18px;
        }
        .url-row { display:flex; gap:8px; }
        .url-input {
            flex:1; background:rgba(5,7,15,0.6); border:1px solid var(--border);
            border-radius:10px; padding:0.55rem 0.75rem; color:var(--text);
            font-family:'DM Mono',monospace; font-size:0.68rem;
            outline:none; transition:border-color 0.2s, box-shadow 0.2s;
        }
        .url-input:focus { border-color: var(--accent2); box-shadow:0 0 12px -4px rgba(167,139,250,0.2); }
        .url-btn {
            background:linear-gradient(135deg, rgba(167,139,250,0.15), rgba(99,102,241,0.15));
            border:1px solid rgba(167,139,250,0.3); color:var(--accent2);
            border-radius:10px; padding:0.55rem 1rem; font-family:'DM Mono',monospace;
            font-size:0.68rem; cursor:pointer; font-weight:600;
            transition:background 0.2s, border-color 0.2s;
        }
        .url-btn:hover { border-color:var(--accent2); background:rgba(167,139,250,0.25); }
        .url-output-list { display:flex; flex-direction:column; gap:6px; margin-top:2px; max-height:120px; overflow-y:auto; }
        .url-item {
            display:flex; align-items:center; justify-content:space-between;
            background:rgba(5,7,15,0.4); border:1px solid var(--border);
            padding:0.45rem 0.65rem; border-radius:10px; font-size:0.65rem;
        }
        .url-link { color:var(--accent2); text-decoration:none; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; max-width:80%; }
        .url-link:hover { text-decoration:underline; }
        .url-actions { display:flex; gap:6px; align-items:center; }
    </style>
</head>
<body>

    <div class="glow-orb a"></div>
    <div class="glow-orb b"></div>

    <canvas id="bg"></canvas>

    <div class="card">
        <div class="header">
            <div class="logo-group">
                <div class="status-dot"></div>
                <div class="logo-text">richardweb.com</div>
            </div>
            <div class="datetime">
                <span class="t" id="time-display">00:00:00</span>
                <span class="sep"></span>
                <span id="date-display">Wednesday, May 6</span>
            </div>
        </div>

        <div class="body">
            <div class="hero">
                <h1>Richard Dev</h1>
                <p>Full-Stack Developer, UI Enthusiast, and Problem Solver.<br>Building functional web experiences and clean interfaces.</p>
            </div>

            <div class="section-label">Task Tracker</div>
            <div class="tasks" id="task-container">
                </div>

            <button class="btn-add-task" id="btn-add-task">
                <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12h14"/></svg>
                Create New Task
            </button>
            
            <div style="height: 4px;"></div>

            <div class="section-label">Web Search</div>
            <div class="search-section">
                <form class="search-row" id="search-form" onsubmit="performSearch(event)">
                    <select class="search-select" id="search-engine">
                        <option value="google">Google</option>
                        <option value="youtube">YouTube</option>
                        <option value="github">GitHub</option>
                        <option value="duckduckgo">DuckDuckGo</option>
                    </select>
                    <input type="text" class="search-input" id="search-input" placeholder="Search the web...">
                    <button type="submit" class="search-btn">Search</button>
                </form>
            </div>

            <div style="height: 4px;"></div>

            <div class="section-label">Shortcuts & Links</div>
            <div class="url-section">
                <div class="url-row">
                    <input type="text" class="url-input" id="url-input" placeholder="Label for the URL...">
                    <input type="text" class="url-input" id="url-value" placeholder="https://...">
                    <button class="url-btn" id="btn-add-url">Add</button>
                </div>
                <div class="url-output-list" id="url-list">
                    </div>
            </div>
        </div>
    </div>

    <script>
        // Digital Clock
        function updateTime() {
            const now = new Date();
            const timeStr = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false });
            const dateStr = now.toLocaleDateString('en-US', { weekday: 'long', month: 'short', day: 'numeric' });
            document.getElementById('time-display').textContent = timeStr;
            document.getElementById('date-display').textContent = dateStr;
        }
        setInterval(updateTime, 1000);
        updateTime();

        // State & Data
        let tasks = [
            { id: 1, title: "Refactor API endpoint and database queries", icon: "teal", tag: "soon", date: "May 8, 2026", completed: false },
            { id: 2, title: "Update dashboard design and typography", icon: "purple", tag: "upcoming", date: "May 12, 2026", completed: true },
            { id: 3, title: "Deploy documentation site", icon: "green", tag: "soon", date: "May 10, 2026", completed: false }
        ];

        let links = [
            { id: 1, label: "GitHub", url: "https://github.com" },
            { id: 2, label: "Vercel Dashboard", url: "https://vercel.com" }
        ];

        // Render Functions
        function renderTasks() {
            const container = document.getElementById('task-container');
            container.innerHTML = '';
            
            tasks.forEach(t => {
                const item = document.createElement('div');
                item.className = 'task';
                if (t.completed) item.classList.add('done');
                
                item.innerHTML = `
                    <div class="task-check ${t.completed ? 'done' : ''}" onclick="toggleTask(${t.id})">
                        ${t.completed ? '✓' : ''}
                    </div>
                    <div class="task-icon ${t.icon}">
                        <span style="transform: rotate(0deg);">💻</span>
                    </div>
                    <div class="task-info">
                        <div class="task-title ${t.completed ? 'done-text' : ''}">${t.title}</div>
                        <div class="task-meta">${t.date}</div>
                    </div>
                    <span class="pill ${t.completed ? 'done-pill' : t.tag}">${t.completed ? 'Completed' : t.tag.toUpperCase()}</span>
                    <button class="btn-delete" onclick="deleteTask(${t.id})">×</button>
                `;
                container.appendChild(item);
            });
        }

        function renderLinks() {
            const container = document.getElementById('url-list');
            container.innerHTML = '';

            links.forEach(l => {
                const item = document.createElement('div');
                item.className = 'url-item';
                item.innerHTML = `
                    <a href="${l.url}" target="_blank" class="url-link" title="${l.label} - ${l.url}">${l.label}</a>
                    <div class="url-actions">
                        <button class="btn-delete" onclick="deleteLink(${l.id})">×</button>
                    </div>
                `;
                container.appendChild(item);
            });
        }

        // Web Search Form Action
        window.performSearch = function(event) {
            event.preventDefault();
            const engine = document.getElementById('search-engine').value;
            const query = document.getElementById('search-input').value;

            if (!query.trim()) return;

            let url = '';
            switch (engine) {
                case 'google':
                    url = `https://www.google.com/search?q=${encodeURIComponent(query)}`;
                    break;
                case 'youtube':
                    url = `https://www.youtube.com/results?search_query=${encodeURIComponent(query)}`;
                    break;
                case 'github':
                    url = `https://github.com/search?q=${encodeURIComponent(query)}`;
                    break;
                case 'duckduckgo':
                    url = `https://duckduckgo.com/?q=${encodeURIComponent(query)}`;
                    break;
                default:
                    url = `https://www.google.com/search?q=${encodeURIComponent(query)}`;
            }

            window.open(url, '_blank');
            document.getElementById('search-input').value = '';
        };

        // Event Actions
        window.toggleTask = function(id) {
            tasks = tasks.map(t => t.id === id ? { ...t, completed: !t.completed } : t);
            renderTasks();
        };

        window.deleteTask = function(id) {
            tasks = tasks.filter(t => t.id !== id);
            renderTasks();
        };

        document.getElementById('btn-add-task').addEventListener('click', () => {
            const title = prompt("Enter the task title:");
            if (title && title.trim()) {
                const newId = Date.now();
                tasks.push({
                    id: newId,
                    title: title,
                    icon: "blue",
                    tag: "upcoming",
                    date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' }),
                    completed: false
                });
                renderTasks();
            }
        });

        document.getElementById('btn-add-url').addEventListener('click', () => {
            const label = document.getElementById('url-input').value;
            const url = document.getElementById('url-value').value;

            if (label && url) {
                let formattedUrl = url.startsWith('http') ? url : 'https://' + url;
                links.push({ id: Date.now(), label, url: formattedUrl });
                document.getElementById('url-input').value = '';
                document.getElementById('url-value').value = '';
                renderLinks();
            } else {
                alert("Please fill out both the label and the URL.");
            }
        });

        window.deleteLink = function(id) {
            links = links.filter(l => l.id !== id);
            renderLinks();
        };

        // Initialize lists
        renderTasks();
        renderLinks();

        // Canvas Effect for futuristic background lines
        const canvas = document.getElementById('bg');
        const ctx = canvas.getContext('2d');
        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        });

        let particles = Array.from({ length: 45 }, () => ({
            x: Math.random() * width,
            y: Math.random() * height,
            vx: (Math.random() - 0.5) * 0.3,
            vy: (Math.random() - 0.5) * 0.3,
            radius: Math.random() * 1.2
        }));

        function draw() {
            ctx.clearRect(0, 0, width, height);
            ctx.fillStyle = 'rgba(29, 233, 255, 0.08)';
            
            particles.forEach(p => {
                p.x += p.vx;
                p.y += p.vy;

                if (p.x < 0) p.x = width;
                if (p.x > width) p.x = 0;
                if (p.y < 0) p.y = height;
                if (p.y > height) p.y = 0;

                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                ctx.fill();
            });

            requestAnimationFrame(draw);
        }
        draw();
    </script>
</body>
</html>
