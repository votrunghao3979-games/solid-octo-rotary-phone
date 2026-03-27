# solid-octo-rotary- 
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Wordle Clone</title>
<style>
  body { font-family: Arial; text-align: center; }
  .board { display: grid; gap: 5px; justify-content: center; margin-bottom: 20px; }
  .row { display: grid; grid-template-columns: repeat(5, 60px); gap: 5px; }
  .cell {
    width: 60px; height: 60px;
    border: 2px solid #ccc;
    font-size: 28px;
    display: flex; align-items: center; justify-content: center;
    font-weight: bold;
    text-transform: uppercase;
  }
  .correct { background: #6aaa64; color: white; }
  .present { background: #c9b458; color: white; }
  .wrong { background: #787c7e; color: white; }

  .keyboard { max-width: 500px; margin: auto; }
  .key {
    display: inline-block;
    margin: 3px;
    padding: 10px;
    background: #ddd;
    cursor: pointer;
    border-radius: 5px;
  }
</style>
</head>
<body>

<h1>🎮 Wordle Clone</h1>

<div class="board" id="board"></div>

<div class="keyboard" id="keyboard"></div>

<script>
const words = ["apple", "grape", "mango", "peach", "lemon"];
const secret = words[Math.floor(Math.random() * words.length)];

let currentRow = 0;
let currentGuess = "";

const board = document.getElementById("board");

// tạo bảng
for (let i = 0; i < 6; i++) {
  const row = document.createElement("div");
  row.className = "row";
  for (let j = 0; j < 5; j++) {
    const cell = document.createElement("div");
    cell.className = "cell";
    row.appendChild(cell);
  }
  board.appendChild(row);
}

// bàn phím
const keys = "QWERTYUIOPASDFGHJKLZXCVBNM";
const keyboard = document.getElementById("keyboard");

keys.split("").forEach(k => {
  const key = document.createElement("div");
  key.className = "key";
  key.textContent = k;
  key.onclick = () => pressKey(k);
  keyboard.appendChild(key);
});

const enter = document.createElement("div");
enter.className = "key";
enter.textContent = "ENTER";
enter.onclick = submitGuess;
keyboard.appendChild(enter);

const del = document.createElement("div");
del.className = "key";
del.textContent = "⌫";
del.onclick = () => {
  currentGuess = currentGuess.slice(0, -1);
  updateBoard();
};
keyboard.appendChild(del);

function pressKey(letter) {
  if (currentGuess.length < 5) {
    currentGuess += letter.toLowerCase();
    updateBoard();
  }
}

function updateBoard() {
  const row = board.children[currentRow].children;
  for (let i = 0; i < 5; i++) {
    row[i].textContent = currentGuess[i] || "";
  }
}

function submitGuess() {
  if (currentGuess.length !== 5) return;

  const row = board.children[currentRow].children;

  for (let i = 0; i < 5; i++) {
    if (currentGuess[i] === secret[i]) {
      row[i].classList.add("correct");
    } else if (secret.includes(currentGuess[i])) {
      row[i].classList.add("present");
    } else {
      row[i].classList.add("wrong");
    }
  }

  if (currentGuess === secret) {
    setTimeout(() => alert("🎉 Thắng!"), 200);
    return;
  }

  currentRow++;
  currentGuess = "";

  if (currentRow === 6) {
    setTimeout(() => alert("💀 Thua! Từ: " + secret), 200);
  }
}
</script>

</body>
</html>
