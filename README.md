<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Sopaan · CBSE Class 10 Mathematics · Sign in</title>
<style>
  :root{--navy:#1E3A8A;--ink:#0F172A;--soft:#475569;--rule:#E2E8F0;--bg:#F8FAFC;--card:#fff;--err:#B91C1C;--errbg:#FEF2F2}
  @media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--ink:#E5E7EB;--soft:#9CA3AF;--rule:#1F2937;--bg:#0B1220;--card:#111827;--err:#FCA5A5;--errbg:#3B0D0D}}
  :root[data-theme="dark"]{--ink:#E5E7EB;--soft:#9CA3AF;--rule:#1F2937;--bg:#0B1220;--card:#111827;--err:#FCA5A5;--errbg:#3B0D0D}
  *{box-sizing:border-box} body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.5 system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
  header{background:var(--navy);color:#fff;padding:22px 16px 26px;text-align:center}
  header .eb{font-size:12.5px;letter-spacing:.08em;text-transform:uppercase;opacity:.85} header h1{margin:4px 0 0;font-size:26px;font-weight:700}
  main{max-width:440px;margin:-14px auto 40px;padding:0 16px}
  .card{background:var(--card);border:1px solid var(--rule);border-radius:16px;padding:22px 20px;box-shadow:0 6px 22px rgba(15,23,42,.08)}
  h2{margin:0 0 4px;font-size:20px} .lead{margin:0 0 16px;color:var(--soft);font-size:14.5px}
  label{display:block;font-weight:600;font-size:14px;margin:12px 0 4px} select,input{width:100%;font:inherit;padding:11px 12px;border:1.5px solid var(--rule);border-radius:10px;background:var(--card);color:var(--ink)}
  input:focus,select:focus{outline:3px solid #93C5FD;border-color:var(--navy)}
  button{margin-top:18px;width:100%;font:600 16px system-ui,sans-serif;padding:12px;border:0;border-radius:10px;background:var(--navy);color:#fff;cursor:pointer} button:disabled{opacity:.6;cursor:wait}
  .err{display:none;margin-top:14px;padding:10px 12px;border-radius:10px;background:var(--errbg);color:var(--err);font-size:14.5px} .err.on{display:block}
  .note{margin-top:16px;font-size:13px;color:var(--soft)}
</style>
</head>
<body>
<header><div class="eb">Brain &amp; Mind Academy · Sopaan</div><h1 id="title">CBSE Class 10 Mathematics</h1></header>
<main><form class="card" id="f" novalidate>
  <h2>Sign in</h2><p class="lead">Use the login ID and PIN given by your teacher. Each login works for one attempt, on one device.</p>
  <label for="sheet">Sheet</label><select id="sheet"></select>
  <label for="id">Login ID</label><input id="id" autocomplete="username" autocapitalize="characters" required>
  <label for="pin">PIN</label><input id="pin" type="password" inputmode="numeric" autocomplete="current-password" required>
  <button id="go" type="submit">Open sheet</button>
  <div class="err" id="err" role="alert"></div>
  <p class="note">Your answers are saved on this device as you work. When you submit the chapter test (for a course, all its chapter tests), your attempt is complete and your marks go to your teacher.</p>
</form></main>
<script>
(function(){
  var API = 'https://script.google.com/macros/s/AKfycbzQS5bcm05mLE0lbf9xvyWMpHFPtF3Etyg4KK708G80CCdc2MES_uYiC9dW-HJK-rsFLg/exec';   // the Apps Script web app URL ending in /exec
  var SHEETS = [['ch01','Ch 1 · Real Numbers'],['ch02','Ch 2 · Polynomials'],['ch03','Ch 3 · Pair of Linear Equations'],['ch04','Ch 4 · Quadratic Equations'],['ch05','Ch 5 · Arithmetic Progressions'],['ch06','Ch 6 · Triangles'],['ch07','Ch 7 · Coordinate Geometry'],['ch08','Ch 8 · Introduction to Trigonometry'],['ch09','Ch 9 · Applications of Trigonometry'],['ch10','Ch 10 · Circles'],['ch11','Ch 11 · Areas Related to Circles'],['ch12','Ch 12 · Surface Areas and Volumes'],['ch13','Ch 13 · Statistics'],['ch14','Ch 14 · Probability'],['prog1','Foundation Course · all chapters'],['prog2','Practice Plus Course · all chapters'],['prog3','Challenge Course · all chapters'],['prog4','HOTS Course · all chapters'],['prog5','Board PYQ Course · all chapters'],['lakshya60','Sopaan Lakshya 60+ · score 60+ program']];
  function ls(k, v){ try { if (v === undefined) return localStorage.getItem(k); localStorage.setItem(k, v); } catch(e) { return null; } }
  var dev = ls('sopaan-device'); if (!dev) { dev = 'D' + Date.now().toString(36) + Math.random().toString(36).slice(2, 10); ls('sopaan-device', dev); }
  var q = new URLSearchParams(location.search), pre = q.get('sheet'), sel = document.getElementById('sheet');
  SHEETS.forEach(function(s){ var o = document.createElement('option'); o.value = s[0]; o.textContent = s[1]; sel.appendChild(o); });
  if (pre && SHEETS.some(function(s){ return s[0] === pre; })) { sel.value = pre; document.getElementById('title').textContent = SHEETS.find(function(s){ return s[0] === pre; })[1].replace(' · all chapters', ''); }
  var last = ls('sopaan-login'); if (last) document.getElementById('id').value = last;
  var err = document.getElementById('err'), go = document.getElementById('go');
  function fail(m){ err.textContent = m; err.classList.add('on'); go.disabled = false; go.textContent = 'Open sheet'; }
  document.getElementById('f').addEventListener('submit', function(e){
    e.preventDefault(); err.classList.remove('on');
    var id = document.getElementById('id').value.trim().toUpperCase(), pin = document.getElementById('pin').value.trim();
    if (!id || !pin) return fail('Please enter your login ID and PIN.');
    go.disabled = true; go.textContent = 'Opening…';
    fetch(API, {method: 'POST', body: JSON.stringify({action: 'start', login: id, pin: pin, sheet: sel.value, device: dev})})
      .then(function(r){ return r.json(); })
      .then(function(d){
        if (!d || !d.ok) return fail(d && d.msg ? d.msg : 'Could not sign in. Please try again.');
        ls('sopaan-login', id);
        var home = location.href.split('?')[0].split('#')[0];
        var lk = '<script>window.__SOPAAN_LOCK__=' + JSON.stringify({token: d.token, api: API, login: d.login, name: d.name, sheet: d.sheet, home: home}).replace(/</g, '\\u003c') + ';<\/script>';
        var html = d.html.replace(/<head>/i, '<head>' + lk);
        document.open(); document.write(html); document.close();
      })
      .catch(function(){ fail('Could not reach the server. Check your internet connection and try again.'); });
  });
})();
</script>
</body>
</html>
