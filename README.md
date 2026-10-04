<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>G@z Story Studio V3</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#090909;color:#f5f5f5}
header{padding:22px 18px;border-bottom:1px solid #252525;background:#0d0d0d}
.logo{font-size:24px;font-weight:800;color:#f5c451}
.sub{margin-top:5px;color:#aaa;font-size:13px}
main{max-width:1050px;margin:auto;padding:22px}
.card{background:#111;border:1px solid #282828;border-radius:18px;padding:20px;margin-bottom:18px}
h2{margin:0 0 16px;font-size:19px}
label{display:block;margin:14px 0 7px;color:#ccc;font-size:14px}
input,textarea,select{width:100%;border:1px solid #333;border-radius:12px;background:#181818;color:#fff;padding:13px;font-size:15px;outline:none}
textarea{min-height:210px;resize:vertical}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:14px}
button{border:0;border-radius:12px;padding:14px 18px;font-weight:700;cursor:pointer;font-size:15px}
.primary{background:#f5c451;color:#111;width:100%;margin-top:18px}
.secondary{background:#222;color:#fff;border:1px solid #383838}
button:hover{opacity:.9}
.hidden{display:none}
.status{margin-top:15px;padding:13px;border-radius:12px;background:#171717;color:#bbb}
.tags{display:flex;flex-wrap:wrap;gap:8px}
.tag{padding:8px 11px;background:#1c1c1c;border:1px solid #333;border-radius:20px;color:#ddd;font-size:13px}
.scene{border:1px solid #303030;background:#161616;border-radius:14px;padding:16px;margin-top:12px}
.scene h3{margin:0 0 8px;color:#f5c451}
.scene p{margin:6px 0;color:#bbb;line-height:1.5}
.badge{display:inline-block;font-size:12px;padding:5px 8px;border-radius:8px;background:#242424;color:#ccc;margin-right:6px}
.small{font-size:12px;color:#777}
footer{text-align:center;color:#666;padding:25px;font-size:12px}
@media(max-width:700px){main{padding:14px}.grid{grid-template-columns:1fr}.card{padding:16px}}
</style>
</head>
<body>

<header>
  <div class="logo">G@z Story Studio <span style="font-size:13px;color:#888">V3</span></div>
  <div class="sub">Transformez n'importe quelle histoire en projet cinématographique IA.</div>
</header>

<main>

<section class="card">
  <h2>🎬 Nouveau projet</h2>

  <label for="title">Titre du projet</label>
  <input id="title" placeholder="Ex. Le secret de la forêt">

  <label for="story">📝 Votre histoire</label>
  <textarea id="story" placeholder="Écrivez ou collez votre histoire ici...&#10;&#10;G@z Story Studio analysera ensuite les personnages, lieux, événements, dialogues et scènes."></textarea>

  <div class="grid">
    <div>
      <label for="duration">⏱️ Durée souhaitée</label>
      <select id="duration">
        <option>1 minute</option>
        <option>5 minutes</option>
        <option>10 minutes</option>
        <option>30 minutes</option>
        <option>Personnalisée</option>
      </select>
    </div>
    <div>
      <label for="style">🎨 Style visuel</label>
      <select id="style">
        <option>Cinématique réaliste</option>
        <option>Animation 3D</option>
        <option>Anime</option>
        <option>Cartoon</option>
        <option>Fantastique</option>
        <option>Personnalisé</option>
      </select>
    </div>
    <div>
      <label for="format">📱 Format</label>
      <select id="format">
        <option>16:9 — YouTube / cinéma</option>
        <option>9:16 — TikTok / Shorts</option>
        <option>1:1 — Carré</option>
      </select>
    </div>
    <div>
      <label for="language">🗣️ Langue</label>
      <select id="language">
        <option>Français</option>
        <option>English</option>
        <option>العربية</option>
      </select>
    </div>
  </div>

  <button class="primary" onclick="analyzeStory()">✨ ANALYSER MON HISTOIRE</button>
  <div id="status" class="status hidden"></div>
</section>

<section id="analysis" class="card hidden">
  <h2>🧠 Analyse de l'histoire</h2>

  <label>👤 Personnages détectés</label>
  <div id="characters" class="tags"></div>

  <label>🌍 Lieux</label>
  <div id="locations" class="tags"></div>

  <label>🎭 Ambiances</label>
  <div id="moods" class="tags"></div>

  <label>⚡ Événements importants</label>
  <div id="events" class="tags"></div>
</section>

<section id="storyboard" class="card hidden">
  <h2>🎞️ Storyboard automatique</h2>
  <p class="small">Chaque scène pourra ensuite être développée en plans, images, vidéos, voix et musique.</p>
  <div id="scenes"></div>
</section>

<section id="next" class="card hidden">
  <h2>🚀 Prochaine étape</h2>
  <p style="color:#bbb;line-height:1.6">
    La structure de ton projet est prête. Dans la prochaine version, nous connecterons
    l'analyse à une véritable IA et commencerons la préparation automatique des prompts
    pour les images et les vidéos.
  </p>
  <button class="secondary" onclick="window.scrollTo({top:0,behavior:'smooth'})">← Modifier le projet</button>
</section>

</main>

<footer>G@z Story Studio — V3 · Création cinématographique assistée par IA</footer>

<script>
function wordsToTags(text, fallback){
  const clean=text.replace(/[.,!?;:()[\]"']/g,' ').trim();
  if(!clean) return fallback;
  const words=clean.split(/\s+/).filter(w=>w.length>3);
  const unique=[];
  for(const w of words){
    const x=w.toLowerCase();
    if(!unique.includes(x)) unique.push(x);
    if(unique.length>=6) break;
  }
  return unique.length ? unique : fallback;
}

function renderTags(id, items){
  document.getElementById(id).innerHTML=items.map(x=>`<span class="tag">${x}</span>`).join('');
}

function analyzeStory(){
  const story=document.getElementById('story').value.trim();
  const title=document.getElementById('title').value.trim() || 'Mon histoire';
  const status=document.getElementById('status');

  if(!story){
    status.classList.remove('hidden');
    status.textContent='⚠️ Écris ou colle d’abord ton histoire.';
    return;
  }

  status.classList.remove('hidden');
  status.textContent='⏳ Analyse du scénario en cours...';

  setTimeout(()=>{
    const people=wordsToTags(story,['Personnage principal','Personnage secondaire']);
    const places=wordsToTags(story,['Lieu principal','Lieu secondaire']);
    const moods=['Cinématique','Dramatique','Émotionnelle'];
    const events=['Début de l’histoire','Événement déclencheur','Développement','Moment important'];

    renderTags('characters',people);
    renderTags('locations',places);
    renderTags('moods',moods);
    renderTags('events',events);

    const sceneTexts=[
      ['Scène 1 — Introduction','Présentation de l’univers, du personnage principal et du contexte.'],
      ['Scène 2 — Événement déclencheur','Un événement vient modifier la situation initiale et lance l’histoire.'],
      ['Scène 3 — Développement','Le personnage avance dans son objectif et rencontre de nouveaux obstacles.'],
      ['Scène 4 — Moment clé','L’histoire atteint un moment important qui fait progresser fortement le récit.'],
      ['Scène 5 — Conclusion','Résolution de l’histoire et conclusion cinématographique.']
    ];

    document.getElementById('scenes').innerHTML=sceneTexts.map((s,i)=>`
      <div class="scene">
        <h3>${s[0]}</h3>
        <span class="badge">Scène ${i+1}</span>
        <span class="badge">${document.getElementById('duration').value}</span>
        <p>${s[1]}</p>
        <p class="small">Personnages : à déterminer par l'IA · Lieu : à déterminer · Plans : à générer</p>
      </div>
    `).join('');

    document.getElementById('analysis').classList.remove('hidden');
    document.getElementById('storyboard').classList.remove('hidden');
    document.getElementById('next').classList.remove('hidden');
    status.textContent='✅ Analyse préparatoire terminée. Le storyboard a été créé.';
    document.getElementById('analysis').scrollIntoView({behavior:'smooth'});
  },900);
}
</script>

</body>
</html>
