<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Nato Sushi | Delivery Oriental</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Arial;background:#0e0e0e;color:#fff}
header{text-align:center;padding:20px;background:#000}
h1{color:#e50914;margin:0}
h2{margin-top:30px;color:#e50914}
.container{padding:15px}
.item{background:#1c1c1c;padding:10px;border-radius:8px;margin-bottom:10px}
.item button{background:#1db954;border:none;padding:6px 10px;border-radius:5px;color:#fff;margin-top:5px}
.cart{background:#141414;padding:15px;border-radius:10px;margin-top:20px}
input,select{width:100%;padding:8px;margin-top:8px;border-radius:5px;border:none}
.btn{margin-top:15px;padding:15px;background:#25d366;color:#fff;text-align:center;border-radius:10px;font-size:18px;cursor:pointer}
.total{font-weight:bold;font-size:18px;color:#e50914;margin-top:10px}
.qty button{margin:0 5px}
</style>
</head>

<body>

<header>
<h1>Nato Sushi</h1>
<p>Delivery oriental • Pix, dinheiro ou cartão 🍣</p>
</header>

<div class="container" id="menu"></div>

<div class="container cart">
<h3>🛒 Carrinho</h3>
<div id="cartItems"></div>

<label>Nome do cliente</label>
<input id="name">

<label>Endereço completo (com bairro)</label>
<input id="address" oninput="calcDelivery()">

<label>Quantidade de hashis</label>
<input type="number" id="hashi" value="1" min="0">

<label>Forma de pagamento</label>
<select id="payment">
  <option>Pix</option>
  <option>Dinheiro</option>
  <option>Cartão</option>
</select>

<label>
<input type="checkbox" id="tare"> Molho Tarê (+ R$ 6,00)
</label>

<p id="delivery"></p>
<p class="total" id="total">Total: R$ 0,00</p>

<div class="btn" onclick="sendWhatsApp()">Finalizar pedido no WhatsApp</div>
</div>

<script>
const menu=[
{cat:"TEMAKI",items:[
["Temaki Salmão Natural",35],
["Temaki Salmão Filadélfia",38],
["Temaki Skin",35],
["Temaki Atum Natural",35],
["Temaki Atum Picante",38],
["Temaki Califórnia",38],
["Temaki Ebi",38]
]},
{cat:"COMBINADOS",items:[
["Combinado 16 peças",65],
["Combinado 30 peças",85],
["Combinado 50 peças",150],
["Combinado Brazeado 22 peças",80]
]}
];

let cart=[];
let deliveryFee=0;

const menuDiv=document.getElementById("menu");

menu.forEach(sec=>{
 menuDiv.innerHTML+=`<h2>${sec.cat}</h2>`;
 sec.items.forEach(i=>{
  menuDiv.innerHTML+=`
   <div class="item">
    <strong>${i[0]}</strong><br>
    R$ ${i[1].toFixed(2)}
    <br><button onclick="addItem('${i[0]}',${i[1]})">Adicionar</button>
   </div>`;
 });
});

function addItem(name,price){
 let item=cart.find(i=>i.name===name);
 if(item){item.qty++}else{cart.push({name,price,qty:1})}
 renderCart();
}

function changeQty(name,val){
 let item=cart.find(i=>i.name===name);
 item.qty+=val;
 if(item.qty<=0) cart=cart.filter(i=>i.name!==name);
 renderCart();
}

function calcDelivery(){
 const addr=address.value.toLowerCase();
 if(addr.includes("centro")) deliveryFee=5;
 else if(addr.includes("jardim")) deliveryFee=8;
 else if(addr.includes("praia")) deliveryFee=10;
 else deliveryFee=12;

 delivery.innerText="🚚 Taxa de entrega: R$ "+deliveryFee.toFixed(2);
 renderCart();
}

function renderCart(){
 let html="",total=0;
 cart.forEach(i=>{
  html+=`
   ${i.name} — R$ ${i.price.toFixed(2)}  
   <div class="qty">
    <button onclick="changeQty('${i.name}',-1)">−</button>
    ${i.qty}
    <button onclick="changeQty('${i.name}',1)">+</button>
   </div><br>`;
  total+=i.price*i.qty;
 });

 if(tare.checked) total+=6;
 total+=deliveryFee;

 cartItems.innerHTML=html;
 totalElem.innerText="Total: R$ "+total.toFixed(2);
}

tare.addEventListener("change",renderCart);

function sendWhatsApp(){
 let msg="🍣 *PEDIDO – NATO SUSHI* 🍣%0A%0A";
 msg+="👤 "+name.value+"%0A";
 msg+="📍 "+address.value+"%0A";
 msg+="🥢 Hashis: "+hashi.value+"%0A";
 msg+="💳 Pagamento: "+payment.value+"%0A%0A";
 msg+="🛒 *Itens:*%0A";

 let total=0;
 cart.forEach(i=>{
  msg+=`- ${i.name} x${i.qty}%0A`;
  total+=i.price*i.qty;
 });

 if(tare.checked){msg+="- Molho Tarê (+ R$ 6,00)%0A";total+=6}
 msg+=`🚚 Taxa: R$ ${deliveryFee.toFixed(2)}%0A`;
 total+=deliveryFee;

 msg+=`%0A💰 *Total:* R$ ${total.toFixed(2)}`;

 window.open("https://wa.me/5527995125034?text="+msg,"_blank");
}
</script>

</body>
</html>
