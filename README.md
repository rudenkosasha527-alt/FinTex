# FinTex

<!DOCTYPE html>

<html lang="ru">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1">

<title>FinTex</title>

<style>

*{box-sizing:border-box}

body{

 margin:0;background:#081225;color:white;

 font-family:Arial,sans-serif;text-align:center;

 padding:20px;

}

h1{font-size:34px;color:#42e5d2}

.card{

 background:#142640;border-radius:20px;

 padding:20px;margin:15px auto;

 max-width:420px;

}

.balance{font-size:38px;font-weight:bold}

.coin{

 width:190px;height:190px;border-radius:50%;

 border:5px solid #42e5d2;

 background:radial-gradient(circle,#9ffff0,#078b91);

 color:#063447;font-size:35px;font-weight:bold;

 margin:25px auto;cursor:pointer;

 touch-action:manipulation;

 box-shadow:0 0 35px #42e5d288;

}

.coin:active{transform:scale(.92)}

button{

 border:0;border-radius:12px;padding:13px;

 background:#42e5d2;color:#062b35;

 font-weight:bold;cursor:pointer;margin:5px;

}

button:disabled{opacity:.4}

.muted{color:#9aacc8}

nav{display:flex;flex-wrap:wrap;justify-content:center}

section{display:none}

section.active{display:block}

</style>

</head>

<body>

<h1>FinTex</h1>

<p class="muted">TAP TO EARN</p>

<div class="card">

 <p class="muted">ТВОЙ БАЛАНС</p>

 <div class="balance" id="balance">0</div>

 <p>FNT</p>

 <button class="coin" id="tap">FNT</button>

 <p id="gain">Нажимай на монету!</p>

 <p>⚡ Энергия: <span id="energy">1000</span></p>

 <p>Доход за тап: +<span id="power">1</span> FNT</p>

</div>

<nav>

 <button onclick="show('home')">Главная</button>

 <button onclick="show('upgrade')">Прокачка</button>

 <button onclick="show('bonus')">Бонусы</button>

 <button onclick="show('friends')">Друзья</button>

</nav>

<section id="home" class="active card">

 <h3>Добро пожаловать в FinTex!</h3>

 <p>Собирай монеты и развивай свой уровень.</p>

</section>

<section id="upgrade" class="card">

 <h3>Улучшения</h3>

 <p>Сила тапа: <span id="level">1</span></p>

 <button onclick="upgrade()">Улучшить за <span id="price">50</span> FNT</button>

</section>

<section id="bonus" class="card">

 <h3>Ежедневный бонус</h3>

 <button onclick="daily()">Получить 100 FNT</button>

</section>

<section id="friends" class="card">

 <h3>Приглашай друзей</h3>

 <p>Твой код: FINTEX-PLAYER</p>

 <button onclick="copyCode()">Копировать код</button>

 <p class="muted">Реферальная система пока демонстрационная.</p>

</section>

<script>

let balance=0,power=1,energy=1000;

let level=1,price=50;

let last=Date.now();

let dailyDate=localStorage.getItem("fintexDaily")||"";

try{

 const saved=JSON.parse(localStorage.getItem("fintexSave"));

 if(saved){

  balance=saved.balance;

  power=saved.power;

  energy=saved.energy;

  level=saved.level;

  price=saved.price;

  last=Date.now();

 }

}catch(e){}

function save(){

 localStorage.setItem("fintexSave",JSON.stringify({

 balance,power,energy,level,price

 }));

}

function render(){

 document.getElementById("balance").textContent=Math.floor(balance);

 document.getElementById("energy").textContent=Math.floor(energy);

 document.getElementById("power").textContent=power;

 document.getElementById("level").textContent=level;

 document.getElementById("price").textContent=price;

 save();

}

document.getElementById("tap").addEventListener("click",()=>{

 if(energy<1){

  document.getElementById("gain").textContent="Нет энергии!";

  return;

 }

 energy--;

 balance+=power;

 document.getElementById("gain").textContent="+"+power+" FNT";

 render();

});

function upgrade(){

 if(balance<price)return alert("Недостаточно FNT");

 balance-=price;

 power++;

 level++;

 price=Math.floor(price*1.7);

 render();

}

function daily(){

 const today=new Date().toLocaleDateString("en-CA");

 if(dailyDate===today)return alert("Бонус уже получен!");

 balance+=100;

 dailyDate=today;

 localStorage.setItem("fintexDaily",today);

 render();

 alert("Получено 100 FNT!");

}

function show(id){

 document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));

 document.getElementById(id).classList.add("active");

}

function copyCode(){

 navigator.clipboard?.writeText("FINTEX-PLAYER");

 alert("Код: FINTEX-PLAYER");

}

setInterval(()=>{

 energy=Math.min(1000,energy+2);