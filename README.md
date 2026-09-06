<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>The Fifteenth Question</title>
<style>
:root{
  --bg:#050607;--panel:#090a0c;--line:#30343a;--text:#e7e8ea;--muted:#969aa1;
  --red:#b52b32;--forest:#07100c;
}
*{box-sizing:border-box}
html,body{margin:0;height:100%;background:#000;color:var(--text);font-family:Arial,Helvetica,sans-serif;overflow:hidden}
#app{height:100%;width:100%;background:#050607;position:relative}
.screen{position:absolute;inset:0;display:none}
.screen.active{display:flex}
button{font:inherit;color:#e7e8ea;background:rgba(5,6,8,.75);border:1px solid #50555c;padding:12px 26px;letter-spacing:1px;cursor:pointer}
button:hover{background:#e7e8ea;color:#08090b}
button:disabled{opacity:.45;cursor:not-allowed}
#menu{align-items:center;justify-content:center;text-align:center;overflow:hidden;background:
 radial-gradient(circle at 50% 80%,rgba(44,65,55,.22),transparent 38%),
 linear-gradient(rgba(0,0,0,.2),rgba(0,0,0,.86)),
 url('robert.png') center/cover no-repeat;
}
#menu:before{content:"";position:absolute;inset:0;background:linear-gradient(90deg,rgba(0,0,0,.9),rgba(0,0,0,.35),rgba(0,0,0,.92));backdrop-filter:blur(1px)}
.menuCard{position:relative;z-index:1;width:min(500px,90vw);padding:40px 20px}
.logo{font-size:clamp(46px,8vw,96px);line-height:.92;letter-spacing:5px;font-family:Impact,Arial Black,sans-serif;font-weight:400;text-shadow:0 0 18px #93a0aa}
.subtitle{letter-spacing:7px;color:#9da3aa;margin:22px 0 30px}
.menuBtns{display:grid;gap:12px;max-width:260px;margin:auto}
.hint{font-size:12px;color:#777;margin-top:28px;letter-spacing:1px}

#quiz{align-items:center;justify-content:center;background:#060708}
#quiz.glitch:after{content:"";position:absolute;inset:0;pointer-events:none;background:repeating-linear-gradient(0deg,transparent 0 7px,rgba(255,255,255,.025) 8px 9px);animation:noise .12s infinite}
@keyframes noise{50%{transform:translateX(4px)}}
.quizTop{position:absolute;top:24px;left:28px;right:28px;display:flex;justify-content:space-between;color:#aeb2b8;font-size:13px;letter-spacing:2px}
.quizCard{width:min(680px,90vw);text-align:center;position:relative;z-index:2}
.qnum{color:#80858b;letter-spacing:3px;margin-bottom:26px}
.question{font-size:clamp(24px,4vw,43px);line-height:1.35;min-height:110px;display:flex;align-items:center;justify-content:center}
.options{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:28px}
.options button{min-height:54px}
.robertPortrait{display:none;width:220px;height:220px;object-fit:cover;filter:grayscale(1) contrast(1.35);margin:0 auto 20px;border:1px solid #333}
#black{align-items:center;justify-content:center;background:#000}
#blackText{font-size:clamp(44px,10vw,130px);letter-spacing:10px;animation:flicker 1.2s infinite}
@keyframes flicker{0%,90%,100%{opacity:1}91%{opacity:.12}93%{opacity:.8}95%{opacity:.25}}

#game{background:#020403}
#view{width:100%;height:100%;display:block;cursor:crosshair}
#hud{position:absolute;inset:0;pointer-events:none}
#objective{position:absolute;top:18px;left:50%;transform:translateX(-50%);padding:10px 20px;border:1px solid #42474d;background:rgba(0,0,0,.55);letter-spacing:1.5px;text-align:center}
#timer{position:absolute;top:18px;right:24px;font-size:26px;letter-spacing:2px}
#stamina{position:absolute;bottom:24px;left:24px;width:190px;color:#cfd3d7;font-size:12px}
.bar{height:8px;border:1px solid #757b80;margin-top:7px;background:#111}
.fill{height:100%;width:100%;background:#d7d9db}
#info{position:absolute;bottom:24px;right:24px;text-align:right;font-size:12px;line-height:1.7;color:#c6c9cc}
#dialogue{position:absolute;bottom:74px;left:50%;transform:translateX(-50%);max-width:min(760px,85vw);padding:14px 20px;text-align:center;background:rgba(0,0,0,.72);border:1px solid #333;display:none}
#danger{position:absolute;top:68px;left:50%;transform:translateX(-50%);color:#d64048;letter-spacing:3px;font-size:13px;display:none}
#mapPanel{position:absolute;inset:8%;display:none;background:rgba(3,5,4,.95);border:1px solid #444;padding:20px;pointer-events:auto}
.mapTitle{letter-spacing:4px;color:#aaa;margin-bottom:12px}
#mapCanvas{width:100%;height:calc(100% - 35px);display:block;background:#07100b;border:1px solid #252c27}
#choice,#ending{align-items:center;justify-content:center;text-align:center;background:radial-gradient(circle at 50% 20%,#18201b,#050605 55%)}
.panel{width:min(760px,90vw);padding:36px;border:1px solid #343a3e;background:rgba(0,0,0,.58)}
.panel h1{letter-spacing:3px;margin-top:0;font-size:clamp(32px,6vw,70px)}
.panel p{color:#b9bdc1;line-height:1.75}
.choices{display:flex;justify-content:center;gap:16px;flex-wrap:wrap;margin-top:26px}
.red{color:#d34c53}
@media(max-width:600px){.options{grid-template-columns:1fr}.quizTop{top:12px;left:14px;right:14px}.menuCard{padding:20px}.question{min-height:130px}}
</style>
</head>
<body>
<div id="app">
<section id="menu" class="screen active">
  <div class="menuCard">
    <div class="logo">THE FIFTEENTH<br>QUESTION</div>
    <div class="subtitle">ANIMAL GUESSING GAME</div>
    <div class="menuBtns"><button id="startBtn">START GAME</button><button id="controlsBtn">CONTROLS</button></div>
    <div class="hint">VISUAL STYLE INSPIRED BY YOUR REFERENCE COLLAGE<br>ROBERT IMAGE: robert.png</div>
  </div>
</section>

<section id="quiz" class="screen">
 <div class="quizTop"><span>ANIMAL QUIZ</span><span id="progress">1 / 15</span></div>
 <div class="quizCard">
  <img id="robPortrait" class="robertPortrait" src="robert.png" alt="">
  <div class="qnum" id="qnum"></div>
  <div class="question" id="question"></div>
  <div class="options" id="options"></div>
 </div>
</section>

<section id="black" class="screen"><div id="blackText">SAVE ME</div></section>

<section id="game" class="screen">
 <canvas id="view" width="1280" height="720"></canvas>
 <div id="hud">
   <div id="objective">OBJECTIVE: FIND ROBERT</div>
   <div id="timer">05:00</div>
   <div id="danger">HE'S COMING...</div>
   <div id="dialogue"></div>
   <div id="stamina">STAMINA<div class="bar"><div id="staminaFill" class="fill"></div></div></div>
   <div id="info">CLUES: <span id="cluesHud">0</span>/4<br>AXE: <span id="axeHud">NO</span><br>MAP: M</div>
   <div id="mapPanel"><div class="mapTitle">MAP</div><canvas id="mapCanvas" width="900" height="600"></canvas></div>
 </div>
</section>

<section id="choice" class="screen">
 <div class="panel"><h1>YOU MADE IT HOME.</h1><p>What will you do with what you discovered?</p>
 <div class="choices"><button id="policeBtn">TELL THE POLICE</button><button id="secretBtn">KEEP IT SECRET</button></div></div>
</section>

<section id="ending" class="screen">
 <div class="panel"><h1 id="endTitle"></h1><p id="endText"></p><div class="choices"><button id="restartBtn">PLAY AGAIN</button></div></div>
</section>
</div>

<script>
const $=s=>document.querySelector(s);
const screens=['menu','quiz','black','game','choice','ending'];
function show(id){screens.forEach(x=>$('#'+x).classList.toggle('active',x===id));}

const quiz=[
["I have a long neck and eat leaves from tall trees. What am I?",["Lion","Giraffe","Elephant","Tiger"],"Giraffe"],
["I am known as man's best friend. What am I?",["Dog","Cat","Horse","Bear"],"Dog"],
["I have black and white stripes.",["Zebra","Tiger","Panda","Wolf"],"Zebra"],
["I am the largest animal on Earth.",["Elephant","Blue Whale","Giraffe","Shark"],"Blue Whale"],
["I sleep during the day and hunt at night.",["Owl","Cow","Rabbit","Horse"],"Owl"],
["I live deep inside forests and sometimes watch without being seen.",["Deer","Wolf","Bear","Fox"],"Deer"],
["What animal follows another creature without making a sound?",["Fox","Predator","Rabbit","Horse"],"Predator"],
["What animal knows where you live?",["Dog","Bird","Cat","Something else"],"Something else"],
["What animal waits until you are alone?",["Wolf","Tiger","Fox","I don't know"],"I don't know"],
["What animal has been standing between the trees?",["Deer","Bear","Wolf","I didn't see anything"],"I didn't see anything"],
["Why do you keep hearing footsteps when nobody is there?",["Wind","Animals","Someone","I don't know"],"Someone"],
["Which animal knows your name?",["Dog","Parrot","Nobody","Something else"],"Something else"],
["What was that sound outside your window?",["A cat","Wind","A branch","Someone"],"Someone"],
["Why are you still playing?",["Because it's a game","I don't know","Someone told me to","Robert"],"Robert"],
["DO YOU RECOGNIZE THIS?",["ROBERT"],"ROBERT"]
];
let qi=0;
function loadQ(){
 const [q,ops]=quiz[qi];
 $('#progress').textContent=`${qi+1} / 15`;
 $('#qnum').textContent=`QUESTION ${qi+1}`;
 $('#question').textContent=q;
 $('#robPortrait').style.display=qi===14?'block':'none';
 $('#quiz').classList.toggle('glitch',qi>=7);
 $('#options').innerHTML='';
 ops.forEach(o=>{let b=document.createElement('button');b.textContent=o;b.onclick=()=>answer(o);$('#options').appendChild(b)});
}
function answer(a){
 if(a!==quiz[qi][2]){ $('#question').textContent='WRONG.'; setTimeout(loadQ,500); return; }
 qi++;
 if(qi<quiz.length)loadQ(); else reveal();
}
function reveal(){
 show('black'); $('#blackText').textContent='ROB... IS THAT YOU?';
 setTimeout(()=>$('#blackText').textContent='SAVE ME',1800);
 setTimeout(startGame,3600);
}

const canvas=$('#view'),ctx=canvas.getContext('2d'), mapC=$('#mapCanvas'),mctx=mapC.getContext('2d');
let keys={},running=false,last=0,mapOpen=false,dialogTimer;
const world={w:2600,h:2600};
let player,killer,objects,clues,hasAxe,clueCount,timeLeft,stamina;
const trees=[];
for(let i=0;i<260;i++)trees.push({x:80+Math.random()*2440,y:80+Math.random()*2440,r:18+Math.random()*35});
const home={x:230,y:1300};
function dist(a,b,c,d){return Math.hypot(a-c,b-d)}
function say(t,ms=2200){$('#dialogue').textContent=t;$('#dialogue').style.display='block';clearTimeout(dialogTimer);dialogTimer=setTimeout(()=>$('#dialogue').style.display='none',ms)}
function startGame(){
 show('game'); running=true; last=performance.now(); mapOpen=false;
 player={x:home.x+130,y:home.y,a:0,speed:210};
 killer={x:2100,y:600,a:0,active:false};
 clueCount=0;hasAxe=false;timeLeft=300;stamina=100;
 clues=[
  {x:520,y:430,name:"Robert's Backpack",found:false},
  {x:2060,y:520,name:"Robert's Watch",found:false},
  {x:2180,y:2050,name:"Robert's Bracelet",found:false},
  {x:620,y:2130,name:"Robert's Photograph",found:false}
 ];
 objects=[{x:1300,y:1450,name:"Barry's axe",type:"axe",found:false}];
 say("You wake up at home... The forest is behind your house.",3000);
 requestAnimationFrame(loop);
}
window.addEventListener('keydown',e=>{
 keys[e.key.toLowerCase()]=true;
 if(e.key.toLowerCase()==='m'&&$('#game').classList.contains('active')){mapOpen=!mapOpen;$('#mapPanel').style.display=mapOpen?'block':'none';if(mapOpen)drawMap();}
 if(e.key.toLowerCase()==='r'&&running&&hasAxe&&killer.active&&dist(player.x,player.y,killer.x,killer.y)<150){hero();}
});
window.addEventListener('keyup',e=>keys[e.key.toLowerCase()]=false);
canvas.addEventListener('mousemove',e=>{if(!running||mapOpen)return; player.a+=e.movementX*.0025});
canvas.addEventListener('click',()=>{if(canvas.requestPointerLock)canvas.requestPointerLock()});

function loop(t){
 if(!running)return;
 let dt=Math.min(.05,(t-last)/1000);last=t;
 update(dt);draw();
 requestAnimationFrame(loop);
}
function update(dt){
 timeLeft-=dt;if(timeLeft<=0){victim("Nightfall came before you escaped.");return}
 let turn=1.8*dt;
 if(keys['arrowleft']||keys['q'])player.a-=turn;
 if(keys['arrowright']||keys['e'])player.a+=turn;
 let move=0,side=0;
 if(keys['w']||keys['arrowup'])move=1;
 if(keys['s']||keys['arrowdown'])move=-1;
 if(keys['a'])side=-1;if(keys['d'])side=1;
 let sprint=(keys['shift']&&stamina>0&&(move||side));
 let sp=player.speed*(sprint?1.7:1);
 if(sprint)stamina=Math.max(0,stamina-28*dt);else stamina=Math.min(100,stamina+16*dt);
 let nx=player.x+(Math.cos(player.a)*move+Math.cos(player.a+Math.PI/2)*side)*sp*dt;
 let ny=player.y+(Math.sin(player.a)*move+Math.sin(player.a+Math.PI/2)*side)*sp*dt;
 nx=Math.max(25,Math.min(world.w-25,nx));ny=Math.max(25,Math.min(world.h-25,ny));
 player.x=nx;player.y=ny;

 clues.forEach(c=>{if(!c.found&&dist(player.x,player.y,c.x,c.y)<65){c.found=true;clueCount++;killer.active=true;say("You found "+c.name+".");if(clueCount===4){$('#objective').textContent='OBJECTIVE: RETURN HOME';say("You found everything. GET HOME!",3200)}}});
 objects.forEach(o=>{if(!o.found&&dist(player.x,player.y,o.x,o.y)<65){o.found=true;hasAxe=true;say("You found Barry's missing axe.");}});
 if(killer.active){
  let d=dist(player.x,player.y,killer.x,killer.y);
  if(d<820){let a=Math.atan2(player.y-killer.y,player.x-killer.x);let ks=185+(clueCount*18);killer.x+=Math.cos(a)*ks*dt;killer.y+=Math.sin(a)*ks*dt}
  if(d<95){if(hasAxe){say("Press R to fight back!",900)}else victim("The figure reached you in the forest.");}
 }
 if(clueCount===4&&dist(player.x,player.y,home.x,home.y)<130){running=false;document.exitPointerLock?.();show('choice')}
 hud();
}
function hud(){
 let m=Math.floor(timeLeft/60),s=Math.floor(timeLeft%60);
 $('#timer').textContent=String(m).padStart(2,'0')+":"+String(s).padStart(2,'0');
 $('#cluesHud').textContent=clueCount;$('#axeHud').textContent=hasAxe?'YES':'NO';
 $('#staminaFill').style.width=stamina+'%';
 $('#danger').style.display=killer.active&&dist(player.x,player.y,killer.x,killer.y)<650?'block':'none';
}

function project(x,y){
 let dx=x-player.x,dy=y-player.y,ca=Math.cos(player.a),sa=Math.sin(player.a);
 let forward=dx*ca+dy*sa,side=-dx*sa+dy*ca;
 return {z:forward,x:canvas.width/2+side*650/forward};
}
function draw(){
 const W=canvas.width,H=canvas.height;
 ctx.clearRect(0,0,W,H);
 let sky=ctx.createLinearGradient(0,0,0,H);sky.addColorStop(0,'#020405');sky.addColorStop(.55,'#0b1412');sky.addColorStop(.56,'#11140f');sky.addColorStop(1,'#030504');ctx.fillStyle=sky;ctx.fillRect(0,0,W,H);
 // distant fog
 for(let i=0;i<7;i++){ctx.fillStyle=`rgba(120,145,130,${.015+i*.005})`;ctx.fillRect(0,H*.38+i*24,W,22)}
 // objects sorted by distance
 let drawables=[];
 trees.forEach(t=>drawables.push({kind:'tree',...t}));
 clues.filter(c=>!c.found).forEach(c=>drawables.push({kind:'clue',...c}));
 objects.filter(o=>!o.found).forEach(o=>drawables.push({kind:'axe',...o}));
 if(killer.active)drawables.push({kind:'killer',x:killer.x,y:killer.y});
 drawables=drawables.map(o=>({...o,p:project(o.x,o.y)})).filter(o=>o.p.z>20&&o.p.z<1100).sort((a,b)=>b.p.z-a.p.z);
 for(const o of drawables){
  let z=o.p.z,scale=Math.min(900,100000/z),x=o.p.x,y=H*.56+Math.min(300,9000/z);
  if(o.kind==='tree'){
   let h=scale*1.9,w=scale*.75;
   ctx.fillStyle='#090f0b';ctx.fillRect(x-w*.12,y-h*.08,w*.24,h*.65);
   ctx.fillStyle='#0a170f';ctx.beginPath();ctx.moveTo(x,y-h);ctx.lineTo(x-w,y+h*.08);ctx.lineTo(x+w,y+h*.08);ctx.closePath();ctx.fill();
   ctx.fillStyle='rgba(0,0,0,.25)';ctx.fillRect(x-w*.9,y+h*.08,w*1.8,3);
  } else if(o.kind==='clue'){
   ctx.fillStyle='#d0bd7a';ctx.fillRect(x-scale*.08,y-scale*.08,scale*.16,scale*.16);
  } else if(o.kind==='axe'){
   ctx.strokeStyle='#8d6a4b';ctx.lineWidth=Math.max(2,scale*.04);ctx.beginPath();ctx.moveTo(x,y);ctx.lineTo(x+scale*.12,y-scale*.45);ctx.stroke();ctx.strokeStyle='#aaa';ctx.lineWidth=Math.max(2,scale*.07);ctx.beginPath();ctx.moveTo(x,y-scale*.42);ctx.lineTo(x+scale*.18,y-scale*.48);ctx.stroke();
  } else {
   let h=scale*1.25,w=scale*.42;
   ctx.fillStyle='#090a0a';ctx.fillRect(x-w/2,y-h,w,h);
   ctx.fillStyle='#6f7476';ctx.fillRect(x-w*.24,y-h*.92,w*.48,h*.22);
   ctx.fillStyle='#d8d8d8';ctx.fillRect(x-w*.12,y-h*.84,w*.08,h*.04);ctx.fillRect(x+w*.04,y-h*.84,w*.08,h*.04);
  }
 }
 // home direction marker
 let hp=project(home.x,home.y);if(hp.z>20){ctx.fillStyle='rgba(230,220,170,.6)';ctx.font='14px Arial';ctx.fillText('HOME',Math.max(20,Math.min(W-60,hp.x)),60)}
 // vignette
 let vg=ctx.createRadialGradient(W/2,H/2,H*.15,W/2,H/2,H*.85);vg.addColorStop(.5,'rgba(0,0,0,0)');vg.addColorStop(1,'rgba(0,0,0,.82)');ctx.fillStyle=vg;ctx.fillRect(0,0,W,H);
}
function drawMap(){
 const W=mapC.width,H=mapC.height;mctx.fillStyle='#07100b';mctx.fillRect(0,0,W,H);
 mctx.strokeStyle='#1e3529';for(let x=0;x<W;x+=90){mctx.beginPath();mctx.moveTo(x,0);mctx.lineTo(x,H);mctx.stroke()}for(let y=0;y<H;y+=90){mctx.beginPath();mctx.moveTo(0,y);mctx.lineTo(W,y);mctx.stroke()}
 function dot(x,y,col,label){let X=x/world.w*W,Y=y/world.h*H;mctx.fillStyle=col;mctx.beginPath();mctx.arc(X,Y,7,0,Math.PI*2);mctx.fill();mctx.fillStyle='#ddd';mctx.font='15px Arial';mctx.fillText(label,X+10,Y-8)}
 dot(home.x,home.y,'#ddd','Home');dot(player.x,player.y,'#b52b32','You');
 [[500,420,'Lumberyard'],[2100,500,'Cave'],[2180,2050,'River'],[620,2130,'Campsite'],[1300,1450,'Cabin']].forEach(a=>dot(a[0],a[1],'#8c9b91',a[2]));
 clues.forEach(c=>!c.found&&dot(c.x,c.y,'#b9a75e','?'));
}
function end(title,text){running=false;document.exitPointerLock?.();show('ending');$('#endTitle').textContent=title;$('#endText').textContent=text}
function victim(extra){end("ENDING: VICTIM",extra+" The forest kept its secrets, and the truth behind the masked figure was never fully discovered.")}
function hero(){end("ENDING: HERO","You fought back and survived. The mask came off, revealing Robert's father behind the trap. You escaped the forest, told the authorities everything, and Robert was finally laid to rest. Then, on the way home, your screen displayed one final message: YOU ANSWERED ALL THE QUESTIONS CORRECTLY. QUESTION 16.")}
$('#startBtn').onclick=()=>{qi=0;show('quiz');loadQ()};
$('#controlsBtn').onclick=()=>alert("QUIZ: click answers. FOREST: WASD move, mouse or Q/E look, Shift sprint, M map. Find 4 clues, return home. If you have the axe and the killer is close, press R.");
$('#policeBtn').onclick=()=>end("ENDING: FAILED HERO","You told the police everything you knew. The story sounded impossible, and suspicion slowly turned toward you. You were left trapped in a nightmare that nobody else could understand.");
$('#secretBtn').onclick=()=>end("ENDING: THE SILENCE","You kept the truth to yourself. Robert's disappearance was written off as a wilderness tragedy, but you knew something else had been waiting in the forest. The secret followed you home.");
$('#restartBtn').onclick=()=>{show('menu');qi=0};
</script>
</body>
</html>


]]]
