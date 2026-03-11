# formydoctor1
order
[love.html](https://github.com/user-attachments/files/25892049/love.html)
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Heart Animation</title>
    <style>
        body,
        html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: #000;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        canvas {
            display: block;
            width: 100vw;
            height: 100vh;
        }

        .text {
            position: absolute;
            width: 100%;
            text-align: center;
            color: #ffe6f2;
            font-family: 'Arial', sans-serif;
            font-size: 2.5rem;
            font-style: italic;
            text-shadow: 0 0 10px #ff3487, 0 0 20px #ff3487;
            pointer-events: none;
            z-index: 10;
            animation: pulse 2s infinite alternate;
        }

        .text-top {
            top: 15%;
        }

        .text-bottom {
            bottom: 15%;
        }

        @keyframes pulse {
            from {
                text-shadow: 0 0 10px #ff3487, 0 0 20px #ff3487;
            }

            to {
                text-shadow: 0 0 15px #ff3487, 0 0 30px #ff3487, 0 0 40px #ff3487;
                color: #fff;
            }
        }
    </style>
</head>

<body>
    <div class="text text-top">i love u foreever</div>
    <div class="text text-bottom">my pretty doctor🎀✨</div>
    <canvas id="heartCanvas"></canvas>
    <script>
        const canvas = document.getElementById('heartCanvas');
        const ctx = canvas.getContext('2d');

        let width, height, cx, cy, scale;

        function resize() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            cx = width / 2;
            cy = height / 2;
            // Scale heart down on smaller screens, base scale on dimension
            scale = Math.min(width, height) / 45;

            // clear background
            ctx.fillStyle = '#000';
            ctx.fillRect(0, 0, width, height);
        }

        window.addEventListener('resize', resize);
        resize();

        let linesDrawn = 0;
        const totalLines = 3000; // Total lines to draw before finishing
        const linesPerFrame = 8; // Adjust for speed of animation

        // Use global composite operation for a vibrant neon blend effect
        ctx.globalCompositeOperation = 'lighter';

        function drawHeartLines() {
            if (linesDrawn >= totalLines) {
                return; // Heart is full, stop animating
            }

            for (let i = 0; i < linesPerFrame; i++) {
                if (linesDrawn >= totalLines) break;

                // Pick a random angle t for points on the heart curve
                const t = Math.random() * Math.PI * 2;

                // Heart equation using trigonometric functions
                // x = 16 * sin^3(t)
                const x = 16 * Math.pow(Math.sin(t), 3);
                // y = 13*cos(t) - 5*cos(2*t) - 2*cos(3*t) - cos(4*t)
                // y is inverted here (-) because canvas y-axis goes down
                const y = -(13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));

                // Scale up the mathematical points to screen space
                const destX = cx + scale * x;
                const destY = cy + scale * y;

                ctx.beginPath();
                ctx.moveTo(cx, cy);
                ctx.lineTo(destX, destY);

                // Randomize slightly the line stroke to create dynamic texture
                const opacity = 0.2 + Math.random() * 0.4;
                ctx.strokeStyle = `rgba(255, 52, 135, ${opacity})`;

                // Most lines are thin, some are occasionally thicker
                ctx.lineWidth = Math.random() < 0.1 ? 1.5 : 0.5;

                // Create the neon glow effect
                ctx.shadowBlur = 8;
                ctx.shadowColor = '#ff3487';

                ctx.stroke();

                linesDrawn++;
            }

            // Loop frame
            requestAnimationFrame(drawHeartLines);
        }

        // Start animation
        requestAnimationFrame(drawHeartLines);
    </script>
</body>

</html>
