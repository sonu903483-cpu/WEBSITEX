<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Local Shop — Fresh Finds, Best Prices</title>

<!-- EmailJS SDK: lets this page send the order email straight from the
     browser, no backend needed. See the SETUP NOTE near the bottom of the
     <script> for the two-minute config step. -->
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>

<style>
  :root{
    --green-deep:   #1F3A2E;
    --green-mid:    #2C5240;
    --marigold:     #E8A33D;
    --marigold-dark:#C6811E;
    --paper:        #FAF6EC;
    --paper-dim:    #F1EAD8;
    --ink:          #2B2620;
    --ink-soft:     #6B6255;
    --line:         #E1D6BC;
    --radius:       14px;
    --shadow:       0 10px 30px rgba(31,58,46,0.14);
  }

  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--paper);
    color:var(--ink);
    font-family:'Segoe UI', -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Arial, sans-serif;
    -webkit-font-smoothing:antialiased;
  }
  h1,h2,h3{
    font-family:Georgia, 'Times New Roman', serif;
    margin:0;
  }
  img{max-width:100%;display:block;}
  a{color:inherit;}
  button{font-family:inherit;cursor:pointer;}

  /* ---------- Top bar ---------- */
  .topbar{
    position:sticky; top:0; z-index:40;
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 20px;
    background:var(--paper);
    border-bottom:1px solid var(--line);
  }
  .brand{
    display:flex; align-items:center; gap:10px;
    font-family:Georgia, serif; font-weight:700; font-size:1.15rem;
    letter-spacing:.3px;
  }
  .brand .stamp{
    width:34px;height:34px;border-radius:50%;
    background:var(--green-deep); color:var(--marigold);
    display:flex;align-items:center;justify-content:center;
    font-size:.85rem; font-weight:800; border:2px solid var(--marigold);
  }
  .cart-btn{
    position:relative;
    background:var(--green-deep);
    color:var(--paper);
    border:none; border-radius:999px;
    width:46px;height:46px;
    display:flex;align-items:center;justify-content:center;
    box-shadow:var(--shadow);
  }
  .cart-btn svg{width:22px;height:22px;}
  .cart-count{
    position:absolute; top:-4px; right:-4px;
    background:var(--marigold); color:var(--green-deep);
    font-size:.7rem; font-weight:800;
    min-width:19px; height:19px; border-radius:999px;
    display:flex;align-items:center;justify-content:center;
    padding:0 4px;
    border:2px solid var(--paper);
  }

  /* ---------- Hero ---------- */
  .hero{
    position:relative;
    min-height:78vh;
    display:flex; align-items:flex-end;
    background:#0f2018 url('https://i.ibb.co/5x2ZCWZt/premium-vector-1727107302518-635256868dfd.png') center/cover no-repeat;
  }
  .hero::before{
    content:"";
    position:absolute; inset:0;
    background:linear-gradient(180deg, rgba(15,32,24,.35) 0%, rgba(15,32,24,.55) 55%, rgba(15,32,24,.92) 100%);
  }
  .hero-inner{
    position:relative; z-index:1;
    width:100%;
    padding:40px 22px 46px;
    color:var(--paper);
    max-width:640px;
  }
  .hero-eyebrow{
    display:inline-block;
    background:var(--marigold);
    color:var(--green-deep);
    font-weight:800; font-size:.72rem;
    letter-spacing:1.5px; text-transform:uppercase;
    padding:6px 12px; border-radius:999px;
    margin-bottom:16px;
  }
  .hero h1{
    font-size:clamp(2.1rem, 8vw, 3.4rem);
    line-height:1.05;
    margin-bottom:14px;
    text-shadow:0 2px 18px rgba(0,0,0,.35);
  }
  .hero p.tagline{
    font-size:1.1rem;
    color:#EDE6D3;
    margin-bottom:26px;
    max-width:420px;
  }
  .btn-primary{
    display:inline-flex; align-items:center; gap:8px;
    background:var(--marigold);
    color:var(--green-deep);
    font-weight:800; font-size:1rem;
    border:none; border-radius:999px;
    padding:15px 30px;
    box-shadow:0 8px 20px rgba(232,163,61,.35);
    transition:transform .15s ease;
  }
  .btn-primary:active{transform:scale(.96);}

  /* ---------- Section headings ---------- */
  .section-head{
    padding:38px 20px 18px;
    text-align:left;
  }
  .section-head .kicker{
    color:var(--marigold-dark);
    font-weight:800; font-size:.75rem;
    letter-spacing:1.5px; text-transform:uppercase;
  }
  .section-head h2{
    font-size:1.7rem; margin-top:6px;
  }
  .section-head p{color:var(--ink-soft); margin-top:6px; font-size:.95rem;}

  /* ---------- Carousel ---------- */
  .carousel{
    display:flex; gap:16px;
    overflow-x:auto;
    padding:6px 20px 26px;
    scroll-snap-type:x mandatory;
    -webkit-overflow-scrolling:touch;
  }
  .carousel::-webkit-scrollbar{height:6px;}
  .carousel::-webkit-scrollbar-thumb{background:var(--line); border-radius:10px;}

  .card{
    scroll-snap-align:start;
    flex:0 0 220px;
    background:#fff;
    border-radius:var(--radius);
    box-shadow:var(--shadow);
    overflow:hidden;
    display:flex; flex-direction:column;
    position:relative;
  }
  .card .thumb{
    width:100%; height:150px; object-fit:cover;
    background:var(--paper-dim);
  }
  .tag{
    position:absolute; top:10px; left:10px;
    background:var(--green-deep); color:var(--marigold);
    font-size:.7rem; font-weight:800;
    padding:4px 9px; border-radius:6px;
    letter-spacing:.4px;
  }
  .card-body{padding:12px 14px 14px; display:flex; flex-direction:column; gap:6px; flex:1;}
  .card-body h3{font-size:1rem; font-family:inherit; font-weight:700;}
  .card-body .price{
    font-weight:800; color:var(--green-deep); font-size:1.05rem;
  }
  .card-body .old-price{
    color:var(--ink-soft); text-decoration:line-through; font-size:.85rem; margin-left:6px; font-weight:400;
  }
  .add-btn{
    margin-top:auto;
    background:var(--green-deep); color:var(--paper);
    border:none; border-radius:999px;
    padding:10px; font-weight:700; font-size:.88rem;
    display:flex; align-items:center; justify-content:center; gap:6px;
    transition:background .15s ease;
  }
  .add-btn:active{background:var(--green-mid);}
  .add-btn.added{background:var(--marigold); color:var(--green-deep);}

  .scroll-hint{
    text-align:center; color:var(--ink-soft); font-size:.78rem;
    padding-bottom:10px;
  }

  /* ---------- About / trust strip ---------- */
  .strip{
    background:var(--green-deep); color:var(--paper);
    display:flex; flex-wrap:wrap; justify-content:space-around;
    text-align:center; padding:26px 16px; gap:16px;
  }
  .strip div{flex:1; min-width:110px;}
  .strip div strong{display:block; font-size:1.1rem; color:var(--marigold);}
  .strip div span{font-size:.78rem; color:#D9D1BC;}

  /* ---------- Footer ---------- */
  footer{
    text-align:center; padding:30px 20px 100px;
    color:var(--ink-soft); font-size:.85rem;
  }
  footer .stamp2{
    width:40px;height:40px;border-radius:50%;
    background:var(--green-deep); color:var(--marigold);
    display:flex;align-items:center;justify-content:center;
    font-weight:800; margin:0 auto 10px; border:2px solid var(--marigold);
  }

  /* ---------- Floating buttons ---------- */
  .fab-wa{
    position:fixed; left:18px; bottom:18px; z-index:50;
    width:56px; height:56px; border-radius:50%;
    background:#25D366; color:#fff;
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 8px 20px rgba(37,211,102,.45);
    text-decoration:none;
  }
  .fab-wa svg{width:28px;height:28px;}

  /* ---------- Cart drawer ---------- */
  .overlay{
    position:fixed; inset:0; background:rgba(15,32,24,.5);
    z-index:60; opacity:0; pointer-events:none; transition:opacity .25s ease;
  }
  .overlay.open{opacity:1; pointer-events:auto;}

  .drawer{
    position:fixed; top:0; right:0; height:100%;
    width:100%; max-width:420px;
    background:var(--paper);
    z-index:61;
    transform:translateX(100%);
    transition:transform .3s ease;
    display:flex; flex-direction:column;
    box-shadow:-10px 0 30px rgba(0,0,0,.2);
  }
  .drawer.open{transform:translateX(0);}
  .drawer-head{
    display:flex; align-items:center; justify-content:space-between;
    padding:18px 20px; border-bottom:1px solid var(--line);
    background:var(--green-deep); color:var(--paper);
  }
  .drawer-head h2{font-size:1.2rem;}
  .close-btn{
    background:none; border:none; color:var(--paper);
    font-size:1.4rem; line-height:1; padding:4px;
  }
  .drawer-body{flex:1; overflow-y:auto; padding:16px 20px;}

  .cart-item{
    display:flex; gap:12px; padding:12px 0;
    border-bottom:1px dashed var(--line);
    align-items:center;
  }
  .cart-item img{width:56px;height:56px;border-radius:10px;object-fit:cover;}
  .cart-item .ci-info{flex:1;}
  .cart-item .ci-info h4{font-size:.92rem; margin:0 0 4px;}
  .cart-item .ci-info .p{color:var(--ink-soft); font-size:.82rem;}
  .qty-ctrl{display:flex; align-items:center; gap:8px;}
  .qty-ctrl button{
    width:26px;height:26px;border-radius:50%;border:1px solid var(--line);
    background:#fff; font-weight:700; display:flex;align-items:center;justify-content:center;
  }
  .remove-btn{
    background:none;border:none;color:#B24A3B;font-size:.78rem;font-weight:700;
    margin-left:8px; text-decoration:underline;
  }
  .empty-msg{color:var(--ink-soft); text-align:center; padding:40px 10px; font-size:.9rem;}

  .cart-total{
    display:flex; justify-content:space-between; align-items:center;
    padding:14px 0 6px; font-weight:800; font-size:1.05rem;
    border-top:1px solid var(--line); margin-top:6px;
  }

  form#order-form{margin-top:18px; display:flex; flex-direction:column; gap:12px;}
  form#order-form label{font-size:.82rem; font-weight:700; color:var(--ink); margin-bottom:-6px;}
  form#order-form input, form#order-form textarea{
    border:1px solid var(--line); border-radius:10px;
    padding:11px 12px; font-size:.95rem; background:#fff; color:var(--ink);
    font-family:inherit;
  }
  form#order-form input:focus, form#order-form textarea:focus{
    outline:2px solid var(--marigold); border-color:var(--marigold);
  }
  .send-btn{
    background:var(--marigold); color:var(--green-deep);
    border:none; border-radius:999px; padding:14px; font-weight:800; font-size:1rem;
    margin-top:4px;
  }
  .send-btn:disabled{opacity:.6;}
  .fallback-note{font-size:.78rem; color:var(--ink-soft); text-align:center; margin-top:2px;}
  .fallback-note a{color:var(--green-deep); font-weight:700; text-decoration:underline;}
  .status-msg{
    font-size:.85rem; text-align:center; padding:10px; border-radius:10px; margin-top:4px;
    display:none;
  }
  .status-msg.show{display:block;}
  .status-msg.ok{background:#E4F1E9; color:#1F5A3C;}
  .status-msg.err{background:#FBE8E4; color:#8A2E1C;}

  @media (min-width:600px){
    .drawer{max-width:400px;}
  }

  @media (prefers-reduced-motion: reduce){
    *{transition:none !important; scroll-behavior:auto !important;}
  }
</style>
</head>
<body>

<!-- ================= TOP BAR ================= -->
<header class="topbar">
  <div class="brand"><span class="stamp">TLS</span> the local shop</div>
  <button class="cart-btn" id="openCartBtn" aria-label="Open cart">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"/></svg>
    <span class="cart-count" id="cartCount">0</span>
  </button>
</header>

<!-- ================= HERO ================= -->
<section class="hero">
  <div class="hero-inner">
    <span class="hero-eyebrow">Your neighborhood, delivered</span>
    <h1>the local shop</h1>
    <p class="tagline">Fresh Finds, Best Prices — hand-picked from the store around the corner, straight to your door.</p>
    <a href="#products" class="btn-primary">
      Shop Now
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 5v14M19 12l-7 7-7-7"/></svg>
    </a>
  </div>
</section>

<!-- ================= PRODUCTS ================= -->
<section id="products">
  <div class="section-head">
    <span class="kicker">On the shelf today</span>
    <h2>Fresh picks, priced right</h2>
    <p>Swipe through — tap "Add to cart" to build your order.</p>
  </div>

  <div class="carousel" id="carousel"></div>
  <p class="scroll-hint">← swipe to see more →</p>
</section>

<!-- ================= TRUST STRIP ================= -->
<div class="strip">
  <div><strong>Same-day</strong><span>Local delivery</span></div>
  <div><strong>100%</strong><span>Freshness checked</span></div>
  <div><strong>No fuss</strong><span>Order on WhatsApp too</span></div> 
</div>

<footer>
  <div class="stamp2">TLS</div>
  <p><strong>the local shop</strong> — Fresh Finds, Best Prices</p>
  <p>Questions? Email <a href="mailto:siddhantkumars9fe@gmail.com">siddhantkumars9fe@gmail.com</a> or WhatsApp us below.</p>
</footer>

<!-- ================= FLOATING BUTTONS ================= -->
<a class="fab-wa" href="https://wa.me/9716067578" target="_blank" rel="noopener" aria-label="Chat on WhatsApp">
  <svg viewBox="0 0 32 32" fill="currentColor"><path d="M16.001 3C9.373 3 4 8.373 4 15c0 2.386.699 4.61 1.902 6.478L4 29l7.723-1.855A11.94 11.94 0 0 0 16.001 27C22.629 27 28 21.627 28 15S22.629 3 16.001 3zm0 21.8a9.77 9.77 0 0 1-4.98-1.36l-.357-.213-4.583 1.1 1.122-4.464-.234-.367A9.76 9.76 0 0 1 6.2 15c0-5.404 4.397-9.8 9.801-9.8 5.403 0 9.8 4.396 9.8 9.8 0 5.403-4.397 9.8-9.8 9.8zm5.36-7.34c-.294-.147-1.738-.858-2.008-.955-.27-.098-.466-.147-.663.147-.196.294-.76.955-.932 1.152-.171.196-.343.221-.637.074-.294-.147-1.243-.458-2.367-1.461-.875-.78-1.466-1.744-1.638-2.038-.171-.294-.018-.453.129-.6.132-.132.294-.343.441-.514.147-.171.196-.294.294-.49.098-.196.049-.368-.025-.515-.074-.147-.663-1.6-.909-2.19-.239-.575-.482-.497-.663-.506-.171-.008-.368-.01-.564-.01-.196 0-.515.074-.785.368-.27.294-1.03 1.007-1.03 2.456 0 1.45 1.055 2.85 1.202 3.046.147.196 2.077 3.172 5.034 4.448.703.303 1.252.484 1.68.62.706.225 1.348.193 1.856.117.566-.085 1.738-.71 1.983-1.395.245-.686.245-1.273.171-1.395-.073-.123-.269-.196-.564-.343z"/></svg>
</a>

<!-- ================= CART OVERLAY + DRAWER ================= -->
<div class="overlay" id="overlay"></div>
<aside class="drawer" id="drawer" aria-label="Shopping cart">
  <div class="drawer-head">
    <h2>Your Cart</h2>
    <button class="close-btn" id="closeCartBtn" aria-label="Close cart">&times;</button>
  </div>
  <div class="drawer-body">
    <div id="cartItems"></div>
    <div class="cart-total" id="cartTotalRow" style="display:none;">
      <span>Total</span>
      <span id="cartTotal">₹0</span>
    </div>

    <form id="order-form">
      <label for="custName">Your name</label>
      <input type="text" id="custName" name="custName" placeholder="e.g. Priya Sharma" required>

      <label for="custPhone">Phone number</label>
      <input type="tel" id="custPhone" name="custPhone" placeholder="e.g. 98765 43210" required>

      <label for="custAddress">Delivery address (optional)</label>
      <textarea id="custAddress" name="custAddress" rows="2" placeholder="House no., street, area"></textarea>

      <button type="submit" class="send-btn" id="sendBtn">Send Order</button>
      <p class="fallback-note">Having trouble? <a href="#" id="emailFallback">Send this order by email instead</a></p>
      <div class="status-msg" id="statusMsg"></div>
    </form>
  </div>
</aside>

<script>
(function(){

  /* ---------------------------------------------------------
     1. PRODUCT DATA — edit freely: name, price, image
  --------------------------------------------------------- */
  const PRODUCTS = [
    {
      id: 'p1',
      name: 'Fresh Veggie Basket',
      price: 199,
      oldPrice: 249,
      tag: 'Best Seller',
      img: 'https://i.ibb.co/GQHYWQJZ/pexels-sasi-tha-18739741.jpg'
    },
    {
      id: 'p2',
      name: 'Kitchen Storage Jar',
      price: 349,
      oldPrice: null,
      tag: 'New',
      img: 'https://i.ibb.co/9mYJgMZ6/61d3l0l9nf-L.jpg'
    },
    {
      id: 'p3',
      name: 'Everyday Combo Pack',
      price: 499,
      oldPrice: 599,
      tag: 'Save 17%',
      img: 'https://i.ibb.co/cGjZz4v/Artboard8.png'
    },
    {
      id: 'p4',
      name: 'Daily Essentials Pack',
      price: 149,
      oldPrice: null,
      tag: 'Value Pick',
      img: 'https://i.ibb.co/TBcdWf67/images.jpg'
    },
    {
      id: 'p5',
      name: 'Seasonal Special',
      price: 249,
      oldPrice: 279,
      tag: 'Limited',
      img: 'https://i.ibb.co/XZ9sHrdT/pexels-photo-15258905.png'
    }
  ];

  const OWNER_EMAIL = 'siddhantkumars9fe@gmail.com';

  /* ---------------------------------------------------------
     2. RENDER CAROUSEL
  --------------------------------------------------------- */
  const carousel = document.getElementById('carousel');
  carousel.innerHTML = PRODUCTS.map(p => `
    <div class="card">
      <span class="tag">${p.tag}</span>
      <img class="thumb" src="${p.img}" alt="${p.name}" loading="lazy">
      <div class="card-body">
        <h3>${p.name}</h3>
        <div class="price">₹${p.price}${p.oldPrice ? `<span class="old-price">₹${p.oldPrice}</span>` : ''}</div>
        <button class="add-btn" data-id="${p.id}">Add to cart</button>
      </div>
    </div>
  `).join('');

  /* ---------------------------------------------------------
     3. CART STATE (in-memory, no backend / no localStorage)
  --------------------------------------------------------- */
  let cart = {}; // { productId: qty }

  function addToCart(id){
    cart[id] = (cart[id] || 0) + 1;
    renderCart();
    const btn = carousel.querySelector(`.add-btn[data-id="${id}"]`);
    if(btn){
      btn.textContent = 'Added ✓';
      btn.classList.add('added');
      setTimeout(()=>{ btn.textContent='Add to cart'; btn.classList.remove('added'); }, 900);
    }
  }
  function changeQty(id, delta){
    if(!cart[id]) return;
    cart[id] += delta;
    if(cart[id] <= 0) delete cart[id];
    renderCart();
  }
  function removeItem(id){
    delete cart[id];
    renderCart();
  }
  function cartCountTotal(){
    return Object.values(cart).reduce((a,b)=>a+b,0);
  }
  function cartPriceTotal(){
    return Object.entries(cart).reduce((sum,[id,qty])=>{
      const p = PRODUCTS.find(x=>x.id===id);
      return sum + (p ? p.price*qty : 0);
    },0);
  }

  const cartItemsEl = document.getElementById('cartItems');
  const cartCountEl = document.getElementById('cartCount');
  const cartTotalRow = document.getElementById('cartTotalRow');
  const cartTotalEl = document.getElementById('cartTotal');

  function renderCart(){
    const count = cartCountTotal();
    cartCountEl.textContent = count;

    if(count === 0){
      cartItemsEl.innerHTML = `<p class="empty-msg">Your cart is empty. Add something tasty from the shelf!</p>`;
      cartTotalRow.style.display = 'none';
      return;
    }

    cartTotalRow.style.display = 'flex';
    cartTotalEl.textContent = '₹' + cartPriceTotal();

    cartItemsEl.innerHTML = Object.entries(cart).map(([id, qty])=>{
      const p = PRODUCTS.find(x=>x.id===id);
      if(!p) return '';
      return `
        <div class="cart-item">
          <img src="${p.img}" alt="${p.name}">
          <div class="ci-info">
            <h4>${p.name}</h4>
            <div class="p">₹${p.price} × ${qty} = ₹${p.price*qty}</div>
            <div class="qty-ctrl">
              <button type="button" data-act="dec" data-id="${id}">−</button>
              <span>${qty}</span>
              <button type="button" data-act="inc" data-id="${id}">+</button>
              <button type="button" class="remove-btn" data-act="rm" data-id="${id}">Remove</button>
            </div>
          </div>
        </div>`;
    }).join('');
  }

  carousel.addEventListener('click', (e)=>{
    const btn = e.target.closest('.add-btn');
    if(btn) addToCart(btn.dataset.id);
  });

  cartItemsEl.addEventListener('click', (e)=>{
    const btn = e.target.closest('button[data-act]');
    if(!btn) return;
    const id = btn.dataset.id;
    if(btn.dataset.act === 'inc') changeQty(id, 1);
    if(btn.dataset.act === 'dec') changeQty(id, -1);
    if(btn.dataset.act === 'rm') removeItem(id);
  });

  renderCart();

  /* ---------------------------------------------------------
     4. DRAWER OPEN / CLOSE
  --------------------------------------------------------- */
  const drawer = document.getElementById('drawer');
  const overlay = document.getElementById('overlay');

  function openDrawer(){
    drawer.classList.add('open');
    overlay.classList.add('open');
    document.body.style.overflow = 'hidden';
  }
  function closeDrawer(){
    drawer.classList.remove('open');
    overlay.classList.remove('open');
    document.body.style.overflow = '';
  }
  document.getElementById('openCartBtn').addEventListener('click', openDrawer);
  document.getElementById('closeCartBtn').addEventListener('click', closeDrawer);
  overlay.addEventListener('click', closeDrawer);

  /* ---------------------------------------------------------
     5. BUILD ORDER TEXT (shared by EmailJS + mailto fallback)
  --------------------------------------------------------- */
  function buildOrderLines(){
    return Object.entries(cart).map(([id, qty])=>{
      const p = PRODUCTS.find(x=>x.id===id);
      if(!p) return '';
      return `• ${p.name}  —  Qty: ${qty}  —  ₹${p.price} each  —  Subtotal: ₹${p.price*qty}`;
    }).join('\n');
  }

  /* ---------------------------------------------------------
     6. SEND ORDER
     Primary path: EmailJS (client-side email, free tier, no backend).
     Fallback path: mailto: link, opens the customer's own email app
     with everything pre-filled — works everywhere, zero setup.

     ============== SETUP NOTE (2 minutes, optional) ==============
     To make "Send Order" deliver automatically in the background:
     1. Create a free account at https://www.emailjs.com
     2. Add an Email Service (e.g. connect the Gmail that should
        receive orders: siddhantkumars9fe@gmail.com)
     3. Create an Email Template with variables:
        {{from_name}}, {{phone}}, {{address}}, {{items}}, {{total}}
     4. Copy your Public Key, Service ID, and Template ID into the
        three constants right below, then set EMAILJS_READY = true.
     Until you do this, tapping "Send Order" will simply open the
     customer's email app instead (the fallback), which still gets
     the order to your inbox — just with one extra tap from them.
     ================================================================
  --------------------------------------------------------- */
  const EMAILJS_READY     = false; // flip to true once configured
  const EMAILJS_PUBLIC_KEY  = 'YOUR_PUBLIC_KEY';
  const EMAILJS_SERVICE_ID  = 'YOUR_SERVICE_ID';
  const EMAILJS_TEMPLATE_ID = 'YOUR_TEMPLATE_ID';

  if(EMAILJS_READY && window.emailjs){
    emailjs.init({ publicKey: EMAILJS_PUBLIC_KEY });
  }

  function mailtoFallback(name, phone, address, items, total){
    const subject = encodeURIComponent(`New Order — the local shop (${name || 'Customer'})`);
    const body = encodeURIComponent(
`New order from the local shop website

Customer name: ${name}
Phone number: ${phone}
Delivery address: ${address || '—'}

Items ordered:
${items}

Order total: ₹${total}
`);
    window.location.href = `mailto:${OWNER_EMAIL}?subject=${subject}&body=${body}`;
  }

  const form = document.getElementById('order-form');
  const sendBtn = document.getElementById('sendBtn');
  const statusMsg = document.getElementById('statusMsg');

  function showStatus(msg, ok){
    statusMsg.textContent = msg;
    statusMsg.className = 'status-msg show ' + (ok ? 'ok' : 'err');
  }

  form.addEventListener('submit', function(e){
    e.preventDefault();

    if(cartCountTotal() === 0){
      showStatus('Your cart is empty — add an item before sending your order.', false);
      return;
    }

    const name = document.getElementById('custName').value.trim();
    const phone = document.getElementById('custPhone').value.trim();
    const address = document.getElementById('custAddress').value.trim();
    const items = buildOrderLines();
    const total = cartPriceTotal();

    if(!name || !phone){
      showStatus('Please fill in your name and phone number.', false);
      return;
    }

    if(EMAILJS_READY && window.emailjs){
      sendBtn.disabled = true;
      sendBtn.textContent = 'Sending…';

      emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID, {
        from_name: name,
        phone: phone,
        address: address || '—',
        items: items,
        total: '₹' + total,
        to_email: OWNER_EMAIL
      }).then(function(){
        sendBtn.disabled = false;
        sendBtn.textContent = 'Send Order';
        showStatus('Order sent! We\u2019ll be in touch shortly. 🎉', true);
        cart = {};
        renderCart();
        form.reset();
      }).catch(function(err){
        sendBtn.disabled = false;
        sendBtn.textContent = 'Send Order';
        showStatus('Could not send automatically — opening your email app instead…', false);
        mailtoFallback(name, phone, address, items, total);
      });
    } else {
      mailtoFallback(name, phone, address, items, total);
      showStatus('Opening your email app to send the order to the shop…', true);
    }
  });

  document.getElementById('emailFallback').addEventListener('click', function(e){
    e.preventDefault();
    const name = document.getElementById('custName').value.trim();
    const phone = document.getElementById('custPhone').value.trim();
    const address = document.getElementById('custAddress').value.trim();
    mailtoFallback(name, phone, address, buildOrderLines() || '(cart is empty)', cartPriceTotal());
  });

})();
</script>
</body>
</html>
