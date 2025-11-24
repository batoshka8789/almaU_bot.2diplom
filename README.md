<html lang="ru">
<head>
    <meta charset="utf-8" />
    <!-- Viewport optimized for mobile apps: no scaling, cover full width -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    
    <!-- PWA / Mobile App Meta Tags -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#002855">
    
    <title>AlmaU Global Mobility Portal</title>
    
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@300;700&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <!-- Icons -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <!-- Telegram Web App Script -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <style>
        :root {
            --primary: #002855; /* Academic Navy */
            --secondary: #004b8d;
            --accent: #c5a065; /* University Gold */
            --text-dark: #1a1a1a;
            --text-light: #595959;
            --bg-light: #f4f6f9;
            --white: #ffffff;
            --border: #e0e0e0;
            --success: #2e7d32;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --radius: 8px;
            --header-height: 120px; /* Approximate for calculations */
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            margin: 0;
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            line-height: 1.6;
            overflow-x: hidden; /* Prevent horizontal scroll on mobile */
        }

        h1, h2, h3, h4 { font-family: 'Merriweather', serif; color: var(--primary); margin-top: 0; }
        
        /* HEADER UTILITIES */
        .top-bar {
            background-color: var(--primary);
            color: var(--white);
            padding: 8px 0;
            font-size: 0.85rem;
        }
        
        .container { max-width: 1400px; margin: 0 auto; padding: 0 16px; }
        .top-flex { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        
        /* STICKY MAIN HEADER */
        .main-header {
            background: var(--white);
            border-bottom: 1px solid var(--border);
            padding: 12px 0;
            box-shadow: var(--shadow);
            position: sticky; 
            top: 0; 
            z-index: 1000;
            width: 100%;
        }
        .header-flex { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 10px; }
        
        .logo-area { display: flex; align-items: center; gap: 12px; flex-shrink: 0; }
        .logo-box {
            width: 42px; height: 42px; background: var(--primary); color: var(--white);
            font-family: 'Merriweather'; font-weight: bold; font-size: 20px;
            display: flex; align-items: center; justify-content: center;
            border-radius: 6px; border: 2px solid var(--accent);
        }
        .brand-text h1 { font-size: 1.2rem; margin: 0; color: var(--primary); line-height: 1.2; }
        .brand-text p { margin: 0; font-size: 0.7rem; color: var(--text-light); text-transform: uppercase; letter-spacing: 0.5px; }

        /* SCROLLABLE NAV FOR MOBILE */
        .nav-menu { 
            display: flex; gap: 10px; 
            overflow-x: auto; 
            padding-bottom: 5px; /* Space for scrollbar if visible */
            -webkit-overflow-scrolling: touch; /* Smooth scroll iOS */
            scrollbar-width: none; /* Firefox hide scrollbar */
            margin-left: auto;
        }
        .nav-menu::-webkit-scrollbar { display: none; /* Chrome hide scrollbar */ }

        .nav-item {
            text-decoration: none; color: var(--text-dark); font-weight: 500; font-size: 0.95rem;
            padding: 8px 12px; position: relative; transition: color 0.3s; cursor: pointer;
            white-space: nowrap; border-radius: 20px;
            background: transparent;
        }
        .nav-item:hover { color: var(--secondary); background: #f0f4f8; }
        .nav-item.active { color: var(--white); background: var(--primary); }
        
        /* CONTENT SECTIONS */
        .content-section { display: none; padding-bottom: 80px; animation: fadeIn 0.4s; }
        .content-section.active { display: block; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        /* DASHBOARD GRID */
        .dashboard-grid {
            display: grid; grid-template-columns: 350px 1fr; gap: 20px;
            margin-top: 20px; height: calc(100vh - 120px); min-height: 600px;
        }

        /* SIDEBAR */
        .sidebar-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            display: flex; flex-direction: column; overflow: hidden; box-shadow: var(--shadow);
        }
        .panel-header { padding: 15px; background: var(--primary); color: var(--white); }
        .panel-header h3 { margin: 0; font-size: 1rem; color: var(--accent); }
        .stats-row { display: flex; gap: 15px; margin-top: 5px; font-size: 0.8rem; opacity: 0.9; }

        .search-box { padding: 10px 15px; border-bottom: 1px solid var(--border); background: #f9f9f9; }
        .input-group { position: relative; }
        .input-group i { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #999; }
        .form-control {
            width: 100%; padding: 10px 10px 10px 35px; border: 1px solid #ccc; border-radius: 20px;
            font-family: inherit; font-size: 0.9rem; transition: border 0.3s;
        }
        .form-control:focus { border-color: var(--secondary); outline: none; }

        .filter-tags { padding: 10px 15px; display: flex; flex-wrap: wrap; gap: 6px; border-bottom: 1px solid var(--border); }
        .filter-tag {
            font-size: 0.75rem; padding: 4px 12px; background: #eef2f6; border-radius: 12px;
            cursor: pointer; border: 1px solid transparent; transition: all 0.2s;
        }
        .filter-tag.active { background: var(--primary); color: var(--white); }

        .university-list { overflow-y: auto; flex: 1; -webkit-overflow-scrolling: touch; }
        .uni-item {
            padding: 12px 15px; border-bottom: 1px solid var(--border); cursor: pointer; 
            border-left: 4px solid transparent; position: relative;
        }
        .uni-item:active { background: #f0f7ff; } /* Mobile tap effect */
        .uni-item.active { background: #e6f0fa; border-left-color: var(--accent); }
        .uni-name { font-weight: 700; color: var(--primary); display: block; font-size: 0.95rem; }
        .uni-meta { font-size: 0.8rem; color: var(--text-light); display: flex; justify-content: space-between; margin-top: 4px; }
        .rank-badge { background: var(--accent); color: var(--white); padding: 2px 6px; border-radius: 3px; font-size: 0.7rem; font-weight: bold; }

        /* MAP */
        .map-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            overflow: hidden; box-shadow: var(--shadow); position: relative; z-index: 1;
        }
        #map { width: 100%; height: 100%; }

        /* DOUBLE DEGREE */
        .double-degree-grid { 
            display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 15px; margin-top: 20px;
        }
        .info-card { 
            border: 1px solid var(--border); background: #fff; padding: 15px; border-radius: var(--radius);
        }
        .dd-info-label { font-size: 0.75rem; text-transform: uppercase; color: var(--text-light); margin-bottom: 4px; }
        .dd-info-value { font-size: 1rem; font-weight: 600; color: var(--primary); line-height: 1.3; }
        .program-list-styled-dd { list-style: none; padding: 0; margin-top: 10px; }
        .program-list-styled-dd li {
            padding: 6px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start;
            font-size: 0.9rem; color: #444;
        }
        .program-list-styled-dd li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 10px; color: var(--accent); font-size: 0.8rem; margin-top: 3px;
        }

        /* FAQ */
        .faq-container { max-width: 800px; margin: 20px auto; }
        .faq-category { margin-bottom: 30px; }
        .cat-title { border-bottom: 2px solid var(--accent); padding-bottom: 10px; margin-bottom: 20px; display: inline-block; font-size: 1.1rem; color: var(--secondary); }
        .faq-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            margin-bottom: 10px; overflow: hidden;
        }
        .faq-header {
            padding: 15px; display: flex; justify-content: space-between; align-items: center;
            font-weight: 600; font-size: 0.95rem; cursor: pointer;
        }
        .faq-body {
            max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out;
            padding: 0 15px; font-size: 0.9rem; color: #555; background: #fafafa;
        }
        .faq-card.open .faq-body { padding: 15px; max-height: 500px; border-top: 1px solid #eee; }

        /* TIMELINE */
        .timeline { position: relative; max-width: 800px; margin: 30px auto; padding: 10px 0; }
        .timeline::before {
            content: ''; position: absolute; top: 0; bottom: 0; left: 20px; width: 2px; background: var(--border);
        }
        .timeline-item { position: relative; padding: 0 20px 0 50px; margin-bottom: 30px; }
        .timeline-dot {
            position: absolute; left: 11px; top: 0; width: 16px; height: 16px; background: var(--white);
            border: 3px solid var(--secondary); border-radius: 50%; z-index: 2;
        }
        .timeline-content {
            background: var(--white); padding: 15px; border-radius: var(--radius); border: 1px solid var(--border);
        }
        .step-num {
            display: inline-block; background: var(--accent); color: var(--white);
            padding: 2px 10px; border-radius: 12px; font-weight: bold; font-size: 0.7rem; margin-bottom: 8px;
        }

        /* MODAL (Passport Style) */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.6); z-index: 2000;
            display: none; justify-content: center; align-items: flex-end; /* Mobile: slide from bottom */
            opacity: 0; transition: opacity 0.3s;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        
        .modal-window {
            background: var(--white); width: 100%; height: 90vh; /* Mobile height */
            border-radius: 16px 16px 0 0; display: flex; flex-direction: column; overflow: hidden;
            box-shadow: 0 -5px 20px rgba(0,0,0,0.2);
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .modal-header {
            background: var(--primary); color: var(--white); padding: 20px;
            display: flex; justify-content: space-between; align-items: flex-start;
            flex-shrink: 0;
        }
        .m-title h2 { color: var(--white); margin: 0; font-size: 1.4rem; }
        .m-subtitle { color: var(--accent); margin-top: 5px; font-size: 0.85rem; display: flex; flex-wrap: wrap; gap: 10px; }
        .close-btn { 
            background: rgba(255,255,255,0.1); border: none; color: var(--white); width: 36px; height: 36px;
            border-radius: 50%; font-size: 1.2rem; cursor: pointer; display: flex; align-items: center; justify-content: center; 
        }

        .modal-body { display: flex; flex-direction: column; flex: 1; overflow: hidden; }
        
        .modal-nav {
            background: #f4f6f9; border-bottom: 1px solid var(--border);
            display: flex; overflow-x: auto; padding: 0; flex-shrink: 0;
            -webkit-overflow-scrolling: touch;
        }
        .m-tab {
            padding: 12px 15px; cursor: pointer; font-weight: 500; color: var(--text-light);
            background: none; border: none; border-bottom: 3px solid transparent; 
            font-size: 0.9rem; white-space: nowrap; flex-shrink: 0;
        }
        .m-tab.active { background: var(--white); color: var(--primary); border-bottom-color: var(--accent); font-weight: 700; }

        .modal-content { flex: 1; padding: 20px; overflow-y: auto; background: var(--white); }
        .tab-panel { display: none; }
        .tab-panel.active { display: block; animation: fadeIn 0.3s; }
        
        .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }

        .program-list-styled { list-style: none; padding: 0; }
        .program-list-styled li {
            padding: 10px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start; font-size: 0.9rem;
        }
        .program-list-styled li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 10px; color: var(--accent); font-size: 0.8rem; margin-top: 3px;
        }

        /* GOLDEN MARKER */
        .golden-marker-icon {
            background-color: transparent !important; border: none !important;
            color: var(--accent); font-size: 30px; text-shadow: 0 0 4px rgba(0,0,0,0.5);
        }

        /* RESPONSIVE BREAKPOINTS */
        @media (min-width: 900px) {
            .top-bar { display: block; } /* Show top bar only on desktop */
            .nav-menu { margin-left: auto; overflow: visible; }
            .nav-item { border-radius: 0; padding: 8px 0; background: none !important; margin-left: 20px; }
            .nav-item.active { color: var(--secondary); background: none; }
            .nav-item.active::after {
                content: ''; position: absolute; bottom: 0; left: 0; width: 100%; height: 3px; background: var(--accent);
            }
            .dashboard-grid { grid-template-columns: 350px 1fr; grid-template-areas: "sidebar map"; }
            .sidebar-panel { grid-area: sidebar; height: auto; }
            .map-panel { grid-area: map; height: auto; }
            .modal-window { 
                width: 800px; height: 80vh; max-width: 90%; 
                border-radius: 8px; justify-content: center;
                animation: fadeIn 0.2s;
            }
            .modal-overlay { align-items: center; } /* Center modal on desktop */
            .modal-body { flex-direction: row; }
            .modal-nav { flex-direction: column; width: 180px; border-right: 1px solid var(--border); border-bottom: none; }
            .m-tab { border-bottom: none; border-left: 4px solid transparent; text-align: left; }
            .m-tab.active { border-left-color: var(--accent); border-bottom: none; }
        }

        @media (max-width: 899px) {
            .top-bar { display: none; } /* Hide top bar on mobile */
            .dashboard-grid { 
                grid-template-columns: 1fr; 
                grid-template-areas: "map" "sidebar";
                height: auto; 
                display: block; /* Stack block */
            }
            .map-panel { height: 350px; margin-bottom: 15px; } /* Fixed height for map on mobile */
            .sidebar-panel { height: 500px; } /* Fixed height for list */
            .container { padding: 0 12px; }
            .logo-box { width: 36px; height: 36px; font-size: 18px; }
        }
    </style>
</head>
<body>

    <!-- Desktop Only Info Bar -->
    <div class="top-bar">
        <div class="container top-flex">
            <span><i class="fas fa-university"></i> Официальный ресурс Академической Мобильности AlmaU</span>
            <span>mobility@almau.edu.kz | Офис 107</span>
        </div>
    </div>

    <!-- Sticky Header -->
    <header class="main-header">
        <div class="container header-flex">
            <div class="logo-area">
                <div class="logo-box">A</div>
                <div class="brand-text">
                    <h1>AlmaU Global</h1>
                    <p>International Dept</p>
                </div>
            </div>
            <nav class="nav-menu">
                <a class="nav-item active" onclick="switchTab('map-section', this)">Карта Партнеров</a>
                <a class="nav-item" onclick="switchTab('double-degree-section', this)">Двойной Диплом</a>
                <a class="nav-item" onclick="switchTab('plan-section', this)">Дорожная Карта</a>
                <a class="nav-item" onclick="switchTab('faq-section', this)">FAQ</a>
            </nav>
        </div>
    </header>

    <div class="container">
        
        <!-- MAP SECTION -->
        <section id="map-section" class="content-section active">
            <div class="dashboard-grid">
                <!-- Map comes first on mobile visually via order, but defined here for desktop -->
                <main class="map-panel">
                    <div id="map"></div>
                </main>

                <aside class="sidebar-panel">
                    <div class="panel-header">
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <h3><i class="fas fa-globe-europe"></i> Навигатор ВУЗов</h3>
                            <span id="count-display" style="background:rgba(255,255,255,0.2); padding:2px 6px; border-radius:4px; font-size:0.8rem;">0</span>
                        </div>
                    </div>
                    
                    <div class="search-box">
                        <div class="input-group">
                            <i class="fas fa-search"></i>
                            <input type="text" id="searchBox" class="form-control" placeholder="Поиск по стране, ВУЗу...">
                        </div>
                    </div>

                    <div class="filter-tags">
                        <span class="filter-tag active" onclick="filterList('all', this)">Все</span>
                        <span class="filter-tag" onclick="filterList('Europe', this)">Европа</span>
                        <span class="filter-tag" onclick="filterList('Asia', this)">Азия</span>
                        <span class="filter-tag" onclick="filterList('CIS', this)">СНГ</span>
                        <span class="filter-tag" onclick="filterList('Americas', this)">Америка</span>
                        <span class="filter-tag" onclick="filterList('Africa', this)">Африка</span>
                    </div>

                    <div class="university-list" id="uni-list-container">
                        <!-- JS renders items here -->
                    </div>
                </aside>
            </div>
        </section>
        
        <!-- DOUBLE DEGREE SECTION -->
        <section id="double-degree-section" class="content-section">
            <div style="text-align: center; margin: 30px 0 20px;">
                <h2 style="font-size:1.5rem; color: var(--secondary);">Программа Двойного Диплома AlmaU</h2>
                <p style="color:var(--text-light); font-size:0.9rem;">Получите два диплома, обучаясь в AlmaU и ведущих университетах мира.</p>
            </div>
            
            <div id="double-degree-content"></div>

            <div style="margin-top: 30px; padding: 15px; background: #e6f0fa; border-radius: var(--radius); border-left: 5px solid var(--accent); font-size: 0.9rem;">
                <h4 style="margin-top: 0; color: var(--primary);"><i class="fas fa-info-circle"></i> Особенности:</h4>
                <ul style="list-style-type: disc; padding-left: 20px; color: #444; margin: 0;">
                    <li>Обучение 1-2 года в вузе-партнере.</li>
                    <li>Два полноценных диплома (AlmaU + Партнер).</li>
                    <li>Обучение в вузе-партнере платное.</li>
                    <li>Высокие требования по успеваемости и языку.</li>
                </ul>
            </div>
        </section>

        <!-- ROADMAP SECTION -->
        <section id="plan-section" class="content-section">
            <div style="text-align: center; margin: 30px 0 20px;">
                <h2 style="font-size:1.5rem;">Процесс подачи заявки</h2>
                <p style="color:var(--text-light); font-size:0.9rem;">Официальный регламент прохождения конкурса</p>
            </div>
            <div class="timeline">
                <div id="timeline-container"></div>
            </div>
        </section>

        <!-- FAQ SECTION -->
        <section id="faq-section" class="content-section">
            <div class="faq-container" id="faq-root"></div>
        </section>

    </div>

    <!-- MODAL -->
    <div class="modal-overlay" id="uniModal">
        <div class="modal-window">
            <div class="modal-header">
                <div class="m-title">
                    <h2 id="m-name">University Name</h2>
                    <div class="m-subtitle">
                        <span><i class="fas fa-map-marker-alt"></i> <span id="m-city">City</span>, <span id="m-country">Country</span></span>
                    </div>
                </div>
                <button class="close-btn" onclick="closeModal()"><i class="fas fa-times"></i></button>
            </div>
            <div class="modal-body">
                <div class="modal-nav">
                    <button class="m-tab active" onclick="modalTab('tab-overview', this)">Обзор</button>
                    <button class="m-tab" onclick="modalTab('tab-bachelor', this)">Бакалавриат</button>
                    <button class="m-tab" onclick="modalTab('tab-master', this)">Магистратура</button>
                    <button class="m-tab" onclick="modalTab('tab-finance', this)">Бюджет</button>
                </div>
                
                <div class="modal-content">
                    <div id="tab-overview" class="tab-panel active">
                        <div class="info-grid">
                            <div class="info-card">
                                <div class="dd-info-label">Квота (мест)</div>
                                <div class="dd-info-value"><span id="m-students">2</span></div>
                            </div>
                            <div class="info-card">
                                <div class="dd-info-label">Язык</div>
                                <div class="dd-info-value">English B2</div>
                            </div>
                            <div class="info-card">
                                <div class="dd-info-label">Тип</div>
                                <div class="dd-info-value">Обмен</div>
                            </div>
                             <div class="info-card">
                                <div class="dd-info-label">Статус</div>
                                <div class="dd-info-value" style="color:var(--success)">Активный</div>
                            </div>
                        </div>
                        <p style="color:var(--text-light); font-size:0.9rem; margin-top:10px;">
                            <b>Tuition Waiver:</b> Вуз является стратегическим партнером. Студентам предоставляется возможность бесплатного обучения.
                        </p>
                    </div>

                    <div id="tab-bachelor" class="tab-panel">
                        <ul class="program-list-styled" id="list-bachelor"></ul>
                    </div>

                    <div id="tab-master" class="tab-panel">
                        <ul class="program-list-styled" id="list-master"></ul>
                    </div>

                    <div id="tab-finance" class="tab-panel">
                        <div class="info-card">
                            <div class="dd-info-label">Расходы в месяц (примерно)</div>
                            <div style="display:flex; justify-content:space-between; margin-top:10px; border-bottom:1px solid #eee; padding-bottom:5px;">
                                <span>Жилье</span> <span style="font-weight:bold" id="cost-living">€300-500</span>
                            </div>
                            <div style="display:flex; justify-content:space-between; margin-top:5px; padding-bottom:5px; border-bottom:1px solid #eee;">
                                <span>Питание</span> <span style="font-weight:bold" id="cost-food">€200-300</span>
                            </div>
                             <div style="display:flex; justify-content:space-between; margin-top:5px; padding-bottom:5px;">
                                <span>Транспорт</span> <span style="font-weight:bold">€30-50</span>
                            </div>
                        </div>
                        <div style="margin-top:15px; font-size:0.85rem; color:#666;">
                            <p><i class="fas fa-passport"></i> <b>Виза:</b> Требуется студенческая виза типа D. Подтверждение финансовой состоятельности обязательно.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Leaflet JS -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    
    <script>
    /* ---------------- DATA STORE (FULL DATA) ---------------- */
    const universities = [
        { region: "Europe", country: "Австрия", name: "Management Center Innsbruck", city: "Innsbruck", students: "2", lat:47.2682, lon:11.3923, bachelorPrograms: ["Management", "Business"], masterPrograms: ["International Business"] },
        { region: "CIS", country: "Азербайджан", name: "ADA University", city: "Baku", students: "5", lat:40.4093, lon:49.8671, bachelorPrograms: ["Business Administration","Economics","Finance","Computer Science","International Studies","Laws"], masterPrograms: ["Global Management","MBA Finance","Public Administration"] },
        { region: "CIS", country: "Азербайджан", name: "Khazar University", city: "Baku", students: "5", lat:40.4100, lon:49.8620, bachelorPrograms: ["BBA Management","Marketing","Finance","Accounting","Computer Science","Tourism","Political Science"], masterPrograms: ["MBA Management","MBA Project Management","Economics"] },
        { region: "CIS", country: "Азербайджан", name: "Azerbaijan University", city: "Baku", students: "5", lat:40.4120, lon:49.8700, bachelorPrograms: ["Marketing","Business Management","Accounting","Finance","IT","Tourism","International Trade"], masterPrograms: ["MBA Marketing","MBA Finance","Cybersecurity"] },
        { region: "CIS", country: "Беларусь", name: "Polotsk State University", city: "Polotsk", students: "2", lat:55.4850, lon:28.7800, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Бельгия", name: "Antwerp Management School", city: "Antwerp", students: "2-3", lat:51.2194, lon:4.4025, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Германия", name: "Cologne Business School", city: "Cologne", students: "-", lat:50.9375, lon:6.9603, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Германия", name: "Hof University of Applied Sciences", city: "Hof", students: "5", lat:50.3219, lon:11.9172, bachelorPrograms: ["International Management","Digital Business","Computer Science"], masterPrograms: ["Digitalization and Innovation","Software Engineering"] },
        { region: "CIS", country: "Грузия", name: "Caucasus University", city: "Tbilisi", students: "2", lat:41.7151, lon:44.8271, bachelorPrograms: ["Business Admin","Economics","Tourism","International Relations"], masterPrograms: ["Management","MBA","Public Administration"] },
        { region: "Africa", country: "Египет", name: "Galala University", city: "Al Galala", students: "-", lat:29.5261, lon:32.7794, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Гонконг", name: "The Hong Kong Polytechnic University", city: "Kowloon", students: "2", lat:22.3032, lon:114.1794, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Гонконг", name: "Lingnan University", city: "Tuen Mun", students: "2", lat:22.3837, lon:114.1305, bachelorPrograms: ["General Business","Management","Marketing","Accounting","Finance","Psychology"], masterPrograms: [] },
        { region: "Europe", country: "Испания", name: "IQS School of Management", city: "Barcelona", students: "2", lat:41.3910, lon:2.1651, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Испания", name: "EDEM Business School", city: "Valencia", students: "3", lat:39.4699, lon:-0.3763, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Латвия", name: "Daugavpils University", city: "Daugavpils", students: "2", lat:55.8728, lon:26.5259, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Латвия", name: "Riseba University", city: "Riga", students: "3", lat:56.9496, lon:24.1052, bachelorPrograms: ["European Business","Business Management","PR and Advertising"], masterPrograms: ["Management Science","HR Management","Project Management"] },
        { region: "Europe", country: "Латвия", name: "Baltic International Academy", city: "Riga", students: "3", lat:56.9496, lon:24.1052, bachelorPrograms: ["European Economics","Financial Management","Tourism","Law"], masterPrograms: [] },
        { region: "Europe", country: "Литва", name: "Vytautas Magnus University", city: "Kaunas", students: "2", lat:54.8985, lon:23.9036, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Литва", name: "ISM University of Management", city: "Vilnius", students: "2", lat:54.6872, lon:25.2797, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Малайзия", name: "Universiti Teknikal Mara / UniKL", city: "Kuala Lumpur", students: "5", lat:3.1390, lon:101.6869, bachelorPrograms: ["Management","BBA Marketing","Islamic Finance","Multimedia Design"], masterPrograms: ["MBA","Creative Digital Media"] },
        { region: "Europe", country: "Польша", name: "Vistula University", city: "Warsaw", students: "2", lat:52.2297, lon:21.0122, bachelorPrograms: ["Management","Computer Engineering","Tourism","International Relations"], masterPrograms: ["Economics, Finance and Accounting"] },
        { region: "Europe", country: "Польша", name: "Kozminski University", city: "Warsaw", students: "3", lat:52.2550, lon:21.0050, bachelorPrograms: ["Entrepreneurship","Marketing","Finance","Law"], masterPrograms: ["Finance and Accounting"] },
        { region: "Europe", country: "Польша", name: "Poznan University of Economics", city: "Poznan", students: "2", lat:52.4064, lon:16.9252, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Польша", name: "Wyzsa Szkola Biznesu", city: "Dąbrowa Górnicza", students: "2", lat:50.3200, lon:19.2390, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Португалия", name: "University of Minho", city: "Braga", students: "2", lat:41.5618, lon:-8.3967, bachelorPrograms: [], masterPrograms: [] },
        { region: "CIS", country: "Россия", name: "Kazan Federal University", city: "Kazan", students: "5", lat:55.7958, lon:49.1064, bachelorPrograms: ["Менеджмент","Экономика","Журналистика","Туризм","Юриспруденция"], masterPrograms: ["Management","IT","Law"] },
        { region: "CIS", country: "Россия", name: "Saint-Petersburg State University", city: "SPB", students: "2", lat:59.9343, lon:30.3351, bachelorPrograms: ["Международный менеджмент","Экономика","IT","Журналистика","Туризм"], masterPrograms: ["Менеджмент","Экономика"] },
        { region: "CIS", country: "Россия", name: "HSE (Вышка)", city: "Moscow", students: "2", lat:55.7558, lon:37.6173, bachelorPrograms: ["Маркетинг","Бизнес и экономика","Логистика","Медиакоммуникации","Политология"], masterPrograms: [] },
        { region: "Asia", country: "Тайвань", name: "National Taipei University", city: "New Taipei", students: "5", lat:25.0330, lon:121.5654, bachelorPrograms: [], masterPrograms: [] },
        { region: "CIS", country: "Таджикистан", name: "Tajik State University of Commerce", city: "Dushanbe", students: "10", lat:38.5598, lon:68.7870, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Турция", name: "Altinbas University", city: "Istanbul", students: "5", lat:41.0082, lon:28.9784, bachelorPrograms: ["Business Admin","Economics","Logistics","International Trade","Computer Engineering","Law"], masterPrograms: ["MBA","Economics","Logistics"] },
        { region: "Asia", country: "Турция", name: "Kadir Has University", city: "Istanbul", students: "2", lat:41.0168, lon:28.9714, bachelorPrograms: ["Business Admin","Economics","International Trade","New Media","Psychology"], masterPrograms: [] },
        { region: "Asia", country: "Турция", name: "Abdullah Gül University", city: "Kayseri", students: "5", lat:38.7312, lon:35.4878, bachelorPrograms: ["Business Admin","Economics","Political Science"], masterPrograms: [] },
        { region: "Asia", country: "Турция", name: "Istanbul Aydin University", city: "Istanbul", students: "2", lat:41.0086, lon:28.9855, bachelorPrograms: ["Business Management","Accounting","International Trade","Law"], masterPrograms: ["MBA","Finance"] },
        { region: "CIS", country: "Узбекистан", name: "Kimyo International University", city: "Tashkent", students: "10", lat:41.3111, lon:69.2797, bachelorPrograms: ["Business management","Marketing","Accounting","Banking","Finance","Tourism"], masterPrograms: ["Финансы","МБА","Маркетинг"] },
        { region: "CIS", country: "Узбекистан", name: "Inha University in Tashkent", city: "Tashkent", students: "5", lat:41.3120, lon:69.2785, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Филиппины", name: "Mapúa University", city: "Manila", students: "5", lat:14.5995, lon:120.9842, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Финляндия", name: "SeAMK", city: "Seinäjoki", students: "2", lat:62.7920, lon:22.8280, bachelorPrograms: [], masterPrograms: [] },
        { region: "Europe", country: "Франция", name: "Emlyon Business School", city: "Lyon", students: "4", lat:45.7640, lon:4.8357, bachelorPrograms: ["Global BBA","Data Science"], masterPrograms: ["Marketing","Data Science","Cybersecurity"] },
        { region: "Europe", country: "Франция", name: "EMLV", city: "Paris", students: "2", lat:48.8950, lon:2.2360, bachelorPrograms: ["Marketing Innovation","Digital Marketing","International Business"], masterPrograms: ["Corporate Finance","International Business"] },
        { region: "Europe", country: "Франция", name: "Yschools", city: "Troyes", students: "4", lat:48.2970, lon:4.0740, bachelorPrograms: ["Business Admin","Tourism Management"], masterPrograms: ["International management"] },
        { region: "Europe", country: "Франция", name: "ESC Rennes School of Business", city: "Rennes", students: "2", lat:48.1173, lon:-1.6778, bachelorPrograms: ["Global management","Marketing","Supply Chain"], masterPrograms: ["Management","Digital Marketing","Luxury Marketing","Data Analytics"] },
        { region: "Europe", country: "Франция", name: "Burgundy School of Business", city: "Dijon", students: "2", lat:47.3216, lon:5.0415, bachelorPrograms: ["International business"], masterPrograms: ["International business"] },
        { region: "Europe", country: "Франция", name: "IESEG School of Management", city: "Lille", students: "6", lat:50.6292, lon:3.0573, bachelorPrograms: ["Audit","International Economics","Entrepreneurship"], masterPrograms: [] },
        { region: "Europe", country: "Франция", name: "Université Gustave Eiffel", city: "Paris", students: "2", lat:48.8556, lon:2.5794, bachelorPrograms: ["Economics","International Management","Computer science"], masterPrograms: ["Finance","Business Management"] },
        { region: "Europe", country: "Франция", name: "Excelia Group", city: "La Rochelle", students: "3", lat:46.1580, lon:-1.1520, bachelorPrograms: ["Business Admin","Tourism"], masterPrograms: ["Luxury Management","Supply Chain","Sustainable Finance"] },
        { region: "Europe", country: "Хорватия", name: "University of Rijeka", city: "Rijeka", students: "2", lat:45.3271, lon:14.4422, bachelorPrograms: ["Economics and Business"], masterPrograms: [] },
        { region: "Europe", country: "Швейцария", name: "Geneva Business School", city: "Geneva", students: "2", lat:46.2044, lon:6.1432, bachelorPrograms: ["International Management","Digital Marketing","Finance"], masterPrograms: [] },
        { region: "Europe", country: "Швейцария", name: "EU Business School", city: "Geneva", students: "5", lat:46.2044, lon:6.1432, bachelorPrograms: ["Business Admin","Digital Marketing","Finance","Tourism"], masterPrograms: [] },
        { region: "Europe", country: "Швейцария", name: "Swiss Education Group", city: "Montreux", students: "2", lat:46.2100, lon:7.0700, bachelorPrograms: [], masterPrograms: [] },
        { region: "Americas", country: "Эквадор", name: "Universidad Internacional Del Ecuador", city: "Quito", students: "5", lat:-0.2299, lon:-78.5249, bachelorPrograms: [], masterPrograms: [] },
        { region: "Asia", country: "Корея", name: "Kyungdong University", city: "Wonju", students: "5", lat:37.8820, lon:128.8250, bachelorPrograms: ["Business Administration","Hotel Management"], masterPrograms: [] },
        { region: "Asia", country: "Япония", name: "Nagoya University of Commerce", city: "Nagoya", students: "2", lat:35.1815, lon:136.9066, bachelorPrograms: ["Business","Entrepreneurial Management","Marketing","Finance"], masterPrograms: ["Management","Accounting"] }
    ];

    const steps = [
        { num: 1, title: "Выбор программы", desc: "Изучите карту партнеров и выберите 3 приоритетных вуза. Проверьте соответствие специальностей на сайте партнера." },
        { num: 2, title: "Сбор документов", desc: "Подготовьте пакет: Транскрипт (GPA 3.0+), 2 рекомендации, Мотивационное письмо, Согласие родителей. Шаблоны в @global_almau." },
        { num: 3, title: "Онлайн подача", desc: "Загрузите PDF-сканы всех документов через официальную форму (ссылка доступна в период приема заявок)." },
        { num: 4, title: "Интервью", desc: "Пройдите собеседование на английском языке (или русском для стран СНГ). Комиссия оценивает мотивацию и язык." },
        { num: 5, title: "Номинация", desc: "После успешного отбора AlmaU номинирует вас в вуз-партнер. Ждите письмо с инструкциями от принимающего вуза." },
        { num: 6, title: "Виза и Выезд", desc: "Получите приглашение, оформите визу типа D, страховку и форму перезачета кредитов (Learning Agreement)." },
        { num: 7, title: "Обучение и Отчет", desc: "Учитесь, сдавайте экзамены. По приезду сдайте транскрипт в офис 107 для перезачета оценок." }
    ];

    const faqData = [
        {
            category: "Документы и Требования",
            items: [
                { q: "Какой минимальный GPA и уровень языка?", a: "Минимальный GPA — 3.0. Уровень английского — B2 (Upper-Intermediate). Подтверждается сертификатом IELTS/TOEFL или внутренним тестом." },
                { q: "Какие документы обязательны?", a: "1. Транскрипт (на английском). 2. Мотивационное письмо. 3. Два рекомендательных письма от преподавателей. 4. Согласие родителей. 5. Копия паспорта." },
                { q: "Нужен ли перевод документов?", a: "Для вузов СНГ — нет. Для дальнего зарубежья все документы (рекомендации, мотивация) должны быть на английском языке." }
            ]
        },
        {
            category: "Финансы и Гранты",
            items: [
                { q: "Сколько стоит обучение?", a: "По программе обмена обучение в вузе-партнере БЕСПЛАТНОЕ. Студент оплачивает только текущий семестр в AlmaU." },
                { q: "Кто оплачивает проживание и перелет?", a: "Все сопутствующие расходы (виза, перелет, жилье, страховка, питание) оплачивает студент самостоятельно, если нет гранта." },
                { q: "Есть ли стипендии?", a: "Да, существуют гранты МНВО РК (покрывают перелет и проживание). Конкурс проходит отдельно, обычно в мае. Следите за анонсами." }
            ]
        },
        {
            category: "Учебный процесс",
            items: [
                { q: "Можно ли с академической задолженностью?", a: "Нет. На момент подачи и выезда у студента не должно быть FX/F оценок. Ритейки должны быть закрыты." },
                { q: "Что такое Double Degree?", a: "Это программа двойного диплома. Доступна для определенных специальностей (Менеджмент, Маркетинг). Обучение в вузе-партнере платное." }
            ]
        }
    ];
    
    const doubleDegreePrograms = [
        // Бакалавриат (Bachelor)
        { level: "Бакалавриат", partner: "Thunderbird School of Global Management, Arizona State University", country: "США", programs: ["Bachelor of Science in International Trade", "Bachelor of Global Management"] },
        { level: "Бакалавриат", partner: "ESC Rennes School of Business", country: "Франция", programs: ["International Bachelor in Management"] },
        { level: "Бакалавриат", partner: "EU Business School", country: "Швейцария", programs: ["Bachelor of Arts in Leisure and Tourism Management", "Bachelor of Business Administration"] },
        { level: "Бакалавриат", partner: "Geneva Business School", country: "Швейцария", programs: ["Bachelor in Management", "Bachelor in Finance"] },
        { level: "Бакалавриат", partner: "SolBridge International School of Business", country: "Южная Корея", programs: ["BBA in Data Analytics", "BBA in Marketing, Technology and Innovation", "BBA in Management and Entrepreneurship", "BBA in Finance"] },
        { level: "Бакалавриат", partner: "Cesine Design and Business School", country: "Испания", programs: ["International Business Management", "Advertising, Marketing Communications & Public Relations", "Hospitality & Travel Management"] },
        { level: "Бакалавриат", partner: "Hof University of Applied Sciences", country: "Германия", programs: ["Bachelor Degree in Business Information Systems"] },
        { level: "Бакалавриат", partner: "Kyungdong University Global Campus", country: "Южная Корея", programs: ["Bachelor of Business Administration", "Bachelor of Computer Engineering in Smart Computing", "Bachelor of Business Administration in Hotel Management", "Bachelor of Korean Studies"] },
        { level: "Бакалавриат", partner: "Swiss Hotel Management School", country: "Швейцария", programs: ["Bachelor in Hospitality"] },
        { level: "Бакалавриат", partner: "Cesar Ritz Colleges Switzerland", country: "Швейцария", programs: ["Bachelor in Hospitality"] },
        { level: "Бакалавриат", partner: "Hotel Institute Montreux", country: "Швейцария", programs: ["Bachelor in Hospitality"] },
        
        // Магистратура (Master)
        { level: "Магистратура", partner: "IE University Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "EU Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "EDEM Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "Hof University", country: "Германия", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "Zhejiang University", country: "Китай", programs: ["Уточняйте программы в Международном Офисе AlmaU"] }
    ];

    /* ---------------- APP LOGIC ---------------- */
    let map;
    let markers = L.layerGroup();
    let currentFilter = 'all';

    // Telegram App Integration
    const tg = window.Telegram?.WebApp;

    function initTelegram() {
        if (tg) {
            tg.ready();
            tg.expand(); // Fullscreen
            
            // Set header color to match app
            if (tg.setHeaderColor) tg.setHeaderColor('#ffffff'); 
            if (tg.setBackgroundColor) tg.setBackgroundColor('#f4f6f9');

            // Handle Back Button specifically for Modals
            tg.BackButton.onClick(function() {
                // If modal is open, close it
                if(document.getElementById('uniModal').classList.contains('open')) {
                    closeModal();
                } else {
                    // Default behavior (close app?) - optional, usually just hide button
                    tg.BackButton.hide();
                }
            });
        }
    }

    const GoldenIcon = L.DivIcon.extend({
        options: {
            iconSize: [30, 30],
            iconAnchor: [15, 30], 
            popupAnchor: [0, -25], 
            className: 'golden-marker-icon',
            html: '<i class="fa fa-map-marker-alt"></i>'
        }
    });
    const goldenIcon = new GoldenIcon();

    function initMap() {
        if (document.getElementById('map')) {
            // Center map initially (generic Europe/Asia view)
            map = L.map('map', { zoomControl: false }).setView([48, 30], 3); 
            
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
                attribution: '',
                maxZoom: 19
            }).addTo(map);

            map.addLayer(markers);
            
            // Add zoom control to bottom right for mobile ergonomics
            L.control.zoom({ position: 'bottomright' }).addTo(map);
        }
    }

    function init() {
        initTelegram();
        initMap();
        renderList(universities);
        renderTimeline();
        renderFAQ();
        renderDoubleDegreePrograms();
        
        // Force activation of "Все" filter button visual state
        const allBtn = document.querySelector('.filter-tag');
        if(allBtn) allBtn.classList.add('active');
    }

    /* ---------------- RENDERING ---------------- */
    function renderList(data) {
        const uniListContainer = document.getElementById('uni-list-container');
        uniListContainer.innerHTML = '';
        markers.clearLayers();

        document.getElementById('count-display').textContent = `${data.length} ВУЗов`;

        data.forEach(u => {
            if (u.lat && u.lon) {
                const marker = L.marker([u.lat, u.lon], { icon: goldenIcon })
                    .bindTooltip(`<b>${u.name}</b>`, { direction: 'top', offset: [0, -25] })
                    .on('click', () => {
                        openModal(u);
                    });
                markers.addLayer(marker);
            }

            const div = document.createElement('div');
            div.className = 'uni-item';
            div.innerHTML = `
                <span class="uni-name">${u.name}</span>
                <div class="uni-meta">
                    <span>${u.city}, ${u.country}</span>
                    <span class="rank-badge">${u.students} мест</span>
                </div>
            `;
            div.onclick = () => {
                // On mobile, scroll to map first
                if (window.innerWidth < 900) {
                    document.getElementById('map-section').scrollIntoView({ behavior: 'smooth' });
                }
                if (map) {
                    map.flyTo([u.lat, u.lon], 6, { duration: 1.5 });
                    setTimeout(() => openModal(u), 1000);
                } else {
                    openModal(u);
                }
            };
            uniListContainer.appendChild(div);
        });

        // Smart Map Fitting
        if (map && data.length > 0) {
            const bounds = data.map(uni => [uni.lat, uni.lon]);
            if(bounds.length > 0) map.fitBounds(bounds, { padding: [50, 50], maxZoom: 5 });
        }
    }

    function filterList(region, btn) {
        if (btn) {
            document.querySelectorAll('.filter-tag').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            currentFilter = region;
        }

        const term = document.getElementById('searchBox').value.toLowerCase();
        
        const filtered = universities.filter(u => {
            const matchRegion = region === 'all' || u.region === region;
            const matchSearch = u.name.toLowerCase().includes(term) || 
                                u.country.toLowerCase().includes(term) ||
                                (u.bachelorPrograms && u.bachelorPrograms.join(' ').toLowerCase().includes(term));
            return matchRegion && matchSearch;
        });

        renderList(filtered);
    }

    document.getElementById('searchBox').addEventListener('input', () => {
        const activeBtn = document.querySelector('.filter-tag.active');
        filterList(activeBtn ? (activeBtn.textContent.trim() === 'Все' ? 'all' : activeBtn.textContent.trim()) : 'all', null);
    });

    /* ---------------- COMPONENTS ---------------- */
    function renderTimeline() {
        document.getElementById('timeline-container').innerHTML = steps.map(step => `
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="timeline-content">
                    <span class="step-num">Этап ${step.num}</span>
                    <div style="font-weight:bold; font-size:1rem; margin-top:5px; margin-bottom:5px;">${step.title}</div>
                    <div style="font-size:0.9rem; color:#666;">${step.desc}</div>
                </div>
            </div>
        `).join('');
    }

    function renderFAQ() {
        document.getElementById('faq-root').innerHTML = faqData.map(cat => `
            <div class="faq-category">
                <h3 class="cat-title">${cat.category}</h3>
                ${cat.items.map(item => `
                    <div class="faq-card">
                        <div class="faq-header" onclick="this.parentElement.classList.toggle('open')">
                            <span>${item.q}</span>
                            <i class="fas fa-chevron-down"></i>
                        </div>
                        <div class="faq-body"><p style="margin:10px 0;">${item.a}</p></div>
                    </div>
                `).join('')}
            </div>
        `).join('');
    }

    function renderDoubleDegreePrograms() {
        const container = document.getElementById('double-degree-content');
        container.innerHTML = '';
        const levels = [...new Set(doubleDegreePrograms.map(p => p.level))];
        
        levels.forEach(level => {
            let html = `<h3 style="font-size:1.1rem; color:var(--secondary); margin:30px 0 15px; border-bottom:1px solid #ccc; display:inline-block; padding-bottom:5px;">${level}</h3>
                        <div class="double-degree-grid">`;
            doubleDegreePrograms.filter(p => p.level === level).forEach(p => {
                html += `
                    <div class="info-card">
                        <div class="dd-info-label">${p.country}</div>
                        <div class="dd-info-value" style="font-size:1.05rem;">${p.partner}</div>
                        <ul class="program-list-styled-dd">${p.programs.map(pr => `<li>${pr}</li>`).join('')}</ul>
                    </div>`;
            });
            html += `</div>`;
            container.innerHTML += html;
        });
    }

    /* ---------------- MODAL LOGIC ---------------- */
    function openModal(u) {
        document.getElementById('m-name').textContent = u.name;
        document.getElementById('m-city').textContent = u.city;
        document.getElementById('m-country').textContent = u.country;
        document.getElementById('m-students').textContent = u.students;

        const fill = (id, arr) => {
            const el = document.getElementById(id);
            el.innerHTML = (!arr || arr.length === 0) 
                ? '<li style="color:#999; padding-left:0;"><i class="fas fa-exclamation-circle" style="color:#e94e4e; margin-right: 8px;"></i>Информация уточняется в офисе.</li>' 
                : arr.map(t => `<li>${t}</li>`).join('');
        };
        fill('list-bachelor', u.bachelorPrograms);
        fill('list-master', u.masterPrograms);

        // Dynamic Cost Logic (Estimation based on country for demo purposes)
        let rent = "€300-500";
        if(u.country === "Франция" || u.country === "Германия" || u.country === "Австрия" || u.country === "Бельгия") rent = "€500-800";
        else if(u.country === "Польша" || u.country === "Литва" || u.country === "Латвия" || u.country === "Турция") rent = "€250-450";
        else if(u.country === "Швейцария") rent = "CHF 700-1200";
        
        document.getElementById('cost-living').textContent = rent;

        // Show Modal
        document.getElementById('uniModal').classList.add('open');
        modalTab('tab-overview', document.querySelector('.modal-nav .m-tab:first-child'));
        
        // Show Telegram Back Button
        if (tg && tg.BackButton) tg.BackButton.show();
    }

    function closeModal() {
        document.getElementById('uniModal').classList.remove('open');
        // Hide Telegram Back Button
        if (tg && tg.BackButton) tg.BackButton.hide();
    }

    function modalTab(id, btn) {
        document.querySelectorAll('.modal-nav .m-tab').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    /* ---------------- TAB SWITCHING ---------------- */
    function switchTab(id, btn) {
        document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        
        // Scroll navigation into view on mobile
        if(window.innerWidth < 900) {
            btn.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
        }

        if(id === 'map-section') setTimeout(() => { if(map) map.invalidateSize(); }, 200);
    }

    // Initialize
    init();
    </script>
</body>
</html>
