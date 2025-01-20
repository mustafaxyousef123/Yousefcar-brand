# Yousefcar-brand<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: #222;
        }
        canvas {
            display: block;
            margin: 0 auto;
            background-color: #ddd;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // Set canvas size to full window
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // Car object
        const car = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            width: 50,
            height: 30,
            angle: 0,
            speed: 0,
            maxSpeed: 5,
            acceleration: 0.1,
            friction: 0.99,
            driftFactor: 0.9,
            image: new Image(),
        };

        // Load car image
        car.image.src = 'https://www.pngmart.com/files/3/Car-PNG-Image.png'; // Replace with any car image URL

        // Input tracking
        const keys = {
            up: false,
            down: false,
            left: false,
            right: false,
        };

        // Event listeners for key controls
        window.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowUp') keys.up = true;
            if (e.key === 'ArrowDown') keys.down = true;
            if (e.key === 'ArrowLeft') keys.left = true;
            if (e.key === 'ArrowRight') keys.right = true;
        });

        window.addEventListener('keyup', (e) => {
            if (e.key === 'ArrowUp') keys.up = false;
            if (e.key === 'ArrowDown') keys.down = false;
            if (e.key === 'ArrowLeft') keys.left = false;
            if (e.key === 'ArrowRight') keys.right = false;
        });

        // Update game mechanics
        function update() {
            // Control car speed
            if (keys.up && car.speed < car.maxSpeed) car.speed += car.acceleration;
            if (keys.down && car.speed > -car.maxSpeed) car.speed -= car.acceleration;

            // Drift mechanics (turning)
            if (keys.left) car.angle -= 0.05;
            if (keys.right) car.angle += 0.05;

            // Apply speed and direction to the car's position
            car.x += Math.cos(car.angle) * car.speed;
            car.y += Math.sin(car.angle) * car.speed;

            // Apply friction
            car.speed *= car.friction;

            // Keep car inside the screen (wrap-around)
            if (car.x > canvas.width) car.x = 0;
            if (car.x < 0) car.x = canvas.width;
            if (car.y > canvas.height) car.y = 0;
            if (car.y < 0) car.y = canvas.height;
        }

        // Draw the car on the canvas
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw track background (simple grid for now)
            ctx.strokeStyle = '#444';
            ctx.lineWidth = 5;
            ctx.setLineDash([5, 15]);

            // Draw dashed road lines
            for (let x = 0; x < canvas.width; x += 40) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }

            // Reset dashed line
            ctx.setLineDash([]);

            // Draw the car
            ctx.save();
            ctx.translate(car.x, car.y);
            ctx.rotate(car.angle);
            ctx.drawImage(car.image, -car.width / 2, -car.height / 2, car.width, car.height);
            ctx.restore();
        }

        // Main game loop
        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        // Start the game
        gameLoop();
    </script>
</body>
</html>
