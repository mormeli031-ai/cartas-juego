# cartas-juego
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Juego de Cartas</title>

<style>
body{
  margin:0;
  background:#000;
  color:#fff;
  font-family:Arial, sans-serif;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
}
.container{
  width:95vw;
  max-width:520px;
  text-align:center;
}
.card{
  min-height:60vh;
  border-radius:20px;
  padding:25px;
  display:flex;
  flex-direction:column;
  justify-content:space-between;
}
.category{
  font-weight:bold;
  letter-spacing:2px;
}
.text{
  font-size:1.4em;
  white-space:pre-line;
}
button{
  margin-top:20px;
  padding:15px;
  border:none;
  border-radius:12px;
  font-size:1em;
  cursor:pointer;
}
</style>
</head>

<body>
<div class="container">
  <div id="card" class="card">
    <div id="category" class="category">REGLAS</div>
    <div id="text" class="text">
Todo lo que diga la carta se acepta.
No se habla.
Se juega por turnos.
    </div>
  </div>
  <button onclick="nextCard()">Sacar carta</button>
</div>

<script>
let activeRule = null;

let categories = [
 {name:'HOT', color:'#ff3366', cards:[
'Dale un beso a la persona de tu izquierda.',
'Lame el labio inferior de alguien al azar.',
'Cierra los ojos. A la cuenta de tres, si se miran, se besan.',
'Susurra algo sucio al oído de quien quieras.',
'Chupa el dedo de la persona frente a ti.',
'Da un beso largo en el cuello de alguien.',
'Muérdele suavemente la oreja a alguien.',
'Pasa tu dedo lentamente por los labios de alguien.',
'Haz contacto visual sexy durante quince segundos.',
'Da un beso apasionado a quien más te atraiga.',
'Pasa un hielo por el cuerpo de alguien.',
'Toca suavemente la pierna de alguien hasta que diga basta.',
'Besan el cachete dejando marca de lengua.',
'Deja que alguien te ponga un reto spicy privado.',
'Juega siete segundos en el paraíso con alguien al azar.'
 ]},

 {name:'BEBER', color:'#4da6ff', cards:[
'Toma dos tragos.',
'Reparte tres tragos.',
'Toma un trago por cada persona que te atraiga.',
'Elige a alguien para beber contigo.',
'Todos beben menos tú.',
'El último en llegar hoy bebe.',
'Bebe si besarías a alguien de aquí.',
'Cascada.',
'El que menos dinero tenga bebe.',
'Beben los que hayan tenido una relación tóxica.'
 ]},

 {name:'VERDAD', color:'#666', cards:[
'¿A quién besarías de este grupo?',
'¿Cuál es tu fantasía sexual más atrevida?',
'¿Te excita alguien que está jugando ahora?',
'¿Con quién tendrías una cita secreta?',
'¿Qué es lo más intenso que has hecho en una fiesta?',
'¿Has fingido un orgasmo?',
'¿Qué parte del cuerpo disfrutas besar más?',
'¿Con quién del grupo te casarías?',
'¿Has tenido sexo con más de una persona el mismo día?',
'¿Cuál fue el lugar más raro donde besaste a alguien?'
 ]},

 {name:'RETO', color:'#ff3300', cards:[
'Haz contacto visual y muérdete el labio.',
'Dale un abrazo lento a quien elijas.',
'Susurra algo coqueto a alguien.',
'Imita tu posición favorita sin decir nada.',
'Deja que el grupo elija una prenda para quitarte.',
'Besa el cuello de alguien.',
'Haz tu mejor gemido.',
'Dile a alguien qué parte de su cuerpo besarías.',
'Déjate vendar los ojos y recibe un beso.',
'Quita una prenda usando solo los dientes.'
 ]},

 {name:'YO NUNCA HE', color:'#999', cards:[
'Yo nunca he sentido atracción por alguien de este grupo.',
'Yo nunca he enviado mensajes hot a la persona equivocada.',
'Yo nunca he tenido una fantasía extraña.',
'Yo nunca he fingido estar enamorado.',
'Yo nunca he cancelado planes por sexo.',
'Yo nunca he tenido sexo de una sola noche.',
'Yo nunca he usado juguetes sexuales.',
'Yo nunca he tenido sexo bajo alcohol.',
'Yo nunca he besado solo por una apuesta.',
'Yo nunca he tenido un sueño húmedo con alguien de aquí.'
 ]},

 {name:'NO LO DIGAS', color:'#003366', cards:[
'Pasa un mensaje coqueto en secreto a alguien.',
'Hablen en voz baja y sensual una ronda.',
'Solo puedes decir paso una vez.',
'Nadie habla sin tocar a alguien.',
'Contacto visual largo con alguien al azar.',
'Cada persona recibe un apodo sexy.',
'Todos se sientan más juntos.',
'Cambio de lugar en cada turno.',
'Diez segundos solo miradas.',
'Describe coquetamente a alguien por un minuto.'
 ]},

 {name:'EVENTO', color:'#ffaa00', cards:[
'Nadie puede negarse a un beso hasta otro evento.',
'Cambio de asientos.',
'Todos dicen a quién besarían.',
'Ronda de besos en la mejilla.',
'Todos cuentan su primera experiencia romántica.',
'Besos grupales en cachete.',
'Giran botella.',
'El más tímido da un beso.',
'El más atrevido crea un reto.',
'Intercambio de prendas.'
 ]},

 {name:'REGLAS', color:'#ff66aa', cards:[
'Todo lo que diga la carta se acepta.',
'No se puede hablar.',
'Se juega por turnos.',
'Nadie puede negarse.',
'Todo es consensuado.'
 ]}
];

let originalDecks = JSON.parse(JSON.stringify(categories));

function nextCard(){
 let available = categories.filter(c=>c.cards.length>0);

 if(available.length===0){
  categories = JSON.parse(JSON.stringify(originalDecks));
  activeRule=null;
  available = categories.filter(c=>c.cards.length>0);
 }

 let cat = available[Math.floor(Math.random()*available.length)];
 let index = Math.floor(Math.random()*cat.cards.length);
 let text = cat.cards.splice(index,1)[0];

 if(cat.name==='REGLAS'){ activeRule=text; }

 document.getElementById('category').innerText=cat.name;

 if(activeRule && cat.name!=='REGLAS'){
  document.getElementById('text').innerText =
   text + '\n\n⚠️ Regla activa:\n' + activeRule;
 }else{
  document.getElementById('text').innerText=text;
 }

 document.getElementById('card').style.background=cat.color;
}
</script>
</body>
</html>
