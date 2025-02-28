<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sitio con Anuncios</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            background: #f0f0f0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
        }

        .ad-block {
            background: white;
            width: 100%;
            max-width: 600px;
            height: 150px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.2rem;
            color: #333;
        }

        .skip-btn {
            background: #4CAF50;
            color: white;
            border: none;
            padding: 0.8rem 2rem;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1rem;
            margin: 20px 0;
            opacity: 0.5;
            transition: opacity 0.3s;
        }

        .skip-btn.active {
            opacity: 1;
        }

        .timer {
            font-size: 1.2rem;
            color: #333;
            margin: 10px 0;
        }

        /* Estilos para el pop-up de anuncio */
        .popup-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .popup {
            background: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            position: relative;
            width: 90%;
            max-width: 400px;
        }

        .close-btn {
            position: absolute;
            top: 10px;
            background: #ff4d4d;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1rem;
        }

        .close-btn.left {
            left: 10px;
        }

        .close-btn.right {
            right: 10px;
        }
    </style>
</head>
<body>
    <!-- Pop-up de anuncio -->
    <div class="popup-overlay" id="popupOverlay">
        <div class="popup">
            <button class="close-btn left" id="closeLeft">X</button>
            <button class="close-btn right" id="closeRight">X</button>
            <div class="ad-block">¡Anuncio! Cierra esta ventana para continuar.</div>
        </div>
    </div>

    <!-- Bloques de anuncio (arriba) -->
    <div class="ad-block" id="adBlock1"></div>
    <div class="ad-block" id="adBlock2"></div>

    <!-- Temporizador -->
    <div class="timer" id="timer">10</div> <!-- Contador inicia en 10 -->

    <!-- Botón para saltar fase -->
    <button class="skip-btn" onclick="nextPhase()" id="skipBtn" disabled>Saltar Fase</button>

    <!-- Bloques de anuncio (abajo) -->
    <div class="ad-block" id="adBlock3"></div>
    <div class="ad-block" id="adBlock4"></div>

    <script>
        let currentPhase = 1;
        const totalPhases = 4;
        let timeLeft = 10; // Contador visible (25 segundos reales)
        const timerInterval = 2500; // 2500 ms = 2.5 segundos
        let timerId = null;

        const adBlocks = document.querySelectorAll('.ad-block');
        const timerElement = document.getElementById('timer');
        const skipButton = document.getElementById('skipBtn');
        const popupOverlay = document.getElementById('popupOverlay');
        const closeLeft = document.getElementById('closeLeft');
        const closeRight = document.getElementById('closeRight');

        // Mostrar pop-up al inicio de cada fase
        function showPopup() {
            popupOverlay.style.display = 'flex';
            setRandomCloseButton();
        }

        // Configurar "X" correcta (50% de probabilidad)
        function setRandomCloseButton() {
            const correctX = Math.random() < 0.5 ? closeLeft : closeRight;
            correctX.onclick = () => {
                closePopup();
                startTimer();
            };
            const wrongX = correctX === closeLeft ? closeRight : closeLeft;
            wrongX.onclick = () => alert('¡Equivocado! Usa la otra "X".');
        }

        // Cerrar pop-up
        function closePopup() {
            popupOverlay.style.display = 'none';
        }

        // Iniciar temporizador (25 segundos reales)
        function startTimer() {
            timeLeft = 10; // Reiniciar contador visible
            timerElement.textContent = timeLeft;
            skipButton.disabled = true;
            skipButton.classList.remove('active');

            if (timerId) clearInterval(timerId); // Limpiar temporizador anterior

            timerId = setInterval(() => {
                timeLeft--;
                timerElement.textContent = timeLeft;

                if (timeLeft <= 0) {
                    clearInterval(timerId);
                    skipButton.disabled = false;
                    skipButton.classList.add('active');
                }
            }, timerInterval);
        }

        // Cambiar de fase
        function nextPhase() {
            if (currentPhase < totalPhases) {
                currentPhase++;
                adBlocks.forEach((block, index) => {
                    block.textContent = `Anuncio ${currentPhase}-${index + 1}`;
                });
                showPopup();
            } else {
                window.location.href = 'https://mega.nz/TU_ENLACE_AQUI';
            }
        }

        // Iniciar primera fase
        showPopup();
    </script>
</body>
</html>
