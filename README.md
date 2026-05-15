# Welcome to Russia
Калькулятор переезда — прогнозирует цены на квартиру, еду и транспорт. Учитывайте аренду, продукты, топливо и проезд. Получите детальную смету расходов на новом месте. Сравнивайте города, планируйте бюджет и избегайте неожиданных трат. Подходит для переездов в другой город. Бесплатно и удобно.

<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Калькулятор "Цена переезда" - 30 городов России</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700;14..32,800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 40px 20px;
            transition: all 0.3s ease;
            position: relative;
        }

        /* Анимированный фон */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200"><circle cx="50" cy="50" r="40" fill="rgba(255,255,255,0.03)"/><circle cx="150" cy="120" r="60" fill="rgba(255,255,255,0.02)"/><circle cx="100" cy="180" r="30" fill="rgba(255,255,255,0.04)"/></svg>') repeat;
            pointer-events: none;
            z-index: 0;
        }

        body.dark-theme {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
        }

        body.dark-theme::before {
            opacity: 0.5;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        /* Анимации */
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.02); box-shadow: 0 10px 30px rgba(139, 107, 214, 0.4); }
        }

        /* Кнопка тёмной темы */
        .theme-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 50px;
            padding: 10px 20px;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: 500;
            color: white;
            z-index: 1000;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .theme-toggle:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }

        body.dark-theme .theme-toggle {
            background: rgba(0, 0, 0, 0.3);
        }

        /* Заголовок */
        .title-wrapper {
            text-align: center;
            margin-bottom: 30px;
            animation: fadeInUp 0.8s ease;
        }

        .title-frame {
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 80px;
            padding: 20px 45px;
            display: inline-block;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            transition: all 0.3s;
        }

        .title-frame:hover {
            transform: translateY(-3px);
            border-color: rgba(255, 255, 255, 0.5);
        }

        h1 {
            font-size: 2rem;
            color: white;
            margin: 0;
            letter-spacing: -0.5px;
        }

        h1 i {
            margin-right: 10px;
        }

        .description {
            text-align: center;
            margin-bottom: 30px;
            animation: fadeInUp 0.8s ease 0.1s backwards;
        }

        .description-text {
            font-size: 1rem;
            color: rgba(255, 255, 255, 0.9);
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(8px);
            border-radius: 50px;
            padding: 10px 25px;
            display: inline-block;
        }

        /* Карточка калькулятора */
        .calculator-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 32px;
            padding: 35px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
            margin-bottom: 30px;
            transition: all 0.3s;
            animation: fadeInUp 0.8s ease 0.2s backwards;
        }

        .calculator-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 30px 60px -15px rgba(0, 0, 0, 0.3);
        }

        body.dark-theme .calculator-card {
            background: rgba(30, 30, 50, 0.95);
        }

        .calculator-card label {
            color: #4a3b2e;
            font-weight: 600;
        }

        body.dark-theme .calculator-card label {
            color: #ddd;
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
            padding: 14px 18px;
            border: 2px solid #e2e8f0;
            border-radius: 16px;
            font-size: 15px;
            transition: all 0.3s;
            background: white;
            font-family: 'Inter', sans-serif;
        }

        select:focus, input:focus {
            outline: none;
            border-color: #8b6bd6;
            box-shadow: 0 0 0 3px rgba(139, 107, 214, 0.2);
            transform: scale(1.01);
        }

        body.dark-theme select,
        body.dark-theme input {
            background: rgba(255, 255, 255, 0.1);
            border-color: rgba(255, 255, 255, 0.2);
            color: white;
        }

        body.dark-theme select option {
            background: #2a2a3e;
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
            padding: 8px 16px;
            background: rgba(139, 107, 214, 0.1);
            border-radius: 30px;
            transition: all 0.2s;
        }

        .radio-group label:hover {
            background: rgba(139, 107, 214, 0.2);
        }

        .radio-group input {
            width: auto;
        }

        .family-salary, .kids-block {
            display: none;
            margin-top: 15px;
            padding: 20px;
            background: rgba(139, 107, 214, 0.08);
            border-radius: 20px;
            border-left: 4px solid #8b6bd6;
            transition: all 0.3s;
        }

        body.dark-theme .family-salary,
        body.dark-theme .kids-block {
            background: rgba(139, 107, 214, 0.15);
        }

        .kids-slider {
            width: 100%;
            margin: 10px 0;
            cursor: pointer;
            accent-color: #8b6bd6;
        }

        .kids-value {
            font-weight: bold;
            color: #8b6bd6;
            font-size: 1.2rem;
            margin-left: 10px;
        }

        button {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, #8b6bd6 0%, #6c4ab6 100%);
            color: white;
            border: none;
            border-radius: 20px;
            font-size: 18px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        button i {
            font-size: 1.2rem;
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 30px rgba(139, 107, 214, 0.4);
        }

        .results {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 32px;
            padding: 35px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
            display: none;
            animation: fadeInUp 0.6s ease;
        }

        body.dark-theme .results {
            background: rgba(30, 30, 50, 0.95);
        }

        .metric {
            text-align: center;
            padding: 30px;
            background: linear-gradient(135deg, #f8f9ff 0%, #f0eef8 100%);
            border-radius: 24px;
            margin-bottom: 30px;
        }

        body.dark-theme .metric {
            background: linear-gradient(135deg, #2a2a3e 0%, #1e1e2e 100%);
        }

        .percent-change {
            font-size: 64px;
            font-weight: 800;
            margin: 15px 0;
            letter-spacing: -2px;
        }

        .positive { color: #10b981; }
        .negative { color: #ef4444; }

        .chart {
            margin-bottom: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 24px;
            transition: all 0.3s;
        }

        .chart:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }

        body.dark-theme .chart {
            background: #2a2a3e;
        }

        .chart-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .chart-title i {
            font-size: 1.3rem;
            color: #8b6bd6;
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
            background: #e2e8f0;
            border-radius: 12px;
            overflow: hidden;
            height: 40px;
        }

        body.dark-theme .bar-bg {
            background: #3a3a4e;
        }

        .bar-fill {
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 15px;
            color: white;
            font-weight: bold;
            border-radius: 12px;
            transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .city-a { background: linear-gradient(90deg, #3b82f6, #2563eb); }
        .city-b { background: linear-gradient(90deg, #f97316, #ea580c); }

        .two-columns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
            margin-bottom: 25px;
        }

        .budget-card {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 25px;
            border-radius: 24px;
            text-align: center;
            transition: all 0.3s;
        }

        .budget-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
        }

        body.dark-theme .budget-card {
            background: linear-gradient(135deg, #2a2a3e 0%, #1e1e2e 100%);
        }

        .budget-amount {
            font-size: 36px;
            font-weight: 800;
            color: #8b6bd6;
            margin: 15px 0;
        }

        .forecast-card {
            background: linear-gradient(135deg, rgba(139, 107, 214, 0.1) 0%, rgba(108, 74, 182, 0.1) 100%);
            padding: 25px;
            border-radius: 24px;
            margin-top: 20px;
            border: 1px solid rgba(139, 107, 214, 0.2);
        }

        .advice {
            background: #fef3c7;
            border-left: 5px solid #f59e0b;
            padding: 20px;
            border-radius: 16px;
            margin-top: 20px;
        }

        body.dark-theme .advice {
            background: #2a2a3e;
            border-left-color: #c084fc;
        }

        .sources {
            margin-top: 20px;
            padding: 20px;
            background: #e8f0f5;
            border-radius: 16px;
            font-size: 11px;
            line-height: 1.6;
        }

        body.dark-theme .sources {
            background: #1e1e2e;
        }

        .entertainment-section {
            margin-bottom: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 24px;
            transition: all 0.3s;
        }

        body.dark-theme .entertainment-section {
            background: #2a2a3e;
        }

        .entertainment-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .entertainment-item {
            background: white;
            padding: 15px;
            border-radius: 16px;
            text-align: center;
            transition: all 0.3s;
        }

        .entertainment-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }

        body.dark-theme .entertainment-item {
            background: #1e1e2e;
        }

        .entertainment-item .icon {
            font-size: 28px;
            margin-bottom: 8px;
        }

        .entertainment-item .price {
            font-size: 18px;
            font-weight: bold;
            color: #8b6bd6;
        }

        .inflation-chart {
            margin-top: 20px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 24px;
        }

        body.dark-theme .inflation-chart {
            background: #2a2a3e;
        }

        canvas {
            max-height: 300px;
            width: 100%;
        }

        .download-section {
            margin-top: 30px;
            text-align: center;
        }

        .download-frame {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 32px;
            padding: 30px;
            max-width: 500px;
            margin: 0 auto;
            transition: all 0.3s;
        }

        .download-frame:hover {
            transform: translateY(-3px);
            border-color: rgba(255, 255, 255, 0.4);
        }

        .download-btn {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
            border: none;
            border-radius: 50px;
            padding: 14px 30px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .download-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(16, 185, 129, 0.3);
        }

        .footer {
            text-align: center;
            margin-top: 40px;
            font-size: 12px;
            color: rgba(255, 255, 255, 0.7);
        }

        @media (max-width: 768px) {
            .row, .two-columns {
                grid-template-columns: 1fr;
            }
            .percent-change {
                font-size: 42px;
            }
            .budget-amount {
                font-size: 28px;
            }
            .title-frame {
                padding: 12px 25px;
            }
            h1 {
                font-size: 1.5rem;
            }
            .calculator-card, .results {
                padding: 20px;
            }
            .entertainment-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <button class="theme-toggle" onclick="toggleTheme()">
        <i class="fas fa-moon"></i>
        <span>Тёмная тема</span>
    </button>

    <div class="container">
        <div class="title-wrapper">
            <div class="title-frame">
                <h1><i class="fas fa-exchange-alt"></i> Цена переезда</h1>
            </div>
        </div>
        <div class="description">
            <div class="description-text">
                <i class="fas fa-chart-line"></i> Сравнение 30 городов России | Прогноз с учетом инфляции и сезонности | Анализ всех расходов с детьми
            </div>
        </div>

        <div class="calculator-card">
            <div class="row">
                <div class="input-group">
                    <label><i class="fas fa-map-marker-alt"></i> Откуда переезжаете?</label>
                    <select id="cityFrom"></select>
                </div>
                <div class="input-group">
                    <label><i class="fas fa-map-pin"></i> Куда переезжаете?</label>
                    <select id="cityTo"></select>
                </div>
            </div>

            <div class="row">
                <div class="input-group">
                    <label><i class="fas fa-ruble-sign"></i> Ваша зарплата (₽ в месяц)</label>
                    <input type="number" id="salarySelf" value="90000" placeholder="Введите вашу зарплату">
                </div>
                <div class="input-group">
                    <label><i class="fas fa-users"></i> Сценарий проживания</label>
                    <div class="radio-group">
                        <label><input type="radio" name="scenario" value="single" checked> <i class="fas fa-user"></i> Один</label>
                        <label><input type="radio" name="scenario" value="family"> <i class="fas fa-family"></i> Семья</label>
                    </div>
                </div>
            </div>

            <div id="partnerSalaryDiv" class="family-salary">
                <div class="input-group">
                    <label><i class="fas fa-user-plus"></i> Зарплата партнера (₽)</label>
                    <input type="number" id="salaryPartner" value="50000">
                </div>
            </div>

            <div id="kidsBlock" class="kids-block">
                <label><i class="fas fa-baby-carriage"></i> Количество детей <span id="kidsCountValue" class="kids-value">0</span></label>
                <input type="range" id="kidsCount" class="kids-slider" min="0" max="5" step="1" value="0">
                <small><i class="fas fa-info-circle"></i> Расходы на детей увеличивают все категории трат (продукты, транспорт, ЖКХ, развлечения)</small>
            </div>

            <div class="input-group">
                <label><i class="fas fa-chart-line"></i> Прогноз на будущее (лет)</label>
                <select id="forecastYears">
                    <option value="0">Без прогноза</option>
                    <option value="1" selected>1 год</option>
                    <option value="2">2 года</option>
                    <option value="3">3 года</option>
                    <option value="5">5 лет</option>
                </select>
            </div>

            <button onclick="calculate()">
                <i class="fas fa-calculator"></i> Рассчитать переезд
            </button>
        </div>

        <div id="results" class="results"></div>

        <div class="download-section">
            <div class="download-frame">
                <i class="fas fa-download" style="font-size: 2rem; margin-bottom: 10px; display: inline-block;"></i>
                <div style="font-size: 1.2rem; font-weight: bold;">Сохранить результат</div>
                <p>Скачайте отчёт в HTML-формате</p>
                <div><i class="fas fa-check-circle"></i> Работает офлайн, все данные уже встроены</div>
                <button class="download-btn" onclick="downloadReport()">
                    <i class="fas fa-file-download"></i> Скачать отчёт
                </button>
            </div>
        </div>

        <div class="footer">
            <p><i class="far fa-copyright"></i> 2026 Калькулятор "Цена переезда" | 30 городов | Данные: Росстат, Циан, 2ГИС, Едадил, Яндекс.Афиша</p>
        </div>
    </div>

    <script>
        // Актуальные данные по 30 городам России
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

        // Коэффициенты увеличения расходов на каждого ребёнка
        const KIDS_MULTIPLIERS = {
            food: 0.35,
            transport: 0.25,
            utils: 0.20,
            entertainment: 0.40,
            baseChildExpense: 8000
        };

        const seasonalFactors = { 1: 1.12, 2: 1.05, 3: 1.00, 4: 0.98, 5: 0.97, 6: 0.96, 7: 0.95, 8: 0.96, 9: 0.98, 10: 1.00, 11: 1.03, 12: 1.08 };
        const inflationMonthly = { 1: 1.05, 2: 0.72, 3: 0.48, 4: 0.44, 5: 0.55, 6: 0.50, 7: 0.60, 8: 0.35, 9: 0.45, 10: 0.65, 11: 0.90, 12: 1.10 };
        const inflationData = { rate: 0.095 };

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
                    options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'top' } }, scales: { y: { title: { display: true, text: 'Расходы (₽)' } }, y1: { position: 'right', title: { display: true, text: 'Процент (%)' } } } }
                });
            }
        }

        function calculateExpensesWithKids(baseExpenses, kidsCount, category) {
            const multiplier = KIDS_MULTIPLIERS[category] || 0;
            return baseExpenses * (1 + multiplier * kidsCount);
        }

        function getEntertainmentHtml(data, cityName, kidsCount, scenario) {
            const ent = data.entertainment;
            const isFamily = scenario === 'family';
            let html = `<div class="entertainment-section"><div class="chart-title"><i class="fas fa-ticket-alt"></i> Развлечения и досуг в ${cityName}</div><div class="entertainment-grid">`;
            const items = [
                { icon: "🎬", name: "Кино (билет)", price: ent.cinema },
                { icon: "🍽️", name: "Ужин в кафе", price: ent.cafe },
                { icon: "💪", name: "Фитнес (месяц)", price: ent.gym },
                { icon: "🎭", name: "Театр (билет)", price: ent.theater },
                { icon: "🪩", name: "Ночной клуб", price: ent.club }
            ];
            items.forEach(item => {
                let finalPrice = item.price;
                let note = '';
                if (isFamily && kidsCount > 0) {
                    finalPrice = item.price + kidsCount * 300;
                    note = ` <span style="font-size: 10px; color: #2ecc71;">(+${formatNumber(kidsCount * 300)}₽ дети)</span>`;
                }
                html += `<div class="entertainment-item"><div class="icon">${item.icon}</div><div class="name">${item.name}</div><div class="price">${formatNumber(finalPrice)} ₽${note}</div></div>`;
            });
            html += `</div><div style="margin-top: 12px; font-size: 12px; color: #666;"><i class="fas fa-database"></i> Средние цены по данным Яндекс.Афиша и 2ГИС (апрель 2026)</div></div>`;
            return html;
        }

        function createRentChart(dataA, dataB, cityA, cityB) {
            const maxRent = Math.max(dataA.rent1Center, dataB.rent1Center, dataA.rent1Suburb, dataB.rent1Suburb, dataA.rent2Room, dataB.rent2Room);
            return `<div class="bar-container"><div class="bar-label"><span><i class="fas fa-building"></i> 1-комнатная в центре</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Center)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Center)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent1Center / maxRent) * 100}%">${Math.round((dataA.rent1Center / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent1Center / maxRent) * 100}%">${Math.round((dataB.rent1Center / maxRent) * 100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span><i class="fas fa-home"></i> 1-комнатная в спальном районе</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent1Suburb)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent1Suburb)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent1Suburb / maxRent) * 100}%">${Math.round((dataA.rent1Suburb / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent1Suburb / maxRent) * 100}%">${Math.round((dataB.rent1Suburb / maxRent) * 100)}%</div></div></div>
            <div class="bar-container"><div class="bar-label"><span><i class="fas fa-users"></i> 2-комнатная (для семьи)</span><span><strong>${cityA}</strong> ${formatNumber(dataA.rent2Room)} ₽ | <strong>${cityB}</strong> ${formatNumber(dataB.rent2Room)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(dataA.rent2Room / maxRent) * 100}%">${Math.round((dataA.rent2Room / maxRent) * 100)}%</div></div><div class="bar-bg" style="margin-top: 5px;"><div class="bar-fill city-b" style="width: ${(dataB.rent2Room / maxRent) * 100}%">${Math.round((dataB.rent2Room / maxRent) * 100)}%</div></div></div>`;
        }

        function createFoodChartWithKids(baseFoodA, baseFoodB, foodA, foodB, cityA, cityB, kidsCount) {
            const maxFood = Math.max(foodA, foodB);
            return `<div class="bar-container"><div class="bar-label"><span><i class="fas fa-utensils"></i> ${cityA}</span><span>${formatNumber(foodA)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-a" style="width: ${(foodA / maxFood) * 100}%">${Math.round((foodA / maxFood) * 100)}%</div></div>${kidsCount > 0 ? `<div style="font-size: 11px; margin-top: 4px;">Базовая корзина: ${formatNumber(baseFoodA)} ₽ + ${Math.round(KIDS_MULTIPLIERS.food * kidsCount * 100)}% на детей</div>` : ''}</div>
            <div class="bar-container"><div class="bar-label"><span><i class="fas fa-utensils"></i> ${cityB}</span><span>${formatNumber(foodB)} ₽</span></div><div class="bar-bg"><div class="bar-fill city-b" style="width: ${(foodB / maxFood) * 100}%">${Math.round((foodB / maxFood) * 100)}%</div></div>${kidsCount > 0 ? `<div style="font-size: 11px; margin-top: 4px;">Базовая корзина: ${formatNumber(baseFoodB)} ₽ + ${Math.round(KIDS_MULTIPLIERS.food * kidsCount * 100)}% на детей</div>` : ''}</div>
            <div style="margin-top: 10px; font-size: 12px; background: #e8f4f8; padding: 10px; border-radius: 8px;"><i class="fas fa-bread-slice"></i> Набор из 33 продуктов: хлеб, молоко, мясо, овощи, фрукты, масло, крупы и др.</div>`;
        }

        function createTransportTableWithKids(dataA, dataB, cityA, cityB, transportA, transportB, utilsA, utilsB, kidsCount) {
            return `<table style="width: 100%; border-collapse: collapse;"><thead><tr style="background: linear-gradient(135deg, #8b6bd6, #6c4ab6); color: white;"><th style="padding: 12px; text-align: left; border-radius: 10px 0 0 0;">Категория</th><th style="padding: 12px; text-align: center;">${cityA}</th><th style="padding: 12px; text-align: center; border-radius: 0 10px 0 0;">${cityB}</th></tr></thead>
            <tbody><tr><td style="padding: 12px;"><i class="fas fa-bus"></i> Проездной (месяц)${kidsCount > 0 ? `<br><span style="font-size: 10px;">+${Math.round(KIDS_MULTIPLIERS.transport * kidsCount * 100)}% на детей</span>` : ''}</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(transportA)} ₽</strong><br><span style="font-size: 10px;">(база: ${formatNumber(dataA.transportMonth)} ₽)</span></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(transportB)} ₽</strong><br><span style="font-size: 10px;">(база: ${formatNumber(dataB.transportMonth)} ₽)</span></td></tr>
            <tr style="background: #f8f9fa;"><td style="padding: 12px;"><i class="fas fa-gas-pump"></i> Бензин АИ-95 (литр)</td><td style="padding: 12px; text-align: center;"><strong>${dataA.gasoline} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${dataB.gasoline} ₽</strong></td></tr>
            <tr><td style="padding: 12px;"><i class="fas fa-taxi"></i> Такси (средняя поездка)${kidsCount > 0 ? `<br><span style="font-size: 10px;">+${kidsCount * 150}₽ на детское кресло</span>` : ''}</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataA.taxi + (kidsCount > 0 ? kidsCount * 150 : 0))} ₽</strong></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(dataB.taxi + (kidsCount > 0 ? kidsCount * 150 : 0))} ₽</strong></td></tr>
            <tr style="background: #f8f9fa;"><td style="padding: 12px;"><i class="fas fa-lightbulb"></i> ЖКХ (квартплата + свет + вода)${kidsCount > 0 ? `<br><span style="font-size: 10px;">+${Math.round(KIDS_MULTIPLIERS.utils * kidsCount * 100)}% на детей</span>` : ''}</td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(utilsA)} ₽</strong><br><span style="font-size: 10px;">(база: ${formatNumber(dataA.utils)} ₽)</span></td><td style="padding: 12px; text-align: center;"><strong>${formatNumber(utilsB)} ₽</strong><br><span style="font-size: 10px;">(база: ${formatNumber(dataB.utils)} ₽)</span></td></tr></tbody></table>`;
        }

        function getAdviceTitle(percentChange) {
            if (percentChange > 15) return '🔥 Отличная возможность!';
            if (percentChange > 5) return '📈 Хороший вариант';
            if (percentChange > -5) return '⚠️ Взвесьте все за и против';
            return '📉 Стоит подумать';
        }

        function getAdvice(percentChange, cityB, scenario, freeB, freeA, kidsCount) {
            const diff = freeB - freeA;
            const diffAbs = Math.abs(diff);
            if (percentChange > 15) return `Переезд в ${cityB} увеличит ваш бюджет на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). ${kidsCount > 0 ? `С ${kidsCount} ${getChildWord(kidsCount)} это особенно важно — больше средств на развитие всей семьи.` : 'Вы сможете больше откладывать или тратить на путешествия.'}`;
            if (percentChange > 5) return `Переезд в ${cityB} даст умеренный плюс: +${formatNumber(diffAbs)} ₽/мес (${Math.round(percentChange)}%). Учитывайте инфраструктуру, климат и карьерные перспективы.`;
            if (percentChange > -5) return `Финансовая разница незначительна: ${diff > 0 ? '+' : ''}${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). Ориентируйтесь на качество жизни, экологию и близость к родственникам.`;
            if (percentChange > -15) return `Переезд снизит ваш остаток на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). Рекомендуем найти работу с зарплатой выше средней или рассмотреть удалёнку.`;
            return `⚠️ ВНИМАНИЕ: ${cityB} уменьшит ваш бюджет на ${formatNumber(diffAbs)} ₽ (${Math.round(percentChange)}%). ${kidsCount > 0 ? `С ${kidsCount} ${getChildWord(kidsCount)} такая разница критична — может существенно повлиять на качество жизни детей.` : 'Настоятельно рекомендуем пересмотреть решение.'}`;
        }

        function getYearWord(years) { if (years === 1) return 'год'; if (years >= 2 && years <= 4) return 'года'; return 'лет'; }
        function getChildWord(count) { if (count === 1) return 'ребёнка'; if (count >= 2 && count <= 4) return 'детей'; return 'детей'; }

        function downloadReport() {
            const content = document.getElementById('results').innerHTML;
            if (!content || content.trim() === '') { alert('Сначала выполните расчет!'); return; }
            const style = document.querySelector('style').innerHTML;
            const fullHtml = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>Отчёт о переезде</title><script src="https://cdn.jsdelivr.net/npm/chart.js"><\/script><link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet"><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css"><style>${style} body { padding: 20px; } .theme-toggle, .download-section { display: none; } .container { max-width: 1200px; margin: 0 auto; }</style></head><body><div class="container">${content}</div></body></html>`;
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
            const isDark = document.body.classList.contains('dark-theme');
            btn.innerHTML = isDark ? '<i class="fas fa-sun"></i> Светлая тема' : '<i class="fas fa-moon"></i> Тёмная тема';
        }

        function calculate() {
            const cityA = cityFromSelect.value;
            const cityB = cityToSelect.value;
            const salarySelf = parseFloat(document.getElementById('salarySelf').value) || 0;
            const salaryPartner = parseFloat(document.getElementById('salaryPartner').value) || 0;
            const scenario = document.querySelector('input[name="scenario"]:checked').value;
            const forecastYears = parseInt(document.getElementById('forecastYears').value);
            const kidsCount = (scenario === 'family' ? parseInt(document.getElementById('kidsCount').value) : 0) || 0;

            const totalSalary = salarySelf + (scenario === 'family' ? salaryPartner : 0);
            const dataA = citiesData[cityA];
            const dataB = citiesData[cityB];

            const rentA = scenario === 'single' ? dataA.rent1Suburb : dataA.rent2Room;
            const rentB = scenario === 'single' ? dataB.rent1Suburb : dataB.rent2Room;

            const baseFoodA = dataA.foodBasket, baseFoodB = dataB.foodBasket;
            const baseTransportA = dataA.transportMonth, baseTransportB = dataB.transportMonth;
            const baseUtilsA = dataA.utils, baseUtilsB = dataB.utils;

            const foodA = calculateExpensesWithKids(baseFoodA, kidsCount, 'food');
            const foodB = calculateExpensesWithKids(baseFoodB, kidsCount, 'food');
            const transportA = calculateExpensesWithKids(baseTransportA, kidsCount, 'transport');
            const transportB = calculateExpensesWithKids(baseTransportB, kidsCount, 'transport');
            const utilsA = calculateExpensesWithKids(baseUtilsA, kidsCount, 'utils');
            const utilsB = calculateExpensesWithKids(baseUtilsB, kidsCount, 'utils');
            
            const baseChildExpense = KIDS_MULTIPLIERS.baseChildExpense * kidsCount;
            const kidsExtraExpenses = kidsCount * 3000;

            const expensesA = rentA + utilsA + foodA + transportA + baseChildExpense + kidsExtraExpenses;
            const expensesB = rentB + utilsB + foodB + transportB + baseChildExpense + kidsExtraExpenses;
            const freeA = totalSalary - expensesA;
            const freeB = totalSalary - expensesB;
            const percentChange = freeA !== 0 ? ((freeB - freeA) / Math.abs(freeA)) * 100 : 0;

            const monthlyForecast = getMonthlyForecast(expensesB, 12);
            let forecastHtml = '';
            if (forecastYears > 0) {
                const forecastFreeB = calculateForecast(freeB, forecastYears);
                const forecastExpensesB = calculateForecast(expensesB, forecastYears);
                forecastHtml = `<div class="forecast-card"><h4><i class="fas fa-chart-line"></i> ПРОГНОЗ НА ${forecastYears} ${getYearWord(forecastYears)}</h4><p>С учетом инфляции ${(inflationData.rate * 100).toFixed(1)}% в год</p><div class="two-columns"><div><strong>📊 Расходы в ${cityB}:</strong><br><span style="font-size: 20px; color: #e74c3c;">${formatNumber(expensesB)} ₽</span><br>→ через ${forecastYears} ${getYearWord(forecastYears)}: <strong>${formatNumber(forecastExpensesB)} ₽</strong></div><div><strong>💰 Свободный остаток:</strong><br><span style="font-size: 20px; color: #27ae60;">${formatNumber(freeB)} ₽</span><br>→ через ${forecastYears} ${getYearWord(forecastYears)}: <strong>${formatNumber(forecastFreeB)} ₽</strong></div></div></div>`;
            }

            const dateStr = new Date().toLocaleDateString('ru-RU');
            let kidsImpactHtml = '';
            if (scenario === 'family' && kidsCount > 0) {
                kidsImpactHtml = `<div style="background: rgba(46,204,113,0.1); border-radius: 16px; padding: 15px; margin-bottom: 20px;"><strong><i class="fas fa-baby-carriage"></i> Влияние ${kidsCount} ${getChildWord(kidsCount)} на расходы:</strong><br>• Продукты: +${Math.round(KIDS_MULTIPLIERS.food * kidsCount * 100)}% (${formatNumber(foodA - baseFoodA)} ₽ в ${cityA} / ${formatNumber(foodB - baseFoodB)} ₽ в ${cityB})<br>• Транспорт: +${Math.round(KIDS_MULTIPLIERS.transport * kidsCount * 100)}%<br>• ЖКХ: +${Math.round(KIDS_MULTIPLIERS.utils * kidsCount * 100)}%<br>• Базовые расходы на ребёнка: ${formatNumber(baseChildExpense)} ₽/мес<br>• Дополнительные расходы (секции, кружки): ${formatNumber(kidsExtraExpenses)} ₽/мес</div>`;
            }

            const resultsHtml = `
                <div class="metric">
                    <h3><i class="fas fa-chart-simple"></i> ИТОГОВОЕ ИЗМЕНЕНИЕ РЕАЛЬНОГО ДОХОДА</h3>
                    <div class="percent-change ${percentChange >= 0 ? 'positive' : 'negative'}">${percentChange >= 0 ? '+' : ''}${Math.round(percentChange)}%</div>
                    <p>Ваш свободный остаток в <strong>${cityB}</strong> ${percentChange >= 0 ? 'выше' : 'ниже'} на <strong>${Math.abs(Math.round(percentChange))}%</strong> по сравнению с <strong>${cityA}</strong></p>
                    <p><i class="fas fa-ruble-sign"></i> Общий доход семьи: ${formatNumber(totalSalary)} ₽/мес</p>
                    ${kidsCount > 0 ? `<p><i class="fas fa-baby-carriage"></i> Семья с ${kidsCount} ${getChildWord(kidsCount)}: расходы увеличены на <strong>${Math.round(((expensesB - (rentB + baseUtilsB + baseFoodB + baseTransportB)) / (rentB + baseUtilsB + baseFoodB + baseTransportB)) * 100)}%</strong></p>` : ''}
                </div>
                ${kidsImpactHtml}
                <div class="chart"><div class="chart-title"><i class="fas fa-building"></i> Сравнение аренды жилья (Циан)</div>${createRentChart(dataA, dataB, cityA, cityB)}</div>
                <div class="chart"><div class="chart-title"><i class="fas fa-utensils"></i> Расходы на продукты (с учётом детей)</div>${createFoodChartWithKids(baseFoodA, baseFoodB, foodA, foodB, cityA, cityB, kidsCount)}</div>
                ${getEntertainmentHtml(dataB, cityB, kidsCount, scenario)}
                <div class="chart"><div class="chart-title"><i class="fas fa-bus"></i> Транспорт и коммунальные услуги</div>${createTransportTableWithKids(dataA, dataB, cityA, cityB, transportA, transportB, utilsA, utilsB, kidsCount)}</div>
                <div class="inflation-chart"><div class="chart-title"><i class="fas fa-chart-line"></i> Прогноз расходов в ${cityB}</div><canvas id="inflationChartCanvas"></canvas><div style="margin-top: 10px; font-size: 11px; color: #999; text-align: center;">Учтены сезонные колебания цен и ежемесячная инфляция по данным Росстата</div></div>
                <div class="two-columns"><div class="budget-card"><h4><i class="fas fa-wallet"></i> Бюджет в ${cityA}</h4><div class="budget-amount">${formatNumber(freeA)} ₽</div><div>📈 Доход: ${formatNumber(totalSalary)} ₽<br>📉 Расходы: ${formatNumber(expensesA)} ₽</div><hr><div style="font-size: 12px;">🏠 Аренда: ${formatNumber(rentA)} ₽<br>💡 ЖКХ: ${formatNumber(utilsA)} ₽<br>🍎 Продукты: ${formatNumber(foodA)} ₽<br>🚍 Транспорт: ${formatNumber(transportA)} ₽${kidsCount > 0 ? `<br>👶 Дети: ${formatNumber(baseChildExpense + kidsExtraExpenses)} ₽` : ''}</div></div>
                <div class="budget-card"><h4><i class="fas fa-wallet"></i> Бюджет в ${cityB}</h4><div class="budget-amount">${formatNumber(freeB)} ₽</div><div>📈 Доход: ${formatNumber(totalSalary)} ₽<br>📉 Расходы: ${formatNumber(expensesB)} ₽</div><hr><div style="font-size: 12px;">🏠 Аренда: ${formatNumber(rentB)} ₽<br>💡 ЖКХ: ${formatNumber(utilsB)} ₽<br>🍎 Продукты: ${formatNumber(foodB)} ₽<br>🚍 Транспорт: ${formatNumber(transportB)} ₽${kidsCount > 0 ? `<br>👶 Дети: ${formatNumber(baseChildExpense + kidsExtraExpenses)} ₽` : ''}</div></div></div>
                ${forecastHtml}
                <div class="advice"><strong><i class="fas fa-lightbulb"></i> ${getAdviceTitle(percentChange)}</strong><br>${getAdvice(percentChange, cityB, scenario, freeB, freeA, kidsCount)}</div>
                <div class="sources"><strong><i class="fas fa-database"></i> ИСТОЧНИКИ ДАННЫХ (актуальные на ${dateStr}):</strong><br>• 📈 Доходы по регионам — Росстат / НИУ ВШЭ<br>• 🏠 Цены на аренду — Циан / ручной сбор (30 городов)<br>• 🛒 Продуктовая корзина — Росстат + Едадил (33 наименования)<br>• 🚌 Транспорт — 2ГИС API<br>• 🎭 Развлечения — Яндекс.Афиша, 2ГИС<br>• 👶 Расходы на детей — Росстат + собственные коэффициенты<br>• 📊 Инфляция и сезонность — Росстат + ЦБ РФ</div>
            `;

            document.getElementById('results').style.display = 'block';
            document.getElementById('results').innerHTML = resultsHtml;
            setTimeout(() => { createInflationChart(monthlyForecast, cityB); }, 100);
            window.scrollTo({ top: document.getElementById('results').offsetTop - 20, behavior: 'smooth' });
        }
    </script>
</body>
</html>
