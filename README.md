<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
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

        * { box-sizing: border-box; }
        
        body {
            margin: 0;
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            line-height: 1.6;
        }

        h1, h2, h3, h4 { font-family: 'Merriweather', serif; color: var(--primary); margin-top: 0; }
        
        /* HEADER */
        .top-bar {
            background-color: var(--primary);
            color: var(--white);
            padding: 10px 0;
            font-size: 0.9rem;
        }
        .container { max-width: 1400px; margin: 0 auto; padding: 0 20px; }
        .top-flex { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        
        .main-header {
            background: var(--white);
            border-bottom: 1px solid var(--border);
            padding: 20px 0;
            box-shadow: var(--shadow);
            position: sticky; top: 0; z-index: 1000;
        }
        .header-flex { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; }
        .logo-area { display: flex; align-items: center; gap: 15px; }
        .logo-box {
            width: 50px; height: 50px; background: var(--primary); color: var(--white);
            font-family: 'Merriweather'; font-weight: bold; font-size: 24px;
            display: flex; align-items: center; justify-content: center;
            border-radius: 4px; border: 2px solid var(--accent);
        }
        .brand-text h1 { font-size: 1.5rem; margin: 0; color: var(--primary); }
        .brand-text p { margin: 0; font-size: 0.85rem; color: var(--text-light); text-transform: uppercase; letter-spacing: 1px; }

        /* NAV */
        .nav-menu { display: flex; gap: 30px; flex-wrap: wrap; }
        .nav-item {
            text-decoration: none; color: var(--text-dark); font-weight: 500; font-size: 1rem;
            padding: 8px 0; position: relative; transition: color 0.3s; cursor: pointer;
            white-space: nowrap;
        }
        .nav-item:hover, .nav-item.active { color: var(--secondary); }
        .nav-item.active::after {
            content: ''; position: absolute; bottom: 0; left: 0; width: 100%; height: 3px; background: var(--accent);
        }

        /* DASHBOARD LAYOUT */
        .dashboard-grid {
            display: grid; grid-template-columns: 350px 1fr; gap: 20px;
            margin-top: 30px; height: calc(100vh - 140px); min-height: 700px;
        }

        /* SIDEBAR (FILTERS & LIST) */
        .sidebar-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            display: flex; flex-direction: column; overflow: hidden; box-shadow: var(--shadow);
        }
        .panel-header {
            padding: 20px; background: var(--primary); color: var(--white);
        }
        .panel-header h3 { margin: 0; font-size: 1.1rem; font-weight: 400; color: var(--accent); }
        .stats-row { display: flex; gap: 15px; margin-top: 10px; font-size: 0.9rem; opacity: 0.9; }

        .search-box { padding: 15px; border-bottom: 1px solid var(--border); background: #f9f9f9; }
        .input-group { position: relative; }
        .input-group i { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #999; }
        .form-control {
            width: 100%; padding: 12px 12px 12px 40px; border: 1px solid #ccc; border-radius: 4px;
            font-family: inherit; font-size: 0.95rem; transition: border 0.3s;
        }
        .form-control:focus { border-color: var(--secondary); outline: none; }

        .filter-tags { padding: 10px 15px; display: flex; flex-wrap: wrap; gap: 8px; border-bottom: 1px solid var(--border); }
        .filter-tag {
            font-size: 0.8rem; padding: 4px 10px; background: #eef2f6; border-radius: 20px;
            cursor: pointer; border: 1px solid transparent; transition: all 0.2s;
        }
        .filter-tag:hover { background: #dfe6ed; }
        .filter-tag.active { background: var(--primary); color: var(--white); border-color: var(--primary); }

        .university-list { overflow-y: auto; flex: 1; }
        .uni-item {
            padding: 15px; border-bottom: 1px solid var(--border); cursor: pointer; transition: background 0.2s;
            border-left: 4px solid transparent;
        }
        .uni-item:hover { background: #f0f7ff; }
        .uni-item.active { background: #e6f0fa; border-left-color: var(--accent); }
        .uni-name { font-weight: 700; color: var(--primary); display: block; margin-bottom: 4px; }
        .uni-meta { font-size: 0.85rem; color: var(--text-light); display: flex; justify-content: space-between; }
        .rank-badge { background: var(--accent); color: var(--white); padding: 2px 6px; border-radius: 3px; font-size: 0.75rem; font-weight: bold; }

        /* MAP AREA */
        .map-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            overflow: hidden; box-shadow: var(--shadow); position: relative;
        }
        #map { width: 100%; height: 100%; z-index: 1; }

        /* CUSTOM GOLDEN MARKER STYLE (RESTORED) */
        .golden-marker-icon {
            background-color: transparent !important;
            border: none !important;
            color: var(--accent); /* University Gold */
            font-size: 30px; 
            text-shadow: 0 0 3px rgba(0,0,0,0.5); 
        }

        /* SECTIONS (FAQ & PLAN & DD) - Initially Hidden */
        .content-section { display: none; padding-bottom: 50px; animation: fadeIn 0.4s; }
        .content-section.active { display: block; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* DOUBLE DEGREE STYLES (NEW) */
        .double-degree-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); 
            gap: 20px;
            margin-top: 20px;
        }
        .info-card { 
            border: 1px solid var(--border); 
            background: #fcfcfc; 
            padding: 20px; 
            border-radius: var(--radius);
        }
        .dd-info-label { font-size: 0.8rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .dd-info-value { font-size: 1.1rem; font-weight: 600; color: var(--primary); }
        .program-list-styled-dd { list-style: none; padding: 0; }
        .program-list-styled-dd li {
            padding: 5px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start;
            font-size: 0.95rem;
        }
        .program-list-styled-dd li:last-child { border-bottom: none; }
        .program-list-styled-dd li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 12px; color: var(--accent); font-size: 0.9rem; margin-top: 3px;
        }


        /* FAQ STYLES */
        .faq-container { max-width: 900px; margin: 40px auto; }
        .faq-category { margin-bottom: 30px; }
        .cat-title { border-bottom: 2px solid var(--accent); padding-bottom: 10px; margin-bottom: 20px; display: inline-block; }
        
        .faq-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            margin-bottom: 15px; overflow: hidden;
        }
        .faq-header {
            padding: 20px; cursor: pointer; display: flex; justify-content: space-between; align-items: center;
            font-weight: 600; background: #fff; transition: background 0.2s;
        }
        .faq-header:hover { background: #f9f9f9; }
        .faq-icon { color: var(--secondary); transition: transform 0.3s; }
        .faq-card.open .faq-icon { transform: rotate(180deg); }
        .faq-card.open .faq-header { color: var(--secondary); border-bottom: 1px solid #f0f0f0; }
        .faq-body {
            max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out;
            padding: 0 20px; font-size: 0.95rem; color: #444; line-height: 1.7;
        }
        /* Adjusted max-height for FAQ body to support content flow on opening */
        .faq-card.open .faq-body { padding: 20px; max-height: 1000px; } 

        /* PLAN STYLES */
        .timeline { position: relative; max-width: 800px; margin: 50px auto; padding: 20px 0; }
        .timeline::before {
            content: ''; position: absolute; top: 0; bottom: 0; left: 50%; width: 2px; background: var(--border); transform: translateX(-50%);
        }
        .timeline-item { margin-bottom: 50px; position: relative; width: 50%; padding: 0 40px; }
        .timeline-item:nth-child(odd) { left: 0; text-align: right; }
        .timeline-item:nth-child(even) { left: 50%; text-align: left; }
        
        .timeline-dot {
            position: absolute; top: 0; width: 20px; height: 20px; background: var(--white);
            border: 4px solid var(--secondary); border-radius: 50%; z-index: 2;
        }
        .timeline-item:nth-child(odd) .timeline-dot { right: -10px; }
        .timeline-item:nth-child(even) .timeline-dot { left: -10px; }
        
        .timeline-content {
            background: var(--white); padding: 25px; border-radius: var(--radius); border: 1px solid var(--border);
            box-shadow: var(--shadow); position: relative;
        }
        .step-num {
            position: absolute; top: -15px; background: var(--accent); color: var(--white);
            padding: 5px 15px; border-radius: 20px; font-weight: bold; font-size: 0.8rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        .timeline-item:nth-child(odd) .step-num { right: 20px; }
        .timeline-item:nth-child(even) .step-num { left: 20px; }

        /* MODAL - PASSPORT STYLE */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 40, 85, 0.7); z-index: 2000;
            display: none; justify-content: center; align-items: center; opacity: 0; transition: opacity 0.3s;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        
        .modal-window {
            background: var(--white); width: 900px; max-width: 95%; height: 85vh;
            border-radius: 8px; display: flex; flex-direction: column; overflow: hidden;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
        }
        
        .modal-header {
            background: var(--primary); color: var(--white); padding: 25px 30px;
            display: flex; justify-content: space-between; align-items: flex-start;
        }
        .m-title h2 { color: var(--white); margin: 0; font-size: 1.8rem; }
        .m-subtitle { color: var(--accent); margin-top: 5px; font-size: 1rem; display: flex; gap: 15px; flex-wrap: wrap; }
        .close-btn { background: none; border: none; color: rgba(255,255,255,0.7); font-size: 1.5rem; cursor: pointer; transition: color 0.2s; }
        .close-btn:hover { color: var(--white); }

        .modal-body { display: flex; flex: 1; overflow: hidden; }
        
        .modal-nav {
            width: 200px; background: #f4f6f9; border-right: 1px solid var(--border);
            display: flex; flex-direction: column; padding-top: 20px; flex-shrink: 0;
        }
        .m-tab {
            padding: 15px 20px; cursor: pointer; font-weight: 500; color: var(--text-light);
            border-left: 4px solid transparent; transition: all 0.2s; text-align: left; background: none; border-bottom: none; border-top: none; border-right: none; width: 100%; font-family: inherit;
        }
        .m-tab:hover { background: #e6eef5; color: var(--primary); }
        .m-tab.active { background: var(--white); color: var(--primary); border-left-color: var(--accent); font-weight: 700; box-shadow: -5px 0 10px rgba(0,0,0,0.05); }

        .modal-content { flex: 1; padding: 30px; overflow-y: auto; background: var(--white); }
        
        .tab-panel { display: none; animation: fadeIn 0.3s; }
        .tab-panel.active { display: block; }
        
        .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-bottom: 30px; }
        .info-card { background: #f9f9f9; padding: 20px; border-radius: var(--radius); border: 1px solid #eee; }
        .info-label { font-size: 0.8rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .info-value { font-size: 1.1rem; font-weight: 600; color: var(--primary); }

        .program-list-styled { list-style: none; padding: 0; }
        .program-list-styled li {
            padding: 12px 0; border-bottom: 1px solid #eee; display: flex; align-items: center;
        }
        .program-list-styled li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 12px; color: var(--accent);
        }

        /* RESPONSIVE */
        @media (max-width: 1200px) {
            .container { padding: 0 15px; }
            .dashboard-grid { 
                grid-template-columns: 300px 1fr; 
                height: calc(100vh - 120px); 
                min-height: 600px;
            }
        }
        
        /* УЛУЧШЕННЫЙ МОБИЛЬНЫЙ МЕДИА-ЗАПРОС */
        @media (max-width: 900px) {
            
            /* --- ГЛАВНАЯ СТРУКТУРА --- */
            .top-flex { 
                flex-direction: column; 
                align-items: flex-start; 
                gap: 5px; 
            }
            .top-bar {
                /* Уменьшаем размер текста для экономии места */
                font-size: 0.8rem;
                padding: 8px 0;
            }
            .header-flex { 
                flex-direction: column; 
                align-items: flex-start; 
                gap: 15px; 
            }
            .main-header {
                 padding: 15px 0;
            }
            /* Навигация (Меню) - Скролл для узких экранов */
            .nav-menu { 
                flex-wrap: nowrap; 
                margin-top: 10px; 
                gap: 15px;
                width: 100%;
                overflow-x: auto;
                padding-bottom: 5px; /* Для видимости тени скролла */
            }
            .nav-item { 
                padding: 5px 0; 
                font-size: 0.95rem;
                flex-shrink: 0; /* Чтобы элементы не сжимались */
            }
            .nav-item.active::after {
                height: 2px;
            }

            /* --- ДАШБОРД КАРТЫ --- */
            .dashboard-grid { 
                grid-template-columns: 1fr; 
                height: auto; 
                min-height: auto; 
                margin-top: 20px; 
                /* Переставляем карту вверх (для лучшего UX) */
                grid-template-areas: 
                    "map"
                    "sidebar";
            }
            .map-panel { 
                grid-area: map; 
                height: 350px; /* Немного ниже, чем 400px для экономии места */
                margin-bottom: 10px;
            }
            .sidebar-panel { 
                grid-area: sidebar; 
                height: auto; 
                max-height: 450px; /* Ограничиваем высоту, чтобы не занимать весь экран */
            }
            .university-list { 
                max-height: 200px; /* Уменьшаем максимальную высоту списка */
                min-height: 100px; /* Обеспечиваем минимальную видимость */
            } 
            .search-box {
                padding: 10px 15px;
            }
            .form-control {
                padding: 10px 10px 10px 40px; /* Немного меньше отступы */
            }
            .filter-tags {
                /* Обеспечиваем прокрутку, если тегов много */
                flex-wrap: nowrap;
                overflow-x: auto;
                padding: 10px 15px;
            }
            .filter-tag {
                flex-shrink: 0;
            }

            /* --- ДВОЙНОЙ ДИПЛОМ --- */
            .double-degree-grid {
                grid-template-columns: 1fr; /* Один столбец на мобильных */
            }
            
            /* --- ТАЙМЛАЙН --- */
            .timeline::before { left: 20px; }
            .timeline-item { width: 100%; padding: 0 20px 0 40px; margin-bottom: 30px; }
            /* Смещаем все элементы на левую сторону */
            .timeline-item:nth-child(odd), .timeline-item:nth-child(even) { 
                left: 0; 
                text-align: left !important; 
            }
            .timeline-item:nth-child(odd) .timeline-dot { left: 10px; right: auto; }
            .timeline-item:nth-child(even) .timeline-dot { left: 10px; }
            .timeline-item:nth-child(odd) .step-num { left: 40px; right: auto; }
            .timeline-item:nth-child(even) .step-num { left: 40px; }
            
            /* --- МОДАЛЬНОЕ ОКНО --- */
            .info-grid { grid-template-columns: 1fr; gap: 20px; }
            
            .modal-window { 
                height: 95vh; 
                max-height: 95vh; 
            }
            .modal-header {
                 padding: 20px;
            }
            .m-title h2 { 
                font-size: 1.4rem; 
            }
            .m-subtitle {
                font-size: 0.9rem;
            }
            .modal-body { 
                flex-direction: column; 
            }
            /* Навигация модального окна - горизонтальный скролл */
            .modal-nav { 
                width: 100%; 
                border-right: none; 
                border-bottom: 1px solid var(--border); 
                padding-top: 0; 
                flex-direction: row; 
                overflow-x: auto; 
                white-space: nowrap;
            }
            .m-tab { 
                border-left: none; 
                border-bottom: 4px solid transparent; 
                flex-shrink: 0; 
                padding: 12px 15px; /* Немного уменьшены отступы */
            }
            .m-tab.active { 
                border-left-color: transparent; 
                border-bottom-color: var(--accent); 
                box-shadow: none;
            }
            .modal-content { 
                padding: 20px; /* Уменьшены отступы */
            }
        }
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
            <nav class="nav-menu">
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

                <main class="map-panel">
                    <div id="map"></div>
                </main>
            </div>
        </section>
        
        <section id="double-degree-section" class="content-section">
            <div class="container" style="padding: 0;">
                <div style="text-align: center; margin: 40px 0;">
                    <h2 style="color: var(--secondary);">Программа Двойного Диплома AlmaU</h2>
                    <p style="color:var(--text-light)">Получите два диплома, обучаясь в AlmaU и ведущих университетах мира.</p>
                </div>
                
                <div id="double-degree-content">
                    </div>
                
                <div style="margin-top: 40px; padding: 20px; background: #e6f0fa; border-radius: var(--radius); border-left: 5px solid var(--accent);">
                    <h4 style="margin-top: 0; color: var(--primary);"><i class="fas fa-info-circle"></i> Особенности Программы Двойного Диплома:</h4>
                    <ul style="list-style-type: disc; padding-left: 20px; font-size: 0.95rem; color: #444;">
                        <li>Программа обычно предполагает обучение **1-2 года** в вузе-партнере.</li>
                        <li>По окончании обучения студент получает **два полноценных диплома** (AlmaU и зарубежного вуза).</li>
                        <li>Обучение в вузе-партнере по программе Двойного Диплома является **платным**, согласно тарифам вуза-партнера.</li>
                        <li>Для участия необходимо соответствовать высоким требованиям обоих вузов по успеваемости и знанию языка, часто предусмотрен конкурсный отбор.</li>
                    </ul>
                </div>
            </div>
        </section>

        <section id="plan-section" class="content-section">
            <div style="text-align: center; margin: 40px 0;">
                <h2>Процесс подачи заявки</h2>
                <p style="color:var(--text-light)">Официальный регламент прохождения конкурса на академическую мобильность</p>
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
                        <h3 style="color: var(--secondary);">Общая информация</h3>
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
                        <div class="info-card" style="background:#fff; border-color:var(--accent); margin-top:20px;">
                            <h4><i class="fas fa-info-circle"></i> Описание</h4>
                            <p style="color:var(--text-light); font-size:0.95rem;">
                                Вуз является стратегическим партнером AlmaU. Студентам предоставляется возможность бесплатного обучения (**Tuition Waiver**). Обязательно проверьте соответствие курсов (Learning Agreement) перед подачей.
                            </p>
                        </div>
                    </div>

                    <div id="tab-bachelor" class="tab-panel">
                        <h4>Доступные дисциплины (Бакалавриат)</h4>
                        <ul class="program-list-styled" id="list-bachelor"></ul>
                    </div>

                    <div id="tab-master" class="tab-panel">
                        <h4>Доступные дисциплины (Магистратура)</h4>
                        <ul class="program-list-styled" id="list-master"></ul>
                    </div>

                    <div id="tab-finance" class="tab-panel">
                        <div class="info-card" style="margin-bottom:20px;">
                            <h4><i class="fas fa-wallet"></i> Примерные расходы (в месяц)</h4>
                            <p style="font-size:0.85rem; color:#666;">*Данные являются приблизительными и зависят от образа жизни.</p>
                            <table style="width:100%; border-collapse:collapse; margin-top:10px;">
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Жилье</td><td style="text-align:right; font-weight:bold;" id="cost-living">€300-500</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Питание</td><td style="text-align:right; font-weight:bold;" id="cost-food">€200-300</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Транспорт</td><td style="text-align:right; font-weight:bold;" id="cost-transport">€30-50</td></tr>
                            </table>
                        </div>
                        <div class="info-card">
                            <h4><i class="fas fa-passport"></i> Визовые требования</h4>
                            <p>Для обучения требуется студенческая виза типа D. Подтверждение финансовой состоятельности обязательно. Страховой полис должен покрывать весь период пребывания.</p>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
    /* ---------------- DATA ---------------- */
    /* Полная база данных, без сокращений, плюс добавлен регион для фильтрации */
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
    
    // ДАННЫЕ ДЛЯ ПРОГРАММЫ ДВОЙНОГО ДИПЛОМА (ДОБАВЛЕНО)
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

    /* ---------------- INITIALIZATION & MAP ---------------- */
    let map;
    let markers = L.layerGroup();
    let currentFilter = 'all';

    // 1. Определение Золотого Маркера (Восстановлено)
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
        // Проверяем, что элемент карты существует
        if (document.getElementById('map')) {
            map = L.map('map').setView([45, 20], 3); 
            
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
                attribution: '© AlmaU Mobility',
                subdomains: 'abcd',
                maxZoom: 19
            }).addTo(map);

            map.addLayer(markers);
        }
    }

    function init() {
        initMap();
        renderList(universities);
        renderTimeline();
        renderFAQ();
        renderDoubleDegreePrograms(); // Запуск рендеринга DD
        // Принудительная активация начального фильтра
        filterList('all', document.querySelector('.filter-tags .filter-tag')); 
    }

    /* ---------------- RENDERING LOGIC ---------------- */
    function renderList(data) {
        // Clear list & map
        const uniListContainer = document.getElementById('uni-list-container');
        uniListContainer.innerHTML = '';
        markers.clearLayers();

        const countDisplay = document.getElementById('count-display');
        countDisplay.textContent = `${data.length} ВУЗов`;

        data.forEach(u => {
            // 1. Add Marker (используем золотой маркер)
            if (u.lat && u.lon) {
                const marker = L.marker([u.lat, u.lon], { icon: goldenIcon })
                    .bindTooltip(`<b>${u.name}</b>`, { direction: 'top', offset: [0, -25] })
                    .on('click', () => openModal(u));
                markers.addLayer(marker);
            }

            // 2. Add List Item
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
                if (map) map.flyTo([u.lat, u.lon], 6, { duration: 1.5 });
                openModal(u);
            };
            uniListContainer.appendChild(div);
        });

        // Fit map bounds if there are markers
        if (map && data.length > 0) {
            const bounds = data.map(uni => [uni.lat, uni.lon]);
            if(bounds.length > 0) {
                 map.fitBounds(bounds, { padding: [50, 50], maxZoom: 4 });
            }
        } else if (map) {
            map.setView([45, 20], 3); // Reset view
        }
    }

    /* ---------------- FILTER LOGIC ---------------- */
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

    // Search Listener
    document.getElementById('searchBox').addEventListener('input', () => {
        const activeBtn = document.querySelector('.filter-tag.active');
        if (activeBtn) {
            filterList(activeBtn.textContent.trim() === 'Все' ? 'all' : activeBtn.textContent.trim(), activeBtn);
        } else {
            filterList('all', document.querySelector('.filter-tags .filter-tag'));
        }
    });

    /* ---------------- TIMELINE LOGIC ---------------- */
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

    /* ---------------- FAQ LOGIC ---------------- */
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

        // Close others in the same category
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

    /* ---------------- DOUBLE DEGREE LOGIC (NEW) ---------------- */
    function renderDoubleDegreePrograms() {
        const container = document.getElementById('double-degree-content');
        container.innerHTML = ''; 
        
        const levels = [...new Set(doubleDegreePrograms.map(p => p.level))];
        
        levels.forEach(level => {
            const levelPrograms = doubleDegreePrograms.filter(p => p.level === level);
            
            let html = `
                <h3 style="border-bottom: 2px solid var(--accent); padding-bottom: 10px; margin-top: 40px; margin-bottom: 20px; display: inline-block; color: var(--secondary);">${level}</h3>
                <div class="double-degree-grid">
            `;
            
            levelPrograms.forEach(p => {
                html += `
                    <div class="info-card">
                        <div class="dd-info-label">${p.country}</div>
                        <div class="dd-info-value" style="font-size: 1.2rem;">${p.partner}</div>
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


    /* ---------------- MODAL LOGIC ---------------- */
    function openModal(u) {
        document.getElementById('m-name').textContent = u.name;
        document.getElementById('m-city').textContent = u.city;
        document.getElementById('m-country').textContent = u.country;
        document.getElementById('m-students').textContent = u.students;
        document.getElementById('m-rank').textContent = "Top Partner"; // Placeholder functionality

        // Populate Lists
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

        // Estimate Cost Logic (Dummy logic for serious functionality feel)
        let rent = "€300-500";
        if(u.country === "Франция" || u.country === "Германия") rent = "€600-900";
        else if(u.country === "Польша" || u.country === "Литва") rent = "€250-400";
        document.getElementById('cost-living').textContent = rent;

        document.getElementById('uniModal').classList.add('open');
        modalTab('tab-overview', document.querySelector('.modal-nav .m-tab:first-child')); // Сброс на вкладку Обзор
    }

    function closeModal() {
        document.getElementById('uniModal').classList.remove('open');
    }

    function modalTab(targetId, btn) {
        // Buttons
        document.querySelectorAll('.modal-nav .m-tab').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        // Panels
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(targetId).classList.add('active');
    }

    /* ---------------- NAVIGATION ---------------- */
    function switchTab(sectionId, btn) {
        document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(sectionId).classList.add('active');
        
        if(sectionId === 'map-section') {
            // Force map to re-render to fix potential display bugs after div was hidden/shown
            setTimeout(() => {
                if(map) map.invalidateSize();
            }, 200);
        }
        
        // Для мобильной навигации: скроллим меню, чтобы активный элемент был виден
        if (window.innerWidth <= 900) {
            btn.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
        }
    }

    // Start
    init();

    // Close modal on outside click
    window.onclick = function(event) {
        const modal = document.getElementById('uniModal');
        if (event.target == modal) {
            closeModal();
        }
    }
    </script>
</body>
</html>
