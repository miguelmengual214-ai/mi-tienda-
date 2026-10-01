<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>🚂 TIENDA LA ESTACION - Riohacha</title>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.2/dist/confetti.browser.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@400;700&display=swap');
*{box-sizing:border-box;font-family:'Fredoka',system-ui}
body{margin:0;background:radial-gradient(circle at 10% 20%,#fef9c3,#dcfce7,#dbeafe,#fce7f3);min-height:100vh;padding-bottom:90px}
#login{position:fixed;inset:0;background:linear-gradient(135deg,#16a34a,#15803d,#facc15);display:flex!important;align-items:center;justify-content:center;padding:20px;z-index:99999}
#login.oculto{display:none!important}
.box{background:#fff;border-radius:30px;padding:28px;width:100%;max-width:380px;text-align:center;border:5px solid #fff;box-shadow:0 25px 50px rgba(0,0,0,.3), inset 0 0 0 4px #facc15}
input{width:100%;padding:14px;border:3px solid #bbf7d0;border-radius:16px;margin:7px 0;font-weight:700}
.btn{width:100%;padding:14px;border:none;border-radius:16px;font-weight:900;cursor:pointer;box-shadow:0 6px 0 rgba(0,0,0,.15)}
header{background:linear-gradient(90deg,#16a34a,#22c55e,#facc15,#fb923c);color:#fff;padding:14px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:20}
.tabs{display:flex;gap:8px;padding:12px;background:rgba(255,255,255,0.9);overflow:auto;border-bottom:4px solid #facc15;position:sticky;top:72px;z-index:10}
.tab{padding:10px 16px;border-radius:24px;border:3px solid #e5e7eb;background:#fff;font-weight:900;font-size:11px;white-space:nowrap}
.tab.active{background:linear-gradient(90deg,#16a34a,#22c55e);color:#fff;transform:scale(1.1)}
.card{background:rgba(255,255,255,0.92);margin:12px;border-radius:22px;padding:16px;border:3px solid #fff;box-shadow:0 8px 20px rgba(0,0,0,.08)}
.prod{display:flex;justify-content:space-between;align-items:center;padding:12px 0;border-bottom:2px dashed #e5e7eb;gap:8px}
.logo{width:88px;height:88px;border-radius:50%;background:conic-gradient(from 0deg,#16a34a,#facc15,#fb923c,#16a34a);display:flex;align-items:center;justify-content:center;font-size:42px;border:5px solid #fff}
.badge{background:#facc15;color:#14532d;padding:3px 10px;border-radius:20px;font-weight:900;font-size:10px}
</style>
</head>
<body>
<div id="login"><div class="box"><div style="display:flex;justify-content:center"><div class="logo" style="width:120px;height:120px;font-size:64px"><span>🚂</span></div></div><h1 style="color:#14532d;margin:10px 0 0">TIENDA LA ESTACION</h1><p style="color:#16a34a;font-weight:800">📍 Km 1 Vía Maicao<br>Riohacha - La Guajira</p><input id="u" value="estacion"><input id="p" type="password" value="Miguel98."><button class="btn" style="background:#111;color:#fff;margin-top:12px" onclick="entrar()">🚀 ENTRAR</button><div id="msg" style="color:red;font-weight:900;font-size:12px"></div></div></div>

<div id="app">
<header><div style="display:flex;align-items:center;gap:12px"><div class="logo"><span>🚂</span></div><div><b>TIENDA LA ESTACIÓN</b><br><span class="badge">📍 Km 1 Vía Maicao - Riohacha</span><br><small id="fecha" class="badge" style="background:#fff"></small></div></div><button onclick="salir()" style="background:#fff;color:#dc2626;border:none;border-radius:12px;padding:8px 14px;font-weight:900">X</button></header>
<div class="tabs">
<button id="tV" class="tab active" onclick="ver('vender')">🛒 VENDER</button>
<button id="tI" class="tab" onclick="ver('inv')">📦 INVENTARIO</button>
<button id="tP" class="tab" onclick="ver('personas')">👤 PERSONAS</button>
<button id="tF" class="tab" onclick="ver('fiados')">🤝 FIADOS</button>
<button id="tG" class="tab" onclick="ver('gastos')">💸 GASTOS</button>
<button id="tC" class="tab" onclick="ver('cierre')">🎉 CIERRE</button>
<button id="tS" class="tab" onclick="ver('config')">⚙️</button>
</div>

<div id="sec-vender" class="card" style="border:4px solid #22c55e">
<div id="alertaStock" style="display:none;padding:12px;border-radius:16px;font-weight:900;margin-bottom:12px;text-align:center"></div>
<div style="background:linear-gradient(135deg,#dcfce7,#fef9c3);border:3px solid #22c55e;padding:14px;border-radius:18px;margin-bottom:12px">
<b>👤 Persona Natural - Registro Obligatorio Riohacha</b>
<input id="clienteVenta" list="listaClientes" placeholder="Nombre completo *">
<div style="display:flex;gap:8px">
<input id="cedulaVenta" placeholder="Cédula * Ej: 1122334455" style="margin:0">
<input id="telefonoVenta" type="tel" placeholder="📱 Cel 300..." style="margin:0">
</div>
<small style="color:#16a34a;font-weight:800">✅ Queda registrado en el cierre automáticamente</small>
<datalist id="listaClientes"></datalist>
</div>
<div id="carrito" style="background:#f8fafc;border-radius:16px;padding:8px;min-height:50px"></div>
<div style="display:flex;justify-content:space-between;font-weight:900;padding:16px 0;border-top:4px dashed #16a34a;font-size:22px"><span>Total:</span><span id="total">$0</span></div>
<button class="btn" style="background:linear-gradient(90deg,#16a34a,#22c55e,#facc15);color:#14532d;font-size:16px" onclick="vender()">📄 VENDER + REGISTRAR PERSONA + PDF</button>
<button class="btn" style="background:#111;color:#fff;margin-top:10px" onclick="C=[];document.getElementById('clienteVenta').value='';document.getElementById('cedulaVenta').value='';document.getElementById('telefonoVenta').value='';render()">🧹 Limpiar</button>
<div id="listaProd" style="margin-top:14px"></div>
</div>

<div id="sec-personas" class="card" style="display:none"><b>👤 PERSONAS NATURALES REGISTRADAS</b><p style="font-size:12px">Todas las personas que han comprado en Km 1 Vía Maicao</p><div style="display:flex;gap:8px;margin:10px 0"><input id="searchPersona" placeholder="Buscar por nombre o cédula" oninput="render()"><button onclick="exportarPersonas()" style="background:#16a34a;color:#fff;border:none;border-radius:12px;padding:0 14px;font-weight:900">📊 EXCEL</button></div><div id="listaPersonas"></div></div>

<div id="sec-inv" class="card" style="display:none"><b>📦 Inventario</b><div style="display:flex;gap:8px;margin:14px 0"><button onclick="filtroInv='todos';render()" style="flex:1;border-radius:16px;padding:12px;font-weight:900;border:3px solid #111">TODOS</button><button onclick="filtroInv='agotados';render()" style="flex:1;border-radius:16px;padding:12px;font-weight:900">🚨 AGOTADOS</button><button onclick="filtroInv='bajo';render()" style="flex:1;border-radius:16px;padding:12px;font-weight:900">⚠️ BAJOS</button></div><button onclick="borrarRepetidos()" style="background:#fff;border:3px dashed #dc2626;color:#dc2626;border-radius:16px;padding:14px;width:100%;font-weight:900;margin-bottom:14px">🧹 BORRAR REPETIDOS</button><div style="background:linear-gradient(135deg,#f0fdf4,#fef9c3);border:4px solid #16a34a;padding:14px;border-radius:18px;margin-bottom:14px"><b id="editTitle">➕ Crear producto</b><input id="nn" placeholder="Nombre"><div style="display:flex;gap:8px"><input id="np" type="number" placeholder="Precio"><input id="nc" type="number" placeholder="Costo"><input id="ns" type="number" placeholder="Stock"></div><div style="display:flex;gap:8px;margin-top:10px"><button id="btnGuardar" class="btn" style="background:#16a34a;color:#fff" onclick="guardar()">Crear</button><button id="btnCancelar" class="btn" style="background:#e5e7eb;display:none" onclick="cancelarEdicion()">Cancelar</button></div></div><div id="inv"></div></div>
<div id="sec-fiados" class="card" style="display:none"><b>🤝 Fiados</b><div style="display:flex;gap:8px;margin-top:12px"><input id="fn" placeholder="Cliente"><input id="fv" type="number" placeholder="Valor"><input id="ftel" placeholder="Cel"></div><button class="btn" style="background:#d97706;color:#fff;margin-top:12px" onclick="addFiado()">+ Fiado</button><div id="fiados" style="margin-top:12px"></div></div>
<div id="sec-gastos" class="card" style="display:none"><b>💸 Gastos</b><div style="display:flex;gap:8px;margin-top:12px"><input id="gn" placeholder="Concepto"><input id="gv" type="number" placeholder="Valor"><button onclick="addGasto()" style="background:#ef4444;color:#fff;border:none;border-radius:14px;padding:0 18px;font-weight:900">+</button></div><div id="listaGastos" style="margin-top:12px"></div></div>

<div id="sec-cierre" class="card" style="display:none">
<b>🎉 CIERRE - Km 1 Vía Maicao - Riohacha</b>
<div id="resumen" style="background:linear-gradient(135deg,#f0fdf4,#dcfce7);padding:16px;border-radius:18px;margin:14px 0;border:3px solid #bbf7d0"></div>
<div id="resumenPersonas" style="background:linear-gradient(135deg,#fef9c3,#fde68a);padding:14px;border-radius:18px;margin:14px 0;border:3px solid #f59e0b"></div>
<button onclick="exportarExcel()" style="background:#16a34a;color:#fff;border:none;border-radius:16px;padding:16px;width:100%;font-weight:900">📊 EXCEL CON PERSONAS</button>
<button onclick="generarPDFCierre()" style="background:#dc2626;color:#fff;border:none;border-radius:16px;padding:16px;width:100%;font-weight:900;margin-top:10px">📄 PDF CIERRE + PERSONAS RIOHACHA</button>
<div style="margin-top:16px"><b>🧾 Facturas hoy</b></div><div id="facturas"></div>
</div>

<div id="sec-config" class="card" style="display:none"><b>⚙️ Config</b><div style="background:#fff;border:3px solid #93c5fd;border-radius:18px;padding:16px;margin-top:14px"><b>Usuario</b><input id="newUser"><b>Contraseña</b><input id="newPass"><button onclick="cambiarClave()" class="btn" style="background:#2563eb;color:#fff;margin-top:12px">💾 Guardar</button></div></div>
</div>

<script>
let P=JSON.parse(localStorage.getItem('p')||'null')||[{n:"Coca-cola 1L",p:5000,c:3500,s:10},{n:"Pan",p:500,c:300,s:20}];
let V=JSON.parse(localStorage.getItem('v')||'[]'),G=JSON.parse(localStorage.getItem('g')||'[]'),F=JSON.parse(localStorage.getItem('f')||'[]');
let PERSONAS=JSON.parse(localStorage.getItem('personas')||'[]');
let C=[],editIdx=-1,filtroInv='todos',hoy=()=>new Date().toISOString().slice(0,10);
document.getElementById('fecha').innerText=new Date().toLocaleDateString('es-CO',{weekday:'long',day:'numeric',month:'long'});
function getCred(){let c=JSON.parse(localStorage.getItem('cred')||'null');return c||{u:'estacion',p:'Miguel98.'};}
function save(){localStorage.setItem('p',JSON.stringify(P));localStorage.setItem('v',JSON.stringify(V));localStorage.setItem('g',JSON.stringify(G));localStorage.setItem('f',JSON.stringify(F));localStorage.setItem('personas',JSON.stringify(PERSONAS));}
function entrar(){let u=document.getElementById('u').value.toLowerCase().trim(),p=document.getElementById('p').value,cred=getCred();if(u===cred.u.toLowerCase()&&p===cred.p){localStorage.setItem('sesion','on');document.getElementById('login').classList.add('oculto');render();confetti({particleCount:100});}else document.getElementById('msg').innerText='Es: '+cred.u+' / '+cred.p;}
function salir(){localStorage.removeItem('sesion');location.reload();}
function ver(s){['vender','inv','personas','fiados','gastos','cierre','config'].forEach(k=>{document.getElementById('sec-'+k).style.display=k===s?'block':'none';});document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));let id={vender:'tV',inv:'tI',personas:'tP',fiados:'tF',gastos:'tG',cierre:'tC',config:'tS'}[s];document.getElementById(id).classList.add('active');if(s==='config'){let c=getCred();document.getElementById('newUser').value=c.u;document.getElementById('newPass').value=c.p;}render();}
function cambiarClave(){let nu=document.getElementById('newUser').value.trim(),np=document.getElementById('newPass').value.trim();if(!nu||!np)return;localStorage.setItem('cred',JSON.stringify({u:nu,p:np}));alert('✅ '+nu);ver('vender');}
function editarProducto(i){editIdx=i;document.getElementById('nn').value=P[i].n;document.getElementById('np').value=P[i].p;document.getElementById('nc').value=P[i].c;document.getElementById('ns').value=P[i].s;document.getElementById('btnGuardar').innerText='Actualizar';document.getElementById('btnCancelar').style.display='block';ver('inv');}
function cancelarEdicion(){editIdx=-1;document.getElementById('nn').value='';document.getElementById('np').value='';document.getElementById('nc').value='';document.getElementById('ns').value='';document.getElementById('btnGuardar').innerText='Crear';document.getElementById('btnCancelar').style.display='none';}
function guardar(){let n=document.getElementById('nn').value.toUpperCase().trim(),p=parseInt(document.getElementById('np').value),s=parseInt(document.getElementById('ns').value)||0,c=parseInt(document.getElementById('nc').value)||0;if(!n||!p)return;if(editIdx>=0)P[editIdx]={n,p,c,s};else P.push({n,p,c,s});save();cancelarEdicion();render();}
function eliminarProducto(i){if(confirm('Eliminar '+P[i].n+'?')){P.splice(i,1);save();render();}}
function borrarRepetidos(){let v={},r=[];for(let i=P.length-1;i>=0;i--){let k=P[i].n.toLowerCase();if(v[k]){r.push(P[i].n);P.splice(i,1);}else v[k]=true;}if(!r.length)alert('No hay repetidos');else{save();render();alert('Borrados: '+r.join(','));}}

function generarFacturaPDF(venta, items){
 const {jsPDF}=window.jspdf; const doc=new jsPDF();
 doc.setFillColor(22,163,74); doc.rect(0,0,210,44,'F'); doc.setFillColor(250,204,21); doc.rect(0,44,210,5,'F');
 doc.setFont('helvetica','bold'); doc.setFontSize(18); doc.setTextColor(255,255,255); doc.text('TIENDA LA ESTACION',105,18,{align:'center'});
 doc.setFontSize(12); doc.text('Km 1 Via Maicao',105,27,{align:'center'}); doc.text('Riohacha - La Guajira',105,34,{align:'center'});
 doc.setFontSize(8); doc.setFont('helvetica','normal'); doc.text('Km 1 Via Maicao, Riohacha - La Guajira, Colombia - Persona Natural',105,41,{align:'center'});
 let y=58; doc.setTextColor(20,83,45); doc.setFontSize(12); doc.setFont('helvetica','bold'); doc.text('FACTURA PERSONA NATURAL',14,y); y+=6; doc.setDrawColor(22,163,74); doc.line(14,y,196,y); y+=8;
 doc.setFont('helvetica','normal'); doc.setFontSize(10); doc.setTextColor(50); doc.text('Fecha: '+new Date().toLocaleString('es-CO'),14,y); y+=6; doc.text('Factura #: '+venta.id.toString().slice(-6),14,y); y+=6;
 doc.setFont('helvetica','bold'); doc.text('Cliente (Persona Natural): '+(venta.cliente||'MOSTRADOR'),14,y); y+=6;
 doc.setFont('helvetica','normal'); doc.text('CC: '+(venta.cedula||'No registrada'),14,y); y+=6; doc.text('Tel: '+(venta.tel||'--'),14,y); y+=6;
 doc.text('Direccion Tienda: Km 1 Via Maicao - Riohacha',14,y); y+=8; doc.setDrawColor(200); doc.line(14,y,196,y); y+=8;
 doc.setFont('helvetica','bold'); doc.setFontSize(11); doc.setTextColor(20,83,45); doc.text('PRODUCTOS',14,y); y+=7;
 doc.setFont('helvetica','normal'); doc.setFontSize(10); doc.setTextColor(0);
 items.forEach(it=>{if(y>270){doc.addPage();y=20;} doc.text('• '+it.n,14,y); doc.text('$'+it.p.toLocaleString(),170,y,{align:'right'}); y+=7;});
 y+=4; doc.setDrawColor(22,163,74); doc.setLineWidth(0.8); doc.line(14,y,196,y); y+=10; doc.setFont('helvetica','bold'); doc.setFontSize(14); doc.setTextColor(22,163,74); doc.text('TOTAL: $'+venta.t.toLocaleString(),14,y); y+=12;
 doc.setFontSize(10); doc.setTextColor(20,83,45); doc.text('Gracias persona natural por comprar en Riohacha! 🙏',105,y,{align:'center'});
 return doc;
}
function generarPDFCierre(){
 let ventasHoy=V.filter(x=>x.f===hoy()),gastosHoy=G.filter(x=>x.f===hoy());
 const {jsPDF}=window.jspdf; const doc=new jsPDF();
 doc.setFillColor(22,163,74); doc.rect(0,0,210,35,'F'); doc.setFillColor(250,204,21); doc.rect(0,35,210,4,'F');
 doc.setTextColor(255); doc.setFontSize(14); doc.setFont('helvetica','bold'); doc.text('CIERRE RIOHACHA + PERSONAS NATURALES',105,16,{align:'center'});
 doc.setFontSize(9); doc.text('Km 1 Via Maicao - Riohacha - La Guajira - '+hoy(),105,24,{align:'center'});
 let y=42; doc.setTextColor(20,83,45); doc.setFontSize(11); doc.text('RESUMEN DIA',14,y); y+=7; doc.setFont('helvetica','normal'); doc.setFontSize(9);
 let totalV=ventasHoy.reduce((a,b)=>a+b.t,0),totalG=gastosHoy.reduce((a,b)=>a+b.v,0);
 doc.text('Ventas: $'+totalV.toLocaleString()+' ('+ventasHoy.length+')',14,y); y+=5; doc.text('Gastos: $'+totalG.toLocaleString(),14,y); y+=5;
 doc.setFont('helvetica','bold'); doc.text('Ganancia: $'+(totalV-totalG).toLocaleString(),14,y); y+=8;
 // PERSONAS DEL DIA
 let personasHoy={}; ventasHoy.forEach(v=>{let k=(v.cedula||v.cliente); if(!personasHoy[k]) personasHoy[k]=v;});
 doc.setFont('helvetica','bold'); doc.text('PERSONAS NATURALES QUE COMPRARON HOY ('+Object.keys(personasHoy).length+'):',14,y); y+=6;
 doc.setFont('helvetica','normal'); doc.setFontSize(8);
 ventasHoy.forEach(v=>{if(y>270){doc.addPage(); y=15;} doc.text('- '+(v.cliente||'MOSTRADOR')+' CC:'+(v.cedula||'--')+' Tel:'+(v.tel||'--')+' $'+v.t.toLocaleString()+' - '+v.detalle.substring(0,40),14,y); y+=5;});
 doc.save('CIERRE_PERSONAS_RIOHACHA_'+hoy()+'.pdf'); confetti();
}
function render(){
 let agot=P.filter(x=>x.s<=0),a=document.getElementById('alertaStock');
 if(agot.length){a.style.display='block';a.style.background='#fee2e2';a.style.border='3px solid #ef4444';a.style.color='#dc2626';a.innerHTML='AGOTADO: '+agot.map(x=>x.n).join(', ');}else a.style.display='none';
 document.getElementById('listaClientes').innerHTML=PERSONAS.map(p=>'<option value="'+p.nombre+'">').join('');
 let hc='';for(let i=0;i<C.length;i++) hc+='<div class=prod><span>'+C[i].n+'</span><b>$'+C[i].p.toLocaleString()+'</b></div>';
 document.getElementById('carrito').innerHTML=hc||'Carrito vacío';
 document.getElementById('total').innerText='$'+C.reduce((a,b)=>a+b.p,0).toLocaleString();
 let hp='';for(let i=0;i<P.length;i++){hp+='<div class=prod><div><b>'+P[i].n+'</b><br><small>$'+P[i].p.toLocaleString()+' Stock '+P[i].s+'</small></div><button onclick="agregar('+i+')" style="background:'+(P[i].s<=0?'#9ca3af':'#16a34a')+';color:#fff;border:none;border-radius:50%;width:38px;height:38px" '+(P[i].s<=0?'disabled':'')+'>+</button></div>';}
 document.getElementById('listaProd').innerHTML=hp;
 // inventario
 let lista=P.map((p,i)=>({...p,idx:i})).filter(x=>{if(filtroInv==='agotados')return x.s<=0;if(filtroInv==='bajo')return x.s>0&&x.s<=3;return true;});
 let hi='';for(let it of lista){let i=it.idx;hi+='<div style="background:#fff;border:3px solid #bbf7d0;border-radius:16px;padding:12px;margin-bottom:8px"><div style="display:flex;justify-content:space-between"><div><b>'+P[i].n+'</b><br><small>Stock '+P[i].s+' $'+P[i].p.toLocaleString()+'</small></div><button onclick="eliminarProducto('+i+')" style="background:#dc2626;color:#fff;border:none;border-radius:8px;padding:6px">🗑️</button></div></div>';}
 document.getElementById('inv').innerHTML=hi;
 // personas
 let busq=(document.getElementById('searchPersona')?.value||'').toLowerCase();
 let persFil=PERSONAS.filter(p=>p.nombre.toLowerCase().includes(busq)||p.cedula.includes(busq));
 let lp='';for(let p of persFil) lp+='<div style="background:#fff;border:3px solid #fde68a;border-radius:16px;padding:12px;margin-bottom:8px"><b>👤 '+p.nombre+'</b><br><small>CC: '+p.cedula+' | 📱 '+p.tel+'<br>🛒 Compras: '+p.compras+' | 💰 Total gastado: $'+p.total.toLocaleString()+'<br>📅 Última: '+p.ultima+'</small></div>';
 document.getElementById('listaPersonas').innerHTML=lp||'Sin personas registradas';
 // fiados y gastos
 let hf='';for(let i=0;i<F.length;i++) hf+='<div class=prod><div><b>'+F[i].n+'</b> $'+F[i].v.toLocaleString()+'</div><button onclick="F.splice('+i+',1);save();render()" style="background:#16a34a;color:#fff;border:none;border-radius:8px;padding:4px">Pagó</button></div>';
 document.getElementById('fiados').innerHTML=hf||'Sin fiados';
 let gastosHoy=G.filter(x=>x.f===hoy()),hg='';for(let g of gastosHoy) hg+='<div class=prod><span>'+g.n+'</span><b>$'+g.v.toLocaleString()+'</b></div>';
 document.getElementById('listaGastos').innerHTML=hg||'Sin gastos';
 let ventasHoy=V.filter(x=>x.f===hoy()),totalV=ventasHoy.reduce((a,b)=>a+b.t,0),totalG=gastosHoy.reduce((a,b)=>a+b.v,0);
 document.getElementById('resumen').innerHTML='Ventas hoy: <b>$'+totalV.toLocaleString()+'</b> ('+ventasHoy.length+')<br>Gastos: <b>$'+totalG.toLocaleString()+'</b><br>Ganancia: <b style="color:#16a34a">$'+(totalV-totalG).toLocaleString()+'</b>';
 let personasHoyMap={}; ventasHoy.forEach(v=>{let key=v.cedula||v.cliente; if(!personasHoyMap[key]) personasHoyMap[key]=v;});
 let personasHoyArr=Object.values(personasHoyMap);
 document.getElementById('resumenPersonas').innerHTML='<b>👤 Personas naturales hoy: '+personasHoyArr.length+'</b><br>'+personasHoyArr.map(p=>`• ${p.cliente} CC:${p.cedula||'--'} $${p.t.toLocaleString()}`).join('<br>');
 let fact='';for(let v of ventasHoy) fact+='<div class=prod><span><b>👤 '+ (v.cliente||'MOSTRADOR')+'</b> CC:'+(v.cedula||'--')+'<br><small>$'+v.t.toLocaleString()+' - '+v.detalle.substring(0,30)+'</small></span><button onclick="let venta=V.find(x=>x.id==='+v.id+');let doc=generarFacturaPDF(venta,[{n:venta.detalle,p:venta.t}]);doc.save(\'FACTURA_PERSONA_'+v.id+'.pdf\')" style="background:#dc2626;color:#fff;border:none;border-radius:8px;padding:4px 8px">📄</button></div>';
 document.getElementById('facturas').innerHTML=fact||'Sin ventas';
}
function agregar(i){if(P[i].s<=0){alert('AGOTADO');ver('inv');filtroInv='agotados';render();return;}C.push({n:P[i].n,p:P[i].p,idx:i});P[i].s--;save();render();}
function addFiado(){let n=document.getElementById('fn').value.trim(),v=parseInt(document.getElementById('fv').value),tel=document.getElementById('ftel').value.trim();if(!n||!v)return;F.push({n,v,tel,f:hoy()});save();render();document.getElementById('fn').value='';document.getElementById('fv').value='';document.getElementById('ftel').value='';}
function addGasto(){let n=document.getElementById('gn').value.trim(),v=parseInt(document.getElementById('gv').value);if(!n||!v)return;G.push({n,v,f:hoy()});save();render();document.getElementById('gn').value='';document.getElementById('gv').value='';}
function vender(){
 if(!C.length){alert('Carrito vacío');return;}
 let nombre=document.getElementById('clienteVenta').value.trim().toUpperCase();
 let cedula=document.getElementById('cedulaVenta').value.trim();
 let tel=document.getElementById('telefonoVenta').value.trim().replace(/\D/g,'');
 if(!nombre){alert('⚠️ Pon el nombre de la persona natural');return;}
 if(!cedula){alert('⚠️ Pon la cédula de la persona');return;}
 let t=C.reduce((a,b)=>a+b.p,0),d=C.map(x=>x.n).join(', ');
 // Guardar persona natural
 let existente=PERSONAS.find(p=>p.cedula===cedula);
 if(existente){existente.nombre=nombre; existente.tel=tel; existente.compras+=1; existente.total+=t; existente.ultima=hoy();}
 else PERSONAS.push({nombre,cedula,tel,compras:1,total:t,ultima:hoy(),registro:hoy()});
 let venta={id:Date.now(),f:hoy(),t,detalle:d,cliente:nombre,cedula,tel};
 V.push(venta); save();
 let items=[...C]; let doc=generarFacturaPDF(venta,items);
 doc.save('FACTURA_PERSONA_'+nombre.replace(/\s+/g,'_')+'_'+venta.id.toString().slice(-4)+'.pdf');
 C=[];document.getElementById('clienteVenta').value='';document.getElementById('cedulaVenta').value='';document.getElementById('telefonoVenta').value='';
 render(); confetti({particleCount:150}); ver('cierre');
}
function exportarExcel(){let ventasHoy=V.filter(x=>x.f===hoy());let wb=XLSX.utils.book_new();
 XLSX.utils.book_append_sheet(wb,XLSX.utils.json_to_sheet(ventasHoy.map(v=>({FECHA:v.f,CLIENTE_PERSONA_NATURAL:v.cliente,CEDULA:v.cedula,TELEFONO:v.tel,DETALLE:v.detalle,TOTAL:v.t,Direccion_Tienda:'Km 1 Via Maicao - Riohacha'}))),"VENTAS_PERSONAS");
 XLSX.utils.book_append_sheet(wb,XLSX.utils.json_to_sheet(PERSONAS),"PERSONAS_NATURALES");
 XLSX.writeFile(wb,'CIERRE_PERSONAS_RIOHACHA_'+hoy()+'.xlsx');
}
function exportarPersonas(){let wb=XLSX.utils.book_new();XLSX.utils.book_append_sheet(wb,XLSX.utils.json_to_sheet(PERSONAS),"PERSONAS");XLSX.writeFile(wb,'PERSONAS_RIOHACHA.xlsx');}
if(localStorage.getItem('sesion')==='on'){document.getElementById('login').classList.add('oculto');}
render();
</script>
</body>
</html>