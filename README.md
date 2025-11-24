<html lang="ru">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
    <title>AlmaU Global Mobility Portal | Official Resource</title>
    
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@300;700&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">

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
            --radius: 6px; /* Строгие углы */
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
            overscroll-behavior: none; /* Prevent pull-to-refresh in webviews */
            touch-action: manipulation; /* Improve touch responsiveness */
        }

        h1, h2, h3, h4 { font-family: 'Merriweather', serif; color: var(--primary); margin-top: 0; }
        
        /* HEADER */
        .top-bar {
            background-color: var(--primary);
            color: var(--white);
            padding: 8px 0;
            font-size: 0.8rem;
            position: sticky;
            top: 0;
            z-index: 1001;
        }
        .container { max-width: 1400px; margin: 0 auto; padding: 0 15px; }
        .top-flex { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 5px; }
        
        .main-header {
            background: var(--white);
            border-bottom: 1px solid var(--border);
            padding: 15px 0;
            box-shadow: var(--shadow);
            position: sticky;
            top: calc(0px + env(safe-area-inset-top)); /* Adjust for notches */
            z-index: 1000;
        }
        .header-flex { display: flex; align-items: center; justify-content: space-between; }
        .logo-area { display: flex; align-items: center; gap: 10px; }
        .logo-box {
            width: 40px; height: 40px; background: var(--primary); color: var(--white);
            font-family: 'Merriweather'; font-weight: bold; font-size: 20px;
            display: flex; align-items: center; justify-content: center;
            border-radius: 4px; border: 2px solid var(--accent);
        }
        .brand-text h1 { font-size: 1.2rem; margin: 0; color: var(--primary); }
        .brand-text p { margin: 0; font-size: 0.75rem; color: var(--text-light); text-transform: uppercase; letter-spacing: 1px; }

        /* NAV */
        .nav-menu { display: none; flex-direction: column; gap: 15px; width: 100%; background: var(--white); padding: 10px; box-shadow: var(--shadow); position: absolute; top: 100%; left: 0; z-index: 999; }
        .nav-menu.open { display: flex; }
        .nav-item {
            text-decoration: none; color: var(--text-dark); font-weight: 500; font-size: 1rem;
            padding: 8px 0; transition: color 0.3s; cursor: pointer;
        }
        .nav-item:hover, .nav-item.active { color: var(--secondary); }
        .burger-menu { display: block; cursor: pointer; font-size: 1.5rem; color: var(--primary); }

        /* DASHBOARD LAYOUT */
        .dashboard-grid {
            display: flex; flex-direction: column; gap: 15px;
            margin-top: 15px; min-height: calc(100vh - 150px);
        }

        /* SIDEBAR (FILTERS & LIST) */
        .sidebar-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            display: flex; flex-direction: column; overflow: hidden; box-shadow: var(--shadow);
        }
        .panel-header {
            padding: 15px; background: var(--primary); color: var(--white);
        }
        .panel-header h3 { margin: 0; font-size: 1rem; font-weight: 400; color: var(--accent); }
        .stats-row { display: flex; gap: 10px; margin-top: 5px; font-size: 0.8rem; opacity: 0.9; }

        .search-box { padding: 10px; border-bottom: 1px solid var(--border); background: #f9f9f9; }
        .input-group { position: relative; }
        .input-group i { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: #999; }
        .form-control {
            width: 100%; padding: 10px 10px 10px 35px; border: 1px solid #ccc; border-radius: 4px;
            font-size: 0.9rem; transition: border 0.3s;
        }
        .form-control:focus { border-color: var(--secondary); outline: none; }

        .filter-tags { padding: 10px; display: flex; overflow-x: auto; gap: 8px; border-bottom: 1px solid var(--border); white-space: nowrap; }
        .filter-tag {
            font-size: 0.8rem; padding: 4px 10px; background: #eef2f6; border-radius: 20px;
            cursor: pointer; border: 1px solid transparent; transition: all 0.2s;
        }
        .filter-tag:hover { background: #dfe6ed; }
        .filter-tag.active { background: var(--primary); color: var(--white); border-color: var(--primary); }

        .university-list { overflow-y: auto; flex: 1; -webkit-overflow-scrolling: touch; max-height: 40vh; }
        .uni-item {
            padding: 12px; border-bottom: 1px solid var(--border); cursor: pointer; transition: background 0.2s;
            border-left: 4px solid transparent;
        }
        .uni-item:hover { background: #f0f7ff; }
        .uni-item.active { background: #e6f0fa; border-left-color: var(--accent); }
        .uni-name { font-weight: 700; color: var(--primary); display: block; margin-bottom: 4px; font-size: 1rem; }
        .uni-meta { font-size: 0.8rem; color: var(--text-light); display: flex; justify-content: space-between; }
        .rank-badge { background: var(--accent); color: var(--white); padding: 2px 6px; border-radius: 3px; font-size: 0.75rem; font-weight: bold; }

        /* MAP AREA */
        .map-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            overflow: hidden; box-shadow: var(--shadow); position: relative; height: 50vh; min-height: 300px;
        }
        #map { width: 100%; height: 100%; z-index: 1; }

        /* CUSTOM GOLDEN MARKER STYLE */
        .golden-marker-icon {
            background-color: transparent !important;
            border: none !important;
            color: var(--accent); /* University Gold */
            font-size: 24px; 
            text-shadow: 0 0 3px rgba(0,0,0,0.5); 
        }

        /* SECTIONS */
        .content-section { display: none; padding-bottom: 50px; animation: fadeIn 0.4s; }
        .content-section.active { display: block; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* DOUBLE DEGREE STYLES */
        .double-degree-grid { 
            display: grid; 
            grid-template-columns: 1fr; 
            gap: 15px;
            margin-top: 15px;
        }
        .info-card { 
            border: 1px solid var(--border); 
            background: #fcfcfc; 
            padding: 15px; 
            border-radius: var(--radius);
            transition: box-shadow 0.3s;
        }
        .info-card:hover { box-shadow: var(--shadow); }
        .dd-info-label { font-size: 0.75rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .dd-info-value { font-size: 1rem; font-weight: 600; color: var(--primary); }
        .program-list-styled-dd { list-style: none; padding: 0; }
        .program-list-styled-dd li {
            padding: 5px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start;
            font-size: 0.9rem;
        }
        .program-list-styled-dd li:last-child { border-bottom: none; }
        .program-list-styled-dd li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 10px; color: var(--accent); font-size: 0.85rem; margin-top: 3px;
        }

        /* FAQ STYLES */
        .faq-container { max-width: 100%; margin: 20px auto; padding: 0 15px; }
        .faq-category { margin-bottom: 20px; }
        .cat-title { border-bottom: 2px solid var(--accent); padding-bottom: 8px; margin-bottom: 15px; display: inline-block; font-size: 1.2rem; }
        
        .faq-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            margin-bottom: 10px; overflow: hidden;
            transition: box-shadow 0.3s;
        }
        .faq-card:hover { box-shadow: var(--shadow); }
        .faq-header {
            padding: 15px; cursor: pointer; display: flex; justify-content: space-between; align-items: center;
            font-weight: 600; background: #fff; transition: background 0.2s; font-size: 0.95rem;
        }
        .faq-header:hover { background: #f9f9f9; }
        .faq-icon { color: var(--secondary); transition: transform 0.3s; }
        .faq-card.open .faq-icon { transform: rotate(180deg); }
        .faq-card.open .faq-header { color: var(--secondary); border-bottom: 1px solid #f0f0f0; }
        .faq-body {
            max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out;
            padding: 0 15px; font-size: 0.9rem; color: #444; line-height: 1.6;
        }
        .faq-card.open .faq-body { padding: 15px; max-height: 500px; } 

        /* PLAN STYLES */
        .timeline { position: relative; max-width: 100%; margin: 30px auto; padding: 15px 0; }
        .timeline::before {
            content: ''; position: absolute; top: 0; bottom: 0; left: 20px; width: 2px; background: var(--border);
        }
        .timeline-item { margin-bottom: 40px; position: relative; width: 100%; padding: 0 15px 0 40px; text-align: left; }
        .timeline-dot {
            position: absolute; top: 0; left: 10px; width: 20px; height: 20px; background: var(--white);
            border: 4px solid var(--secondary); border-radius: 50%; z-index: 2;
        }
        .timeline-content {
            background: var(--white); padding: 20px; border-radius: var(--radius); border: 1px solid var(--border);
            box-shadow: var(--shadow); position: relative;
        }
        .step-num {
            position: absolute; top: -12px; left: 40px; background: var(--accent); color: var(--white);
            padding: 4px 12px; border-radius: 20px; font-weight: bold; font-size: 0.75rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        /* MODAL */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100vh;
            background: rgba(0, 40, 85, 0.7); z-index: 2000;
            display: none; justify-content: center; align-items: flex-start; opacity: 0; transition: opacity 0.3s;
            overflow: hidden;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        
        .modal-window {
            background: var(--white); width: 100%; max-width: 100%; height: 100vh;
            border-radius: 0; display: flex; flex-direction: column; overflow: hidden;
            box-shadow: none;
        }
        
        .modal-header {
            background: var(--primary); color: var(--white); padding: 15px 20px;
            display: flex; justify-content: space-between; align-items: flex-start;
        }
        .m-title h2 { color: var(--white); margin: 0; font-size: 1.4rem; }
        .m-subtitle { color: var(--accent); margin-top: 5px; font-size: 0.9rem; display: flex; gap: 10px; flex-wrap: wrap; }
        .close-btn { background: none; border: none; color: rgba(255,255,255,0.7); font-size: 1.2rem; cursor: pointer; transition: color 0.2s; }
        .close-btn:hover { color: var(--white); }

        .modal-body { display: flex; flex-direction: column; flex: 1; overflow: hidden; }
        
        .modal-nav {
            background: #f4f6f9; border-bottom: 1px solid var(--border);
            display: flex; flex-direction: row; overflow-x: auto; white-space: nowrap; padding: 5px 0;
        }
        .m-tab {
            padding: 8px 12px; cursor: pointer; font-weight: 500; color: var(--text-light);
            border-bottom: 4px solid transparent; transition: all 0.2s; font-size: 0.85rem;
        }
        .m-tab:hover { color: var(--primary); }
        .m-tab.active { color: var(--primary); border-bottom-color: var(--accent); font-weight: 700; }

        .modal-content { flex: 1; padding: 15px; overflow-y: auto; background: var(--white); -webkit-overflow-scrolling: touch; }
        
        .tab-panel { display: none; animation: fadeIn 0.3s; }
        .tab-panel.active { display: block; }
        
        .info-grid { display: grid; grid-template-columns: 1fr; gap: 15px; margin-bottom: 20px; }
        .info-card { background: #f9f9f9; padding: 15px; border-radius: var(--radius); border: 1px solid #eee; transition: box-shadow 0.3s; }
        .info-card:hover { box-shadow: var(--shadow); }
        .info-label { font-size: 0.75rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .info-value { font-size: 1rem; font-weight: 600; color: var(--primary); }

        .program-list-styled { list-style: none; padding: 0; }
        .program-list-styled li {
            padding: 8px 0; border-bottom: 1px solid #eee; display: flex; align-items: center; font-size: 0.85rem;
        }
        .program-list-styled li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 10px; color: var(--accent);
        }

        /* RESPONSIVE ADJUSTMENTS FOR DESKTOP */
        @media (min-width: 901px) {
            .burger-menu { display: none; }
            .nav-menu { display: flex; flex-direction: row; gap: 30px; position: static; padding: 0; box-shadow: none; width: auto; }
            .dashboard-grid { flex-direction: row; height: calc(100vh - 140px); min-height: 700px; }
            .map-panel { height: 100%; flex: 1; }
            .sidebar-panel { width: 350px; flex-shrink: 0; }
            .university-list { max-height: none; }
            .modal-window { width: 900px; max-width: 95%; height: 85vh; border-radius: 8px; }
            .modal-body { flex-direction: row; }
            .modal-nav { width: 200px; flex-direction: column; border-bottom: none; border-right: 1px solid var(--border); overflow-x: hidden; }
            .m-tab { border-bottom: none; border-left: 4px solid transparent; padding: 15px 20px; font-size: 1rem; }
            .m-tab.active { border-left-color: var(--accent); border-bottom: none; }
            .info-grid { grid-template-columns: 1fr 1fr; }
            .double-degree-grid { grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); }
            .timeline::before { left: 50%; }
            .timeline-item { width: 50%; padding: 0 40px; }
            .timeline-item:nth-child(odd) { left: 0; text-align: right; }
            .timeline-item:nth-child(even) { left: 50%; text-align: left; }
            .timeline-item:nth-child(odd) .timeline-dot { right: -10px; left: auto; }
            .timeline-item:nth-child(even) .timeline-dot { left: -10px; }
            .timeline-item:nth-child(odd) .step-num { right: 20px; left: auto; }
            .timeline-item:nth-child(even) .step-num { left: 20px; }
        }

        /* Additional fixes */
        html, body { height: 100%; overflow-x: hidden; -webkit-text-size-adjust: 100%; }
        .container { overflow-y: auto; -webkit-overflow-scrolling: touch; padding-bottom: env(safe-area-inset-bottom); }
    </style>
</head>
<body>

    <div class="top-bar">
        <div class="container top-flex">
            <span><i class="fas fa-university"></i> Официальный ресурс Академической Мобильности AlmaU</span>
            <span><i class="fas fa-envelope"></i> mobility@almau.edu.kz | <i class="fas fa-map-marker-alt"></i> Офис 107</span>
        </div>
    </div>

    <header class="main-header">
        <div class="container header-flex">
            <div class="logo-area">
                <div class="logo-box">A</div>
                <div class="brand-text">
                    <h1>AlmaU Global</h1>
                    <p>International Cooperation Department</p>
                </div>
            </div>
            <span class="burger-menu" onclick="toggleNav()"><i class="fas fa-bars"></i></span>
            <nav class="nav-menu" id="navMenu">
                <a class="nav-item active" onclick="switchTab('map-section', this)">Карта Партнеров</a>
                <a class="nav-item" onclick="switchTab('double-degree-section', this)">Программа Двойного Диплома</a>
                <a class="nav-item" onclick="switchTab('plan-section', this)">Дорожная Карта</a>
                <a class="nav-item" onclick="switchTab('faq-section', this)">База Знаний (FAQ)</a>
            </nav>
        </div>
    </header>

    <div class="container">
        
        <section id="map-section" class="content-section active">
            <div class="dashboard-grid">
                <main class="map-panel">
                    <div id="map"></div>
                </main>
                <aside class="sidebar-panel">
                    <div class="panel-header">
                        <h3><i class="fas fa-globe-europe"></i> Навигатор ВУЗов</h3>
                        <div class="stats-row">
                            <span id="count-display">0 ВУЗов</span> • <span>30+ Стран</span>
                        </div>
                    </div>
                    
                    <div class="search-box">
                        <div class="input-group">
                            <i class="fas fa-search"></i>
                            <input type="text" id="searchBox" class="form-control" placeholder="Поиск по стране, ВУЗу или программе...">
                        </div>
                    </div>

                    <div class="filter-tags">
                        <span class="filter-tag active" onclick="filterList('all', this)">Все</span>
                        <span class="filter-tag" onclick="filterList('Europe', this)">Европа</span>
                        <span class="filter-tag" onclick="filterList('Asia', this)">Азия</span>
                        <span class="filter-tag" onclick="filterList('Americas', this)">Америка</span>
                        <span class="filter-tag" onclick="filterList('CIS', this)">СНГ</span>
                        <span class="filter-tag" onclick="filterList('Africa', this)">Африка</span>
                    </div>

                    <div class="university-list" id="uni-list-container">
                        </div>
                </aside>
            </div>
        </section>
        
        <section id="double-degree-section" class="content-section">
            <div style="text-align: center; margin: 30px 0;">
                <h2 style="color: var(--secondary); font-size: 1.4rem;">Программа Двойного Диплома AlmaU</h2>
                <p style="color:var(--text-light); font-size: 0.9rem;">Получите два диплома, обучаясь в AlmaU и ведущих университетах мира.</p>
            </div>
            
            <div id="double-degree-content">
                </div>
            
            <div style="margin-top: 30px; padding: 15px; background: #e6f0fa; border-radius: var(--radius); border-left: 5px solid var(--accent);">
                <h4 style="margin-top: 0; color: var(--primary); font-size: 1.1rem;"><i class="fas fa-info-circle"></i> Особенности Программы Двойного Диплома:</h4>
                <ul style="list-style-type: disc; padding-left: 20px; font-size: 0.9rem; color: #444;">
                    <li>Программа обычно предполагает обучение **1-2 года** в вузе-партнере.</li>
                    <li>По окончании обучения студент получает **два полноценных диплома** (AlmaU и зарубежного вуза).</li>
                    <li>Обучение в вузе-партнере по программе Двойного Диплома является **платным**, согласно тарифам вуза-партнера.</li>
                    <li>Для участия необходимо соответствовать высоким требованиям обоих вузов по успеваемости и знанию языка, часто предусмотрен конкурсный отбор.</li>
                </ul>
            </div>
        </section>

        <section id="plan-section" class="content-section">
            <div style="text-align: center; margin: 30px 0;">
                <h2 style="font-size: 1.4rem;">Процесс подачи заявки</h2>
                <p style="color:var(--text-light); font-size: 0.9rem;">Официальный регламент прохождения конкурса на академическую мобильность</p>
            </div>
            
            <div class="timeline">
                <div id="timeline-container">
                    </div>
            </div>
        </section>

        <section id="faq-section" class="content-section">
            <div class="faq-container" id="faq-root">
                </div>
        </section>

    </div>

    <div class="modal-overlay" id="uniModal">
        <div class="modal-window">
            <div class="modal-header">
                <div class="m-title">
                    <h2 id="m-name">University Name</h2>
                    <div class="m-subtitle">
                        <span><i class="fas fa-map-marker-alt"></i> <span id="m-city">City</span>, <span id="m-country">Country</span></span>
                        <span><i class="fas fa-star"></i> <span id="m-rank">Top Partner</span></span>
                    </div>
                </div>
                <button class="close-btn" onclick="closeModal()"><i class="fas fa-times"></i></button>
            </div>
            <div class="modal-body">
                <div class="modal-nav">
                    <button class="m-tab active" onclick="modalTab('tab-overview', this)">Обзор</button>
                    <button class="m-tab" onclick="modalTab('tab-bachelor', this)">Бакалавриат</button>
                    <button class="m-tab" onclick="modalTab('tab-master', this)">Магистратура</button>
                    <button class="m-tab" onclick="modalTab('tab-finance', this)">Бюджет и Виза</button>
                </div>
                
                <div class="modal-content">
                    
                    <div id="tab-overview" class="tab-panel active">
                        <h3 style="color: var(--secondary); font-size: 1.2rem;">Общая информация</h3>
                        <div class="info-grid">
                            <div class="info-card">
                                <div class="info-label">Доступные места (Квота)</div>
                                <div class="info-value"><span id="m-students">2</span> студентов</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Язык обучения</div>
                                <div class="info-value">Английский (B2)</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Тип программы</div>
                                <div class="info-value">Семестровый обмен</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Статус партнера</div>
                                <div class="info-value" style="color:var(--success)">Активный</div>
                            </div>
                        </div>
                        <div class="info-card" style="background:#fff; border-color:var(--accent); margin-top:15px;">
                            <h4 style="font-size: 1.1rem;"><i class="fas fa-info-circle"></i> Описание</h4>
                            <p style="color:var(--text-light); font-size:0.9rem;">
                                Вуз является стратегическим партнером AlmaU. Студентам предоставляется возможность бесплатного обучения (**Tuition Waiver**). Обязательно проверьте соответствие курсов (Learning Agreement) перед подачей.
                            </p>
                        </div>
                    </div>

                    <div id="tab-bachelor" class="tab-panel">
                        <h4 style="font-size: 1.1rem;">Доступные дисциплины (Бакалавриат)</h4>
                        <ul class="program-list-styled" id="list-bachelor"></ul>
                    </div>

                    <div id="tab-master" class="tab-panel">
                        <h4 style="font-size: 1.1rem;">Доступные дисциплины (Магистратура)</h4>
                        <ul class="program-list-styled" id="list-master"></ul>
                    </div>

                    <div id="tab-finance" class="tab-panel">
                        <div class="info-card" style="margin-bottom:15px;">
                            <h4 style="font-size: 1.1rem;"><i class="fas fa-wallet"></i> Примерные расходы (в месяц)</h4>
                            <p style="font-size:0.85rem; color:#666;">*Данные являются приблизительными и зависят от образа жизни.</p>
                            <table style="width:100%; border-collapse:collapse; margin-top:10px; font-size: 0.9rem;">
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Жилье</td><td style="text-align:right; font-weight:bold;" id="cost-living">€300-500</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Питание</td><td style="text-align:right; font-weight:bold;" id="cost-food">€200-300</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Транспорт</td><td style="text-align:right; font-weight:bold;" id="cost-transport">€30-50</td></tr>
                            </table>
                        </div>
                        <div class="info-card">
                            <h4 style="font-size: 1.1rem;"><i class="fas fa-passport"></i> Визовые требования</h4>
                            <p style="font-size: 0.9rem;">Для обучения требуется студенческая виза типа D. Подтверждение финансовой состоятельности обязательно. Страховой полис должен покрывать весь период пребывания.</p>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
    /* ---------------- DATA ---------------- */
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
        
        { level: "Магистратура", partner: "IE University Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "EU Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "EDEM Business School", country: "Испания", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "Hof University", country: "Германия", programs: ["Уточняйте программы в Международном Офисе AlmaU"] },
        { level: "Магистратура", partner: "Zhejiang University", country: "Китай", programs: ["Уточняйте программы в Международном Офисе AlmaU"] }
    ];

    /* ---------------- INITIALIZATION & MAP ---------------- */
    let map;
    let markers = L.layerGroup();
    let currentFilter = 'all';

    const GoldenIcon = L.DivIcon.extend({
        options: {
            iconSize: [24, 24],
            iconAnchor: [12, 24], 
            popupAnchor: [0, -20], 
            className: 'golden-marker-icon',
            html: '<i class="fa fa-map-marker-alt"></i>'
        }
    });
    const goldenIcon = new GoldenIcon();

    function initMap() {
        if (document.getElementById('map')) {
            map = L.map('map', { zoomControl: true, attributionControl: false }).setView([45, 20], 3); 
            
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
                attribution: '© AlmaU Mobility',
                subdomains: 'abcd',
                maxZoom: 19
            }).addTo(map);

            map.addLayer(markers);
            map.attributionControl.setPrefix('');

            // Invalidate on load, resize, orientation
            setTimeout(() => map.invalidateSize(), 100);
            window.addEventListener('resize', () => setTimeout(() => map.invalidateSize(), 100));
            window.addEventListener('orientationchange', () => setTimeout(() => map.invalidateSize(), 200));
        }
    }

    function init() {
        initMap();
        renderList(universities);
        renderTimeline();
        renderFAQ();
        renderDoubleDegreePrograms();
        filterList('all', document.querySelector('.filter-tags .filter-tag')); 
    }

    function renderList(data) {
        const uniListContainer = document.getElementById('uni-list-container');
        uniListContainer.innerHTML = '';
        markers.clearLayers();

        const countDisplay = document.getElementById('count-display');
        countDisplay.textContent = `${data.length} ВУЗов`;

        data.forEach(u => {
            if (u.lat && u.lon) {
                const marker = L.marker([u.lat, u.lon], { icon: goldenIcon })
                    .bindTooltip(`<b>${u.name}</b>`, { direction: 'top', offset: [0, -20] })
                    .on('click', () => openModal(u));
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
                if (map) map.flyTo([u.lat, u.lon], 6, { duration: 1 });
                openModal(u);
            };
            uniListContainer.appendChild(div);
        });

        if (map && data.length > 0) {
            const bounds = data.map(uni => [uni.lat, uni.lon]);
            if (bounds.length > 0) map.fitBounds(bounds, { padding: [20, 20], maxZoom: 4 });
        } else if (map) {
            map.setView([45, 20], 3);
        }
        if (map) map.invalidateSize();
    }

    function filterList(region = currentFilter, btn = null) {
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
                                (u.bachelorPrograms && u.bachelorPrograms.join(' ').toLowerCase().includes(term)) ||
                                (u.masterPrograms && u.masterPrograms.join(' ').toLowerCase().includes(term));
            return matchRegion && matchSearch;
        });

        renderList(filtered);
    }

    document.getElementById('searchBox').addEventListener('input', () => {
        const activeBtn = document.querySelector('.filter-tag.active');
        if (activeBtn) {
            filterList(activeBtn.textContent.trim() === 'Все' ? 'all' : activeBtn.textContent.trim(), activeBtn);
        } else {
            filterList('all', document.querySelector('.filter-tags .filter-tag'));
        }
    });

    function renderTimeline() {
        const container = document.getElementById('timeline-container');
        container.innerHTML = steps.map(step => `
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="timeline-content">
                    <span class="step-num">Этап ${step.num}</span>
                    <h3 style="margin-top:10px; margin-bottom:5px; font-size:1.1rem;">${step.title}</h3>
                    <p style="margin:0; font-size:0.9rem; color:#666;">${step.desc}</p>
                </div>
            </div>
        `).join('');
    }

    function renderFAQ() {
        const root = document.getElementById('faq-root');
        root.innerHTML = faqData.map(cat => `
            <div class="faq-category">
                <h3 class="cat-title">${cat.category}</h3>
                ${cat.items.map(item => `
                    <div class="faq-card">
                        <div class="faq-header" onclick="toggleFaq(this)">
                            <span>${item.q}</span>
                            <i class="fas fa-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-body"><p>${item.a}</p></div>
                    </div>
                `).join('')}
            </div>
        `).join('');
    }

    function toggleFaq(header) {
        const card = header.parentElement;
        const body = card.querySelector('.faq-body');
        const isOpen = card.classList.contains('open');

        card.closest('.faq-category').querySelectorAll('.faq-card.open').forEach(openCard => {
            if (openCard !== card) {
                openCard.classList.remove('open');
                openCard.querySelector('.faq-body').style.maxHeight = null;
            }
        });

        if (!isOpen) {
            card.classList.add('open');
            body.style.maxHeight = body.scrollHeight + "px";
        } else {
            card.classList.remove('open');
            body.style.maxHeight = null;
        }
    }

    function renderDoubleDegreePrograms() {
        const container = document.getElementById('double-degree-content');
        container.innerHTML = ''; 
        
        const levels = [...new Set(doubleDegreePrograms.map(p => p.level))];
        
        levels.forEach(level => {
            const levelPrograms = doubleDegreePrograms.filter(p => p.level === level);
            
            let html = `
                <h3 style="border-bottom: 2px solid var(--accent); padding-bottom: 8px; margin-top: 30px; margin-bottom: 15px; display: inline-block; color: var(--secondary); font-size: 1.2rem;">${level}</h3>
                <div class="double-degree-grid">
            `;
            
            levelPrograms.forEach(p => {
                html += `
                    <div class="info-card">
                        <div class="dd-info-label">${p.country}</div>
                        <div class="dd-info-value" style="font-size: 1.1rem;">${p.partner}</div>
                        <ul class="program-list-styled-dd" style="margin-top: 10px;">
                            ${p.programs.map(prog => `<li>${prog}</li>`).join('')}
                        </ul>
                    </div>
                `;
            });
            
            html += `</div>`;
            container.innerHTML += html;
        });
    }

    function openModal(u) {
        document.getElementById('m-name').textContent = u.name;
        document.getElementById('m-city').textContent = u.city;
        document.getElementById('m-country').textContent = u.country;
        document.getElementById('m-students').textContent = u.students;
        document.getElementById('m-rank').textContent = "Top Partner";

        const fillList = (id, arr) => {
            const el = document.getElementById(id);
            el.innerHTML = '';
            if(!arr || arr.length === 0) {
                el.innerHTML = '<li style="color:#999; padding-left:0;"><i class="fas fa-exclamation-circle" style="color:#e94e4e; margin-right: 12px;"></i>Информация уточняется в офисе координатора.</li>';
            } else {
                arr.forEach(txt => {
                    const li = document.createElement('li');
                    li.textContent = txt;
                    el.appendChild(li);
                });
            }
        };

        fillList('list-bachelor', u.bachelorPrograms);
        fillList('list-master', u.masterPrograms);

        let rent = "€300-500";
        if(u.country === "Франция" || u.country === "Германия") rent = "€600-900";
        else if(u.country === "Польша" || u.country === "Литва") rent = "€250-400";
        document.getElementById('cost-living').textContent = rent;

        document.getElementById('uniModal').classList.add('open');
        modalTab('tab-overview', document.querySelector('.modal-nav .m-tab:first-child'));
    }

    function closeModal() {
        document.getElementById('uniModal').classList.remove('open');
    }

    function modalTab(targetId, btn) {
        document.querySelectorAll('.modal-nav .m-tab').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(targetId).classList.add('active');
    }

    function switchTab(sectionId, btn) {
        document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(sectionId).classList.add('active');
        
        if(sectionId === 'map-section') {
            setTimeout(() => {
                if(map) map.invalidateSize();
            }, 200);
        }
        toggleNav(false);
    }

    function toggleNav(forceClose = null) {
        const nav = document.getElementById('navMenu');
        if (forceClose === false) {
            nav.classList.remove('open');
        } else {
            nav.classList.toggle('open');
        }
    }

    init();

    document.getElementById('uniModal').addEventListener('click', function(event) {
        if (event.target === this) {
            closeModal();
        }
    });

    window.addEventListener('orientationchange', () => {
        setTimeout(() => {
            if (map) map.invalidateSize();
        }, 200);
    });
    </script>
</body>
</html>
