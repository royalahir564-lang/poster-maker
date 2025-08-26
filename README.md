<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple Festival Poster Maker</title>
<style>
  :root{
    --bg:#0f1226; --card:#151936; --accent:#7c5cff; --muted:#8a91b4; --good:#19c37d;
  }
  *{box-sizing:border-box}
  body{
    margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Inter,Arial,sans-serif;
    background: radial-gradient(1200px 800px at 20% -10%, #1d2250 10%, #0f1226 60%);
    color:#e6e8ff;
  }
  header{
    padding:14px 18px; display:flex; align-items:center; gap:10px; border-bottom:1px solid #202552;
    position:sticky; top:0; backdrop-filter: blur(6px);
    background:linear-gradient(180deg, rgba(15,18,38,0.75), rgba(15,18,38,0.35));
    z-index:5;
  }
  header h1{font-size:18px; margin:0; letter-spacing:.3px}
  .app{display:grid; grid-template-columns: 360px 1fr; gap:18px; padding:18px; max-width:1400px; margin:0 auto;}
  @media (max-width: 980px){ .app{grid-template-columns:1fr; } }
  .panel{
    background:linear-gradient(180deg, #151936, #101433);
    border:1px solid #262b5b; border-radius:14px; padding:14px; box-shadow: 0 8px 30px rgba(0,0,0,.25);
  }
  .panel h2{font-size:14px; color:#bfc6ff; margin:4px 0 10px 0; font-weight:600; letter-spacing:.4px; text-transform:uppercase}
  .row{display:flex; align-items:center; gap:10px; margin:10px 0;}
  .row label{font-size:13px; width:110px; color:#a9b2ff}
  .row input[type="text"]{flex:1; padding:10px 12px; border-radius:10px; border:1px solid #2a3170; background:#0f1330; color:#e6e8ff; outline:none}
  .row input[type="file"]{flex:1; color:#b8bff8}
  .row select, .row input[type="color"]{
    flex:1; padding:10px; border-radius:10px; border:1px solid #2a3170; background:#0f1330; color:#e6e8ff; outline:none
  }
  .slider{appearance:none; width:100%; height:6px; border-radius:20px; background:#242a63; outline:none}
  .slider::-webkit-slider-thumb{appearance:none; width:18px; height:18px; border-radius:50%; background:var(--accent); cursor:pointer; border:2px solid #cfd2ff}
  .hint{font-size:12px; color:#96a1ff; margin-top:-6px}
  .btn{
    background:linear-gradient(180deg, #7c5cff, #6043ff);
    color:white; border:0; padding:11px 14px; border-radius:10px; cursor:pointer; font-weight:600;
    box-shadow:0 6px 18px rgba(124,92,255,.35); transition:transform .05s ease;
  }
  .btn:active{ transform:translateY(1px) }
  .btn.alt{ background:#0f1330; border:1px solid #2a3170; box-shadow:none }
  .canvas-wrap{display:flex; justify-content:center; align-items:center;}
  canvas{width:100%; height:auto; background:#000; border-radius:16px; border:1px solid #242a63}
  .ads{
    height:90px; border-radius:12px; border:1px dashed #3a4186; display:flex; align-items:center; justify-content:center;
    color:#9aa3ff; font-size:13px; background:linear-gradient(180deg,#13183a,#0f1432); margin-top:8px
  }
  .pill{display:inline-flex; gap:8px; padding:8px 10px; border-radius:999px; border:1px solid #2f3570; color:#b8bff8; font-size:12px}
  .grid-2{display:grid; grid-template-columns:1fr 1fr; gap:10px}
  .sep{height:1px; background:#222863; margin:10px 0}
</style>
</head>
<body>
  <header>
    <span class="pill">⚡ Free Poster Editor • Ads-supported</span>
    <h1>Festival Poster Maker</h1>
  </header>

  <div class="app">
    <!-- Controls -->
    <section class="panel" aria-label="controls">
      <h2>Template & Inputs</h2>

      <div class="row">
        <label>Template</label>
        <select id="templateSelect" title="Choose a built-in template">
          <option value="diwali">🎆 Diwali Glow (built-in)</option>
          <option value="holi">🎨 Holi Splash (built-in)</option>
          <option value="plain">🖼️ Solid Color (built-in)</option>
        </select>
      </div>
      <div class="row">
        <label>Upload Template</label>
        <input type="file" id="bgUpload" accept="image/*" />
      </div>
      <div class="row">
        <label>Base Color</label>
        <input type="color" id="baseColor" value="#7c5cff" />
      </div>

      <div class="sep"></div>
      <div class="row">
        <label>Your Photo</label>
        <input type="file" id="photoUpload" accept="image/*" />
      </div>
      <div class="grid-2">
        <div>
          <div class="row" style="display:block">
            <label>Photo Zoom</label>
            <input type="range" id="photoScale" class="slider" min="0.5" max="2.0" step="0.01" value="1.0" />
          </div>
        </div>
        <div>
          <div class="row" style="display:block">
            <label>Photo Offset</label>
            <input type="range" id="photoOffsetX" class="slider" min="-300" max="300" step="1" value="0" />
            <input type="range" id="photoOffsetY" class="slider" min="-300" max="300" step="1" value="0" />
            <div class="hint">Drag sliders to reposition</div>
          </div>
        </div>
      </div>

      <div class="sep"></div>
      <div class="row">
        <label>Display Name</label>
        <input type="text" id="userName" placeholder="e.g. Sharma Traders" maxlength="40" />
      </div>
      <div class="grid-2">
        <div class="row">
          <label>Font</label>
          <select id="fontSelect">
            <option value="700 72px Inter, system-ui, sans-serif">Bold Modern</option>
            <option value="700 88px Georgia, 'Times New Roman', serif">Elegant Serif</option>
            <option value="800 84px Poppins, Inter, sans-serif">Poppins Heavy</option>
            <option value="700 80px 'Segoe UI', Inter, sans-serif">Clean UI</option>
          </select>
        </div>
        <div class="row">
          <label>Text Color</label>
          <input type="color" id="textColor" value="#ffffff" />
        </div>
      </div>

      <div class="sep"></div>
      <div class="grid-2">
        <button class="btn" id="downloadBtn">⬇️ Download PNG</button>
        <button class="btn alt" id="resetBtn">↺ Reset</button>
      </div>

      <div class="ads">Ad slot (728×90 / Responsive)</div>
    </section>

    <!-- Canvas Preview -->
    <section class="panel" aria-label="canvas">
      <h2>Preview (1080×1080)</h2>
      <div class="canvas-wrap">
        <canvas id="c" width="1080" height="1080"></canvas>
      </div>
      <div class="ads" style="margin-top:12px">Ad slot (728×90 / Responsive)</div>
    </section>
  </div>

<script>
(() => {
  const W = 1080, H = 1080; // Square for WhatsApp/Instagram
  const c = document.getElementById('c');
  const ctx = c.getContext('2d');

  // Controls
  const templateSelect = document.getElementById('templateSelect');
  const bgUpload = document.getElementById('bgUpload');
  const baseColor = document.getElementById('baseColor');

  const photoUpload = document.getElementById('photoUpload');
  const photoScale = document.getElementById('photoScale');
  const photoOffsetX = document.getElementById('photoOffsetX');
  const photoOffsetY = document.getElementById('photoOffsetY');

  const userName = document.getElementById('userName');
  const fontSelect = document.getElementById('fontSelect');
  const textColor = document.getElementById('textColor');

  const downloadBtn = document.getElementById('downloadBtn');
  const resetBtn = document.getElementById('resetBtn');

  // State
  let bgImg = null;     // user uploaded background
  let photoImg = null;  // user uploaded portrait

  // Utility: load file -> Image
  function fileToImage(file){
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = e => {
        const img = new Image();
        img.onload = () => resolve(img);
        img.onerror = reject;
        // To avoid tainting the canvas, we don't set crossOrigin (local file is fine)
        img.src = e.target.result;
      };
      reader.onerror = reject;
      reader.readAsDataURL(file);
    });
  }

  // Draw background template variants
  function drawBuiltInBG(kind){
    // gradient base
    const hue = baseColor.value;
    const g = ctx.createLinearGradient(0,0,W,H);
    g.addColorStop(0, shade(hue, -10));
    g.addColorStop(1, shade(hue, 40));
    ctx.fillStyle = g;
    ctx.fillRect(0,0,W,H);

    if(kind === 'diwali'){
      // glow rings + diyas (simple shapes)
      for(let i=0;i<5;i++){
        const x = 180 + i*180, y = 260 + (i%2)*60, r = 80 + i*12;
        radialGlow(x,y,r, `rgba(255,232,160,${0.18 - i*0.02})`);
      }
      // bottom arc
      ctx.fillStyle = 'rgba(0,0,0,0.25)';
      ctx.beginPath();
      ctx.moveTo(-50,H-280); ctx.quadraticCurveTo(W/2,H-80, W+50,H-280);
      ctx.lineTo(W+50,H+50); ctx.lineTo(-50,H+50); ctx.closePath(); ctx.fill();

      // tiny sparkles
      sparkle(80); // N sparkles
    }
    else if(kind === 'holi'){
      // color splashes
      blob(260,260,220, hex2rgba(hue,0.80));
      blob(840,360,240, hex2rgba('#ff6b6b',0.75));
      blob(340,820,250, hex2rgba('#45d1a6',0.75));
      paintStroke(160,720, 820,920, hex2rgba('#ffd166',0.85), 120);
      paintStroke(900,140, 260,420, hex2rgba('#4dabf7',0.85), 90);
      sparkle(40);
    }
    else if(kind === 'plain'){
      // subtle vignette
      vignette();
    }

    // Top banner ribbon for festival title (editable later if you want)
    roundedRect(40,40, W-80, 120, 28, hex2rgba('#000',0.18));
  }

  // Helpers
  function roundedRect(x,y,w,h,r, fill){
    ctx.beginPath();
    ctx.moveTo(x+r, y);
    ctx.arcTo(x+w, y, x+w, y+h, r);
    ctx.arcTo(x+w, y+h, x, y+h, r);
    ctx.arcTo(x, y+h, x, y, r);
    ctx.arcTo(x, y, x+w, y, r);
    ctx.closePath();
    ctx.fillStyle = fill; ctx.fill();
  }
  function radialGlow(x,y,r,color){
    const rg = ctx.createRadialGradient(x,y, r*0.1, x,y,r);
    rg.addColorStop(0, color);
    rg.addColorStop(1, 'rgba(255,255,255,0)');
    ctx.fillStyle = rg; ctx.beginPath(); ctx.arc(x,y,r,0,Math.PI*2); ctx.fill();
  }
  function blob(x,y,r, color){
    ctx.save(); ctx.translate(x,y);
    ctx.beginPath();
    for(let i=0;i<7;i++){
      const angle = (i/7)*Math.PI*2;
      const rr = r*(0.75+Math.random()*0.35);
      const nx = Math.cos(angle)*rr, ny = Math.sin(angle)*rr;
      if(i===0) ctx.moveTo(nx,ny); else ctx.quadraticCurveTo(nx*0.7, ny*0.7, nx,ny);
    }
    ctx.closePath(); ctx.fillStyle = color; ctx.fill(); ctx.restore();
  }
  function paintStroke(x1,y1,x2,y2, color, thick=80){
    ctx.save();
    ctx.lineCap='round'; ctx.lineJoin='round';
    ctx.strokeStyle = color; ctx.lineWidth = thick;
    ctx.beginPath(); ctx.moveTo(x1,y1); ctx.quadraticCurveTo((x1+x2)/2+80,(y1+y2)/2-80, x2,y2); ctx.stroke();
    ctx.restore();
  }
  function sparkle(n=40){
    ctx.save();
    for(let i=0;i<n;i++){
      const x = Math.random()*W, y = Math.random()*H*0.9;
      const r = Math.random()*2+0.6;
      ctx.fillStyle = 'rgba(255,255,255,0.85)';
      ctx.beginPath(); ctx.arc(x,y,r,0,Math.PI*2); ctx.fill();
    }
    ctx.restore();
  }
  function vignette(){
    const vg = ctx.createRadialGradient(W/2,H/2, 100, W/2,H/2, W/1.1);
    vg.addColorStop(0, hex2rgba('#000',0));
    vg.addColorStop(1, hex2rgba('#000',0.45));
    ctx.fillStyle = vg;
    ctx.fillRect(0,0,W,H);
  }
  function shade(hex, amt){ // lighten/darken
    const c = hex.replace('#','');
    const num = parseInt(c,16);
    let r=(num>>16)+amt, g=((num>>8)&0xff)+amt, b=(num&0xff)+amt;
    r=Math.max(0,Math.min(255,r)); g=Math.max(0,Math.min(255,g)); b=Math.max(0,Math.min(255,b));
    return '#'+((1<<24)+(r<<16)+(g<<8)+b).toString(16).slice(1);
  }
  function hex2rgba(hex, alpha=1){
    const c = hex.replace('#','');
    const bigint = parseInt(c,16);
    const r = (bigint>>16)&255, g=(bigint>>8)&255, b=bigint&255;
    return `rgba(${r},${g},${b},${alpha})`;
  }

  function drawBackground(){
    if(bgImg){
      // Draw user bg fit-cover
      const ratio = Math.max(W/bgImg.width, H/bgImg.height);
      const nw = bgImg.width*ratio, nh = bgImg.height*ratio;
      const ox = (W - nw)/2, oy = (H - nh)/2;
      ctx.drawImage(bgImg, ox, oy, nw, nh);
      // overlay subtle vignette to improve text contrast
      vignette();
    } else {
      drawBuiltInBG(templateSelect.value);
    }
  }

  function drawPhoto(){
    if(!photoImg) return;
    // Circle frame area
    const cx = W/2, cy = H*0.48;
    const radius = 260;

    // Shadow ring
    radialGlow(cx, cy+10, radius+24, 'rgba(0,0,0,0.35)');

    // Clip circle
    ctx.save();
    ctx.beginPath(); ctx.arc(cx,cy,radius,0,Math.PI*2); ctx.clip();

    // Compute scaled size
    const scale = parseFloat(photoScale.value);
    const pw = photoImg.width * scale;
    const ph = photoImg.height * scale;

    const sx = cx - pw/2 + parseInt(photoOffsetX.value,10);
    const sy = cy - ph/2 + parseInt(photoOffsetY.value,10);

    ctx.drawImage(photoImg, sx, sy, pw, ph);
    ctx.restore();

    // Outer ring
    ctx.lineWidth = 14;
    ctx.strokeStyle = hex2rgba(baseColor.value, 0.9);
    ctx.beginPath(); ctx.arc(cx,cy,radius+6,0,Math.PI*2); ctx.stroke();
  }

  function drawText(){
    const text = userName.value.trim() || 'Your Business Name';
    ctx.save();
    ctx.fillStyle = textColor.value;
    ctx.font = fontSelect.value;
    ctx.textAlign = 'center'; ctx.textBaseline = 'middle';

    // Shadow for better contrast
    ctx.shadowColor = 'rgba(0,0,0,0.45)'; ctx.shadowBlur = 18; ctx.shadowOffsetY = 4;

    // Fit text if too long
    const maxWidth = W - 140;
    let fontSpec = fontSelect.value;
    let size = parseInt(fontSpec.match(/(\d+)px/)[1],10);
    ctx.font = fontSpec;
    while(ctx.measureText(text).width > maxWidth && size > 36){
      size -= 2;
      ctx.font = fontSpec.replace(/(\d+)px/, size+'px');
    }

    ctx.fillText(text, W/2, H*0.82);

    // Optional subtext (festival greeting)
    ctx.shadowBlur = 0; ctx.shadowOffsetY = 0;
    ctx.font = `600 40px Inter, system-ui, sans-serif`;
    ctx.fillStyle = 'rgba(255,255,255,0.9)';
    const festTitle = templateSelect.value === 'diwali' ? 'Happy Diwali' :
                      templateSelect.value === 'holi'   ? 'Happy Holi'   :
                      'Warm Wishes';
    ctx.fillText(festTitle, W/2, 110);
    ctx.restore();
  }

  function render(){
    // Clear
    ctx.clearRect(0,0,W,H);
    // BG
    drawBackground();
    // Photo
    drawPhoto();
    // Text
    drawText();
  }

  // Event wiring
  templateSelect.addEventListener('change', render);
  baseColor.addEventListener('input', render);
  userName.addEventListener('input', render);
  fontSelect.addEventListener('change', render);
  textColor.addEventListener('input', render);

  photoScale.addEventListener('input', render);
  photoOffsetX.addEventListener('input', render);
  photoOffsetY.addEventListener('input', render);

  bgUpload.addEventListener('change', async (e) => {
    if(!e.target.files?.[0]){ bgImg = null; render(); return; }
    bgImg = await fileToImage(e.target.files[0]);
    render();
  });

  photoUpload.addEventListener('change', async (e) => {
    if(!e.target.files?.[0]){ photoImg = null; render(); return; }
    photoImg = await fileToImage(e.target.files[0]);
    // Reset photo transforms
    photoScale.value = "1.0"; photoOffsetX.value = "0"; photoOffsetY.value = "0";
    render();
  });

  downloadBtn.addEventListener('click', () => {
    // Optional: add small unobtrusive watermark/brand (comment out if not needed)
    ctx.save();
    ctx.fillStyle = 'rgba(255,255,255,0.75)';
    ctx.font = '600 24px Inter, system-ui, sans-serif';
    ctx.textAlign = 'right';
    ctx.fillText('Made with PosterMaker', W-24, H-24);
    ctx.restore();

    const url = c.toDataURL('image/png'); // lossless PNG
    const a = document.createElement('a');
    a.href = url; a.download = (userName.value.trim() || 'poster') + '.png';
    document.body.appendChild(a); a.click(); a.remove();

    render(); // re-render to remove temporary watermark from preview
  });

  resetBtn.addEventListener('click', () => {
    bgImg = null; photoImg = null;
    templateSelect.value = 'diwali';
    baseColor.value = '#7c5cff';
    userName.value = '';
    fontSelect.selectedIndex = 0;
    textColor.value = '#ffffff';
    photoScale.value = '1.0'; photoOffsetX.value = '0'; photoOffsetY.value = '0';
    bgUpload.value = ''; photoUpload.value = '';
    render();
  });

  // Initial render
  render();
})();
</script>
</body>
</html>
![1000711204](https://github.com/user-attachments/assets/dfb81f33-3128-4090-9fde-dab9f58e5475)
# poster-maker
