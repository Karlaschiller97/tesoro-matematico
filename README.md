# tesoro-matematico
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🗺️ La Búsqueda del Tesoro Matemático</title>
<link href="https://fonts.googleapis.com/css2?family=Pirata+One&family=Crimson+Pro:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #D4A017;
    --dark-gold: #8B6914;
    --parchment: #F5E6C8;
    --parchment-dark: #E8D5A3;
    --ink: #2C1810;
    --blood-red: #8B1A1A;
    --ocean: #1A3A5C;
    --wood: #5C3317;
    --correct: #2D6A4F;
    --wrong: #8B1A1A;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Crimson Pro', Georgia, serif;
    background: var(--ocean);
    min-height: 100vh;
    overflow-x: hidden;
    background-image:
      radial-gradient(ellipse at 20% 50%, rgba(26,58,92,0.8) 0%, transparent 60%),
      repeating-linear-gradient(45deg, transparent, transparent 30px, rgba(255,255,255,0.01) 30px, rgba(255,255,255,0.01) 31px);
  }
  .waves { position: fixed; bottom: 0; left: 0; right: 0; height: 80px; opacity: 0.15; pointer-events: none; z-index: 0; }
  .wave { position: absolute; bottom: 0; width: 200%; height: 60px; background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 1200 60'%3E%3Cpath d='M0 30 Q150 0 300 30 Q450 60 600 30 Q750 0 900 30 Q1050 60 1200 30 V60 H0Z' fill='%23fff'/%3E%3C/svg%3E") repeat-x; animation: wave 6s linear infinite; }
  .wave:nth-child(2) { animation: wave 9s linear infinite reverse; opacity: 0.5; bottom: 5px; }
  @keyframes wave { from { transform: translateX(0); } to { transform: translateX(-50%); } }
  .container { max-width: 900px; margin: 0 auto; padding: 20px; position: relative; z-index: 1; }
  .header { text-align: center; padding: 30px 20px 20px; animation: fadeDown 0.8s ease; }
  @keyframes fadeDown { from { opacity:0; transform: translateY(-30px);} to { opacity:1; transform: translateY(0);} }
  .title { font-family: 'Pirata One', cursive; font-size: clamp(2rem, 6vw, 3.5rem); color: var(--gold); text-shadow: 3px 3px 0 var(--dark-gold), 6px 6px 15px rgba(0,0,0,0.5); letter-spacing: 2px; line-height: 1.1; }
  .subtitle { color: var(--parchment); font-size: 1.1rem; margin-top: 8px; opacity: 0.8; font-style: italic; }
  .scorebar { display: flex; justify-content: center; gap: 30px; margin: 15px 0; flex-wrap: wrap; }
  .score-badge { background: rgba(212,160,23,0.15); border: 2px solid var(--gold); border-radius: 50px; padding: 8px 22px; color: var(--gold); font-family: 'Pirata One', cursive; font-size: 1.1rem; display: flex; align-items: center; gap: 8px; }
  .tabs { display: flex; justify-content: center; gap: 8px; margin: 20px 0 15px; flex-wrap: wrap; }
  .tab { font-family: 'Pirata One', cursive; padding: 10px 20px; border: 2px solid var(--dark-gold); border-radius: 8px 8px 0 0; cursor: pointer; font-size: 1rem; color: var(--parchment-dark); background: rgba(44,24,16,0.4); transition: all 0.3s; }
  .tab:hover { background: rgba(212,160,23,0.2); color: var(--gold); }
  .tab.active { background: var(--parchment); color: var(--ink); border-color: var(--gold); border-bottom-color: var(--parchment); }
  .parchment { background: var(--parchment); border-radius: 4px 12px 12px 4px; padding: 30px; position: relative; box-shadow: 5px 5px 25px rgba(0,0,0,0.5), inset 0 0 60px rgba(139,105,20,0.1); border-left: 8px solid var(--wood); animation: slideIn 0.4s ease; }
  @keyframes slideIn { from { opacity:0; transform: translateX(-20px);} to {opacity:1; transform:translateX(0);} }
  .parchment::before { content: ''; position: absolute; top: 0; right: 0; bottom: 0; width: 30px; background: linear-gradient(to right, transparent, rgba(139,105,20,0.08)); border-radius: 0 12px 12px 0; }
  .problems-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(380px, 1fr)); gap: 20px; margin-top: 10px; }
  .problem-card { background: rgba(255,255,255,0.6); border: 2px dashed var(--dark-gold); border-radius: 8px; padding: 18px; position: relative; transition: all 0.3s; animation: popIn 0.4s ease backwards; }
  .problem-card:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,0,0,0.15); }
  @keyframes popIn { from { opacity:0; transform: scale(0.95);} to {opacity:1; transform: scale(1);} }
  .problem-card.solved { border-color: var(--correct); background: rgba(45,106,79,0.1); }
  .problem-card.wrong { border-color: var(--wrong); background: rgba(139,26,26,0.1); animation: shake 0.4s ease; }
  @keyframes shake { 0%,100%{transform:translateX(0)} 20%{transform:translateX(-8px)} 40%{transform:translateX(8px)} 60%{transform:translateX(-4px)} 80%{transform:translateX(4px)} }
  .pnum { font-family: 'Pirata One', cursive; font-size: 0.8rem; color: var(--blood-red); letter-spacing: 1px; margin-bottom: 6px; display: flex; align-items: center; gap: 6px; }
  .problem-text { font-size: 1.2rem; color: var(--ink); font-weight: 600; margin-bottom: 10px; line-height: 1.4; }
  .hint-text { font-size: 0.88rem; color: var(--wood); font-style: italic; margin-bottom: 12px; line-height: 1.4; padding-left: 10px; border-left: 3px solid var(--dark-gold); }
  .answer-row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
  .answer-input { flex: 1; min-width: 80px; padding: 8px 12px; border: 2px solid var(--dark-gold); border-radius: 6px; font-family: 'Crimson Pro', serif; font-size: 1.1rem; color: var(--ink); background: rgba(255,255,255,0.8); transition: border-color 0.2s; outline: none; }
  .answer-input:focus { border-color: var(--ocean); }
  .btn-check { padding: 8px 18px; background: var(--ocean); color: var(--gold); border: 2px solid var(--dark-gold); border-radius: 6px; cursor: pointer; font-family: 'Pirata One', cursive; font-size: 1rem; transition: all 0.2s; white-space: nowrap; }
  .btn-check:hover { background: var(--dark-gold); color: white; transform: scale(1.05); }
  .btn-check:disabled { opacity: 0.5; cursor: default; }
  .feedback { margin-top: 8px; font-size: 0.95rem; font-weight: 600; min-height: 22px; }
  .feedback.ok { color: var(--correct); }
  .feedback.err { color: var(--wrong); }
  .solution-box { margin-top: 10px; background: rgba(44,24,16,0.06); border-radius: 6px; padding: 10px 14px; font-size: 0.9rem; color: var(--ink); line-height: 1.6; display: none; }
  .solution-box.visible { display: block; }
  .solution-box strong { color: var(--blood-red); }
  .pemdas-legend { display: flex; justify-content: center; gap: 8px; flex-wrap: wrap; margin-bottom: 20px; }
  .pemdas-badge { padding: 4px 14px; border-radius: 20px; font-family: 'Pirata One', cursive; font-size: 1rem; color: white; }
  .p1{background:#8B1A1A;} .p2{background:#5C3317;} .p3{background:#1A3A5C;} .p4{background:#2D6A4F;} .p5{background:#6B4C9A;} .p6{background:#A0522D;}
  .section-title { font-family: 'Pirata One', cursive; font-size: 1.4rem; color: var(--ink); margin-bottom: 16px; display: flex; align-items: center; gap: 10px; border-bottom: 3px double var(--dark-gold); padding-bottom: 8px; }
  .progress-bar-wrap { background: rgba(139,105,20,0.2); border-radius: 20px; height: 10px; margin: 12px 0; overflow: hidden; }
  .progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--dark-gold), var(--gold)); border-radius: 20px; transition: width 0.6s ease; }
  .treasure-msg { text-align: center; padding: 30px; display: none; }
  .treasure-msg.show { display: block; animation: fadeDown 0.8s ease; }
  .treasure-msg .big { font-family: 'Pirata One', cursive; font-size: 3rem; color: var(--gold); text-shadow: 3px 3px 0 var(--dark-gold); }
  .tab-content { display: none; }
  .tab-content.active { display: block; }
  @media(max-width:600px) { .problems-grid { grid-template-columns: 1fr; } .parchment { padding: 18px 14px; } }
</style>
</head>
<body>
<div class="waves"><div class="wave"></div><div class="wave"></div></div>
<div class="container">
  <div class="header">
    <div class="title">☠️ La Búsqueda del Tesoro ☠️</div>
    <div class="subtitle">Resuelve los acertijos matemáticos · Sigue la orden de operaciones PEMDAS · ¡Halla el tesoro!</div>
  </div>
  <div class="scorebar">
    <div class="score-badge"><span>⚓</span> Resueltos: <strong id="solved-count">0</strong></div>
    <div class="score-badge"><span>💰</span> Monedas: <strong id="coins">0</strong></div>
    <div class="score-badge"><span>🗺️</span> Progreso: <strong id="prog-pct">0%</strong></div>
  </div>
  <div class="progress-bar-wrap"><div class="progress-bar-fill" id="prog-bar" style="width:0%"></div></div>
  <div class="pemdas-legend">
    <span class="pemdas-badge p1">P – Paréntesis</span>
    <span class="pemdas-badge p2">E – Exponentes</span>
    <span class="pemdas-badge p3">M – Multiplicación</span>
    <span class="pemdas-badge p4">D – División</span>
    <span class="pemdas-badge p5">A – Adición</span>
    <span class="pemdas-badge p6">S – Sustracción</span>
  </div>
  <div class="tabs">
    <button class="tab active" onclick="showTab('numericos',this)">🔢 Numéricos</button>
    <button class="tab" onclick="showTab('letras',this)">🔡 Con Letras</button>
    <button class="tab" onclick="showTab('desafio',this)">💀 Desafío Final</button>
  </div>
  <div id="tab-numericos" class="tab-content active">
    <div class="parchment">
      <div class="section-title">⚓ Pista 1 — Enteros y PEMDAS</div>
      <div class="problems-grid" id="grid-numericos"></div>
    </div>
  </div>
  <div id="tab-letras" class="tab-content">
    <div class="parchment">
      <div class="section-title">🗡️ Pista 2 — Álgebra del Pirata</div>
      <div class="problems-grid" id="grid-letras"></div>
    </div>
  </div>
  <div id="tab-desafio" class="tab-content">
    <div class="parchment">
      <div class="section-title">💀 Pista Final — El Cofre del Capitán</div>
      <div class="problems-grid" id="grid-desafio"></div>
      <div class="treasure-msg" id="treasure-msg">
        <div class="big">🏆 ¡TESORO HALLADO! 🏆</div>
        <p style="font-size:1.3rem;color:var(--ink);margin-top:10px;">¡Eres el mejor pirata matemático del Mar PEMDAS!</p>
      </div>
    </div>
  </div>
</div>
<script>
const ANSWERS = {
  N1:11,N2:24,N3:4,N4:-4,N5:-8,N6:12,N7:-44,N8:27,N9:12,N10:14,N11:-29,N12:20,
  L1:11,L2:0,L3:8,L4:6,L5:6,L6:6,L7:11,L8:9,L9:2,L10:21,
  D2:1,D3:-27,D4:14,D5:76
};
const PROBLEMS = {
  numericos:[
    {id:'N1',diff:1,text:'3 + 4 × 2 = ?',hint:'La multiplicación va ANTES que la suma.',solution:'4×2=8 → 3+8=<strong>11</strong>'},
    {id:'N2',diff:1,text:'(6 + 2) × 3 = ?',hint:'Los paréntesis mandan primero.',solution:'(6+2)=8 → 8×3=<strong>24</strong>'},
    {id:'N3',diff:1,text:'20 ÷ 4 − 1 = ?',hint:'División antes que sustracción.',solution:'20÷4=5 → 5−1=<strong>4</strong>'},
    {id:'N4',diff:2,text:'2³ + 4 × (−3) = ?',hint:'Primero el exponente, luego multiplica, finalmente suma.',solution:'2³=8 → 4×(−3)=−12 → 8+(−12)=<strong>−4</strong>'},
    {id:'N5',diff:2,text:'(−5 + 3) × (−2)² = ?',hint:'Paréntesis primero, luego exponentes.',solution:'(−5+3)=−2 → (−2)²=4 → −2×4=<strong>−8</strong>'},
    {id:'N6',diff:2,text:'15 − 3 × (2 + 1)² ÷ 9 = ?',hint:'P→E→M/D→A/S paso a paso.',solution:'(2+1)=3 → 3²=9 → 3×9=27 → 27÷9=3 → 15−3=<strong>12</strong>'},
    {id:'N7',diff:3,text:'−4 × (−3)² − (10 ÷ 2 + 3) = ?',hint:'Cuidado con los signos negativos.',solution:'(−3)²=9 → −4×9=−36 → (10÷2+3)=8 → −36−8=<strong>−44</strong>'},
    {id:'N8',diff:2,text:'(8 − 12) ÷ (−2) + 5² = ?',hint:'Paréntesis primero.',solution:'(8−12)=−4 → −4÷(−2)=2 → 5²=25 → 2+25=<strong>27</strong>'},
    {id:'N9',diff:1,text:'6 + 2 × 4 − 10 ÷ 5 = ?',hint:'M y D antes que A y S.',solution:'2×4=8 → 10÷5=2 → 6+8−2=<strong>12</strong>'},
    {id:'N10',diff:3,text:'3 × [2 + (5 − 7)²] − 4 = ?',hint:'Trabaja de adentro hacia afuera.',solution:'(5−7)=−2 → (−2)²=4 → [2+4]=6 → 3×6=18 → 18−4=<strong>14</strong>'},
    {id:'N11',diff:2,text:'−(3 + 2)² + 4 × (−1)³ = ?',hint:'El signo negativo afuera actúa al final.',solution:'(3+2)=5 → 5²=25 → −25 → (−1)³=−1 → 4×(−1)=−4 → −25+(−4)=<strong>−29</strong>'},
    {id:'N12',diff:3,text:'(−2 + 6)³ ÷ 8 − 3 × (−4) = ?',hint:'¡Sigue el mapa: P, E, M, D, A, S!',solution:'(−2+6)=4 → 4³=64 → 64÷8=8 → 3×(−4)=−12 → 8−(−12)=<strong>20</strong>'},
  ],
  letras:[
    {id:'L1',diff:1,text:'Si x = 3, halla: 2x + 5',hint:'Sustituye x por 3.',solution:'2(3)+5=6+5=<strong>11</strong>'},
    {id:'L2',diff:1,text:'Si a = −2, halla: a² − 4',hint:'Eleva al cuadrado primero.',solution:'(−2)²=4 → 4−4=<strong>0</strong>'},
    {id:'L3',diff:2,text:'Si x = 4, halla: 3(x − 2)² − x',hint:'Paréntesis → Exponente → Multiplica → Resta.',solution:'(4−2)=2 → 2²=4 → 3×4=12 → 12−4=<strong>8</strong>'},
    {id:'L4',diff:1,text:'Simplifica: 5x + 3x − 2x (coeficiente)',hint:'Combina términos semejantes.',solution:'(5+3−2)x=<strong>6x</strong>'},
    {id:'L5',diff:2,text:'Si n = −1, halla: 4n³ + 2n² − n + 7',hint:'Sigue PEMDAS con cada término.',solution:'4(−1)+2(1)−(−1)+7=−4+2+1+7=<strong>6</strong>'},
    {id:'L6',diff:2,text:'Si x=2, y=3, halla: 2x²y − xy²',hint:'Sustituye y aplica exponentes antes de multiplicar.',solution:'2(4)(3)=24 → (2)(9)=18 → 24−18=<strong>6</strong>'},
    {id:'L7',diff:3,text:'Si a=−3, b=2, halla: (a+b)³ − a·b²',hint:'Paréntesis primero, luego exponentes.',solution:'(−3+2)=−1 → (−1)³=−1 → b²=4 → −(−3)(4)=12 → −1+12=<strong>11</strong>'},
    {id:'L8',diff:2,text:'Si t = 5, halla: t² − 3(t + 1) + 2',hint:'Usa paréntesis antes de multiplicar.',solution:'25−3(6)+2=25−18+2=<strong>9</strong>'},
    {id:'L9',diff:1,text:'Simplifica: 4(x + 3) − 2x (coeficiente de x)',hint:'Distribuye primero.',solution:'4x+12−2x=2x+12 → coeficiente=<strong>2</strong>'},
    {id:'L10',diff:3,text:'Si x=−2, halla: −x³ + (x−1)² × 2 − 5',hint:'Cuidado con el signo negativo delante del cubo.',solution:'−(−8)=8 → (−3)²=9 → 9×2=18 → 8+18−5=<strong>21</strong>'},
  ],
  desafio:[
    {id:'D2',diff:3,text:'−2³ + (−3)³ ÷ 9 × (4 − 7) = ?',hint:'−2³ ≠ (−2)³. ¡El exponente solo aplica al 2!',solution:'−8 → (−27)÷9=−3 → −3×(−3)=9 → −8+9=<strong>1</strong>'},
    {id:'D3',diff:3,text:'Si a=3, b=−1: a²b³ − (a−b)² + 2b = ?',hint:'Sustituye con cuidado los signos.',solution:'9×(−1)=−9 → (3+1)²=16 → 2(−1)=−2 → −9−16−2=<strong>−27</strong>'},
    {id:'D4',diff:3,text:'4 × {3 − [2(5 − 8)² − 1]} ÷ (−4) = ?',hint:'Trabaja de adentro hacia afuera: (), [], {}.',solution:'(5−8)=−3 → 9×2=18 → 18−1=17 → 3−17=−14 → 4×(−14)=−56 → −56÷(−4)=<strong>14</strong>'},
    {id:'D5',diff:3,text:'Si x=−4: x⁴ ÷ (x+2)² − 3x = ?',hint:'Cuidado: x⁴ con base negativa.',solution:'(−4)⁴=256 → (−2)²=4 → 256÷4=64 → −3(−4)=12 → 64+12=<strong>76</strong>'},
  ]
};

let solvedSet=new Set(), coins=0, totalProblems=0;

function stars(n){ return '⚔️'.repeat(n)+'🗡️'.repeat(3-n); }

function buildGrid(problems, containerId){
  const container=document.getElementById(containerId);
  container.innerHTML='';
  problems.forEach((p,i)=>{
    const correct=ANSWERS[p.id]??p.answer;
    totalProblems++;
    const card=document.createElement('div');
    card.className='problem-card';
    card.id='card-'+p.id;
    card.style.animationDelay=(i*0.07)+'s';
    card.innerHTML=`
      <div class="pnum"><span>Pista #${p.id}</span><span class="difficulty">${stars(p.diff)}</span></div>
      <div class="problem-text">${p.text}</div>
      <div class="hint-text">🧭 ${p.hint}</div>
      <div class="answer-row">
        <input class="answer-input" type="number" id="inp-${p.id}" placeholder="Tu respuesta..."/>
        <button class="btn-check" onclick="check('${p.id}',${correct})">⚓ Verificar</button>
        <button class="btn-check" style="background:var(--wood);border-color:var(--wood);font-size:0.85rem;" onclick="showSol('${p.id}')">🗺️ Ver</button>
      </div>
      <div class="feedback" id="fb-${p.id}"></div>
      <div class="solution-box" id="sol-${p.id}">🗺️ Solución: ${p.solution}</div>`;
    container.appendChild(card);
  });
}

function check(id,correct){
  const inp=document.getElementById('inp-'+id);
  const fb=document.getElementById('fb-'+id);
  const card=document.getElementById('card-'+id);
  const val=parseFloat(inp.value);
  if(isNaN(val)){fb.textContent='⚠️ Escribe un número';fb.className='feedback err';return;}
  if(Math.abs(val-correct)<0.6){
    fb.innerHTML='✅ ¡Correcto! +'+(10*getDiff(id))+' monedas';
    fb.className='feedback ok';
    card.className='problem-card solved';
    inp.disabled=true;
    card.querySelector('.btn-check').disabled=true;
    if(!solvedSet.has(id)){solvedSet.add(id);coins+=10*getDiff(id);updateScore();}
    showSol(id);
  } else {
    fb.innerHTML='❌ Intenta de nuevo. Recuerda PEMDAS.';
    fb.className='feedback err';
    card.classList.remove('wrong');
    void card.offsetWidth;
    card.classList.add('wrong');
    setTimeout(()=>card.classList.remove('wrong'),500);
  }
}

function getDiff(id){
  const all=[...PROBLEMS.numericos,...PROBLEMS.letras,...PROBLEMS.desafio];
  const p=all.find(x=>x.id===id);
  return p?p.diff:1;
}

function showSol(id){ document.getElementById('sol-'+id).classList.add('visible'); }

function updateScore(){
  const n=solvedSet.size;
  document.getElementById('solved-count').textContent=n;
  document.getElementById('coins').textContent=coins;
  const pct=Math.round((n/totalProblems)*100);
  document.getElementById('prog-pct').textContent=pct+'%';
  document.getElementById('prog-bar').style.width=pct+'%';
  if(pct>=80) document.getElementById('treasure-msg').classList.add('show');
}

function showTab(name,btn){
  document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('tab-'+name).classList.add('active');
  btn.classList.add('active');
}

buildGrid(PROBLEMS.numericos,'grid-numericos');
buildGrid(PROBLEMS.letras,'grid-letras');
buildGrid(PROBLEMS.desafio,'grid-desafio');
</script>
</body>
</html>
