<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>G@z Story Studio — V1</title>
<style>
:root{--bg:#0b0d12;--card:#131722;--card2:#191e2b;--text:#f5f7fb;--muted:#9ba5b5;--gold:#e8b84a;--line:#2a3140;--danger:#ef6b73}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--text);font-family:Inter,Arial,sans-serif}
header{padding:22px 18px;border-bottom:1px solid var(--line);background:#0d1017;position:sticky;top:0;z-index:5}
.brand{font-size:22px;font-weight:800}.brand span{color:var(--gold)}
.container{max-width:1100px;margin:auto;padding:20px}
.grid{display:grid;grid-template-columns:340px 1fr;gap:18px}
.card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:18px;margin-bottom:18px}
h2{font-size:17px;margin:0 0 14px}h3{font-size:15px;margin:0 0 8px}
label{display:block;color:var(--muted);font-size:13px;margin:13px 0 6px}
input,textarea,select{width:100%;background:#0e121a;color:var(--text);border:1px solid var(--line);border-radius:10px;padding:11px;font:inherit}
textarea{min-height:120px;resize:vertical}
button{border:0;border-radius:10px;padding:11px 14px;font-weight:700;cursor:pointer}
.primary{background:var(--gold);color:#111}.secondary{background:var(--card2);color:var(--text);border:1px solid var(--line)}
.danger{background:#351b20;color:#ffb5bb;border:1px solid #5a2a32}
.row{display:flex;gap:8px;flex-wrap:wrap}.row>*{flex:1}
.preview{width:100%;aspect-ratio:1/1;border-radius:12px;background:#0e121a;border:1px dashed var(--line);display:flex;align-items:center;justify-content:center;overflow:hidden;color:var(--muted);font-size:13px;text-align:center}
.preview img{width:100%;height:100%;object-fit:cover}
.actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px}
.status{padding:10px 12px;border-radius:10px;background:#10151e;color:var(--muted);font-size:13px;margin-top:12px}
.scene{background:#10141d;border:1px solid var(--line);border-radius:14px;padding:15px;margin:10px 0}
.scene-head{display:flex;justify-content:space-between;gap:10px;align-items:center}.badge{font-size:11px;color:#111;background:var(--gold);padding:4px 8px;border-radius:99px;font-weight:800}
.scene p{margin:8px 0;color:#c8ced9;font-size:13px;line-height:1.45}.mono{font-family:ui-monospace,SFMono-Regular,Menlo,monospace;font-size:12px;background:#0a0d12;padding:10px;border-radius:9px;color:#cbd4e2;white-space:pre-wrap}
.small{font-size:12px;color:var(--muted)}.hidden{display:none}
@media(max-width:800px){.grid{grid-template-columns:1fr}.container{padding:12px}header{position:static}}
</style>
</head>
<body>
<header><div class="container" style="padding:0"><div class="brand"><span>G@z</span> Story Studio <small class="small">V1</small></div></div></header>

<main class="container">
<div class="grid">
<section>
  <div class="card">
    <h2>👤 Personnage principal</h2>
    <label>Nom</label><input id="charName" value="Sami">
    <label>Apparence permanente</label>
    <textarea id="charDesc">Garçon de 10 ans, visage doux et expressif, cheveux noirs courts, silhouette mince. Il porte un t-shirt rouge simple, un pantalon bleu et des chaussures simples. Style animation 3D cinématique.</textarea>
    <label>Personnalité</label>
    <textarea id="charPersonality">Curieux, courageux, gentil, intelligent et légèrement réservé.</textarea>
    <label>Image de référence</label>
    <input id="charImage" type="file" accept="image/*">
    <div class="preview" id="preview">Aucune image sélectionnée</div>
  </div>

  <div class="card">
    <h2>🎬 Projet</h2>
    <label>Titre</label><input id="title" value="Sami — Épisode 1">
    <label>Style visuel</label>
    <select id="style">
      <option>Animation 3D cinématique</option>
      <option>Cartoon 3D</option>
      <option>Anime cinématique</option>
      <option>Réalisme cinématographique</option>
    </select>
    <label>Format</label>
    <select id="format"><option value="9:16">Vertical 9:16</option><option value="16:9">Paysage 16:9</option><option value="1:1">Carré 1:1</option></select>
    <label>Nombre de scènes</label>
    <input id="sceneCount" type="number" min="1" max="100" value="12">
    <label>Durée par scène (secondes)</label>
    <input id="sceneDuration" type="number" min="1" max="30" value="5">
  </div>
</section>

<section>
  <div class="card">
    <h2>📖 Histoire</h2>
    <textarea id="story" style="min-height:190px" placeholder="Écris ici l'idée ou l'histoire complète...">Sami travaille tranquillement dans son atelier lorsqu'un événement mystérieux attire son attention. Il décide de découvrir ce qui se passe et commence une aventure.</textarea>
    <div class="actions">
      <button class="primary" id="generate">✨ Générer le storyboard</button>
      <button class="secondary" id="save">💾 Sauvegarder le projet</button>
      <button class="secondary" id="export">⬇️ Exporter JSON</button>
      <button class="danger" id="clear">Effacer</button>
    </div>
    <div class="status" id="status">Prêt. La V1 génère localement le storyboard et les prompts cohérents.</div>
  </div>

  <div class="card">
    <div class="scene-head"><h2>🧠 Bible de cohérence</h2><span class="badge" id="sceneBadge">0 scènes</span></div>
    <div id="bible" class="mono">Génère un storyboard pour créer la bible.</div>
  </div>

  <div class="card">
    <div class="scene-head"><h2>🎞️ Storyboard</h2><span class="small" id="totalInfo"></span></div>
    <div id="scenes"><div class="small">Aucune scène générée.</div></div>
  </div>
</section>
</div>
</main>

<script>
const $ = id => document.getElementById(id);
let project = {scenes:[], imageData:null};

$("charImage").addEventListener("change", e=>{
  const file=e.target.files[0]; if(!file)return;
  const reader=new FileReader();
  reader.onload=()=>{project.imageData=reader.result;$("preview").innerHTML='<img alt="Référence personnage">';$("preview").querySelector("img").src=reader.result;};
  reader.readAsDataURL(file);
});

function getData(){
  return {
    title:$("title").value.trim() || "Projet sans titre",
    name:$("charName").value.trim() || "Personnage",
    appearance:$("charDesc").value.trim(),
    personality:$("charPersonality").value.trim(),
    style:$("style").value,
    format:$("format").value,
    count:Math.max(1,Math.min(100,Number($("sceneCount").value)||12)),
    duration:Math.max(1,Math.min(30,Number($("sceneDuration").value)||5)),
    story:$("story").value.trim()
  };
}

function buildBible(d){
  return `PERSONNAGE PRINCIPAL
Nom : ${d.name}
Apparence IMMUTABLE : ${d.appearance}
Personnalité : ${d.personality}

STYLE VISUEL
${d.style}
Format : ${d.format}

RÈGLES DE CONTINUITÉ
- Conserver exactement le même visage, âge, coiffure et morphologie.
- Conserver les vêtements et accessoires sauf changement explicitement prévu dans l'histoire.
- Conserver le même style visuel et le même niveau de détail.
- Respecter la chronologie et les événements déjà établis.
- Ne pas inventer un nouveau personnage à la place du personnage principal.
- Chaque scène doit être compatible avec la scène précédente.

Cette bible doit être ajoutée aux prompts de toutes les scènes.`;
}

function splitStory(story,count){
  // Cette V1 crée une progression de scènes à partir de l'idée générale.
  // Le backend/LLM remplacera ensuite cette logique par un vrai découpage narratif.
  const sentences=(story.match(/[^.!?]+[.!?]+/g)||[story]).map(s=>s.trim()).filter(Boolean);
  const beats=[
    "Introduction du lieu et de l'ambiance.",
    "Le personnage principal poursuit son activité.",
    "Un élément nouveau attire son attention.",
    "Le personnage observe et cherche à comprendre.",
    "Un obstacle ou une difficulté apparaît.",
    "Le personnage prend une décision.",
    "L'action commence et le rythme augmente.",
    "Une découverte importante change la situation.",
    "Le personnage fait face aux conséquences.",
    "Un moment de tension ou de surprise survient.",
    "Le personnage trouve une piste ou une solution.",
    "La situation évolue vers la suite de l'histoire."
  ];
  const arr=[];
  for(let i=0;i<count;i++){
    const source=sentences[i%sentences.length]||story;
    const beat=beats[i%beats.length];
    arr.push({n:i+1,beat,source});
  }
  return arr;
}

function makePrompt(d,scene,previous){
  return `${d.style}, cinematic storytelling, vertical ${d.format === "9:16" ? "9:16" : d.format},
same recurring character: ${d.name}. CHARACTER CONSISTENCY: ${d.appearance}.
Personality: ${d.personality}.
Story context: ${d.story}.
Scene ${scene.n}: ${scene.beat}
Narrative detail: ${scene.source}
Continuity from previous scene: ${previous || "opening scene; establish the character and location clearly"}.
Keep the character's face, age, hair, body proportions, clothing and visual identity unchanged.
Natural body movement, coherent environment, cinematic lighting, detailed composition, no text, no watermark.`;
}

function renderScenes(d,items){
  $("scenes").innerHTML="";
  project.scenes=[];
  items.forEach((x,i)=>{
    const prev=i?`Scene ${i}: ${items[i-1].beat}`:"Opening scene";
    const prompt=makePrompt(d,x,prev);
    const scene={...x,duration:d.duration,prompt};
    project.scenes.push(scene);
    const el=document.createElement("div");el.className="scene";
    el.innerHTML=`<div class="scene-head"><h3>Scène ${x.n}</h3><span class="badge">${d.duration}s</span></div>
      <p><b>Action :</b> ${escapeHtml(x.beat)}</p>
      <p><b>Base narrative :</b> ${escapeHtml(x.source)}</p>
      <div class="mono">${escapeHtml(prompt)}</div>`;
    $("scenes").appendChild(el);
  });
  $("sceneBadge").textContent=`${items.length} scènes`;
  $("totalInfo").textContent=`Durée théorique : ${items.length*d.duration}s`;
}

function escapeHtml(s){return String(s).replace(/[&<>"']/g,m=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"}[m]));}

$("generate").onclick=()=>{
  const d=getData();
  if(!d.story){$("status").textContent="Écris d'abord une histoire.";return;}
  $("bible").textContent=buildBible(d);
  const items=splitStory(d.story,d.count);
  renderScenes(d,items);
  $("status").textContent=`Storyboard généré : ${items.length} scènes. La génération vidéo sera branchée dans l'étape suivante.`;
};

$("save").onclick=()=>{
  const d=getData(); localStorage.setItem("gazStoryProject",JSON.stringify({...d,imageData:project.imageData,scenes:project.scenes}));
  $("status").textContent="Projet sauvegardé dans ce navigateur.";
};

$("export").onclick=()=>{
  const d=getData();
  const data={project:d,bible:buildBible(d),imageReference:project.imageData?"présente":"absente",scenes:project.scenes};
  const blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"});
  const a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download=(d.title||"gaz-story").replace(/[^a-z0-9_-]+/gi,"_")+".json";a.click();URL.revokeObjectURL(a.href);
};

$("clear").onclick=()=>{
  if(!confirm("Effacer le projet et les scènes ?"))return;
  $("scenes").innerHTML='<div class="small">Aucune scène générée.</div>';
  $("bible").textContent="Génère un storyboard pour créer la bible.";
  $("sceneBadge").textContent="0 scènes";$("totalInfo").textContent="";
  project.scenes=[];localStorage.removeItem("gazStoryProject");
  $("status").textContent="Projet effacé.";
};

(function load(){
  const raw=localStorage.getItem("gazStoryProject"); if(!raw)return;
  try{
    const p=JSON.parse(raw);
    ["title","name","appearance","personality","style","format","story"].forEach(k=>{if(p[k]!==undefined){
      const map={name:"charName",appearance:"charDesc",personality:"charPersonality"};
      $(map[k]||k).value=p[k];
    }});
    if(p.count)$("sceneCount").value=p.count;if(p.duration)$("sceneDuration").value=p.duration;
    if(p.imageData){project.imageData=p.imageData;$("preview").innerHTML='<img alt="Référence personnage">';$("preview").querySelector("img").src=p.imageData;}
    if(p.scenes?.length){project.scenes=p.scenes;$("bible").textContent=buildBible(getData());renderScenes(getData(),p.scenes);}
    $("status").textContent="Projet précédent chargé.";
  }catch(e){}
})();
</script>
</body>
</html>
