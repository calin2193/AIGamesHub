# AIGamesHub
Jocuri creeate cu AI
<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pagina Principală Jocuri</title>
    <style>
        body {
            font-family: sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }
        .button {
            padding: 15px 30px;
            font-size: 1.2em;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            text-decoration: none;
        }
        .button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <h1>Bine ai venit la pagina de jocuri!</h1>
    <a href="escape_room.html" class="button">Joacă Escape Room Horror</a>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Escape Room Horror</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: black;
      color: white;
      font-family: 'Courier New', Courier, monospace;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      overflow: hidden;
      transition: transform 0.1s ease-out;
    }
    #game-container {
      text-align: center;
      max-width: 800px;
      padding: 20px;
    }
    .hidden {
      display: none;
    }
    #timer {
      font-size: 24px;
      margin-bottom: 20px;
    }
    .button {
      background-color: #444;
      border: none;
      color: white;
      padding: 10px 20px;
      cursor: pointer;
      margin-top: 20px;
    }
    input {
      padding: 10px;
      margin-top: 10px;
      width: 200px;
    }
    #screamer {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: black;
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 999;
    }
    #screamer img {
      width: 100%;
      max-width: 700px;
    }

    /* Vibrarea ecranului */
    .vibrate {
      animation: shake 0.2s 3;
    }

    @keyframes shake {
      0% { transform: translateX(0); }
      25% { transform: translateX(-10px); }
      50% { transform: translateX(10px); }
      75% { transform: translateX(-10px); }
      100% { transform: translateX(10px); }
    }
  </style>
</head>
<body>
  <div id="game-container">
    <div id="camera1">
      <h1>🕯️ Escape Room: Camera 1</h1>
      <div id="timer">Timp rămas: <span id="time">00:30</span></div>
      <p>Pe perete este scris cu sânge: <em>„Numele tău uitat... scrie-l din nou.”</em></p>
      <input type="text" id="answer" placeholder="Introdu numele tău">
      <br>
      <button class="button" onclick="checkAnswer()">Confirmă</button>
      <p id="result"></p>
    </div>

    <div id="camera2" class="hidden">
      <h1>🔐 Escape Room: Camera 2</h1>
      <p>Două ani bântuie pereții: 1984 și 1996. Ce rezultat obții dacă le aduni?</p>
      <input type="text" id="code" placeholder="Scrie codul">
      <br>
      <button class="button" onclick="checkCode()">Confirmă</button>
      <p id="code-result"></p>
    </div>

    <div id="camera3" class="hidden">
      <h1>🕳️ Escape Room: Camera 3</h1>
      <p>Un ceas fără limbi arată ora exactă... Ce oră este?</p>
      <input type="text" id="riddle3" placeholder="Răspunsul tău">
      <br>
      <button class="button" onclick="checkRiddle3()">Confirmă</button>
      <p id="riddle3-result"></p>
    </div>

    <div id="camera4" class="hidden">
      <h1>🩸 Escape Room: Camera 4</h1>
      <p>Ce are 4 picioare dimineața, 2 la prânz și 3 seara?</p>
      <input type="text" id="riddle4" placeholder="Răspunsul tău">
      <br>
      <button class="button" onclick="checkRiddle4()">Confirmă</button>
      <p id="riddle4-result"></p>
    </div>

    <div id="camera5" class="hidden">
      <h1>🧠 Escape Room: Camera 5</h1>
      <p>Se sparge când îl rostești. Ce este?</p>
      <input type="text" id="riddle5" placeholder="Răspunsul tău">
      <br>
      <button class="button" onclick="checkRiddle5()">Confirmă</button>
      <p id="riddle5-result"></p>
    </div>

    <div id="camera6" class="hidden">
      <h1>🕸️ Escape Room: Camera 6</h1>
      <p>O umbră trece pe lângă tine. Pe podea sunt urme ude. Ce culoare are apa?</p>
      <input type="text" id="riddle6" placeholder="Răspunsul tău">
      <br>
      <button class="button" onclick="checkRiddle6()">Confirmă</button>
      <p id="riddle6-result"></p>
    </div>

    <div id="camera7" class="hidden">
      <h1>🎭 Escape Room: Camera Finală</h1>
      <p>Felicitări... dar ai fost păcălit.</p>
      <h2>„Jocul ăsta a fost doar să pot intra mult mai ușor în contact cu tine... la noapte o să ne jucăm mai bine.”</h2>
    </div>
  </div>

  <div id="screamer">
    <img src="https://i.pinimg.com/originals/61/5e/33/615e33f0253df3bee40cd848cea5cc6b.jpg" alt="Screamer">
  </div>

  <script>
    let time = 30;
    let currentCamera = 1;
    const timerDisplay = document.getElementById('time');
    const screamer = document.getElementById('screamer');
    let countdown;

    // Functia de timer
    function startTimer() {
      clearInterval(countdown);
      time = 30;
      countdown = setInterval(() => {
        let minutes = Math.floor(time / 60);
        let seconds = time % 60;
        timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        if (time > 0) {
          time--;
        } else {
          clearInterval(countdown);
          triggerScreamer();
        }
      }, 1000);
    }

    startTimer();

    // Muta jocul la camera următoare
    function goToCamera(num) {
      document.getElementById(`camera${currentCamera}`).classList.add('hidden');
      currentCamera = num;
      document.getElementById(`camera${currentCamera}`).classList.remove('hidden');
      startTimer();
    }

    // Functia de control pentru camera 1
    function checkAnswer() {
      const input = document.getElementById('answer').value.trim().toLowerCase();
      if (input !== '') {
        goToCamera(2);
      } else {
        triggerScreamer();
      }
    }

    // Functia de control pentru camera 2
    function checkCode() {
      const code = document.getElementById('code').value.trim();
      if (code === '3980') {
        goToCamera(3);
      } else {
        triggerScreamer();
      }
    }

    // Functia de control pentru camera 3
    function checkRiddle3() {
      const answer = document.getElementById('riddle3').value.trim().toLowerCase();
      if (answer.includes('12') || answer.includes('douăsprezece')) {
        goToCamera(4);
      } else {
        triggerScreamer();
      }
    }

    // Functia de control pentru camera 4
    function checkRiddle4() {
      const answer = document.getElementById('riddle4').value.trim().toLowerCase();
      if (answer.includes('om')) {
        goToCamera(5);
      } else {
        triggerScreamer();
      }
    }

    // Functia de control pentru camera 5
    function checkRiddle5() {
      const answer = document.getElementById('riddle5').value.trim().toLowerCase();
      if (answer.includes('tăcerea')) {
        goToCamera(6);
      } else {
        triggerScreamer();
      }
    }

    // Functia de control pentru camera 6
    function checkRiddle6() {
      const answer = document.getElementById('riddle6').value.trim().toLowerCase();
      if (answer.includes('nu are') || answer.includes('transparent')) {
        goToCamera(7);
      } else {
        triggerScreamer();
      }
    }

    // Functia de activare a screamer-ului
    function triggerScreamer() {
      screamer.style.display = 'flex';

      // Creare sunet groază prin AudioContext
      const audioContext = new (window.AudioContext || window.webkitAudioContext)();
      const oscillator = audioContext.createOscillator();
      oscillator.type = 'sine'; // tipul sunetului
      oscillator.frequency.setValueAtTime(20, audioContext.currentTime); // frecvență joasă pentru sunet înfricoșător
      oscillator.connect(audioContext.destination);
      oscillator.start();
      oscillator.stop(audioContext.currentTime + 1); // sunetul se oprește după 1 secundă

      document.body.classList.add('vibrate');
      setTimeout(() => {
        document.body.classList.remove('vibrate');
      }, 1000);
    }
  </script>
</body>
</html>

