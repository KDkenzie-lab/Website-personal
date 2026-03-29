<!doctype html>
<html lang="id"> 
 <head> 
  <meta charset="UTF-8"> 
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
  <title>KENZ STORE</title> 
  <style>
*{margin:0;padding:0;box-sizing:border-box;}

body{
font-family:Arial, sans-serif;
color:white;
min-height:100vh;
background:
linear-gradient(rgba(5,10,30,.55), rgba(5,10,30,.85)),
url("https://files.catbox.moe/1n4d4s.jpg");
background-size:cover;
background-position:center;
background-repeat:no-repeat;
background-attachment:fixed;
}

.hero{
text-align:center;
padding:35px 15px 20px;
}

.hero h1{
font-size:38px;
font-weight:900;
color:#60a5fa;
text-shadow:0 0 20px rgba(96,165,250,.7);
}

.hero p{
color:#dbeafe;
margin-top:6px;
}

.tabs{
display:flex;
gap:10px;
overflow-x:auto;
padding:10px 15px;
}

.tabs button{
border:none;
padding:10px 18px;
border-radius:15px;
background:rgba(2,6,23,.88);
color:#fff;
font-weight:bold;
cursor:pointer;
box-shadow:0 0 12px rgba(59,130,246,.45);
flex:0 0 auto;
}

.tabs button.active{
background:#3b82f6;
}

.grid{
padding:20px;
display:grid;
grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
gap:18px;
}

.card{
background:rgba(2,6,23,.82);
border-radius:18px;
padding:18px;
box-shadow:0 0 20px rgba(59,130,246,.35);
backdrop-filter:blur(4px);
}

.badge{
display:inline-block;
background:linear-gradient(45deg,#facc15,#f97316);
color:#111;
font-weight:bold;
font-size:11px;
padding:5px 10px;
border-radius:20px;
margin-bottom:8px;
box-shadow:0 0 10px rgba(250,204,21,.8);
letter-spacing:1px;
}

.card h3{
font-size:15px;
margin-bottom:6px;
}

.price{
font-size:18px;
font-weight:bold;
color:#60a5fa;
margin-bottom:8px;
}

.add{
width:100%;
padding:10px;
border:none;
border-radius:10px;
background:#3b82f6;
color:#fff;
font-weight:bold;
cursor:pointer;
}

.cart{
margin:20px;
background:rgba(2,6,23,.88);
padding:20px;
border-radius:20px;
box-shadow:0 0 20px rgba(59,130,246,.35);
}

.total{
margin:12px 0;
font-weight:bold;
}

.checkout{
width:100%;
padding:14px;
border:none;
border-radius:14px;
background:#3b82f6;
color:white;
font-weight:bold;
cursor:pointer;
}
</style> 
 </head> 
 <body> 
  <div class="hero"> 
   <h1>🔰KenzXiterz🔰</h1>
   <h1>DANA KURANG BISA NEGO</h1> 
   <p>Pilih produk lalu checkout via WhatsApp</p> 
  </div> 
  <div class="tabs" id="tabs"></div> 
  <div class="grid" id="grid"></div> 
  <div class="cart"> 
   <h2>Keranjang</h2> 
   <div id="cartList"></div> 
   <div class="total" id="total">
    Total: Rp 0
   </div> <label><input type="radio" name="pay" value="DANA"> DANA</label>
   <br> <label><input type="radio" name="pay" value="GOPAY"> GOPAY</label>
   <br> <label><input type="radio" name="pay" value="QRIS"> QRIS</label>
   <br>
   <br> <button class="checkout" onclick="checkout()">Checkout WhatsApp</button> 
  </div> 
  <script>
const phone="6285135658021"

const data = {
  "HEADLOCK":[
    {name:"HEADLOCK 30%",price:20000},
    {name:"HEADLOCK 40%",price:30000},
    {name:"HEADLOCK 50%",price:40000},
    {name:"HEADLOCK 65%",price:45000},
    {name:"HEADLOCK 80%",price:55000},
      ],
  "EDT fanatical":[
    {name:"EDT fanatical V1",price:10000},
    {name:"EDT fanatical V2",price:15000},
    {name:"EDT fanatical V3",price:25000},
    {name:"EDT fanatical V4",price:35000},
    {name:"EDT fanatical V5",price:45000},
    {name:"EDT VENUZ",price:60000},
    {name:"ETD EXTRIM",price:85000}
          ],
  "DRIP CLINT NO ROOT APKMOD":[
    {name:"DRIP CLINT 1HARI",price:30000},
    {name:"DRIP CLINT 3HARI",price:60000},
    {name:"DRIP CLINT 7HARI",price:100000},
    {name:"DRIP CLINT 15HARI",price:155000},
    {name:"DRIP CLINT 30HARI",price:200000}
              ],
  "BADAN HS NON ROOT":[
    {name:"BADAN HS 50%",price:30000},
    {name:"BADAN HS 100%",price:50000}  
  ]
}

let active=Object.keys(data)[0]
let cart=[]

function rupiah(n){
  return "Rp "+n.toLocaleString("id-ID")
}

function renderTabs(){
  const tabs=document.getElementById("tabs")
  tabs.innerHTML=""

  Object.keys(data).forEach(cat=>{
    let btn=document.createElement("button")
    btn.textContent=cat
    btn.className=(cat==active?"active":"")
    btn.onclick=()=>{
      active=cat
      renderTabs()
      renderProducts()
    }
    tabs.appendChild(btn)
  })
}

function renderProducts(){
  const grid=document.getElementById("grid")
  grid.innerHTML=""

  data[active].forEach((p,i)=>{
    let card=document.createElement("div")
    card.className="card"

    card.innerHTML=`
      ${p.badge ? '<div class="badge">🔥 BEST SELLER</div>' : ''}
      <h3>${p.name}</h3>
      <div class="price">${rupiah(p.price)}</div>
      <button class="add" onclick="add(${i})">Tambah</button>
    `

    grid.appendChild(card)
  })
}

function add(i){
  cart.push(data[active][i])
  renderCart()
}

function renderCart(){
  let list=document.getElementById("cartList")
  list.innerHTML=""

  if(cart.length===0){
    list.innerHTML="<div>Belum ada produk.</div>"
  }else{
    cart.forEach(item=>{
      let d=document.createElement("div")
      d.innerHTML=item.name+" - "+rupiah(item.price)
      list.appendChild(d)
    })
  }

  let total=cart.reduce((a,b)=>a+b.price,0)
  document.getElementById("total").innerText="Total: "+rupiah(total)
}

function checkout(){
  if(cart.length==0){
    alert("Keranjang kosong")
    return
  }

  let pay=document.querySelector('input[name="pay"]:checked')
  if(!pay){
    alert("Pilih pembayaran dulu")
    return
  }

  let msg="Halo bang Kenz saya mau order:%0A"

  cart.forEach((item,i)=>{
    msg+=`${i+1}. ${item.name} - ${rupiah(item.price)}%0A`
  })

  let total=cart.reduce((a,b)=>a+b.price,0)

  msg+=`%0ATotal: ${rupiah(total)}%0A`
  msg+=`Pembayaran: ${pay.value}`

  window.open(`https://wa.me/${phone}?text=${msg}`, "_blank")
}

renderTabs()
renderProducts()
renderCart()
</script> 
 </body>
</html>
