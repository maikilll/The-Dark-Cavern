# The-Dark-Cavern
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Dark Cavern - RPG</title>
    <style>
        * { box-sizing: border-box; }
        body {
            background-color: #050507;
            color: #d1d5db;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 15px;
        }
        @keyframes shake {
            0% { transform: translate(1px, 1px) rotate(0deg); }
            20% { transform: translate(-3px, 0px) rotate(-1deg); }
            40% { transform: translate(1px, -1px) rotate(1deg); }
            60% { transform: translate(-1px, 2px) rotate(0deg); }
            80% { transform: translate(-3px, 1px) rotate(-1deg); }
            100% { transform: translate(1px, -2px) rotate(-1deg); }
        }
        .shake { animation: shake 0.3s; }
        .container {
            width: 100%;
            max-width: 550px;
            background: #111827;
            border: 2px solid #374151;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.9);
        }
        h1 {
            color: #ef4444;
            text-align: center;
            margin-top: 0;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 0 0 10px rgba(239, 68, 68, 0.5);
        }
        .stats {
            display: flex;
            justify-content: space-between;
            background: #1f2937;
            padding: 10px 15px;
            border-radius: 6px;
            margin-bottom: 15px;
            font-weight: bold;
            font-size: 0.95em;
        }
        .log-box {
            background: #030712;
            border: 1px solid #1f2937;
            height: 160px;
            overflow-y: auto;
            padding: 10px;
            border-radius: 6px;
            margin-bottom: 15px;
            font-size: 0.9em;
            line-height: 1.4;
        }
        .log-entry { margin-bottom: 8px; }
        .danger { color: #f87171; }
        .success { color: #34d399; }
        .boss { color: #a855f7; font-weight: bold; }
        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 10px;
        }
        button {
            background: #2563eb;
            color: white;
            border: none;
            padding: 12px;
            border-radius: 6px;
            font-size: 1em;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.2s;
        }
        button:hover { background: #1d4ed8; }
        button:disabled { background: #374151; color: #6b7280; cursor: not-allowed; }
        button.attack { background: #dc2626; }
        button.attack:hover { background: #b91c1c; }
        button.potion { background: #059669; grid-column: span 2; }
        button.potion:hover { background: #047857; }
        footer {
            margin-top: 15px;
            text-align: center;
            font-size: 0.85em;
            line-height: 1.6;
        }
        footer a { color: #a855f7; text-decoration: none; font-weight: bold; }
    </style>
</head>
<body>

<div class="container" id="game-container">
    <h1>The Dark Cavern</h1>
    
    <div class="stats">
        <span>HP: <span id="player-hp">100</span></span>
        <span>Poções: <span id="player-potions">2</span></span>
        <span>Inimigo HP: <span id="enemy-hp">0</span></span>
    </div>

    <div style="font-size: 0.85em; color: #9ca3af; margin-bottom: 8px; text-align: center;">
        Alvo: <span id="enemy-name" style="color: #f3f4f6; font-weight: bold;">Nenhum</span>
    </div>

    <div class="log-box" id="log">
        <div class="log-entry">Entraste nas profundezas da caverna obscura... Cuidado com o Warkell.</div>
    </div>

    <div class="controls">
        <button id="btn-explore" onclick="explore()">Explorar</button>
        <button id="btn-attack" onclick="attack()" disabled class="attack">Atacar</button>
        <button id="btn-potion" onclick="usePotion()" class="potion">Usar Poção de Cura</button>
    </div>

    <footer>
        Link oficial: <a href="https://maikilll.github.io/The-Dark-Cavern/" target="_blank">maikilll.github.io/The-Dark-Cavern/</a><br>
        Vilão: Warkell | Insta: <a href="https://instagram.com/_maiko_lone_" target="_blank">@_maiko_lone_</a>
    </footer>
</div>

<script>
    let playerHP = 100;
    let maxPlayerHP = 100;
    let potions = 2;
    let enemyHP = 0;
    let currentEnemy = null;

    const logBox = document.getElementById('log');
    const playerHpEl = document.getElementById('player-hp');
    const playerPotionsEl = document.getElementById('player-potions');
    const enemyHpEl = document.getElementById('enemy-hp');
    const enemyNameEl = document.getElementById('enemy-name');
    const btnExplore = document.getElementById('btn-explore');
    const btnAttack = document.getElementById('btn-attack');
    const containerEl = document.getElementById('game-container');

    function triggerShake() {
        containerEl.classList.add('shake');
        setTimeout(() => containerEl.classList.remove('shake'), 300);
    }

    function log(msg, type = '') {
        const div = document.createElement('div');
        div.className = `log-entry ${type}`;
        div.innerHTML = msg;
        logBox.appendChild(div);
        logBox.scrollTop = logBox.scrollHeight;
    }

    function explore() {
        if (currentEnemy) return;

        const roll = Math.random();
        if (roll < 0.35) {
            log("Caminhas pelas sombras... O eco dos teus passos ecoa na caverna vazia.");
        } else if (roll < 0.65) {
            startBattle("Esqueleto Sombrio", 45);
        } else {
            startBattle("WARKELL", 100, true);
        }
    }

    function startBattle(name, hp, isBoss = false) {
        currentEnemy = { name, hp, maxHp: hp, isBoss };
        enemyHP = hp;
        enemyNameEl.innerText = name;
        enemyHpEl.innerText = hp;
        
        btnExplore.disabled = true;
        btnAttack.disabled = false;

        if (isBoss) {
            triggerShake();
            log(`<b>ATENÇÃO!</b> O temido vilão <span class="boss">${name}</span> bloqueia o teu caminho!`, 'danger');
        } else {
            log(`Um <b>${name}</b> selvagem apareceu!`, 'danger');
        }
    }

    function attack() {
        if (!currentEnemy) return;

        let pDamage = Math.floor(Math.random() * 18) + 12;
        enemyHP -= pDamage;
        if (enemyHP < 0) enemyHP = 0;
        enemyHpEl.innerText = enemyHP;
        log(`Atacaste ${currentEnemy.name} causando <b>${pDamage}</b> de dano.`, 'success');

        if (enemyHP <= 0) {
            log(`Vitória! Derrotaste ${currentEnemy.name}.`, 'success');
            currentEnemy = null;
            enemyNameEl.innerText = "Nenhum";
            btnExplore.disabled = false;
            btnAttack.disabled = true;
            return;
        }

        let eDamage = currentEnemy.isBoss ? Math.floor(Math.random() * 22) + 14 : Math.floor(Math.random() * 12) + 6;
        playerHP -= eDamage;
        if (playerHP < 0) playerHP = 0;
        playerHpEl.innerText = playerHP;
        triggerShake();
        log(`${currentEnemy.name} contra-atacou e causou-te <b>${eDamage}</b> de dano!`, 'danger');

        if (playerHP <= 0) {
            log("<b>Foste derrotado... As trevas consumiram-te. Atualiza a página para renascer.</b>", 'danger');
            btnAttack.disabled = true;
            btnExplore.disabled = true;
        }
    }

    function usePotion() {
        if (potions <= 0) {
            log("Não tens mais poções de cura disponíveis!");
            return;
        }
        if (playerHP >= maxPlayerHP) {
            log("A tua vida já está cheia!");
            return;
        }

        potions--;
        let heal = 35;
        playerHP += heal;
        if (playerHP > maxPlayerHP) playerHP = maxPlayerHP;

        playerHpEl.innerText = playerHP;
        playerPotionsEl.innerText = potions;
        log(`Bebeste uma poção e recuperaste <b>${heal}</b> de HP.`, 'success');
    }
</script>
</body>
</html>
