<!DOCTYPE html><html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Lucky Bee POS 4.0</title><style>

body{
font-family:Arial;
margin:0;
background:#eef1f7;
}

header{
background:#111827;
color:white;
padding:18px;
text-align:center;
font-size:24px;
font-weight:bold;
}

.container{
padding:15px;
}

.card{
background:white;
padding:15px;
border-radius:10px;
margin-bottom:15px;
box-shadow:0 4px 8px rgba(0,0,0,0.15);
}

button{
cursor:pointer;
}

input{
width:95%;
padding:10px;
margin:5px;
border:1px solid #ccc;
border-radius:6px;
}

.dashboard{
display:grid;
grid-template-columns:repeat(2,1fr);
gap:10px;
}

.dashboard button{
padding:18px;
font-size:16px;
border:none;
border-radius:8px;
color:white;
font-weight:bold;
}

.inventoryBtn{background:#2563eb}
.checkoutBtn{background:#16a34a}
.salesBtn{background:#f59e0b}
.logoutBtn{background:#dc2626}

.products{
display:grid;
grid-template-columns:repeat(2,1fr);
gap:10px;
margin-top:10px;
}

.productBtn{
background:#2563eb;
color:white;
border:none;
padding:15px;
border-radius:8px;
font-size:15px;
}

.cart{
margin-top:10px;
}

.cartItem{
display:flex;
justify-content:space-between;
padding:6px;
border-bottom:1px solid #ddd;
}

.checkoutButton{
background:#16a34a;
padding:15px;
color:white;
border:none;
border-radius:8px;
width:100%;
font-size:18px;
margin-top:10px;
}

#dashboard,#inventory,#pos{
display:none;
}

</style></head><body><header>🐝 Lucky Bee POS 4.0</header><div class="container"><div id="login" class="card"><h3>Login</h3><input id="username" placeholder="Username">
<input id="password" type="password" placeholder="Password"><button onclick="login()">Login</button>

<p>Admin / 1234<br>Cashier / 1111</p></div><div id="dashboard" class="card"><h3>Dashboard</h3><div class="dashboard"><button class="inventoryBtn" onclick="openInventory()">Inventory</button>

<button class="checkoutBtn" onclick="openPOS()">Start Selling</button>

<button class="salesBtn" onclick="viewSales()">Sales</button>

<button class="logoutBtn" onclick="logout()">Logout</button>

</div></div><div id="inventory" class="card"><h3>Add Product</h3><input id="name" placeholder="Product Name">
<input id="price" placeholder="Price">
<input id="qty" placeholder="Quantity"><button onclick="addProduct()">Add Product</button>

<div id="inventoryList"></div></div><div id="pos" class="card"><h3>Products</h3><div id="productButtons" class="products"></div><h3>Cart</h3><div id="cart" class="cart"></div><h3>Total: K <span id="total">0</span></h3><button class="checkoutButton" onclick="checkout()">CHECKOUT</button>

</div></div><script>

let users={
admin:{pass:"1234"},
cashier:{pass:"1111"}
}

let inventory=JSON.parse(localStorage.getItem("inventory"))||[]
let sales=JSON.parse(localStorage.getItem("sales"))||[]
let cart=[]

function save(){
localStorage.setItem("inventory",JSON.stringify(inventory))
localStorage.setItem("sales",JSON.stringify(sales))
}

function login(){

let u=username.value
let p=password.value

if(users[u] && users[u].pass==p){

login.style.display="none"
dashboard.style.display="block"

loadInventory()
loadProducts()

}else{

alert("Wrong login")

}

}

function logout(){
location.reload()
}

function hideAll(){

inventory.style.display="none"
pos.style.display="none"

}

function openInventory(){

hideAll()
inventory.style.display="block"

}

function openPOS(){

hideAll()
pos.style.display="block"

}

function addProduct(){

let n=name.value
let p=parseFloat(price.value)
let q=parseInt(qty.value)

inventory.push({name:n,price:p,qty:q})

save()

loadInventory()
loadProducts()

}

function loadInventory(){

inventoryList.innerHTML=""

inventory.forEach(p=>{
inventoryList.innerHTML+=p.name+" K"+p.price+" stock:"+p.qty+"<br>"
})

}

function loadProducts(){

productButtons.innerHTML=""

inventory.forEach((p,i)=>{

if(p.qty>0){

let b=document.createElement("button")

b.className="productBtn"

b.innerText=p.name+" K"+p.price

b.onclick=function(){

addToCart(i)

}

productButtons.appendChild(b)

}

})

}

function addToCart(i){

let item=inventory[i]

cart.push({name:item.name,price:item.price,index:i})

renderCart()

}

function renderCart(){

cart.innerHTML=""

let total=0

cart.forEach((c,i)=>{

total+=c.price

cart.innerHTML+=`
<div class="cartItem">
${c.name} K${c.price}
<button onclick="removeItem(${i})">X</button>
</div>
`

})

document.getElementById("total").innerText=total

}

function removeItem(i){

cart.splice(i,1)

renderCart()

}

function checkout(){

if(cart.length==0){

alert("Cart empty")

return

}

let total=0

cart.forEach(c=>{

inventory[c.index].qty--

total+=c.price

})

sales.push({total:total,items:cart.length})

cart=[]

save()

renderCart()

loadProducts()

alert("Sale complete")

}

</script></body>
</html>
