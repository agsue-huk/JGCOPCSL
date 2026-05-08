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
z-index:20;
}

h1{
font-size:4rem;
text-shadow:0 0 15px rgba(255,255,255,0.5);
}

.subtitle{
font-size:1.2rem;
opacity:0.9;
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
transition:0.3s;
}

button:hover{
transform:scale(1.08);
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
border:2px solid #ffffff44;
border-radius:15px;
}

.player{
position:absolute;
width:60px;
height:60px;
border-radius:20px;
display:flex;
align-items:center;
justify-content:center;
font-weight:bold;
font-size:28px;
}

#player1{
background:#ffc0cb;
color:#ff0066;
box-shadow:0 0 25px #ffb6c1;
}

#player2{
background:#0d1117;
color:#00ffee;
box-shadow:0 0 25px #00ffee;
}

.portal{
position:absolute;
width:100px;
height:100px;
border-radius:50%;
background:radial-gradient(circle,#ffffff,#00ffcc,#00aa88);
box-shadow:0 0 40px #00ffcc;
animation:pulse 2s infinite;
}

@keyframes pulse{
0%{transform:scale(1)}
50%{transform:scale(1.1)}
100%{transform:scale(1)}
}

.ui{
position:absolute;
top:20px;
left:20px;
background:#00000055;
padding:18px;
border-radius:20px;
backdrop-filter:blur(8px);
z-index:10;
}

.mode{
font-size:1.2rem;
margin-top:10px;
}

.message{
position:absolute;
bottom:20px;
left:50%;
transform:translateX(-50%);
background:#00000077;
padding:15px 30px;
border-radius:20px;
backdrop-filter:blur(8px);
}

.star{
position:absolute;
width:4px;
height:4px;
background:white;
border-radius:50%;
animation:twinkle 3s infinite;
}

@keyframes twinkle{
0%{opacity:0.2}
50%{opacity:1}
100%{opacity:0.2}
}
</style>
</head>
<body>

<div id="menu">
<h1>💚 Love Dimension 💚</h1>
<p class="subtitle">Jogo cooperativo inspirado em aventuras emocionais</p>
<button onclick="startGame(1)">🎮 Jogar Solo</button>
<button onclick="startGame(2)">👫 Jogar Cooperativo</button>
</div>

<div id="game">
<div class="ui">
<h2>✨ Nível 1/24</h2>
<p>Jardim das Primeiras Conversas</p>
<div class="mode" id="modeText"></div>
<br>
<p><b>Gabriella:</b> WASD</p>
<p><b>Diego:</b> Setas</p>
</div>

<div class="message">
Leve os personagens até o portal 💚
</div>

<div class="platform" style="left:0;bottom:0;width:100%;height:100px"></div>
<div class="platform" style="left:180px;bottom:220px;width:240px;height:30px"></div>
<div class="platform" style="left:550px;bottom:340px;width:240px;height:30px"></div>
<div class="platform" style="left:920px;bottom:250px;width:240px;height:30px"></div>

<div id="player1" class="player">Gabriella</div>
<div id="player2" class="player">Diego</div>

<div id="portal" class="portal" style="right:120px;bottom:130px"></div>
</div>

<script>
const game=document.getElementById('game');
const menu=document.getElementById('menu');
const modeText=document.getElementById('modeText');

const p1=document.getElementById('player1');
const p2=document.getElementById('player2');
const portal=document.getElementById('portal');

let multiplayer=false;

let player1={x:100,y:500,vx:0,vy:0,onGround:false};
let player2={x:220,y:500,vx:0,vy:0,onGround:false};

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
modeText.innerHTML='🎮 Modo Solo';
}
else{
multiplayer=true;
modeText.innerHTML='👫 Modo Cooperativo';
}
}

function updatePlayer(player,element,left,right,jumpKey){
if(keys[left]) player.vx=-speed;
else if(keys[right]) player.vx=speed;
else player.vx=0;

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

const platforms=document.querySelectorAll('.platform');
platforms.forEach(platform=>{
const rect=platform.getBoundingClientRect();

if(player.x+60>rect.left && player.x<rect.right && player.y+60>rect.top && player.y+60<rect.top+25 && player.vy>=0){
player.y=rect.top-60;
player.vy=0;
player.onGround=true;
}
});

if(player.x<0) player.x=0;
if(player.x>window.innerWidth-60) player.x=window.innerWidth-60;

element.style.left=player.x+'px';
element.style.top=player.y+'px';
}

function checkWin(){
const portalRect=portal.getBoundingClientRect();
const p1Rect=p1.getBoundingClientRect();
const p2Rect=p2.getBoundingClientRect();

const p1Inside=Math.abs(p1Rect.left-portalRect.left)<90;
const p2Inside=Math.abs(p2Rect.left-portalRect.left)<90;

if(multiplayer){
if(p1Inside && p2Inside){
setTimeout(()=>{
alert('💚 Vocês completaram o nível!');
},200);
}
}
else{
if(p1Inside){
setTimeout(()=>{
alert('💚 Você completou o nível!');
},200);
}
}
}

function createStars(){
for(let i=0;i<80;i++){
const star=document.createElement('div');
star.classList.add('star');
star.style.left=Math.random()*window.innerWidth+'px';
star.style.top=Math.random()*window.innerHeight+'px';
star.style.animationDelay=Math.random()*3+'s';
document.body.appendChild(star);
}
}

function gameLoop(){
updatePlayer(player1,p1,'a','d','w');

if(multiplayer){
updatePlayer(player2,p2,'ArrowLeft','ArrowRight','ArrowUp');
p2.style.display='flex';
}
else{
p2.style.display='none';
}

checkWin();
requestAnimationFrame(gameLoop);
}

createStars();
gameLoop();
</script>
</body>
</html>

