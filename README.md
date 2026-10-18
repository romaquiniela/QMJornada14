# QMJornada14
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#b8860b">
<title>QUINIELAS ROMA — JORNADA 14</title>
<style>
:root{color-scheme:dark;--gold:#ffd700;--gold2:#b8860b}
*{box-sizing:border-box}
html,body{margin:0;min-height:100%;width:100%}
body{font-family:Arial,Helvetica,sans-serif;background:#050505;color:#fff;padding:10px;max-width:560px;margin:auto}
header{display:flex;align-items:center;justify-content:center;gap:9px;text-align:center;padding:12px 7px;background:linear-gradient(100deg,#b8860b,#ffe36a,#b8860b);border-radius:12px;color:#000}
.ball{font-size:24px}h1{font-size:clamp(18px,5vw,23px);line-height:1.05;margin:0;font-weight:900}.sub{font-weight:800;margin-top:5px}.price{font-size:17px;font-weight:900;color:#000}.small{font-size:10px;font-weight:bold}
.name{display:block;width:100%;padding:12px 9px;font-size:18px;font-weight:bold;text-align:center;border:2px solid var(--gold);border-radius:9px;background:#fff;color:#111;margin:10px 0}
.note{text-align:center;color:var(--gold);font-size:12px;margin:7px 0}
.match{display:grid;grid-template-columns:minmax(0,1fr) auto minmax(0,1fr);align-items:center;gap:5px;background:#191919;border:1px solid #444;border-radius:9px;padding:8px 5px;margin:6px 0;min-height:60px}
.team{display:flex;align-items:center;gap:5px;min-width:0}.team.away{justify-content:flex-end}.crest{width:37px;height:37px;flex:0 0 37px;object-fit:contain;background:#fff;border-radius:50%;padding:2px}.teamname{font-size:clamp(9px,2.6vw,12px);font-weight:800;overflow-wrap:anywhere}.team.away .teamname{text-align:right}
.scores{display:flex;align-items:center;gap:3px}.score{width:37px;height:39px;border:2px solid #777;border-radius:7px;background:#fff;color:#111;text-align:center;font-size:20px;font-weight:900;padding:0;appearance:textfield;-moz-appearance:textfield}.score::-webkit-inner-spin-button,.score::-webkit-outer-spin-button{-webkit-appearance:none;margin:0}.score:focus{outline:2px solid var(--gold);border-color:var(--gold)}.dash{color:#aaa;font-weight:bold}
.actions{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px}.btn{border:0;border-radius:9px;padding:13px 8px;font-size:14px;font-weight:900;color:#fff;cursor:pointer;min-height:46px}.send{background:#168a4a}.copy{background:#b8860b;color:#000}.clear{background:#9f1c1c;grid-column:1/-1}.foot{text-align:center;color:#999;font-size:11px;margin:10px 0}
@media(max-width:360px){body{padding:5px}.match{gap:3px;padding:6px 3px}.crest{width:29px;height:29px;flex-basis:29px}.score{width:30px;height:35px;font-size:17px}.team{gap:3px}.btn{font-size:12px}}
</style>
</head>
<body>
<header><span class="ball">⚽</span><div><h1>QUINIELAS ROMA</h1><div class="sub">JORNADA 14</div></div><div><div class="ball">⚽</div><div class="price">$30</div><div class="small">POR PERSONA</div></div></header>
<input id="nombre" class="name" type="text" placeholder="ESCRIBE TU NOMBRE" autocomplete="name" aria-label="Nombre del participante">
<div class="note">💰 Pozo total: depende de cuántos participantes entren</div>
<div id="matches"></div>
<section class="actions">
<button class="btn copy" type="button" onclick="copiarTransferencia()">📋 COPIAR DATOS DE TRANSFERENCIA</button>
<button class="btn send" type="button" onclick="enviarWhatsApp()">ENVIAR POR WHATSAPP</button>
<button class="btn clear" type="button" onclick="borrar()">🗑 BORRAR QUINIELA</button>
</section>
<div class="foot">Completa tu nombre y los resultados de los 9 partidos antes de enviar.</div>
<script>
/* Los escudos se cargan desde Wikimedia; si una imagen no está disponible se muestra un balón. */
const teams = {
"FC Juárez":{logo:"https://upload.wikimedia.org/wikipedia/en/2/2e/FC_Juarez_logo.svg"},
"Necaxa":{logo:"https://upload.wikimedia.org/wikipedia/en/8/8b/Club_Necaxa_logo.svg"},
"Atlante":{logo:"https://upload.wikimedia.org/wikipedia/en/7/7d/Atlante_FC_logo.svg"},
"Atlético San Luis":{logo:"https://upload.wikimedia.org/wikipedia/en/1/1d/Atl%C3%A9tico_San_Luis_logo.svg"},
"León":{logo:"https://upload.wikimedia.org/wikipedia/en/0/0e/Club_Leon_FC_logo.svg"},
"Toluca":{logo:"https://upload.wikimedia.org/wikipedia/en/2/2c/Deportivo_Toluca_FC_logo.svg"},
"Rayados":{logo:"https://upload.wikimedia.org/wikipedia/en/8/8a/CF_Monterrey_logo.svg"},
"Chivas":{logo:"https://upload.wikimedia.org/wikipedia/en/6/67/Guadalajara_CD.png"},
"Pumas":{logo:"https://upload.wikimedia.org/wikipedia/en/2/2d/Club_Universidad_Nacional_Pumas.png"},
"Tigres":{logo:"https://upload.wikimedia.org/wikipedia/en/2/2a/Tigres_UANL_logo.svg"},
"Atlas":{logo:"https://upload.wikimedia.org/wikipedia/en/4/47/Atlas_FC_logo.svg"},
"Puebla":{logo:"https://upload.wikimedia.org/wikipedia/en/f/f1/Club_Puebla_logo.svg"},
"Querétaro":{logo:"https://upload.wikimedia.org/wikipedia/en/9/9d/Queretaro_FC_logo.svg"},
"Cruz Azul":{logo:"https://upload.wikimedia.org/wikipedia/en/5/5c/Cruz_Azul_FC_logo.svg"},
"América":{logo:"https://upload.wikimedia.org/wikipedia/en/9/9d/Club_America_crest.svg"},
"Pachuca":{logo:"https://upload.wikimedia.org/wikipedia/en/0/0c/CF_Pachuca_logo.svg"},
"Tijuana":{logo:"https://upload.wikimedia.org/wikipedia/en/8/8b/Club_Tijuana_logo.svg"},
"Santos Laguna":{logo:"https://upload.wikimedia.org/wikipedia/en/8/8e/Santos_Laguna_logo.svg"}
};
/* Partidos transcritos de la captura que compartiste (23–25 de octubre). Confirma si corresponden a la jornada 14. */
const games = [
["FC Juárez","Necaxa"],
["Atlante","Atlético San Luis"],
["León","Toluca"],
["Rayados","Chivas"],
["Pumas","Tigres"],
["Atlas","Puebla"],
["Querétaro","Cruz Azul"],
["América","Pachuca"],
["Tijuana","Santos Laguna"]
];
const root=document.getElementById('matches');
function crest(team){const t=teams[team];return `<img class="crest" src="${t.logo}" alt="Escudo ${team}" loading="lazy" onerror="this.onerror=null;this.src='data:image/svg+xml,%3Csvg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 64 64%22%3E%3Ctext x=%2232%22 y=%2245%22 font-size=%2245%22 text-anchor=%22middle%22%3E%E2%9A%BD%3C/text%3E%3C/svg%3E">`}
games.forEach((g,i)=>{const n=i+1;root.insertAdjacentHTML('beforeend',`<div class="match"><div class="team">${crest(g[0])}<span class="teamname">${g[0]}</span></div><div class="scores"><input class="score" id="g${n}a" type="number" min="0" max="99" inputmode="numeric" aria-label="Goles ${g[0]}"><span class="dash">–</span><input class="score" id="g${n}b" type="number" min="0" max="99" inputmode="numeric" aria-label="Goles ${g[1]}"></div><div class="team away"><span class="teamname">${g[1]}</span>${crest(g[1])}</div></div>`)});
function validar(){const name=document.getElementById('nombre').value.trim();if(!name){alert('Escribe tu nombre.');document.getElementById('nombre').focus();return false}for(let i=1;i<=games.length;i++){const a=document.getElementById(`g${i}a`).value,b=document.getElementById(`g${i}b`).value;if(a===''||b===''||Number(a)<0||Number(b)<0){alert('Completa los 18 recuadros de goles antes de continuar.');return false}}return true}
function textoQuiniela(){const lines=['QUINIELAS ROMA — JORNADA 14','Nombre: '+document.getElementById('nombre').value.trim(),''];games.forEach((g,i)=>{const a=document.getElementById(`g${i+1}a`).value,b=document.getElementById(`g${i+1}b`).value;lines.push(`${i+1}. ${g[0]} *${a}* - *${b}* ${g[1]}`)});return lines.join('\n')}
function enviarWhatsApp(){if(!validar())return;const message=encodeURIComponent(textoQuiniela());const url='https://wa.me/529342771745?text='+message;window.location.href=url}
async function copiarTransferencia(){
 const data='QUINIELAS ROMA — JORNADA 14\nInscripción: $30 por persona\nDatos para transferencia:\nTitular: Felipe Rodríguez\nCuenta: Spin by OXXO\nTarjeta: 4217470275099253';
 try{if(navigator.clipboard&&window.isSecureContext){await navigator.clipboard.writeText(data)}else{const ta=document.createElement('textarea');ta.value=data;ta.style.position='fixed';ta.style.opacity='0';document.body.appendChild(ta);ta.select();const ok=document.execCommand('copy');ta.remove();if(!ok)throw new Error('No se pudo copiar')}alert('Datos de transferencia copiados.')}catch(e){prompt('Copia los datos de transferencia:',data)}
}
function borrar(){if(!confirm('¿Borrar el nombre y todos los goles capturados?'))return;document.getElementById('nombre').value='';document.querySelectorAll('.score').forEach(x=>x.value='');document.getElementById('nombre').focus()}
</script>
</body>
</html>