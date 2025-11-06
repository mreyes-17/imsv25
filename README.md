<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>No siempre que llueve es un desastre natural — Satin</title>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Baloo+2:wght@400;700;800&display=swap" rel="stylesheet">

<style>
:root{
  --rose:#ff69b4; --lav:#ab49cc; --sig:#7f00b2; --ink:#1f1023;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{font-family:"Baloo 2",system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,Cantarell,sans-serif;background:#0b0810;color:var(--ink);overflow:hidden}

/* Fondo borroso */
.bg{position:fixed;inset:0;z-index:0;pointer-events:none}
.bg::before{
  content:"";position:absolute;inset:-10%;
  background:url('photo_4900317553574480829_y.jpg') center/cover no-repeat;
  filter:blur(8px) brightness(1.05) saturate(1.04);transform:scale(1.04);
}

/* Lluvia detrás */
.rain{position:fixed;inset:0;z-index:1;pointer-events:none}
.drop{
  position:fixed;top:-40px;left:0;opacity:0;pointer-events:none;
  transform:translate3d(var(--x),-40px,0) rotate(var(--r));
  animation:fall var(--dur) linear forwards;
  font-size:var(--fs);
  filter:drop-shadow(0 6px 12px rgba(0,0,0,.14));
}
@keyframes fall{
  0%{opacity:0} 10%{opacity:1}
  100%{transform:translate3d(var(--x),calc(100vh + 60px),0) rotate(var(--r));opacity:0}
}

/* Escena base */
.stage{position:relative;z-index:2;display:grid;place-items:center;height:100vh}

/* =========== PASO 1: CORAZÓN =========== */
.heart-screen{position:absolute;inset:0;display:grid;place-items:center;background:linear-gradient(180deg,rgba(255,255,255,.05),rgba(255,255,255,.02))}
.banner{
  position:fixed;top:6vh;left:50%;transform:translateX(-50%);
  padding:10px 18px;border-radius:14px;font-weight:900;color:#fff;z-index:5;
  background:linear-gradient(90deg,#ff7ab8,#ff68b0 45%,#d362ff);
  box-shadow:0 10px 24px rgba(0,0,0,.2);
}
.heart-wrap{display:grid;place-items:center;gap:16px}
.heart{width:min(62vh,70vw);max-width:760px;aspect-ratio:1;filter:drop-shadow(0 10px 20px rgba(255,105,180,.35))}
svg{display:block}
.outline{fill:none;stroke:var(--rose);stroke-width:3}
.level{fill:#ffc1dd;opacity:.58}
.base{fill:#ffd1e8;opacity:.16}
.bloom{opacity:0;animation:bl .28s ease forwards}
@keyframes bl{to{opacity:1}}
.progress{font-weight:900;font-size:1.05rem;color:#ffe8f3;text-shadow:0 0 10px rgba(255,105,180,.6),0 2px 6px rgba(0,0,0,.35)}

.controls{position:fixed;left:50%;bottom:8vh;transform:translateX(-50%);display:flex;gap:12px;z-index:5}
.btn{
  border:none;border-radius:999px;padding:14px 22px;font-weight:900;cursor:pointer;
  background:linear-gradient(180deg,#ffd7ec,#ffc3e3);color:#7a2c56;box-shadow:0 6px 14px rgba(0,0,0,.18)
}
.btn:disabled{opacity:.6;cursor:not-allowed}

/* Mensaje 4s */
.msg4s{
  position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);
  z-index:6;font-weight:900;font-size:clamp(1.2rem,3.4vw,2.2rem);line-height:1.22;color:#fff;text-align:center;
  padding:18px 22px;border-radius:22px;
  background:
    radial-gradient(200% 140% at 20% 20%, rgba(255,255,255,.28) 0 20%, transparent 21%),
    linear-gradient(90deg,#ff78b5,#ff68b0 45%,#d362ff 100%);
  box-shadow:0 18px 50px rgba(0,0,0,.25),0 0 24px rgba(255,105,180,.55),inset 0 0 40px rgba(255,255,255,.18);
  -webkit-backdrop-filter: blur(6px); backdrop-filter: blur(6px);
  letter-spacing:.2px;opacity:0;scale:.96;transition:opacity .3s ease,scale .3s ease
}
.msg4s.show{opacity:1;scale:1}

/* Latido al llenarse */
.pulse{animation:pulse 1.6s ease-in-out infinite}
@keyframes pulse{0%{transform:scale(1)}30%{transform:scale(1.045)}60%{transform:scale(1)}100%{transform:scale(1)}}

/* =========== PASO 2: SOBRE REALISTA (se abre solo) =========== */
.env-wrap{position:fixed;inset:0;display:none;place-items:center;z-index:7}
.envelope{width:min(78vw,820px);height:min(56vh,520px);position:relative;opacity:0;perspective:1600px}
.envelope.show{animation:popIn .8s cubic-bezier(.22,1,.36,1) .1s forwards}
@keyframes popIn{0%{opacity:0;transform:scale(.86)}60%{opacity:1;transform:scale(1.05)}100%{opacity:1;transform:scale(1)}}

/* Cuerpo satinado */
.env-body{
  position:absolute;inset:0;border-radius:28px;
  background:
    radial-gradient(180% 120% at 20% 10%, rgba(255,255,255,.75) 0 12%, transparent 13%),
    radial-gradient(140% 100% at 80% 90%, rgba(255,255,255,.55) 0 10%, transparent 11%),
    linear-gradient(180deg,#ffe7f2 0%,#ffd2e5 55%,#ffe8f3 100%);
  box-shadow:
    inset 0 1px 0 rgba(255,255,255,.9),
    inset 0 -10px 18px rgba(0,0,0,.05),
    0 24px 64px rgba(0,0,0,.25);
}

/* Flaps con brillo perlado */
.flapT,.flapL,.flapR{
  position:absolute;transform-style:preserve-3d;backface-visibility:hidden;
  transition:transform .8s cubic-bezier(.4,0,.2,1);
}
.flapT{
  top:0;left:0;right:0;height:60%;border-radius:28px 28px 0 0;
  background:
    radial-gradient(150% 90% at 50% 0%, rgba(255,255,255,.9) 0 22%, transparent 23%),
    linear-gradient(180deg,#fff6fb 0%,#ffe2ee 65%,#ffd3e6 100%);
  clip-path:polygon(0 0,100% 0,50% 100%);
  transform-origin:top;
}
.flapL{
  top:0;left:0;bottom:0;width:52%;
  background:
    radial-gradient(140% 100% at 0% 50%, rgba(255,255,255,.85) 0 16%, transparent 17%),
    linear-gradient(180deg,#ffd8e9 0%,#ffcfe3 100%);
  clip-path:polygon(0 0,100% 50%,0 100%);border-radius:28px 0 0 28px;transform-origin:left;transition-delay:.2s
}
.flapR{
  top:0;right:0;bottom:0;width:52%;
  background:
    radial-gradient(140% 100% at 100% 50%, rgba(255,255,255,.85) 0 16%, transparent 17%),
    linear-gradient(180deg,#ffd8e9 0%,#ffcfe3 100%);
  clip-path:polygon(100% 0,0 50%,100% 100%);border-radius:0 28px 28px 0;transform-origin:right;transition-delay:.2s
}

/* Sello rombo (solo decoración) */
.diamond{
  position:absolute;left:50%;top:50%;transform:translate(-50%,-50%) rotate(45deg);
  width:180px;height:180px;border-radius:18px;z-index:30;pointer-events:none;
  background:
    radial-gradient(120% 120% at 30% 30%, rgba(255,255,255,.85) 0 40%, transparent 41%),
    linear-gradient(180deg,#ffffff,#ffeaf5);
  box-shadow:0 18px 40px rgba(0,0,0,.18),0 0 0 12px rgba(255,255,255,.7) inset,0 8px 30px rgba(255,105,180,.18);
}
.diamond .label{transform:rotate(-45deg);width:100%;height:100%;display:grid;place-items:center;font-weight:900;color:#7a2c56;font-size:18px}

/* Destello + confeti */
.flash{position:fixed;inset:0;pointer-events:none;z-index:40;opacity:0}
.flash.show{animation:flashIn 1.3s ease forwards}
@keyframes flashIn{0%{opacity:0}15%{opacity:1;background:radial-gradient(60% 60% at 50% 50%, rgba(255,105,180,.52), rgba(255,105,180,.22) 45%, rgba(255,105,180,0) 70%)}100%{opacity:0}}
.spark{position:fixed;pointer-events:none;z-index:41;will-change:transform,opacity}

/* =========== PASO 3: CARTA =========== */
.letter{
  position:fixed;left:50%;top:50%;transform:translate(-50%,-50%) scale(.9);
  opacity:0;width:min(92vw,1020px);height:min(88vh,930px);z-index:50;
  border-radius:22px;padding:34px;
  background:url('269e17ff0854a88e3c019cbf3ab1c8b1.jpg') center/cover no-repeat;
  box-shadow:0 28px 80px rgba(0,0,0,.32)
}
.letter.show{animation:letterIn 1.05s cubic-bezier(.22,1,.36,1) forwards}
@keyframes letterIn{
  0%{opacity:0;transform:translate(-50%,-50%) scale(.9) rotate(-1deg)}
  60%{opacity:1;transform:translate(-50%,-50%) scale(1.03) rotate(.3deg)}
  100%{opacity:1;transform:translate(-50%,-50%) scale(1) rotate(0)}
}
.title{font-family:"Great Vibes",cursive;color:#ff69b4;text-align:center;font-size:clamp(2.4rem,6.2vw,4.1rem);margin-bottom:10px}
.p{font-weight:800;color:#ab49cc;line-height:2.0;font-size:clamp(1.08rem,1.75vw,1.22rem);margin:0 0 14px}
.sign{display:flex;align-items:center;justify-content:flex-end;gap:14px;margin-top:4px}
.sign span{font-family:"Great Vibes",cursive;color:#7f00b2;font-size:clamp(1.24rem,1.9vw,1.55rem)}
.sign img{width:84px;height:84px;object-fit:cover;border-radius:12px;box-shadow:0 8px 18px rgba(0,0,0,.18)}

/* Aviso audio */
.notice{position:fixed;left:50%;bottom:72px;transform:translateX(-50%);z-index:60;display:none;gap:12px;align-items:center;background:rgba(255,255,255,.95);border:1px solid rgba(0,0,0,.08);padding:10px 16px;border-radius:14px;font-weight:800;color:#7a2c56;box-shadow:0 8px 18px rgba(0,0,0,.16)}
.notice .btn{cursor:pointer;border:none;padding:8px 12px;border-radius:999px;font-weight:800;background:linear-gradient(180deg,#ffd7ec,#ffc3e3);color:#7a2c56}
</style>
</head>
<body>
<div class="bg"></div>
<div class="rain" id="rain"></div>

<div class="stage" id="stage">
  <!-- CORAZÓN -->
  <section class="heart-screen" id="heartScreen">
    <div class="banner" id="banner">🌸 El primero floreció.</div>
    <div class="heart-wrap">
      <div class="heart" id="heart">
        <svg viewBox="0 0 100 100" aria-hidden="true">
          <defs>
            <clipPath id="clipH">
              <path d="M50 86C50 86 9 61 9 36C9 23 20 12 33 12C41 12 47 16 50 22C53 16 59 12 67 12C80 12 91 23 91 36C91 61 50 86 50 86Z"/>
            </clipPath>
          </defs>
          <g clip-path="url(#clipH)">
            <rect id="level" class="level" x="10" y="86" width="80" height="0"></rect>
            <rect id="base" class="base" x="10" y="12" width="80" height="74"></rect>
            <g id="rise"></g>
            <g id="fill"></g>
          </g>
          <path class="outline" d="M50 86C50 86 9 61 9 36C9 23 20 12 33 12C41 12 47 16 50 22C53 16 59 12 67 12C80 12 91 23 91 36C91 61 50 86 50 86Z"/>
        </svg>
      </div>
      <div class="progress" id="progress">Tulipanes: 0/18</div>
    </div>
    <div class="controls">
      <button class="btn" id="btnTulip" type="button">🌷 Haz florecer otro tulipán</button>
    </div>
    <div class="msg4s" id="msg4s">✨ Desde que llegaste, ya no hay espacio para nadie más. ✨</div>
  </section>

  <!-- SOBRE SATINADO (auto-apertura) -->
  <div class="env-wrap" id="envWrap" aria-hidden="true">
    <div class="envelope" id="env">
      <div class="env-body"></div>
      <div class="flapT" id="flapT"></div>
      <div class="flapL" id="flapL"></div>
      <div class="flapR" id="flapR"></div>
      <div class="diamond"><div class="label">Con cariño 💌</div></div>
    </div>
  </div>

  <!-- CARTA -->
  <article class="letter" id="letter" aria-live="polite">
    <h2 class="title">No siempre que llueve es un desastre natural</h2>
    <p class="p"><b>He pensado mucho en nosotras, en las veces que peleamos y en todo lo que pasa después. Sé que muchas veces eres tú quien da el primer paso, quien busca arreglar las cosas, quien intenta acercarse aunque siga dolida. Y sí, sé que te duele que yo no haga lo mismo. A veces necesito quedarme callada, pensar, ordenar todo lo que siento antes de hablar. No porque quiera alejarme, sino porque temo decir algo sin pensar y lastimarte más.</b></p>
    <p class="p"><b>No es falta de ganas ni de amor; es mi manera de cuidar lo que más me importa: tú. No sabes cuántas veces me he quedado con las ganas de escribirte o correr detrás de ti, pero me freno para no hablar desde el enojo o la confusión. Sé que desde afuera puede parecer que no me importa, pero es todo lo contrario: incluso cuando parece que todo se rompe, sigo pensando en ti, queriendo que estés bien, eligiéndote.</b></p>
    <p class="p"><b>Si la lluvia trae flores, que a nosotras nos traiga tulipanes rosados, helados compartidos y canciones de Morat que suenen a promesa. Porque incluso cuando todo tiembla, mi corazón vuelve a su lugar: contigo.</b></p>
    <div class="sign"><span>te quiere con todo su corazón M</span><img src="photo_4900317553574480831_y.jpg" alt="Nosotras"></div>
  </article>
</div>

<!-- AUDIO -->
<audio id="song" src="morat.mp3" preload="auto" loop></audio>
<div class="notice" id="notice">Tu navegador bloqueó el audio. <button class="btn" id="playBtn" type="button">▶️ Reproducir</button></div>

<!-- Destello -->
<div class="flash" id="flash"></div>

<script>
/* Lluvia */
const rain = document.getElementById('rain'); const em = ['🌷','🍦'];
function spawn(){
  const d=document.createElement('div'); d.className='drop';
  d.textContent=em[Math.random()<.5?0:1];
  d.style.setProperty('--x',Math.random()*innerWidth+'px');
  d.style.setProperty('--r',(Math.random()-0.5)*60+'deg');
  d.style.setProperty('--dur',(Math.random()*5+6).toFixed(2)+'s');
  d.style.setProperty('--fs',(Math.random()*10+20|0)+'px');
  rain.appendChild(d); d.addEventListener('animationend',()=>d.remove());
}
setInterval(spawn,330); for(let i=0;i<14;i++) spawn();

/* Corazón */
const btnTulip=document.getElementById('btnTulip');
const progress=document.getElementById('progress');
const banner=document.getElementById('banner');
const msg4s=document.getElementById('msg4s');
const level=document.getElementById('level');
const base=document.getElementById('base');
const rise=document.getElementById('rise');
const fill=document.getElementById('fill');
const heartScreen=document.getElementById('heartScreen');
let clicks=0,total=18,busy=false,done=false;
const msgs=["🌸 El primero floreció.","🌷 Otro más, por ti.","💞 Late más fuerte.","✨ Ya se siente el amor.","🌷 Cada clic es cariño.","💗 Qué bonito va quedando.","🌸 Se llena de ti.","💞 Cada flor me recuerda a ti.","🌷 Ya casi, mi vida.","💖 Qué lindo está tu amor.","🌸 Sigue así, corazón.","💞 Casi lo logras.","🌷 Se ve hermoso.","💗 Falta poquito.","🌸 Ya casi, mi amor.","💞 Un poco más.","🌷 Último empujón.","💖 Lo llenaste todo."];

banner.textContent=msgs[0]; progress.textContent=`Tulipanes: 0/${total}`;
const placed=[];
function d2(a,b){const dx=a.x-b.x,dy=a.y-b.y;return dx*dx+dy*dy}
function addTulip(x,y,s){const t=document.createElementNS('http://www.w3.org/2000/svg','text');t.setAttribute('x',x.toFixed(2));t.setAttribute('y',y.toFixed(2));t.setAttribute('font-size',s);t.textContent='🌷';t.setAttribute('class','bloom');fill.appendChild(t)}
function pack(n,minD=26){let tries=0,add=0;while(add<n&&tries<500){tries++;const x=12+Math.random()*76,y=16+Math.random()*64,p={x,y};let ok=true;for(const q of placed){if(d2(p,q)<minD){ok=false;break}}if(ok){placed.push(p);addTulip(p.x,p.y,5.2+Math.random()*2.2);add++}}}
function riser(){const x=18+Math.random()*64,y=70+Math.random()*4,s=5.4+Math.random()*2.2;const t=document.createElementNS('http://www.w3.org/2000/svg','text');t.setAttribute('x',x.toFixed(2));t.setAttribute('y',y.toFixed(2));t.setAttribute('font-size',s);t.textContent='🌷';rise.appendChild(t);t.animate([{transform:'translateY(0)',opacity:.9},{transform:'translateY(-40px)',opacity:0}],{duration:900,easing:'ease-out'}).onfinish=()=>t.remove()}
function updateLevel(){const r=Math.min(clicks/total,1),H=74,h=H*r,y=86-h;level.setAttribute('y',y.toFixed(2));level.setAttribute('height',h.toFixed(2));base.setAttribute('opacity',(0.16+r*0.30).toFixed(2))}
let ac;function pop(){try{if(!ac) ac=new (window.AudioContext||window.webkitAudioContext)();const t=ac.currentTime,o=ac.createOscillator(),g=ac.createGain();o.type='sine';o.frequency.setValueAtTime(680,t);o.frequency.exponentialRampToValueAtTime(260,t+0.09);g.gain.setValueAtTime(0.003,t);g.gain.exponentialRampToValueAtTime(0.00002,t+0.12);o.connect(g).connect(ac.destination);o.start(t);o.stop(t+0.14)}catch(e){}}
function fillAll(){for(let gx=14;gx<=86;gx+=6){for(let gy=16;gy<=76;gy+=6){const dx=gx-50,dy=gy-56;if(dy<22||(dy*0.9+Math.abs(dx)*0.55)<38){placed.push({x:gx,y:gy});addTulip(gx,gy,5+Math.random()*2)}}}pack(120,18)}
function advance(){if(busy||done) return; busy=true; clicks++; pop(); pack(5,26); for(let i=0;i<3;i++) riser(); updateLevel(); progress.textContent=`Tulipanes: ${clicks}/${total}`; banner.textContent=msgs[Math.min(clicks-1,msgs.length-1)];
  if(clicks===total){fillAll(); document.querySelector('.heart').classList.add('pulse'); btnTulip.disabled=true; done=true; msg4s.classList.add('show'); setTimeout(()=>{msg4s.classList.remove('show'); heartScreen.style.display='none'; showEnvelope();},4000);}
  setTimeout(()=>busy=false,140);
}
btnTulip.addEventListener('click',advance);
btnTulip.addEventListener('keydown',e=>{if(e.key==='Enter'||e.key===' '){e.preventDefault();advance()}});

/* SOBRE -> Carta (auto-abrir) */
const envWrap=document.getElementById('envWrap');
const env=document.getElementById('env');
const flapT=document.getElementById('flapT');
const flapL=document.getElementById('flapL');
const flapR=document.getElementById('flapR');
const flash=document.getElementById('flash');
const letter=document.getElementById('letter');
const song=document.getElementById('song');
const notice=document.getElementById('notice');
const playBtn=document.getElementById('playBtn');

function showEnvelope(){
  envWrap.style.display='grid';
  requestAnimationFrame(()=>env.classList.add('show'));
  // Apertura épica automática
  setTimeout(openEpic, 900);
}
let opened=false;
function swoosh(){try{if(!ac) ac=new (window.AudioContext||window.webkitAudioContext)();const t=ac.currentTime,o=ac.createOscillator(),g=ac.createGain();o.type='sawtooth';o.frequency.setValueAtTime(180,t);o.frequency.exponentialRampToValueAtTime(40,t+0.35);g.gain.setValueAtTime(0.0008,t);g.gain.exponentialRampToValueAtTime(0.00001,t+0.38);o.connect(g).connect(ac.destination);o.start(t);o.stop(t+0.4)}catch(e){}}
function confetti(){const pieces=48, arr=['🌷','🍦'];for(let i=0;i<pieces;i++){const s=document.createElement('div');s.className='spark';s.textContent=arr[Math.random()<.5?0:1];const ang=(Math.PI*2)*(i/pieces),dist=120+Math.random()*150;const x=innerWidth/2+Math.cos(ang)*dist,y=innerHeight/2+Math.sin(ang)*dist;s.style.left=innerWidth/2+'px';s.style.top=innerHeight/2+'px';s.style.fontSize=(18+Math.random()*16|0)+'px';document.body.appendChild(s);s.animate([{transform:'translate(-50%,-50%) scale(1)',opacity:1},{transform:`translate(${x-innerWidth/2}px,${y-innerHeight/2}px) scale(.9)`,opacity:0}],{duration:980+Math.random()*520,easing:'cubic-bezier(.22,1,.36,1)'}).onfinish=()=>s.remove();}}
function openEpic(){
  if(opened) return; opened=true;
  flapT.style.transform='rotateX(-170deg)';
  flapL.style.transform='rotateY(-170deg)';
  flapR.style.transform='rotateY(170deg)';
  setTimeout(()=>{flash.classList.add('show');confetti()},320);
  setTimeout(()=>{envWrap.style.display='none';letter.classList.add('show');song.play().catch(()=>notice.style.display='flex')},680);
  swoosh();
}

/* Audio fallback */
playBtn.addEventListener('click',()=>{song.play().then(()=>notice.style.display='none').catch(()=>{})});
</script>
</body>
</html>
