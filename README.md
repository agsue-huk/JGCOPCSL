<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mini Game Gabriella</title>

<style>

body{
margin:0;
overflow:hidden;
background:linear-gradient(#ff9ecf,#ff5fa2);
font-family:Arial;
}

#game{
position:relative;
width:100vw;
height:100vh;
overflow:hidden;
}

#player{
position:absolute;
width:60px;
height:60px;
background:white;
border-radius:20px;
left:100px;
bottom:100px;
display:flex;
align-items:center;
justify-content:center;
font-weight:bold;
color:#ff1493;
font-size:14px;
}

.coin{
position:absolute;
width:40px;
height:40px;
border-radius:50%;
background:gold;
box-shadow:0 0 15px yellow;
}

#score{
position:absolute;
top:20px;
left:20px;
background:#ffffffaa;
padding:15px;
border-radius:15px;
font-size:20px;
color:#ff1493;
font-weight:bold;
}

</style>
</head>

<body>

<div id="game">

<div id="score">
Moedas: 0
</div>

<div id="player">
Gabriella
</div>

</div>

<script>

const player =
document.getElementById('player');

const game =
document.getElementById('game');

const scoreText =
document.getElementById('score');

let x = 100;
let y = 100;

let score = 0;

const speed = 8;

const keys = {};

document.addEventListener('keydown',e=>{
keys[e.key]=true;
});

document.addEventListener('keyup',e=>{
keys[e.key]=false;
});

function movePlayer(){

if(keys['a']){
x-=speed;
}

if(keys['d']){
x+=speed;
}

if(keys['w']){
y+=speed;
}

if(keys['s']){
y-=speed;
}

x=Math.max(0,
Math.min(window.innerWidth-60,x));

y=Math.max(0,
Math.min(window.innerHeight-60,y));

player.style.left=x+'px';

player.style.bottom=y+'px';

}

function createCoin(){

const coin =
document.createElement('div');

coin.classList.add('coin');

coin.style.left=
Math.random()*
(window.innerWidth-40)+'px';

coin.style.top=
Math.random()*
(window.innerHeight-40)+'px';

game.appendChild(coin);

return coin;

}

let coin = createCoin();

function checkCollision(){

const p =
player.getBoundingClientRect();

const c =
coin.getBoundingClientRect();

if(
!(p.right<c.left ||
p.left>c.right ||
p.bottom<c.top ||
p.top>c.bottom)
){

score++;

scoreText.innerHTML=
'Moedas: '+score;

coin.remove();

coin=createCoin();

}

}

function gameLoop(){

movePlayer();

checkCollision();

requestAnimationFrame(gameLoop);

}

gameLoop();

</script>

</body>
</html>
