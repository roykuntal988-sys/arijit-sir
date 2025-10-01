<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Social Science Hub (Class 8–12)</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');
:root{
--accent:#f59e0b;--accent2:#ef4444;--bg:#0f172a;--card:#1e293b;--muted:#94a3b8;--text:#f1f5f9
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Poppins',sans-serif;background:var(--bg);color:var(--text);overflow-x:hidden}
header{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff;padding:20px 24px;position:sticky;top:0;z-index:1000;box-shadow:0 4px 20px rgba(0,0,0,0.4)}
.container{max-width:1200px;margin:0 auto;padding:0 16px}
.brand{font-weight:700;font-size:24px;letter-spacing:1px}
.hero{margin-top:20px;background:linear-gradient(135deg,rgba(245,158,11,0.15),rgba(239,68,68,0.15));padding:40px;border-radius:20px;display:flex;gap:20px;align-items:center;backdrop-filter:blur(8px)}
.hero h1{margin:0;font-size:36px;font-weight:700;line-height:1.2}
.hero p{margin:10px 0 0;color:var(--muted);font-size:18px}
.search{margin-left:auto}
input[type=search]{padding:12px 16px;border-radius:12px;border:1px solid #334155;background:#0f172a;color:var(--text);min-width:250px;transition:0.3s}
input[type=search]:focus{border-color:var(--accent);outline:none;box-shadow:0 0 8px var(--accent)}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:25px;margin-top:30px}
.card{background:var(--card);padding:28px;border-radius:20px;box-shadow:0 8px 30px rgba(0,0,0,0.4);transition:transform 0.3s,box-shadow 0.3s;position:relative;overflow:hidden}
.card:hover{transform:translateY(-6px) scale(1.03);box-shadow:0 12px 40px rgba(0,0,0,0.6)}
.card h3{margin:0 0 14px;font-size:24px;font-weight:700}
.badge{display:inline-block;padding:6px 12px;border-radius:999px;font-size:12px;background:var(--accent);color:#fff;font-weight:600}
.subject-list{list-style:none;padding:0;margin:14px 0 0}
.subject-list li{padding:12px 16px;border-radius:12px;margin-bottom:12px;background:#0f172a;border:1px solid #334155;display:flex;flex-direction:column;gap:6px;color:var(--muted)}
.class-switch{background:transparent;color:var(--accent);border:1px solid var(--accent);padding:6px 12px;border-radius:8px;cursor:pointer;transition:0.3s;font-size:14px}
.class-switch.active{background:var(--accent2);color:#fff;border:none}
.class-switch:hover{background:var(--accent);color:#fff}
button{background:var(--accent);color:#fff;border:0;padding:10px 18px;border-radius:12px;cursor:pointer;font-weight:600;transition:0.3s;font-size:14px}
button:hover{background:var(--accent2)}
button.secondary{background:transparent;color:var(--accent);border:1px solid var(--accent)}
button.secondary:hover{background:var(--accent);color:#fff}
footer{margin:40px 0 60px;color:var(--muted);text-align:center;font-size:14px}
.modal{position:fixed;inset:0;background:rgba(0,0,0,0.7);display:flex;align-items:center;justify-content:center;padding:16px;visibility:hidden;opacity:0;transition:all .3s ease;z-index:9999}
.modal.open{visibility:visible;opacity:1}
.modal-card{background:#1e293b;padding:28px;border-radius:16px;max-width:760px;width:100%;box-shadow:0 18px 60px rgba(0,0,0,0.6)}
.modal-card h3{margin-bottom:14px}
.modal-card button{margin-top:12px}
</style>
</head>
<body>
<header>
<div class="container" style="display:flex;align-items:center;gap:16px;flex-wrap:wrap">
<div style="display:flex;flex-direction:column">
<div class="brand">Social Science Hub</div>
<div style="font-size:13px;color:rgba(255,255,255,0.9)">Owner: <strong>Arijit Sir</strong> • Developer: <strong>Kuntal Roy</strong></div>
</div>
</div>
</header>
<main class="container">
<section class="hero">
<div>
<h1>🌍 Social Science Learning Hub</h1>
<p>Explore History, Geography, Civics, and Economics with notes, past papers, and quizzes.</p>
</div>
<div class="search">
<input id="searchInput" type="search" placeholder="🔍 Search subjects" oninput="filterSubjects()" />
</div>
</section>
<section style="margin-top:24px">
<div style="display:flex;gap:12px;align-items:center;">
<h2 style="margin:0;font-size:24px">Subjects</h2>
<span class="badge">Class 8–12</span>
</div>
<div class="grid" id="subjectsGrid"></div>
</section>
</main>
<footer>© <span id="year"></span> Social Science Hub — Learn for the Future 🚀</footer>
<div id="modal" class="modal" onclick="if(event.target.id==='modal') closeModal()">
<div class="modal-card" id="modalCard"></div>
</div>
<script>
// Subjects
const subjects=[
{ id:'his',name:'History',classes:['8','9','10','11','12'],desc:'World history, national history and civics context.'},
{ id:'geo',name:'Geography',classes:['8','9','10','11','12'],desc:'Maps, environment, population and resources.'},
{ id:'civ',name:'Civics',classes:['8','9','10','11','12'],desc:'Citizenship, government, rights, responsibilities.'},
{ id:'eco',name:'Economics',classes:['9','10','11','12'],desc:'Microeconomics, macroeconomics, development studies.'}
];

// Notes
const notesData={
civ:{'8':[{"question":"What is marginalisation?","answer":"Being pushed to the margins of society, denied basic services and rights."}]}};

// Quizzes
const quizData={
civ:{'8':[{"question":"Marginalisation affects which groups?","options":["Majority","Minority","Both"],"answer":2}]}};

document.getElementById('year').textContent=new Date().getFullYear();
const subjectsGrid=document.getElementById('subjectsGrid');

// Render subjects
function renderSubjects(list){
subjectsGrid.innerHTML='';
list.forEach(s=>{
const el=document.createElement('div');el.className='card';
let classButtons=s.classes.map(cls=>`<button class="class-switch" data-subject="${s.id}" data-class="${cls}">${cls}</button>`).join('');
el.innerHTML=`<h3>${s.name} <span style="font-size:12px;color:var(--muted)">(Class ${s.classes.join(',')})</span></h3>
<p style="color:var(--muted);margin:6px 0">${s.desc}</p>
<ul class="subject-list">
<li>Notes <div style="display:flex;gap:5px;">${classButtons}</div></li>
<li>Test <button class="test-btn" data-subject="${s.id}" data-class="${s.classes[0]}">Start Test</button></li>
</ul>`;
subjectsGrid.appendChild(el);
});
attachClassEvents();
attachTestEvents();
}

// Class buttons
function attachClassEvents(){
document.querySelectorAll('.class-switch').forEach(btn=>{
btn.addEventListener('click',function(){
const subject=this.dataset.subject;
const cls=this.dataset.class;
document.querySelectorAll(`.class-switch[data-subject="${subject}"]`).forEach(b=>b.classList.remove('active'));
this.classList.add('active');
openModal('notes',subject,cls);
});
});
}

// Test buttons
function attachTestEvents(){
document.querySelectorAll('.test-btn').forEach(btn=>{
btn.addEventListener('click',function(){
const subject=this.dataset.subject;
const cls=this.dataset.class;
openModal('quiz',subject,cls);
});
});
}

// Filter
function filterSubjects(){
const q=document.getElementById('searchInput').value.toLowerCase().trim();
renderSubjects(subjects.filter(s=>s.name.toLowerCase().includes(q)||s.desc.toLowerCase().includes(q)));
}

// Modal open
function openModal(type,id,cls){
const modal=document.getElementById('modal');
const card=document.getElementById('modalCard');
modal.classList.add('open');
let content='';
if(type==='notes'){
const data=notesData[id]&&notesData[id][cls];
if(data){let qaHTML=data.map(q=>`<li><strong>Q:</strong> ${q.question}<br><strong>A:</strong> ${q.answer}</li>`).join('');content=`<h3>${id.toUpperCase()} — Class ${cls} Notes</h3><ul>${qaHTML}</ul><button onclick="closeModal()">Close</button>`;}
else{content=`<h3>${id.toUpperCase()} — Class ${cls} Notes</h3><p>No notes available yet.</p><button onclick="closeModal()">Close</button>`;}
}
if(type==='quiz'){
const quiz=quizData[id]&&quizData[id][cls];
if(quiz){let qHTML=quiz.map((q,i)=>`<p><strong>${i+1}. ${q.question}</strong></p>${q.options.map((o,j)=>`<button onclick="alert('${o} selected')">${o}</button>`).join(' ')}`).join('<hr>');content=`<h3>${id.toUpperCase()} — Class ${cls} Test</h3>${qHTML}<button onclick="closeModal()">Close</button>`;}
else{content=`<h3>${id.toUpperCase()} — Class ${cls} Test</h3><p>No quiz available yet.</p><button onclick="closeModal()">Close</button>`;}
}
card.innerHTML=content;
}

function closeModal(){document.getElementById('modal').classList.remove('open');}

renderSubjects(subjects);
</script>
</body>
</html>
