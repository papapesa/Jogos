<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Site de Jogos</title>
<style>
body { font-family: Arial,sans-serif; background:#f0f2f5; text-align:center; margin:0; }
header { background:#4CAF50; color:white; padding:1rem; }
button { margin:0.5rem; padding:1rem 2rem; font-size:1rem; border:none; border-radius:5px; cursor:pointer; background:#2196F3; color:white; transition:0.3s; }
button:hover { background:#0b7dda; }
#game-container { margin-top:2rem; }
#board div { width:100px; height:100px; display:flex; align-items:center; justify-content:center; font-size:2rem; border:1px solid #333; cursor:pointer; }
canvas { background:#000; display:block; margin:1rem auto; }
</style>
</head>
<body>

<header><h1>🎮 Meu Site de Jogos</h1></header>

<main>
<section class="games">
<button onclick="loadGame('tic-tac-toe')">Jogo da Velha</button>
<button onclick="loadGame('snake')">Snake</button>
<button onclick="loadGame('guess-number')">Adivinha o Número</button>
</section>

<section id="game-container"><p>Escolha um jogo para começar!</p></section>
</main>

<script>
// ------------------ Jogo da Velha ------------------
function startTicTacToe(container){
container.innerHTML = `<h2>Jogo da Velha</h2><div id="board" style="display:grid; grid-template-columns: repeat(3, 100px); gap:5px;"></div><p id="status"></p><button onclick="startTicTacToe(container)">Reiniciar</button>`;
const board = container.querySelector('#board'); const status = container.querySelector('#status');
let currentPlayer='X'; let cells=Array(9).fill(null);
function checkWinner(){
const lines=[[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];
for(let [a,b,c] of lines) if(cells[a] && cells[a]===cells[b] && cells[a]===cells[c]) return cells[a];
return cells.includes(null)?null:'Empate';
}
for(let i=0;i<9;i++){
let cell=document.createElement('div'); cell.addEventListener('click',()=>{
if(cells[i] || checkWinner()) return; cells[i]=currentPlayer; cell.textContent=currentPlayer;
let winner=checkWinner(); if(winner){status.textContent=winner==='Empate'?'Empate!':'Vencedor: '+winner;} else {currentPlayer=currentPlayer==='X'?'O':'X'; status.textContent='Vez de '+currentPlayer;}}); board.appendChild(cell);
} status.textContent='Vez de '+currentPlayer;
}

// ------------------ Snake ------------------
function startSnake(container){
container.innerHTML='<h2>Snake</h2><canvas id="snakeCanvas" width="300" height="300"></canvas><p id="score">Score: 0</p><button onclick="startSnake(container)">Reiniciar</button>';
const canvas=document.getElementById('snakeCanvas'); const ctx=canvas.getContext('2d');
const box=20; let snake=[{x:5*box,y:5*box}]; let dir='RIGHT'; let food={x:Math.floor(Math.random()*15)*box,y:Math.floor(Math.random()*15)*box}; let score=0;
document.addEventListener('keydown',e=>{if(e.key==='ArrowUp'&&dir!=='DOWN')dir='UP'; if(e.key==='ArrowDown'&&dir!=='UP')dir='DOWN'; if(e.key==='ArrowLeft'&&dir!=='RIGHT')dir='LEFT'; if(e.key==='ArrowRight'&&dir!=='LEFT')dir='RIGHT';});
function draw(){
ctx.fillStyle='black'; ctx.fillRect(0,0,canvas.width,canvas.height);
for(let s of snake){ctx.fillStyle='lime'; ctx.fillRect(s.x,s.y,box,box);}
ctx.fillStyle='red'; ctx.fillRect(food.x,food.y,box,box);
let head={x:snake[0].x,y:snake[0].y};
if(dir==='LEFT') head.x-=box; if(dir==='RIGHT') head.x+=box; if(dir==='UP') head.y-=box; if(dir==='DOWN') head.y+=box;
if(head.x<0||head.x>=300||head.y<0||head.y>=300||snake.some(s=>s.x===head.x&&s.y===head.y)){alert('Game Over'); return;}
snake.unshift(head);
if(head.x===food.x && head.y===food.y){score++; document.getElementById('score').textContent='Score: '+score; food={x:Math.floor(Math.random()*15)*box,y:Math.floor(Math.random()*15)*box};} else {snake.pop();}
setTimeout(draw,100);
} draw();
}

// ------------------ Adivinha Número ------------------
function startGuessNumber(container){
container.innerHTML='<h2>Adivinha o Número</h2><p>Escolha um número entre 1 e 100</p><input type="number" id="guessInput" min="1" max="100"><button id="guessBtn">Chutar</button><p id="result"></p><button onclick="startGuessNumber(container)">Reiniciar</button>';
let number=Math.floor(Math.random()*100)+1;
document.getElementById('guessBtn').onclick=()=>{
let val=parseInt(document.getElementById('guessInput').value);
let result=document.getElementById('result');
if(val===number) result.textContent='🎉 Acertou!'; else if(val<number) result.textContent='⬆ Tente maior'; else result.textContent='⬇ Tente menor';
};
}

// ------------------ Loader ------------------
function loadGame(game){const container=document.getElementById('game-container'); container.innerHTML=''; switch(game){case 'tic-tac-toe':startTicTacToe(container); break; case 'snake':startSnake(container); break; case 'guess-number':startGuessNumber(container); break;}}
</script>

</body>
</html>
