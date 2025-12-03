<html lang="ru">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    
    <!-- PWA / Mobile App Settings -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#ffffff">

    <title>AlmaU Global Mobility Portal | Official Resource</title>
    
    <!-- Libraries -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@300;700&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
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
            --radius: 12px;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow-x: hidden; /* CRITICAL FIX: Prevents side-scrolling overflow */
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            line-height: 1.6;
        }

        h1, h2, h3, h4 { font-family: 'Merriweather', serif; color: var(--primary); margin-top: 0; }

        /* HEADER */
        .main-header {
            background: var(--white);
            border-bottom: 1px solid var(--border);
            padding: 12px 0;
            position: sticky; top: 0; z-index: 1000;
            width: 100%;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 0 16px;
        }

        .header-flex {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 15px;
        }
        
        .logo-area { display: flex; align-items: center; gap: 12px; flex-shrink: 0; }
        .logo-box {
            width: 40px; height: 40px; background: var(--primary); color: var(--white);
            font-family: 'Merriweather'; font-weight: bold; font-size: 20px;
            display: flex; align-items: center; justify-content: center;
            border-radius: 6px; border: 2px solid var(--accent);
        }
        .brand-text h1 { font-size: 1.1rem; margin: 0; line-height: 1.1; }
        .brand-text p { margin: 0; font-size: 0.65rem; color: var(--text-light); text-transform: uppercase; letter-spacing: 0.5px; }

        /* APP-STYLE SCROLLABLE NAVIGATION */
        .nav-scroller {
            width: 100%;
            overflow-x: auto;
            white-space: nowrap;
            -webkit-overflow-scrolling: touch; /* Smooth iOS scroll */
            padding: 8px 16px;
            background: var(--white);
            border-bottom: 1px solid var(--border);
            /* Hide scrollbar */
            scrollbar-width: none; 
            -ms-overflow-style: none;
            position: sticky;
            top: 65px; /* Stick below header */
            z-index: 999;
        }
        .nav-scroller::-webkit-scrollbar { display: none; }

        .nav-menu { display: inline-flex; gap: 10px; }
        .nav-item {
            text-decoration: none; color: var(--text-dark); font-weight: 500; font-size: 0.9rem;
            padding: 8px 16px; border-radius: 20px; background: #f0f4f8; transition: all 0.2s;
            cursor: pointer;
        }
        .nav-item.active { background: var(--primary); color: var(--white); box-shadow: 0 2px 4px rgba(0,40,85,0.2); }

        /* CONTENT LAYOUT */
        .content-section { display: none; padding-bottom: 80px; animation: fadeIn 0.3s; width: 100%; }
        .content-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        /* MAP & LIST DASHBOARD */
        .dashboard-grid {
            display: flex; flex-direction: column; gap: 15px; margin-top: 15px;
        }

        .map-panel {
            height: 350px; width: 100%; 
            border-radius: var(--radius); overflow: hidden; 
            border: 1px solid var(--border); position: relative; z-index: 1;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }
        #map { width: 100%; height: 100%; }

        /* SEARCH BAR */
        .search-container { position: relative; }
        .search-input {
            width: 100%; padding: 12px 12px 12px 45px; border: 1px solid #ccc; border-radius: 12px;
            font-size: 0.95rem; outline: none; -webkit-appearance: none;
            background: #fff; box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        .search-icon { position: absolute; left: 15px; top: 50%; transform: translateY(-50%); color: #999; }

        /* FILTER CHIPS SCROLLER */
        .filter-scroller {
            display: flex; gap: 8px; overflow-x: auto; padding-bottom: 5px; 
            -webkit-overflow-scrolling: touch; scrollbar-width: none;
        }
        .filter-scroller::-webkit-scrollbar { display: none; }
        .filter-tag {
            font-size: 0.8rem; padding: 6px 14px; background: #eef2f6; border-radius: 16px;
            cursor: pointer; white-space: nowrap; border: 1px solid transparent;
        }
        .filter-tag.active { background: var(--primary); color: var(--white); }

        /* UNIVERSITY LIST */
        .uni-list { 
            max-height: 500px; overflow-y: auto; background: var(--white); 
            border-radius: var(--radius); border: 1px solid var(--border); 
        }
        .uni-item {
            padding: 15px 16px; border-bottom: 1px solid #eee; display: flex; 
            justify-content: space-between; align-items: flex-start;
            cursor: pointer;
        }
        .uni-item:active { background: #f5f5f5; }
        .uni-name { font-weight: 700; color: var(--primary); display: block; margin-bottom: 4px; line-height: 1.3; }
        .uni-meta { font-size: 0.85rem; color: var(--text-light); }
        .rank-badge { 
            background: var(--accent); color: var(--white); padding: 3px 8px; 
            border-radius: 4px; font-size: 0.75rem; font-weight: bold; white-space: nowrap; margin-left: 10px;
        }

        /* CARD STYLES (DD & FAQ) */
        .info-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            padding: 16px; margin-bottom: 15px; box-shadow: 0 1px 3px rgba(0,0,0,0.03);
        }
        .dd-info-label { font-size: 0.75rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .dd-info-value { font-size: 1.05rem; font-weight: 600; color: var(--primary); line-height: 1.3; }
        
        .program-list-styled-dd { list-style: none; padding: 0; margin-top: 10px; }
        .program-list-styled-dd li {
            padding: 8px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start;
            font-size: 0.9rem; color: #444; word-break: break-word; /* FIXES OVERFLOW */
        }
        .program-list-styled-dd li:last-child { border-bottom: none; }
        .program-list-styled-dd li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 10px; color: var(--accent); font-size: 0.8rem; margin-top: 3px; flex-shrink: 0;
        }

        /* FAQ STYLES (FIXED) */
        .faq-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            margin-bottom: 10px; overflow: hidden; width: 100%;
        }
        .faq-header {
            padding: 16px; display: flex; justify-content: space-between; align-items: center;
            font-weight: 600; font-size: 0.95rem; cursor: pointer; width: 100%;
        }
        .faq-header span {
            padding-right: 15px; line-height: 1.4;
        }
        .faq-body {
            max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out;
            background: #fafafa; padding: 0 16px; font-size: 0.9rem; color: #444; line-height: 1.6;
        }
        .faq-card.open .faq-body { padding: 16px; max-height: 1000px; border-top: 1px solid #eee; }

        /* TIMELINE (FIXED) */
        .timeline { position: relative; margin: 20px 0 20px 10px; padding-left: 20px; border-left: 2px solid var(--border); }
        .timeline-item { position: relative; margin-bottom: 25px; }
        .timeline-dot {
            position: absolute; left: -26px; top: 0; width: 14px; height: 14px; background: var(--white);
            border: 3px solid var(--accent); border-radius: 50%;
        }
        .step-num {
            display: inline-block; background: var(--secondary); color: var(--white);
            padding: 2px 8px; border-radius: 4px; font-weight: bold; font-size: 0.7rem; margin-bottom: 6px;
        }

        /* MODAL (Passport Style) */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.6); z-index: 2000;
            display: none; justify-content: center; align-items: flex-end; /* Slide from bottom */
        }
        .modal-overlay.open { display: flex; }
        
        .modal-window {
            background: var(--white); width: 100%; height: 92vh; /* Tall modal */
            border-radius: 16px 16px 0 0; display: flex; flex-direction: column;
            animation: slideUp 0.3s ease-out; overflow: hidden;
        }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .modal-header {
            background: var(--primary); color: var(--white); padding: 20px;
            display: flex; justify-content: space-between; align-items: flex-start; flex-shrink: 0;
        }
        .modal-tabs {
            background: #f4f6f9; display: flex; overflow-x: auto; flex-shrink: 0;
            border-bottom: 1px solid var(--border);
        }
        .m-tab {
            padding: 14px 20px; cursor: pointer; font-weight: 500; color: var(--text-light);
            background: none; border: none; white-space: nowrap; border-bottom: 3px solid transparent;
        }
        .m-tab.active { background: var(--white); color: var(--primary); border-bottom-color: var(--accent); font-weight: 700; }
        
        .modal-content { flex: 1; overflow-y: auto; padding: 20px; -webkit-overflow-scrolling: touch; }
        .tab-panel { display: none; }
        .tab-panel.active { display: block; }
        
        .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }

        /* Leaflet Marker Fix */
        .golden-marker-icon {
            background-color: transparent !important; border: none !important;
            color: var(--accent); font-size: 30px; text-shadow: 0 0 3px rgba(0,0,0,0.5);
        }
    </style>
</head>
<body>

    <header class="main-header">
        <div class="container header-flex">
            <div class="logo-area">
                <div class="logo-box">A</div>
                <div class="brand-text">
                    <h1>AlmaU Global</h1>
                    <p>International Cooperation Department</p>
                </div>
            </div>
        </div>
    </header>

    <!-- HORIZONTAL SCROLL NAV -->
    <div class="nav-scroller">
        <nav class="nav-menu">
            <a class="nav-item active" onclick="switchTab('map-section', this)">Карта Партнеров</a>
            <a class="nav-item" onclick="switchTab('double-degree-section', this)">Двойной Диплом</a>
            <a class="nav-item" onclick="switchTab('plan-section', this)">Дорожная Карта</a>
            <a class="nav-item" onclick="switchTab('faq-section', this)">База Знаний (FAQ)</a>
        </nav>
    </div>

    <div class="container">
        
        <!-- === MAP SECTION === -->
        <section id="map-section" class="content-section active">
            <div class="dashboard-grid">
                
                <div class="search-container">
                    <i class="fas fa-search search-icon"></i>
                    <input type="text" id="searchBox" class="search-input" placeholder="Поиск по стране, ВУЗу...">
                </div>

                <div class="filter-scroller">
                    <span class="filter-tag active" onclick="filterList('all', this)">Все</span>
                    <span class="filter-tag" onclick="filterList('Europe', this)">Европа</span>
                    <span class="filter-tag" onclick="filterList('Asia', this)">Азия</span>
                    <span class="filter-tag" onclick="filterList('CIS', this)">СНГ</span>
                    <span class="filter-tag" onclick="filterList('Americas', this)">Америка</span>
                    <span class="filter-tag" onclick="filterList('Africa', this)">Африка</span>
                </div>

                <div class="map-panel">
                    <div id="map"></div>
                </div>

                <div style="padding: 10px 0; font-size: 0.9rem; color: #666; display: flex; justify-content: space-between;">
                    <span><i class="fas fa-list"></i> Список ВУЗов</span>
                    <span id="count-display">0 найдено</span>
                </div>

                <div class="uni-list" id="uni-list-container">
                    <!-- List items generated by JS -->
                </div>
            </div>
        </section>
        
        <!-- === DOUBLE DEGREE SECTION === -->
        <section id="double-degree-section" class="content-section">
            <div style="text-align: center; margin: 30px 0 20px;">
                <h2 style="font-size: 1.4rem;">Программа Двойного Диплома</h2>
                <p style="color:var(--text-light); font-size: 0.9rem;">Получите два диплома, обучаясь в AlmaU и ведущих университетах мира.</p>
            </div>
            
            <div id="double-degree-content">
                <!-- DD Cards generated by JS -->
            </div>
            
            <div class="info-card" style="background: #e6f0fa; border-left: 4px solid var(--accent);">
                <h4 style="margin-top: 0; color: var(--primary);"><i class="fas fa-info-circle"></i> Важно знать:</h4>
                <ul style="padding-left: 20px; font-size: 0.9rem; color: #444; margin-bottom: 0;">
                    <li style="margin-bottom: 8px;">Программа обычно предполагает обучение <strong>1-2 года</strong> в вузе-партнере.</li>
                    <li style="margin-bottom: 8px;">Обучение в вузе-партнере является <strong>платным</strong> (согласно тарифам партнера).</li>
                    <li>Требуется высокий GPA и знание языка.</li>
                </ul>
            </div>
        </section>

        <!-- === ROADMAP SECTION === -->
        <section id="plan-section" class="content-section">
            <div style="margin: 30px 0 20px;">
                <h2 style="font-size: 1.4rem;">Процесс подачи заявки</h2>
                <p style="color:var(--text-light); font-size: 0.9rem;">Официальный регламент прохождения конкурса</p>
            </div>
            
            <div class="timeline">
                <div id="timeline-container">
                    <!-- Steps generated by JS -->
                </div>
            </div>
        </section>

        <!-- === FAQ SECTION === -->
        <section id="faq-section" class="content-section">
            <div style="margin: 30px 0 20px;">
                <h2 style="font-size: 1.4rem;">Вопросы и Ответы</h2>
                <p style="color:var(--text-light); font-size: 0.9rem;">Вся информация об академической мобильности в одном месте</p>
            </div>
            <div id="faq-root">
                <!-- FAQ generated by JS -->
            </div>
        </section>

    </div>

    <!-- === MODAL === -->
    <div class="modal-overlay" id="uniModal">
        <div class="modal-window">
            <div class="modal-header">
                <div class="m-title" style="flex:1; padding-right:10px;">
                    <h2 id="m-name" style="color: white; font-size: 1.3rem; margin: 0; line-height: 1.2;">University Name</h2>
                    <div style="color: var(--accent); margin-top: 5px; font-size: 0.9rem;">
                        <i class="fas fa-map-marker-alt"></i> <span id="m-city">City</span>, <span id="m-country">Country</span>
                    </div>
                </div>
                <i class="fas fa-times" onclick="closeModal()" style="font-size: 1.5rem; cursor: pointer;"></i>
            </div>
            
            <div class="modal-tabs">
                <button class="m-tab active" onclick="modalTab('tab-overview', this)">Обзор</button>
                <button class="m-tab" onclick="modalTab('tab-bachelor', this)">Бакалавриат</button>
                <button class="m-tab" onclick="modalTab('tab-master', this)">Магистратура</button>
                <button class="m-tab" onclick="modalTab('tab-finance', this)">Бюджет</button>
            </div>
            
            <div class="modal-content">
                
                <div id="tab-overview" class="tab-panel active">
                    <div class="info-grid">
                        <div class="info-card" style="margin:0; text-align:center;">
                            <div class="dd-info-label">Квота (мест)</div>
                            <div class="dd-info-value" style="font-size:1.5rem;"><span id="m-students">0</span></div>
                        </div>
                        <div class="info-card" style="margin:0; text-align:center;">
                            <div class="dd-info-label">Язык</div>
                            <div class="dd-info-value">Eng B2</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 20px;">
                        <h4 style="border-bottom: 2px solid var(--accent); display: inline-block; padding-bottom: 5px;">Тип программы</h4>
                        <p style="font-size: 0.95rem; color: #444; margin-top: 10px;">
                            Семестровый обмен. Вуз является стратегическим партнером AlmaU. Студентам предоставляется возможность бесплатного обучения (<strong>Tuition Waiver</strong>).
                        </p>
                    </div>
                </div>

                <div id="tab-bachelor" class="tab-panel">
                    <h4>Доступные дисциплины</h4>
                    <ul class="program-list-styled-dd" id="list-bachelor"></ul>
                </div>

                <div id="tab-master" class="tab-panel">
                    <h4>Доступные дисциплины</h4>
                    <ul class="program-list-styled-dd" id="list-master"></ul>
                </div>

                <div id="tab-finance" class="tab-panel">
                    <div class="info-card">
                        <h4><i class="fas fa-wallet"></i> Расходы (в месяц)</h4>
                        <p style="font-size:0.8rem; color:#888;">*Примерные данные</p>
                        
                        <div style="display:flex; justify-content:space-between; padding: 10px 0; border-bottom:1px solid #eee;">
                            <span>Жилье</span>
                            <span style="font-weight:bold" id="cost-living">€300-500</span>
                        </div>
                        <div style="display:flex; justify-content:space-between; padding: 10px 0; border-bottom:1px solid #eee;">
                            <span>Питание</span>
                            <span style="font-weight:bold">€200-300</span>
                        </div>
                        <div style="display:flex; justify-content:space-between; padding: 10px 0;">
                            <span>Транспорт</span>
                            <span style="font-weight:bold">€30-50</span>
                        </div>
                    </div>
                    <div style="margin-top: 20px;">
                        <h4><i class="fas fa-passport"></i> Виза</h4>
                        <p style="font-size:0.9rem; color:#444;">Требуется студенческая виза типа D. Подтверждение финансовой состоятельности обязательно.</p>
                    </div>
                </div>

            </div>
        </div>
    </div>

    <!-- Scripts -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
    /* ---------------- FULL DATA ---------------- */
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

    /* ---------------- NEW FAQ DATA (25 QUESTIONS) ---------------- */
    const faqData = [
        {
            category: "Общие вопросы (General)",
            items: [
                { q: "Что такое академическая мобильность?", a: "Академическая мобильность – это возможность для студентов обучаться в другой стране на протяжении 1 семестра и более и получить по завершению транскрипт и/или сертификат вуза-партнера." },
                { q: "Что такое двойной диплом?", a: "Двойной диплом – это программа, которая позволяет обучаться в другой стране на протяжении 1 года или более и получить диплом зарубежного вуза." },
                { q: "Что такое летние и зимние школы?", a: "Летние и зимние школы – это краткосрочные программы мобильности, которые длятся от 2 недель до 3 месяцев, где Вы получите сертификат за участие." },
                { q: "Какие вузы мне подходят?", a: "С информацией по академической мобильности можно ознакомиться на сайте AlmaU. Для двойного диплома необходимо заходить на сайт вуза-партнера, чтобы посмотреть свою специальность." }
            ]
        },
        {
            category: "Требования к участникам",
            items: [
                { q: "Какие требования к участнику?", a: "1) Студент 1, 2, 3 курса бакалавриата или магистрант 1 курса. 2) GPA не ниже 3.0. 3) Отсутствие академической задолженности. 4) Английский язык – Upper-intermediate (В2)." },
                { q: "Если GPA ниже 3.0, могу ли я участвовать?", a: "Да, можете. Однако, это на рассмотрении Вашей Школы и комиссии. Они вправе отказать по итогам собеседования." },
                { q: "Если у меня есть академическая задолженность?", a: "Да, можете, при условии закрытия задолженности до отъезда. Решение принимает комиссия и Школа." },
                { q: "Если уровень английского ниже В2?", a: "Да, можете. Решение принимает комиссия по результатам собеседования внутри университета." },
                { q: "Кто может участвовать в обмене за счет МНВО?", a: "Студенты, обучающиеся на государственном гранте." },
                { q: "Кто может участвовать в Erasmus+?", a: "Все студенты AlmaU." }
            ]
        },
        {
            category: "Финансы и Типы программ",
            items: [
                { q: "Обучение в вузе-партнере платное?", a: "По академической мобильности вы НЕ платите вузу-партнеру, но продолжаете платить AlmaU (кроме грантников - они не платят никому). По двойному диплому вы платите вузу-партнеру и освобождаетесь от оплаты AlmaU." },
                { q: "Какие расходы я буду нести?", a: "Студент оплачивает: проживание, питание, страховку, визу, перелет туда-обратно и личные расходы." },
                { q: "Что такое обмен за счет самофинансирования?", a: "Это обучение за счет собственных средств студента (все расходы на проживание и перелет оплачивает студент)." },
                { q: "Что такое обмен за счет средств МНВО?", a: "Это грант Министерства для студентов-грантников. Он частично покрывает расходы (перелет, проживание, виза). Актуально только для осеннего семестра." },
                { q: "Что такое обмен Erasmus+?", a: "Европейская программа, где расходы полностью или частично покрываются грантом ЕС." }
            ]
        },
        {
            category: "Документы",
            items: [
                { q: "Какие документы необходимо собрать?", a: "1. Форма заявления. 2. Мотивационное письмо. 3. Транскрипт (каб. 111). 4. Два рекомендательных письма. 5. Согласие родителей. 6. Копия паспорта. 7. ИУП. 8. Сертификаты (при наличии)." },
                { q: "Мотивационное письмо нужно писать на 3 вуза?", a: "Нет, только на тот вуз-партнер, который у Вас стоит в приоритете (№1)." },
                { q: "Согласие родителей от обоих родителей?", a: "Нет, достаточно от одного родителя или законного представителя." },
                { q: "Если нет паспорта, что делать?", a: "Временно можно прикрепить копию удостоверения личности в PDF." },
                { q: "Обязательно ли сдавать IELTS/TOEFL?", a: "Нет, необязательно (если нет требования партнера)." },
                { q: "Как узнать свой GPA?", a: "GPA указан в транскрипте (каб. 111). Смотреть нужно GPA за весь период обучения." },
                { q: "Где найти средний балл аттестата?", a: "В личном кабинете на egov.kz или в системе hero.study (раздел документы)." }
            ]
        },
        {
            category: "Процесс и Технические вопросы",
            items: [
                { q: "Что делать, если не могу зайти по ссылке?", a: "Авторизуйтесь через почту @almau.edu.kz. Попробуйте с компьютера. Если не выходит - обратитесь в 103 кабинет." },
                { q: "Можно ли отказаться после собеседования?", a: "Можно, но если вы откажетесь ПОСЛЕ получения приглашения от партнера, вы лишаетесь права на мобильность в будущем." },
                { q: "Можно ли участвовать повторно?", a: "Да, можно. Но если были долги с прошлой мобильности или это последний курс - решение принимает комиссия." }
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
    // Telegram App Integration
    const tg = window.Telegram?.WebApp;
    if (tg) {
        tg.ready();
        tg.expand();
        tg.enableClosingConfirmation();
        tg.BackButton.onClick(() => closeModal());
    }

    let map;
    let markers = L.layerGroup();

    // Map Init
    function initMap() {
        if (document.getElementById('map')) {
            map = L.map('map', { zoomControl: false }).setView([48, 20], 3);
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
                attribution: '', maxZoom: 19
            }).addTo(map);
            map.addLayer(markers);
            L.control.zoom({ position: 'bottomright' }).addTo(map);
        }
    }

    // App Init
    function init() {
        initMap();
        renderList(universities);
        renderTimeline();
        renderFAQ();
        renderDD();
        // Activate "All" filter visual
        const allBtn = document.querySelector('.filter-tag:first-child');
        if(allBtn) allBtn.classList.add('active');
    }

    /* ---------------- RENDERING ---------------- */
    function renderList(data) {
        const list = document.getElementById('uni-list-container');
        list.innerHTML = '';
        markers.clearLayers();
        document.getElementById('count-display').textContent = `${data.length} найдено`;

        // Golden Marker Icon
        const icon = L.divIcon({ 
            className: 'golden-marker-icon', 
            html: '<i class="fas fa-map-marker-alt"></i>',
            iconSize: [30, 30], iconAnchor: [15, 30]
        });

        data.forEach(u => {
            // Map Marker
            if (u.lat && u.lon) {
                const m = L.marker([u.lat, u.lon], { icon: icon }).addTo(markers);
                m.on('click', () => openModal(u));
            }

            // List Item
            const el = document.createElement('div');
            el.className = 'uni-item';
            el.innerHTML = `
                <div style="flex:1; padding-right:10px;">
                    <span class="uni-name">${u.name}</span>
                    <div class="uni-meta"><i class="fas fa-map-pin" style="font-size:0.7rem;"></i> ${u.city}, ${u.country}</div>
                </div>
                <div style="text-align:right;">
                    <span class="rank-badge">${u.students} мест</span>
                </div>
            `;
            el.onclick = () => {
                if(map) map.flyTo([u.lat, u.lon], 6, {duration: 1.2});
                // Smooth scroll to map on mobile
                document.getElementById('map-section').scrollIntoView({ behavior: 'smooth' });
                // Open modal after delay
                setTimeout(() => openModal(u), 1200);
            };
            list.appendChild(el);
        });
        
        // Fit Map Bounds
        if(data.length > 0 && map) {
            const bounds = data.map(u => [u.lat, u.lon]);
            map.fitBounds(bounds, { padding: [50, 50], maxZoom: 5 });
        }
    }

    function filterList(region, btn) {
        if (btn) {
            document.querySelectorAll('.filter-tag').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }
        
        const term = document.getElementById('searchBox').value.toLowerCase();
        const filtered = universities.filter(u => {
            const matchReg = region === 'all' || u.region === region;
            const matchSearch = u.name.toLowerCase().includes(term) || u.country.toLowerCase().includes(term);
            return matchReg && matchSearch;
        });
        renderList(filtered);
    }
    
    // Search input listener
    document.getElementById('searchBox').addEventListener('input', () => {
        const activeBtn = document.querySelector('.filter-tag.active');
        const reg = activeBtn ? (activeBtn.textContent === 'Все' ? 'all' : activeBtn.textContent) : 'all';
        filterList(reg, null); // Pass null as btn to keep current selection
    });

    function renderTimeline() {
        document.getElementById('timeline-container').innerHTML = steps.map(s => `
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="step-num">ЭТАП ${s.num}</div>
                <div style="font-weight:700; color:var(--primary); font-size:1.05rem;">${s.title}</div>
                <div style="font-size:0.9rem; color:#555; margin-top:5px;">${s.desc}</div>
            </div>
        `).join('');
    }

    function renderFAQ() {
        document.getElementById('faq-root').innerHTML = faqData.map(cat => `
            <h3 style="font-size:1rem; color:var(--secondary); margin:20px 0 10px 5px; border-bottom:1px solid #ddd; padding-bottom:5px;">${cat.category}</h3>
            ${cat.items.map(i => `
                <div class="faq-card">
                    <div class="faq-header" onclick="this.parentElement.classList.toggle('open')">
                        <span>${i.q}</span>
                        <i class="fas fa-chevron-down" style="color:#aaa;"></i>
                    </div>
                    <div class="faq-body"><p>${i.a}</p></div>
                </div>
            `).join('')}
        `).join('');
    }

    function renderDD() {
        const container = document.getElementById('double-degree-content');
        container.innerHTML = '';
        // Group by level
        ['Бакалавриат', 'Магистратура'].forEach(lvl => {
            const progs = doubleDegreePrograms.filter(p => p.level === lvl);
            if(progs.length > 0) {
                container.innerHTML += `<h3 style="color:var(--secondary); font-size:1.1rem; margin:25px 0 10px;">${lvl}</h3>`;
                progs.forEach(p => {
                    container.innerHTML += `
                        <div class="info-card">
                            <div class="dd-info-label">${p.country}</div>
                            <div class="dd-info-value">${p.partner}</div>
                            <ul class="program-list-styled-dd">
                                ${p.programs.map(pr => `<li>${pr}</li>`).join('')}
                            </ul>
                        </div>
                    `;
                });
            }
        });
    }

    /* ---------------- MODAL LOGIC ---------------- */
    function openModal(u) {
        document.getElementById('m-name').textContent = u.name;
        document.getElementById('m-city').textContent = u.city;
        document.getElementById('m-country').textContent = u.country;
        document.getElementById('m-students').textContent = u.students;

        const fillList = (id, arr) => {
            const el = document.getElementById(id);
            if (!arr || arr.length === 0) {
                el.innerHTML = '<li style="color:#888;">Программы уточняются в офисе.</li>';
            } else {
                el.innerHTML = arr.map(p => `<li>${p}</li>`).join('');
            }
        };
        fillList('list-bachelor', u.bachelorPrograms);
        fillList('list-master', u.masterPrograms);

        // Dynamic cost estimation based on country
        let cost = "€300-500";
        if(['Франция','Германия','Австрия','Бельгия','Финляндия'].includes(u.country)) cost = "€700-900";
        else if(['Испания','Италия','Португалия'].includes(u.country)) cost = "€500-700";
        else if(['США','Гонконг','Япония','Корея'].includes(u.country)) cost = "$800-1200";
        document.getElementById('cost-living').textContent = cost;

        document.getElementById('uniModal').classList.add('open');
        modalTab('tab-overview', document.querySelector('.m-tab')); // Reset to first tab
        
        if (tg) tg.BackButton.show();
    }

    function closeModal() {
        document.getElementById('uniModal').classList.remove('open');
        if (tg) tg.BackButton.hide();
    }

    function modalTab(id, btn) {
        document.querySelectorAll('.m-tab').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    /* ---------------- NAVIGATION ---------------- */
    function switchTab(id, btn) {
        document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');

        // Scroll nav item into view (User Experience)
        btn.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });

        if(id === 'map-section' && map) {
            setTimeout(() => map.invalidateSize(), 200);
        }
    }

    // Start App
    init();

    </script>
</body>
</html>
