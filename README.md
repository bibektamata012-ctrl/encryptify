<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Encryptify – AES Encryption Tool</title>
<meta name="description" content="Encryptify is a free client-side AES-256 encryption tool for protecting your privacy. Encrypt text and files securely in your browser.">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Favicon -->
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='0.9em' font-size='90'>🔐</text></svg>">

<style>
:root {
  --bg: #020617;
  --text: #e5e7eb;
  --card: #0f172a;
  --accent: #38bdf8;
  --gradient-card: linear-gradient(135deg, #2563eb, #38bdf8);
}

body.light {
  --bg: #f8fafc;
  --text: #020617;
  --card: #ffffff;
  --gradient-card: linear-gradient(135deg, #38bdf8, #60a5fa);
}

body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: var(--bg);
  color: var(--text);
  transition: all 0.5s ease;
}

/* Header */
header {
  display: flex;
  justify-content: space-between;
  padding: 15px 30px;
  background: var(--bg);
  position: sticky;
  top: 0;
  z-index: 100;
  transition: background 0.5s;
}
header .logo { font-weight: bold; font-size: 22px; color: var(--accent); }
header nav a { color: var(--text); margin-left: 15px; text-decoration: none; font-size: 14px; }
.toggle { cursor: pointer; margin-left: 20px; font-size: 20px; }

/* Hero Section */
.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 120px 20px;
  background: linear-gradient(135deg, #1e293b, #0f172a);
  overflow: hidden;
}

.hero h1 {
  font-size: 48px;
  margin: 0;
  opacity: 0;
  transform: translateY(-50px);
  animation: fadeUp 1s ease forwards 0.5s;
}

.hero p {
  max-width: 650px;
  margin: 20px auto;
  color: #94a3b8;
  font-size: 18px;
  opacity: 0;
  transform: translateY(-30px);
  animation: fadeUp 1s ease forwards 1s;
}

.hero button {
  margin-top: 20px;
  padding: 16px 35px;
  border-radius: 12px;
  border: none;
  background: var(--accent);
  color: var(--bg);
  font-weight: bold;
  cursor: pointer;
  font-size: 16px;
  opacity: 0;
  transform: scale(0.8);
  animation: fadeUp 0.8s ease forwards 1.5s;
  transition: transform 0.3s;
}

.hero button:hover { transform: scale(1.05); }

@keyframes fadeUp { to { opacity: 1; transform: translateY(0) scale(1); } }

/* Sections */
section { padding: 60px 20px; }
.card {
  max-width: 500px;
  margin: 20px auto;
  padding: 25px;
  border-radius: 16px;
  background: var(--gradient-card);
  color: var(--text);
  box-shadow: 0 20px 40px rgba(0,0,0,.6);
  transition: all 0.4s;
}
.card:hover { transform: translateY(-5px); box-shadow: 0 25px 50px rgba(0,0,0,0.7); }

/* Floating Labels */
.input-container { position: relative; margin-bottom: 20px; }
.input-container input,
.input-container textarea {
  width: 100%; padding: 12px 10px 12px 10px;
  border-radius: 8px; border: none;
  background: rgba(0,0,0,0.2);
  color: var(--text); transition: 0.3s;
}
.input-container label {
  position: absolute; left: 12px; top: 12px; color: #94a3b8;
  pointer-events: none; transition: 0.3s;
}
.input-container input:focus + label,
.input-container input:not(:placeholder-shown) + label,
.input-container textarea:focus + label,
.input-container textarea:not(:placeholder-shown) + label {
  top: -8px; left: 8px; background: var(--gradient-card); padding: 0 5px;
  font-size: 12px; color: #fff;
}
textarea:focus, input:focus { outline: 2px solid var(--accent); }

/* Buttons */
.btn-group { display: flex; gap: 10px; margin-bottom: 10px; }
button { flex: 1; padding: 12px; border-radius: 10px; border: none; cursor: pointer; background: #2563eb; color: white; transition: 0.3s; }
button:hover { background: linear-gradient(90deg, #2563eb, #38bdf8); }

/* Password Strength Bar */
#strength-bar { height: 6px; width: 100%; border-radius: 4px; background: #374151; margin-bottom: 16px; overflow: hidden; }
#strength-bar div { height: 100%; width: 0%; transition: width 0.3s; }

/* Features Grid */
.features { text-align: center; }
.grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; max-width: 900px; margin: auto; }
.feature { background: var(--gradient-card); padding: 20px; border-radius: 12px; transition: transform 0.3s, box-shadow 0.3s; }
.feature:hover { transform: translateY(-5px); box-shadow: 0 20px 40px rgba(0,0,0,0.5); }

/* Footer */
footer { text-align: center; padding: 20px; font-size: 12px; color: #64748b; }

/* Toast */
#toast {
  display: none; position: fixed; bottom: 20px; right: 20px;
  background: var(--accent); color: var(--bg); padding: 10px 20px;
  border-radius: 12px; box-shadow: 0 5px 15px rgba(0,0,0,.3); font-weight: bold; z-index: 1000;
}
</style>
</head>
<body>

<header>
  <div class="logo">Encryptify</div>
  <nav>
    <a href="#tool">Tool</a>
    <a href="#features">Features</a>
    <span class="toggle" onclick="toggleTheme()">🌗</span>
  </nav>
</header>

<section class="hero">
  <h1>Protect Your Privacy</h1>
  <p>Encrypt text and files using AES‑256 directly in your browser. No data is sent to any server.</p>
  <button onclick="document.getElementById('tool').scrollIntoView({behavior:'smooth'})">Start Encrypting</button>
</section>

<section id="tool">
  <div class="card">
    <h2>AES Encrypt / Decrypt</h2>

    <div class="input-container">
      <textarea id="text" placeholder=" "></textarea>
      <label>Text</label>
    </div>

    <div class="input-container">
      <input type="password" id="password" placeholder=" " oninput="checkStrength()"/>
      <label>Password</label>
    </div>

    <div id="strength-bar"><div></div></div>

    <div class="btn-group">
      <button onclick="encryptText()">Encrypt Text</button>
      <button onclick="decryptText()">Decrypt Text</button>
    </div>

    <div class="input-container">
      <input type="file" id="fileInput" />
      <label>File</label>
    </div>

    <div class="btn-group">
      <button onclick="encryptFile()">Encrypt File</button>
      <button onclick="decryptFile()">Decrypt File</button>
    </div>

    <div class="input-container">
      <textarea id="result" readonly placeholder=" "></textarea>
      <label>Result</label>
    </div>

    <div class="btn-group">
      <button onclick="copyResult()">Copy Result</button>
      <button onclick="clearAll()">Clear All</button>
    </div>
  </div>
</section>

<section id="features" class="features">
  <h2>Why Encryptify?</h2>
  <div class="grid">
    <div class="feature">🔐 AES‑256 Encryption</div>
    <div class="feature">🌐 Browser‑Only</div>
    <div class="feature">🚫 No Data Stored</div>
    <div class="feature">🌗 Dark / Light Mode</div>
    <div class="feature">📱 Mobile Friendly</div>
    <div class="feature">🧠 Educational & Secure</div>
  </div>
</section>

<footer>© 2025 Encryptify • Privacy First</footer>
<div id="toast"></div>

<script>
// Dark/Light theme
function toggleTheme(){ document.body.classList.toggle('light'); }

// Toast notification
function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;
  t.style.display='block';
  setTimeout(()=>{t.style.display='none';},2000);
}

// Password strength
function checkStrength(){
  const pass=document.getElementById('password').value;
  const bar=document.querySelector('#strength-bar div');
  if(pass.length<6){ bar.style.width='30%'; bar.style.background='red'; }
  else if(pass.match(/[A-Z]/)&&pass.match(/[0-9]/)){ bar.style.width='100%'; bar.style.background='green'; }
  else{ bar.style.width='60%'; bar.style.background='orange'; }
}

// Copy / clear
function copyResult(){ navigator.clipboard.writeText(document.getElementById('result').value); showToast('Copied!'); }
function clearAll(){
  document.getElementById('text').value='';
  document.getElementById('password').value='';
  document.getElementById('result').value='';
  document.getElementById('fileInput').value='';
  document.querySelector('#strength-bar div').style.width='0%';
}

// AES key generation
async function getKey(password){
  const enc=new TextEncoder();
  const keyMaterial=await crypto.subtle.importKey('raw', enc.encode(password), {name:'PBKDF2'}, false, ['deriveKey']);
  return crypto.subtle.deriveKey({name:'PBKDF2', salt: enc.encode('encryptify'), iterations:100000, hash:'SHA-256'}, keyMaterial, {name:'AES-GCM', length:256}, false, ['encrypt','decrypt']);
}

// Text encryption/decryption
async function encryptText(){
  const text=document.getElementById('text').value; if(!text){showToast('Enter text!'); return;}
  const pass=document.getElementById('password').value;
  const iv=crypto.getRandomValues(new Uint8Array(12));
  const key=await getKey(pass);
  const enc=new TextEncoder();
  const encrypted=await crypto.subtle.encrypt({name:'AES-GCM', iv}, key, enc.encode(text));
  const data=new Uint8Array([...iv,...new Uint8Array(encrypted)]);
  document.getElementById('result').value=btoa(String.fromCharCode(...data));
}

async function decryptText(){
  const text=document.getElementById('text').value; if(!text){showToast('Enter text!'); return;}
  try{
    const data=Uint8Array.from(atob(text),c=>c.charCodeAt(0));
    const iv=data.slice(0,12); const encData=data.slice(12);
    const key=await getKey(document.getElementById('password').value);
    const decrypted=await crypto.subtle.decrypt({name:'AES-GCM', iv}, key, encData);
    document.getElementById('result').value=new TextDecoder().decode(decrypted);
  }catch{ showToast('Decryption failed!'); }
}

// File encryption/decryption
async function encryptFile(){
  const file=document.getElementById('fileInput').files[0]; if(!file){showToast('Select file!'); return;}
  const pass=document.getElementById('password').value;
  const key=await getKey(pass); const iv=crypto.getRandomValues(new Uint8Array(12));
  const buffer=await file.arrayBuffer();
  const encBuffer=await crypto.subtle.encrypt({name:'AES-GCM', iv}, key, buffer);
  const blob=new Blob([iv, new Uint8Array(encBuffer)],{type:'application/octet-stream'});
  const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='encrypted_'+file.name; a.click();
  showToast('File encrypted!');
}

async function decryptFile(){
  const file=document.getElementById('fileInput').files[0]; if(!file){showToast('Select file!'); return;}
  const pass=document.getElementById('password').value;
  const buffer=await file.arrayBuffer(); const data=new Uint8Array(buffer);
  const iv=data.slice(0,12); const encData=data.slice(12);
  try{
    const key=await getKey(pass);
    const decBuffer=await crypto.subtle.decrypt({name:'AES-GCM', iv}, key, encData);
    const blob=new Blob([decBuffer],{type:'application/octet-stream'});
    const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='decrypted_'+file.name; a.click();
    showToast('File decrypted!');
  }catch{ showToast('Decryption failed!'); }
}
</script>

</body>
</html>
