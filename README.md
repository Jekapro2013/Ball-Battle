# Ball-Battle[Game .html](https://github.com/user-attachments/files/24402847/Game.html)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Шарики-Бойцы: Все против всех!</title>
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
            max-width: 900px;
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
            padding: 14px 28px;
            margin: 10px;
            border-radius: 12px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(76, 201, 240, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(76, 201, 240, 0.4);
        }
        
        button:hover::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            animation: shimmer 0.8s;
        }
        
        @keyframes shimmer {
            to { left: 100%; }
        }
        
        input {
            padding: 14px;
            margin: 12px;
            border-radius: 10px;
            border: 2px solid #4CC9F0;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            width: 300px;
            font-size: 16px;
            transition: all 0.3s;
        }
        
        input:focus {
            outline: none;
            border-color: #4361ee;
            box-shadow: 0 0 15px rgba(76, 201, 240, 0.5);
            transform: scale(1.02);
        }
        
        #gameCanvas {
            background: linear-gradient(135deg, #0a1931, #1a1a2e);
            border-radius: 15px;
            border: 4px solid #4CC9F0;
            margin: 20px auto;
            display: block;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }
        
        .stats {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(0, 0, 0, 0.85);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid #4CC9F0;
            min-width: 220px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
            backdrop-filter: blur(10px);
            animation: statsGlow 3s infinite alternate;
        }
        
        @keyframes statsGlow {
            from { box-shadow: 0 5px 15px rgba(76, 201, 240, 0.3); }
            to { box-shadow: 0 5px 25px rgba(76, 201, 240, 0.6); }
        }
        
        .weapon-info {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(0, 0, 0, 0.85);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid #ff6b6b;
            min-width: 220px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
            backdrop-filter: blur(10px);
        }
        
        .game-container {
            position: relative;
            display: inline-block;
        }
        
        h1 {
            color: #4CC9F0;
            margin-bottom: 30px;
            text-shadow: 0 2px 10px rgba(76, 201, 240, 0.5);
            font-size: 2.8em;
            background: linear-gradient(45deg, #4CC9F0, #4361ee);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: titleGlow 2s infinite alternate;
        }
        
        @keyframes titleGlow {
            from { filter: drop-shadow(0 0 10px rgba(76, 201, 240, 0.5)); }
            to { filter: drop-shadow(0 0 20px rgba(76, 201, 240, 0.8)); }
        }
        
        h2 {
            color: #4CC9F0;
            margin-bottom: 20px;
            font-size: 2em;
        }
        
        .error {
            color: #ff6b6b;
            background: rgba(255, 107, 107, 0.1);
            padding: 12px;
            border-radius: 8px;
            margin: 10px auto;
            max-width: 400px;
            display: none;
            animation: errorShake 0.5s;
        }
        
        @keyframes errorShake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        .health-bar {
            width: 120px;
            height: 12px;
            background: rgba(255, 0, 0, 0.3);
            border-radius: 6px;
            margin: 8px auto;
            overflow: hidden;
            position: relative;
        }
        
        .health-fill {
            height: 100%;
            background: linear-gradient(90deg, #00ff00, #00cc00);
            width: 100%;
            transition: width 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
            position: relative;
        }
        
        .health-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, 
                transparent, 
                rgba(255, 255, 255, 0.3), 
                transparent);
            animation: healthShine 2s infinite;
        }
        
        @keyframes healthShine {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }
        
        .instructions {
            background: rgba(0, 0, 0, 0.7);
            padding: 25px;
            border-radius: 15px;
            margin: 20px auto;
            max-width: 600px;
            border: 2px solid #4CC9F0;
            backdrop-filter: blur(10px);
        }
        
        .instructions h3 {
            color: #4CC9F0;
            margin-bottom: 15px;
            font-size: 1.4em;
        }
        
        .weapon-icon {
            display: inline-block;
            margin: 0 5px;
            animation: weaponFloat 3s infinite ease-in-out;
        }
        
        @keyframes weaponFloat {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
        
        .game-message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.9);
            padding: 30px 50px;
            border-radius: 20px;
            border: 3px solid #4CC9F0;
            font-size: 28px;
            display: none;
            z-index: 100;
            animation: messagePop 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
            backdrop-filter: blur(10px);
            box-shadow: 0 0 50px rgba(76, 201, 240, 0.5);
        }
        
        @keyframes messagePop {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }
        
        .floating-text {
            position: absolute;
            pointer-events: none;
            z-index: 50;
            font-weight: bold;
            animation: floatUp 1.5s ease-out forwards;
        }
        
        @keyframes floatUp {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-80px) scale(1.2); opacity: 0; }
        }
        
        .floating-coin {
            position: absolute;
            pointer-events: none;
            z-index: 40;
            animation: coinFloat 2s ease-out forwards;
        }
        
        @keyframes coinFloat {
            0% { transform: translateY(0) rotate(0deg); opacity: 1; }
            100% { transform: translateY(-100px) rotate(720deg); opacity: 0; }
        }
        
        .particle {
            position: absolute;
            pointer-events: none;
            z-index: 30;
            width: 8px;
            height: 8px;
            border-radius: 50%;
        }
        
        .particle-explosion {
            animation: particleExplode 1s ease-out forwards;
        }
        
        @keyframes particleExplode {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(1) translate(var(--tx), var(--ty)); opacity: 0; }
        }
        
        .trail {
            position: absolute;
            pointer-events: none;
            z-index: 20;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            animation: trailFade 0.6s ease-out forwards;
        }
        
        @keyframes trailFade {
            0% { transform: scale(1); opacity: 0.7; }
            100% { transform: scale(0.1); opacity: 0; }
        }
        
        .shield-effect {
            position: absolute;
            pointer-events: none;
            z-index: 25;
            border-radius: 50%;
            animation: shieldPulse 0.5s ease-out forwards;
        }
        
        @keyframes shieldPulse {
            0% { transform: scale(0.5); opacity: 0.8; }
            100% { transform: scale(2); opacity: 0; }
        }
        
        .heal-effect {
            position: absolute;
            pointer-events: none;
            z-index: 25;
            animation: healRise 1s ease-out forwards;
        }
        
        @keyframes healRise {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-60px) scale(1.5); opacity: 0; }
        }
        
        .level-up {
            position: absolute;
            pointer-events: none;
            z-index: 60;
            animation: levelUpPop 1s ease-out forwards;
        }
        
        @keyframes levelUpPop {
            0% { transform: scale(0) rotate(0deg); opacity: 0; }
            50% { transform: scale(1.5) rotate(180deg); opacity: 1; }
            100% { transform: scale(1) rotate(360deg); opacity: 0; }
        }
        
        .combo-text {
            position: absolute;
            pointer-events: none;
            z-index: 70;
            font-size: 36px;
            font-weight: bold;
            text-shadow: 0 0 10px currentColor;
            animation: comboBounce 1s cubic-bezier(0.68, -0.55, 0.27, 1.55) forwards;
        }
        
        @keyframes comboBounce {
            0% { transform: scale(0) translateY(0); opacity: 0; }
            50% { transform: scale(1.5) translateY(-20px); opacity: 1; }
            100% { transform: scale(1) translateY(-40px); opacity: 0; }
        }
        
        .screen-shake {
            animation: screenShake 0.3s ease-out;
        }
        
        @keyframes screenShake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            50% { transform: translateX(10px); }
            75% { transform: translateX(-5px); }
        }
        
        .slow-motion {
            animation: slowMotion 2s ease-out;
        }
        
        @keyframes slowMotion {
            0% { filter: brightness(1) blur(0); }
            50% { filter: brightness(1.5) blur(2px); }
            100% { filter: brightness(1) blur(0); }
        }
        
        .power-up-indicator {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            padding: 10px 20px;
            border-radius: 10px;
            border: 2px solid gold;
            display: none;
            animation: powerUpGlow 1s infinite alternate;
        }
        
        @keyframes powerUpGlow {
            from { box-shadow: 0 0 10px gold; }
            to { box-shadow: 0 0 20px gold; }
        }
    </style>
</head>
<body>
    <!-- Экран меню -->
    <div id="menuScreen" class="screen active">
        <h1>⚔️ ШАРИКИ-БОЙЦЫ ⚔️</h1>
        <p style="font-size: 1.2em; margin-bottom: 20px; color: #4CC9F0;">Все против всех! Только оружие наносит урон!</p>
        
        <div id="errorMessage" class="error"></div>
        
        <div style="margin: 30px 0;">
            <input type="text" id="playerName" placeholder="Введите ваше имя" maxlength="15" value="Герой">
        </div>
        
        <div style="margin: 30px 0;">
            <button onclick="startGame()" style="background: linear-gradient(45deg, #FF416C, #FF4B2B);">
                ⚔️ НАЧАТЬ БИТВУ
            </button>
            <button onclick="toggleInstructions()">📖 ИНСТРУКЦИИ</button>
        </div>
        
        <div id="instructions" class="instructions" style="display: none;">
            <h3>🎮 КАК ИГРАТЬ:</h3>
            <p>• Шарики летают автоматически</p>
            <p>• Собирайте оружие для нанесения урона</p>
            <p>• Без оружия урон НЕ наносится</p>
            <p>• Боты атакуют всех подряд</p>
            <p>• Оружие одноразовое</p>
            <p>• Последний выживший побеждает!</p>
            
            <div style="margin-top: 20px; display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px;">
                <div style="background: rgba(128, 128, 128, 0.2); padding: 10px; border-radius: 8px;">
                    <div style="font-size: 24px;">🔪</div>
                    <div>Нож</div>
                    <div style="color: #ff6b6b; font-weight: bold;">10 урона</div>
                </div>
                <div style="background: rgba(139, 69, 19, 0.2); padding: 10px; border-radius: 8px;">
                    <div style="font-size: 24px;">🔫</div>
                    <div>Пистолет</div>
                    <div style="color: #ff6b6b; font-weight: bold;">15 урона</div>
                </div>
                <div style="background: rgba(34, 139, 34, 0.2); padding: 10px; border-radius: 8px;">
                    <div style="font-size: 24px;">💣</div>
                    <div>Граната</div>
                    <div style="color: #ff6b6b; font-weight: bold;">25 урона</div>
                </div>
            </div>
            
            <button onclick="toggleInstructions()" style="margin-top: 20px;">СКРЫТЬ</button>
        </div>
        
        <div style="margin-top: 40px; font-size: 1.2em; background: rgba(0,0,0,0.3); padding: 20px; border-radius: 15px;">
            <p>💰 Всего монет: <span id="totalCoinsDisplay" style="color: gold;">0</span></p>
            <p>🎯 Всего убийств: <span id="totalKillsDisplay" style="color: #ff6b6b;">0</span></p>
            <p>🏆 Рекорд убийств: <span id="recordDisplay" style="color: #4CC9F0;">0</span></p>
            <p>👑 Побед: <span id="winsDisplay" style="color: #FFD700;">0</span></p>
        </div>
    </div>

    <!-- Экран игры -->
    <div id="gameScreen" class="screen">
        <div class="game-container">
            <div id="powerUpIndicator" class="power-up-indicator">
                ⚡ СИЛОВОЕ ПОЛЕ АКТИВНО!
            </div>
            
            <div class="stats">
                <p style="color: #4CC9F0; font-weight: bold;">👤 <span id="playerNameDisplay"></span></p>
                <p>❤️ ЗДОРОВЬЕ: <span id="healthDisplay">100</span></p>
                <div class="health-bar">
                    <div id="healthBarFill" class="health-fill"></div>
                </div>
                <p>💰 МОНЕТЫ: <span id="coinsDisplay" style="color: gold;">0</span></p>
                <p>🎯 ВРАГОВ: <span id="enemiesDisplay" style="color: #ff6b6b;">8</span></p>
                <p>⚔️ УБИЙСТВ: <span id="killsDisplay" style="color: #4CC9F0;">0</span></p>
                <p>🔥 СЕРИЯ: <span id="comboDisplay" style="color: #FF416C;">0</span></p>
            </div>
            
            <div class="weapon-info">
                <p>🔫 ОРУЖИЕ: <span id="weaponDisplay" style="color: #ff6b6b; font-weight: bold;">НЕТ</span></p>
                <p>🎯 ПАТРОНЫ: <span id="ammoDisplay" style="color: #4CC9F0;">-</span></p>
                <p>💥 УРОН: <span id="damageDisplay" style="color: #ff6b6b;">-</span></p>
                <p>⏱️ ПЕРЕЗАРЯДКА: <span id="cooldownDisplay">-</span></p>
            </div>
            
            <canvas id="gameCanvas" width="800" height="600"></canvas>
            
            <div style="margin: 20px;">
                <button onclick="togglePause()" id="pauseBtn">⏸️ ПАУЗА</button>
                <button onclick="exitToMenu()" style="background: linear-gradient(45deg, #ff6b6b, #ee5a52);">
                    🏠 В МЕНЮ
                </button>
                <button onclick="addBot()" style="background: linear-gradient(45deg, #06d6a0, #118ab2);">
                    🤖 +БОТ
                </button>
            </div>
            
            <div id="gameMessage" class="game-message"></div>
        </div>
    </div>

    <!-- Экран паузы -->
    <div id="pauseScreen" class="screen">
        <h2>⏸️ ИГРА НА ПАУЗЕ</h2>
        <div style="margin: 30px 0;">
            <button onclick="togglePause()" style="background: linear-gradient(45deg, #4CAF50, #2E7D32);">
                ▶️ ПРОДОЛЖИТЬ
            </button>
            <button onclick="exitToMenu()">🏠 В МЕНЮ</button>
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
            coins: 0,
            kills: 0,
            totalKills: 0,
            wins: 0,
            record: 0,
            playerName: 'Герой',
            gameTime: 0,
            combo: 0,
            comboTime: 0,
            lastComboTime: 0,
            powerUpActive: false,
            powerUpEndTime: 0,
            screenShake: 0
        };

        // Типы оружия
        const WEAPON_TYPES = {
            KNIFE: { name: '🔪 Нож', color: '#808080', damage: 10, ammo: 1, range: 60, speed: 1.0 },
            PISTOL: { name: '🔫 Пистолет', color: '#8B4513', damage: 15, ammo: 1, range: 120, speed: 0.8 },
            GRENADE: { name: '💣 Граната', color: '#228B22', damage: 25, ammo: 1, range: 150, speed: 0.6 }
        };

        // Цвета для шариков
        const BALL_COLORS = [
            '#4CC9F0', '#4361ee', '#3a0ca3', '#7209b7', // Игрок и синие
            '#ff6b6b', '#ff9e6d', '#ffd166', '#ef476f', // Красные/оранжевые
            '#06d6a0', '#118ab2', '#073b4c', '#ff9e00', // Зеленые/бирюзовые
            '#9d4edd', '#c77dff', '#e0aaff', '#ff5d8f'  // Фиолетовые/розовые
        ];

        // Инициализация при загрузке
        window.onload = function() {
            console.log('Игра загружается...');
            
            // Загружаем сохраненные данные
            loadGameData();
            
            // Инициализируем canvas
            game.canvas = document.getElementById('gameCanvas');
            game.ctx = game.canvas.getContext('2d');
            
            console.log('Игра готова');
        };

        // Загрузить данные игры
        function loadGameData() {
            const savedData = localStorage.getItem('ballFightersUltimate');
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

        // Сохранить данные игры
        function saveGameData() {
            const data = {
                coins: game.coins,
                totalKills: game.totalKills,
                record: game.record,
                wins: game.wins,
                playerName: game.playerName
            };
            localStorage.setItem('ballFightersUltimate', JSON.stringify(data));
        }

        // Обновить статистику в меню
        function updateMenuStats() {
            document.getElementById('totalCoinsDisplay').textContent = game.coins;
            document.getElementById('totalKillsDisplay').textContent = game.totalKills;
            document.getElementById('recordDisplay').textContent = game.record;
            document.getElementById('winsDisplay').textContent = game.wins;
        }

        // Показать/скрыть инструкции
        function toggleInstructions() {
            const instructions = document.getElementById('instructions');
            instructions.style.display = instructions.style.display === 'none' ? 'block' : 'none';
        }

        // Начать игру
        function startGame() {
            const nameInput = document.getElementById('playerName').value.trim();
            if (!nameInput) {
                showError('Введите имя игрока');
                return;
            }
            
            game.playerName = nameInput;
            
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
                radius: 35,
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
                shield: false,
                shieldEndTime: 0,
                killStreak: 0
            };
            
            // Создаем врагов
            createEnemies(8);
            
            // Сбрасываем оружие и эффекты
            game.weapons = [];
            game.effects = [];
            game.particles = [];
            game.trails = [];
            game.floatingTexts = [];
            game.kills = 0;
            game.combo = 0;
            game.gameTime = 0;
            game.powerUpActive = false;
            game.screenShake = 0;
            
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
                    // Шанс на спавн баффа
                    if (Math.random() < 0.1) {
                        spawnPowerUp();
                    }
                }
            }, 4000); // Новое оружие каждые 4 секунды
            
            console.log('Игра началась');
        }

        // Создать врагов
        function createEnemies(count) {
            game.enemies = [];
            
            for (let i = 0; i < count; i++) {
                const angle = (i / count) * Math.PI * 2;
                const distance = 180;
                
                game.enemies.push({
                    x: Math.cos(angle) * distance,
                    y: Math.sin(angle) * distance,
                    radius: 30 + Math.random() * 10,
                    color: BALL_COLORS[4 + (i % 12)],
                    health: 100,
                    maxHealth: 100,
                    speed: 2.5 + Math.random() * 2,
                    vx: (Math.random() - 0.5) * 3.5,
                    vy: (Math.random() - 0.5) * 3.5,
                    name: ['Злой', 'Хитрый', 'Сильный', 'Быстрый', 'Умный', 'Страшный', 'Ловкий', 'Жестокий'][i % 8] + ' Бот',
                    weapon: null,
                    isPlayer: false,
                    aiTimer: 0,
                    target: null,
                    aggression: 0.5 + Math.random() * 0.5,
                    lastAttackTime: 0,
                    attackCooldown: 1000 + Math.random() * 2000,
                    lastHitTime: 0,
                    damageEffect: 0,
                    trailTimer: 0,
                    killStreak: 0
                });
            }
        }

        // Добавить бота
        function addBot() {
            if (!game.running || game.paused || game.enemies.length >= 15) return;
            
            const newBot = {
                x: (Math.random() - 0.5) * 300,
                y: (Math.random() - 0.5) * 300,
                radius: 30 + Math.random() * 10,
                color: BALL_COLORS[4 + Math.floor(Math.random() * 12)],
                health: 100,
                maxHealth: 100,
                speed: 2.5 + Math.random() * 2,
                vx: (Math.random() - 0.5) * 3.5,
                vy: (Math.random() - 0.5) * 3.5,
                name: ['Новый', 'Свежий', 'Дополнительный', 'Экстра'][Math.floor(Math.random() * 4)] + ' Бот',
                weapon: null,
                isPlayer: false,
                aiTimer: 0,
                target: null,
                aggression: 0.5 + Math.random() * 0.5,
                lastAttackTime: 0,
                attackCooldown: 1000 + Math.random() * 2000,
                lastHitTime: 0,
                damageEffect: 0,
                trailTimer: 0,
                killStreak: 0
            };
            
            game.enemies.push(newBot);
            createSpawnEffect(newBot.x, newBot.y, newBot.color);
            updateGameUI();
        }

        // Спавн оружия
        function spawnWeapon() {
            if (!game.running || game.paused || game.weapons.length >= 8) return;
            
            const weaponTypes = Object.values(WEAPON_TYPES);
            const weaponType = weaponTypes[Math.floor(Math.random() * weaponTypes.length)];
            
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
                createSpawnEffect(x, y, weapon.color);
                
                // Перестаем мигать через 1.5 секунды
                setTimeout(() => {
                    const w = game.weapons.find(w => w.id === weapon.id);
                    if (w) w.blink = false;
                }, 1500);
            }
        }

        // Спавн баффа
        function spawnPowerUp() {
            if (!game.running || game.paused) return;
            
            const powerUps = [
                { type: 'shield', color: '#4CC9F0', name: '🛡️ Щит', duration: 10000 },
                { type: 'heal', color: '#4CAF50', name: '❤️ Лечение', amount: 30 },
                { type: 'speed', color: '#FF9800', name: '⚡ Скорость', multiplier: 1.5, duration: 8000 }
            ];
            
            const powerUp = powerUps[Math.floor(Math.random() * powerUps.length)];
            
            // Находим свободную позицию
            let x, y;
            let attempts = 0;
            const maxAttempts = 30;
            
            do {
                const angle = Math.random() * Math.PI * 2;
                const distance = Math.random() * 150;
                x = Math.cos(angle) * distance;
                y = Math.sin(angle) * distance;
                attempts++;
            } while (isTooCloseToEntity(x, y, 30) && attempts < maxAttempts);
            
            if (attempts < maxAttempts) {
                game.effects.push({
                    type: 'powerUp',
                    x: x,
                    y: y,
                    color: powerUp.color,
                    powerUp: powerUp,
                    life: 300, // 5 секунд при 60 FPS
                    rotation: 0,
                    size: 25
                });
                
                createPowerUpSpawnEffect(x, y, powerUp.color);
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
            
            // Проверяем другие эффекты
            for (const effect of game.effects) {
                if (effect.type === 'powerUp') {
                    const dx = effect.x - x;
                    const dy = effect.y - y;
                    if (Math.sqrt(dx*dx + dy*dy) < radius + 25) {
                        return true;
                    }
                }
            }
            
            return false;
        }

        // Игровой цикл
        function gameLoop() {
            if (!game.running || game.paused) return;
            
            // Увеличиваем время игры
            game.gameTime++;
            
            // Обновление комбо
            updateCombo();
            
            // Обновление
            updateGame();
            
            // Отрисовка
            drawGame();
            
            // Следующий кадр
            requestAnimationFrame(gameLoop);
        }

        // Обновить комбо
        function updateCombo() {
            const now = Date.now();
            if (now - game.lastComboTime > 3000) { // 3 секунды без убийств сбрасывает комбо
                if (game.combo > 1) {
                    createComboResetEffect();
                }
                game.combo = 0;
            }
            game.comboTime = now;
        }

        // Обновление игры
        function updateGame() {
            // Обновление всех сущностей
            updateEntity(game.player);
            game.enemies.forEach(enemy => updateEntity(enemy));
            
            // Обновление ИИ ботов
            updateBotAI();
            
            // Проверка сбора оружия и баффов
            checkPickups();
            
            // Проверка использования оружия
            checkWeaponUsage();
            
            // Проверка столкновений (без урона!)
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
            
            // Ограничение в пределах арены
            keepInArena(entity);
            
            // Случайное изменение направления
            if (Math.random() < 0.01) {
                entity.vx += (Math.random() - 0.5) * 0.3;
                entity.vy += (Math.random() - 0.5) * 0.3;
                
                // Ограничение скорости
                const speed = Math.sqrt(entity.vx*entity.vx + entity.vy*entity.vy);
                const maxSpeed = entity.shield ? entity.speed * 1.5 : entity.speed;
                if (speed > maxSpeed) {
                    entity.vx = (entity.vx / speed) * maxSpeed;
                    entity.vy = (entity.vy / speed) * maxSpeed;
                }
            }
            
            // Постепенно уменьшаем эффект урона
            if (entity.damageEffect > 0) {
                entity.damageEffect -= 0.03;
                if (entity.damageEffect < 0) entity.damageEffect = 0;
            }
            
            // Обновление щита
            if (entity.shield && Date.now() > entity.shieldEndTime) {
                entity.shield = false;
                createShieldEndEffect(entity);
            }
            
            // Создание следов
            entity.trailTimer++;
            if (entity.trailTimer >= 3) {
                entity.trailTimer = 0;
                createTrail(entity.x, entity.y, entity.color, entity.radius * 0.7);
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
                    
                    if (distance < enemy.weapon.range) {
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
                }
                
                // Избегание других ботов
                game.enemies.forEach(other => {
                    if (other !== enemy) {
                        const dx = enemy.x - other.x;
                        const dy = enemy.y - other.y;
                        const distance = Math.sqrt(dx*dx + dy*dy);
                        const minDistance = enemy.radius + other.radius + 20;
                        
                        if (distance < minDistance) {
                            enemy.vx += (dx / distance) * 0.1;
                            enemy.vy += (dy / distance) * 0.1;
                        }
                    }
                });
                
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
            if (!game.player.shield) {
                targets.push({
                    entity: game.player,
                    distance: getDistance(bot, game.player),
                    priority: 2.0
                });
            }
            
            // Добавляем других ботов как цели
            game.enemies.forEach(enemy => {
                if (enemy !== bot && !enemy.shield) {
                    targets.push({
                        entity: enemy,
                        distance: getDistance(bot, enemy),
                        priority: 1.0
                    });
                }
            });
            
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

        // Ограничение в пределах арены
        function keepInArena(entity) {
            const arenaRadius = 250;
            const dist = Math.sqrt(entity.x * entity.x + entity.y * entity.y);
            
            if (dist + entity.radius > arenaRadius) {
                const angle = Math.atan2(entity.y, entity.x);
                entity.x = Math.cos(angle) * (arenaRadius - entity.radius);
                entity.y = Math.sin(angle) * (arenaRadius - entity.radius);
                
                // Отскок от стен
                const normalX = Math.cos(angle);
                const normalY = Math.sin(angle);
                const dot = entity.vx * normalX + entity.vy * normalY;
                
                entity.vx = entity.vx - 1.8 * dot * normalX;
                entity.vy = entity.vy - 1.8 * dot * normalY;
                
                // Уменьшаем скорость при отскоке
                entity.vx *= 0.85;
                entity.vy *= 0.85;
                
                // Эффект отскока
                createWallBounceEffect(entity.x, entity.y, angle, entity.color);
            }
        }

        // Проверка сбора предметов
        function checkPickups() {
            // Проверяем игрока
            checkEntityPickups(game.player);
            
            // Проверяем врагов
            game.enemies.forEach(enemy => {
                checkEntityPickups(enemy);
            });
        }

        // Проверка сбора предметов сущностью
        function checkEntityPickups(entity) {
            const now = Date.now();
            
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
                        createWeaponPickupEffect(entity, weapon);
                        
                        // Обновляем интерфейс для игрока
                        if (entity.isPlayer) {
                            updateWeaponUI();
                            createFloatingText('+ ' + weapon.name, entity.x, entity.y, weapon.color);
                        }
                    }
                    break;
                }
            }
            
            // Сбор баффов
            for (let i = game.effects.length - 1; i >= 0; i--) {
                const effect = game.effects[i];
                if (effect.type === 'powerUp') {
                    const dx = entity.x - effect.x;
                    const dy = entity.y - effect.y;
                    const dist = Math.sqrt(dx*dx + dy*dy);
                    
                    if (dist < entity.radius + 25) {
                        applyPowerUp(entity, effect.powerUp);
                        game.effects.splice(i, 1);
                        
                        // Эффект применения баффа
                        createPowerUpPickupEffect(entity, effect.powerUp);
                        break;
                    }
                }
            }
        }

        // Применить бафф
        function applyPowerUp(entity, powerUp) {
            const now = Date.now();
            
            switch(powerUp.type) {
                case 'shield':
                    entity.shield = true;
                    entity.shieldEndTime = now + powerUp.duration;
                    createShieldEffect(entity);
                    if (entity.isPlayer) {
                        game.powerUpActive = true;
                        game.powerUpEndTime = now + powerUp.duration;
                        document.getElementById('powerUpIndicator').style.display = 'block';
                        setTimeout(() => {
                            if (now >= game.powerUpEndTime) {
                                document.getElementById('powerUpIndicator').style.display = 'none';
                                game.powerUpActive = false;
                            }
                        }, powerUp.duration);
                    }
                    break;
                    
                case 'heal':
                    entity.health = Math.min(entity.maxHealth, entity.health + powerUp.amount);
                    createHealEffect(entity);
                    if (entity.isPlayer) {
                        createFloatingText('+' + powerUp.amount + ' HP', entity.x, entity.y, '#4CAF50');
                    }
                    break;
                    
                case 'speed':
                    const originalSpeed = entity.speed;
                    entity.speed *= powerUp.multiplier;
                    createSpeedEffect(entity);
                    if (entity.isPlayer) {
                        createFloatingText('⚡ СКОРОСТЬ!', entity.x, entity.y, '#FF9800');
                    }
                    setTimeout(() => {
                        entity.speed = originalSpeed;
                    }, powerUp.duration);
                    break;
            }
            
            if (entity.isPlayer) {
                createFloatingText(powerUp.name, entity.x, entity.y, powerUp.color);
            }
        }

        // Проверка использования оружия
        function checkWeaponUsage() {
            const now = Date.now();
            
            // Проверяем игрока (автоматическая атака)
            if (game.player.weapon && now - game.player.lastHitTime > 1000) {
                let closestEnemy = null;
                let minDist = game.player.weapon.range;
                
                game.enemies.forEach(enemy => {
                    if (!enemy.shield) {
                        const dx = game.player.x - enemy.x;
                        const dy = game.player.y - enemy.y;
                        const dist = Math.sqrt(dx*dx + dy*dy);
                        
                        if (dist < minDist) {
                            minDist = dist;
                            closestEnemy = enemy;
                        }
                    }
                });
                
                if (closestEnemy) {
                    useWeapon(game.player, closestEnemy);
                }
            }
            
            // Боты атакуют автоматически через updateBotAI
        }

        // Использовать оружие
        function useWeapon(attacker, target) {
            const now = Date.now();
            
            // Нельзя атаковать через щит
            if (target.shield) {
                createShieldBlockEffect(target);
                return;
            }
            
            // Наносим урон
            const damage = attacker.weapon.damage;
            target.health -= damage;
            
            // Эффекты урона
            createDamageEffect(target, damage);
            createAttackEffect(attacker, target);
            
            // Обнуляем оружие (оно одноразовое)
            attacker.weapon = null;
            attacker.lastHitTime = now;
            
            // Эффект разрушения оружия
            createWeaponBreakEffect(attacker);
            
            // Обновляем интерфейс для игрока
            if (attacker.isPlayer) {
                updateWeaponUI();
                
                // Увеличиваем комбо
                game.combo++;
                game.lastComboTime = now;
                if (game.combo > 1) {
                    createComboEffect(game.combo);
                }
            }
            
            // Убийство врага игроком
            if (target.health <= 0 && attacker.isPlayer) {
                attacker.killStreak++;
                if (attacker.killStreak > 1) {
                    createKillStreakEffect(attacker, attacker.killStreak);
                }
            }
        }

        // Проверка столкновений (без урона!)
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
                    
                    // Эффект столкновения (без урона!)
                    createCollisionEffect(game.player, enemy);
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
                        
                        // Эффект столкновения
                        createCollisionEffect(a, b);
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
                    } else {
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
                            killer.killStreak++;
                            
                            // Награда монетами
                            const coinReward = 2 + Math.floor(game.combo / 2);
                            game.coins += coinReward;
                            
                            // Эффекты
                            createCoinEffect(enemy, coinReward);
                            createKillEffect(killer, enemy);
                            
                            // Обновляем рекорд
                            if (game.kills > game.record) {
                                game.record = game.kills;
                                if (game.kills >= 5) {
                                    createNewRecordEffect();
                                }
                            }
                        } else {
                            // Бот убил другого бота
                            killer.killStreak++;
                            createBotKillEffect(killer, enemy);
                        }
                    }
                    
                    // Эффект смерти
                    createDeathEffect(enemy);
                    
                    // Удаляем врага
                    game.enemies.splice(i, 1);
                }
            }
        }

        // ========== ЭФФЕКТЫ И АНИМАЦИИ ==========

        // Создать эффект урона
        function createDamageEffect(target, damage) {
            // Текст урона
            createFloatingText('-' + damage, target.x, target.y - target.radius - 15, '#ff0000', 28);
            
            // Кровавые брызги
            for (let i = 0; i < 12; i++) {
                createParticle(
                    target.x, target.y,
                    '#ff0000',
                    (Math.random() - 0.5) * 8,
                    (Math.random() - 0.5) * 8,
                    2 + Math.random() * 4,
                    30 + Math.random() * 60
                );
            }
            
            // Эффект встряски
            target.damageEffect = 1;
            game.screenShake = 10;
            
            // Звуковой эффект (визуальный)
            createHitSoundWave(target.x, target.y, damage);
        }

        // Создать эффект атаки
        function createAttackEffect(attacker, target) {
            // Линия атаки
            createBeamEffect(attacker, target, attacker.weapon.color);
            
            // Вспышка на цели
            createFlashEffect(target.x, target.y, '#ffffff', 35, 15);
            
            // Частицы от атаки
            const angle = Math.atan2(target.y - attacker.y, target.x - attacker.x);
            for (let i = 0; i < 8; i++) {
                const speed = 3 + Math.random() * 4;
                createParticle(
                    attacker.x, attacker.y,
                    attacker.weapon.color,
                    Math.cos(angle) * speed + (Math.random() - 0.5) * 2,
                    Math.sin(angle) * speed + (Math.random() - 0.5) * 2,
                    3 + Math.random() * 3,
                    40 + Math.random() * 40
                );
            }
        }

        // Создать эффект подбора оружия
        function createWeaponPickupEffect(entity, weapon) {
            // Круговые волны
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createRingEffect(entity.x, entity.y, weapon.color, 20 + i * 15, 30);
                }, i * 100);
            }
            
            // Взрыв частиц
            for (let i = 0; i < 16; i++) {
                const angle = (i / 16) * Math.PI * 2;
                createParticle(
                    entity.x, entity.y,
                    weapon.color,
                    Math.cos(angle) * 3,
                    Math.sin(angle) * 3,
                    2 + Math.random() * 3,
                    50 + Math.random() * 50
                );
            }
        }

        // Создать эффект разрушения оружия
        function createWeaponBreakEffect(entity) {
            // Взрыв осколков
            for (let i = 0; i < 24; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 2 + Math.random() * 5;
                createParticle(
                    entity.x, entity.y,
                    entity.weapon ? entity.weapon.color : '#ffffff',
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    1 + Math.random() * 2,
                    40 + Math.random() * 40
                );
            }
            
            // Кольцевая волна
            createRingEffect(entity.x, entity.y, '#ffffff', 30, 20);
        }

        // Создать эффект смерти
        function createDeathEffect(entity) {
            // Большой взрыв
            for (let i = 0; i < 48; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 1 + Math.random() * 8;
                createParticle(
                    entity.x, entity.y,
                    entity.color,
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    2 + Math.random() * 6,
                    60 + Math.random() * 90
                );
            }
            
            // Волна смерти
            createRingEffect(entity.x, entity.y, entity.color, entity.radius * 2, 40);
            
            // Текст смерти
            createFloatingText('УНИЧТОЖЕН!', entity.x, entity.y, '#ff0000', 24);
        }

        // Создать эффект убийства
        function createKillEffect(killer, victim) {
            // Эффект вокруг убийцы
            createRingEffect(killer.x, killer.y, '#FFD700', killer.radius * 1.5, 30);
            
            // Текст убийства
            if (killer.isPlayer) {
                createFloatingText('УБИЙСТВО!', killer.x, killer.y, '#FFD700', 32);
            }
        }

        // Создать эффект убийства бота
        function createBotKillEffect(killer, victim) {
            // Маленький эффект для ботов
            createRingEffect(killer.x, killer.y, killer.color, killer.radius * 1.2, 20);
        }

        // Создать эффект комбо
        function createComboEffect(combo) {
            const x = game.player.x;
            const y = game.player.y;
            
            // Текст комбо
            createFloatingText('КОМБО x' + combo, x, y - 50, '#FF416C', 36 + combo * 2);
            
            // Эффект вокруг игрока
            for (let i = 0; i < combo * 2; i++) {
                setTimeout(() => {
                    createRingEffect(x, y, '#FF416C', 20 + i * 10, 15);
                }, i * 50);
            }
        }

        // Создать эффект серии убийств
        function createKillStreakEffect(entity, streak) {
            if (streak >= 3) {
                const texts = [
                    [3, '🔥 ГОРЯЧО!', '#FF9800'],
                    [5, '⚡ НЕОСТАНОВИМ!', '#4CC9F0'],
                    [7, '💀 УБИЙЦА!', '#ff0000'],
                    [10, '👑 ЛЕГЕНДА!', '#FFD700']
                ];
                
                for (const [minStreak, text, color] of texts) {
                    if (streak === minStreak) {
                        createFloatingText(text, entity.x, entity.y, color, 42);
                        createRingEffect(entity.x, entity.y, color, 50, 50);
                        game.screenShake = 15;
                        break;
                    }
                }
            }
        }

        // Создать эффект сброса комбо
        function createComboResetEffect() {
            createFloatingText('КОМБО СБРОШЕНО', game.player.x, game.player.y, '#808080', 24);
        }

        // Создать эффект нового рекорда
        function createNewRecordEffect() {
            createFloatingText('НОВЫЙ РЕКОРД!', game.player.x, game.player.y, '#FFD700', 48);
            
            // Фейерверк
            for (let i = 0; i < 20; i++) {
                setTimeout(() => {
                    const angle = Math.random() * Math.PI * 2;
                    const distance = 50 + Math.random() * 100;
                    const x = game.player.x + Math.cos(angle) * distance;
                    const y = game.player.y + Math.sin(angle) * distance;
                    
                    // Маленький взрыв
                    for (let j = 0; j < 12; j++) {
                        const a = Math.random() * Math.PI * 2;
                        const s = 1 + Math.random() * 4;
                        createParticle(
                            x, y,
                            ['#FFD700', '#FF9800', '#FF416C'][Math.floor(Math.random() * 3)],
                            Math.cos(a) * s,
                            Math.sin(a) * s,
                            2 + Math.random() * 3,
                            40 + Math.random() * 40
                        );
                    }
                }, i * 100);
            }
        }

        // Создать эффект монет
        function createCoinEffect(entity, amount) {
            // Текст монет
            createFloatingText('+' + amount + '💰', entity.x, entity.y, '#FFD700', 24);
            
            // Летящие монеты
            for (let i = 0; i < amount; i++) {
                setTimeout(() => {
                    createFloatingCoin(entity.x, entity.y);
                }, i * 100);
            }
        }

        // Создать эффект щита
        function createShieldEffect(entity) {
            // Кольца щита
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createRingEffect(entity.x, entity.y, '#4CC9F0', entity.radius + 10 + i * 20, 30);
                }, i * 200);
            }
            
            // Частицы щита
            for (let i = 0; i < 24; i++) {
                const angle = (i / 24) * Math.PI * 2;
                const distance = entity.radius + 15;
                createParticle(
                    entity.x + Math.cos(angle) * distance,
                    entity.y + Math.sin(angle) * distance,
                    '#4CC9F0',
                    0,
                    0,
                    2 + Math.random() * 2,
                    60 + Math.random() * 60
                );
            }
        }

        // Создать эффект окончания щита
        function createShieldEndEffect(entity) {
            createRingEffect(entity.x, entity.y, '#808080', entity.radius + 20, 20);
            createFloatingText('ЩИТ ПРОБИТ', entity.x, entity.y, '#808080', 20);
        }

        // Создать эффект блокировки щитом
        function createShieldBlockEffect(entity) {
            createRingEffect(entity.x, entity.y, '#4CC9F0', entity.radius + 15, 15);
            createFloatingText('БЛОК!', entity.x, entity.y, '#4CC9F0', 24);
        }

        // Создать эффект лечения
        function createHealEffect(entity) {
            // Поднимающиеся сердца
            for (let i = 0; i < 8; i++) {
                setTimeout(() => {
                    createFloatingText('❤️', 
                        entity.x + (Math.random() - 0.5) * 30, 
                        entity.y,
                        '#4CAF50',
                        20 + Math.random() * 10
                    );
                }, i * 100);
            }
            
            // Кольца лечения
            createRingEffect(entity.x, entity.y, '#4CAF50', entity.radius + 10, 30);
        }

        // Создать эффект скорости
        function createSpeedEffect(entity) {
            // Следы скорости
            for (let i = 0; i < 5; i++) {
                setTimeout(() => {
                    createTrail(entity.x, entity.y, '#FF9800', entity.radius * 0.5);
                }, i * 50);
            }
            
            // Вспышка
            createFlashEffect(entity.x, entity.y, '#FF9800', 40, 20);
        }

        // Создать эффект спавна
        function createSpawnEffect(x, y, color) {
            // Расширяющееся кольцо
            createRingEffect(x, y, color, 10, 40);
            
            // Взрыв частиц
            for (let i = 0; i < 16; i++) {
                const angle = (i / 16) * Math.PI * 2;
                createParticle(
                    x, y,
                    color,
                    Math.cos(angle) * 4,
                    Math.sin(angle) * 4,
                    2 + Math.random() * 3,
                    50 + Math.random() * 50
                );
            }
        }

        // Создать эффект спавна баффа
        function createPowerUpSpawnEffect(x, y, color) {
            // Мерцающее кольцо
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createRingEffect(x, y, color, 20 + i * 10, 20);
                }, i * 300);
            }
            
            // Вращающиеся частицы
            for (let i = 0; i < 12; i++) {
                const angle = (i / 12) * Math.PI * 2;
                const distance = 20;
                createParticle(
                    x + Math.cos(angle) * distance,
                    y + Math.sin(angle) * distance,
                    color,
                    0,
                    0,
                    3,
                    120
                );
            }
        }

        // Создать эффект подбора баффа
        function createPowerUpPickupEffect(entity, powerUp) {
            // Вспышка
            createFlashEffect(entity.x, entity.y, powerUp.color, 50, 25);
            
            // Текст баффа
            createFloatingText(powerUp.name, entity.x, entity.y, powerUp.color, 28);
            
            // Кольца
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createRingEffect(entity.x, entity.y, powerUp.color, 30 + i * 20, 20);
                }, i * 150);
            }
        }

        // Создать эффект столкновения
        function createCollisionEffect(a, b) {
            const midX = (a.x + b.x) / 2;
            const midY = (a.y + b.y) / 2;
            
            // Маленькая волна
            createRingEffect(midX, midY, '#ffffff', 20, 15);
            
            // Частицы
            for (let i = 0; i < 8; i++) {
                const angle = Math.atan2(b.y - a.y, b.x - a.x) + (Math.random() - 0.5) * 1;
                createParticle(
                    midX, midY,
                    '#ffffff',
                    Math.cos(angle) * 3,
                    Math.sin(angle) * 3,
                    1 + Math.random() * 2,
                    30 + Math.random() * 30
                );
            }
        }

        // Создать эффект отскока от стены
        function createWallBounceEffect(x, y, angle, color) {
            // Линия отскока
            createBeamEffect(
                {x: x, y: y},
                {x: x + Math.cos(angle) * 50, y: y + Math.sin(angle) * 50},
                color,
                15
            );
            
            // Частицы
            for (let i = 0; i < 6; i++) {
                const a = angle + Math.PI + (Math.random() - 0.5) * 0.5;
                createParticle(
                    x, y,
                    color,
                    Math.cos(a) * 4,
                    Math.sin(a) * 4,
                    1 + Math.random() * 2,
                    40 + Math.random() * 40
                );
            }
        }

        // Создать звуковую волну
        function createHitSoundWave(x, y, damage) {
            const intensity = Math.min(damage / 25, 1.5);
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createRingEffect(x, y, '#ffffff', 10 + i * 15 * intensity, 10 * intensity);
                }, i * 50);
            }
        }

        // Создать луч
        function createBeamEffect(from, to, color, duration = 20) {
            game.effects.push({
                type: 'beam',
                from: {x: from.x, y: from.y},
                to: {x: to.x, y: to.y},
                color: color,
                life: duration,
                maxLife: duration,
                width: 3
            });
        }

        // Создать кольцо
        function createRingEffect(x, y, color, radius, duration = 30) {
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

        // Создать вспышку
        function createFlashEffect(x, y, color, size, duration = 20) {
            game.effects.push({
                type: 'flash',
                x: x,
                y: y,
                color: color,
                size: size,
                life: duration,
                maxLife: duration
            });
        }

        // Создать след
        function createTrail(x, y, color, size) {
            game.trails.push({
                x: x,
                y: y,
                color: color,
                size: size,
                life: 30,
                maxLife: 30
            });
        }

        // Создать частицу
        function createParticle(x, y, color, vx, vy, size, life) {
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
        function createFloatingText(text, x, y, color, size = 20) {
            game.floatingTexts.push({
                text: text,
                x: x,
                y: y,
                color: color,
                size: size,
                life: 90,
                maxLife: 90,
                vy: -1
            });
        }

        // Создать летящую монету
        function createFloatingCoin(x, y) {
            const angle = Math.random() * Math.PI * 2;
            const distance = 30;
            game.floatingTexts.push({
                text: '💰',
                x: x + Math.cos(angle) * distance,
                y: y + Math.sin(angle) * distance,
                color: '#FFD700',
                size: 24,
                life: 120,
                maxLife: 120,
                vy: -0.8,
                rotation: 0,
                rotationSpeed: 0.1
            });
        }

        // Обновить эффекты
        function updateEffects() {
            for (let i = game.effects.length - 1; i >= 0; i--) {
                const effect = game.effects[i];
                effect.life--;
                
                // Обновление в зависимости от типа
                switch(effect.type) {
                    case 'powerUp':
                        effect.rotation += 0.05;
                        effect.y += Math.sin(game.gameTime * 0.1 + effect.x * 0.01) * 0.2;
                        break;
                        
                    case 'beam':
                    case 'ring':
                    case 'flash':
                        // Просто исчезают
                        break;
                }
                
                // Удаляем мертвые эффекты
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
                
                // Удаляем мертвые частицы
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
                
                // Удаляем мертвые следы
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
                
                if (text.rotation !== undefined) {
                    text.rotation += text.rotationSpeed || 0;
                }
                
                // Удаляем мертвый текст
                if (text.life <= 0) {
                    game.floatingTexts.splice(i, 1);
                }
            }
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
            if (game.screenShake > 0) {
                offsetX = (Math.random() - 0.5) * game.screenShake;
                offsetY = (Math.random() - 0.5) * game.screenShake;
            }
            
            // Смещение для центрирования
            const centerX = canvas.width / 2 + offsetX;
            const centerY = canvas.height / 2 + offsetY;
            
            // Рисуем арену
            drawArena(centerX, centerY);
            
            // Рисуем следы
            drawTrails(centerX, centerY);
            
            // Рисуем эффекты
            drawEffects(centerX, centerY);
            
            // Рисуем оружие на земле
            drawWeapons(centerX, centerY);
            
            // Рисуем баффы
            drawPowerUps(centerX, centerY);
            
            // Рисуем врагов
            game.enemies.forEach(enemy => {
                drawEntity(enemy, centerX, centerY);
            });
            
            // Рисуем игрока
            drawEntity(game.player, centerX, centerY);
            
            // Рисуем частицы
            drawParticles(centerX, centerY);
            
            // Рисуем плавающий текст
            drawFloatingTexts(centerX, centerY);
        }

        // Рисовать арену
        function drawArena(offsetX, offsetY) {
            const ctx = game.ctx;
            const arenaRadius = 250;
            
            // Фон арены с градиентом
            const gradient = ctx.createRadialGradient(
                offsetX, offsetY, arenaRadius * 0.3,
                offsetX, offsetY, arenaRadius
            );
            gradient.addColorStop(0, 'rgba(10, 25, 49, 0.9)');
            gradient.addColorStop(0.5, 'rgba(15, 30, 60, 0.8)');
            gradient.addColorStop(1, 'rgba(20, 35, 70, 0.7)');
            
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(offsetX, offsetY, arenaRadius, 0, Math.PI * 2);
            ctx.fill();
            
            // Сетка арены
            ctx.strokeStyle = 'rgba(76, 201, 240, 0.08)';
            ctx.lineWidth = 1;
            
            // Концентрические круги
            for (let i = 1; i <= 5; i++) {
                const radius = arenaRadius * (i / 5);
                ctx.beginPath();
                ctx.arc(offsetX, offsetY, radius, 0, Math.PI * 2);
                ctx.stroke();
            }
            
            // Радиальные линии
            for (let i = 0; i < 12; i++) {
                const angle = (i / 12) * Math.PI * 2;
                ctx.beginPath();
                ctx.moveTo(offsetX, offsetY);
                ctx.lineTo(
                    offsetX + Math.cos(angle) * arenaRadius,
                    offsetY + Math.sin(angle) * arenaRadius
                );
                ctx.stroke();
            }
            
            // Анимированные точки на границе
            const time = Date.now() * 0.001;
            for (let i = 0; i < 24; i++) {
                const angle = (i / 24) * Math.PI * 2 + time;
                const pulse = Math.sin(time * 2 + i * 0.5) * 0.5 + 0.5;
                
                ctx.fillStyle = `rgba(76, 201, 240, ${0.3 + pulse * 0.4})`;
                ctx.beginPath();
                ctx.arc(
                    offsetX + Math.cos(angle) * arenaRadius,
                    offsetY + Math.sin(angle) * arenaRadius,
                    2 + pulse * 2,
                    0, Math.PI * 2
                );
                ctx.fill();
            }
            
            // Граница арены
            ctx.strokeStyle = '#4CC9F0';
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.arc(offsetX, offsetY, arenaRadius, 0, Math.PI * 2);
            ctx.stroke();
            
            // Свечение границы
            ctx.shadowColor = '#4CC9F0';
            ctx.shadowBlur = 20;
            ctx.stroke();
            ctx.shadowBlur = 0;
            
            // Центральная точка
            const centerPulse = Math.sin(Date.now() * 0.002) * 0.3 + 0.7;
            ctx.fillStyle = `rgba(76, 201, 240, ${0.5 * centerPulse})`;
            ctx.beginPath();
            ctx.arc(offsetX, offsetY, 10 * centerPulse, 0, Math.PI * 2);
            ctx.fill();
        }

        // Рисовать сущность
        function drawEntity(entity, offsetX, offsetY) {
            const ctx = game.ctx;
            const x = entity.x + offsetX;
            const y = entity.y + offsetY;
            const time = Date.now() * 0.001;
            
            // Эффект получения урона (красное свечение)
            if (entity.damageEffect > 0) {
                ctx.save();
                ctx.globalAlpha = entity.damageEffect * 0.6;
                const gradient = ctx.createRadialGradient(x, y, 0, x, y, entity.radius + 10);
                gradient.addColorStop(0, 'rgba(255, 0, 0, 0.8)');
                gradient.addColorStop(1, 'transparent');
                ctx.fillStyle = gradient;
                ctx.beginPath();
                ctx.arc(x, y, entity.radius + 10, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
            
            // Щит
            if (entity.shield) {
                ctx.save();
                const shieldPulse = Math.sin(time * 3) * 0.2 + 0.8;
                ctx.globalAlpha = 0.4;
                ctx.strokeStyle = '#4CC9F0';
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.arc(x, y, entity.radius + 8, 0, Math.PI * 2);
                ctx.stroke();
                
                // Вращающиеся щитовые сегменты
                for (let i = 0; i < 6; i++) {
                    const angle = time + (i / 6) * Math.PI * 2;
                    ctx.beginPath();
                    ctx.arc(
                        x + Math.cos(angle) * (entity.radius + 8),
                        y + Math.sin(angle) * (entity.radius + 8),
                        4 * shieldPulse,
                        0, Math.PI * 2
                    );
                    ctx.fillStyle = '#4CC9F0';
                    ctx.fill();
                }
                ctx.restore();
            }
            
            // Тень
            ctx.fillStyle = 'rgba(0, 0, 0, 0.4)';
            ctx.beginPath();
            ctx.arc(x, y + 5, entity.radius * 0.9, 0, Math.PI * 2);
            ctx.fill();
            
            // Основной круг с градиентом
            const gradient = ctx.createRadialGradient(
                x - entity.radius/3, y - entity.radius/3, 0,
                x, y, entity.radius
            );
            gradient.addColorStop(0, lightenColor(entity.color, 40));
            gradient.addColorStop(0.7, entity.color);
            gradient.addColorStop(1, darkenColor(entity.color, 20));
            
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(x, y, entity.radius, 0, Math.PI * 2);
            ctx.fill();
            
            // Блики
            ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
            ctx.beginPath();
            ctx.ellipse(
                x - entity.radius/3, 
                y - entity.radius/3, 
                entity.radius/3, 
                entity.radius/4, 
                0, 0, Math.PI * 2
            );
            ctx.fill();
            
            // Контур
            ctx.strokeStyle = entity.isPlayer ? '#4CC9F0' : darkenColor(entity.color, 30);
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.arc(x, y, entity.radius, 0, Math.PI * 2);
            ctx.stroke();
            
            // Иконка оружия (если есть)
            if (entity.weapon) {
                ctx.save();
                ctx.translate(x, y);
                
                // Вращающаяся иконка
                const iconAngle = time * 2;
                const iconDistance = entity.radius + 15;
                const iconX = Math.cos(iconAngle) * iconDistance;
                const iconY = Math.sin(iconAngle) * iconDistance;
                
                // Фон иконки
                ctx.fillStyle = entity.weapon.color;
                ctx.beginPath();
                ctx.arc(iconX, iconY, 12, 0, Math.PI * 2);
                ctx.fill();
                
                // Обводка иконки
                ctx.strokeStyle = '#ffffff';
                ctx.lineWidth = 2;
                ctx.stroke();
                
                // Свечение
                ctx.shadowColor = entity.weapon.color;
                ctx.shadowBlur = 10;
                ctx.fill();
                ctx.shadowBlur = 0;
                
                ctx.restore();
            }
            
            // Имя
            ctx.fillStyle = '#fff';
            ctx.font = `bold ${entity.isPlayer ? 16 : 14}px Arial`;
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(entity.name, x, y);
            
            // Серия убийств
            if (entity.killStreak >= 3) {
                ctx.font = 'bold 12px Arial';
                ctx.fillStyle = ['#FF9800', '#4CC9F0', '#ff0000', '#FFD700'][
                    Math.min(Math.floor(entity.killStreak / 3), 3)
                ];
                ctx.fillText('x' + entity.killStreak, x, y + entity.radius + 15);
            }
            
            // Полоса здоровья
            drawHealthBar(entity, x, y);
        }

        // Рисовать полосу здоровья
        function drawHealthBar(entity, x, y) {
            const ctx = game.ctx;
            const barWidth = 80;
            const barHeight = 8;
            const barX = x - barWidth / 2;
            const barY = y - entity.radius - 20;
            const healthPercent = entity.health / entity.maxHealth;
            
            // Фон
            ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
            ctx.fillRect(barX, barY, barWidth, barHeight);
            
            // Здоровье с анимацией
            const currentWidth = barWidth * healthPercent;
            
            // Градиент здоровья
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
            
            // Анимация низкого здоровья
            if (healthPercent < 0.3) {
                const pulse = Math.sin(Date.now() * 0.01) * 0.3 + 0.7;
                ctx.globalAlpha = pulse;
                ctx.fillStyle = '#ff0000';
                ctx.fillRect(barX, barY, currentWidth, barHeight);
                ctx.globalAlpha = 1;
            }
            
            // Рамка
            ctx.strokeStyle = '#000';
            ctx.lineWidth = 1;
            ctx.strokeRect(barX, barY, barWidth, barHeight);
            
            // Текст здоровья (только если не полное)
            if (healthPercent < 1) {
                ctx.fillStyle = '#fff';
                ctx.font = 'bold 11px Arial';
                ctx.textAlign = 'center';
                ctx.fillText(Math.ceil(entity.health), x, barY - 6);
            }
        }

        // Рисовать оружие на земле
        function drawWeapons(offsetX, offsetY) {
            const ctx = game.ctx;
            const time = Date.now() * 0.001;
            
            game.weapons.forEach(weapon => {
                const x = weapon.x + offsetX;
                const y = weapon.y + offsetY;
                
                // Плавающая анимация
                const floatY = y + Math.sin(time + weapon.floatOffset) * 5;
                
                // Мигание нового оружия
                if (weapon.blink) {
                    const blink = Math.sin(time * 10) * 0.5 + 0.5;
                    ctx.globalAlpha = blink;
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
                    ctx.beginPath();
                    ctx.arc(x, floatY, 30, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.globalAlpha = 1;
                }
                
                // Вращение
                ctx.save();
                ctx.translate(x, floatY);
                weapon.rotation += 0.02;
                ctx.rotate(weapon.rotation);
                
                // Иконка оружия
                const gradient = ctx.createRadialGradient(0, 0, 0, 0, 0, 20);
                gradient.addColorStop(0, lightenColor(weapon.color, 30));
                gradient.addColorStop(1, weapon.color);
                
                ctx.fillStyle = gradient;
                ctx.beginPath();
                ctx.arc(0, 0, 20, 0, Math.PI * 2);
                ctx.fill();
                
                // Обводка
                ctx.strokeStyle = '#fff';
                ctx.lineWidth = 3;
                ctx.stroke();
                
                // Свечение
                ctx.shadowColor = weapon.color;
                ctx.shadowBlur = 15;
                ctx.fill();
                ctx.shadowBlur = 0;
                
                // Детали оружия
                ctx.fillStyle = darkenColor(weapon.color, 30);
                ctx.fillRect(-8, -5, 16, 10);
                
                ctx.restore();
                
                // Название
                ctx.fillStyle = '#fff';
                ctx.font = 'bold 14px Arial';
                ctx.textAlign = 'center';
                ctx.fillText(weapon.name, x, floatY + 35);
                
                // Урон
                ctx.font = '12px Arial';
                ctx.fillStyle = '#ff6b6b';
                ctx.fillText(weapon.damage + ' урона', x, floatY + 50);
            });
        }

        // Рисовать баффы
        function drawPowerUps(offsetX, offsetY) {
            const ctx = game.ctx;
            const time = Date.now() * 0.001;
            
            game.effects.forEach(effect => {
                if (effect.type === 'powerUp') {
                    const x = effect.x + offsetX;
                    const y = effect.y + offsetY + Math.sin(time * 2 + effect.x * 0.01) * 10;
                    
                    ctx.save();
                    ctx.translate(x, y);
                    ctx.rotate(effect.rotation);
                    
                    // Фон баффа
                    const gradient = ctx.createRadialGradient(0, 0, 0, 0, 0, effect.size);
                    gradient.addColorStop(0, lightenColor(effect.color, 40));
                    gradient.addColorStop(1, effect.color);
                    
                    ctx.fillStyle = gradient;
                    ctx.beginPath();
                    ctx.arc(0, 0, effect.size, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Обводка
                    ctx.strokeStyle = '#fff';
                    ctx.lineWidth = 3;
                    ctx.stroke();
                    
                    // Свечение
                    ctx.shadowColor = effect.color;
                    ctx.shadowBlur = 20;
                    ctx.fill();
                    ctx.shadowBlur = 0;
                    
                    // Иконка в зависимости от типа
                    ctx.fillStyle = '#fff';
                    ctx.font = '20px Arial';
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'middle';
                    
                    const icon = {
                        'shield': '🛡️',
                        'heal': '❤️',
                        'speed': '⚡'
                    }[effect.powerUp.type];
                    
                    ctx.fillText(icon, 0, 0);
                    
                    ctx.restore();
                    
                    // Название баффа
                    ctx.fillStyle = '#fff';
                    ctx.font = 'bold 12px Arial';
                    ctx.textAlign = 'center';
                    ctx.fillText(effect.powerUp.name, x, y + effect.size + 20);
                }
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
                if (effect.type === 'powerUp') return; // Уже отрисованы
                
                const x = effect.x + offsetX;
                const y = effect.y + offsetY;
                const lifePercent = effect.life / effect.maxLife;
                
                ctx.save();
                
                switch(effect.type) {
                    case 'beam':
                        ctx.globalAlpha = lifePercent;
                        ctx.strokeStyle = effect.color;
                        ctx.lineWidth = effect.width;
                        ctx.lineCap = 'round';
                        ctx.beginPath();
                        ctx.moveTo(effect.from.x + offsetX, effect.from.y + offsetY);
                        ctx.lineTo(effect.to.x + offsetX, effect.to.y + offsetY);
                        ctx.stroke();
                        break;
                        
                    case 'ring':
                        ctx.globalAlpha = lifePercent;
                        ctx.strokeStyle = effect.color;
                        ctx.lineWidth = effect.width;
                        ctx.beginPath();
                        ctx.arc(x, y, effect.radius * (1 - lifePercent), 0, Math.PI * 2);
                        ctx.stroke();
                        break;
                        
                    case 'flash':
                        ctx.globalAlpha = lifePercent;
                        ctx.fillStyle = effect.color;
                        ctx.beginPath();
                        ctx.arc(x, y, effect.size * (1 - lifePercent), 0, Math.PI * 2);
                        ctx.fill();
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
                
                if (text.rotation) {
                    ctx.translate(x, y);
                    ctx.rotate(text.rotation);
                    ctx.translate(-x, -y);
                }
                
                // Тень текста
                ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
                ctx.font = `bold ${text.size}px Arial`;
                ctx.textAlign = 'center';
                ctx.fillText(text.text, x + 2, y + 2);
                
                // Основной текст
                ctx.fillStyle = text.color;
                ctx.font = `bold ${text.size}px Arial`;
                ctx.textAlign = 'center';
                ctx.fillText(text.text, x, y);
                
                // Свечение для важного текста
                if (text.size >= 30) {
                    ctx.shadowColor = text.color;
                    ctx.shadowBlur = 10;
                    ctx.fillText(text.text, x, y);
                    ctx.shadowBlur = 0;
                }
                
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

        // Обновить игровой интерфейс
        function updateGameUI() {
            document.getElementById('playerNameDisplay').textContent = game.player.name;
            document.getElementById('healthDisplay').textContent = Math.ceil(game.player.health);
            document.getElementById('coinsDisplay').textContent = game.coins;
            document.getElementById('enemiesDisplay').textContent = game.enemies.length;
            document.getElementById('killsDisplay').textContent = game.kills;
            document.getElementById('comboDisplay').textContent = game.combo;
            
            // Обновление полосы здоровья
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
                // Пульсация при низком здоровье
                healthBar.style.animation = healthPercent < 20 ? 'pulse 0.5s infinite' : 'none';
            }
            
            // Обновление индикатора щита
            if (game.powerUpActive && Date.now() < game.powerUpEndTime) {
                const timeLeft = Math.ceil((game.powerUpEndTime - Date.now()) / 1000);
                document.getElementById('powerUpIndicator').textContent = `🛡️ ЩИТ: ${timeLeft}с`;
            } else if (game.powerUpActive) {
                document.getElementById('powerUpIndicator').style.display = 'none';
                game.powerUpActive = false;
            }
        }

        // Обновить интерфейс оружия
        function updateWeaponUI() {
            if (game.player.weapon) {
                document.getElementById('weaponDisplay').textContent = game.player.weapon.name;
                document.getElementById('ammoDisplay').textContent = game.player.weapon.ammo;
                document.getElementById('damageDisplay').textContent = game.player.weapon.damage;
                document.getElementById('cooldownDisplay').textContent = 'ГОТОВО';
            } else {
                document.getElementById('weaponDisplay').textContent = 'НЕТ';
                document.getElementById('ammoDisplay').textContent = '-';
                document.getElementById('damageDisplay').textContent = '-';
                const timeSinceLastHit = Date.now() - game.player.lastHitTime;
                if (timeSinceLastHit < 1000) {
                    document.getElementById('cooldownDisplay').textContent = 
                        Math.ceil((1000 - timeSinceLastHit) / 1000) + 'с';
                } else {
                    document.getElementById('cooldownDisplay').textContent = 'ГОТОВО';
                }
            }
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
                gameLoop();
            }
        }

        // Показать игровое сообщение
        function showGameMessage(text, color = '#4CC9F0') {
            const messageDiv = document.getElementById('gameMessage');
            messageDiv.textContent = text;
            messageDiv.style.borderColor = color;
            messageDiv.style.display = 'block';
            messageDiv.style.boxShadow = `0 0 50px ${color}`;
            
            setTimeout(() => {
                messageDiv.style.display = 'none';
            }, 2000);
        }

        // Выйти в меню
        function exitToMenu() {
            game.running = false;
            game.paused = false;
            
            // Сохраняем данные
            saveGameData();
            
            // Обновляем статистику в меню
            updateMenuStats();
            
            // Переключаем экраны
            document.getElementById('gameScreen').classList.remove('active');
            document.getElementById('pauseScreen').classList.remove('active');
            document.getElementById('menuScreen').classList.add('active');
        }

        // Победа
        function winGame() {
            game.running = false;
            
            // Награда
            const baseReward = 20;
            const killReward = game.kills * 3;
            const comboBonus = Math.floor(game.combo * 0.5);
            const winBonus = 10;
            const totalReward = baseReward + killReward + comboBonus + winBonus;
            
            game.coins += totalReward;
            game.wins++;
            
            // Сохраняем
            saveGameData();
            
            // Эффекты победы
            createVictoryEffects();
            
            // Показываем сообщение о победе
            setTimeout(() => {
                showGameMessage(`🎉 ПОБЕДА! +${totalReward}💰`, '#FFD700');
                
                // Ждем 3 секунды и возвращаем в меню
                setTimeout(() => {
                    exitToMenu();
                    alert(`🏆 ПОБЕДА!\n\nВаш результат:\nУбийств: ${game.kills}\nКомбо: x${game.combo}\nМонет заработано: ${totalReward}\nВсего монет: ${game.coins}`);
                }, 3000);
            }, 500);
        }

        // Эффекты победы
        function createVictoryEffects() {
            // Большой фейерверк
            for (let i = 0; i < 30; i++) {
                setTimeout(() => {
                    const angle = Math.random() * Math.PI * 2;
                    const distance = 100 + Math.random() * 150;
                    const x = game.player.x + Math.cos(angle) * distance;
                    const y = game.player.y + Math.sin(angle) * distance;
                    const color = ['#FFD700', '#4CC9F0', '#FF416C', '#4CAF50'][Math.floor(Math.random() * 4)];
                    
                    createFireworkEffect(x, y, color);
                }, i * 100);
            }
            
            // Короны вокруг игрока
            for (let i = 0; i < 8; i++) {
                setTimeout(() => {
                    const angle = (i / 8) * Math.PI * 2;
                    const distance = 60;
                    createFloatingText(
                        '👑',
                        game.player.x + Math.cos(angle) * distance,
                        game.player.y + Math.sin(angle) * distance,
                        '#FFD700',
                        36
                    );
                }, i * 200);
            }
        }

        // Эффект фейерверка
        function createFireworkEffect(x, y, color) {
            // Центральная вспышка
            createFlashEffect(x, y, color, 40, 30);
            
            // Разлетающиеся частицы
            for (let i = 0; i < 24; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 2 + Math.random() * 6;
                createParticle(
                    x, y,
                    color,
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    2 + Math.random() * 4,
                    60 + Math.random() * 60
                );
            }
            
            // Звезды
            for (let i = 0; i < 8; i++) {
                const angle = (i / 8) * Math.PI * 2;
                const speed = 1 + Math.random() * 3;
                createParticle(
                    x, y,
                    '#ffffff',
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    1 + Math.random() * 2,
                    90 + Math.random() * 60
                );
            }
        }

        // Конец игры
        function gameOver() {
            game.running = false;
            
            // Сохраняем
            saveGameData();
            
            // Эффект смерти игрока
            createDeathEffect(game.player);
            
            // Показываем сообщение о поражении
            setTimeout(() => {
                showGameMessage('💀 ПОРАЖЕНИЕ', '#ff0000');
                
                // Ждем 2 секунды и возвращаем в меню
                setTimeout(() => {
                    exitToMenu();
                    alert(`💀 ИГРА ОКОНЧЕНА!\n\nВаш результат:\nУбийств: ${game.kills}\nКомбо: x${game.combo}\nМонет заработано: ${game.kills * 2}\nВсего монет: ${game.coins}`);
                }, 2000);
            }, 500);
        }
    </script>
</body>
</html>
