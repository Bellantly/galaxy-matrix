<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selamat Ulang Tahun - Galaxy Matrix</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #000;
            font-family: 'Courier New', Courier, monospace;
        }

        /* Layar Pembuka (Intro) */
        .intro-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
            text-align: center;
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .intro-text {
            color: #00ffff;
            font-size: 5rem;
            font-weight: bold;
            text-shadow: 0 0 20px #00ffff;
            opacity: 0;
            display: none;
            animation: fader 1s ease-in-out;
        }

        @keyframes fader {
            0% { opacity: 0; transform: scale(0.5); }
            50% { opacity: 1; transform: scale(1.1); }
            100% { opacity: 0; transform: scale(1); }
        }

        /* Kanvas Matrix Galaksi */
        canvas {
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
            opacity: 0;
            transition: opacity 2s ease-in-out;
        }

        /* Konten Utama (Muncul Belakangan) */
        .main-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 2;
            text-align: center;
            width: 90%;
            pointer-events: none;
            opacity: 0;
            transition: opacity 2s ease-in-out;
        }

        h1 {
            color: #fff;
            font-size: 3.5rem;
            text-transform: uppercase;
            letter-spacing: 5px;
            margin-bottom: 20px;
            text-shadow: 0 0 10px #ff007f, 0 0 20px #ff007f, 0 0 40px #ff007f;
            animation: pulse 2s infinite alternate;
        }

        p {
            color: #00ffff;
            font-size: 1.5rem;
            letter-spacing: 2px;
            text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff;
        }

        @keyframes pulse {
            0% {
                transform: scale(0.98);
                text-shadow: 0 0 10px #ff007f, 0 0 20px #ff007f;
            }
            100% {
                transform: scale(1.02);
                text-shadow: 0 0 20px #9d00ff, 0 0 40px #9d00ff, 0 0 60px #00ffff;
            }
        }

        @media (max-width: 768px) {
            .intro-text { font-size: 3.5rem; }
            h1 { font-size: 2rem; }
            p { font-size: 1rem; }
        }
    </style>
</head>
<body>

    <!-- Wadah untuk teks langkah-langkah intro -->
    <div class="intro-container">
        <div id="introBox" class="intro-text"></div>
    </div>

    <!-- Efek Matrix Galaksi -->
    <canvas id="matrixCanvas"></canvas>

    <!-- Ucapan Utama -->
    <div id="mainContent" class="main-container">
        <h1>Selamat Ulang Tahun!</h1>
        <p>Semoga Hari-Harimu Penuh Keberuntungan di Seluruh Galaksi</p>
    </div>

    <script>
        const canvas = document.getElementById('matrixCanvas');
        const ctx = canvas.getContext('2d');
        const introBox = document.getElementById('introBox');
        const mainContent = document.getElementById('mainContent');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);

        // Karakter Matrix dan elemen astronomi galaksi
        const alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789$+-*/=%" + "ΩΨΦΞ★☄🛸🌌✨🌟";
        const fontSize = 16;
        const columns = canvas.width / fontSize;
        const rainDrops = Array(Math.floor(columns)).fill(1);
        const galaxyColors = ['#ff007f', '#9d00ff', '#00ffff', '#ffffff', '#7000ff'];

        function drawMatrix() {
            ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.font = fontSize + 'px monospace';

            for (let i = 0; i < rainDrops.length; i++) {
                const text = alphabet.charAt(Math.floor(Math.random() * alphabet.length));
                ctx.fillStyle = galaxyColors[Math.floor(Math.random() * galaxyColors.length)];
                ctx.fillText(text, i * fontSize, rainDrops[i] * fontSize);

                if (rainDrops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    rainDrops[i] = 0;
                }
                rainDrops[i]++;
            }
        }

        // Jalur Alur Waktu Intro (Timeline Animasi Langkah 1, 2, 3)
        const steps = ["1", "2", "3", "Are you ready?"];
        let currentStep = 0;

        function runIntro() {
            if (currentStep < steps.length) {
                // Ubah warna teks khusus untuk tulisan "Are you ready?" menjadi pink galaksi
                if (currentStep === 3) {
                    introBox.style.color = '#ff007f';
                    introBox.style.textShadow = '0 0 20px #ff007f';
                }
                
                introBox.innerText = steps[currentStep];
                introBox.style.display = 'block';
                
                // Reset animasi CSS agar memicu efek kedip ulang
                introBox.style.animation = 'none';
                introBox.offsetHeight; 
                introBox.style.animation = 'fader 1.2s ease-in-out';

                currentStep++;
                setTimeout(runIntro, 1200); // Jeda antar langkah (1.2 detik)
            } else {
                // Intro Selesai -> Munculkan Efek Utama Galaksi & Ucapan
                introBox.style.display = 'none';
                canvas.style.opacity = '1';
                mainContent.style.opacity = '1';
                setInterval(drawMatrix, 30);
            }
        }

        // Mulai jalankan langkah-langkah begitu halaman terbuka
        window.onload = runIntro;
    </script>
</body>
</html>
