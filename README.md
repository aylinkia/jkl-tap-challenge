from IPython.core.display import display, HTML

html_code = """
<!DOCTYPE html>
<html>
<head>
    <title>JKL Tap Challenge</title>
    <style>
        body { font-family: Arial, text-align: center; margin-top: 50px; }
        #letter { font-size: 100px; margin: 40px 0; }
        #stats { margin-top: 20px; }
        button { font-size: 24px; }
    </style>
</head>
<body>
    <h1>JKL Tap Challenge</h1>
    <div id="letter">---</div>
    <button id="startBtn">Start Game</button>
    <div id="info"></div>
    <div id="stats"></div>

    <script>
        const letters = ['J', 'K', 'L'];
        let currentLetter = '', score = 0, tries = 0, startTime = 0, times = [], timer, running = false;
        const duration = 30000;

        document.getElementById('startBtn').onclick = startGame;
        document.addEventListener('keydown', handleKey);

        function showLetter() {
            currentLetter = letters[Math.floor(Math.random() * 3)];
            document.getElementById('letter').innerText = currentLetter;
            startTime = Date.now();
        }

        function handleKey(e) {
            if (!running) return;
            let pressed = e.key.toUpperCase();
            if (letters.includes(pressed)) {
                tries++;
                if (pressed === currentLetter) {
                    let reaction = Date.now() - startTime;
                    times.push(reaction);
                    score++;
                    document.getElementById('info').innerText = `Good! Reaction: ${reaction} ms`;
                } else {
                    document.getElementById('info').innerText = `Oops, wrong key!`;
                }
                showLetter();
            }
        }

        function startGame() {
            score = 0; tries = 0; times = [];
            running = true;
            document.getElementById('info').innerText = "";
            document.getElementById('stats').innerText = "";
            showLetter();
            timer = setTimeout(endGame, duration);
            document.getElementById('startBtn').disabled = true;
        }

        function endGame() {
            running = false;
            clearTimeout(timer);
            let avg = times.length ? Math.round(times.reduce((a,b) => a+b,0)/times.length) : 0;
            let best = times.length ? Math.min(...times) : 0;
            let worst = times.length ? Math.max(...times) : 0;
            document.getElementById('letter').innerText = '---';
            document.getElementById('stats').innerHTML = `
                <h2>Game Over!</h2>
                <b>Score:</b> ${score}<br>
                <b>Total Tries:</b> ${tries}<br>
                <b>Accuracy:</b> ${tries ? Math.round(100 * score/tries) : 0}%<br>
                <b>Average Reaction:</b> ${avg} ms<br>
                <b>Best Reaction:</b> ${best} ms<br>
                <b>Slowest Reaction:</b> ${worst} ms
            `;
            document.getElementById('startBtn').disabled = false;
        }
    </script>
</body>
</html>
"""

display(HTML(html_code))
