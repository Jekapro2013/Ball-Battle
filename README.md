<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Шарики-Бойцы: Полная версия</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: white;
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }
        
        .screen {
            display: none;
            max-width: 1200px;
            margin: 0 auto;
            text-align: center;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .active {
            display: block;
        }
        
        button {
            background: linear-gradient(45deg, #4CC9F0, #4361ee);
            color: white;
            border: none;
            padding: 12px 24px;
            margin: 8px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 15px;
            font-weight: bold;
            transition: all 0.3s;
            box-shadow: 0 4px 12px rgba(76, 201, 240, 0.3);
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(76, 201, 240, 0.4);
        }
        
        .container {
            display: flex;
            gap: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .left-panel, .right-panel {
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid #4CC9F0;
            min-width: 250px;
            max-width: 300px;
        }
        
        .center-panel {
            flex: 1;
            min-width: 300px;
            max-width: 800px;
        }
        
        input {
            padding: 12px;
            margin: 8px;
            border-radius: 8px;
            border: 2px solid #4CC9F0;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            width: 90%;
            font-size: 15px;
        }
        
        #gameCanvas {
            background: linear-gradient(135deg, #0a1931, #1a1a2e);
            border-radius: 15px;
            border: 4px solid #4CC9F0;
            margin: 10px auto;
            display: block;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.5);
        }
        
        .stats {
            background: rgba(0, 0, 0, 0.8);
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #4CC9F0;
            margin-bottom: 15px;
            text-align: left;
        }
        
        .weapon-info {
            background: rgba(0, 0, 0, 0.8);
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #ff6b6b;
            margin-bottom: 15px;
        }
        
        .leaderboard {
            background: rgba(0, 0, 0, 0.8);
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #4CAF50;
            max-height: 300px;
            overflow-y: auto;
            margin-bottom: 15px;
        }
        
        .leaderboard table {
            width: 100%;
            border-collapse: collapse;
            font-size: 14px;
        }
        
        .leaderboard th {
            background: rgba(76, 175, 80, 0.3);
            padding: 8px;
            text-align: left;
            border-bottom: 2px solid #4CAF50;
        }
        
        .leaderboard td {
            padding: 6px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .leaderboard tr:hover {
            background: rgba(255, 255, 255, 0.05);
        }
        
        .health-bar {
            width: 80px;
            height: 8px;
            background: rgba(255, 0, 0, 0.3);
            border-radius: 4px;
            margin: 4px 0;
            overflow: hidden;
            display: inline-block;
            vertical-align: middle;
        }
        
        .health-fill {
            height: 100%;
            background: linear-gradient(90deg, #00ff00, #00cc00);
            width: 100%;
            transition: width 0.3s;
        }
        
        h1 {
            color: #4CC9F0;
            margin-bottom: 20px;
            font-size: 2.2em;
            background: linear-gradient(45deg, #4CC9F0, #4361ee);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        h2 {
            color: #4CC9F0;
            margin-bottom: 15px;
            font-size: 1.5em;
        }
        
        h3 {
            color: #4CC9F0;
            margin-bottom: 10px;
            font-size: 1.2em;
        }
        
        .error {
            color: #ff6b6b;
            background: rgba(255, 107, 107, 0.1);
            padding: 10px;
            border-radius: 8px;
            margin: 10px auto;
            max-width: 400px;
            display: none;
        }
        
        .settings-section {
            background: rgba(0, 0, 0, 0.6);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 15px;
            text-align: left;
        }
        
        .setting-item {
            margin: 10px 0;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .setting-item label {
            cursor: pointer;
        }
        
        .checkbox {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        
        .slider {
            width: 100%;
            margin: 10px 0;
        }
        
        .info-box {
            background: rgba(0, 0, 0, 0.6);
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
            border: 2px solid #FFD700;
            text-align: left;
        }
        
        .info-box h3 {
            color: #FFD700;
        }
        
        .creator-badge {
            display: inline-block;
            background: linear-gradient(45deg, #FFD700, #FFA500);
            color: #000;
            padding: 3px 8px;
            border-radius: 5px;
            font-weight: bold;
            margin-left: 5px;
            font-size: 12px;
        }
        
        .crown-effect {
            position: absolute;
            pointer-events: none;
            z-index: 100;
            animation: crownFloat 2s infinite ease-in-out;
        }
        
        @keyframes crownFloat {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-10px) rotate(5deg); }
        }
        
        .game-message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.9);
            padding: 20px 40px;
            border-radius: 15px;
            border: 3px solid #4CC9F0;
            font-size: 24px;
            display: none;
            z-index: 1000;
            box-shadow: 0 0 30px rgba(76, 201, 240, 0.5);
        }
        
        .performance-warning {
            color: #ff9800;
            font-size: 12px;
            margin-top: 5px;
        }
        
        .weapon-selector {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 10px 0;
        }
        
        .weapon-option {
            background: rgba(255, 255, 255, 0.1);
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid transparent;
        }
        
        .weapon-option:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .weapon-option.selected {
            border-color: #4CC9F0;
            background: rgba(76, 201, 240, 0.2);
        }
        
        .tab-container {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 2px solid #4CC9F0;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background: rgba(76, 201, 240, 0.2);
            margin-right: 5px;
            border-radius: 8px 8px 0 0;
            transition: all 0.3s;
        }
        
        .tab.active {
            background: #4CC9F0;
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
            animation: fadeIn 0.3s ease;
        }
        
        .tab-content.active {
            display: block;
        }
    </style>
</head>
<body>
    <!-- Экран меню -->
    <div id="menuScreen" class="screen active">
        <h1>⚔️ ШАРИКИ-БОЙЦЫ ⚔️</h1>
        <p style="margin-bottom: 20px; color: #4CC9F50;">Все против всех! Только оружие наносит урон!</p>
        
        <div id="errorMessage" class="error"></div>
        
        <div class="container">
            <div class="left-panel">
                <h3>👤 ИГРОК</h3>
                <input type="text" id="playerName" placeholder="Ваше имя" maxlength="15" value="Игрок">
                
                <div class="settings-section">
                    <h3>🎮 РЕЖИМ ИГРЫ</h3>
                    <button onclick="startGame()" style="background: linear-gradient(45deg, #FF416C, #FF4B2B); width: 100%;">
                        ⚔️ НАЧАТЬ ИГРУ
                    </button>
                    <button onclick="showTab('settings')" style="width: 100%; margin-top: 10px;">
                        ⚙️ НАСТРОЙКИ
                    </button>
                </div>
                
                <div class="info-box">
                    <h3>📊 СТАТИСТИКА</h3>
                    <p>💰 Монеты: <span id="totalCoinsDisplay" style="color: gold;">0</span></p>
                    <p>🎯 Убийств: <span id="totalKillsDisplay" style="color: #ff6b6b;">0</span></p>
                    <p>🏆 Рекорд: <span id="recordDisplay" style="color: #4CC9F0;">0</span></p>
                    <p>👑 Побед: <span id="winsDisplay" style="color: #FFD700;">0</span></p>
                </div>
            </div>
            
            <div class="center-panel">
                <div class="tab-container">
                    <div class="tab active" onclick="showTab('main')">ГЛАВНАЯ</div>
                    <div class="tab" onclick="showTab('settings')">НАСТРОЙКИ</div>
                    <div class="tab" onclick="showTab('info')">ИНФОРМАЦИЯ</div>
                    <div class="tab" onclick="showTab('leaderboard')">ЛИДЕРЫ</div>
                </div>
                
                <div id="tabMain" class="tab-content active">
                    <div class="settings-section">
                        <h3>🤖 НАСТРОЙКА БОТОВ</h3>
                        <div class="setting-item">
                            <label>Количество ботов:</label>
                            <span id="botCountDisplay">6</span>
                        </div>
                        <input type="range" id="botCountSlider" class="slider" min="1" max="12" value="6" step="1">
                        <p class="performance-warning">Больше ботов = меньше производительность</p>
                        
                        <div class="setting-item">
                            <label>Сложность ботов:</label>
                            <select id="botDifficulty" style="padding: 5px; border-radius: 5px; background: rgba(255,255,255,0.1); color: white; border: 1px solid #4CC9F0;">
                                <option value="easy">Легкая</option>
                                <option value="medium" selected>Средняя</option>
                                <option value="hard">Сложная</option>
                                <option value="insane">Безумная</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="settings-section">
                        <h3>🔫 ВЫБОР ОРУЖИЯ</h3>
                        <div class="weapon-selector">
                            <div class="weapon-option selected" data-weapon="knife" onclick="toggleWeapon('knife')">
                                <div style="font-size: 24px;">🔪</div>
                                <div>Нож</div>
                                <div style="color: #ff6b6b; font-size: 12px;">10 урона</div>
                            </div>
                            <div class="weapon-option selected" data-weapon="pistol" onclick="toggleWeapon('pistol')">
                                <div style="font-size: 24px;">🔫</div>
                                <div>Пистолет</div>
                                <div style="color: #ff6b6b; font-size: 12px;">15 урона</div>
                            </div>
                            <div class="weapon-option selected" data-weapon="grenade" onclick="toggleWeapon('grenade')">
                                <div style="font-size: 24px;">💣</div>
                                <div>Граната</div>
                                <div style="color: #ff6b6b; font-size: 12px;">25 урона</div>
                            </div>
                        </div>
                        <p style="font-size: 12px; margin-top: 10px;">Кликните по оружию, чтобы включить/выключить его появление</p>
                    </div>
                    
                    <div class="settings-section">
                        <h3>⚡ НАСТРОЙКА ПРОИЗВОДИТЕЛЬНОСТИ</h3>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="effectsEnabled" class="checkbox" checked>
                                Эффекты частиц
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="trailsEnabled" class="checkbox" checked>
                                Следы за шариками
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="floatingTextEnabled" class="checkbox" checked>
                                Летающий текст
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="screenShakeEnabled" class="checkbox" checked>
                                Тряска экрана
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="crownEffectEnabled" class="checkbox" checked>
                                Короны создателя
                            </label>
                        </div>
                        <p class="performance-warning">Отключите эффекты для лучшей производительности</p>
                    </div>
                </div>
                
                <div id="tabSettings" class="tab-content">
                    <div class="settings-section">
                        <h3>🎮 УПРАВЛЕНИЕ</h3>
                        <p>• Шарики летают автоматически</p>
                        <p>• Собирайте оружие для нанесения урона</p>
                        <p>• Без оружия урон НЕ наносится</p>
                        <p>• Боты атакуют всех подряд</p>
                        <p>• Оружие одноразовое</p>
                        <p>• Отскок от стенок включен</p>
                    </div>
                    
                    <div class="settings-section">
                        <h3>🔧 ДОПОЛНИТЕЛЬНО</h3>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="autoCollectEnabled" class="checkbox" checked>
                                Автоподбор оружия
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="powerUpsEnabled" class="checkbox" checked>
                                Баффы на арене
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="botFriendlyFire" class="checkbox" checked>
                                Боты атакуют друг друга
                            </label>
                        </div>
                        <div class="setting-item">
                            <label>
                                <input type="checkbox" id="wallBounceEnabled" class="checkbox" checked>
                                Отскок от стенок
                            </label>
                        </div>
                    </div>
                    
                    <button onclick="resetSettings()" style="background: linear-gradient(45deg, #ff6b6b, #ee5a52);">
                        🔄 Сбросить настройки
                    </button>
                </div>
                
                <div id="tabInfo" class="tab-content">
                    <div class="info-box">
                        <h3>🎮 ОБ ИГРЕ</h3>
                        <p><strong>Шарики-Бойцы: Все против всех!</strong></p>
                        <p>Игра создана в 2026 году</p>
                        <p>Автор: <span style="color: #FFD700; font-weight: bold;">Jekapro2013</span></p>
                        
                        <div style="margin-top: 15px; padding: 10px; background: rgba(255, 215, 0, 0.1); border-radius: 8px;">
                            <p style="color: #FFD700;">✨ Особенность создателя:</p>
                            <p>Если имя шарика "Jekapro2013", от него будут отлетать короны!</p>
                            <p>Это работает даже если эффекты отключены в настройках.</p>
                            <p>В таблице лидеров создатель отмечен <span class="creator-badge">СОЗДАТЕЛЬ</span></p>
                        </div>
                    </div>
                    
                    <div class="settings-section">
                        <h3>📱 УПРАВЛЕНИЕ</h3>
                        <p><strong>ESC</strong> - Пауза/Продолжить</p>
                        <p><strong>R</strong> - Перезапустить игру</p>
                        <p><strong>+/-</strong> - Добавить/убрать бота</p>
                        <p><strong>F</strong> - Полноэкранный режим</p>
                    </div>
                </div>
                
                <div id="tabLeaderboard" class="tab-content">
                    <div class="leaderboard">
                        <h3>🏆 ЛУЧШИЕ ИГРОКИ</h3>
                        <table id="globalLeaderboard">
                            <thead>
                                <tr>
                                    <th>#</th>
                                    <th>Игрок</th>
                                    <th>Убийства</th>
                                    <th>Монеты</th>
                                    <th>Победы</th>
                                </tr>
                            </thead>
                            <tbody>
                                <!-- Заполняется из localStorage -->
                            </tbody>
                        </table>
                    </div>
                    <button onclick="clearLeaderboard()" style="background: linear-gradient(45deg, #808080, #666);">
                        🗑️ Очистить таблицу
                    </button>
                </div>
            </div>
            
            <div class="right-panel">
                <div class="stats">
                    <h3>📈 ТЕКУЩАЯ СЕССИЯ</h3>
                    <p>Текущие убийства: <span id="sessionKills">0</span></p>
                    <p>Лучшее комбо: <span id="sessionCombo">0</span></p>
                    <p>Время игры: <span id="sessionTime">0:00</span></p>
                </div>
                
                <div class="leaderboard">
                    <h3>🎯 БЫСТРЫЕ РЕЗУЛЬТАТЫ</h3>
                    <div id="quickStats">
                        <!-- Заполняется во время игры -->
                    </div>
                </div>
                
                <button onclick="showTab('info')" style="width: 100%; margin-top: 10px;">
                    ℹ️ Помощь
                </button>
            </div>
        </div>
    </div>

    <!-- Экран игры -->
    <div id="gameScreen" class="screen">
        <div class="container">
            <div class="left-panel">
                <div class="stats">
                    <h3>👤 ИГРОК</h3>
                    <p id="playerNameDisplay"></p>
                    <p>❤️ Здоровье: <span id="healthDisplay">100</span></p>
                    <div class="health-bar">
                        <div id="healthBarFill" class="health-fill"></div>
                    </div>
                    <p>💰 Монеты: <span id="coinsDisplay" style="color: gold;">0</span></p>
                    <p>🎯 Убийств: <span id="killsDisplay" style="color: #4CC9F0;">0</span></p>
                    <p>🔥 Комбо: <span id="comboDisplay" style="color: #FF416C;">0</span></p>
                </div>
                
                <div class="weapon-info">
                    <h3>🔫 ОРУЖИЕ</h3>
                    <p>Тип: <span id="weaponDisplay" style="color: #ff6b6b;">Нет</span></p>
                    <p>Патроны: <span id="ammoDisplay">-</span></p>
                    <p>Урон: <span id="damageDisplay">-</span></p>
                </div>
                
                <div class="settings-section">
                    <h3>⚙️ БЫСТРЫЕ НАСТРОЙКИ</h3>
                    <div class="setting-item">
                        <label>Ботов: <span id="gameBotCount">6</span></label>
                        <div>
                            <button onclick="changeBotCount(-1)" style="padding: 5px 10px;">-</button>
                            <button onclick="changeBotCount(1)" style="padding: 5px 10px;">+</button>
                        </div>
                    </div>
                    <button onclick="togglePause()" id="pauseBtn" style="width: 100%;">
                        ⏸️ ПАУЗА
                    </button>
                </div>
            </div>
            
            <div class="center-panel">
                <canvas id="gameCanvas" width="800" height="600"></canvas>
                
                <div style="margin-top: 15px;">
                    <button onclick="togglePause()">⏸️ ПАУЗА</button>
                    <button onclick="exitToMenu()" style="background: linear-gradient(45deg, #ff6b6b, #ee5a52);">
                        🏠 В МЕНЮ
                    </button>
                    <button onclick="addBot()" style="background: linear-gradient(45deg, #06d6a0, #118ab2);">
                        🤖 +БОТ
                    </button>
                    <button onclick="removeBot()" style="background: linear-gradient(45deg, #808080, #666);">
                        🤖 -БОТ
                    </button>
                </div>
                
                <div id="gameMessage" class="game-message"></div>
            </div>
            
            <div class="right-panel">
                <div class="leaderboard">
                    <h3>🏆 ТАБЛИЦА ЛИДЕРОВ</h3>
                    <table id="gameLeaderboard">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>Имя</th>
                                <th>Урон</th>
                                <th>ХП</th>
                                <th>Убийства</th>
                            </tr>
                        </thead>
                        <tbody>
                            <!-- Заполняется во время игры -->
                        </tbody>
                    </table>
                </div>
                
                <div class="stats">
                    <h3>📊 ИНФОРМАЦИЯ</h3>
                    <p>Врагов: <span id="enemiesDisplay" style="color: #ff6b6b;">6</span></p>
                    <p>Оружия на карте: <span id="weaponsDisplay">0</span></p>
                    <p>Время: <span id="gameTimeDisplay">0:00</span></p>
                    <p>ФПС: <span id="fpsDisplay">60</span></p>
                </div>
                
                <button onclick="showControls()" style="width: 100%; margin-top: 10px;">
                    🎮 Управление
                </button>
            </div>
        </div>
    </div>

    <!-- Экран паузы -->
    <div id="pauseScreen" class="screen">
        <h2>⏸️ ИГРА НА ПАУЗЕ</h2>
        <div style="margin: 30px 0;">
            <button onclick="togglePause()" style="background: linear-gradient(45deg, #4CAF50, #2E7D32); padding: 15px 30px;">
                ▶️ ПРОДОЛЖИТЬ
            </button>
            <button onclick="exitToMenu()" style="padding: 15px 30px;">🏠 В МЕНЮ</button>
        </div>
        
        <div class="container" style="max-width: 600px; margin: 20px auto;">
            <div class="settings-section">
                <h3>⚡ НАСТРОЙКИ ПРОИЗВОДИТЕЛЬНОСТИ</h3>
                <div class="setting-item">
                    <label>
                        <input type="checkbox" id="pauseEffectsEnabled" class="checkbox" checked>
                        Эффекты частиц
                    </label>
                </div>
                <div class="setting-item">
                    <label>
                        <input type="checkbox" id="pauseTrailsEnabled" class="checkbox" checked>
                        Следы за шариками
                    </label>
                </div>
                <p class="performance-warning">Изменения вступят в силу после продолжения игры</p>
            </div>
        </div>
    </div>

    <script>
        // Глобальные переменные
        const game = {
            canvas: null,
            ctx: null,
            running: false,
            paused: false,
            player: null,
            enemies: [],
            weapons: [],
            effects: [],
            particles: [],
            trails: [],
            floatingTexts: [],
            crowns: [],
            coins: 0,
            kills: 0,
            totalKills: 0,
            wins: 0,
            record: 0,
            playerName: 'Игрок',
            gameTime: 0,
            combo: 0,
            comboTime: 0,
            lastComboTime: 0,
            screenShake: 0,
            fps: 60,
            lastFrameTime: 0,
            frameCount: 0,
            leaderboard: [],
            sessionStartTime: 0,
            botCount: 6,
            enabledWeapons: new Set(['knife', 'pistol', 'grenade']),
            settings: {
                effects: true,
                trails: true,
                floatingText: true,
                screenShake: true,
                crownEffect: true,
                autoCollect: true,
                powerUps: true,
                botFriendlyFire: true,
                wallBounce: true,
                botDifficulty: 'medium'
            }
        };

        // Типы оружия
        const WEAPON_TYPES = {
            KNIFE: { id: 'knife', name: '🔪 Нож', color: '#808080', damage: 10, ammo: 1, range: 60, speed: 1.0 },
            PISTOL: { id: 'pistol', name: '🔫 Пистолет', color: '#8B4513', damage: 15, ammo: 1, range: 120, speed: 0.8 },
            GRENADE: { id: 'grenade', name: '💣 Граната', color: '#228B22', damage: 25, ammo: 1, range: 150, speed: 0.6 }
        };

        // Цвета для шариков
        const BALL_COLORS = [
            '#4CC9F0', '#4361ee', '#3a0ca3', '#7209b7',
            '#ff6b6b', '#ff9e6d', '#ffd166', '#ef476f',
            '#06d6a0', '#118ab2', '#073b4c', '#ff9e00',
            '#9d4edd', '#c77dff', '#e0aaff', '#ff5d8f'
        ];

        // Инициализация при загрузке
        window.onload = function() {
            console.log('Игра загружается...');
            
            // Загружаем сохраненные данные
            loadGameData();
            loadSettings();
            loadLeaderboard();
            
            // Инициализируем canvas
            game.canvas = document.getElementById('gameCanvas');
            game.ctx = game.canvas.getContext('2d');
            
            // Настройка управления
            setupControls();
            
            // Обновляем отображение настроек
            updateSettingsDisplay();
            
            console.log('Игра готова');
        };

        // Загрузить данные игры
        function loadGameData() {
            const savedData = localStorage.getItem('ballFightersData');
            if (savedData) {
                try {
                    const data = JSON.parse(savedData);
                    game.coins = data.coins || 0;
                    game.totalKills = data.totalKills || 0;
                    game.record = data.record || 0;
                    game.wins = data.wins || 0;
                    updateMenuStats();
                } catch(e) {
                    console.log('Ошибка загрузки данных:', e);
                }
            }
        }

        // Загрузить настройки
        function loadSettings() {
            const savedSettings = localStorage.getItem('ballFightersSettings');
            if (savedSettings) {
                try {
                    const settings = JSON.parse(savedSettings);
                    Object.assign(game.settings, settings);
                    
                    // Загружаем включенное оружие
                    if (settings.enabledWeapons) {
                        game.enabledWeapons = new Set(settings.enabledWeapons);
                    }
                    
                    // Загружаем количество ботов
                    if (settings.botCount) {
                        game.botCount = settings.botCount;
                        document.getElementById('botCountSlider').value = game.botCount;
                        document.getElementById('botCountDisplay').textContent = game.botCount;
                    }
                    
                    // Загружаем сложность
                    if (settings.botDifficulty) {
                        document.getElementById('botDifficulty').value = settings.botDifficulty;
                        game.settings.botDifficulty = settings.botDifficulty;
                    }
                } catch(e) {
                    console.log('Ошибка загрузки настроек:', e);
                }
            }
        }

        // Сохранить настройки
        function saveSettings() {
            const settings = {
                ...game.settings,
                enabledWeapons: Array.from(game.enabledWeapons),
                botCount: game.botCount,
                botDifficulty: game.settings.botDifficulty
            };
            localStorage.setItem('ballFightersSettings', JSON.stringify(settings));
        }

        // Загрузить таблицу лидеров
        function loadLeaderboard() {
            const savedLeaderboard = localStorage.getItem('ballFightersLeaderboard');
            if (savedLeaderboard) {
                try {
                    game.leaderboard = JSON.parse(savedLeaderboard);
                    updateGlobalLeaderboard();
                } catch(e) {
                    console.log('Ошибка загрузки таблицы лидеров:', e);
                }
            }
        }

        // Сохранить таблицу лидеров
        function saveLeaderboard() {
            localStorage.setItem('ballFightersLeaderboard', JSON.stringify(game.leaderboard));
        }

        // Обновить глобальную таблицу лидеров
        function updateGlobalLeaderboard() {
            const tbody = document.querySelector('#globalLeaderboard tbody');
            tbody.innerHTML = '';
            
            // Сортируем по убийствам
            const sorted = [...game.leaderboard].sort((a, b) => b.kills - a.kills);
            
            sorted.slice(0, 10).forEach((player, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>
                        ${player.name}
                        ${player.name === 'Jekapro2013' ? '<span class="creator-badge">СОЗДАТЕЛЬ</span>' : ''}
                    </td>
                    <td>${player.kills}</td>
                    <td>${player.coins}</td>
                    <td>${player.wins || 0}</td>
                `;
                tbody.appendChild(row);
            });
        }

        // Очистить таблицу лидеров
        function clearLeaderboard() {
            if (confirm('Вы уверены, что хотите очистить таблицу лидеров?')) {
                game.leaderboard = [];
                saveLeaderboard();
                updateGlobalLeaderboard();
            }
        }

        // Обновить статистику в меню
        function updateMenuStats() {
            document.getElementById('totalCoinsDisplay').textContent = game.coins;
            document.getElementById('totalKillsDisplay').textContent = game.totalKills;
            document.getElementById('recordDisplay').textContent = game.record;
            document.getElementById('winsDisplay').textContent = game.wins;
        }

        // Показать вкладку
        function showTab(tabName) {
            // Убираем активный класс у всех вкладок
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            
            // Активируем выбранную вкладку
            document.querySelector(`[onclick="showTab('${tabName}')"]`).classList.add('active');
            document.getElementById(`tab${tabName.charAt(0).toUpperCase() + tabName.slice(1)}`).classList.add('active');
        }

        // Переключить оружие
        function toggleWeapon(weaponId) {
            const option = document.querySelector(`.weapon-option[data-weapon="${weaponId}"]`);
            
            if (game.enabledWeapons.has(weaponId)) {
                game.enabledWeapons.delete(weaponId);
                option.classList.remove('selected');
            } else {
                game.enabledWeapons.add(weaponId);
                option.classList.add('selected');
            }
            
            saveSettings();
        }

        // Обновить отображение настроек
        function updateSettingsDisplay() {
            // Оружие
            document.querySelectorAll('.weapon-option').forEach(option => {
                const weaponId = option.dataset.weapon;
                if (game.enabledWeapons.has(weaponId)) {
                    option.classList.add('selected');
                } else {
                    option.classList.remove('selected');
                }
            });
            
            // Количество ботов
            document.getElementById('botCountSlider').value = game.botCount;
            document.getElementById('botCountDisplay').textContent = game.botCount;
            
            // Чекбоксы
            document.getElementById('effectsEnabled').checked = game.settings.effects;
            document.getElementById('trailsEnabled').checked = game.settings.trails;
            document.getElementById('floatingTextEnabled').checked = game.settings.floatingText;
            document.getElementById('screenShakeEnabled').checked = game.settings.screenShake;
            document.getElementById('crownEffectEnabled').checked = game.settings.crownEffect;
            document.getElementById('autoCollectEnabled').checked = game.settings.autoCollect;
            document.getElementById('powerUpsEnabled').checked = game.settings.powerUps;
            document.getElementById('botFriendlyFire').checked = game.settings.botFriendlyFire;
            document.getElementById('wallBounceEnabled').checked = game.settings.wallBounce;
            
            // Сложность
            document.getElementById('botDifficulty').value = game.settings.botDifficulty;
        }

        // Сбросить настройки
        function resetSettings() {
            if (confirm('Сбросить все настройки к значениям по умолчанию?')) {
                game.settings = {
                    effects: true,
                    trails: true,
                    floatingText: true,
                    screenShake: true,
                    crownEffect: true,
                    autoCollect: true,
                    powerUps: true,
                    botFriendlyFire: true,
                    wallBounce: true,
                    botDifficulty: 'medium'
                };
                
                game.enabledWeapons = new Set(['knife', 'pistol', 'grenade']);
                game.botCount = 6;
                
                localStorage.removeItem('ballFightersSettings');
                updateSettingsDisplay();
            }
        }

        // Настройка управления
        function setupControls() {
            // Клавиша паузы
            window.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    togglePause();
                }
                
                // Добавление/удаление ботов
                if (e.key === '+' || e.key === '=') {
                    if (game.running && !game.paused) {
                        addBot();
                    }
                }
                
                if (e.key === '-' || e.key === '_') {
                    if (game.running && !game.paused) {
                        removeBot();
                    }
                }
                
                // Рестарт
                if (e.key === 'r' || e.key === 'R') {
                    if (game.running && !game.paused) {
                        if (confirm('Перезапустить игру?')) {
                            exitToMenu();
                            startGame();
                        }
                    }
                }
                
                // Полноэкранный режим
                if (e.key === 'f' || e.key === 'F') {
                    toggleFullscreen();
                }
            });
            
            // Слайдер количества ботов
            document.getElementById('botCountSlider').addEventListener('input', (e) => {
                game.botCount = parseInt(e.target.value);
                document.getElementById('botCountDisplay').textContent = game.botCount;
                saveSettings();
            });
            
            // Чекбоксы настроек
            document.getElementById('effectsEnabled').addEventListener('change', (e) => {
                game.settings.effects = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('trailsEnabled').addEventListener('change', (e) => {
                game.settings.trails = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('floatingTextEnabled').addEventListener('change', (e) => {
                game.settings.floatingText = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('screenShakeEnabled').addEventListener('change', (e) => {
                game.settings.screenShake = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('crownEffectEnabled').addEventListener('change', (e) => {
                game.settings.crownEffect = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('autoCollectEnabled').addEventListener('change', (e) => {
                game.settings.autoCollect = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('powerUpsEnabled').addEventListener('change', (e) => {
                game.settings.powerUps = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('botFriendlyFire').addEventListener('change', (e) => {
                game.settings.botFriendlyFire = e.target.checked;
                saveSettings();
            });
            
            document.getElementById('wallBounceEnabled').addEventListener('change', (e) => {
                game.settings.wallBounce = e.target.checked;
                saveSettings();
            });
            
            // Сложность ботов
            document.getElementById('botDifficulty').addEventListener('change', (e) => {
                game.settings.botDifficulty = e.target.value;
                saveSettings();
            });
        }

        // Показать управление
        function showControls() {
            alert(`
🎮 УПРАВЛЕНИЕ В ИГРЕ:

• Шарики летают автоматически
• Собирайте оружие для атаки
• Без оружия урон не наносится
• Боты атакуют всех подряд

ГОРЯЧИЕ КЛАВИШИ:
ESC - Пауза/Продолжить
R - Перезапустить игру
+/- - Добавить/убрать бота
F - Полноэкранный режим
            `);
        }

        // Полноэкранный режим
        function toggleFullscreen() {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen().catch(err => {
                    console.log(`Ошибка включения полноэкранного режима: ${err.message}`);
                });
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }
            }
        }

        // Начать игру
        function startGame() {
            const nameInput = document.getElementById('playerName').value.trim();
            if (!nameInput) {
                showError('Введите имя игрока');
                return;
            }
            
            game.playerName = nameInput;
            
            // Загружаем актуальные настройки из чекбоксов паузы
            if (document.getElementById('pauseEffectsEnabled')) {
                game.settings.effects = document.getElementById('pauseEffectsEnabled').checked;
            }
            if (document.getElementById('pauseTrailsEnabled')) {
                game.settings.trails = document.getElementById('pauseTrailsEnabled').checked;
            }
            
            // Переключаем экраны
            document.getElementById('menuScreen').classList.remove('active');
            document.getElementById('gameScreen').classList.add('active');
            
            // Инициализируем игру
            initGame();
        }

        // Показать ошибку
        function showError(message) {
            const errorDiv = document.getElementById('errorMessage');
            errorDiv.textContent = message;
            errorDiv.style.display = 'block';
            setTimeout(() => errorDiv.style.display = 'none', 3000);
        }

        // Инициализация игры
        function initGame() {
            console.log('Инициализация игры...');
            
            // Создаем игрока
            game.player = {
                x: 0,
                y: 0,
                radius: 32,
                color: BALL_COLORS[0],
                health: 100,
                maxHealth: 100,
                speed: 3.5,
                vx: (Math.random() - 0.5) * 4,
                vy: (Math.random() - 0.5) * 4,
                weapon: null,
                name: game.playerName,
                isPlayer: true,
                lastHitTime: 0,
                damageEffect: 0,
                trailTimer: 0,
                killStreak: 0,
                totalDamage: 0,
                kills: 0
            };
            
            // Создаем врагов
            createEnemies(game.botCount);
            
            // Сбрасываем игровые данные
            game.weapons = [];
            game.effects = [];
            game.particles = [];
            game.trails = [];
            game.floatingTexts = [];
            game.crowns = [];
            game.kills = 0;
            game.combo = 0;
            game.gameTime = 0;
            game.screenShake = 0;
            game.frameCount = 0;
            game.lastFrameTime = performance.now();
            game.sessionStartTime = Date.now();
            
            // Обновляем интерфейс
            updateGameUI();
            
            // Запускаем игру
            game.running = true;
            game.paused = false;
            
            // Запускаем игровой цикл
            gameLoop();
            
            // Запускаем спавн оружия
            setTimeout(() => spawnWeapon(), 1000);
            setInterval(() => {
                if (game.running && !game.paused) {
                    spawnWeapon();
                }
            }, 5000);
            
            console.log('Игра началась');
        }

        // Создать врагов
        function createEnemies(count) {
            game.enemies = [];
            
            // Настройки сложности
            const difficultySettings = {
                easy: { speed: 2.0, aggression: 0.3, accuracy: 0.3 },
                medium: { speed: 2.5, aggression: 0.5, accuracy: 0.5 },
                hard: { speed: 3.0, aggression: 0.7, accuracy: 0.7 },
                insane: { speed: 3.5, aggression: 0.9, accuracy: 0.9 }
            };
            
            const settings = difficultySettings[game.settings.botDifficulty] || difficultySettings.medium;
            
            for (let i = 0; i < count; i++) {
                const angle = (i / count) * Math.PI * 2;
                const distance = 180;
                
                game.enemies.push({
                    x: Math.cos(angle) * distance,
                    y: Math.sin(angle) * distance,
                    radius: 30 + Math.random() * 8,
                    color: BALL_COLORS[4 + (i % 12)],
                    health: 100,
                    maxHealth: 100,
                    speed: settings.speed,
                    vx: (Math.random() - 0.5) * 3.5,
                    vy: (Math.random() - 0.5) * 3.5,
                    name: ['Злой', 'Хитрый', 'Сильный', 'Быстрый', 'Умный', 'Страшный', 'Ловкий', 'Жестокий'][i % 8] + ' Бот',
                    weapon: null,
                    isPlayer: false,
                    aiTimer: 0,
                    target: null,
                    aggression: settings.aggression,
                    accuracy: settings.accuracy,
                    lastAttackTime: 0,
                    attackCooldown: 1000 + Math.random() * 2000,
                    lastHitTime: 0,
                    damageEffect: 0,
                    trailTimer: 0,
                    killStreak: 0,
                    totalDamage: 0,
                    kills: 0
                });
            }
        }

        // Изменить количество ботов
        function changeBotCount(delta) {
            if (!game.running || game.paused) return;
            
            if (delta > 0 && game.enemies.length < 15) {
                addBot();
            } else if (delta < 0 && game.enemies.length > 1) {
                removeBot();
            }
            
            document.getElementById('gameBotCount').textContent = game.enemies.length;
        }

        // Добавить бота
        function addBot() {
            if (!game.running || game.paused || game.enemies.length >= 15) return;
            
            const difficultySettings = {
                easy: { speed: 2.0, aggression: 0.3, accuracy: 0.3 },
                medium: { speed: 2.5, aggression: 0.5, accuracy: 0.5 },
                hard: { speed: 3.0, aggression: 0.7, accuracy: 0.7 },
                insane: { speed: 3.5, aggression: 0.9, accuracy: 0.9 }
            };
            
            const settings = difficultySettings[game.settings.botDifficulty] || difficultySettings.medium;
            
            const newBot = {
                x: (Math.random() - 0.5) * 300,
                y: (Math.random() - 0.5) * 300,
                radius: 30 + Math.random() * 8,
                color: BALL_COLORS[4 + Math.floor(Math.random() * 12)],
                health: 100,
                maxHealth: 100,
                speed: settings.speed,
                vx: (Math.random() - 0.5) * 3.5,
                vy: (Math.random() - 0.5) * 3.5,
                name: ['Новый', 'Свежий', 'Дополнительный', 'Экстра'][Math.floor(Math.random() * 4)] + ' Бот',
                weapon: null,
                isPlayer: false,
                aiTimer: 0,
                target: null,
                aggression: settings.aggression,
                accuracy: settings.accuracy,
                lastAttackTime: 0,
                attackCooldown: 1000 + Math.random() * 2000,
                lastHitTime: 0,
                damageEffect: 0,
                trailTimer: 0,
                killStreak: 0,
                totalDamage: 0,
                kills: 0
            };
            
            game.enemies.push(newBot);
            
            // Эффект спавна
            if (game.settings.effects) {
                createSpawnEffect(newBot.x, newBot.y, newBot.color);
            }
            
            updateGameUI();
        }

        // Удалить бота
        function removeBot() {
            if (!game.running || game.paused || game.enemies.length <= 1) return;
            
            const removedBot = game.enemies.pop();
            
            // Эффект удаления
            if (game.settings.effects) {
                createDeathEffect(removedBot);
            }
            
            updateGameUI();
        }

        // Спавн оружия
        function spawnWeapon() {
            if (!game.running || game.paused || game.weapons.length >= 8) return;
            
            // Фильтруем доступные типы оружия
            const availableWeapons = Object.values(WEAPON_TYPES).filter(w => 
                game.enabledWeapons.has(w.id)
            );
            
            if (availableWeapons.length === 0) return;
            
            const weaponType = availableWeapons[Math.floor(Math.random() * availableWeapons.length)];
            
            // Находим свободную позицию
            let x, y;
            let attempts = 0;
            const maxAttempts = 50;
            
            do {
                const angle = Math.random() * Math.PI * 2;
                const distance = Math.random() * 180;
                x = Math.cos(angle) * distance;
                y = Math.sin(angle) * distance;
                attempts++;
            } while (isTooCloseToEntity(x, y) && attempts < maxAttempts);
            
            if (attempts < maxAttempts) {
                const weapon = {
                    ...weaponType,
                    x: x,
                    y: y,
                    id: Date.now() + Math.random(),
                    blink: true,
                    rotation: 0,
                    floatOffset: Math.random() * Math.PI * 2
                };
                
                game.weapons.push(weapon);
                
                if (game.settings.effects) {
                    createSpawnEffect(x, y, weapon.color);
                }
            }
        }

        // Проверка, не слишком ли близко к другим объектам
        function isTooCloseToEntity(x, y, radius = 40) {
            // Проверяем игрока
            const dxPlayer = game.player.x - x;
            const dyPlayer = game.player.y - y;
            if (Math.sqrt(dxPlayer*dxPlayer + dyPlayer*dyPlayer) < radius + game.player.radius) {
                return true;
            }
            
            // Проверяем врагов
            for (const enemy of game.enemies) {
                const dx = enemy.x - x;
                const dy = enemy.y - y;
                if (Math.sqrt(dx*dx + dy*dy) < radius + enemy.radius) {
                    return true;
                }
            }
            
            // Проверяем другое оружие
            for (const weapon of game.weapons) {
                const dx = weapon.x - x;
                const dy = weapon.y - y;
                if (Math.sqrt(dx*dx + dy*dy) < radius + 20) {
                    return true;
                }
            }
            
            return false;
        }

        // Игровой цикл
        function gameLoop() {
            if (!game.running || game.paused) return;
            
            // Расчет FPS
            const now = performance.now();
            game.frameCount++;
            if (now >= game.lastFrameTime + 1000) {
                game.fps = Math.round((game.frameCount * 1000) / (now - game.lastFrameTime));
                game.frameCount = 0;
                game.lastFrameTime = now;
            }
            
            // Увеличиваем время игры
            game.gameTime++;
            
            // Обновление комбо
            updateCombo();
            
            // Обновление
            updateGame();
            
            // Отрисовка
            drawGame();
            
            // Обновление короны создателя (всегда, независимо от настроек)
            updateCreatorCrown();
            
            // Следующий кадр
            requestAnimationFrame(gameLoop);
        }

        // Обновить комбо
        function updateCombo() {
            const now = Date.now();
            if (now - game.lastComboTime > 3000) {
                if (game.combo > 1 && game.settings.floatingText) {
                    createComboResetEffect();
                }
                game.combo = 0;
            }
            game.comboTime = now;
        }

        // Обновление короны создателя
        function updateCreatorCrown() {
            // Создаем короны для Jekapro2013 (всегда, даже если эффекты отключены)
            if (game.player.name === 'Jekapro2013') {
                game.crowns.push({
                    x: game.player.x + (Math.random() - 0.5) * 60,
                    y: game.player.y - 40 + (Math.random() - 0.5) * 20,
                    size: 20 + Math.random() * 10,
                    life: 60,
                    rotation: Math.random() * Math.PI * 2,
                    rotationSpeed: (Math.random() - 0.5) * 0.1
                });
            }
            
            // Обновляем существующие короны
            for (let i = game.crowns.length - 1; i >= 0; i--) {
                const crown = game.crowns[i];
                crown.life--;
                crown.rotation += crown.rotationSpeed;
                crown.y -= 0.5;
                
                if (crown.life <= 0) {
                    game.crowns.splice(i, 1);
                }
            }
        }

        // Обновление игры
        function updateGame() {
            // Обновление всех сущностей
            updateEntity(game.player);
            game.enemies.forEach(enemy => updateEntity(enemy));
            
            // Обновление ИИ ботов
            updateBotAI();
            
            // Проверка сбора оружия
            checkPickups();
            
            // Проверка использования оружия
            checkWeaponUsage();
            
            // Проверка столкновений
            checkCollisions();
            
            // Обновление эффектов
            updateEffects();
            
            // Обновление частиц
            updateParticles();
            
            // Обновление следов
            updateTrails();
            
            // Обновление плавающего текста
            updateFloatingTexts();
            
            // Проверка здоровья
            checkHealth();
            
            // Проверка победы
            if (game.enemies.length === 0) {
                winGame();
            }
            
            // Обновление интерфейса
            updateGameUI();
            updateLeaderboard();
            
            // Обновление тряски экрана
            if (game.screenShake > 0) {
                game.screenShake--;
            }
        }

        // Обновить сущность
        function updateEntity(entity) {
            // Автоматическое движение
            entity.x += entity.vx;
            entity.y += entity.vy;
            
            // Ограничение в пределах арены с отскоком
            if (game.settings.wallBounce) {
                keepInArenaWithBounce(entity);
            } else {
                keepInArena(entity);
            }
            
            // Случайное изменение направления
            if (Math.random() < 0.01) {
                entity.vx += (Math.random() - 0.5) * 0.3;
                entity.vy += (Math.random() - 0.5) * 0.3;
                
                // Ограничение скорости
                const speed = Math.sqrt(entity.vx*entity.vx + entity.vy*entity.vy);
                if (speed > entity.speed) {
                    entity.vx = (entity.vx / speed) * entity.speed;
                    entity.vy = (entity.vy / speed) * entity.speed;
                }
            }
            
            // Постепенно уменьшаем эффект урона
            if (entity.damageEffect > 0) {
                entity.damageEffect -= 0.03;
                if (entity.damageEffect < 0) entity.damageEffect = 0;
            }
            
            // Создание следов
            if (game.settings.trails) {
                entity.trailTimer++;
                if (entity.trailTimer >= 3) {
                    entity.trailTimer = 0;
                    createTrail(entity.x, entity.y, entity.color, entity.radius * 0.7);
                }
            }
        }

        // Ограничение в пределах арены с отскоком
        function keepInArenaWithBounce(entity) {
            const arenaRadius = 250;
            const dist = Math.sqrt(entity.x * entity.x + entity.y * entity.y);
            
            if (dist + entity.radius > arenaRadius) {
                // Отодвигаем от границы
                const angle = Math.atan2(entity.y, entity.x);
                entity.x = Math.cos(angle) * (arenaRadius - entity.radius);
                entity.y = Math.sin(angle) * (arenaRadius - entity.radius);
                
                // Рассчитываем отскок
                const normalX = Math.cos(angle);
                const normalY = Math.sin(angle);
                const dot = entity.vx * normalX + entity.vy * normalY;
                
                // Отражение вектора скорости
                entity.vx = entity.vx - 1.8 * dot * normalX;
                entity.vy = entity.vy - 1.8 * dot * normalY;
                
                // Немного теряем энергию при отскоке
                entity.vx *= 0.9;
                entity.vy *= 0.9;
                
                // Эффект отскока
                if (game.settings.effects) {
                    createWallBounceEffect(entity.x, entity.y, angle, entity.color);
                }
            }
        }

        // Ограничение в пределах арены без отскока
        function keepInArena(entity) {
            const arenaRadius = 250;
            const dist = Math.sqrt(entity.x * entity.x + entity.y * entity.y);
            
            if (dist + entity.radius > arenaRadius) {
                const angle = Math.atan2(entity.y, entity.x);
                entity.x = Math.cos(angle) * (arenaRadius - entity.radius);
                entity.y = Math.sin(angle) * (arenaRadius - entity.radius);
                
                // Просто останавливаем у границы
                entity.vx = 0;
                entity.vy = 0;
            }
        }

        // Обновить ИИ ботов
        function updateBotAI() {
            const now = Date.now();
            
            game.enemies.forEach(enemy => {
                // Поиск цели
                if (!enemy.target || Math.random() < 0.02) {
                    enemy.target = findTarget(enemy);
                }
                
                // Если есть цель и оружие, пытаемся атаковать
                if (enemy.target && enemy.weapon && now - enemy.lastAttackTime > enemy.attackCooldown) {
                    const dx = enemy.x - enemy.target.x;
                    const dy = enemy.y - enemy.target.y;
                    const distance = Math.sqrt(dx*dx + dy*dy);
                    
                    // Учет точности бота
                    const effectiveRange = enemy.weapon.range * enemy.accuracy;
                    
                    if (distance < effectiveRange) {
                        useWeapon(enemy, enemy.target);
                        enemy.lastAttackTime = now;
                        enemy.attackCooldown = 1000 + Math.random() * 2000;
                    }
                }
                
                // Движение к цели или случайное блуждание
                if (enemy.target) {
                    const dx = enemy.target.x - enemy.x;
                    const dy = enemy.target.y - enemy.y;
                    const distance = Math.sqrt(dx*dx + dy*dy);
                    
                    if (distance > 50) {
                        enemy.vx += (dx / distance) * 0.05 * enemy.aggression;
                        enemy.vy += (dy / distance) * 0.05 * enemy.aggression;
                    }
                } else {
                    // Случайное блуждание
                    if (Math.random() < 0.02) {
                        enemy.vx += (Math.random() - 0.5) * 0.5;
                        enemy.vy += (Math.random() - 0.5) * 0.5;
                    }
                }
                
                // Ограничение скорости
                const speed = Math.sqrt(enemy.vx*enemy.vx + enemy.vy*enemy.vy);
                if (speed > enemy.speed) {
                    enemy.vx = (enemy.vx / speed) * enemy.speed;
                    enemy.vy = (enemy.vy / speed) * enemy.speed;
                }
            });
        }

        // Найти цель для бота
        function findTarget(bot) {
            let targets = [];
            
            // Добавляем игрока как цель
            targets.push({
                entity: game.player,
                distance: getDistance(bot, game.player),
                priority: 2.0
            });
            
            // Добавляем других ботов как цели (если включено)
            if (game.settings.botFriendlyFire) {
                game.enemies.forEach(enemy => {
                    if (enemy !== bot) {
                        targets.push({
                            entity: enemy,
                            distance: getDistance(bot, enemy),
                            priority: 1.0
                        });
                    }
                });
            }
            
            // Сортируем по приоритету и расстоянию
            targets.sort((a, b) => {
                const scoreA = a.priority / (a.distance + 1);
                const scoreB = b.priority / (b.distance + 1);
                return scoreB - scoreA;
            });
            
            return targets.length > 0 ? targets[0].entity : null;
        }

        // Получить расстояние между сущностями
        function getDistance(a, b) {
            const dx = a.x - b.x;
            const dy = a.y - b.y;
            return Math.sqrt(dx*dx + dy*dy);
        }

        // Проверка сбора предметов
        function checkPickups() {
            // Проверяем игрока
            if (game.settings.autoCollect) {
                checkEntityPickups(game.player);
            }
            
            // Проверяем врагов
            game.enemies.forEach(enemy => {
                checkEntityPickups(enemy);
            });
        }

        // Проверка сбора предметов сущностью
        function checkEntityPickups(entity) {
            // Сбор оружия
            for (let i = game.weapons.length - 1; i >= 0; i--) {
                const weapon = game.weapons[i];
                const dx = entity.x - weapon.x;
                const dy = entity.y - weapon.y;
                const dist = Math.sqrt(dx*dx + dy*dy);
                
                if (dist < entity.radius + 20) {
                    if (!entity.weapon) {
                        entity.weapon = { ...weapon };
                        game.weapons.splice(i, 1);
                        
                        // Эффект подбора
                        if (game.settings.effects) {
                            createWeaponPickupEffect(entity, weapon);
                        }
                        
                        // Обновляем интерфейс для игрока
                        if (entity.isPlayer) {
                            updateWeaponUI();
                            if (game.settings.floatingText) {
                                createFloatingText('+ ' + weapon.name, entity.x, entity.y, weapon.color);
                            }
                        }
                    }
                    break;
                }
            }
        }

        // Проверка использования оружия
        function checkWeaponUsage() {
            const now = Date.now();
            
            // Проверяем игрока
            if (game.player.weapon && now - game.player.lastHitTime > 1000) {
                let closestEnemy = null;
                let minDist = game.player.weapon.range;
                
                game.enemies.forEach(enemy => {
                    const dx = game.player.x - enemy.x;
                    const dy = game.player.y - enemy.y;
                    const dist = Math.sqrt(dx*dx + dy*dy);
                    
                    if (dist < minDist) {
                        minDist = dist;
                        closestEnemy = enemy;
                    }
                });
                
                if (closestEnemy) {
                    useWeapon(game.player, closestEnemy);
                }
            }
            
            // Боты атакуют автоматически
        }

        // Использовать оружие
        function useWeapon(attacker, target) {
            const now = Date.now();
            
            // Наносим урон
            const damage = attacker.weapon.damage;
            target.health -= damage;
            
            // Учитываем нанесенный урон
            attacker.totalDamage += damage;
            
            // Эффекты урона
            if (game.settings.effects) {
                createDamageEffect(target, damage);
                createAttackEffect(attacker, target);
            }
            
            // Обнуляем оружие
            attacker.weapon = null;
            attacker.lastHitTime = now;
            
            // Эффект разрушения оружия
            if (game.settings.effects) {
                createWeaponBreakEffect(attacker);
            }
            
            // Обновляем интерфейс для игрока
            if (attacker.isPlayer) {
                updateWeaponUI();
                
                // Увеличиваем комбо
                game.combo++;
                game.lastComboTime = now;
                if (game.combo > 1 && game.settings.floatingText) {
                    createComboEffect(game.combo);
                }
            }
        }

        // Проверка столкновений
        function checkCollisions() {
            // Столкновения между врагами и игроком
            game.enemies.forEach(enemy => {
                const dx = game.player.x - enemy.x;
                const dy = game.player.y - enemy.y;
                const dist = Math.sqrt(dx*dx + dy*dy);
                const minDist = game.player.radius + enemy.radius;
                
                if (dist < minDist) {
                    // Разделение
                    const angle = Math.atan2(dy, dx);
                    const overlap = minDist - dist;
                    
                    game.player.x += Math.cos(angle) * overlap * 0.5;
                    game.player.y += Math.sin(angle) * overlap * 0.5;
                    enemy.x -= Math.cos(angle) * overlap * 0.5;
                    enemy.y -= Math.sin(angle) * overlap * 0.5;
                    
                    // Отскок при включенной настройке
                    if (game.settings.wallBounce) {
                        const force = 0.5;
                        game.player.vx += Math.cos(angle) * force;
                        game.player.vy += Math.sin(angle) * force;
                        enemy.vx -= Math.cos(angle) * force;
                        enemy.vy -= Math.sin(angle) * force;
                    }
                    
                    // Эффект столкновения
                    if (game.settings.effects) {
                        createCollisionEffect(game.player, enemy);
                    }
                }
            });
            
            // Столкновения между ботами
            for (let i = 0; i < game.enemies.length; i++) {
                for (let j = i + 1; j < game.enemies.length; j++) {
                    const a = game.enemies[i];
                    const b = game.enemies[j];
                    
                    const dx = a.x - b.x;
                    const dy = a.y - b.y;
                    const dist = Math.sqrt(dx*dx + dy*dy);
                    const minDist = a.radius + b.radius;
                    
                    if (dist < minDist) {
                        // Разделение
                        const angle = Math.atan2(dy, dx);
                        const overlap = minDist - dist;
                        
                        a.x += Math.cos(angle) * overlap * 0.5;
                        a.y += Math.sin(angle) * overlap * 0.5;
                        b.x -= Math.cos(angle) * overlap * 0.5;
                        b.y -= Math.sin(angle) * overlap * 0.5;
                        
                        // Отскок при включенной настройке
                        if (game.settings.wallBounce) {
                            const force = 0.5;
                            a.vx += Math.cos(angle) * force;
                            a.vy += Math.sin(angle) * force;
                            b.vx -= Math.cos(angle) * force;
                            b.vy -= Math.sin(angle) * force;
                        }
                        
                        // Эффект столкновения
                        if (game.settings.effects) {
                            createCollisionEffect(a, b);
                        }
                    }
                }
            }
        }

        // Проверка здоровья
        function checkHealth() {
            // Проверка здоровья игрока
            if (game.player.health <= 0) {
                game.player.health = 0;
                gameOver();
            }
            
            // Удаляем мертвых врагов
            for (let i = game.enemies.length - 1; i >= 0; i--) {
                const enemy = game.enemies[i];
                if (enemy.health <= 0) {
                    // Определяем, кто убил
                    let killer = null;
                    const now = Date.now();
                    
                    // Ищем, кто недавно атаковал этого врага
                    if (now - game.player.lastHitTime < 1000 && 
                        getDistance(game.player, enemy) < 200) {
                        killer = game.player;
                    } else if (game.settings.botFriendlyFire) {
                        // Проверяем других ботов
                        for (const otherEnemy of game.enemies) {
                            if (otherEnemy !== enemy && 
                                now - otherEnemy.lastAttackTime < 1000 &&
                                getDistance(otherEnemy, enemy) < 200) {
                                killer = otherEnemy;
                                break;
                            }
                        }
                    }
                    
                    // Награда за убийство
                    if (killer) {
                        if (killer.isPlayer) {
                            // Игрок убил
                            game.kills++;
                            game.totalKills++;
                            killer.kills++;
                            killer.killStreak++;
                            
                            // Награда монетами
                            const coinReward = 2 + Math.floor(game.combo / 2);
                            game.coins += coinReward;
                            killer.coins = (killer.coins || 0) + coinReward;
                            
                            // Эффекты
                            if (game.settings.effects) {
                                createCoinEffect(enemy, coinReward);
                                createKillEffect(killer, enemy);
                            }
                            
                            // Обновляем рекорд
                            if (game.kills > game.record) {
                                game.record = game.kills;
                                if (game.kills >= 5 && game.settings.effects) {
                                    createNewRecordEffect();
                                }
                            }
                        } else {
                            // Бот убил
                            killer.kills++;
                            killer.killStreak++;
                            if (game.settings.effects) {
                                createBotKillEffect(killer, enemy);
                            }
                        }
                    }
                    
                    // Эффект смерти
                    if (game.settings.effects) {
                        createDeathEffect(enemy);
                    }
                    
                    // Удаляем врага
                    game.enemies.splice(i, 1);
                }
            }
        }

        // ========== ЭФФЕКТЫ И АНИМАЦИИ ==========

        // Создать эффект урона
        function createDamageEffect(target, damage) {
            if (!game.settings.effects) return;
            
            // Текст урона
            if (game.settings.floatingText) {
                createFloatingText('-' + damage, target.x, target.y - target.radius - 15, '#ff0000', 24);
            }
            
            // Эффект встряски
            target.damageEffect = 1;
            if (game.settings.screenShake) {
                game.screenShake = 8;
            }
        }

        // Создать эффект атаки
        function createAttackEffect(attacker, target) {
            if (!game.settings.effects) return;
            
            // Линия атаки
            createBeamEffect(attacker, target, attacker.weapon.color);
        }

        // Создать эффект подбора оружия
        function createWeaponPickupEffect(entity, weapon) {
            if (!game.settings.effects) return;
            
            // Кольцевая волна
            createRingEffect(entity.x, entity.y, weapon.color, 30, 20);
        }

        // Создать эффект разрушения оружия
        function createWeaponBreakEffect(entity) {
            if (!game.settings.effects) return;
            
            // Взрыв частиц
            for (let i = 0; i < 8; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 1 + Math.random() * 3;
                createParticle(
                    entity.x, entity.y,
                    entity.weapon ? entity.weapon.color : '#ffffff',
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    2,
                    30
                );
            }
        }

        // Создать эффект смерти
        function createDeathEffect(entity) {
            if (!game.settings.effects) return;
            
            // Взрыв
            for (let i = 0; i < 12; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 1 + Math.random() * 4;
                createParticle(
                    entity.x, entity.y,
                    entity.color,
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    3 + Math.random() * 3,
                    40
                );
            }
        }

        // Создать эффект убийства
        function createKillEffect(killer, victim) {
            if (!game.settings.effects) return;
            
            // Волна от убийцы
            createRingEffect(killer.x, killer.y, '#FFD700', killer.radius * 1.5, 25);
        }

        // Создать эффект убийства бота
        function createBotKillEffect(killer, victim) {
            if (!game.settings.effects) return;
            
            // Маленькая волна
            createRingEffect(killer.x, killer.y, killer.color, killer.radius * 1.2, 15);
        }

        // Создать эффект комбо
        function createComboEffect(combo) {
            if (!game.settings.effects || !game.settings.floatingText) return;
            
            const x = game.player.x;
            const y = game.player.y;
            
            // Текст комбо
            createFloatingText('КОМБО x' + combo, x, y - 50, '#FF416C', 28 + combo);
        }

        // Создать эффект сброса комбо
        function createComboResetEffect() {
            if (!game.settings.effects || !game.settings.floatingText) return;
            
            createFloatingText('КОМБО СБРОШЕНО', game.player.x, game.player.y, '#808080', 20);
        }

        // Создать эффект нового рекорда
        function createNewRecordEffect() {
            if (!game.settings.effects || !game.settings.floatingText) return;
            
            createFloatingText('НОВЫЙ РЕКОРД!', game.player.x, game.player.y, '#FFD700', 32);
        }

        // Создать эффект монет
        function createCoinEffect(entity, amount) {
            if (!game.settings.effects || !game.settings.floatingText) return;
            
            // Текст монет
            createFloatingText('+' + amount + '💰', entity.x, entity.y, '#FFD700', 20);
        }

        // Создать эффект спавна
        function createSpawnEffect(x, y, color) {
            if (!game.settings.effects) return;
            
            createRingEffect(x, y, color, 20, 30);
        }

        // Создать эффект столкновения
        function createCollisionEffect(a, b) {
            if (!game.settings.effects) return;
            
            const midX = (a.x + b.x) / 2;
            const midY = (a.y + b.y) / 2;
            
            createRingEffect(midX, midY, '#ffffff', 15, 10);
        }

        // Создать эффект отскока от стены
        function createWallBounceEffect(x, y, angle, color) {
            if (!game.settings.effects) return;
            
            createRingEffect(x, y, color, 25, 15);
        }

        // Создать луч
        function createBeamEffect(from, to, color, duration = 15) {
            if (!game.settings.effects) return;
            
            game.effects.push({
                type: 'beam',
                from: {x: from.x, y: from.y},
                to: {x: to.x, y: to.y},
                color: color,
                life: duration,
                maxLife: duration,
                width: 2
            });
        }

        // Создать кольцо
        function createRingEffect(x, y, color, radius, duration = 20) {
            if (!game.settings.effects) return;
            
            game.effects.push({
                type: 'ring',
                x: x,
                y: y,
                color: color,
                radius: radius,
                life: duration,
                maxLife: duration,
                width: 2
            });
        }

        // Создать след
        function createTrail(x, y, color, size) {
            if (!game.settings.trails) return;
            
            game.trails.push({
                x: x,
                y: y,
                color: color,
                size: size,
                life: 20,
                maxLife: 20
            });
        }

        // Создать частицу
        function createParticle(x, y, color, vx, vy, size, life) {
            if (!game.settings.effects) return;
            
            game.particles.push({
                x: x,
                y: y,
                color: color,
                vx: vx,
                vy: vy,
                size: size,
                life: life,
                maxLife: life,
                gravity: 0.05
            });
        }

        // Создать плавающий текст
        function createFloatingText(text, x, y, color, size = 18) {
            if (!game.settings.floatingText) return;
            
            game.floatingTexts.push({
                text: text,
                x: x,
                y: y,
                color: color,
                size: size,
                life: 60,
                maxLife: 60,
                vy: -1
            });
        }

        // Обновить эффекты
        function updateEffects() {
            for (let i = game.effects.length - 1; i >= 0; i--) {
                const effect = game.effects[i];
                effect.life--;
                
                if (effect.life <= 0) {
                    game.effects.splice(i, 1);
                }
            }
        }

        // Обновить частицы
        function updateParticles() {
            for (let i = game.particles.length - 1; i >= 0; i--) {
                const p = game.particles[i];
                p.x += p.vx;
                p.y += p.vy;
                p.vy += p.gravity;
                p.life--;
                
                if (p.life <= 0) {
                    game.particles.splice(i, 1);
                }
            }
        }

        // Обновить следы
        function updateTrails() {
            for (let i = game.trails.length - 1; i >= 0; i--) {
                const trail = game.trails[i];
                trail.life--;
                
                if (trail.life <= 0) {
                    game.trails.splice(i, 1);
                }
            }
        }

        // Обновить плавающий текст
        function updateFloatingTexts() {
            for (let i = game.floatingTexts.length - 1; i >= 0; i--) {
                const text = game.floatingTexts[i];
                text.y += text.vy;
                text.life--;
                
                if (text.life <= 0) {
                    game.floatingTexts.splice(i, 1);
                }
            }
        }

        // ========== ИНТЕРФЕЙС И ЛИДЕРБОРД ==========

        // Обновить игровой интерфейс
        function updateGameUI() {
            // Статистика игрока
            document.getElementById('playerNameDisplay').textContent = game.player.name;
            document.getElementById('healthDisplay').textContent = Math.ceil(game.player.health);
            document.getElementById('coinsDisplay').textContent = game.coins;
            document.getElementById('killsDisplay').textContent = game.kills;
            document.getElementById('comboDisplay').textContent = game.combo;
            document.getElementById('enemiesDisplay').textContent = game.enemies.length;
            document.getElementById('weaponsDisplay').textContent = game.weapons.length;
            
            // Полоса здоровья
            const healthPercent = (game.player.health / game.player.maxHealth) * 100;
            document.getElementById('healthBarFill').style.width = healthPercent + '%';
            
            // Цвет полосы здоровья
            const healthBar = document.getElementById('healthBarFill');
            if (healthPercent > 60) {
                healthBar.style.background = 'linear-gradient(90deg, #00ff00, #00cc00)';
            } else if (healthPercent > 30) {
                healthBar.style.background = 'linear-gradient(90deg, #FF9800, #EF6C00)';
            } else {
                healthBar.style.background = 'linear-gradient(90deg, #ff0000, #cc0000)';
            }
            
            // Время игры
            const minutes = Math.floor(game.gameTime / 60);
            const seconds = game.gameTime % 60;
            document.getElementById('gameTimeDisplay').textContent = 
                `${minutes}:${seconds.toString().padStart(2, '0')}`;
            
            // FPS
            document.getElementById('fpsDisplay').textContent = game.fps;
            
            // Время сессии
            const sessionTime = Math.floor((Date.now() - game.sessionStartTime) / 1000);
            const sessionMinutes = Math.floor(sessionTime / 60);
            const sessionSeconds = sessionTime % 60;
            document.getElementById('sessionTime').textContent = 
                `${sessionMinutes}:${sessionSeconds.toString().padStart(2, '0')}`;
            
            document.getElementById('sessionKills').textContent = game.kills;
            document.getElementById('sessionCombo').textContent = game.combo;
            
            // Количество ботов в игре
            document.getElementById('gameBotCount').textContent = game.enemies.length;
        }

        // Обновить интерфейс оружия
        function updateWeaponUI() {
            if (game.player.weapon) {
                document.getElementById('weaponDisplay').textContent = game.player.weapon.name;
                document.getElementById('ammoDisplay').textContent = game.player.weapon.ammo;
                document.getElementById('damageDisplay').textContent = game.player.weapon.damage;
            } else {
                document.getElementById('weaponDisplay').textContent = 'Нет';
                document.getElementById('ammoDisplay').textContent = '-';
                document.getElementById('damageDisplay').textContent = '-';
            }
        }

        // Обновить таблицу лидеров
        function updateLeaderboard() {
            const tbody = document.querySelector('#gameLeaderboard tbody');
            tbody.innerHTML = '';
            
            // Собираем всех участников
            let participants = [
                {
                    name: game.player.name,
                    damage: game.player.totalDamage,
                    health: Math.ceil(game.player.health),
                    kills: game.player.kills || 0,
                    isPlayer: true,
                    isCreator: game.player.name === 'Jekapro2013'
                }
            ];
            
            game.enemies.forEach(enemy => {
                participants.push({
                    name: enemy.name,
                    damage: enemy.totalDamage || 0,
                    health: Math.ceil(enemy.health),
                    kills: enemy.kills || 0,
                    isPlayer: false,
                    isCreator: false
                });
            });
            
            // Сортируем по урону
            participants.sort((a, b) => b.damage - a.damage);
            
            // Заполняем таблицу
            participants.forEach((p, index) => {
                const row = document.createElement('tr');
                if (p.isPlayer) {
                    row.style.background = 'rgba(76, 201, 240, 0.1)';
                }
                
                const healthPercent = (p.health / 100) * 100;
                const healthColor = healthPercent > 60 ? '#4CAF50' : 
                                  healthPercent > 30 ? '#FF9800' : '#F44336';
                
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>
                        ${p.name}
                        ${p.isCreator ? '<span class="creator-badge">СОЗДАТЕЛЬ</span>' : ''}
                    </td>
                    <td>${p.damage}</td>
                    <td>
                        <div class="health-bar" style="width: 50px; display: inline-block;">
                            <div class="health-fill" style="width: ${healthPercent}%; background: ${healthColor};"></div>
                        </div>
                        <span style="margin-left: 5px;">${p.health}</span>
                    </td>
                    <td>${p.kills}</td>
                `;
                tbody.appendChild(row);
            });
            
            // Обновляем быструю статистику
            updateQuickStats(participants);
        }

        // Обновить быструю статистику
        function updateQuickStats(participants) {
            const quickStatsDiv = document.getElementById('quickStats');
            
            // Находим лидеров
            const damageLeader = participants[0];
            const killsLeader = [...participants].sort((a, b) => b.kills - a.kills)[0];
            const healthLeader = [...participants].sort((a, b) => b.health - a.health)[0];
            
            quickStatsDiv.innerHTML = `
                <p><strong>🥇 Урон:</strong> ${damageLeader.name} (${damageLeader.damage})</p>
                <p><strong>🎯 Убийства:</strong> ${killsLeader.name} (${killsLeader.kills})</p>
                <p><strong>❤️ Здоровье:</strong> ${healthLeader.name} (${healthLeader.health})</p>
            `;
        }

        // Пауза
        function togglePause() {
            if (!game.running) return;
            
            game.paused = !game.paused;
            
            if (game.paused) {
                document.getElementById('gameScreen').classList.remove('active');
                document.getElementById('pauseScreen').classList.add('active');
                document.getElementById('pauseBtn').textContent = '▶️ ПРОДОЛЖИТЬ';
            } else {
                document.getElementById('pauseScreen').classList.remove('active');
                document.getElementById('gameScreen').classList.add('active');
                document.getElementById('pauseBtn').textContent = '⏸️ ПАУЗА';
                
                // Обновляем настройки из экрана паузы
                if (document.getElementById('pauseEffectsEnabled')) {
                    game.settings.effects = document.getElementById('pauseEffectsEnabled').checked;
                }
                if (document.getElementById('pauseTrailsEnabled')) {
                    game.settings.trails = document.getElementById('pauseTrailsEnabled').checked;
                }
                
                gameLoop();
            }
        }

        // Показать игровое сообщение
        function showGameMessage(text, color = '#4CC9F0') {
            const messageDiv = document.getElementById('gameMessage');
            messageDiv.textContent = text;
            messageDiv.style.borderColor = color;
            messageDiv.style.display = 'block';
            messageDiv.style.boxShadow = `0 0 30px ${color}`;
            
            setTimeout(() => {
                messageDiv.style.display = 'none';
            }, 2000);
        }

        // Выйти в меню
        function exitToMenu() {
            game.running = false;
            game.paused = false;
            
            // Сохраняем данные игрока в таблицу лидеров
            savePlayerToLeaderboard();
            
            // Сохраняем все данные
            saveGameData();
            saveSettings();
            saveLeaderboard();
            
            // Обновляем статистику в меню
            updateMenuStats();
            updateGlobalLeaderboard();
            
            // Переключаем экраны
            document.getElementById('gameScreen').classList.remove('active');
            document.getElementById('pauseScreen').classList.remove('active');
            document.getElementById('menuScreen').classList.add('active');
        }

        // Сохранить игрока в таблицу лидеров
        function savePlayerToLeaderboard() {
            const playerData = {
                name: game.playerName,
                kills: game.totalKills,
                coins: game.coins,
                wins: game.wins,
                record: game.record,
                lastPlayed: Date.now()
            };
            
            // Находим существующую запись
            const existingIndex = game.leaderboard.findIndex(p => p.name === game.playerName);
            
            if (existingIndex >= 0) {
                // Обновляем существующую запись
                const existing = game.leaderboard[existingIndex];
                existing.kills = Math.max(existing.kills, playerData.kills);
                existing.coins = Math.max(existing.coins, playerData.coins);
                existing.wins = Math.max(existing.wins, playerData.wins);
                existing.record = Math.max(existing.record, playerData.record);
                existing.lastPlayed = playerData.lastPlayed;
            } else {
                // Добавляем новую запись
                game.leaderboard.push(playerData);
            }
            
            // Ограничиваем размер таблицы
            if (game.leaderboard.length > 100) {
                game.leaderboard.sort((a, b) => b.kills - a.kills);
                game.leaderboard = game.leaderboard.slice(0, 50);
            }
        }

        // Сохранить данные игры
        function saveGameData() {
            const data = {
                coins: game.coins,
                totalKills: game.totalKills,
                record: game.record,
                wins: game.wins,
                playerName: game.playerName
            };
            localStorage.setItem('ballFightersData', JSON.stringify(data));
        }

        // Победа
        function winGame() {
            game.running = false;
            game.wins++;
            
            // Награда
            const baseReward = 20;
            const killReward = game.kills * 3;
            const comboBonus = Math.floor(game.combo * 0.5);
            const winBonus = 10;
            const totalReward = baseReward + killReward + comboBonus + winBonus;
            
            game.coins += totalReward;
            
            // Сохраняем
            savePlayerToLeaderboard();
            saveGameData();
            saveLeaderboard();
            
            // Показываем сообщение о победе
            showGameMessage(`🎉 ПОБЕДА! +${totalReward}💰`, '#FFD700');
            
            // Ждем 2 секунды и возвращаем в меню
            setTimeout(() => {
                exitToMenu();
                alert(`🏆 ПОБЕДА!\n\nВаш результат:\nУбийств: ${game.kills}\nКомбо: x${game.combo}\nМонет заработано: ${totalReward}\nВсего монет: ${game.coins}`);
            }, 2000);
        }

        // Конец игры
        function gameOver() {
            game.running = false;
            
            // Сохраняем
            savePlayerToLeaderboard();
            saveGameData();
            saveLeaderboard();
            
            // Показываем сообщение о поражении
            showGameMessage('💀 ПОРАЖЕНИЕ', '#ff0000');
            
            // Ждем 2 секунды и возвращаем в меню
            setTimeout(() => {
                exitToMenu();
                alert(`💀 ИГРА ОКОНЧЕНА!\n\nВаш результат:\nУбийств: ${game.kills}\nКомбо: x${game.combo}\nМонет заработано: ${game.kills * 2}\nВсего монет: ${game.coins}`);
            }, 2000);
        }

        // ========== ОТРИСОВКА ==========

        // Отрисовка игры
        function drawGame() {
            const ctx = game.ctx;
            const canvas = game.canvas;
            
            // Очистка
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Тряска экрана
            let offsetX = 0, offsetY = 0;
            if (game.settings.screenShake && game.screenShake > 0) {
                offsetX = (Math.random() - 0.5) * game.screenShake;
                offsetY = (Math.random() - 0.5) * game.screenShake;
            }
            
            // Смещение для центрирования
            const centerX = canvas.width / 2 + offsetX;
            const centerY = canvas.height / 2 + offsetY;
            
            // Рисуем арену
            drawArena(centerX, centerY);
            
            // Рисуем следы
            if (game.settings.trails) {
                drawTrails(centerX, centerY);
            }
            
            // Рисуем эффекты
            if (game.settings.effects) {
                drawEffects(centerX, centerY);
            }
            
            // Рисуем оружие на земле
            drawWeapons(centerX, centerY);
            
            // Рисуем врагов
            game.enemies.forEach(enemy => {
                drawEntity(enemy, centerX, centerY);
            });
            
            // Рисуем игрока
            drawEntity(game.player, centerX, centerY);
            
            // Рисуем частицы
            if (game.settings.effects) {
                drawParticles(centerX, centerY);
            }
            
            // Рисуем плавающий текст
            if (game.settings.floatingText) {
                drawFloatingTexts(centerX, centerY);
            }
            
            // Рисуем короны создателя (всегда!)
            drawCrowns(centerX, centerY);
        }

        // Рисовать арену
        function drawArena(offsetX, offsetY) {
            const ctx = game.ctx;
            const arenaRadius = 250;
            
            // Фон арены
            const gradient = ctx.createRadialGradient(
                offsetX, offsetY, arenaRadius * 0.3,
                offsetX, offsetY, arenaRadius
            );
            gradient.addColorStop(0, 'rgba(10, 25, 49, 0.9)');
            gradient.addColorStop(1, 'rgba(20, 35, 70, 0.7)');
            
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(offsetX, offsetY, arenaRadius, 0, Math.PI * 2);
            ctx.fill();
            
            // Сетка арены
            ctx.strokeStyle = 'rgba(76, 201, 240, 0.1)';
            ctx.lineWidth = 1;
            
            // Концентрические круги
            for (let i = 1; i <= 3; i++) {
                const radius = arenaRadius * (i / 3);
                ctx.beginPath();
                ctx.arc(offsetX, offsetY, radius, 0, Math.PI * 2);
                ctx.stroke();
            }
            
            // Граница арены
            ctx.strokeStyle = '#4CC9F0';
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.arc(offsetX, offsetY, arenaRadius, 0, Math.PI * 2);
            ctx.stroke();
        }

        // Рисовать сущность
        function drawEntity(entity, offsetX, offsetY) {
            const ctx = game.ctx;
            const x = entity.x + offsetX;
            const y = entity.y + offsetY;
            
            // Эффект получения урона
            if (entity.damageEffect > 0) {
                ctx.save();
                ctx.globalAlpha = entity.damageEffect * 0.5;
                ctx.fillStyle = '#ff0000';
                ctx.beginPath();
                ctx.arc(x, y, entity.radius + 5, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
            
            // Тень
            ctx.fillStyle = 'rgba(0, 0, 0, 0.3)';
            ctx.beginPath();
            ctx.arc(x, y + 4, entity.radius, 0, Math.PI * 2);
            ctx.fill();
            
            // Основной круг
            const gradient = ctx.createRadialGradient(
                x - entity.radius/3, y - entity.radius/3, 0,
                x, y, entity.radius
            );
            gradient.addColorStop(0, lightenColor(entity.color, 30));
            gradient.addColorStop(1, entity.color);
            
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(x, y, entity.radius, 0, Math.PI * 2);
            ctx.fill();
            
            // Контур
            ctx.strokeStyle = entity.isPlayer ? '#4CC9F0' : darkenColor(entity.color, 30);
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.arc(x, y, entity.radius, 0, Math.PI * 2);
            ctx.stroke();
            
            // Иконка оружия
            if (entity.weapon) {
                ctx.save();
                ctx.translate(x, y);
                
                // Вращающаяся иконка
                const iconAngle = Date.now() * 0.002;
                const iconDistance = entity.radius + 12;
                const iconX = Math.cos(iconAngle) * iconDistance;
                const iconY = Math.sin(iconAngle) * iconDistance;
                
                ctx.fillStyle = entity.weapon.color;
                ctx.beginPath();
                ctx.arc(iconX, iconY, 10, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.strokeStyle = '#ffffff';
                ctx.lineWidth = 1;
                ctx.stroke();
                
                ctx.restore();
            }
            
            // Имя
            ctx.fillStyle = '#fff';
            ctx.font = `bold ${entity.isPlayer ? 15 : 13}px Arial`;
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(entity.name, x, y);
            
            // Полоса здоровья
            drawHealthBar(entity, x, y);
        }

        // Рисовать полосу здоровья
        function drawHealthBar(entity, x, y) {
            const ctx = game.ctx;
            const barWidth = 60;
            const barHeight = 6;
            const barX = x - barWidth / 2;
            const barY = y - entity.radius - 20;
            const healthPercent = entity.health / entity.maxHealth;
            
            // Фон
            ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
            ctx.fillRect(barX, barY, barWidth, barHeight);
            
            // Здоровье
            const currentWidth = barWidth * healthPercent;
            
            const healthGradient = ctx.createLinearGradient(barX, barY, barX + currentWidth, barY);
            if (healthPercent > 0.6) {
                healthGradient.addColorStop(0, '#00ff00');
                healthGradient.addColorStop(1, '#00cc00');
            } else if (healthPercent > 0.3) {
                healthGradient.addColorStop(0, '#FF9800');
                healthGradient.addColorStop(1, '#EF6C00');
            } else {
                healthGradient.addColorStop(0, '#ff0000');
                healthGradient.addColorStop(1, '#cc0000');
            }
            
            ctx.fillStyle = healthGradient;
            ctx.fillRect(barX, barY, currentWidth, barHeight);
            
            // Рамка
            ctx.strokeStyle = '#000';
            ctx.lineWidth = 1;
            ctx.strokeRect(barX, barY, barWidth, barHeight);
        }

        // Рисовать оружие на земле
        function drawWeapons(offsetX, offsetY) {
            const ctx = game.ctx;
            const time = Date.now() * 0.001;
            
            game.weapons.forEach(weapon => {
                const x = weapon.x + offsetX;
                const y = weapon.y + offsetY + Math.sin(time + weapon.floatOffset) * 5;
                
                // Вращение
                ctx.save();
                ctx.translate(x, y);
                weapon.rotation += 0.02;
                ctx.rotate(weapon.rotation);
                
                // Иконка оружия
                ctx.fillStyle = weapon.color;
                ctx.beginPath();
                ctx.arc(0, 0, 18, 0, Math.PI * 2);
                ctx.fill();
                
                // Обводка
                ctx.strokeStyle = '#fff';
                ctx.lineWidth = 2;
                ctx.stroke();
                
                ctx.restore();
            });
        }

        // Рисовать следы
        function drawTrails(offsetX, offsetY) {
            const ctx = game.ctx;
            
            game.trails.forEach(trail => {
                const x = trail.x + offsetX;
                const y = trail.y + offsetY;
                const lifePercent = trail.life / trail.maxLife;
                
                ctx.globalAlpha = lifePercent * 0.5;
                ctx.fillStyle = trail.color;
                ctx.beginPath();
                ctx.arc(x, y, trail.size * lifePercent, 0, Math.PI * 2);
                ctx.fill();
                ctx.globalAlpha = 1;
            });
        }

        // Рисовать эффекты
        function drawEffects(offsetX, offsetY) {
            const ctx = game.ctx;
            
            game.effects.forEach(effect => {
                const x = effect.x + offsetX;
                const y = effect.y + offsetY;
                const lifePercent = effect.life / effect.maxLife;
                
                ctx.save();
                ctx.globalAlpha = lifePercent;
                
                switch(effect.type) {
                    case 'beam':
                        ctx.strokeStyle = effect.color;
                        ctx.lineWidth = effect.width;
                        ctx.lineCap = 'round';
                        ctx.beginPath();
                        ctx.moveTo(effect.from.x + offsetX, effect.from.y + offsetY);
                        ctx.lineTo(effect.to.x + offsetX, effect.to.y + offsetY);
                        ctx.stroke();
                        break;
                        
                    case 'ring':
                        ctx.strokeStyle = effect.color;
                        ctx.lineWidth = effect.width;
                        ctx.beginPath();
                        ctx.arc(x, y, effect.radius * (1 - lifePercent), 0, Math.PI * 2);
                        ctx.stroke();
                        break;
                }
                
                ctx.restore();
            });
        }

        // Рисовать частицы
        function drawParticles(offsetX, offsetY) {
            const ctx = game.ctx;
            
            game.particles.forEach(p => {
                const x = p.x + offsetX;
                const y = p.y + offsetY;
                const lifePercent = p.life / p.maxLife;
                
                ctx.globalAlpha = lifePercent;
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(x, y, p.size * lifePercent, 0, Math.PI * 2);
                ctx.fill();
                ctx.globalAlpha = 1;
            });
        }

        // Рисовать плавающий текст
        function drawFloatingTexts(offsetX, offsetY) {
            const ctx = game.ctx;
            
            game.floatingTexts.forEach(text => {
                const x = text.x + offsetX;
                const y = text.y + offsetY;
                const lifePercent = text.life / text.maxLife;
                
                ctx.save();
                ctx.globalAlpha = lifePercent;
                
                // Тень
                ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
                ctx.font = `bold ${text.size}px Arial`;
                ctx.textAlign = 'center';
                ctx.fillText(text.text, x + 1, y + 1);
                
                // Основной текст
                ctx.fillStyle = text.color;
                ctx.fillText(text.text, x, y);
                
                ctx.restore();
            });
        }

        // Рисовать короны
        function drawCrowns(offsetX, offsetY) {
            const ctx = game.ctx;
            
            game.crowns.forEach(crown => {
                const x = crown.x + offsetX;
                const y = crown.y + offsetY;
                const lifePercent = crown.life / 60;
                
                ctx.save();
                ctx.globalAlpha = lifePercent;
                ctx.translate(x, y);
                ctx.rotate(crown.rotation);
                
                // Корона
                ctx.fillStyle = '#FFD700';
                ctx.font = `${crown.size}px Arial`;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText('👑', 0, 0);
                
                // Свечение
                ctx.shadowColor = '#FFD700';
                ctx.shadowBlur = 10;
                ctx.fillText('👑', 0, 0);
                ctx.shadowBlur = 0;
                
                ctx.restore();
            });
        }

        // Вспомогательная функция для осветления цвета
        function lightenColor(color, percent) {
            const num = parseInt(color.slice(1), 16);
            const amt = Math.round(2.55 * percent);
            const R = Math.min(255, (num >> 16) + amt);
            const G = Math.min(255, (num >> 8 & 0x00FF) + amt);
            const B = Math.min(255, (num & 0x0000FF) + amt);
            
            return "#" + ((1 << 24) + (R << 16) + (G << 8) + B).toString(16).slice(1);
        }

        // Вспомогательная функция для затемнения цвета
        function darkenColor(color, percent) {
            const num = parseInt(color.slice(1), 16);
            const amt = Math.round(2.55 * percent);
            const R = Math.max(0, (num >> 16) - amt);
            const G = Math.max(0, (num >> 8 & 0x00FF) - amt);
            const B = Math.max(0, (num & 0x0000FF) - amt);
            
            return "#" + ((1 << 24) + (R << 16) + (G << 8) + B).toString(16).slice(1);
        }
    </script>
</body>
</html>
