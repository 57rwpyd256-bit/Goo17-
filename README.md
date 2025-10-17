<!doctype html>
<html lang="mn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Goomarald — Special</title>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&family=Parisienne&display=swap" rel="stylesheet">
<style>
  :root{
    --bg1:#f7d9e6;
    --bg2:#e8d9ff;
    --accent:#ff6fa6;
    --muted: rgba(255,255,255,0.85);
    --panel: rgba(255,255,255,0.06);
  }
  *{box-sizing:border-box;margin:0;padding:0}
  html,body{height:100%}
  body{
    font-family: "Poppins", system-ui, -apple-system, "Segoe UI", Roboto, Arial;
    background: linear-gradient(135deg,var(--bg2),#fbe8f0 60%);
    color:#111;
    -webkit-font-smoothing:antialiased;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:28px;
  }

  /* Main card */
  .card{
    width: min(980px,96vw);
    height: 82vh;
    min-height:560px;
    border-radius:20px;
    padding:20px;
    display:flex;
    gap:18px;
    position:relative;
    overflow:hidden;
    box-shadow: 0 18px 60px rgba(80,40,70,0.12);
    background: linear-gradient(180deg, rgba(255,255,255,0.65), rgba(255,255,255,0.42));
    backdrop-filter: blur(6px) saturate(1.05);
  }

  /* Left column (menu) */
  .left {
    width: 34%;
    min-width:260px;
    display:flex;
    flex-direction:column;
    gap:14px;
    align-items:flex-start;
    padding:18px;
  }
  .brand {
    display:flex;
    align-items:center;
    gap:12px;
  }
  .logo {
    width:56px;height:56px;border-radius:12px;background:linear-gradient(180deg,var(--accent),#ffb3d0);
    display:flex;align-items:center;justify-content:center;color:#fff;font-weight:800;font-size:20px;font-family: "Parisienne", cursive;
    box-shadow: 0 8px 24px rgba(255,100,150,0.12);
  }
  h1{font-size:1.35rem;color:#3b2b3b}
  .subtitle{font-size:0.9rem;color:rgba(40,30,40,0.6)}

  .menu {
    margin-top:8px;
    display:flex;
    flex-direction:column;
    gap:12px;
    width:100%;
  }
  .pill {
    display:inline-flex;
    align-items:center;
    gap:8px;
    padding:10px 14px;
    border-radius:999px;
    background:linear-gradient(180deg, rgba(255,255,255,0.85), rgba(255,255,255,0.85));
    box-shadow: 0 6px 22px rgba(0,0,0,0.06);
    cursor:pointer;
    font-weight:700;
    color:#6b3b4f;
    user-select:none;
  }
  .small-note{font-size:12px;color:rgba(0,0,0,0.45);margin-top:10px}

  /* Right panel */
  .right { flex:1; position:relative; display:flex; align-items:center; justify-content:center; padding:10px; }
  .panel {
    width:100%; height:100%; border-radius:14px; position:relative; overflow:hidden;
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    box-shadow: inset 0 0 40px rgba(255,255,255,0.02);
    padding:18px;
    display:flex;
    align-items:center;
    justify-content:center;
  }

  /* background photo (cover) */
  .bg-photo {
    position:absolute; inset:0; background-position:center; background-size:cover; filter: blur(6px) saturate(.95) brightness(.72);
    z-index:0;
    transform: scale(1.03);
  }
  .bg-overlay{ position:absolute; inset:0; background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,200,230,0.02)); z-index:1; }

  /* content area */
  .content {
    position:relative; z-index:3; width:100%; max-width:760px; text-align:center; display:flex; flex-direction:column; align-items:center; gap:12px;
  }

  .main-card {
    width:100%; border-radius:12px; padding:18px; background: rgba(255,255,255,0.95); box-shadow: 0 12px 40px rgba(60,20,40,0.06);
  }

  /* image preview and thumbs */
  .img-wrap{ position:relative; display:flex; flex-direction:column; align-items:center; gap:10px }
  .main-img { width:auto; max-width:520px; max-height:52vh; border-radius:12px; box-shadow: 0 18px 48px rgba(80,40,70,0.08); cursor:pointer; display:block;
             opacity:0; transition: opacity .28s ease; }
  .main-img.visible { opacity:1; }
  .thumbs { display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin-top:6px }
  .thumbs img { width:64px;height:64px;object-fit:cover;border-radius:8px;border:2px solid transparent; cursor:pointer; opacity:0.6 }
  .thumbs img.active{ border-color: rgba(180,120,150,0.18); transform: scale(1.03); opacity:1 }

  /* message card */
  .msg-card { margin-top:12px; text-align:left; padding:12px 14px; border-radius:12px; background: linear-gradient(180deg, #fff, #fff); color:#3b2b3b; }
  .msg-text{ white-space:pre-wrap; max-height:26vh; overflow:auto; line-height:1.5; font-size:1rem; display:none }
  .msg-text.visible{ display:block }

  .controls-row { display:flex; gap:10px; justify-content:center; margin-top:12px }
  .btn-ghost { padding:8px 12px; border-radius:999px; border:1px solid rgba(0,0,0,0.06); background:transparent; cursor:pointer }
  .btn-primary { padding:10px 14px; border-radius:999px; background:linear-gradient(180deg,var(--accent),#ff9fc3); color:#fff; border:none; cursor:pointer; font-weight:700 }

  /* lock overlay (login) */
  .lock-overlay{
    position:fixed; inset:0; display:flex; align-items:center; justify-content:center; z-index:60; background: linear-gradient(180deg, rgba(10,8,12,0.65), rgba(30,14,28,0.6));
  }
  .lock-box{ width:min(520px,92vw); background:linear-gradient(180deg, #fff, #fff); border-radius:12px; padding:22px; box-shadow: 0 40px 100px rgba(10,10,12,0.6); text-align:center; }
  .lock-box h2{ font-size:1.25rem; margin-bottom:8px; color:#3b2b3b }
  .lock-input{ width:100%; padding:12px 14px; border-radius:10px; border:1px solid rgba(0,0,0,0.08); margin-top:10px; font-size:1rem }
  .lock-actions{ display:flex; gap:10px; margin-top:12px; justify-content:center }

  /* edit modal */
  .modal{ position:fixed; left:50%; top:50%; transform:translate(-50%,-50%); z-index:80; width:min(680px,94vw); background:#fff; border-radius:12px; padding:14px; box-shadow:0 30px 80px rgba(0,0,0,0.4); display:none; }
  .modal.show{ display:block }

  /* small helpers */
  .note-small{ font-size:12px; color:rgba(0,0,0,0.45) }
  .badge { display:inline-block; padding:6px 10px; border-radius:999px; background:linear-gradient(180deg,#fff,#ffe6f0); color:#4b2b3b; box-shadow: 0 6px 18px rgba(255,105,180,0.06); font-weight:700; }

  /* responsive */
  @media (max-width:820px){
    .card{flex-direction:column;height:92vh}
    .left{width:100%;flex-direction:row;justify-content:space-between;padding:12px}
    .main-img{max-width:88vw; max-height:40vh}
    .msg-text{max-height:22vh}
    .thumbs img{width:52px;height:52px}
  }
</style>
</head>
<body>

<div class="card" role="main" aria-live="polite">
  <div class="left">
    <div class="brand">
      <div class="logo">G</div>
      <div>
        <h1>Goomarald</h1>
        <div class="subtitle">0412 — Special Access</div>
      </div>
    </div>

    <div class="menu" aria-hidden="false">
      <div class="pill" id="openPic">1. Зураг (дарж үзэх)</div>
      <div class="pill" id="openMsg">2. Захиа (дарж үзэх)</div>
      <div class="pill" id="openGift">3. Бэлэг</div>
      <div class="small-note">Контентыг үзэхэд нэвтрэх код шаардлагатай — код оруулсан үед та бүх зүйлийг үзэж засварлах боломжтой.</div>
    </div>
  </div>

  <div class="right">
    <div class="panel" id="panel" aria-hidden="true">
      <!-- background photo like phone image (Image 1 style) -->
      <div class="bg-photo" id="bgPhoto" style="background-image:url('https://i.postimg.cc/qMHmSt38/IMG-0216.png');"></div>
      <div class="bg-overlay"></div>

      <div class="content" id="contentArea" aria-hidden="true">
        <div class="main-card" id="mainCard">
          <div class="img-wrap" id="imgWrap">
            <img id="mainImg" class="main-img" src="" alt="Preview image (click for hearts/fireworks)" />
            <div class="thumbs" id="thumbs">
              <img data-src="https://i.postimg.cc/qMHmSt38/IMG-0216.png" src="https://i.postimg.cc/qMHmSt38/IMG-0216.png" alt="1">
              <img data-src="https://i.postimg.cc/gJfMYjBF/IMG-0214.png" src="https://i.postimg.cc/gJfMYjBF/IMG-0214.png" alt="2">
              <img data-src="https://i.postimg.cc/pL2sVgkN/IMG-7896.jpg" src="https://i.postimg.cc/pL2sVgkN/IMG-7896.jpg" alt="3">
              <img data-src="https://i.postimg.cc/8ztt7tnM/IMG-8600.jpg" src="https://i.postimg.cc/8ztt7tnM/IMG-8600.jpg" alt="4">
            </div>
            <div class="note-small" style="margin-top:6px">Зураг харахын тулд "1. Зураг" дээр дарна — зураг автоматаар гарч ирэхгүй.</div>
          </div>

          <div class="msg-card" id="msgCard">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div style="display:flex;gap:8px;align-items:center">
                <div class="badge">Message</div>
                <div class="note-small">"2. Захиа" дээр дарж захиаг үзнэ үү.</div>
              </div>
              <div>
                <button class="btn-ghost" id="editMsgBtn">Edit</button>
              </div>
            </div>
            <div class="msg-text" id="msgText">Сайн уу Гоо маань өнөөдөр Гоогийн маань 17 насны төрсөн өдөр. Том анай болж байна даа яаана вэ надаас эгч болчихож байгаа юм байна шд хха.<br><br>2лаа танилцаад 7 сар өнгөрчээ бас л их худан явсан байгаа шүү тийнмээ?<br><br>4 сарын 12 нд чи бид 2-ын анх бие биенээ харсан, танилцасан өдөр санаж байна уу? Тэр өдөр тэр газар анх очоод л хаалгаар орох үед чи зогсож байсан. (  har jeans har ungiin tgd make it gsn bichигtei tsamts tgd polito )өмсчихсон байсан. Тэр үед хараад уаав ямар хөөрхөн охин бэ? Гээд дотроо бодож билээ тэгээд л шууд инста-г нь олоод дагах гэсэн ичээд л байж байсан чинь гэнэт намайг дагахаар нь барлаж билээ. Тэр үеийн 16 тай охин 17 хүрлээ дээ. Чи үргэлж гэрэлтээсэй үргэлж жаргалтай байгаарай. Гэж би үргэлж хүсэх болно оо. Гомдоож гуниглуулж байсан бол уучлаарай.<br><br>Заа төрсөн өдрийн баярын мэнд хүргье❤️ 🍀🍰🫂</div>
          </div>

          <div class="controls-row">
            <button class="btn-primary" id="openGiftBtn">🎀 Бэлэг авах</button>
            <button class="btn-ghost" id="muteBtn">🔊 Music: Off</button>
          </div>
        </div>
      </div>

      <!-- canvas for fireworks/confetti -->
      <canvas id="fxCanvas" style="position:absolute;inset:0;z-index:20;pointer-events:none"></canvas>
      <div id="heartsLayer" style="position:absolute;inset:0;z-index:18;pointer-events:none"></div>
      <div id="sparkles" style="position:absolute;inset:0;z-index:17;pointer-events:none"></div>

    </div>
  </div>

  <!-- controls -->
  <div style="position:fixed;left:18px;bottom:18px;z-index:80">
    <button id="startBtn" class="play-btn">✨ Start (Try autoplay)</button>
  </div>
</div>

<!-- lock overlay (requires code 0412) -->
<div class="lock-overlay" id="lockOverlay" role="dialog" aria-modal="true">
  <div class="lock-box" role="document">
    <h2>Контентыг үзэхэд нэвтрэх шаардагдана</h2>
    <div class="note-small">Нэвтрэх код: (таад 0412 оруулна) — код оруулснаар дуу автоматаар эхлэх боломжтой.</div>
    <input id="codeInput" class="lock-input" type="password" placeholder="Нэвтрэх код оруулна уу">
    <div class="lock-actions">
      <button id="unlockBtn" class="btn-primary">Unlock</button>
      <button id="closeLockBtn" class="btn-ghost">Cancel</button>
    </div>
    <div style="margin-top:10px" class="note-small">Remember me: <input type="checkbox" id="rememberChk"></div>
  </div>
</div>

<!-- edit modal -->
<div class="modal" id="editModal" role="dialog" aria-modal="true">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
    <strong>Захиа засах</strong>
    <button id="closeEdit" class="btn-ghost">Close</button>
  </div>
  <textarea id="editArea" style="width:100%;height:160px;border-radius:8px;border:1px solid rgba(0,0,0,0.08);padding:10px;font-size:1rem">Сайн уу Гоо маань өнөөдөр Гоогийн маань 17 насны төрсөн өдөр. Том анай болж байна даа яаана вэ надаас эгч болчихож байгаа юм байна шд хха.

2лаа танилцаад 7 сар өнгөрчээ бас л их худан явсан байгаа шүү тийнмээ?

4 сарын 12 нд чи бид 2-ын анх бие биенээ харсан, танилцасан өдөр санаж байна уу? Тэр өдөр тэр газар анх очоод л хаалгаар орох үед чи зогсож байсан. (  har jeans har ungiin tgd make it gsn bichигtei tsamts tgd polito )өмсчихсон байсан. Тэр үед хараад уаав ямар хөөрхөн охин бэ? Гээд дотроо бодож билээ тэгээд л шууд инста-г нь олоод дагах гэсэн ичээд л байж байсан чинь гэнэт намайг дагахаар нь барлаж билээ. Тэр үеийн 16 тай охин 17 хүрлээ дээ. Чи үргэлж гэрэлтээсэй үргэлж жаргалтай байгаарай. Гэж би үргэлж хүсэх болно оо. Гомдоож гуниглуулж байсан бол уучлаарай.

Заа төрсөн өдрийн баярын мэнд хүргье❤️ 🍀🍰🫂</textarea>
  <div style="display:flex;gap:10px;justify-content:flex-end;margin-top:8px">
    <button id="saveMsg" class="btn-primary">Save</button>
  </div>
</div>

<!-- audio (attempt) -->
<audio id="bgAudio" loop preload="auto">
  <source src="https://cdn.pixabay.com/download/audio/2023/02/22/audio_6cb7c83b26.mp3?filename=lofi-study-112191.mp3" type="audio/mpeg">
</audio>

<script>
/* Elements */
const lockOverlay = document.getElementById('lockOverlay');
const unlockBtn = document.getElementById('unlockBtn');
const codeInput = document.getElementById('codeInput');
const rememberChk = document.getElementById('rememberChk');
const closeLockBtn = document.getElementById('closeLockBtn');

const contentArea = document.getElementById('contentArea');
const panel = document.getElementById('panel');
const bgPhoto = document.getElementById('bgPhoto');
const fxCanvas = document.getElementById('fxCanvas');
const ctx = fxCanvas.getContext('2d');
const heartsLayer = document.getElementById('heartsLayer');
const sparkles = document.getElementById('sparkles');

const startBtn = document.getElementById('startBtn');
const muteBtn = document.getElementById('muteBtn');
const bgAudio = document.getElementById('bgAudio');

const editModal = document.getElementById('editModal');
const editArea = document.getElementById('editArea');
const editMsgBtn = document.getElementById('editMsgBtn');
const saveMsg = document.getElementById('saveMsg');
const closeEdit = document.getElementById('closeEdit');
const msgText = document.getElementById('msgText');

const openPic = document.getElementById('openPic');
const openMsg = document.getElementById('openMsg');
const openGift = document.getElementById('openGift');
const openGiftBtn = document.getElementById('openGiftBtn');

const thumbs = Array.from(document.querySelectorAll('#thumbs img'));
const mainImg = document.getElementById('mainImg');

let unlocked = false;
let imageShown = false;
let messageShown = false;
const PASS = '0412';

/* Canvas sizing */
function resizeCanvas(){
  const r = panel.getBoundingClientRect();
  fxCanvas.width = r.width * devicePixelRatio;
  fxCanvas.height = r.height * devicePixelRatio;
  fxCanvas.style.width = r.width + 'px';
  fxCanvas.style.height = r.height + 'px';
  ctx.setTransform(devicePixelRatio,0,0,devicePixelRatio,0,0);
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

/* Try autoplay once on load (may be blocked) */
async function tryAutoplay(){
  try{
    bgAudio.volume = 0.55;
    await bgAudio.play();
    muteBtn.textContent = '🔊 Music: On';
  }catch(e){
    // blocked; keep off until user gesture
    muteBtn.textContent = '🔊 Music: Off';
  }
}
tryAutoplay();

/* Unlock logic */
function setUnlocked(state){
  unlocked = state;
  if(state){
    lockOverlay.style.display = 'none';
    contentArea.style.display = 'flex';
    contentArea.setAttribute('aria-hidden','false');
    panel.setAttribute('aria-hidden','false');
    localStorage.setItem('unlocked', '1');
    // do not auto-show image/message — require user to click
    imageShown = false;
    messageShown = false;
    // ensure main image hidden until user clicks 1. Зураг
    mainImg.classList.remove('visible');
    msgText.classList.remove('visible');
    // Start audio after a user gesture (we are in unlocked by user click)
    bgAudio.volume = 0.6;
    bgAudio.play().catch(()=>{/*ignore*/});
  } else {
    lockOverlay.style.display = 'flex';
    contentArea.style.display = 'none';
    panel.setAttribute('aria-hidden','true');
    localStorage.removeItem('unlocked');
  }
}

/* If user previously chose remember */
if(localStorage.getItem('unlocked') === '1'){
  setUnlocked(true);
} else {
  setUnlocked(false);
}

/* Unlock button handler */
unlockBtn.addEventListener('click', ()=>{
  const val = codeInput.value.trim();
  if(val === PASS){
    if(rememberChk.checked) localStorage.setItem('unlocked','1');
    setUnlocked(true);
  } else {
    alert('Код буруу байна. Дахин оролдоно уу.');
    codeInput.value='';
    codeInput.focus();
  }
});
closeLockBtn.addEventListener('click', ()=> {
  // allow user to cancel but keep overlay shown
  lockOverlay.style.display = 'none';
});

/* Start button: user gesture — attempt to play */
startBtn.addEventListener('click', async ()=>{
  try{
    bgAudio.volume = 0.6;
    await bgAudio.play();
    muteBtn.textContent = '🔊 Music: On';
  }catch(e){
    alert('Дуу тоглох оролдлого амжилтгүй боллоо.');
  }
});

/* Mute toggle */
muteBtn.addEventListener('click', ()=>{
  if(bgAudio.paused){
    bgAudio.play().then(()=> muteBtn.textContent = '🔊 Music: On').catch(()=> alert('Play blocked'));
  } else {
    bgAudio.pause(); muteBtn.textContent = '🔊 Music: Off';
  }
});

/* Show image function (user must click) */
function showImage(src){
  if(src) mainImg.src = src;
  imageShown = true;
  mainImg.classList.add('visible');
  // mark active thumbnail based on src
  thumbs.forEach(t=> t.classList.remove('active'));
  const active = thumbs.find(t=> t.getAttribute('data-src') === mainImg.src);
  if(active) active.classList.add('active');
  // small pop & spawn hearts to show it's opened
  spawnClickHearts(6, panel.getBoundingClientRect().width/2, panel.getBoundingClientRect().height/2);
}

/* Show message function (user must click) */
function showMessage(){
  messageShown = true;
  msgText.classList.add('visible');
  openMessageEffects();
}

/* Thumbnails switching — if image not shown, clicking a thumbnail will also show it */
thumbs.forEach((t,i)=>{
  t.addEventListener('click', ()=>{
    const src = t.getAttribute('data-src');
    if(!imageShown) {
      showImage(src);
    } else {
      mainImg.src = src;
      // animate
      mainImg.classList.add('visible');
    }
    thumbs.forEach(x=>x.classList.remove('active'));
    t.classList.add('active');
    // also update background photo slightly to match
    bgPhoto.style.backgroundImage = `url('${src}')`;
  });
});

/* Edit message modal */
editMsgBtn.addEventListener('click', ()=>{
  if(!unlocked){ alert('Нэвтэрнэ үү (код шаардлагатай)'); return; }
  editArea.value = msgText.textContent.trim() === '' ? '' : msgText.textContent.replace(/<br><br>/g, '\n\n');
  editModal.classList.add('show');
});
closeEdit.addEventListener('click', ()=> editModal.classList.remove('show'));
saveMsg.addEventListener('click', ()=>{
  const text = editArea.value.trim();
  msgText.innerHTML = text ? text.replace(/\n/g, '<br><br>') : '';
  editModal.classList.remove('show');
});

/* Open tabs (require clicks to reveal content) */
openPic.addEventListener('click', ()=> { 
  if(!unlocked){ alert('Нэвтэрнэ үү (код шаардлагатай)'); return; }
  // show image area (if not shown yet)
  if(!imageShown){
    // default to first thumbnail if none
    const defaultSrc = thumbs[0] ? thumbs[0].getAttribute('data-src') : '';
    showImage(defaultSrc);
    // update background too
    if(defaultSrc) bgPhoto.style.backgroundImage = `url('${defaultSrc}')`;
  } else {
    mainImg.scrollIntoView({behavior:'smooth', block:'center'});
  }
});
openMsg.addEventListener('click', ()=> { 
  if(!unlocked){ alert('Нэвтэрнэ үү (код шаардлагатай)'); return; }
  if(!messageShown){
    showMessage();
  } else {
    msgText.scrollIntoView({behavior:'smooth', block:'center'});
  }
});
openGift.addEventListener('click', ()=> {
  if(!unlocked){ alert('Нэвтэрнэ үү (код шаардлагатай)'); return; }
  openGiftBtn.scrollIntoView({behavior:'smooth', block:'center'});
});
openGiftBtn.addEventListener('click', ()=> {
  window.open('https://docs.google.com/forms/d/e/1FAIpQLSf0HQZNTqq9a-tNA426YsT_GyWDYjvRCLxIWA2QOgId0Sef8w/viewform?pli=1', '_blank', 'noopener');
});

/* Click image => fireworks + hearts (only if unlocked and image shown) */
function panelRect(){
  return panel.getBoundingClientRect();
}
mainImg.addEventListener('click', (e)=>{
  if(!unlocked){
    alert('Эхлээд нэвтэрнэ үү (код шаардлагатай).');
    return;
  }
  if(!imageShown){
    alert('Зургыг харахын тулд эхлээд "1. Зураг" дээр дарна уу.');
    return;
  }
  const rect = panelRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  spawnFireworks(x, y);
  spawnClickHearts(8, x, y);
});

/* Fireworks particle system */
let particles = [];
function createFirework(x, y, color){
  for(let i=0;i<26 + Math.floor(Math.random()*18); i++){
    const angle = Math.random()*Math.PI*2;
    const speed = 2 + Math.random()*4;
    particles.push({
      x,y,
      vx: Math.cos(angle)*speed,
      vy: Math.sin(angle)*speed - (Math.random()*1.2),
      life: 60 + Math.floor(Math.random()*60),
      decay: 0.95 + Math.random()*0.02,
      size: 2 + Math.random()*3,
      color: color || `hsl(${Math.floor(Math.random()*360)},80%,60%)`
    });
  }
}
function spawnFireworks(x,y){
  createFirework(x,y, `hsl(${Math.random()*40 + 320},80%,60%)`);
  createFirework(x,y, `hsl(${Math.random()*80 + 160},80%,60%)`);
}

/* hearts at click */
function spawnClickHearts(n,x,y){
  for(let i=0;i<n;i++){
    const s = 12 + Math.random()*28;
    const el = document.createElementNS('http://www.w3.org/2000/svg','svg');
    el.setAttribute('viewBox','0 0 24 24');
    el.style.position='absolute';
    el.style.left = (x - s/2) + 'px';
    el.style.top = (y - s/2) + 'px';
    el.style.width = s + 'px';
    el.style.height = s + 'px';
    el.style.zIndex = 40;
    const path = document.createElementNS('http://www.w3.org/2000/svg','path');
    path.setAttribute('d','M12 21s-7-4.35-9-7.21C-0.96 9.99 4 4 7.5 7.5 9 9 12 12 12 12s3-3 4.5-4.5C20 4 24.96 9.99 21 13.79 19 16.65 12 21 12 21z');
    path.setAttribute('fill', ['#ff6fa6','#ff9ab3','#ffd1de','#ff7aa5'][Math.floor(Math.random()*4)]);
    el.appendChild(path);
    heartsLayer.appendChild(el);
    const dx = (Math.random()*140 - 70);
    const dy = - (80 + Math.random()*140);
    el.animate([
      { transform: 'translate(0,0) rotate(0deg)', opacity: 1 },
      { transform: `translate(${dx}px, ${dy}px) rotate(${(Math.random()*80-40)}deg)`, opacity: 0 }
    ], { duration: 900 + Math.random()*900, easing: 'cubic-bezier(.2,.7,.2,1)'});
    setTimeout(()=> { try{ el.remove() }catch(e){} }, 2000);
  }
}

/* draw loop for fireworks/confetti */
let confetti = [];
function spawnConfetti(count=40){
  const r = panelRect();
  for(let i=0;i<count;i++){
    confetti.push({
      x: Math.random()*r.width,
      y: -10 - Math.random()*200,
      vx: (Math.random()-0.5)*1.6,
      vy: 1 + Math.random()*2.6,
      w: 6 + Math.random()*12,
      h: 4 + Math.random()*8,
      rot: Math.random()*360,
      color: `hsl(${Math.floor(Math.random()*360)},80%,60%)`,
      life: 300 + Math.random()*200
    });
  }
}

function update(){
  ctx.clearRect(0,0,fxCanvas.width,fxCanvas.height);
  for(let i=particles.length-1;i>=0;i--){
    const p=particles[i];
    p.vy += 0.03;
    p.x += p.vx; p.y += p.vy;
    p.vx *= p.decay; p.vy *= p.decay;
    p.life--;
    ctx.globalCompositeOperation = 'lighter';
    ctx.fillStyle = p.color;
    ctx.beginPath();
    ctx.arc(p.x, p.y, Math.max(0.6, p.size * (p.life/120)), 0, Math.PI*2);
    ctx.fill();
    if(p.life<=0 || p.y > fxCanvas.height + 50) particles.splice(i,1);
  }
  for(let i=confetti.length-1;i>=0;i--){
    const c = confetti[i];
    c.x += c.vx; c.y += c.vy; c.vy += 0.03; c.rot += 3;
    ctx.save(); ctx.translate(c.x, c.y); ctx.rotate(c.rot * Math.PI/180);
    ctx.fillStyle = c.color; ctx.fillRect(-c.w/2, -c.h/2, c.w, c.h);
    ctx.restore();
    c.life--;
    if(c.y > fxCanvas.height + 40 || c.life<=0) confetti.splice(i,1);
  }
  requestAnimationFrame(update);
}
update();

/* message open effects */
function openMessageEffects(){
  spawnConfetti(80);
  try{
    const AudioCtx = window.AudioContext || window.webkitAudioContext;
    if(AudioCtx){
      const ac = new AudioCtx();
      const o = ac.createOscillator(); const g = ac.createGain();
      o.type = 'sine'; o.frequency.value = 880;
      o.connect(g); g.connect(ac.destination);
      const now = ac.currentTime; g.gain.setValueAtTime(0.0001, now); g.gain.exponentialRampToValueAtTime(0.22, now+0.02);
      o.start(now); g.gain.exponentialRampToValueAtTime(0.0001, now+0.45);
      setTimeout(()=> o.stop(), 600);
    }
  }catch(e){}
}

/* small accessibility: pressing Enter on code input triggers unlock */
codeInput.addEventListener('keydown', (e)=>{
  if(e.key === 'Enter') unlockBtn.click();
});

/* If user closes overlay (Cancel) and later wants it back, show lock again by double-clicking Start (debug) */
startBtn.addEventListener('dblclick', ()=> { lockOverlay.style.display = 'flex'; });

/* initial UI state */
contentArea.style.display = unlocked ? 'flex' : 'none';

</script>
</body>
</html>
