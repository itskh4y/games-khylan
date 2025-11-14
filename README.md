<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Flirty Truth or Dare — Untuk Dia</title>
  <style>
    :root{--bg:#fff0f6;--card:#ffffff;--accent:#ff6b95;--muted:#6b6b6b}
    *{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{margin:0;background:linear-gradient(180deg,var(--bg),#ffeef6);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px}
    .wrap{width:100%;max-width:760px;background:radial-gradient(circle at 10% 10%, rgba(255,255,255,0.8), rgba(255,255,255,0.95));border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,0.08);padding:28px}
    header{display:flex;align-items:center;gap:16px}
    .logo{width:68px;height:68px;border-radius:14px;background:linear-gradient(135deg,#fff,#ffd9e6);display:flex;align-items:center;justify-content:center;font-weight:700;color:var(--accent);font-size:28px;border:3px solid rgba(255,107,149,0.12)}
    h1{margin:0;font-size:20px;color:#3a2b33}
    p.lead{margin:6px 0 18px;color:var(--muted)}

    .controls{display:flex;gap:12px;align-items:center;flex-wrap:wrap;margin-bottom:18px}
    .btn{background:var(--accent);color:white;border:none;padding:10px 16px;border-radius:10px;cursor:pointer;font-weight:600;box-shadow:0 6px 18px rgba(255,107,149,0.18)}
    .btn.secondary{background:#fff;border:1px solid rgba(0,0,0,0.06);color:#333}
    .card{background:var(--card);border-radius:14px;padding:20px;box-shadow:0 6px 20px rgba(0,0,0,0.04);min-height:120px;display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center}
    .prompt{font-size:18px;font-weight:700;color:#3a2b33;margin-bottom:10px}
    .hint{color:var(--muted);font-size:14px}
    footer{display:flex;justify-content:space-between;align-items:center;margin-top:14px}
    .spice{display:flex;gap:8px;align-items:center}
    input[type=range]{accent-color:var(--accent)}

    .small{font-size:13px;color:var(--muted)}
    .actions{display:flex;gap:10px}

    /* responsive */
    @media (max-width:520px){.controls{flex-direction:column;align-items:stretch}.actions{justify-content:space-between}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">💗</div>
      <div>
        <h1>Flirty Truth or Dare</h1>
        <p class="lead">Tema: <strong>Flirty & Nakal Manis (PG-13)</strong> — manis, menggoda, tapi aman. Simpan game ini offline atau bagikan link berkas ke dia.</p>
      </div>
    </header>

    <div style="margin-top:18px" class="controls">
      <div class="actions">
        <button id="truthBtn" class="btn">Truth</button>
        <button id="dareBtn" class="btn">Dare</button>
        <button id="surpriseBtn" class="btn secondary">Surprise</button>
      </div>

      <div style="margin-left:auto;display:flex;gap:10px;align-items:center">
        <div class="spice">
          <label for="spiceRange" class="small">Tingkat nakal:</label>
          <input id="spiceRange" type="range" min="1" max="10" value="4">
        </div>
        <button id="remixBtn" class="btn secondary">Remix</button>
      </div>
    </div>

    <div class="card">
      <div id="prompt" class="prompt">Tekan <strong>Truth</strong> atau <strong>Dare</strong> untuk memulai — atau pilih Surprise untuk kejutan.</div>
      <div id="hint" class="hint">Atur tingkat nakal untuk menyesuaikan mood. (1 = manis, 10 = nakal tapi tetap PG-13)</div>
    </div>

    <footer>
      <div class="small">Mode dibuat untuk: <strong>Flirty & Nakal Manis</strong> — cocok buat suasana LDR atau malam santai.</div>
      <div style="display:flex;gap:8px;align-items:center">
        <button id="addCustom" class="btn secondary">Tambah Custom</button>
        <button id="resetBtn" class="btn secondary">Reset</button>
      </div>
    </footer>

    <div id="customArea" style="margin-top:14px;display:none;gap:8px">
      <input id="customInput" placeholder="Tulis truth/dare (max 120 char)" style="flex:1;padding:10px;border-radius:8px;border:1px solid rgba(0,0,0,0.08)">
      <select id="typeSelect" style="padding:10px;border-radius:8px;border:1px solid rgba(0,0,0,0.08)">
        <option value="truth">Truth</option>
        <option value="dare">Dare</option>
      </select>
      <label class="small">Level
        <input id="customLevel" type="number" min="1" max="10" value="4" style="width:60px;margin-left:6px">
      </label>
      <button id="saveCustom" class="btn">Simpan</button>
    </div>
  </div>

  <script>
    // Data: truth & dare lists with spice level (1-10). Keep content PG-13: suggestive but non-explicit.
    const truths = [
      {t: 'Bagian tubuh aku mana yang paling kamu perhatiin?', lvl:4},
      {t: 'Pernah kepikiran mimpi nakal tentang aku? Ceritakan (1 kalimat).', lvl:6},
      {t: 'Hal manis apa yang kamu ingin aku lakukan besok?', lvl:3},
      {t: 'Kapan terakhir kamu kepikiran aku saat sendiri?', lvl:4},
      {t: 'Pernah naksir gara-gara pakaian aku? Jelasin!', lvl:5},
      {t: 'Apa kata gombal yang pernah berhasil bikin kamu meleleh?', lvl:2},
      {t: 'Pernah kepikiran kita berciuman di tempat umum? Ceritain reaksimu.', lvl:7},
      {t: 'Apa fantasi sederhana yang kamu punya tentang aku?', lvl:7},
      {t: 'Hal kecil apa yang membuatmu langsung kangen?', lvl:3},
      {t: 'Jika aku lagi sedih, apa yang kamu lakukan supaya aku tenang?', lvl:2}
    ];

    const dares = [
      {t: 'Kirim voice note 10 detik bilang "aku kangen" dengan nada menggoda.', lvl:5},
      {t: 'Kirim foto imut dengan ekspresi genit.', lvl:4},
      {t: 'Buat 3 julukan manis untuk aku dan kirim satu demi satu.', lvl:3},
      {t: 'Deskripsikan bagaimana kamu mau aku peluk kamu, 2 kalimat.', lvl:5},
      {t: 'Ketik 3 emoji yang mewakili mood nakalmu sekarang.', lvl:2},
      {t: 'Buat suara "kiss" lewat voice note 3 detik.', lvl:6},
      {t: 'Tutup matamu 5 detik dan bayangkan aku di depanmu — ceritakan.', lvl:7},
      {t: 'Kirimi aku pesan singkat seolah-olah kita lagi berpelukan sekarang.', lvl:4},
      {t: 'Pura-pura cemburu selama 10 detik di chat (sesuai batas nyaman).', lvl:6},
      {t: 'Tulis satu kalimat yang bikin aku melting tanpa kata kasar.', lvl:3}
    ];

    // allow user additions in session
    let customTruths = [];
    let customDares = [];

    const promptEl = document.getElementById('prompt');
    const hintEl = document.getElementById('hint');
    const spiceRange = document.getElementById('spiceRange');

    function pickRandom(type){
      const level = Number(spiceRange.value);
      const pool = (type === 'truth')
        ? truths.concat(customTruths)
        : dares.concat(customDares);
      // filter by level: allow +-2 tolerance so results vary
      const filtered = pool.filter(x => Math.abs(x.lvl - level) <= 2 || x.lvl <= level);
      if(filtered.length === 0){
        return {t: 'Wah, belum ada pertanyaan sesuai tingkat nakal ini. Turunkan atau tambah custom!'};
      }
      return filtered[Math.floor(Math.random()*filtered.length)];
    }

    document.getElementById('truthBtn').addEventListener('click', ()=>{
      const item = pickRandom('truth');
      showPrompt('Truth', item.t);
    });
    document.getElementById('dareBtn').addEventListener('click', ()=>{
      const item = pickRandom('dare');
      showPrompt('Dare', item.t);
    });
    document.getElementById('surpriseBtn').addEventListener('click', ()=>{
      // surprise picks either truth or dare randomly
      const type = Math.random() < 0.5 ? 'truth' : 'dare';
      const item = pickRandom(type);
      showPrompt(type.charAt(0).toUpperCase() + type.slice(1), item.t);
    });

    document.getElementById('remixBtn').addEventListener('click', ()=>{
      // simply re-run last displayed type if possible
      const lastType = promptEl.getAttribute('data-type') || (Math.random()<0.5?'Truth':'Dare');
      const item = pickRandom(lastType.toLowerCase());
      showPrompt(lastType, item.t);
    });

    function showPrompt(kind, text){
      promptEl.setAttribute('data-type', kind);
      promptEl.innerHTML = `<span style=\"font-size:14px;color:var(--muted)\">${kind}</span><br>${escapeHtml(text)}`;
      hintEl.textContent = 'Tekan Remix untuk opsi lain, atau ganti tingkat nakal.';
      pulseCard();
    }

    function pulseCard(){
      const card = document.querySelector('.card');
      card.animate([
        {transform:'scale(1)'},
        {transform:'scale(1.02)'},
        {transform:'scale(1)'}
      ],{duration:420,iterations:1});
    }

    // small utilities for custom add
    document.getElementById('addCustom').addEventListener('click', ()=>{
      const area = document.getElementById('customArea');
      area.style.display = area.style.display === 'flex' ? 'none' : 'flex';
      area.style.marginTop = '12px';
      area.style.alignItems = 'center';
    });
    document.getElementById('saveCustom').addEventListener('click', ()=>{
      const text = document.getElementById('customInput').value.trim();
      const type = document.getElementById('typeSelect').value;
      const level = Number(document.getElementById('customLevel').value) || 4;
      if(!text || text.length > 180){
        alert('Teks kosong atau terlalu panjang. Maks 180 karakter.');
        return;
      }
      if(type === 'truth') customTruths.push({t:text,lvl:level}); else customDares.push({t:text,lvl:level});
      document.getElementById('customInput').value = '';
      alert('Tersimpan (sementara di sesi ini). Tekan Truth/Dare untuk coba.');
    });

    document.getElementById('resetBtn').addEventListener('click', ()=>{
      if(confirm('Reset custom untuk sesi ini?')){
        customTruths = [];
        customDares = [];
        promptEl.textContent = 'Sudah direset. Tekan Truth atau Dare untuk mulai lagi.';
        promptEl.removeAttribute('data-type');
      }
    });

    // tiny escape
    function escapeHtml(s){
      return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    }

    // friendly tip on first load
    (function(){
      hintEl.textContent = 'Tip: atur tingkat nakal dan pakai Surprise kalau mau kejutan.';
    })();
  </script>
</body>
</html>

