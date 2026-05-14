# RelocationCalculator.github.io
Калькулятор переезда — прогнозирует цены на квартиру, еду и транспорт. Учитывайте аренду, продукты, топливо и проезд. Получите детальную смету расходов на новом месте. Сравнивайте города, планируйте бюджет и избегайте неожиданных трат. Подходит для переездов в другой город или страну. Бесплатно и удобно.
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Калькулятор переезда | Квартира, еда, транспорт</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #e0c3fc, #8ec5fc);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            color: #4a3b6e;
            margin-bottom: 10px;
            font-size: 2rem;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.05);
        }

        .subtitle {
            text-align: center;
            color: #5e4b7a;
            margin-bottom: 30px;
            font-size: 1rem;
        }

        .card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            margin-bottom: 20px;
            border: 1px solid rgba(255,255,255,0.6);
        }

        .card h2 {
            color: #4a3b6e;
            margin-bottom: 20px;
            border-left: 4px solid #a888e5;
            padding-left: 15px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #4a3b6e;
        }

        input, select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #d9caf7;
            border-radius: 16px;
            font-size: 16px;
            background: white;
            color: #333;
            transition: all 0.3s ease;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #a888e5;
            box-shadow: 0 0 0 3px rgba(168, 136, 229, 0.2);
        }

        button {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #a888e5, #8b6bd6);
            color: white;
            border: none;
            border-radius: 16px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(139, 107, 214, 0.3);
        }

        .result {
            background: rgba(255, 255, 255, 0.6);
            border-radius: 20px;
            padding: 20px;
            margin-top: 20px;
            text-align: center;
        }

        .result h3 {
            color: #4a3b6e;
            margin-bottom: 15px;
        }

        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid rgba(0,0,0,0.05);
            color: #4a3b6e;
        }

        .result-item:last-child {
            border-bottom: none;
        }

        .result-item .value {
            font-weight: bold;
            color: #7c5cc9;
        }

        .total {
            margin-top: 15px;
            padding-top: 15px;
            font-size: 1.3rem;
            font-weight: bold;
            color: #6b4fc0;
            text-align: right;
            border-top: 2px solid rgba(0,0,0,0.1);
        }

        footer {
            text-align: center;
            color: rgba(0,0,0,0.4);
            margin-top: 30px;
            font-size: 12px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🏠 Калькулятор переезда</h1>
        <div class="subtitle">Прогноз цен на квартиру, еду и транспорт</div>

        <div class="card">
            <h2>📊 Введите данные для расчёта</h2>

            <div class="input-group">
                <label>🏙️ Город / регион</label>
                <select id="city">
                    <option value="moscow">Москва</option>
                    <option value="spb">Санкт-Петербург</option>
                    <option value="novosibirsk">Новосибирск</option>
                    <option value="ekaterinburg">Екатеринбург</option>
                    <option value="other">Другой город</option>
                </select>
            </div>

            <div class="input-group">
                <label>🏢 Аренда квартиры (1-комн.)</label>
                <input type="number" id="rent" placeholder="Введите сумму в месяц, руб." value="40000">
            </div>

            <div class="input-group">
                <label>🍎 Расходы на еду (в месяц)</label>
                <input type="number" id="food" placeholder="Введите сумму в месяц, руб." value="15000">
            </div>

            <div class="input-group">
                <label>🚗 Расходы на транспорт (в месяц)</label>
                <input type="number" id="transport" placeholder="Введите сумму в месяц, руб." value="5000">
            </div>

            <button onclick="calculate()">🔮 Рассчитать прогноз</button>

            <div class="result" id="result">
                <h3>📋 Ваш прогноз расходов</h3>
                <div class="result-item">
                    <span>🏢 Квартира:</span>
                    <span class="value" id="rentResult">—</span>
                </div>
                <div class="result-item">
                    <span>🍎 Еда:</span>
                    <span class="value" id="foodResult">—</span>
                </div>
                <div class="result-item">
                    <span>🚗 Транспорт:</span>
                    <span class="value" id="transportResult">—</span>
                </div>
                <div class="total">
                    📌 Итого в месяц: <span id="totalResult">—</span> ₽
                </div>
            </div>
        </div>
        <footer>Данные являются прогнозом. Точные цифры зависят от района и привычек.</footer>
    </div>

    <script>
        function calculate() {
            let rent = parseFloat(document.getElementById('rent').value);
            let food = parseFloat(document.getElementById('food').value);
            let transport = parseFloat(document.getElementById('transport').value);
            let city = document.getElementById('city').value;

            if (city === 'moscow') {
                rent = rent || 45000;
                food = food || 18000;
                transport = transport || 6000;
            } else if (city === 'spb') {
                rent = rent || 35000;
                food = food || 15000;
                transport = transport || 5000;
            } else if (city === 'novosibirsk') {
                rent = rent || 25000;
                food = food || 12000;
                transport = transport || 4000;
            } else if (city === 'ekaterinburg') {
                rent = rent || 27000;
                food = food || 13000;
                transport = transport || 4200;
            }

            if (isNaN(rent)) rent = 0;
            if (isNaN(food)) food = 0;
            if (isNaN(transport)) transport = 0;

            let total = rent + food + transport;

            document.getElementById('rentResult').innerText = rent.toLocaleString() + ' ₽';
            document.getElementById('foodResult').innerText = food.toLocaleString() + ' ₽';
            document.getElementById('transportResult').innerText = transport.toLocaleString() + ' ₽';
            document.getElementById('totalResult').innerText = total.toLocaleString();
        }

        window.onload = calculate;
    </script>
</body>
</html>
