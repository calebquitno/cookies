<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Rohen Clicker Site</title>

<style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: "Segoe UI", sans-serif; }

    body {
        min-height: 100vh;
        background: linear-gradient(120deg, #6a00ff, #00d4ff, #00ff9d);
        background-size: 300% 300%;
        animation: gradientShift 12s ease infinite;
        display: flex;
        flex-direction: column;
        align-items: center;
        overflow-x: hidden;
        overflow-y: hidden;
        padding-bottom: 200px;
    }

    @keyframes gradientShift {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }

    .title {
        font-size: 3rem;
        font-weight: 900;
        margin-top: 20px;
        background: linear-gradient(90deg, red, orange, yellow, green, cyan, blue, purple);
        background-size: 400% 400%;
        -webkit-background-clip: text;
        color: transparent;
        animation: chromaWave 6s linear infinite, wave 2s ease-in-out infinite;
        text-shadow: 0 0 20px rgba(255,255,255,0.4);
    }

    @keyframes chromaWave {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }

    @keyframes wave {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-10px); }
    }

    .clicker-container {
        margin-top: 40px;
        text-align: center;
        color: white;
    }

    .spawn-btn {
        padding: 15px 40px;
        font-size: 1.3rem;
        border-radius: 30px;
        border: 2px solid rgba(255,255,255,0.5);
        background: rgba(255,255,255,0.2);
        color: white;
        cursor: pointer;
        transition: 0.3s;
    }

    .spawn-btn:hover {
        background: white;
        color: #6a00ff;
        box-shadow: 0 0 20px white;
    }

    .score {
        font-size: 1.5rem;
    }

    .floating-head {
        position: absolute;
        width: 80px;
        height: 80px;
        border-radius: 50%;
        animation: floatUp 1.2s ease-out forwards;
        pointer-events: none;
        z-index: 999;
    }

    @keyframes floatUp {
        0% { opacity: 1; transform: scale(1); }
        100% { opacity: 0; transform: scale(1.2); }
    }

    .upgrade-row {
        margin-top: 25px;
        display: flex;
        align-items: center;
        gap: 20px;
        justify-content: center;
    }

    .upgrade-btn {
        padding: 10px 20px;
        background: rgba(255,255,255,0.15);
        border: 2px solid rgba(255,255,255,0.4);
        border-radius: 15px;
        cursor: pointer;
        color: white;
        display: flex;
        align-items: center;
        gap: 10px;
        transition: 0.3s;
    }

    .upgrade-btn:hover {
        background: white;
        color: #6a00ff;
        box-shadow: 0 0 20px white;
    }

    .upgrade-icon {
        width: 40px;
        height: 40px;
        border-radius: 10px;
    }
</style>
</head>
<body>

<div class="title">SKCUS NEHOR</div>

<div class="clicker-container">
    <h2>Rohen Clicker</h2>

    <button id="clickButton" class="spawn-btn">Click Me</button>

    <div class="score" style="margin-top:20px;">
        Rohens: <span id="scoreCount">0</span>
    </div>

    <div class="upgrade-row">
        <button id="upgradeButton" class="upgrade-btn" disabled>
            <img src="RohenLibraries/IMG_6401.jpg" class="upgrade-icon">
            Auto‑Rohen (+1/sec) — Cost: <span id="upgradeCost">50</span>
        </button>

        <div class="score">Auto‑Rohens: <span id="autoCount">0</span></div>
    </div>
</div>

<script>
    const clickButton = document.getElementById("clickButton");
    const scoreCount = document.getElementById("scoreCount");
    const autoCount = document.getElementById("autoCount");
    const upgradeButton = document.getElementById("upgradeButton");
    const upgradeCostDisplay = document.getElementById("upgradeCost");

    let score = 0;
    let autoClicks = 0;
    let upgradeCost = 50;

    const sound = new Audio("RohenLibraries/RohenHello.mp3");

    function spawnHead() {
        const head = document.createElement("img");
        head.src = "RohenLibraries/IMG_6400.jpg";
        head.classList.add("floating-head");

        const x = Math.random() * window.innerWidth;
        const y = Math.random() * window.innerHeight;

        head.style.left = x + "px";
        head.style.top = y + "px";

        document.body.appendChild(head);
        setTimeout(() => head.remove(), 1200);
    }

    function generateRohen() {
        score++;
        scoreCount.textContent = score;

        spawnHead();

        sound.currentTime = 0;
        sound.play();

        if (score >= upgradeCost) upgradeButton.disabled = false;
    }

    clickButton.addEventListener("click", generateRohen);

    upgradeButton.addEventListener("click", () => {
        if (score >= upgradeCost) {
            score -= upgradeCost;
            scoreCount.textContent = score;

            autoClicks++;
            autoCount.textContent = autoClicks;

            upgradeCost = Math.floor(upgradeCost * 1.025);
            upgradeCostDisplay.textContent = upgradeCost;

            upgradeButton.disabled = true;

            setTimeout(() => {
                if (score >= upgradeCost) upgradeButton.disabled = false;
            }, 300);
        }
    });

    setInterval(() => {
        if (autoClicks > 0) {
            generateRohen();
        }
    }, 1000);
</script>

</body>
</html>
