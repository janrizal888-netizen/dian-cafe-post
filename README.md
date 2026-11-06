# dian-cafe-post index.html
<!DOCTYPE html>
<html lang="ms">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>DIAN CAFE POS</title>
<style>
body{font-family:Arial;background:#f4f0e6;margin:15px;color:#292015}
h1{text-align:center;color:#76551b}
button{padding:12px;margin:5px;border:0;border-radius:7px;background:#76551b;color:white}
input,select{padding:12px;margin:5px;width:90%}
.card{background:white;padding:15px;margin:10px 0;border-radius:10px}
</style>
</head>
<body>
<h1>DIAN CAFE POS</h1>
<p style="text-align:center">Sistem Jualan Offline</p>

<div class="card">
<h3>Tambah Menu</h3>
<input id="nama" placeholder="Nama menu">
<input id="harga" type="number" placeholder="Harga RM">
<button onclick="tambah()">Simpan Menu</button>
</div>

<div class="card">
<h3>Senarai Menu</h3>
<div id="menu"></div>
</div>

<div class="card">
<h3>Jumlah Jualan</h3>
<h2 id="jumlah">RM0.00</h2>
<div id="cart"></div>
<button onclick="bayar('Tunai')">Bayar Tunai</button>
<button onclick="bayar('DuitNow QR')">Bayar QR</button>
</div>

<script>
let menu=JSON.parse(localStorage.menu||"[]");
let cart=[];
let jumlah=0;

function papar(){
 document.getElementById("menu").innerHTML=menu.map((m,i)=>
 `<button onclick="pilih(${i})">${m.nama} RM${m.harga.toFixed(2)}</button>`
 ).join("");
}
function tambah(){
 let nama=document.getElementById("nama").value;
 let harga=Number(document.getElementById("harga").value);
 if(!nama||harga<=0)return alert("Isi nama dan harga menu");
 menu.push({nama,harga});
 localStorage.menu=JSON.stringify(menu);
 document.getElementById("nama").value="";
 document.getElementById("harga").value="";
 papar();
}
function pilih(i){
 cart.push(menu[i]);
 jumlah+=menu[i].harga;
 paparCart();
}
function paparCart(){
 document.getElementById("cart").innerHTML=
 cart.map(x=>x.nama+" RM"+x.harga.toFixed(2)).join("<br>");
 document.getElementById("jumlah").innerText="RM"+jumlah.toFixed(2);
}
function bayar(cara){
 if(!cart.length)return alert("Pilih menu dahulu");
 let jualan=JSON.parse(localStorage.jualan||"[]");
 jualan.push({tarikh:new Date().toLocaleString(),cara,jumlah,cart});
 localStorage.jualan=JSON.stringify(jualan);
 alert("Jualan berjaya direkodkan!");
 cart=[];jumlah=0;paparCart();
}
papar();
</script>
</body>
</html>
