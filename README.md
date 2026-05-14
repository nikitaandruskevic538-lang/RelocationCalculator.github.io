# Welcome to Russia
Калькулятор переезда — прогнозирует цены на квартиру, еду и транспорт. Учитывайте аренду, продукты, топливо и проезд. Получите детальную смету расходов на новом месте. Сравнивайте города, планируйте бюджет и избегайте неожиданных трат. Подходит для переездов в другой город или страну. Бесплатно и удобно.

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Калькулятор переезда | Сравнение 30 городов России</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f0e1, #e8dccc);
            min-height: 100vh;
            padding: 40px 20px;
            transition: all 0.3s ease;
        }
        body.dark-theme {
            background: linear-gradient(135deg, #0f0f1a, #1a1a2e);
            color: #e0e0e0;
        }
        body.dark-theme .title-frame { background: rgba(30,30,50,0.9); border-color: #7c3aed; }
        body.dark-theme .title-frame h1 { color: #f0f0f0; }
        body.dark-theme .description-text { background: rgba(30,30,50,0.8); color: #ccc; }
        body.dark-theme .calculator-card { background: rgba(20,20,40,0.95); }
        body.dark-theme .results { background: rgba(30,30,50,0.9); color: #e0e0e0; }
        body.dark-theme .budget-card { background: linear-gradient(135deg, #2a2a3e, #1e1e2e); color: #e0e0e0; }
        body.dark-theme .budget-card h4, body.dark-theme .budget-card .budget-amount { color: #c084fc; }
        body.dark-theme .chart { background: #2a2a3e; }
        body.dark-theme .chart-title { color: #f0f0f0; }
        body.dark-theme .sources { background: #1e1e2e; color: #bbb; }
        body.dark-theme .footer { color: #aaa; }
        body.dark-theme .download-frame { background: rgba(30,30,50,0.9); border-color: #7c3aed; }
        body.dark-theme .download-frame p, body.dark-theme .download-frame .download-title, body.dark-theme .download-frame .offline-note { color: #e0e0e0; }
        body.dark-theme .metric { background: linear-gradient(135deg, #2a2a3e, #1e1e2e); color: #e0e0e0; }
        body.dark-theme .advice { background: #2a2a3e; border-left-color: #c084fc; color: #e0e0e0; }
        body.dark-theme .inflation-chart { background: #2a2a3e; }
        body.dark-theme .bar-label span { color: #e0e0e0; }
        body.dark-theme .radio-group label { color: #e0e0e0; }
        body.dark-theme .rating-frame { background: rgba(30,30,50,0.8); }
        body.dark-theme .rating-frame h3 { color: #f0f0f0; }
        body.dark-theme .rating-item { background: #2a2a3e; color: #e0e0e0; }
        body.dark-theme .rating-item .city { color: #e0e0e0; }
        body.dark-theme .yearly-bonus { background: linear-gradient(135deg, #27ae6030, #2ecc7130); color: #e0e0e0; }
        body.dark-theme .yearly-bonus span { color: #2ecc71; }
        body.dark-theme table td { color: #e0e0e0; }
        body.dark-theme table tr { border-bottom-color: #3a3a4e; }
        body.dark-theme table tr:nth-child(even) { background: #2a2a3e; }
        body.dark-theme .forecast-card { background: linear-gradient(135deg, #a888e520, #8b6bd620); color: #e0e0e0; }
        body.dark-theme .kids-slider-label { color: #ddd; }
        .container { max-width: 1400px; margin: 0 auto; }
        .theme-toggle {
            position: fixed; top: 20px; right: 20px;
            background: rgba(107,76,58,0.8); backdrop-filter: blur(8px);
            border: none; border-radius: 50px; padding: 10px 18px;
            cursor: pointer; font-size: 1rem; font-weight: 500;
            color: white; z-index: 1000; transition: all 0.2s;
        }
        .theme-toggle:hover { transform: scale(1.02); background: rgba(107,76,58,1); }
        body.dark-theme .theme-toggle { background: rgba(124,58,237,0.8); }
        .title-wrapper { text-align: center; margin-bottom: 20px; }
        .title-frame {
            border: 3px solid #b68b6b; border-radius: 60px; padding: 18px 35px;
            display: inline-block; background: rgba(255,248,235,0.7);
            backdrop-filter: blur(4px); transition: all 0.3s;
        }
        h1 { font-size: 2rem; color: #6b4c3a; margin: 0; }
        .description { text-align: center; margin-bottom: 30px; }
        .description-text {
            font-size: 1.1rem; color: #7a5a48; background: rgba(255,248,235,0.6);
            backdrop-filter: blur(4px); border-radius: 40px; padding: 10px 25px;
            display: inline-block; border: 1px solid rgba(182,139,107,0.3);
        }
        .calculator-card {
            background: rgba(30,30,50,0.85); backdrop-filter: blur(10px);
            border-radius: 24px; padding: 30px; margin-bottom: 30px;
            border: 1px solid rgba(255,255,255,0.15); transition: all 0.3s;
        }
        .calculator-card:hover { box-shadow: 0 25px 60px rgba(0,0,0,0.25); }
        .calculator-card label { color: #ddd; }
        .calculator-card select, .calculator-card input {
            background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
            color: white; transition: all 0.2s; border-radius: 14px; padding: 12px 15px;
        }
        .calculator-card select:focus, .calculator-card input:focus { border-color: #a888e5; transform: scale(1.01); }
        .calculator-card select option { background: #2a2a3e; color: white; }
        .family-salary {
            background: rgba(255,255,255,0.08); border-left: 4px solid #a888e5;
            padding: 15px; border-radius: 14px; margin-top: 10px; display: none;
        }
        .kids-block {
            margin: 15px 0; padding: 12px; background: rgba(255,255,255,0.05);
            border-radius: 14px; display: none;
        }
        .kids-block label { margin-bottom: 8px; display: block; }
        .kids-slider {
            width: 100%; margin: 10px 0; cursor: pointer;
        }
        .kids-value {
            font-weight: bold; color: #a888e5; font-size: 1.2rem;
            display: inline-block; margin-left: 10px;
        }
        button {
            background: linear-gradient(135deg, #a888e5, #8b6bd6);
            transition: all 0.2s; width: 100%; padding: 15px;
            color: white; border: none; border-radius: 14px;
            font-size: 18px; font-weight: bold; cursor: pointer;
        }
        button:hover { transform: translateY(-2px); box-shadow: 0 10px 25px rgba(139,107,214,0.4); }
        .download-section { margin: 30px 0; text-align: center; }
        .download-frame {
            background: rgba(255,248,235,0.85); backdrop-filter: blur(4px);
            border: 2px solid #b68b6b; border-radius: 24px; padding: 25px;
            max-width: 500px; margin: 0 auto; transition: transform 0.2s;
        }
        .download-frame:hover { transform: translateY(-3px); }
        .download-frame .download-title { font-size: 1.3rem; font-weight: bold; color: #5a3e2e; }
        .download-btn {
            display: inline-block; background: linear-gradient(135deg, #6b4c3a, #8b6a54);
            color: white; border-radius: 50px; padding: 12px 28px;
            font-weight: bold; cursor: pointer; transition: all 0.2s;
        }
        .download-btn:hover { transform: translateY(-2px); }
        .rating-section { margin-bottom: 30px; }
        .rating-frame {
            background: rgba(255,248,235,0.7); backdrop-filter: blur(4px);
            border-radius: 20px; padding: 20px; text-align: center;
            border: 1px solid rgba(182,139,107,0.3);
        }
        .rating-list { display: flex; justify-content: center; gap: 30px; flex-wrap: wrap; }
        .rating-item {
            background: white; border-radius: 15px; padding: 12px 20px;
            min-width: 120px; transition: transform 0.2s;
        }
        .rating-item:hover { transform: translateY(-3px); }
        .rating-item .place { font-size: 1.5rem; font-weight: bold; color: #a888e5; }
        .rating-item .city { font-weight: 600; color: #4a3b2e; }
        .rating-item .diff { font-size: 0.85rem; color: #27ae60; }
        .yearly-bonus {
            margin-top: 20px; padding: 15px;
            background: linear-gradient(135deg, #27ae6020, #2ecc7120);
            border-radius: 16px; text-align: center; border-left: 4px solid #27ae60;
        }
        .yearly-bonus span { font-weight: bold; color: #27ae60; font-size: 1.3rem; }
        .loading { display: inline-block; width: 20px; height: 20px; border: 2px solid rgba(255,255,255,0.3); border-radius: 50%; border-top-color: white; animation: spin 0.6s linear infinite; margin-left: 10px; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .results { background: rgba(255,248,235,0.9); border-radius: 24px; padding: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.1); display: none; transition: all 0.3s; }
        .bar-fill { transition: width 0.6s cubic-bezier(0.4,0,0.2,1); }
        .results .metric { background: linear-gradient(135deg, #e8e0d0, #ddd0bc); }
        .results .metric h3, .results .metric p { color: #4a3b2e; }
        .budget-card { background: linear-gradient(135deg, #f5efe5, #e8e0d4); transition: transform 0.2s; border-radius: 15px; padding: 25px; text-align: center; }
        .budget-card:hover { transform: translateY(-3px); }
        .budget-card h4, .budget-card .budget-amount { color: #5a4a3a; }
        .advice { padding: 18px; border-radius: 10px; margin-top: 20px; background: #fff3cd; border-left: 4px solid #ffc107; color: #856404; }
        .sources { background: #e8f0f5; color: #4a627a; padding: 15px; border-radius: 10px; margin-top: 20px; font-size: 11px; }
        .footer { text-align: center; margin-top: 30px; font-size: 12px; color: #7a5a48; }
        .row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
        .input-group { margin-bottom: 20px; }
        label { display: block; margin-bottom: 8px; font-weight: 600; }
        .radio-group { display: flex; gap: 20px; padding: 10px 0; }
        .radio-group label { display: flex; align-items: center; gap: 8px; font-weight: normal; cursor: pointer; }
        .radio-group input { width: auto; }
        .chart { margin-bottom: 30px; padding: 20px; background: #f8f9fa; border-radius: 15px; }
        .chart-title { font-size: 18px; font-weight: bold; margin-bottom: 20px; border-left: 4px solid #a888e5; padding-left: 12px; }
        .bar-container { margin-bottom: 20px; }
        .bar-label { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 14px; font-weight: 500; }
        .bar-bg { background: #e0e0e0; border-radius: 10px; overflow: hidden; height: 38px; }
        .bar-fill { height: 100%; display: flex; align-items: center; justify-content: flex-end; padding-right: 15px; color: white; font-weight: bold; border-radius: 10px; }
        .city-a { background: #3498db; }
        .city-b { background: #e67e22; }
        .two-columns { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
        .budget-amount { font-size: 32px; font-weight: bold; margin: 15px 0; }
        .forecast-card { background: linear-gradient(135deg, #a888e520, #8b6bd620); padding: 20px; border-radius: 15px; margin-top: 20px; }
        .inflation-chart { margin-top: 20px; padding: 20px; background: #f8f9fa; border-radius: 15px; }
        canvas { max-height: 300px; width: 100%; }
        @media (max-width: 768px) {
            .row, .two-columns { grid-template-columns: 1fr; }
            .percent-change { font-size: 36px; }
            .rating-list { flex-direction: column; align-items: center; }
        }
    </style>
</head>
<body>
    <button class="theme-toggle" onclick="toggleTheme()">🌙 Тёмная тема</button>
    <div class="container">
        <div class="title-wrapper"><div class="title-frame"><h1>💰 Калькулятор переезда</h1></div></div>
        <div class="description"><div class="description-text">🔮 Прогнозирует цены на квартиру, еду и транспорт | Сравнение 30 городов России</div></div>

        <div class="calculator-card">
            <div class="row">
                <div class="input-group"><label>📍 Откуда переезжаете?</label><select id="cityFrom"></select></div>
                <div class="input-group"><label>📍 Куда переезжаете?</label><select id="cityTo"></select></div>
            </div>
            <div class="input-group"><label>💰 Ваша зарплата (₽ в месяц)</label><input type="number" id="salarySelf" value="90000" oninput="updateRatingOnly()"></div>
            <div class="input-group"><label>👨‍👩‍👧 Сценарий проживания</label>
                <div class="radio-group">
                    <label><input type="radio" name="scenario" value="single" checked onchange="toggleFamilyOptions()"> 🧑 Один</label>
                    <label><input type="radio" name="scenario" value="family" onchange="toggleFamilyOptions()"> 👨‍👩‍👧 Семья (с детьми)</label>
                </div>
            </div>
            <div id="partnerSalaryDiv" class="family-salary">
                <div class="input-group"><label>👤 Зарплата партнера/супруга (₽)</label><input type="number" id="salaryPartner" value="50000" oninput="updateRatingOnly()"></div>
            </div>
            <div id="kidsBlock" class="kids-block">
                <label>👶 Количество детей: <span id="kidsValueDisplay" class="kids-value">0</span></label>
                <input type="range" id="kidsCount" class="kids-slider" min="0" max="5" value="0" step="1" oninput="updateKidsDisplay(); updateRatingOnly();">
                <div style="display: flex; justify-content: space-between; font-size: 12px; color: #aaa;"><span>0</span><span>1</span><span>2</span><span>3</span><span>4</span><span>5</span></div>
            </div>
            <div class="input-group"><label>📈 Прогноз на будущее (лет)</label>
                <select id="forecastYears"><option value="0">Без прогноза</option><option value="1" selected>1 год</option><option value="2">2 года</option><option value="3">3 года</option><option value="5">5 лет</option></select>
            </div>
            <button onclick="calculateWithLoading()">📊 Рассчитать переезд</button>
        </div>

        <div class="rating-section"><div class="rating-frame"><h3>🏆 ТОП-3 города для переезда по вашим параметрам</h3><div class="rating-list" id="ratingList">📊 Выберите город и зарплату</div></div></div>

        <div class="download-section"><div class="download-frame"><div class="download-title">📥 Скачать калькулятор</div><p>🇷🇺 Россия — сравнение 30 городов</p><div class="offline-note">💡 Сохраните HTML-файл и пользуйтесь без интернета (графики требуют Wi-Fi)</div><button class="download-btn" onclick="downloadCalculator()">⬇️ Скачать .html</button></div></div>

        <div id="results" class="results"></div>
        <div class="footer"><p>© 2026 Калькулятор "Цена переезда" | 30 городов | Данные: Росстат, Циан, 2ГИС, Едадил</p></div>
    </div>

    <script>
        // ========== ДАННЫЕ ПО ГОРОДАМ ==========
        const citiesData = {
            "Москва": { salary: 132000, rent1Center: 58000, rent1Suburb: 42000, rent2Room: 89000, rent3Room: 120000, foodBasket: 9120, transportMonth: 3400, gasoline: 56.5, taxi: 550, utils: 7800 },
            "Санкт-Петербург": { salary: 101000, rent1Center: 48000, rent1Suburb: 34000, rent2Room: 74000, rent3Room: 100000, foodBasket: 8890, transportMonth: 2950, gasoline: 54.2, taxi: 480, utils: 6700 },
            "Казань": { salary: 71000, rent1Center: 35000, rent1Suburb: 27000, rent2Room: 54000, rent3Room: 73000, foodBasket: 6850, transportMonth: 2650, gasoline: 52.0, taxi: 380, utils: 5600 },
            "Екатеринбург": { salary: 74000, rent1Center: 33000, rent1Suburb: 25000, rent2Room: 51000, rent3Room: 69000, foodBasket: 7120, transportMonth: 2850, gasoline: 53.1, taxi: 410, utils: 5800 },
            "Новосибирск": { salary: 66000, rent1Center: 31000, rent1Suburb: 24000, rent2Room: 49000, rent3Room: 66000, foodBasket: 6980, transportMonth: 2750, gasoline: 53.0, taxi: 390, utils: 5900 },
            "Краснодар": { salary: 68000, rent1Center: 36000, rent1Suburb: 28000, rent2Room: 56000, rent3Room: 76000, foodBasket: 6750, transportMonth: 2650, gasoline: 52.5, taxi: 370, utils: 5500 },
            "Нижний Новгород": { salary: 64000, rent1Center: 30000, rent1Suburb: 23000, rent2Room: 47000, rent3Room: 64000, foodBasket: 6640, transportMonth: 2650, gasoline: 52.0, taxi: 360, utils: 5400 },
            "Ростов-на-Дону": { salary: 65000, rent1Center: 33000, rent1Suburb: 26000, rent2Room: 52000, rent3Room: 70000, foodBasket: 6780, transportMonth: 2650, gasoline: 52.2, taxi: 380, utils: 5600 },
            "Самара": { salary: 63000, rent1Center: 29000, rent1Suburb: 22000, rent2Room: 46000, rent3Room: 62000, foodBasket: 6550, transportMonth: 2550, gasoline: 51.5, taxi: 350, utils: 5300 },
            "Владивосток": { salary: 89000, rent1Center: 51000, rent1Suburb: 37000, rent2Room: 79000, rent3Room: 107000, foodBasket: 10350, transportMonth: 3200, gasoline: 61.0, taxi: 480, utils: 7200 },
            "Воронеж": { salary: 55000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 38000, rent3Room: 51000, foodBasket: 6200, transportMonth: 2300, gasoline: 51.0, taxi: 280, utils: 4800 },
            "Ярославль": { salary: 53000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 36000, rent3Room: 49000, foodBasket: 6100, transportMonth: 2200, gasoline: 51.0, taxi: 270, utils: 4700 },
            "Тула": { salary: 52000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, rent3Room: 47000, foodBasket: 6000, transportMonth: 2200, gasoline: 50.5, taxi: 260, utils: 4600 },
            "Сочи": { salary: 70000, rent1Center: 45000, rent1Suburb: 32000, rent2Room: 68000, rent3Room: 92000, foodBasket: 7500, transportMonth: 2800, gasoline: 53.0, taxi: 400, utils: 6500 },
            "Волгоград": { salary: 54000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 37000, rent3Room: 50000, foodBasket: 6100, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4700 },
            "Севастополь": { salary: 56000, rent1Center: 28000, rent1Suburb: 20000, rent2Room: 43000, rent3Room: 58000, foodBasket: 6400, transportMonth: 2400, gasoline: 52.0, taxi: 300, utils: 5000 },
            "Уфа": { salary: 58000, rent1Center: 26000, rent1Suburb: 19000, rent2Room: 40000, rent3Room: 54000, foodBasket: 6200, transportMonth: 2400, gasoline: 51.5, taxi: 290, utils: 4900 },
            "Пермь": { salary: 57000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 39000, rent3Room: 53000, foodBasket: 6150, transportMonth: 2400, gasoline: 51.5, taxi: 280, utils: 4900 },
            "Саратов": { salary: 52000, rent1Center: 22000, rent1Suburb: 15000, rent2Room: 34000, rent3Room: 46000, foodBasket: 5900, transportMonth: 2200, gasoline: 50.5, taxi: 260, utils: 4500 },
            "Ижевск": { salary: 53000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, rent3Room: 47000, foodBasket: 6000, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4600 },
            "Челябинск": { salary: 55000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 37000, rent3Room: 50000, foodBasket: 6100, transportMonth: 2400, gasoline: 51.5, taxi: 280, utils: 4800 },
            "Тюмень": { salary: 70000, rent1Center: 32000, rent1Suburb: 24000, rent2Room: 50000, rent3Room: 68000, foodBasket: 6800, transportMonth: 2700, gasoline: 52.5, taxi: 350, utils: 5500 },
            "Курган": { salary: 48000, rent1Center: 19000, rent1Suburb: 13000, rent2Room: 29000, rent3Room: 39000, foodBasket: 5600, transportMonth: 2000, gasoline: 50.0, taxi: 230, utils: 4200 },
            "Красноярск": { salary: 64000, rent1Center: 30000, rent1Suburb: 22000, rent2Room: 46000, rent3Room: 62000, foodBasket: 6600, transportMonth: 2600, gasoline: 52.5, taxi: 340, utils: 5400 },
            "Иркутск": { salary: 62000, rent1Center: 29000, rent1Suburb: 21000, rent2Room: 45000, rent3Room: 61000, foodBasket: 6500, transportMonth: 2600, gasoline: 52.5, taxi: 330, utils: 5300 },
            "Кемерово": { salary: 53000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, rent3Room: 47000, foodBasket: 6000, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4600 },
            "Томск": { salary: 56000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 38000, rent3Room: 51000, foodBasket: 6200, transportMonth: 2400, gasoline: 51.5, taxi: 290, utils: 4800 },
            "Барнаул": { salary: 51000, rent1Center: 21000, rent1Suburb: 14000, rent2Room: 32000, rent3Room: 43000, foodBasket: 5800, transportMonth: 2100, gasoline: 50.5, taxi: 250, utils: 4400 },
            "Хабаровск": { salary: 75000, rent1Center: 42000, rent1Suburb: 30000, rent2Room: 65000, rent3Room: 88000, foodBasket: 8500, transportMonth: 3000, gasoline: 58.0, taxi: 420, utils: 6800 },
            "Якутск": { salary: 82000, rent1Center: 55000, rent1Suburb: 40000, rent2Room: 85000, rent3Room: 115000, foodBasket: 11000, transportMonth: 3500, gasoline: 65.0, taxi: 500, utils: 8500 },
            "Калининград": { salary: 60000, rent1Center: 30000, rent1Suburb: 22000, rent2Room: 46000, rent3Room: 62000, foodBasket: 6700, transportMonth: 2600, gasoline: 52.0, taxi: 340, utils: 5400 }
        };
        for (let city in citiesData) { if (!citiesData[city].rent3Room) citiesData[city].rent3Room = citiesData[city].rent2Room * 1.35; }

        const seasonalFactors = {1:1.12,2:1.05,3:1.00,4:0.98,5:0.97,6:0.96,7:0.95,8:0.96,9:0.98,10:1.00,11:1.03,12:1.08};
        const inflationMonthly = {1:1.05,2:0.72,3:0.48,4:0.44,5:0.55,6:0.50,7:0.60,8:0.35,9:0.45,10:0.65,11:0.90,12:1.10};
        const inflationData = { rate: 0.095 };
        let inflationChart = null;

        const cities = Object.keys(citiesData);
        const cityFromSelect = document.getElementById('cityFrom');
        const cityToSelect = document.getElementById('cityTo');
        cities.forEach(city => { cityFromSelect.add(new Option(city, city)); cityToSelect.add(new Option(city, city)); });
        cityFromSelect.value = "Москва";
        cityToSelect.value = "Санкт-Петербург";

        function toggleFamilyOptions() {
            const isFamily = document.querySelector('input[name="scenario"]:checked').value === 'family';
            document.getElementById('partnerSalaryDiv').style.display = isFamily ? 'block' : 'none';
            document.getElementById('kidsBlock').style.display = isFamily ? 'block' : 'none';
            updateRatingOnly();
        }
        function updateKidsDisplay() { document.getElementById('kidsValueDisplay').innerText = document.getElementById('kidsCount').value; updateRatingOnly(); }
        function getKidsCount() { return parseInt(document.getElementById('kidsCount').value) || 0; }
        function getTotalSalary() {
            const isFamily = document.querySelector('input[name="scenario"]:checked').value === 'family';
            const self = parseFloat(document.getElementById('salarySelf').value) || 0;
            const partner = isFamily ? (parseFloat(document.getElementById('salaryPartner').value) || 0) : 0;
            return self + partner;
        }
        function formatNumber(num) { return Math.round(num).toLocaleString('ru-RU'); }
        function calculateForecast(currentValue, years) { return currentValue * Math.pow(1 + inflationData.rate, years); }
        function getMonthlyForecast(startValue, months) {
            let forecast = [], currentValue = startValue, currentMonth = new Date().getMonth() + 1;
            for (let i = 0; i < months; i++) {
                let month = ((currentMonth + i - 1) % 12) + 1;
                currentValue = currentValue * (1 + inflationMonthly[month]/100) * seasonalFactors[month];
                forecast.push({ month, value: currentValue, seasonal: seasonalFactors[month], inflation: inflationMonthly[month]/100 });
            }
            return forecast;
        }
        function getExpensesForCity(cityData, scenario, kids) {
            const isFamily = scenario === 'family';
            let rent, utils, food = cityData.foodBasket;
            if (isFamily) {
                if (kids >= 2) rent = cityData.rent3Room || cityData.rent2Room * 1.35;
                else rent = cityData.rent2Room;
                utils = cityData.utils * 1.3;
                food = cityData.foodBasket * (1 + kids * 0.25);
            } else {
                rent = cityData.rent1Suburb;
                utils = cityData.utils;
            }
            return rent + utils + food + cityData.transportMonth;
        }
        function updateRatingOnly() {
            const scenario = document.querySelector('input[name="scenario"]:checked').value;
            const kids = getKidsCount();
            const totalSalary = getTotalSalary();
            const ratings = [];
            for (const city of cities) {
                const expenses = getExpensesForCity(citiesData[city], scenario, kids);
                const freeMoney = totalSalary - expenses;
                ratings.push({ city, freeMoney });
            }
            ratings.sort((a,b) => b.freeMoney - a.freeMoney);
            const top3 = ratings.slice(0,3);
            document.getElementById('ratingList').innerHTML = top3.map((r,idx) => {
                const medal = idx===0?'🥇':idx===1?'🥈':'🥉';
                return `<div class="rating-item"><div class="place">${medal} #${idx+1}</div><div class="city">${r.city}</div><div class="diff">💰 ${Math.round(r.freeMoney).toLocaleString()} ₽/мес</div></div>`;
            }).join('');
        }
        function showYearlyBonus(freeA, freeB, cityA, cityB) {
            const yearlyDiff = (freeB - freeA) * 12;
            if (yearlyDiff > 0) return `<div class="yearly-bonus">🎉 Если переедете в ${cityB}, ваш годовой бонус составит <span>+${Math.round(yearlyDiff).toLocaleString()} ₽</span> в год!</div>`;
            if (yearlyDiff < 0) return `<div class="yearly-bonus">⚠️ Переезд в ${cityB} может уменьшить ваш годовой бюджет на <span>${Math.round(Math.abs(yearlyDiff)).toLocaleString()} ₽</span></div>`;
            return '';
        }
        function createInflationChart(forecastData, cityName) {
            const ctx = document.getElementById('inflationChartCanvas');
            if (!ctx) return;
            if (inflationChart) inflationChart.destroy();
            inflationChart = new Chart(ctx, {
                type: 'line', data: { labels: forecastData.map((_,i)=>`${i+1} мес`), datasets: [
                    { label: `Прогноз расходов в ${cityName} (₽)`, data: forecastData.map(d=>d.value), borderColor: '#e74c3c', backgroundColor: 'rgba(231,76,60,0.1)', borderWidth: 3, fill: true, tension: 0.4, yAxisID: 'y' },
                    { label: 'Сезонность (%)', data: forecastData.map(d=>(d.seasonal-1)*100), borderColor: '#3498db', borderWidth: 2, fill: false, yAxisID: 'y1' },
                    { label: 'Инфляция (%)', data: forecastData.map(d=>d.inflation*100), borderColor: '#27ae60', borderWidth: 2, fill: false, yAxisID: 'y1' }
                ]}, options: { responsive: true, plugins: { legend: { position: 'top' } }, scales: { y: { title: { display: true, text: 'Расходы (₽)' } }, y1: { position: 'right', title: { display: true, text: 'Процент (%)' } } } }
            });
        }
        function createRentChart(dataA, dataB, cityA, cityB, scenario, kids) {
            const isFamily = scenario === 'family';
            let rentA, rentB;
            if (isFamily) { rentA = kids>=2 ? (dataA.rent3Room||dataA.rent2Room*1.35) : dataA.rent2Room; rentB = kids>=2 ? (dataB.rent3Room||dataB.rent2Room*1.35) : dataB.rent2Room; }
            else { rentA = dataA.rent1Suburb; rentB = dataB.rent1Suburb; }
            const maxRent = Math.max(dataA.rent1Center, dataB.rent1Center, dataA.rent1Suburb, dataB.rent1Suburb, dataA.rent2Room, dataB.rent2Room, dataA.rent3Room||0, dataB.rent3Room||0);
            return `<div class="bar-container"><div class="bar-label"><span>🏢 1-комнатная в центре</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Center)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Center)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width:${(dataA.rent1Center/maxRent)*100}%">${Math.round((dataA.rent1Center/maxRent)*100)}%</div></div><div class="bar-bg" style="margin-top:5px"><div class="bar-fill city-b" style="width:${(dataB.rent1Center/maxRent)*100}%">${Math.round((dataB.rent1Center/maxRent)*100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span>🏘️ 1-комнатная в спальном</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Suburb)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Suburb)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width:${(dataA.rent1Suburb/maxRent)*100}%">${Math.round((dataA.rent1Suburb/maxRent)*100)}%</div></div><div class="bar-bg" style="margin-top:5px"><div class="bar-fill city-b" style="width:${(dataB.rent1Suburb/maxRent)*100}%">${Math.round((dataB.rent1Suburb/maxRent)*100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span>👨‍👩‍👧 2-комнатная (семья)</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent2Room)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent2Room)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width:${(dataA.rent2Room/maxRent)*100}%">${Math.round((dataA.rent2Room/maxRent)*100)}%</div></div><div class="bar-bg" style="margin-top:5px"><div class="bar-fill city-b" style="width:${(dataB.rent2Room/maxRent)*100}%">${Math.round((dataB.rent2Room/maxRent)*100)}%</div></div></div>
            <div style="margin-top:10px;font-size:11px;color:#999;">📍 Ваш вариант аренды: <strong>${isFamily?(kids>=2?'3-комнатная':'2-комнатная'):'1-комнатная в спальном'}</strong> — ${formatNumber(isFamily?(kids>=2?rentA:rentA):rentA)} ₽ (${cityA}) / ${formatNumber(isFamily?(kids>=2?rentB:rentB):rentB)} ₽ (${cityB})</div>`;
        }
        function getAdvice(percentChange, cityB, freeB, freeA) {
            const diff = Math.abs(freeB-freeA);
            if (percentChange > 15) return `Переезд в ${cityB} увеличит бюджет на ${formatNumber(diff)} ₽. Отличная возможность!`;
            if (percentChange > 5) return `Переезд в ${cityB} даст +${formatNumber(diff)} ₽ в месяц. Хороший вариант.`;
            if (percentChange > -5) return `Разница незначительна. Решайте по нефинансовым факторам.`;
            if (percentChange > -15) return `Переезд уменьшит бюджет на ${formatNumber(diff)} ₽. Стоит подумать.`;
            return `⚠️ Переезд в ${cityB} сильно уменьшит бюджет на ${formatNumber(diff)} ₽. Не рекомендуется.`;
        }
        function calculate() {
            const cityA = cityFromSelect.value, cityB = cityToSelect.value;
            const salarySelf = parseFloat(document.getElementById('salarySelf').value)||0;
            const salaryPartner = parseFloat(document.getElementById('salaryPartner').value)||0;
            const scenario = document.querySelector('input[name="scenario"]:checked').value;
            const kids = getKidsCount();
            const totalSalary = salarySelf + (scenario==='family'?salaryPartner:0);
            const dataA = citiesData[cityA], dataB = citiesData[cityB];
            const expensesA = getExpensesForCity(dataA, scenario, kids);
            const expensesB = getExpensesForCity(dataB, scenario, kids);
            const freeA = totalSalary - expensesA, freeB = totalSalary - expensesB;
            const percentChange = freeA!==0 ? ((freeB-freeA)/Math.abs(freeA))*100 : 0;
            const monthlyForecast = getMonthlyForecast(expensesB, 12);
            let forecastHtml = '';
            const forecastYears = parseInt(document.getElementById('forecastYears').value);
            if (forecastYears > 0) {
                const forecastFreeB = calculateForecast(freeB, forecastYears);
                const forecastExpensesB = calculateForecast(expensesB, forecastYears);
                forecastHtml = `<div class="forecast-card"><h4>📈 ПРОГНОЗ НА ${forecastYears} ${forecastYears===1?'год':forecastYears<=4?'года':'лет'}</h4><p>С учетом инфляции 9.5% в год</p><div class="two-columns"><div><strong>📊 Расходы в ${cityB}:</strong><br><span style="color:#e74c3c;">${formatNumber(expensesB)} ₽</span><br>→ <strong>${formatNumber(forecastExpensesB)} ₽</strong></div><div><strong>💰 Свободный остаток:</strong><br><span style="color:#27ae60;">${formatNumber(freeB)} ₽</span><br>→ <strong>${formatNumber(forecastFreeB)} ₽</strong></div></div></div>`;
            }
            const resultsHtml = `
                <div class="metric"><h3>⚡ ИЗМЕНЕНИЕ РЕАЛЬНОГО ДОХОДА</h3><div class="percent-change ${percentChange>=0?'positive':'negative'}">${percentChange>=0?'+':''}${Math.round(percentChange)}%</div><p>Свободный остаток в <strong>${cityB}</strong> ${percentChange>=0?'выше':'ниже'} на <strong>${Math.abs(Math.round(percentChange))}%</strong></p><p>💰 Общий доход: ${formatNumber(totalSalary)} ₽/мес | 👶 Детей: ${kids}</p></div>
                ${showYearlyBonus(freeA, freeB, cityA, cityB)}
                <div class="chart"><div class="chart-title">🏠 Сравнение аренды</div>${createRentChart(dataA, dataB, cityA, cityB, scenario, kids)}</div>
                <div class="two-columns"><div class="budget-card"><h4>💰 Бюджет в ${cityA}</h4><div class="budget-amount">${formatNumber(freeA)} ₽</div><div>🏠 Аренда: ${formatNumber(getExpensesForCity(dataA,scenario,kids)-dataA.transportMonth-(scenario==='family'?dataA.utils*1.3:dataA.utils)- (scenario==='family'?dataA.foodBasket*(1+kids*0.25):dataA.foodBasket))} ₽<br>🍎 Еда: ${formatNumber(scenario==='family'?dataA.foodBasket*(1+kids*0.25):dataA.foodBasket)} ₽<br>💡 ЖКХ: ${formatNumber(scenario==='family'?dataA.utils*1.3:dataA.utils)} ₽<br>🚍 Транспорт: ${formatNumber(dataA.transportMonth)} ₽</div></div>
                <div class="budget-card"><h4>💰 Бюджет в ${cityB}</h4><div class="budget-amount">${formatNumber(freeB)} ₽</div><div>🏠 Аренда: ${formatNumber(getExpensesForCity(dataB,scenario,kids)-dataB.transportMonth-(scenario==='family'?dataB.utils*1.3:dataB.utils)- (scenario==='family'?dataB.foodBasket*(1+kids*0.25):dataB.foodBasket))} ₽<br>🍎 Еда: ${formatNumber(scenario==='family'?dataB.foodBasket*(1+kids*0.25):dataB.foodBasket)} ₽<br>💡 ЖКХ: ${formatNumber(scenario==='family'?dataB.utils*1.3:dataB.utils)} ₽<br>🚍 Транспорт: ${formatNumber(dataB.transportMonth)} ₽</div></div></div>
                ${forecastHtml}
                <div class="advice"><strong>💡 Совет</strong><br>${getAdvice(percentChange, cityB, freeB, freeA)}</div>
                <div class="inflation-chart"><div class="chart-title">📈 Прогноз расходов в ${cityB}</div><canvas id="inflationChartCanvas"></canvas></div>
                <div class="sources"><strong>📊 ИСТОЧНИКИ ДАННЫХ (${new Date().toLocaleDateString('ru-RU')})</strong><br>• Доходы — Росстат • Аренда — Циан • Продукты — Росстат+Едадил • Транспорт — 2ГИС</div>
            `;
            document.getElementById('results').style.display = 'block';
            document.getElementById('results').innerHTML = resultsHtml;
            setTimeout(()=>createInflationChart(monthlyForecast, cityB),100);
            window.scrollTo({ top: document.getElementById('results').offsetTop-20, behavior:'smooth' });
        }
        function calculateWithLoading() { calculate(); }
        function downloadCalculator() {
            const htmlContent = document.documentElement.outerHTML;
            const blob = new Blob([htmlContent], {type:'text/html'});
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = 'calculator_pereezda_rossiya.html';
            a.click();
            URL.revokeObjectURL(a.href);
        }
        function toggleTheme() {
            document.body.classList.toggle('dark-theme');
            document.querySelector('.theme-toggle').innerHTML = document.body.classList.contains('dark-theme') ? '☀️ Светлая тема' : '🌙 Тёмная тема';
        }
        toggleFamilyOptions();
        updateKidsDisplay();
        updateRatingOnly();
        cityFromSelect.onchange = updateRatingOnly;
        cityToSelect.onchange = updateRatingOnly;
        document.getElementById('salarySelf').oninput = updateRatingOnly;
        document.getElementById('salaryPartner').oninput = updateRatingOnly;
        document.getElementById('kidsCount').oninput = function(){ updateKidsDisplay(); updateRatingOnly(); };
    </script>
</body>
</html>
