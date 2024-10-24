<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Mario</title>
    <style>
        canvas { background: lightblue; display: block; margin: 0 auto; }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let jugador = { x: 50, y: 300, width: 50, height: 50, dy: 4, saltando: false };
        const gravedad = 0.3;
        let obstaculos = [];
        let velocidadObstaculo = 4;

        function dibujarJugador() {
            ctx.fillStyle = 'green';
            ctx.fillRect(jugador.x, jugador.y, jugador.width, jugador.height);
        }

        function crearObstaculo() {
            const x = canvas.width;
            const y = 310; // altura del obstáculo
            const width = 40;
            const height = 40;
            obstaculos.push({ x, y, width, height });
        }

        function dibujarObstaculos() {
            ctx.fillStyle = 'red';
            obstaculos.forEach(obstaculo => {
                ctx.fillRect(obstaculo.x, obstaculo.y, obstaculo.width, obstaculo.height);
                obstaculo.x -= velocidadObstaculo; // mover obstáculo hacia la izquierda
            });
        }

        function colision() {
            for (const obstaculo of obstaculos) {
                if (jugador.x < obstaculo.x + obstaculo.width &&
                    jugador.x + jugador.width > obstaculo.x &&
                    jugador.y < obstaculo.y + obstaculo.height &&
                    jugador.y + jugador.height > obstaculo.y) {
                    alert("¡Chocaste! perdiste.");
                    document.location.reload(); // reiniciar juego
                }
            }
        }

        function actualizar() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dibujarJugador();
            dibujarObstaculos();
            colision();

            if (jugador.saltando) {
                jugador.dy += gravedad;
                jugador.y += jugador.dy;
                if (jugador.y >= 300) {
                    jugador.y = 300;
                    jugador.saltando = false;
                    jugador.dy = 0;
                }
            }

            // Crear obstáculos aleatoriamente
            if (Math.random() < 0.02) {
                crearObstaculo();
            }

            requestAnimationFrame(actualizar);
        }

        window.addEventListener('keydown', (event) => {
            if (event.key === 'ArrowLeft') {
                jugador.x -= 5;
            }
            if (event.key === 'ArrowRight') {
                jugador.x += 5;
            }
            if (event.key === ' ' && !jugador.saltando) {
                jugador.saltando = true;
                jugador.dy = -10;
            }
        });

        actualizar();
    </script>
</body>
</html># Mario 
