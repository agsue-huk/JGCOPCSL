<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Love Dimension</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
overflow:hidden;
background:linear-gradient(180deg,#ff7eb9,#ff4d88,#14532d);
height:100vh;
color:white;
}

#menu{
position:absolute;
inset:0;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
gap:20px;
background:linear-gradient(180deg,#ff7eb9,#ff4d88,#14532d);
z-index:10;
}

h1{
font-size:4rem;
}

button{
padding:15px 35px;
font-size:1.1rem;
border:none;
border-radius:18px;
cursor:pointer;
background:white;
color:#14532d;
font-weight:bold;
}

#game{
position:relative;
width:100vw;
height:100vh;
display:none;
overflow:hidden;
}

.platform{
position:absolute;
background:#ffffff33;
border-radius:15px;
}

.player{
position:absolute;
width:70px;
height:70px;
border-radius:20px;
display:flex;
align-items:center;
justify-content:center;
font-weight:bold;
font-size:16px;
}

#player1{
background:#ffc0cb;
color:#ff0066;
}

#player2{
background:#0d1117;
color:#00ffee;
}

.portal{
position:absolute;
width:100px;
height:100px;
border-radius:50%;
background:radial-gradient(circle,#ffffff,#00ffcc,#00aa88);
box-shadow:0 0 40px #00ffcc;
}

.ui{
position:absolute;
top:20px;
left:20px;
background:#00000055;
padding:18px;
border-radius:20px;
}

.message{
position:absolute;
bottom:20px;
left:50%;
transform:translateX(-50%);
background:#00000077;
padding:15px 30px;
border-radius:20px;
}

</style>
</head>

<body>

<div id="menu">

<h1>💚 Love Dimension 💚</h1>

<button onclick="startGame(1)">
🎮 Jogar Solo
</button>

<button onclick="startGame(2)">
👫 Jogar Cooperativo
</button>

</div>

<div id="game">

<div class="ui">

<h2 id="levelText">
✨ Nível 1/24
</h2>

<p>
Jardim das Primeiras Conversas
</p>

<br>

<p><b>Gabriella:</b> WASD</p>
<p><b>Diego:</b> Setas</p>

</div>

<div class="message">
Chegue até o portal 💚
</div>

<div class="platform"
style="left:0;bottom:0;width:100%;height:100px">
</div>

<div class="platform"
style="left:180px;bottom:220px;width:240px;height:30px">
</div>

<div class="platform"
style="left:550px;bottom:340px;width:240px;height:30px">
</div>

<div class="platform"
style="left:920px;bottom:250px;width:240px;height:30px">
</div>

<div id="player1" class="player">
Gabriella
</div>

<div id="player2" class="player">
Diego
</div>

<div id="portal"
class="portal"
style="right:120px;bottom:130px">
</div>

</div>

<script>

const game=document.getElementById('game');
const menu=document.getElementById('menu');

const p1=document.getElementById('player1');
const p2=document.getElementById('player2');

const portal=document.getElementById('portal');

const levelText=document.getElementById('levelText');

let multiplayer=false;

let currentLevel=1;

let player1={
x:100,
y:500,
vx:0,
vy:0,
onGround:false
};

let player2={
x:220,
y:500,
vx:0,
vy:0,
onGround:false
};

const gravity=0.7;
const speed=5;
const jump=14;

const keys={};

document.addEventListener('keydown',e=>{
keys[e.key]=true;
});

document.addEventListener('keyup',e=>{
keys[e.key]=false;
});

function startGame(players){

menu.style.display='none';

game.style.display='block';

if(players===1){
multiplayer=false;
}
else{
multiplayer=true;
}

initializeLevel();

}

function initializeLevel(){

player1.x=100;
player1.y=500;
player1.vx=0;
player1.vy=0;

player2.x=220;
player2.y=500;
player2.vx=0;
player2.vy=0;

levelText.innerHTML=
'✨ Nível '+currentLevel+'/24';

}

function updatePlayer(
player,
element,
left,
right,
jumpKey
){

if(keys[left]){
player.vx=-speed;
}
else if(keys[right]){
player.vx=speed;
}
else{
player.vx=0;
}

if(keys[jumpKey] && player.onGround){
player.vy=-jump;
player.onGround=false;
}

player.vy+=gravity;

player.x+=player.vx;
player.y+=player.vy;

if(player.y>500){
player.y=500;
player.vy=0;
player.onGround=true;
}

if(player.x<0){
player.x=0;
}

if(player.x>window.innerWidth-70){
player.x=window.innerWidth-70;
}

element.style.left=player.x+'px';
element.style.top=player.y+'px';

}

function checkWin(){

const portalRect=portal.getBoundingClientRect();

const p1Rect=p1.getBoundingClientRect();

const p2Rect=p2.getBoundingClientRect();

const p1Inside=
Math.abs(p1Rect.left-portalRect.left)<90;

const p2Inside=
Math.abs(p2Rect.left-portalRect.left)<90;

if(multiplayer){

if(p1Inside && p2Inside){

nextLevel();

}

}
else{

if(p1Inside){

nextLevel();

}

}

}

function nextLevel(){

currentLevel++;

if(currentLevel>24){

alert(
'💚 Parabéns! Vocês terminaram Love Dimension!'
);

location.reload();

return;

}

alert(
'💚 Nível '+(currentLevel-1)+' concluído!'
);

initializeLevel();

}

function gameLoop(){

updatePlayer(
player1,
p1,
'a',
'd',
'w'
);

if(multiplayer){

p2.style.display='flex';

updatePlayer(
player2,
p2,
'ArrowLeft',
'ArrowRight',
'ArrowUp'
);

}
else{

p2.style.display='none';

}

checkWin();

requestAnimationFrame(gameLoop);

}

gameLoop();

</script>

</body>
</html>

