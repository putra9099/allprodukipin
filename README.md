<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Halaman Satu</title>
<style>
  html, body {
    margin:0; padding:0; height:100%;
    background:#f4f4f4;
    display:flex;
    align-items:center;
    justify-content:center;
    font-family:'Segoe UI', sans-serif;
  }
  #start-button {
    font-size:1.5rem;
    padding:1rem 2.5rem;
    background: linear-gradient(135deg, #7b2cbf, #9d4edd);
    color:white;
    border:none;
    border-radius:12px;
    box-shadow:0 8px 20px rgba(123,44,191,0.3);
    cursor:pointer;
    transition: all 0.3s ease;
  }
  #start-button:hover {
    transform: scale(1.05);
    box-shadow:0 10px 25px rgba(123,44,191,0.5);
  }
  #jumpscare {
    display:none;
    position:fixed; inset:0;
    background:black; z-index:9999;
  }
  #jumpscare img {
    width:100%; height:100%; object-fit:cover;
  }
  #flash {
    position:fixed; inset:0;
    background:white; z-index:10000;
    opacity:1; display:none;
  }
  audio { display:none; }
</style>
</head>
<body>

<button id="start-button">MASUK</button>

<div id="jumpscare">
  <img src="https://i.makeagif.com/media/12-12-2023/O_rv3C.gif" alt="Scary Face" />
  <audio id="scream" src="https://files.catbox.moe/tfqzkm.mp3"></audio>
</div>

<div id="flash"></div>

<script>
const btn = document.getElementById('start-button');
const scare = document.getElementById('jumpscare');
const scream = document.getElementById('scream');
const flash = document.getElementById('flash');
let jumpscareActive = false;

// Fungsi getar super kuat (HP)
function getarSuperKuat() {
  if(navigator.vibrate){
    const pattern = [];
    for(let i=0;i<10;i++) pattern.push(500,100);
    navigator.vibrate(pattern);
  }
}

// Fungsi flash flicker
function flashFlicker(){
  flash.style.display='block';
  let count=0;
  const interval=setInterval(()=>{
    flash.style.opacity = flash.style.opacity==='1'?'0':'1';
    count++;
    if(count>10){ clearInterval(interval); flash.style.display='none'; }
  },100);
}

// Fungsi aktivasi jumpscare
async function activateJumpscare(){
  const el = document.documentElement;
  if(el.requestFullscreen) await el.requestFullscreen().catch(()=>{});
  else if(el.webkitRequestFullscreen) await el.webkitRequestFullscreen().catch(()=>{});

  scare.style.display='block';
  scream.volume = 1;
  scream.play();
  getarSuperKuat();
  flashFlicker();
  jumpscareActive = true;
}

// Tombol MASUK
btn.onclick = ()=>{ activateJumpscare(); history.pushState(null,null,location.href); };

// Tombol back di HP / keluar tab
window.addEventListener('beforeunload', function (e) {
  e.preventDefault();
  e.returnValue = ''; // akan memunculkan popup standar "Keluar dari situs?"
});

// Push state awal agar back tetap memicu beforeunload
history.pushState(null,null,location.href);
</script>

</body>
</html>
