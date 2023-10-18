<!DOCTYPE html>
<html>
<head>
  <style>
    #game-container {
      width: 400px;
      height: 400px;
      background-color: #f0f0f0;
      position: relative;
      overflow: hidden;
    }
    .player {
      width: 50px;
      height: 50px;
      position: absolute;
      bottom: 0;
      left: 0;
      background-image: url('gorilla.png'); /* Use the gorilla image */
      background-size: cover;
    }
    .platform {
      width: 50px;
      height: 10px;
      position: absolute;
      background-color: #333;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <div id="player" class="player"></div>
  </div>
  <div>
    <label for="characterSelect">Select Character:</label>
    <select id="characterSelect">
      <option value="blue">Blue</option>
      <option value="red">Red</option>
      <option value="green">Green</option>
    </select>
    <div id="score">Score: 0</div>
  </div>

  <script>
    const player = document.getElementById('player');
    const gameContainer = document.getElementById('game-container');
    const scoreElement = document.getElementById('score');
    let jumping = false;
    let playerBottom = 0;
    let score = 0;
    const jumpHeight = 100;
    const jumpSpeed = 3;

    document.addEventListener('keydown', (event) => {
      if (event.key === ' ' && !jumping) {
        jumping = true;
        jump();
      }
    });

    function jump() {
      function animateJump() {
        if (playerBottom < jumpHeight) {
          player.style.bottom = playerBottom + 'px';
          playerBottom += jumpSpeed;
          requestAnimationFrame(animateJump);
        } else {
          animateFall();
        }
      }

      function animateFall() {
        if (playerBottom > 0) {
          player.style.bottom = playerBottom + 'px';
          playerBottom -= jumpSpeed;
          requestAnimationFrame(animateFall);
        } else {
          player.style.bottom = '0';
          jumping = false;
          score++;
          scoreElement.textContent = `Score: ${score}`;
          createPlatform();
        }
      }

      animateJump();
    }

    function createPlatform() {
      const platform = document.createElement('div');
      platform.classList.add('platform');
      platform.style.left = Math.random() * 350 + 'px';
      platform.style.bottom = '0';
      gameContainer.appendChild(platform);

      // Randomly adjust platform movement speed and direction
      let platformDirection = Math.random() < 0.5 ? 1 : -1; // Random direction
      let platformSpeed = Math.random() * 2 + 1; // Random speed

      function movePlatform() {
        let left = parseInt(getComputedStyle(platform).left, 10);
        left += platformDirection * platformSpeed;

        // Ensure platforms stay within the game container
        if (left < 0 || left > 350) {
          platformDirection = -platformDirection;
        }

        platform.style.left = left + 'px';
        requestAnimationFrame(movePlatform);
      }

      requestAnimationFrame(movePlatform);
    }

    // Character customization
    const characterSelect = document.getElementById('characterSelect');

    characterSelect.addEventListener('change', (event) => {
      const selectedCharacter = event.target.value;
      player.className = `player ${selectedCharacter}`;
    }

    // Game loop
    setInterval(movePlatforms, 10);
  </script>
</body>
</html>

