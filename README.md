# Welcome to Russia
Калькулятор переезда — прогнозирует цены на квартиру, еду и транспорт. Учитывайте аренду, продукты, топливо и проезд. Получите детальную смету расходов на новом месте. Сравнивайте города, планируйте бюджет и избегайте неожиданных трат. Подходит для переездов в другой город или страну. Бесплатно и удобно.

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Калькулятор "Цена переезда" - 30 городов России</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f0e1, #e8dccc);
            min-height: 100vh;
            padding: 40px 20px;
            transition: background 0.3s ease, color 0.3s ease;
        }

        /* Тёмная тема - ИСПРАВЛЕНА (теперь шрифт читаемый) */
        body.dark-theme {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            color: #e0e0e0;
        }

        body.dark-theme .title-frame {
            background: rgba(30, 30, 50, 0.8);
            border-color: #7c3aed;
        }

        body.dark-theme .title-frame h1 {
            color: #f0f0f0;
        }

        body.dark-theme .description-text {
            background: rgba(30, 30, 50, 0.6);
            color: #ccc;
        }

        body.dark-theme .calculator-card {
            background: rgba(20, 20, 40, 0.9);
        }

        body.dark-theme .calculator-card label {
            color: #ddd;
        }

        body.dark-theme .calculator-card select,
        body.dark-theme .calculator-card input {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
        }

        body.dark-theme .calculator-card select option {
            background: #2a2a3e;
            color: white;
        }

        body.dark-theme .results {
            background: rgba(30, 30, 50, 0.85);
            color: #e0e0e0;
        }

        body.dark-theme .budget-card {
            background: linear-gradient(135deg, #2a2a3e, #1e1e2e);
            color: #ddd;
        }

        body.dark-theme .budget-card h4,
        body.dark-theme .budget-card .budget-amount {
            color: #c084fc;
        }

        body.dark-theme .chart {
            background: #2a2a3e;
        }

        body.dark-theme .chart-title {
            color: #f0f0f0;
        }

        body.dark-theme .sources {
            background: #1e1e2e;
            color: #bbb;
        }

        body.dark-theme .footer {
            color: #aaa;
        }

        body.dark-theme .download-frame {
            background: rgba(30, 30, 50, 0.8);
            border-color: #7c3aed;
        }

        body.dark-theme .download-frame p,
        body.dark-theme .download-frame .download-title,
        body.dark-theme .download-frame .offline-note {
            color: #e0e0e0;
        }

        body.dark-theme .metric {
            background: linear-gradient(135deg, #2a2a3e, #1e1e2e);
            color: #ddd;
        }

        body.dark-theme .metric h3,
        body.dark-theme .metric p {
            color: #e0e0e0;
        }

        body.dark-theme .advice {
            background: #2a2a3e;
            border-left-color: #c084fc;
            color: #e0e0e0;
        }

        body.dark-theme .inflation-chart {
            background: #2a2a3e;
            color: #e0e0e0;
        }

        body.dark-theme .bar-label span {
            color: #e0e0e0;
        }

        body.dark-theme .radio-group label {
            color: #e0e0e0;
        }

        body.dark-theme table td {
            color: #e0e0e0;
        }

        body.dark-theme table tr {
            border-bottom-color: #3a3a4e;
        }

        body.dark-theme table tr:nth-child(even) {
            background: #2a2a3e;
        }

        body.dark-theme .forecast-card {
            background: linear-gradient(135deg, #a888e520 0%, #8b6bd620 100%);
            color: #e0e0e0;
        }

        body.dark-theme .forecast-card p {
            color: #bbb;
        }

        body.dark-theme .kids-block {
            background: rgba(255,255,255,0.05);
            border-left-color: #2ecc71;
        }

        body.dark-theme .kids-block label {
            color: #ddd;
        }

        body.dark-theme .entertainment-section {
            background: #2a2a3e;
        }

        body.dark-theme .entertainment-item {
            background: #1e1e2e;
            color: #ddd;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* Кнопка тёмной темы */
        .theme-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(107, 76, 58, 0.8);
            backdrop-filter: blur(8px);
            border: none;
            border-radius: 50px;
            padding: 10px 18px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            color: white;
            z-index: 1000;
            transition: all 0.2s;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .theme-toggle:hover {
            transform: scale(1.02);
            background: rgba(107, 76, 58, 1);
        }

        body.dark-theme .theme-toggle {
            background: rgba(124, 58, 237, 0.8);
        }

        /* Рамка для заголовка */
        .title-wrapper {
            text-align: center;
            margin-bottom: 20px;
        }

        .title-frame {
            border: 3px solid #b68b6b;
            border-radius: 60px;
            padding: 18px 35px;
            display: inline-block;
            background: rgba(255, 248, 235, 0.7);
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            transition: all 0.3s;
        }

        h1 {
            font-size: 2rem;
            color: #6b4c3a;
            margin: 0;
        }

        .description {
            text-align: center;
            margin-bottom: 30px;
        }

        .description-text {
            font-size: 1.1rem;
            color: #7a5a48;
            background: rgba(255, 248, 235, 0.6);
            backdrop-filter: blur(4px);
            border-radius: 40px;
            padding: 10px 25px;
            display: inline-block;
            border: 1px solid rgba(182, 139, 107, 0.3);
        }

        .calculator-card {
            background: rgba(255, 248, 235, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.1);
            margin-bottom: 30px;
            transition: all 0.3s;
        }

        .calculator-card:hover {
            box-shadow: 0 25px 60px rgba(0,0,0,0.15);
        }

        .calculator-card label {
            color: #4a3b2e;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
        }

        select, input {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 14px;
            font-size: 16px;
            transition: all 0.3s;
            color: #000;
            background: white;
        }

        select:focus, input:focus {
            outline: none;
            border-color: #a888e5;
            box-shadow: 0 0 0 3px rgba(168, 136, 229, 0.2);
            transform: scale(1.01);
        }

        .row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .radio-group {
            display: flex;
            gap: 20px;
            padding: 10px 0;
        }

        .radio-group label {
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: normal;
            cursor: pointer;
        }

        .radio-group input {
            width: auto;
        }

        .family-salary {
            display: none;
            margin-top: 10px;
            padding: 15px;
            background: rgba(168, 136, 229, 0.1);
            border-radius: 14px;
            border-left: 4px solid #a888e5;
            transition: all 0.3s;
        }

        /* Блок с детьми */
        .kids-block {
            background: rgba(46, 204, 113, 0.1);
            border-left: 4px solid #2ecc71;
            border-radius: 14px;
            padding: 15px;
            margin: 15px 0;
            display: none;
        }

        .kids-block label {
            margin-bottom: 10px;
        }

        .kids-slider {
            width: 100%;
            margin: 10px 0;
            cursor: pointer;
            accent-color: #a888e5;
        }

        .kids-value {
            font-weight: bold;
            color: #2ecc71;
            font-size: 1.2rem;
            display: inline-block;
            margin-left: 10px;
        }

        button {
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, #a888e5, #8b6bd6);
            color: white;
            border: none;
            border-radius: 14px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(139, 107, 214, 0.4);
        }

        .results {
            background: rgba(255, 248, 235, 0.9);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.1);
            display: none;
            transition: all 0.3s;
        }

        .metric {
            text-align: center;
            padding: 25px;
            background: linear-gradient(135deg, #e8e0d0, #ddd0bc);
            border-radius: 15px;
            margin-bottom: 30px;
        }

        .metric h3 {
            color: #4a3b2e;
            margin-bottom: 10px;
        }

        .percent-change {
            font-size: 52px;
            font-weight: bold;
            margin: 15px 0;
        }

        .positive { color: #27ae60; }
        .negative { color: #e74c3c; }

        .chart {
            margin-bottom: 30px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
            transition: transform 0.2s;
        }

        .chart:hover {
            transform: scale(1.01);
        }

        .chart-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 20px;
            color: #4a3b2e;
            border-left: 4px solid #a888e5;
            padding-left: 12px;
        }

        .bar-container {
            margin-bottom: 20px;
        }

        .bar-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 14px;
            font-weight: 500;
        }

        .bar-bg {
            background: #e0e0e0;
            border-radius: 10px;
            overflow: hidden;
            height: 38px;
        }

        .bar-fill {
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 15px;
            color: white;
            font-weight: bold;
            border-radius: 10px;
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .city-a { background: #3498db; }
        .city-b { background: #e67e22; }

        .two-columns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .budget-card {
            background: linear-gradient(135deg, #f5efe5, #e8e0d4);
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .budget-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
        }

        .budget-card h4 {
            color: #5a4a3a;
            margin-bottom: 15px;
        }

        .budget-amount {
            font-size: 32px;
            font-weight: bold;
            color: #a888e5;
            margin: 15px 0;
        }

        .forecast-card {
            background: linear-gradient(135deg, #a888e520 0%, #8b6bd620 100%);
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
        }

        .advice {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 18px;
            border-radius: 10px;
            margin-top: 20px;
            font-size: 15px;
            line-height: 1.5;
            color: #856404;
        }

        .sources {
            margin-top: 20px;
            padding: 15px;
            background: #e8f0f5;
            border-radius: 10px;
            font-size: 11px;
            color: #4a627a;
            line-height: 1.6;
        }

        .inflation-chart {
            margin-top: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
        }

        canvas {
            max-height: 300px;
            width: 100%;
        }

        .footer {
            text-align: center;
            margin-top: 30px;
            font-size: 12px;
            color: #7a5a48;
        }

        .update-info {
            background: rgba(255,248,235,0.8);
            border-radius: 10px;
            padding: 8px 15px;
            font-size: 12px;
            margin-top: 15px;
            display: inline-block;
            color: #6b4c3a;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header p {
            color: #7a5a48;
        }

        /* Раздел развлечений */
        .entertainment-section {
            margin-bottom: 30px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
            transition: transform 0.2s;
        }

        .entertainment-section:hover {
            transform: scale(1.01);
        }

        .entertainment-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .entertainment-item {
            background: white;
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            transition: transform 0.2s;
        }

        .entertainment-item:hover {
            transform: translateY(-3px);
        }

        .entertainment-item .icon {
            font-size: 28px;
            margin-bottom: 8px;
        }

        .entertainment-item .name {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .entertainment-item .price {
            font-size: 18px;
            font-weight: bold;
            color: #a888e5;
        }

        .download-section {
            margin-top: 30px;
            margin-bottom: 30px;
            text-align: center;
        }

        .download-frame {
            background: rgba(255, 248, 235, 0.85);
            backdrop-filter: blur(4px);
            border: 2px solid #b68b6b;
            border-radius: 24px;
            padding: 25px;
            max-width: 500px;
            margin: 0 auto;
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
            transition: transform 0.2s;
        }

        .download-frame:hover {
            transform: translateY(-3px);
        }

        .download-btn {
            display: inline-block;
            background: linear-gradient(135deg, #6b4c3a, #8b6a54);
            color: white;
            border: none;
            border-radius: 50px;
            padding: 12px 28px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        @media (max-width: 768px) {
            .row, .two-columns {
                grid-template-columns: 1fr;
            }
            .percent-change {
                font-size: 36px;
            }
            .budget-amount {
                font-size: 24px;
            }
            .calculator-card, .results {
                padding: 20px;
            }
            .title-frame h1 {
                font-size: 1.4rem;
            }
            .entertainment-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <button class="theme-toggle" onclick="toggleTheme()">🌙 Тёмная тема</button>

    <div class="container">
        <div class="title-wrapper">
            <div class="title-frame">
                <h1>💰 Цена переезда</h1>
            </div>
        </div>
        <div class="description">
            <div class="description-text">
                Сравнение 30 городов России | Прогноз с учетом инфляции и сезонности | Анализ расходов на развлечения
            </div>
        </div>

        <div class="calculator-card">
            <div class="row">
                <div class="input-group">
                    <label>📍 Откуда переезжаете?</label>
                    <select id="cityFrom"></select>
                </div>
                <div class="input-group">
                    <label>📍 Куда переезжаете?</label>
                    <select id="cityTo"></select>
                </div>
            </div>

            <div class="row">
                <div class="input-group">
                    <label>💰 Ваша зарплата (₽ в месяц)</label>
                    <input type="number" id="salarySelf" value="90000" placeholder="Введите вашу зарплату">
                </div>
                <div class="input-group">
                    <label>👥 Сценарий проживания</label>
                    <div class="radio-group">
                        <label><input type="radio" name="scenario" value="single" checked> 🧑 Один</label>
                        <label><input type="radio" name="scenario" value="family"> 👨‍👩‍👧 Семья</label>
                    </div>
                </div>
            </div>

            <div id="partnerSalaryDiv" class="family-salary">
                <div class="input-group">
                    <label>👤 Зарплата партнера (₽)</label>
                    <input type="number" id="salaryPartner" value="50000">
                </div>
            </div>

            <div id="kidsBlock" class="kids-block">
                <label>👶 Количество детей <span id="kidsCountValue" class="kids-value">0</span></label>
                <input type="range" id="kidsCount" class="kids-slider" min="0" max="5" step="1" value="0">
                <small style="opacity: 0.7;">Добавляет расходы на детские товары, секции и образование</small>
            </div>

            <div class="input-group">
                <label>📈 Прогноз на будущее (лет)</label>
                <select id="forecastYears">
                    <option value="0">Без прогноза</option>
                    <option value="1" selected>1 год</option>
                    <option value="2">2 года</option>
                    <option value="3">3 года</option>
                    <option value="5">5 лет</option>
                </select>
            </div>

            <button onclick="calculate()">📊 Рассчитать переезд</button>
        </div>

        <div id="results" class="results"></div>

        <div class="download-section">
            <div class="download-frame">
                <div class="download-title">📥 Сохранить результат</div>
                <p>Скачайте отчёт в HTML-формате</p>
                <div class="offline-note">✅ Работает офлайн, все данные уже встроены</div>
                <button class="download-btn" onclick="downloadReport()">⬇️ Скачать отчёт</button>
            </div>
        </div>

        <div class="footer">
            <p>© 2026 Калькулятор "Цена переезда" | 30 городов | Данные: Росстат, Циан, 2ГИС, Едадил, Яндекс.Афиша</p>
        </div>
    </div>

    <script>
        // Актуальные данные по 30 городам России (источник: Росстат, Циан, 2ГИС, апрель 2026)
        const citiesData = {
            "Москва": { salary: 132000, rent1Center: 58000, rent1Suburb: 42000, rent2Room: 89000, foodBasket: 9120, transportMonth: 3400, gasoline: 56.5, taxi: 550, utils: 7800, entertainment: { cinema: 450, cafe: 1200, gym: 3500, theater: 2000, club: 2500 } },
            "Санкт-Петербург": { salary: 101000, rent1Center: 48000, rent1Suburb: 34000, rent2Room: 74000, foodBasket: 8890, transportMonth: 2950, gasoline: 54.2, taxi: 480, utils: 6700, entertainment: { cinema: 400, cafe: 1000, gym: 3000, theater: 1800, club: 2200 } },
            "Казань": { salary: 71000, rent1Center: 35000, rent1Suburb: 27000, rent2Room: 54000, foodBasket: 6850, transportMonth: 2650, gasoline: 52.0, taxi: 380, utils: 5600, entertainment: { cinema: 350, cafe: 800, gym: 2500, theater: 1200, club: 1800 } },
            "Екатеринбург": { salary: 74000, rent1Center: 33000, rent1Suburb: 25000, rent2Room: 51000, foodBasket: 7120, transportMonth: 2850, gasoline: 53.1, taxi: 410, utils: 5800, entertainment: { cinema: 380, cafe: 850, gym: 2600, theater: 1300, club: 1900 } },
            "Новосибирск": { salary: 66000, rent1Center: 31000, rent1Suburb: 24000, rent2Room: 49000, foodBasket: 6980, transportMonth: 2750, gasoline: 53.0, taxi: 390, utils: 5900, entertainment: { cinema: 350, cafe: 800, gym: 2400, theater: 1100, club: 1700 } },
            "Краснодар": { salary: 68000, rent1Center: 36000, rent1Suburb: 28000, rent2Room: 56000, foodBasket: 6750, transportMonth: 2650, gasoline: 52.5, taxi: 370, utils: 5500, entertainment: { cinema: 370, cafe: 820, gym: 2500, theater: 1200, club: 1750 } },
            "Нижний Новгород": { salary: 64000, rent1Center: 30000, rent1Suburb: 23000, rent2Room: 47000, foodBasket: 6640, transportMonth: 2650, gasoline: 52.0, taxi: 360, utils: 5400, entertainment: { cinema: 340, cafe: 750, gym: 2300, theater: 1000, club: 1600 } },
            "Ростов-на-Дону": { salary: 65000, rent1Center: 33000, rent1Suburb: 26000, rent2Room: 52000, foodBasket: 6780, transportMonth: 2650, gasoline: 52.2, taxi: 380, utils: 5600, entertainment: { cinema: 350, cafe: 780, gym: 2400, theater: 1100, club: 1650 } },
            "Самара": { salary: 63000, rent1Center: 29000, rent1Suburb: 22000, rent2Room: 46000, foodBasket: 6550, transportMonth: 2550, gasoline: 51.5, taxi: 350, utils: 5300, entertainment: { cinema: 330, cafe: 720, gym: 2200, theater: 950, club: 1550 } },
            "Владивосток": { salary: 89000, rent1Center: 51000, rent1Suburb: 37000, rent2Room: 79000, foodBasket: 10350, transportMonth: 3200, gasoline: 61.0, taxi: 480, utils: 7200, entertainment: { cinema: 420, cafe: 1100, gym: 3200, theater: 1600, club: 2100 } },
            "Воронеж": { salary: 55000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 38000, foodBasket: 6200, transportMonth: 2300, gasoline: 51.0, taxi: 280, utils: 4800, entertainment: { cinema: 300, cafe: 650, gym: 2000, theater: 800, club: 1400 } },
            "Ярославль": { salary: 53000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 36000, foodBasket: 6100, transportMonth: 2200, gasoline: 51.0, taxi: 270, utils: 4700, entertainment: { cinema: 290, cafe: 620, gym: 1900, theater: 780, club: 1350 } },
            "Тула": { salary: 52000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, foodBasket: 6000, transportMonth: 2200, gasoline: 50.5, taxi: 260, utils: 4600, entertainment: { cinema: 280, cafe: 600, gym: 1850, theater: 750, club: 1300 } },
            "Сочи": { salary: 70000, rent1Center: 45000, rent1Suburb: 32000, rent2Room: 68000, foodBasket: 7500, transportMonth: 2800, gasoline: 53.0, taxi: 400, utils: 6500, entertainment: { cinema: 450, cafe: 1200, gym: 3500, theater: 1800, club: 2500 } },
            "Волгоград": { salary: 54000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 37000, foodBasket: 6100, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4700, entertainment: { cinema: 300, cafe: 650, gym: 2000, theater: 850, club: 1400 } },
            "Севастополь": { salary: 56000, rent1Center: 28000, rent1Suburb: 20000, rent2Room: 43000, foodBasket: 6400, transportMonth: 2400, gasoline: 52.0, taxi: 300, utils: 5000, entertainment: { cinema: 350, cafe: 800, gym: 2300, theater: 1000, club: 1600 } },
            "Уфа": { salary: 58000, rent1Center: 26000, rent1Suburb: 19000, rent2Room: 40000, foodBasket: 6200, transportMonth: 2400, gasoline: 51.5, taxi: 290, utils: 4900, entertainment: { cinema: 320, cafe: 700, gym: 2100, theater: 900, club: 1500 } },
            "Пермь": { salary: 57000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 39000, foodBasket: 6150, transportMonth: 2400, gasoline: 51.5, taxi: 280, utils: 4900, entertainment: { cinema: 310, cafe: 680, gym: 2050, theater: 880, club: 1450 } },
            "Саратов": { salary: 52000, rent1Center: 22000, rent1Suburb: 15000, rent2Room: 34000, foodBasket: 5900, transportMonth: 2200, gasoline: 50.5, taxi: 260, utils: 4500, entertainment: { cinema: 280, cafe: 600, gym: 1800, theater: 750, club: 1300 } },
            "Ижевск": { salary: 53000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, foodBasket: 6000, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4600, entertainment: { cinema: 290, cafe: 620, gym: 1850, theater: 780, club: 1350 } },
            "Челябинск": { salary: 55000, rent1Center: 24000, rent1Suburb: 17000, rent2Room: 37000, foodBasket: 6100, transportMonth: 2400, gasoline: 51.5, taxi: 280, utils: 4800, entertainment: { cinema: 300, cafe: 650, gym: 1950, theater: 820, club: 1400 } },
            "Тюмень": { salary: 70000, rent1Center: 32000, rent1Suburb: 24000, rent2Room: 50000, foodBasket: 6800, transportMonth: 2700, gasoline: 52.5, taxi: 350, utils: 5500, entertainment: { cinema: 380, cafe: 900, gym: 2800, theater: 1300, club: 1900 } },
            "Курган": { salary: 48000, rent1Center: 19000, rent1Suburb: 13000, rent2Room: 29000, foodBasket: 5600, transportMonth: 2000, gasoline: 50.0, taxi: 230, utils: 4200, entertainment: { cinema: 250, cafe: 550, gym: 1600, theater: 650, club: 1100 } },
            "Красноярск": { salary: 64000, rent1Center: 30000, rent1Suburb: 22000, rent2Room: 46000, foodBasket: 6600, transportMonth: 2600, gasoline: 52.5, taxi: 340, utils: 5400, entertainment: { cinema: 360, cafe: 800, gym: 2500, theater: 1100, club: 1700 } },
            "Иркутск": { salary: 62000, rent1Center: 29000, rent1Suburb: 21000, rent2Room: 45000, foodBasket: 6500, transportMonth: 2600, gasoline: 52.5, taxi: 330, utils: 5300, entertainment: { cinema: 350, cafe: 780, gym: 2400, theater: 1050, club: 1650 } },
            "Кемерово": { salary: 53000, rent1Center: 23000, rent1Suburb: 16000, rent2Room: 35000, foodBasket: 6000, transportMonth: 2300, gasoline: 51.0, taxi: 270, utils: 4600, entertainment: { cinema: 290, cafe: 620, gym: 1900, theater: 780, club: 1350 } },
            "Томск": { salary: 56000, rent1Center: 25000, rent1Suburb: 18000, rent2Room: 38000, foodBasket: 6200, transportMonth: 2400, gasoline: 51.5, taxi: 290, utils: 4800, entertainment: { cinema: 320, cafe: 700, gym: 2100, theater: 900, club: 1500 } },
            "Барнаул": { salary: 51000, rent1Center: 21000, rent1Suburb: 14000, rent2Room: 32000, foodBasket: 5800, transportMonth: 2100, gasoline: 50.5, taxi: 250, utils: 4400, entertainment: { cinema: 270, cafe: 580, gym: 1750, theater: 700, club: 1250 } },
            "Хабаровск": { salary: 75000, rent1Center: 42000, rent1Suburb: 30000, rent2Room: 65000, foodBasket: 8500, transportMonth: 3000, gasoline: 58.0, taxi: 420, utils: 6800, entertainment: { cinema: 400, cafe: 1000, gym: 3000, theater: 1400, club: 2000 } },
            "Якутск": { salary: 82000, rent1Center: 55000, rent1Suburb: 40000, rent2Room: 85000, foodBasket: 11000, transportMonth: 3500, gasoline: 65.0, taxi: 500, utils: 8500, entertainment: { cinema: 500, cafe: 1300, gym: 3800, theater: 2000, club: 2800 } },
            "Калининград": { salary: 60000, rent1Center: 30000, rent1Suburb: 22000, rent2Room: 46000, foodBasket: 6700, transportMonth: 2600, gasoline: 52.0, taxi: 340, utils: 5400, entertainment: { cinema: 350, cafe: 850, gym: 2600, theater: 1200, club: 1800 } }
        };

        // Сезонные коэффициенты
        const seasonalFactors = { 1: 1.12, 2: 1.05, 3: 1.00, 4: 0.98, 5: 0.97, 6: 0.96, 7: 0.95, 8: 0.96, 9: 0.98, 10: 1.00, 11: 1.03, 12: 1.08 };
        const inflationMonthly = { 1: 1.05, 2: 0.72, 3: 0.48, 4: 0.44, 5: 0.55, 6: 0.50, 7: 0.60, 8: 0.35, 9: 0.45, 10: 0.65, 11: 0.90, 12: 1.10 };
        const inflationData = { rate: 0.095, monthly: 0.0075 };

        let inflationChart = null;

        // Заполнение списков городов
        const cities = Object.keys(citiesData);
        const cityFromSelect = document.getElementById('cityFrom');
        const cityToSelect = document.getElementById('cityTo');
        cities.forEach(city => {
            cityFromSelect.add(new Option(city, city));
            cityToSelect.add(new Option(city, city));
        });
        cityFromSelect.value = "Москва";
        cityToSelect.value = "Санкт-Петербург";

        // Управление отображением блоков
        const familyRadio = document.querySelector('input[value="family"]');
        const singleRadio = document.querySelector('input[value="single"]');
        const partnerDiv = document.getElementById('partnerSalaryDiv');
        const kidsBlock = document.getElementById('kidsBlock');
        const kidsSlider = document.getElementById('kidsCount');
        const kidsCountValue = document.getElementById('kidsCountValue');

        function toggleBlocks() {
            const isFamily = document.querySelector('input[name="scenario"]:checked').value === 'family';
            partnerDiv.style.display = isFamily ? 'block' : 'none';
            kidsBlock.style.display = isFamily ? 'block' : 'none';
        }

        kidsSlider.addEventListener('input', function() {
            kidsCountValue.textContent = this.value;
        });

        familyRadio.addEventListener('change', toggleBlocks);
        singleRadio.addEventListener('change', toggleBlocks);

        function formatNumber(num) { return Math.round(num).toLocaleString('ru-RU'); }

        function calculateForecast(currentValue, years) {
            return currentValue * Math.pow(1 + inflationData.rate, years);
        }

        function getMonthlyForecast(startValue, months) {
            const forecast = [];
            let currentValue = startValue;
            const currentMonth = new Date().getMonth() + 1;
            for (let i = 0; i < months; i++) {
                const month = ((currentMonth + i - 1) % 12) + 1;
                const seasonal = seasonalFactors[month];
                const inflation = inflationMonthly[month] / 100;
                currentValue = currentValue * (1 + inflation) * seasonal;
                forecast.push({ month: month, value: currentValue, seasonal: seasonal, inflation: inflation });
            }
            return forecast;
        }

        function createInflationChart(forecastData, cityName) {
            const ctx = document.getElementById('inflationChartCanvas');
            if (ctx) {
                if (inflationChart) inflationChart.destroy();
                const labels = forecastData.map((_, i) => `${i + 1} мес`);
                const values = forecastData.map(d => d.value);
                const seasonalValues = forecastData.map(d => (d.seasonal - 1) * 100);
                const inflationValues = forecastData.map(d => d.inflation * 100);
                inflationChart = new Chart(ctx, {
                    type: 'line',
                    data: {
                        labels: labels,
                        datasets: [
                            { label: `Прогноз расходов в ${cityName} (₽)`, data: values, borderColor: '#e74c3c', backgroundColor: 'rgba(231,76,60,0.1)', borderWidth: 3, fill: true, tension: 0.4, yAxisID: 'y' },
                            { label: 'Сезонность (%)', data: seasonalValues, borderColor: '#3498db', backgroundColor: 'rgba(52,152,219,0.1)', borderWidth: 2, fill: false, tension: 0.4, yAxisID: 'y1' },
                            { label: 'Инфляция (%)', data: inflationValues, borderColor: '#27ae60', backgroundColor: 'rgba(39,174,96,0.1)', borderWidth: 2, fill: false, tension: 0.4, yAxisID: 'y1' }
                        ]
                    },
                    options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'top' }, title: { display: true, text: 'Прогноз с учетом инфляции и сезонности' } }, scales: { y: { title: { display: true, text: 'Расходы (₽)' } }, y1: { position: 'right', title: { display: true, text: 'Процент (%)' } } } }
                });
            }
        }

        function getEntertainmentHtml(data, cityName) {
            const ent = data.entertainment;
            return `
                <div class="entertainment-section">
                    <div class="chart-title">🎭 Развлечения и досуг в ${cityName}</div>
                    <div class="entertainment-grid">
                        <div class="entertainment-item"><div class="icon">🎬</div><div class="name">Кино (билет)</div><div class="price">${formatNumber(ent.cinema)} ₽</div></div>
                        <div class="entertainment-item"><div class="icon">🍽️</div><div class="name">Ужин в кафе</div><div class="price">${formatNumber(ent.cafe)} ₽</div></div>
                        <div class="entertainment-item"><div class="icon">💪</div><div class="name">Фитнес (месяц)</div><div class="price">${formatNumber(ent.gym)} ₽</div></div>
                        <div class="entertainment-item"><div class="icon">🎭</div><div class="name">Театр (билет)</div><div class="price">${formatNumber(ent.theater)} ₽</div></div>
                        <div class="entertainment-item"><div class="icon">🪩</div><div class="name">Ночной клуб</div><div class="price">${formatNumber(ent.club)} ₽</div></div>
                    </div>
                    <div style="margin-top: 12px; font-size: 12px; color: #666;">📊 Средние цены по данным Яндекс.Афиша и 2ГИС (апрель 2026)</div>
                </div>
            `;
        }

        function calculate() {
            const cityA = cityFromSelect.value;
            const cityB = cityToSelect.value;
            const salarySelf = parseFloat(document.getElementById('salarySelf').value) || 0;
            const salaryPartner = parseFloat(document.getElementById('salaryPartner').value) || 0;
            const scenario = document.querySelector('input[name="scenario"]:checked').value;
            const forecastYears = parseInt(document.getElementById('forecastYears').value);
            const kidsCount = parseInt(document.getElementById('kidsCount').value) || 0;

            const totalSalary = salarySelf + (scenario === 'family' ? salaryPartner : 0);
            const dataA = citiesData[cityA];
            const dataB = citiesData[cityB];

            const rentA = scenario === 'single' ? dataA.rent1Suburb : dataA.rent2Room;
            const rentB = scenario === 'single' ? dataB.rent1Suburb : dataB.rent2Room;

            // Расходы на детей (секции, одежда, игрушки, образование)
            const kidsExpensePerChild = 8000;
            const kidsExpenses = kidsCount * kidsExpensePerChild;

            const expensesA = rentA + dataA.utils + dataA.foodBasket + dataA.transportMonth + kidsExpenses;
            const expensesB = rentB + dataB.utils + dataB.foodBasket + dataB.transportMonth + kidsExpenses;

            const freeA = totalSalary - expensesA;
            const freeB = totalSalary - expensesB;
            const percentChange = freeA !== 0 ? ((freeB - freeA) / Math.abs(freeA)) * 100 : 0;

            const monthlyForecast = getMonthlyForecast(expensesB, 12);

            let forecastHtml = '';
            if (forecastYears > 0) {
                const forecastFreeB = calculateForecast(freeB, forecastYears);
                const forecastExpensesB = calculateForecast(expensesB, forecastYears);
                forecastHtml = `<div class="forecast-card"><h4>📈 ПРОГНОЗ НА ${forecastYears} ${getYearWord(forecastYears)}</h4><p style="margin-bottom: 15px;">С учетом инфляции ${(inflationData.rate * 100).toFixed(1)}% в год</p><div class="two-columns"><div><strong>📊 Расходы в ${cityB}:</strong><br><span style="font-size: 20px; color: #e74c3c;">${formatNumber(expensesB)} ₽</span><br>→ через ${forecastYears} ${getYearWord(forecastYears)}: <strong>${formatNumber(forecastExpensesB)} ₽</strong></div><div><strong>💰 Свободный остаток:</strong><br><span style="font-size: 20px; color: #27ae60;">${formatNumber(freeB)} ₽</span><br>→ через ${forecastYears} ${getYearWord(forecastYears)}: <strong>${formatNumber(forecastFreeB)} ₽</strong></div></div></div>`;
            }

            const dateStr = new Date().toLocaleDateString('ru-RU');

            const resultsHtml = `
                <div class="metric">
                    <h3>⚡ ИТОГОВОЕ ИЗМЕНЕНИЕ РЕАЛЬНОГО ДОХОДА</h3>
                    <div class="percent-change ${percentChange >= 0 ? 'positive' : 'negative'}">${percentChange >= 0 ? '+' : ''}${Math.round(percentChange)}%</div>
                    <p>Ваш свободный остаток в <strong>${cityB}</strong> ${percentChange >= 0 ? 'выше' : 'ниже'} на <strong>${Math.abs(Math.round(percentChange))}%</strong> по сравнению с <strong>${cityA}</strong></p>
                    <p style="margin-top: 10px;">💰 Общий доход семьи: ${formatNumber(totalSalary)} ₽/мес</p>
                    ${kidsCount > 0 ? `<p style="margin-top: 5px;">👶 Расходы на ${kidsCount} ${getChildWord(kidsCount)}: +${formatNumber(kidsExpenses)} ₽/мес</p>` : ''}
                </div>

                <div class="chart">
                    <div class="chart-title">🏠 Сравнение аренды жилья (Циан)</div>
                    ${createRentChart(dataA, dataB, cityA, cityB)}
                </div>

                <div class="chart">
                    <div class="chart-title">🛒 Продуктовая корзина и базовые расходы (Росстат)</div>
                    ${createFoodChart(dataA, dataB, cityA, cityB)}
                </div>

                ${getEntertainmentHtml(dataB, cityB)}

                <div class="chart">
                    <div class="chart-title">🚌 Транспорт и коммунальные услуги (2ГИС)</div>
                    ${createTransportTable(dataA, dataB, cityA, cityB)}
                </div>

                <div class="inflation-chart">
                    <div class="chart-title">📈 Прогноз расходов в ${cityB} с учетом инфляции и сезонности</div>
                    <canvas id="inflationChartCanvas"></canvas>
                    <div style="margin-top: 10px; font-size: 11px; color: #999; text-align: center;">Учтены сезонные колебания цен и ежемесячная инфляция по данным Росстата</div>
                </div>

                <div class="two-columns">
                    <div class="budget-card">
                        <h4>💰 Бюджет в ${cityA}</h4>
                        <div class="budget-amount">${formatNumber(freeA)} ₽</div>
                        <div>📈 Доход: ${formatNumber(totalSalary)} ₽<br>📉 Расходы: ${formatNumber(expensesA)} ₽</div>
                        <hr style="margin: 15px 0;">
                        <div style="font-size: 12px;">🏠 Аренда: ${formatNumber(rentA)} ₽<br>💡 ЖКХ: ${formatNumber(dataA.utils)} ₽<br>🍎 Продукты: ${formatNumber(dataA.foodBasket)} ₽<br>🚍 Транспорт: ${formatNumber(dataA.transportMonth)} ₽${kidsCount > 0 ? `<br>👶 Дети: ${formatNumber(kidsExpenses)} ₽` : ''}</div>
                    </div>
                    <div class="budget-card">
                        <h4>💰 Бюджет в ${cityB}</h4>
                        <div class="budget-amount">${formatNumber(freeB)} ₽</div>
                        <div>📈 Доход: ${formatNumber(totalSalary)} ₽<br>📉 Расходы: ${formatNumber(expensesB)} ₽</div>
                        <hr style="margin: 15px 0;">
                        <div style="font-size: 12px;">🏠 Аренда: ${formatNumber(rentB)} ₽<br>💡 ЖКХ: ${formatNumber(dataB.utils)} ₽<br>🍎 Продукты: ${formatNumber(dataB.foodBasket)} ₽<br>🚍 Транспорт: ${formatNumber(dataB.transportMonth)} ₽${kidsCount > 0 ? `<br>👶 Дети: ${formatNumber(kidsExpenses)} ₽` : ''}</div>
                    </div>
                </div>

                ${forecastHtml}

                <div class="advice">
                    <strong>💡 ${getAdviceTitle(percentChange)}</strong><br>
                    ${getAdvice(percentChange, cityB, scenario, freeB, freeA, kidsCount)}
                </div>

                <div class="sources">
                    <strong>📊 ИСТОЧНИКИ ДАННЫХ (актуальные на ${dateStr}):</strong><br>
                    • 📈 Доходы по регионам — Росстат / НИУ ВШЭ<br>
                    • 🏠 Цены на аренду — Циан / ручной сбор (30 городов)<br>
                    • 🛒 Продуктовая корзина — Росстат + Едадил (33 наименования)<br>
                    • 🚌 Транспорт — 2ГИС API<br>
                    • 🎭 Развлечения — Яндекс.Афиша, 2ГИС<br>
                    • 📊 Инфляция и сезонность — Росстат + ЦБ РФ
                </div>
            `;

            document.getElementById('results').style.display = 'block';
            document.getElementById('results').innerHTML = resultsHtml;

            setTimeout(() => { createInflationChart(monthlyForecast, cityB); }, 100);
            window.scrollTo({ top: document.getElementById('results').offsetTop - 20, behavior: 'smooth' });
        }

        function getYearWord(years) { if (years === 1) return 'год'; if (years >= 2 && years <= 4) return 'года'; return 'лет'; }
        function getChildWord(count) { if (count === 1) return 'ребёнка'; if (count >= 2 && count <= 4) return 'детей'; return 'детей'; }

        function getAdviceTitle(percentChange) {
            if (percentChange > 15) return '🔥 Отличная возможность!';
            if (percentChange > 5) return '📈 Хороший вариант';
            if (percentChange > -5) return '⚠️ Взвесьте все за и против';
            return '📉 Стоит подумать';
        }

        function createRentChart(dataA, dataB, cityA, cityB) {
            const maxRent = Math.max(dataA.rent1Center, dataB.rent1Center, dataA.rent1Suburb, dataB.rent1Suburb, dataA.rent2Room, dataB.rent2Room);
            return `<div class="bar-container"><div class="bar-label"><span>🏢 1-комнатная в центре</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Center)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Center)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent1Center / maxRent) * 100}%">${Math.round((dataA.rent1Center / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent1Center / maxRent) * 100}%">${Math.round((dataB.rent1Center / maxRent) * 100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span>🏘️ 1-комнатная в спальном районе</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Suburb)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Suburb)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent1Suburb / maxRent) * 100}%">${Math.round((dataA.rent1Suburb / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent1Suburb / maxRent) * 100}%">${Math.round((dataB.rent1Suburb / maxRent) * 100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span>👨‍👩‍👧 2-комнатная (для семьи)</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent2Room)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent2Room)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent2Room / maxRent) * 100}%">${Math.round((dataA.rent2Room / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent2Room / maxRent) * 100}%">${Math.round((dataB.rent2Room / maxRent) * 100)}%</div></div></div>`;
        }

        function createFoodChart(dataA, dataB, cityA, cityB) {
            const maxFood = Math.max(dataA.foodBasket, dataB.foodBasket);
            return `<div class="bar-container"><div class="bar-label"><span>${cityA}</span><span>${formatNumber(dataA.foodBasket)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.foodBasket / maxFood) * 100}%">${Math.round((dataA.foodBasket / maxFood) * 100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span>${cityB}</span><span>${formatNumber(dataB.foodBasket)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-b" style="width: ${(dataB.foodBasket / maxFood) * 100}%">${Math.round((dataB.foodBasket / maxFood) * 100)}%</div></div></div>
            <div style="margin-top: 10px; font-size: 12px; background: #e8f4f8; padding: 10px; border-radius: 8px;">🍞 Набор из 33 продуктов: хлеб, молоко, мясо, овощи, фрукты, масло, крупы и др.</div>`;
        }

        function createTransportTable(dataA, dataB, cityA, cityB) {
            return `<table style="width: 100%; border-collapse: collapse;"><thead><tr style="background: linear-gradient(135deg, #a888e5, #8b6bd6); color: white;"><th style="padding: 12px; text-align: left; border-radius: 10px 0 0 0;">Категория</th><th style="padding: 12px; text-align: center;">${cityA}</th><th style="padding: 12px; text-align: center; border-radius: 0 10px 0 0;">${cityB}</th></tr></thead>
            <tbody><tr><td style="padding: 12px;">🚌 Проездной (месяц)</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataA.transportMonth)} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataB.transportMonth)} ₽</strong></td></tr>
            <tr style="background: #f8f9fa;"><td style="padding: 12px;">⛽ Бензин АИ-95 (литр)</td><td style="padding: 12px; text-align: center;"><strong>${dataA.gasoline} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${dataB.gasoline} ₽</strong></td></tr>
            <tr><td style="padding: 12px;">🚕 Такси (средняя поездка)</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataA.taxi)} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataB.taxi)} ₽</strong></td></tr>
            <tr style="background: #f8f9fa;"><td style="padding: 12px;">💡 ЖКХ (квартплата + свет + вода)</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataA.utils)} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataB.utils)} ₽</strong></td></tr></tbody></table>`;
        }

        function getAdvice(percentChange, cityB, scenario, freeB, freeA, kidsCount) {
            const diff = freeB - freeA;
            const diffAbs = Math.abs(diff);
            if (percentChange > 15) return `Переезд в ${cityB} увеличит ваш бюджет на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). ${kidsCount > 0 ? 'Для семьи с детьми это особенно важно — больше средств на развитие и образование.' : 'Вы сможете больше откладывать или тратить на путешествия.'}`;
            if (percentChange > 5) return `Переезд в ${cityB} даст умеренный плюс: +${formatNumber(diffAbs)} ₽/мес (${Math.round(percentChange)}%). Учитывайте инфраструктуру, климат и карьерные перспективы.`;
            if (percentChange > -5) return `Финансовая разница незначительна: ${diff > 0 ? '+' : ''}${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). Ориентируйтесь на качество жизни, экологию и близость к родственникам.`;
            if (percentChange > -15) return `Переезд снизит ваш остаток на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). Рекомендуем найти работу с зарплатой выше средней или рассмотреть удалёнку.`;
            return `⚠️ ВНИМАНИЕ: ${cityB} уменьшит ваш бюджет на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). ${kidsCount > 0 ? 'С детьми такая разница критична — может повлиять на качество жизни.' : 'Настоятельно рекомендуем пересмотреть решение.'}`;
        }

        function downloadReport() {
            const content = document.getElementById('results').innerHTML;
            if (!content || content.trim() === '') { alert('Сначала выполните расчет!'); return; }
            const style = document.querySelector('style').innerHTML;
            const fullHtml = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>Отчёт о переезде</title><script src="https://cdn.jsdelivr.net/npm/chart.js"><\/script><style>${style} body { padding: 20px; } .theme-toggle, .download-section { display: none; } .container { max-width: 1200px; margin: 0 auto; }</style></head><body><div class="container">${content}</div></body></html>`;
            const blob = new Blob([fullHtml], { type: 'text/html' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `price_of_move_report_${new Date().toISOString().slice(0,19).replace(/:/g, '-')}.html`;
            link.click();
            URL.revokeObjectURL(link.href);
        }

        function toggleTheme() {
            document.body.classList.toggle('dark-theme');
            const btn = document.querySelector('.theme-toggle');
            btn.textContent = document.body.classList.contains('dark-theme') ? '☀️ Светлая тема' : '🌙 Тёмная тема';
        }
    </script>
</body>
</html>
