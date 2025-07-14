<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>خلية الحروف</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: linear-gradient(to bottom, green 100px, orange 100px, orange calc(100% - 100px), green calc(100% - 100px));
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
    }
    .container {
      display: grid;
      grid-template-columns: repeat(5, 70px);
      grid-template-rows: repeat(5, 70px);
      gap: 5px;
      padding: 20px;
      background: white;
      border: 10px solid orange;
    }
    .cell {
      display: flex;
      justify-content: center;
      align-items: center;
      background: #f0f0f0;
      border: 1px solid #ccc;
      font-size: 24px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .cell.green { background: #4CAF50; color: white; }
    .cell.orange { background: orange; color: white; }
    .overlay {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 999;
    }
    .popup {
      background: white;
      padding: 20px;
      border-radius: 10px;
      text-align: center;
    }
    .popup button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
    }
    .winner {
      position: fixed;
      top: 20px;
      background: yellow;
      padding: 10px 20px;
      font-size: 20px;
      font-weight: bold;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <div class="winner" id="winner" style="display:none;"></div>
  <div class="container" id="grid"></div>
  <div class="overlay" id="overlay" style="display:none;">
    <div class="popup">
      <p>اختر اللون:</p>
      <button onclick="colorCell('green')">أخضر</button>
      <button onclick="colorCell('orange')">برتقالي</button>
    </div>
  </div>  <script>
    const letters = ['أ','ب','ت','ث','ج','ح','خ','د','ذ','ر','ز','س','ش','ص','ض','ط','ظ','ع','غ','ف','ق','ك','ل','م','ن'];
    const gridSize = 5;
    const grid = document.getElementById('grid');
    const overlay = document.getElementById('overlay');
    const winnerDiv = document.getElementById('winner');
    let selectedCell = null;

    function createGrid() {
      let index = 0;
      for (let i = 0; i < gridSize * gridSize; i++) {
        const cell = document.createElement('div');
        cell.className = 'cell';
        cell.dataset.index = i;
        cell.textContent = letters[index++] || '-';
        cell.addEventListener('click', () => selectCell(cell));
        grid.appendChild(cell);
      }
    }

    function selectCell(cell) {
      if (cell.classList.contains('green') || cell.classList.contains('orange')) return;
      selectedCell = cell;
      overlay.style.display = 'flex';
    }

    function colorCell(color) {
      if (selectedCell) {
        selectedCell.classList.add(color);
        overlay.style.display = 'none';
        checkWinner(color);
      }
    }

    function checkWinner(color) {
      const cells = Array.from(document.getElementsByClassName('cell'));
      const board = cells.map(cell => cell.classList.contains(color));

      // check rows
      for (let i = 0; i < gridSize; i++) {
        const start = i * gridSize;
