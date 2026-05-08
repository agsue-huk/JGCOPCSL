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
font-family:Arial, sans-serif;
}

body{
background:linear-gradient(180deg,#0b3d2e,#14532d,#1f7a4f);
overflow:hidden;
color:white;
height:100vh;
}

#game{
position:relative;
width:100vw;
height:100vh;
overflow:hidden;
}

.background{
position:absolute;
inset:0;
opacity:0.08;
font-size:40px;
line-height:60px;
pointer-events:none;
}

.platform{
position:absolute;
background:#ffffff22;
border:2px solid #ffffff33;
border-radius:15px;
}

.player{
position:absolute;
width:55px;
height:55px;
border-radius:15px;
display:flex;
align-items:center;
justify-content:center;
font-size:28px;
font-weight:bold;
transition:transform 0.1s;
}

#player1{
background:#90ee90;
color:#14532d;
}

#player2{
background:#0d1117;
color:white;
}

.portal{
position:absolute;
width:90px;
height:90px;
border-radius:50%;
background:radial-gradient(circle,#ffffff,#7fffd4,#00ff99);
box-shadow:0 0 40px #7fffd4;
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
z-index:10;
background:#00000055;
padding:18px;
border-radius:20px;
backdrop-filter:blur(8px);
}

.ui h1{
font-size:2rem;
margin-bottom:10px;
}

.controls{
margin-top:10px;
font-size:0.9rem;
line-height:1.6;
}

.level-box{
position:absolute;
top:20px;
right:20px;
background:#00000055;
padding:18px;
border-radius:20px;
backdrop-filter:blur(8px);
font-size:1.2rem;
}

.message{
position:absolute;
bottom:30px;
left:50%;
transform:translateX(-50%);
background:#00000088;
padding:16px 25px;
border-radius:20px;
font-size:1.1rem;
backdrop-filter:blur(8px);
}

.star{
position:absolute;
width:5px;
height:5px;
background:white;
border-radius:50%;
opacity:0.8;
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
<div id="game">
<div class="background">
G D G D G D G D G D G D G D G D G D G D G D G D G D
<br><br>
LOVE DIMENSION LOVE DIMENSION LOVE DIMENSION
<br><br>
G D G D G D G D G D G D G D G D G D G D G D G D
</div>

<div class="ui">
<h1>💚 Love Dimension 💚</h1>
<p>Jogo cooperativo inspirado em aventuras emocionais</p>
<div class="controls">
<b>Player G</b>: WASD<br>
<b>Player D</b>: Setas do teclado
</div>
</div>

<div class="level-box">
✨ Nível 1/24<br>
Jardim das Primeiras Conversas
</div>

<div class="message">
Levem os dois personagens até o portal 💚
</div>

<div class="platform" style="left:0;bottom:0;width:100%;height:100px"></div>
<div class="platform" style="left:200px;bottom:220px;width:250px;height:30px"></div>
<div class="platform" style="left:600px;bottom:360px;width:250px;height:30px"></div>
<div class="platform" style="left:1000px;bottom:230px;width:200px;height:30px"></div>

<div id="player1" class="player">G</div>
<div id="player2" class="player">D</div>

<div class="portal" id="portal" style="right:120px;bottom:140px"></div>
</div>

<script>
const p1=document.getElementById('player1');
const p2=document.getElementById('player2');
const portal=document.getElementById('portal');

let player1={x:100,y:500,vx:0,vy:0,onGround:false};
let player2={x:220,y:500,vx:0,vy:0,onGround:false};

const gravity=0.7;
const speed=5;
const jump=14;

const keys={};

document.addEventListener('keydown',(e)=>{
keys[e.key]=true;
});

document.addEventListener('keyup',(e)=>{
keys[e.key]=false;
});

function updatePlayer(player,element,controls){
if(keys[controls.left]) player.vx=-speed;
else if(keys[controls.right]) player.vx=speed;
else player.vx=0;

if(keys[controls.jump] && player.onGround){
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

const px=player.x;
const py=player.y;

if(px+55>rect.left && px<rect.right && py+55>rect.top && py+55<rect.top+25 && player.vy>=0){
player.y=rect.top-55;
player.vy=0;
player.onGround=true;
}
});

if(player.x<0) player.x=0;
if(player.x>window.innerWidth-55) player.x=window.innerWidth-55;

element.style.left=player.x+'px';
element.style.top=player.y+'px';
}

function checkWin(){
const portalRect=portal.getBoundingClientRect();
const p1Rect=p1.getBoundingClientRect();
const p2Rect=p2.getBoundingClientRect();

const p1Inside=(Math.abs(p1Rect.left-portalRect.left)<80);
const p2Inside=(Math.abs(p2Rect.left-portalRect.left)<80);

if(p1Inside && p2Inside){
setTimeout(()=>{
alert('💚 Vocês completaram o Nível 1!');
},200);
}
}

function createStars(){
for(let i=0;i<70;i++){
const star=document.createElement('div');
star.classList.add('star');
star.style.left=Math.random()*window.innerWidth+'px';
star.style.top=Math.random()*window.innerHeight+'px';
star.style.animationDelay=Math.random()*3+'s';
document.body.appendChild(star);
}
}

function gameLoop(){
updatePlayer(player1,p1,{left:'a',right:'d',jump:'w'});
updatePlayer(player2,p2,{left:'ArrowLeft',right:'ArrowRight',jump:'ArrowUp'});
checkWin();
requestAnimationFrame(gameLoop);
}

createStars();
gameLoop();
</script>
</body>
</html>
