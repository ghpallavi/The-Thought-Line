<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Train of Thoughts</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      text-align: center;
      background: linear-gradient(to top, #a1c4fd, #c2e9fb);
      color: #333;
      padding: 20px;
    }
    h1 {
      font-size: 2.5em;
      color: #5f6caf;
    }
    #game-board {
      margin: 40px auto;
      display: flex;
      gap: 20px;
      justify-content: center;
    }
    .tile {
      width: 100px;
      height: 100px;
      border-radius: 20px;
      background-color: #ccc;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    .glow {
      box-shadow: 0 0 15px 5px rgba(255, 255, 255, 0.8);
    }
    #status {
      font-size: 1.2em;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>🚂 Train of Thoughts</h1>
  <div id="game-board"></div>
  <div id="status">Watch the pattern...</div>

  <script>
    const colors = ['#ff9999', '#99ff99', '#9999ff', '#ffff99'];
    const board = document.getElementById('game-board');
    const statusText = document.getElementById('status');
    let pattern = [], userIndex = 0, round = 1;

    function createTiles() {
      colors.forEach((color, index) => {
        const tile = document.createElement('div');
        tile.classList.add('tile');
        tile.style.backgroundColor = color;
        tile.dataset.index = index;
        tile.addEventListener('click', handleUserInput);
        board.appendChild(tile);
      });
    }

    function glowTile(index) {
      const tile = board.children[index];
      tile.classList.add('glow');
      setTimeout(() => tile.classList.remove('glow'), 600);
    }

    function playPattern() {
      statusText.textContent = 'Watch the pattern...';
      userIndex = 0;
      pattern.push(Math.floor(Math.random() * colors.length));
      pattern.forEach((colorIndex, i) => {
        setTimeout(() => glowTile(colorIndex), 800 * i);
      });
      setTimeout(() => statusText.textContent = 'Your turn...', 800 * pattern.length);
    }

    function handleUserInput(e) {
      const index = parseInt(e.target.dataset.index);
      if (index === pattern[userIndex]) {
        glowTile(index);
        userIndex++;
        if (userIndex === pattern.length) {
          statusText.textContent = `Good job! Thought unlocked: \"${getThought()}\"`;
          setTimeout(playPattern, 1500);
        }
      } else {
        statusText.textContent = 'Oops... let's restart your journey.';
        pattern = [];
        setTimeout(playPattern, 2000);
      }
    }

    function getThought() {
      const thoughts = ['Clarity', 'Peace', 'Joy', 'Kindness', 'Focus', 'Balance'];
      return thoughts[Math.floor(Math.random() * thoughts.length)];
    }

    createTiles();
    playPattern();
  </script>
</body>
</html>
