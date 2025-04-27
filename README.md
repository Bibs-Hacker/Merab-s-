# Merab-s-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hacker Console</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background-color: black;
    }

    #matrix {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      animation: matrixAnimation 3s infinite linear;
    }

    @keyframes matrixAnimation {
      0% { opacity: 0.1; }
      50% { opacity: 0.7; }
      100% { opacity: 0.1; }
    }

    #loading-bar {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 60%;
      height: 30px;
      background-color: #000;
      border: 2px solid #fff;
      display: none;
      animation: loadingBarAnimation 1.5s ease-in-out forwards;
    }

    @keyframes loadingBarAnimation {
      0% { width: 0%; }
      100% { width: 100%; }
    }

    #progress {
      height: 100%;
      width: 0;
      background-color: #0f0;
      border-radius: 5px;
      box-shadow: 0 0 10px 2px #0f0;
    }

    /* Styling for text messages */
    .text {
      color: #0f0;
      font-family: "Courier New", Courier, monospace;
      font-size: 30px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      max-width: 90%;
      padding: 20px;
      background-color: rgba(0, 0, 0, 0.7);
      border-radius: 10px;
      display: none;
      opacity: 0;
      animation: fadeInAnimation 2s forwards;
    }

    @keyframes fadeInAnimation {
      0% { opacity: 0; }
      100% { opacity: 1; }
    }

    .countdown {
      font-size: 40px;
      color: #f00;
      font-weight: bold;
      animation: pulseCountdown 1s ease-in-out infinite;
    }

    @keyframes pulseCountdown {
      0% { transform: scale(1); }
      50% { transform: scale(1.2); color: #ff0; }
      100% { transform: scale(1); }
    }

    .matrix-face {
      font-size: 50px;
      text-align: center;
      position: absolute;
      top: 70%;
      width: 100%;
      color: #0f0;
      display: none;
      animation: bounceAnimation 1.5s ease-in-out forwards;
    }

    @keyframes bounceAnimation {
      0% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
      100% { transform: translateY(0); }
    }

    /* Glow Effect for text */
    .glow {
      text-shadow: 0 0 10px #0f0, 0 0 20px #0f0, 0 0 30px #0f0;
    }
  </style>
</head>
<body>

  <div id="matrix"></div>

  <div id="loading-bar">
    <div id="progress"></div>
  </div>

  <div id="message1" class="text glow"></div>
  <div id="countdown" class="text countdown"></div>
  <div id="message2" class="text glow"></div>
  <div id="matrix-face" class="matrix-face"></div>

  <script>
    let progress = 0;
    let loadingInterval;
    let beepSound = new Audio('beep_sound.wav'); // Add your beep sound file
    let matrixElement = document.getElementById('matrix');
    let matrixFaceElement = document.getElementById('matrix-face');
    let message1Element = document.getElementById('message1');
    let message2Element = document.getElementById('message2');
    let countdownElement = document.getElementById('countdown');

    // Function to simulate the typewriter effect
    function typewriterEffect(element, text, delay, callback) {
      let i = 0;
      element.innerHTML = ""; // Clear previous text
      element.style.display = 'block';  // Show text element
      const interval = setInterval(() => {
        element.innerHTML += text.charAt(i);
        i++;
        if (i > text.length - 1) {
          clearInterval(interval);
          setTimeout(() => {
            element.style.display = 'none';  // Hide text element after it's fully typed
            callback();  // Proceed to the next step
          }, 1000);  // Delay before the next step
        }
      }, delay);
    }

    // Matrix falling effect
    function createMatrixEffect() {
      const canvas = document.createElement('canvas');
      matrixElement.appendChild(canvas);
      const ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      
      const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      const font_size = 18;
      const columns = canvas.width / font_size;
      const drops = Array.from({ length: columns }, () => 1);
      
      function drawMatrix() {
        ctx.fillStyle = "rgba(0, 0, 0, 0.05)"; // Transparent background effect
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        ctx.fillStyle = "#0F0";
        ctx.font = `${font_size}px monospace`;

        for (let x = 0; x < columns; x++) {
          const text = chars.charAt(Math.floor(Math.random() * chars.length));
          ctx.fillText(text, x * font_size, drops[x] * font_size);
          
          if (drops[x] * font_size > canvas.height && Math.random() > 0.975) {
            drops[x] = 0;
          }
          drops[x]++;
        }
      }

      setInterval(drawMatrix, 50);
    }

    // Loading bar animation
    function loadingBar() {
      document.getElementById('loading-bar').style.display = 'block';
      loadingInterval = setInterval(() => {
        if (progress < 100) {
          progress++;
          document.getElementById('progress').style.width = progress + '%';
        } else {
          clearInterval(loadingInterval);
          setTimeout(startHacking, 1000); // Delay before starting hacking process
        }
      }, 50);
    }

    // Start displaying each step
    function startHacking() {
      showMessage(message1Element, "BRIAN IS HACKING MERAB'S HEART🤨🥺", function() {
        setTimeout(startCountdown, 1000);
      });
    }

    // Countdown timer with beep sound
    function startCountdown() {
      let countdownTime = 10;
      countdownElement.style.display = 'block';
      countdownElement.innerHTML = `Countdown: ${countdownTime}`;
      
      const countdownInterval = setInterval(() => {
        countdownTime--;
        countdownElement.innerHTML = `Countdown: ${countdownTime}`;
        beepSound.play();  // Play beep sound on each countdown
        if (countdownTime <= 0) {
          clearInterval(countdownInterval);
          setTimeout(showSuccessMessage, 1000); // Show success message after countdown
        }
      }, 1000);
    }

    // Show success message
    function showSuccessMessage() {
      countdownElement.style.display = 'none';
      showMessage(message2Element, "BRIAN HAS SUCCESSFULLY HACKED MERAB'S HEART🤣😂", function() {
        setTimeout(showMatrixFace, 1000); // Show matrix face after success message
      });
    }

    // Show matrix laughing face
    function showMatrixFace() {
      matrixFaceElement.style.display = 'block';
      matrixFaceElement.innerHTML = "_FINALLY👻, THE HEART🫀 OF MAUMBWA🐕🐶 ZANGU HAS BEEN STOLEN🛍🏃";
    }

    // Function to show each message
    function showMessage(element, text, callback) {
      typewriterEffect(element, text, 100, callback);  // Use the typewriter effect for text
    }

    // Initialize
    createMatrixEffect();
    loadingBar();
  </script>

</body>
</html>
