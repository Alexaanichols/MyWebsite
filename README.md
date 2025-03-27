<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Game - Collect the Coins</title>
    <style>
        body { text-align: center; padding: 50px; font-family: Arial, sans-serif; }
        h1 { color: #333; }
        canvas { border: 1px solid #000; background-color: #f4f4f4; }
        p { font-size: 20px; }
    </style>
</head>
<body>
    <h1>Collect the Coins!</h1>
    <p>Score: <span id="score">0</span></p>
    <canvas id="gameCanvas" width="500" height="500"></canvas>
    <script>
        // Set up canvas and context
        let canvas = document.getElementById('gameCanvas');
        let ctx = canvas.getContext('2d');

        // Game variables
        let player = { x: 50, y: 50, width: 30, height: 30, speed: 5 };
        let coins = [];
        let score = 0;

        // Generate random coins
        function generateCoins() {
            for (let i = 0; i < 5; i++) {
                coins.push({
                    x: Math.random() * (canvas.width - 30),
                    y: Math.random() * (canvas.height - 30),
                    width: 20,
                    height: 20,
                    collected: false
                });
            }
        }

        // Draw player (a blue square)
        function drawPlayer() {
            ctx.fillStyle = "#00F";
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        // Draw coins
        function drawCoins() {
            coins.forEach(coin => {
                if (!coin.collected) {
                    ctx.fillStyle = "#FF0";
                    ctx.fillRect(coin.x, coin.y, coin.width, coin.height);
                }
            });
        }

        // Move player based on arrow keys
        function movePlayer() {
            window.addEventListener('keydown', (e) => {
                if (e.key === "ArrowUp" && player.y > 0) player.y -= player.speed;
                if (e.key === "ArrowDown" && player.y < canvas.height - player.height) player.y += player.speed;
                if (e.key === "ArrowLeft" && player.x > 0) player.x -= player.speed;
                if (e.key === "ArrowRight" && player.x < canvas.width - player.width) player.x += player.speed;
            });
        }

        // Check for collisions with coins
        function checkCoinCollection() {
            coins.forEach(coin => {
                if (!coin.collected && player.x < coin.x + coin.width && player.x + player.width > coin.x &&
                    player.y < coin.y + coin.height && player.y + player.height > coin.y) {
                    coin.collected = true;
                    score += 10; // Increase score by 10 for each coin collected
                    document.getElementById("score").innerText = score; // Update score on screen
                }
            });
        }

        // Game loop
        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear canvas for next frame

            drawPlayer(); // Draw player
            drawCoins(); // Draw coins
            checkCoinCollection(); // Check if player collects a coin

            requestAnimationFrame(gameLoop); // Keep the game loop running
        }

        // Start the game
        generateCoins(); // Generate coins at random positions
        movePlayer(); // Set up player movement
        gameLoop(); // Start the game loop
    </script>
</body>
</html>
