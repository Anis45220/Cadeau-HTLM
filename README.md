<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carte à gratter</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
        }
        h1 {
            font-size: 2.5em;
            color: #ff0000;
            text-shadow: 2px 2px #aaa;
        }
        canvas {
            margin: 20px auto;
            display: block;
            background-color: #d3d3d3;
            border: 2px solid #ccc;
            cursor: pointer;
        }
        #message {
            font-size: 1.5em;
            margin-top: 20px;
            display: none;
        }
        .santa {
            width: 100px;
            margin: 10px;
        }
    </style>
</head>
<body>
    <h1>POUR MA DADOU 🎅</h1>
    <img src="https://upload.wikimedia.org/wikipedia/commons/4/42/Santa-claus.jpg" alt="Père Noël" class="santa">
    <img src="https://upload.wikimedia.org/wikipedia/commons/2/27/Santa_Claus_Christmas.png" alt="Père Noël" class="santa">

    <h2>Gratte la carte pour découvrir ton cadeau !</h2>
    <canvas id="scratchCard" width="300" height="150"></canvas>
    <div id="message">🎉 Tu as gagné un cinéma ! 🎥</div>

    <script>
        const canvas = document.getElementById('scratchCard');
        const ctx = canvas.getContext('2d');
        const message = document.getElementById('message');

        // Remplir le canvas avec une couleur grise
        ctx.fillStyle = '#a9a9a9';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Afficher un texte "Gratte ici"
        ctx.fillStyle = 'white';
        ctx.font = '20px Arial';
        ctx.fillText('Gratte ici', 100, 80);

        let isDrawing = false;

        canvas.addEventListener('mousedown', () => isDrawing = true);
        canvas.addEventListener('mouseup', () => isDrawing = false);
        canvas.addEventListener('mousemove', (event) => {
            if (isDrawing) {
                const rect = canvas.getBoundingClientRect();
                const x = event.clientX - rect.left;
                const y = event.clientY - rect.top;
                ctx.clearRect(x - 10, y - 10, 20, 20); // Effacer une petite zone
            }
        });

        canvas.addEventListener('mouseup', () => {
            const pixels = ctx.getImageData(0, 0, canvas.width, canvas.height).data;
            const scratched = Array.from(pixels).filter((pixel, index) => index % 4 === 3 && pixel === 0).length;
            const totalPixels = canvas.width * canvas.height;
            const scratchPercentage = (scratched / totalPixels) * 100;

            // Si plus de 70% est gratté, révéler le message
            if (scratchPercentage > 70) {
                canvas.style.display = 'none';
                message.style.display = 'block';
            }
        });
    </script>
</body>
</html>
