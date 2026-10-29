var $=function(s){return document.querySelector(s)};
var K={aire:{r:.5,h:"Disipador y ventilador: económica y fácil de mantener."},liquida:{r:.345,h:"Bomba, radiador y líquido: enfría más, cuesta más."},pasiva:{r:.91,h:"Sin ventiladores: silenciosa, solo para equipos de poco consumo."}};
function sim(){
  var load=+$("#load").value,dust=+$("#dust").value,amb=+$("#amb").value,t=document.querySelector("input[name=t]:checked").value;
  $("#o-load").textContent=load+"%";$("#o-dust").textContent=dust+"%";$("#o-amb").textContent=amb+" °C";$("#thint").textContent=K[t].h;
  var T=Math.min(110,amb+(15+load*.9)*K[t].r*(1+dust/100*.8)+4);
  var rpm=t==="pasiva"?0:Math.round(600+Math.min(1,Math.max(0,(T-35)/50))*2400);
  var perf=T>=100?0:T>88?Math.max(40,Math.round(100-(T-88)*5)):100;
  var s,c,tip;
  if(T<60){s="Óptimo";c="#39A900";tip="Todo fresco. Así debe verse un equipo bien cuidado."}
  else if(T<75){s="Normal";c="#39A900";tip="Temperatura de trabajo sana."}
  else if(T<88){s="Alerta";c="#C98A00";tip=dust>50?"El polvo está atrapando calor. Limpia el equipo.":"Se acerca al límite. Baja la carga o mejora la refrigeración."}
  else if(T<100){s="Throttling";c="#E8590C";tip="El procesador se frena solo para no quemarse: por eso el equipo se pone lento."}
  else{s="Apagado";c="#C1121F";tip="Apagado de emergencia: el sistema se protege y puedes perder trabajo."}
  $("#temp").textContent=Math.round(T);$("#state").textContent=s;$("#state").style.background=c;$("#tip").textContent=tip;
  $("#mark").style.left=Math.min(100,Math.max(0,(T-20)/90*100))+"%";
  $("#rpm").textContent=rpm.toLocaleString("es-CO");$("#perf").textContent=perf+"%";
  $("#disp").classList.toggle("off",T>=100);
  var f=$("#fan");f.style.animationPlayState=rpm?"running":"paused";f.style.animationDuration=(2.8-rpm/3000*2.5).toFixed(2)+"s";
}
document.querySelectorAll("#sim input").forEach(function(e){e.addEventListener("input",sim)});
sim();
/* Checklist */
var cks=document.querySelectorAll(".chk input");
cks.forEach(function(c){c.addEventListener("change",function(){
  var n=0;cks.forEach(function(x){if(x.checked)n++});
  $("#pbi").style.width=n*20+"%";$("#pb").setAttribute("aria-valuenow",n*20);
  $("#pbt").textContent=n+" de 5 tareas hechas"+(n===5?". Tu equipo está en buenas manos.":"");
  if(c.dataset.dust&&c.checked){$("#dust").value=0;sim()}
})});
/* Quiz */
var Q=[
{q:"¿Qué componente suele ser el más caliente en un PC gamer?",o:["La memoria RAM","La tarjeta gráfica (GPU)","El teclado"],a:1,e:"La GPU procesa miles de cálculos en paralelo y genera mucho calor."},
{q:"¿Para qué sirve la pasta térmica?",o:["Pegar el disipador a la placa","Llenar los huecos microscópicos entre el chip y el disipador","Enfriar con líquido"],a:1,e:"El aire atrapado entre ambas piezas conduce mal el calor; la pasta lo evita."},
{q:"El procesador baja su velocidad al calentarse demasiado. ¿Cómo se llama?",o:["Thermal throttling","Overclocking","Formateo"],a:0,e:"Es una protección: menos velocidad, menos calor, pero también menos rendimiento."},
{q:"¿Cada cuánto conviene limpiar el polvo interno?",o:["Cada 3 a 6 meses","Cada 10 años","Nunca, el polvo protege"],a:0,e:"Más seguido si hay mascotas, polvo o mucho calor."}];
var qi=0,sc=0;
function show(){
  var q=Q[qi];$("#qn").textContent="Pregunta "+(qi+1)+" de "+Q.length;$("#qq").textContent=q.q;$("#fb").textContent="";$("#next").style.display="none";
  var box=$("#qo");box.innerHTML="";
  q.o.forEach(function(t,i){var b=document.createElement("button");b.className="opt";b.textContent=t;b.onclick=function(){pick(i)};box.appendChild(b)});
}
function pick(i){
  var q=Q[qi],bs=document.querySelectorAll("#qo .opt");
  bs.forEach(function(b,j){b.disabled=true;if(j===q.a)b.classList.add("ok");else if(j===i)b.classList.add("bad")});
  if(i===q.a)sc++;
  $("#fb").textContent=(i===q.a?"Correcto. ":"Casi. ")+q.e;
  var n=$("#next");n.style.display="inline-block";n.textContent=qi<Q.length-1?"Siguiente pregunta":"Ver resultado";
}
function nextQ(){
  if(qi<Q.length-1){qi++;show();return}
  $("#qn").textContent="Resultado";$("#qq").textContent=sc+" de "+Q.length+" respuestas correctas";$("#qo").innerHTML="";
  $("#fb").textContent=sc===Q.length?"Perfecto. Ya sabes cuidar la temperatura de un equipo.":"Repasa el simulador y vuelve a intentarlo.";
  var n=$("#next");n.textContent="Intentar de nuevo";n.onclick=function(){qi=0;sc=0;n.onclick=nextQ;show()};
}
$("#next").onclick=nextQ;
show();
/* Tema */
$("#tg").onclick=function(){
  var r=document.documentElement,d=r.dataset.theme?r.dataset.theme==="dark":matchMedia("(prefers-color-scheme:dark)").matches;
  r.dataset.theme=d?"light":"dark";
};
