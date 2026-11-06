# saturo-v4-delta-force
Jeu de guerre style Delta Force - SATÜRO V4 - Korhogo à Abidjan
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>SATÜRO V4 - DELTA FORCE STYLE</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{background:#000;font-family:Arial,sans-serif;overflow:hidden;touch-action:none;color:white}
#topBar{display:flex;justify-content:space-between;align-items:center;background:linear-gradient(90deg,#111,#1a2a00,#111);padding:6px 10px;font-weight:bold;font-size:12px;border-bottom:2px solid #ffcc00}
#topBar b{color:#ffcc00}
#gameWrap{position:relative;width:100%;height:58vh;overflow:hidden;background:#000}
#bg{position:absolute;inset:0;background-image:url('https://images.unsplash.com/photo-1541185934-01b600069069?w=900&q=80');background-size:cover;background-position:center;filter:brightness(0.85) contrast(1.1)}
#bg.abidjan{background-image:url('https://images.unsplash.com/photo-1577948000111-9c970dfe3743?w=900&q=80')}
#vignette{position:absolute;inset:0;background:radial-gradient(circle,transparent 55%,rgba(0,0,0,0.7) 100%)}
#crosshair{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:28px;opacity:0.9;z-index:5;text-shadow:0 0 5px #000}
#hudBottom{position:absolute;bottom:8px;left:8px;right:8px;display:flex;justify-content:space-between;align-items:flex-end;z-index:6}
#healthBar{width:130px;height:12px;background:#333;border:1px solid #fff;border-radius:6px;overflow:hidden}
#healthFill{height:100%;background:#00ff00;width:100%;transition:width 0.2s}
#ammo{font-weight:bold;font-size:16px;background:rgba(0,0,0,0.6);padding:5px 10px;border-radius:8px;border:1px solid #ffcc00}
#minimap{width:80px;height:80px;background:rgba(0,0,0,0.7);border:2px solid #00ff00;border-radius:50%;position:relative;overflow:hidden}
#minimapDot{position:absolute;width:8px;height:8px;background:#ffcc00;border-radius:50%;left:35px;top:35px}
#enemyDot{position:absolute;width:6px;height:6px;background:red;border-radius:50%}
#killFeed{position:absolute;top:8px;right:8px;background:rgba(0,0,0,0.6);padding:5px 8px;border-radius:6px;font-size:11px;z-index:6}
#enemiesLayer{position:absolute;inset:0;z-index:4}
.enemy{position:absolute;width:70px;height:95px;background-size:cover;background-position:center;border:2px solid red;border-radius:8px;transition:left 0.1s}
.enemy::after{content:'👹';position:absolute;top:-18px;left:20px;font-size:16px}
#controls{position:fixed;bottom:0;left:0;right:0;background:#111;padding:8px;display:flex;justify-content:space-between;align-items:center;z-index:20}
.btn{width:56px;height:56px;border-radius:50%;border:2px solid #fff;background:#333;color:white;display:flex;align-items:center;justify-content:center;font-size:20px;user-select:none}
.btn:active{transform:scale(0.9)}
#shootBtn{background:#ff2d55;width:72px;height:72px;font-weight:bold;font-size:12px;border-color:#ffcc00}
#scopeBtn{background:#0055ff}
#startScreen,#gameOver,#winScreen{position:absolute;inset:0;background:rgba(0,0,0,0.88);display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;z-index:30;padding:15px}
#startScreen img{width:100%;max-width:280px;border-radius:12px;border:3px solid #ffcc00;margin-bottom:10px}
#gameOver,#winScreen{display:none}
#playBtn{background:#ffcc00;color:#000;border:none;padding:14px 35px;font-size:20px;font-weight:bold;border-radius:30px;margin-top:12px}
.muzzle{position:absolute;width:40px;height:40px;background:radial-gradient(circle,#ffcc00,#ff6600,transparent);border-radius:50%;pointer-events:none;z-index:7}
</style>
</head>
<body>
<div id="topBar"><div>🎖️ SATÜRO <b>V4</b> • DELTA FORCE STYLE</div><div>SCORE: <b id="score">0</b></div><div id="levelTxt">KORHOGO</div></div>
<div id="gameWrap">
<div id="bg"></div>
<div id="vignette"></div>
<div id="enemiesLayer"></div>
<div id="crosshair">✛</div>
<div id="killFeed">🎯 Ennemis tués: <b id="kills">0</b></div>
<div id="hudBottom">
<div><div style="font-size:11px">❤️ VIE</div><div id="healthBar"><div id="healthFill"></div></div><div id="livesTxt" style="font-size:11px;margin-top:3px">100 / 100</div></div>
<div id="minimap"><div id="minimapDot"></div><div id="enemyDot" style="left:55px;top:20px"></div><div style="position:absolute;bottom:2px;left:50%;transform:translateX(-50%);font-size:8px;color:#00ff00">MAP</div></div>
<div id="ammo">🔫 <span id="ammoCount">30</span> / 120<br><span style="font-size:11px">M4A1 • AUTO</span></div>
</div>
<div id="startScreen">
<img src="https://images.unsplash.com/photo-1595590424283-b8f17842773f?w=600&q=80">
<h2 style="font-size:32px;color:#ffcc00">SATÜRO V4</h2>
<p style="margin:6px">🎖️ STYLE DELTA FORCE • GUERRE RÉALISTE</p>
<p style="font-size:13px;opacity:0.8">Korhogo → Abidjan • Escouade de 5 • Boss Final</p>
<p style="font-size:12px;margin-top:6px">🎯 Vise au centre • 🔥 Tire • 🧭 Survie</p>
<button id="playBtn" onclick="startGame()">▶️ DÉPLOYER</button>
<p style="font-size:11px;margin-top:8px;opacity:0.6">Par Satüro • Côte d'Ivoire 🇨🇮</p>
</div>
<div id="gameOver"><h2 style="color:#ff2d55;font-size:30px">💀 MORT AU COMBAT</h2><p id="finalScore"></p><button onclick="location.reload()" style="background:#ffcc00;border:none;padding:12px 25px;border-radius:25px;font-weight:bold;margin-top:10px">🔄 RE-DÉPLOYER</button></div>
<div id="winScreen"><h2 style="color:#00ff88;font-size:28px">🏆 MISSION RÉUSSIE !</h2><p>Tu as libéré Abidjan !</p><p id="winScore"></p><button onclick="location.reload()" style="background:#ffcc00;border:none;padding:12px 25px;border-radius:25px;font-weight:bold;margin-top:10px">🔄 REJOUER</button></div>
</div>
<div style="background:#222;padding:6px;display:flex;justify-content:center;gap:8px">
<button onclick="setWeapon('M4')" style="background:#ffcc00;border:none;padding:7px 12px;border-radius:15px;font-weight:bold;font-size:12px">M4A1</button>
<button onclick="setWeapon('SNIPER')" style="background:#333;color:white;border:1px solid #555;padding:7px 12px;border-radius:15px;font-weight:bold;font-size:12px">🎯 SNIPER</button>
<button onclick="setWeapon('BAZOOKA')" style="background:#333;color:white;border:1px solid #555;padding:7px 12px;border-radius:15px;font-weight:bold;font-size:12px">💥 BAZOOKA</button>
<button onclick="reload()" style="background:#0055ff;color:white;border:none;padding:7px 12px;border-radius:15px;font-weight:bold;font-size:12px">🔄 RECHARGER</button>
</div>
<div id="controls">
<div style="display:flex;gap:8px"><div class="btn" id="left">◀️</div><div class="btn" id="right">▶️</div></div>
<div style="display:flex;gap:8px;align-items:center"><div class="btn" id="scopeBtn" onclick="toggleScope()">🎯</div><div class="btn" id="shootBtn" onclick="shoot()">🔥<br>TIRER</div></div>
</div>
<script>
let score=0,kills=0,health=100,ammo=30,playing=false,level=1,scoped=false,weapon='M4';
const layer=document.getElementById('enemiesLayer');
const bg=document.getElementById('bg');
function startGame(){document.getElementById('startScreen').style.display='none';playing=true;spawnLoop();}
function setWeapon(w){weapon=w;alert('Arme: '+w);if(w==='SNIPER') ammo=10;if(w==='BAZOOKA') ammo=5;if(w==='M4') ammo=30;updateHUD();}
function reload(){ammo=weapon==='SNIPER'?10:weapon==='BAZOOKA'?5:30;updateHUD();}
function toggleScope(){scoped=!scoped;document.getElementById('crosshair').style.fontSize=scoped?'45px':'28px';document.getElementById('crosshair').textContent=scoped?'◎':'✛';}
function updateHUD(){document.getElementById('score').textContent=score;document.getElementById('kills').textContent=kills;document.getElementById('ammoCount').textContent=ammo;document.getElementById('healthFill').style.width=health+'%';document.getElementById('healthFill').style.background=health>50?'#00ff00':health>25?'#ffcc00':'red';document.getElementById('livesTxt').textContent=health+' / 100';document.getElementById('levelTxt').textContent=level===1?'KORHOGO':'ABIDJAN 🌆';}
function shoot(){
 if(!playing||ammo<=0){if(ammo<=0) alert('Plus de balles ! Clique RECHARGER');return;}
 ammo--;updateHUD();
 let m=document.createElement('div');m.className='muzzle';m.style.left='48%';m.style.top='48%';document.getElementById('gameWrap').appendChild(m);setTimeout(()=>m.remove(),120);
 let enemies=document.querySelectorAll('.enemy');
 if(enemies.length>0){
  let target=enemies[Math.floor(Math.random()*enemies.length)];
  if(Math.random()>0.25){target.remove();kills++;score+=100;if(kills===8&&level===1){level=2;bg.classList.add('abidjan');alert('🌆 MISSION 2 : ABIDJAN ! Tu avances !');}if(kills>=15){winGame();}}
 }
 if(Math.random()>0.7){health-=10;updateHUD();if(health<=0) gameOver();}
}
function spawnLoop(){
 if(!playing) return;
 if(document.querySelectorAll('.enemy').length<3){
  let e=document.createElement('div');e.className='enemy';
  e.style.left=(Math.random()*75+10)+'%';e.style.top=(Math.random()*45+15)+'%';
  e.style.backgroundImage="url('https://images.unsplash.com/photo-1541185934-01b600069069?w=300&q=80')";
  e.onclick=()=>{e.remove();kills++;score+=100;updateHUD();};
  layer.appendChild(e);
 }
 setTimeout(spawnLoop,1500);
}
function gameOver(){playing=false;document.getElementById('finalScore').textContent='Score: '+score+' • Kills: '+kills;document.getElementById('gameOver').style.display='flex';}
function winGame(){playing=false;document.getElementById('winScore').textContent='Score: '+score+' • Kills: '+kills+' • Tu es un héros !';document.getElementById('winScreen').style.display='flex';}
document.getElementById('shootBtn').addEventListener('touchstart',e=>{e.preventDefault();shoot();});
updateHUD();
</script>
</body>
</html>
