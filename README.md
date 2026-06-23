# Create-Shopping-Website
Shopping Website is a responsive e-commerce web application built using HTML, CSS, and JavaScript. It allows users to browse products, view details, add items to the cart, manage quantities, and experience a smooth shopping interface with a modern, mobile-friendly design
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Local Shop – Fresh Finds, Best Prices</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --green: #2E7D32;
    --green-light: #4CAF50;
    --green-pale: #E8F5E9;
    --cream: #FAFAF7;
    --dark: #1A1A1A;
    --mid: #555;
    --border: #E0E0E0;
    --radius: 12px;
    --shadow: 0 4px 24px rgba(0,0,0,0.10);
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--cream);
    color: var(--dark);
    overflow-x: hidden;
  }

  /* NAV */
  nav {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    background: rgba(255,255,255,0.95);
    backdrop-filter: blur(8px);
    border-bottom: 1px solid var(--border);
    padding: 0 1.5rem;
    height: 60px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .nav-logo {
    font-family: 'Playfair Display', serif;
    font-size: 1.4rem;
    color: var(--green);
    letter-spacing: -0.5px;
  }
  .nav-tagline {
    font-size: 0.75rem;
    color: var(--mid);
    display: none;
  }
  @media(min-width:600px){ .nav-tagline { display: block; } }
  #cart-btn {
    background: var(--green);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 0.45rem 1.1rem;
    font-size: 0.9rem;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    display: flex; align-items: center; gap: 8px;
    transition: background 0.2s;
  }
  #cart-btn:hover { background: var(--green-light); }
  .cart-badge {
    background: #FF5722;
    color: white;
    border-radius: 50%;
    width: 20px; height: 20px;
    font-size: 0.7rem;
    font-weight: 600;
    display: flex; align-items: center; justify-content: center;
  }

  /* HERO */
  .hero {
    margin-top: 60px;
    height: calc(100vh - 60px);
    min-height: 480px;
    position: relative;
    display: flex; align-items: center; justify-content: center;
    overflow: hidden;
    text-align: center;
  }
  .hero-img {
    position: absolute; inset: 0;
    width: 100%; height: 100%;
    object-fit: cover;
    object-position: center;
  }
  .hero-overlay {
    position: absolute; inset: 0;
    background: linear-gradient(to bottom, rgba(0,0,0,0.35) 0%, rgba(0,0,0,0.55) 100%);
  }
  .hero-content {
    position: relative; z-index: 2;
    padding: 0 1.5rem;
    max-width: 640px;
  }
  .hero-eyebrow {
    display: inline-block;
    background: rgba(255,255,255,0.18);
    border: 1px solid rgba(255,255,255,0.4);
    color: white;
    font-size: 0.78rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 0.3rem 1rem;
    border-radius: 50px;
    margin-bottom: 1.25rem;
  }
  .hero h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(2.4rem, 7vw, 4.2rem);
    color: white;
    line-height: 1.1;
    margin-bottom: 1rem;
    text-shadow: 0 2px 12px rgba(0,0,0,0.3);
  }
  .hero p {
    font-size: 1.1rem;
    color: rgba(255,255,255,0.85);
    margin-bottom: 2rem;
  }
  .btn-primary {
    display: inline-block;
    background: var(--green);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 0.85rem 2.2rem;
    font-size: 1rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    cursor: pointer;
    text-decoration: none;
    transition: background 0.2s, transform 0.15s;
  }
  .btn-primary:hover { background: var(--green-light); transform: translateY(-2px); }

  /* SECTIONS */
  section { padding: 4rem 1.5rem; }
  .section-label {
    text-transform: uppercase;
    letter-spacing: 2px;
    font-size: 0.75rem;
    color: var(--green);
    font-weight: 600;
    margin-bottom: 0.5rem;
  }
  .section-title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(1.6rem, 4vw, 2.4rem);
    color: var(--dark);
    margin-bottom: 0.5rem;
  }
  .section-sub {
    color: var(--mid);
    font-size: 1rem;
    margin-bottom: 2.5rem;
  }

  /* PRODUCTS */
  #products { background: white; }
  .carousel-wrapper {
    display: flex;
    gap: 1.25rem;
    overflow-x: auto;
    padding-bottom: 1rem;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: thin;
    scrollbar-color: var(--border) transparent;
  }
  .carousel-wrapper::-webkit-scrollbar { height: 6px; }
  .carousel-wrapper::-webkit-scrollbar-track { background: transparent; }
  .carousel-wrapper::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  .product-card {
    flex: 0 0 260px;
    scroll-snap-align: start;
    background: var(--cream);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .product-card:hover { transform: translateY(-4px); box-shadow: var(--shadow); }
  .product-img-wrap {
    width: 100%; height: 200px;
    overflow: hidden; background: #f0f0f0;
  }
  .product-img-wrap img {
    width: 100%; height: 100%;
    object-fit: cover;
    transition: transform 0.3s;
  }
  .product-card:hover .product-img-wrap img { transform: scale(1.05); }
  .product-info { padding: 1rem; }
  .product-name {
    font-weight: 600;
    font-size: 1rem;
    color: var(--dark);
    margin-bottom: 0.3rem;
  }
  .product-desc {
    font-size: 0.82rem;
    color: var(--mid);
    margin-bottom: 0.75rem;
    line-height: 1.5;
  }
  .product-footer {
    display: flex; align-items: center; justify-content: space-between;
    gap: 0.5rem;
  }
  .product-price {
    font-family: 'Playfair Display', serif;
    font-size: 1.2rem;
    color: var(--green);
    font-weight: 700;
  }
  .add-btn {
    background: var(--green);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 0.45rem 1rem;
    font-size: 0.82rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.2s, transform 0.1s;
  }
  .add-btn:hover { background: var(--green-light); }
  .add-btn:active { transform: scale(0.96); }
  .add-btn.added { background: #FF5722; }

  /* ABOUT STRIP */
  .about-strip {
    background: var(--green-pale);
    border-top: 1px solid #C8E6C9;
    border-bottom: 1px solid #C8E6C9;
    padding: 3rem 1.5rem;
    text-align: center;
  }
  .about-strip .section-title { margin-bottom: 0.75rem; }
  .about-strip p { color: var(--mid); max-width: 560px; margin: 0 auto 1.5rem; line-height: 1.7; }
  .features-row {
    display: flex; flex-wrap: wrap; gap: 1rem;
    justify-content: center; margin-top: 2rem;
  }
  .feature-pill {
    background: white;
    border: 1px solid #C8E6C9;
    border-radius: 50px;
    padding: 0.5rem 1.25rem;
    font-size: 0.85rem;
    color: var(--green);
    font-weight: 500;
  }

  /* CONTACT */
  #contact { background: var(--dark); color: white; text-align: center; }
  #contact .section-label { color: #81C784; }
  #contact .section-title { color: white; }
  #contact .section-sub { color: rgba(255,255,255,0.6); }
  .contact-email {
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(255,255,255,0.08);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 50px;
    padding: 0.6rem 1.5rem;
    color: white;
    text-decoration: none;
    font-size: 0.95rem;
    transition: background 0.2s;
  }
  .contact-email:hover { background: rgba(255,255,255,0.14); }

  /* FOOTER */
  footer {
    background: #111;
    color: rgba(255,255,255,0.45);
    text-align: center;
    padding: 1.5rem;
    font-size: 0.8rem;
  }

  /* CART DRAWER */
  .cart-backdrop {
    display: none;
    position: fixed; inset: 0; z-index: 200;
    background: rgba(0,0,0,0.45);
  }
  .cart-backdrop.open { display: block; }
  .cart-drawer {
    position: fixed; top: 0; right: -100%; bottom: 0;
    width: min(420px, 100vw);
    background: white;
    z-index: 201;
    display: flex; flex-direction: column;
    transition: right 0.3s cubic-bezier(0.4,0,0.2,1);
    box-shadow: -8px 0 40px rgba(0,0,0,0.15);
  }
  .cart-drawer.open { right: 0; }
  .drawer-header {
    display: flex; align-items: center; justify-content: space-between;
    padding: 1.25rem 1.5rem;
    border-bottom: 1px solid var(--border);
  }
  .drawer-header h2 {
    font-family: 'Playfair Display', serif;
    font-size: 1.3rem;
  }
  #close-cart {
    background: none; border: none; cursor: pointer;
    font-size: 1.5rem; color: var(--mid);
    line-height: 1; padding: 4px;
  }
  .drawer-body { flex: 1; overflow-y: auto; padding: 1rem 1.5rem; }
  .cart-empty {
    text-align: center; padding: 3rem 1rem;
    color: var(--mid);
  }
  .cart-empty svg { margin-bottom: 1rem; opacity: 0.4; }
  .cart-item {
    display: flex; align-items: center; gap: 1rem;
    padding: 0.85rem 0;
    border-bottom: 1px solid var(--border);
  }
  .cart-item img {
    width: 60px; height: 60px;
    object-fit: cover; border-radius: 8px;
    border: 1px solid var(--border);
  }
  .cart-item-info { flex: 1; }
  .cart-item-name { font-weight: 600; font-size: 0.9rem; }
  .cart-item-price { color: var(--green); font-size: 0.9rem; margin-top: 2px; }
  .cart-item-qty {
    display: flex; align-items: center; gap: 8px;
    margin-top: 6px;
  }
  .qty-btn {
    width: 26px; height: 26px; border-radius: 50%;
    border: 1px solid var(--border);
    background: none; cursor: pointer; font-size: 1rem;
    display: flex; align-items: center; justify-content: center;
    color: var(--dark);
    transition: background 0.15s;
  }
  .qty-btn:hover { background: var(--green-pale); }
  .qty-val { font-size: 0.9rem; font-weight: 600; min-width: 20px; text-align: center; }
  .remove-btn {
    background: none; border: none; cursor: pointer;
    color: #ccc; font-size: 1.1rem; padding: 4px;
    transition: color 0.15s;
  }
  .remove-btn:hover { color: #e53935; }
  .drawer-footer { padding: 1.25rem 1.5rem; border-top: 1px solid var(--border); }
  .cart-total {
    display: flex; justify-content: space-between;
    font-weight: 600; font-size: 1.1rem;
    margin-bottom: 1rem;
  }
  .cart-total span:last-child { color: var(--green); }

  /* ORDER FORM */
  .order-form { margin-bottom: 1rem; }
  .order-form input {
    width: 100%;
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 0.65rem 0.9rem;
    font-family: 'Inter', sans-serif;
    font-size: 0.9rem;
    margin-bottom: 0.6rem;
    outline: none;
    transition: border-color 0.15s;
  }
  .order-form input:focus { border-color: var(--green); }
  #send-order {
    width: 100%;
    background: var(--green);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 0.85rem;
    font-size: 1rem;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.2s;
  }
  #send-order:hover { background: var(--green-light); }
  #send-order:disabled { background: #aaa; cursor: not-allowed; }
  .send-status {
    text-align: center;
    font-size: 0.85rem;
    margin-top: 0.5rem;
    min-height: 1.2rem;
  }
  .send-status.success { color: var(--green); }
  .send-status.error { color: #e53935; }

  /* FLOATERS */
  .float-wa {
    position: fixed; bottom: 5rem; right: 1.25rem; z-index: 150;
    width: 54px; height: 54px;
    background: #25D366;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    box-shadow: 0 4px 16px rgba(37,211,102,0.4);
    text-decoration: none;
    transition: transform 0.2s;
  }
  .float-wa:hover { transform: scale(1.1); }
  .float-wa svg { width: 28px; height: 28px; }
  .float-scroll-hint {
    position: absolute; bottom: 2rem; left: 50%; transform: translateX(-50%);
    color: rgba(255,255,255,0.7);
    font-size: 0.8rem;
    letter-spacing: 1px;
    text-transform: uppercase;
    display: flex; flex-direction: column; align-items: center; gap: 6px;
    animation: bounce 2s infinite;
  }
  @keyframes bounce {
    0%,100%{ transform: translateX(-50%) translateY(0); }
    50%{ transform: translateX(-50%) translateY(6px); }
  }

  /* TOAST */
  .toast {
    position: fixed; bottom: 5rem; left: 50%; transform: translateX(-50%) translateY(100px);
    background: var(--dark); color: white;
    padding: 0.65rem 1.5rem;
    border-radius: 50px;
    font-size: 0.88rem;
    font-weight: 500;
    z-index: 300;
    transition: transform 0.3s;
    white-space: nowrap;
  }
  .toast.show { transform: translateX(-50%) translateY(0); }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div>
    <div class="nav-logo">The Local Shop</div>
    <div class="nav-tagline">Fresh Finds, Best Prices</div>
  </div>
  <button id="cart-btn" onclick="openCart()">
    <svg width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/><line x1="3" y1="6" x2="21" y2="6"/><path d="M16 10a4 4 0 01-8 0"/></svg>
    Cart
    <div class="cart-badge" id="cart-count">0</div>
  </button>
</nav>

<!-- HERO -->
<section class="hero" id="home">
  <img class="hero-img" src="https://i.ibb.co/qLxdDwGX/urban-happy-outdoor-sale-beautiful.jpg" alt="The Local Shop – outdoor sale" loading="eager" onerror="this.style.background='linear-gradient(135deg,#2E7D32 0%,#1A1A1A 100%)'">
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <div class="hero-eyebrow">Now Open · Nashik</div>
    <h1>Fresh Finds,<br>Best Prices.</h1>
    <p>Your neighbourhood store for everyday essentials and hidden gems.</p>
    <a href="#products" class="btn-primary" onclick="smoothScroll(event,'products')">Shop Now ↓</a>
  </div>
  <div class="float-scroll-hint">
    <span>Scroll</span>
    <svg width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg>
  </div>
</section>

<!-- PRODUCTS -->
<section id="products">
  <div class="section-label">What's in store</div>
  <div class="section-title">Our Products</div>
  <p class="section-sub">Hand-picked items at prices you'll love.</p>
  <div class="carousel-wrapper" id="product-carousel"></div>
</section>

<!-- ABOUT STRIP -->
<div class="about-strip">
  <div class="section-label">Why us</div>
  <div class="section-title">Your Trusted Neighbourhood Shop</div>
  <p>We've been serving the community with quality products, honest prices, and a smile. Every item on our shelf is chosen with care so you never have to compromise.</p>
  <div class="features-row">
    <div class="feature-pill">✓ Fresh stock daily</div>
    <div class="feature-pill">✓ Best local prices</div>
    <div class="feature-pill">✓ Fast home delivery</div>
    <div class="feature-pill">✓ Order by WhatsApp</div>
  </div>
</div>

<!-- CONTACT -->
<section id="contact">
  <div class="section-label">Get in touch</div>
  <div class="section-title">We're always here</div>
  <p class="section-sub">Questions, bulk orders, or just a chat — reach out anytime.</p>
  <a class="contact-email" href="mailto:omargade489@gmail.com">
    <svg width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,12 2,6"/></svg>
    omargade489@gmail.com
  </a>
</section>

<footer>
  &copy; 2025 The Local Shop &nbsp;·&nbsp; Fresh Finds, Best Prices &nbsp;·&nbsp; Nashik, Maharashtra
</footer>

<!-- WHATSAPP FLOAT -->
<a class="float-wa" href="https://wa.me/7828516989" target="_blank" rel="noopener" aria-label="Chat on WhatsApp">
  <svg viewBox="0 0 32 32" fill="white" xmlns="http://www.w3.org/2000/svg">
    <path d="M16 3C9.373 3 4 8.373 4 15c0 2.385.832 4.584 2.236 6.334L4.5 28l6.89-1.714A12.94 12.94 0 0016 27c6.627 0 12-5.373 12-12S22.627 3 16 3zm0 22a11.02 11.02 0 01-5.678-1.578l-.407-.243-4.088 1.017 1.04-3.978-.266-.41A10.95 10.95 0 015 15C5 8.924 9.924 4 16 4s11 4.924 11 11-4.924 11-11 11zm6.145-8.264c-.337-.168-1.99-.982-2.299-1.094-.308-.112-.533-.168-.757.168-.225.337-.868 1.094-1.064 1.318-.196.225-.392.253-.729.084-.337-.168-1.422-.524-2.709-1.672-.999-.891-1.674-1.994-1.87-2.33-.196-.337-.021-.519.147-.687.15-.151.337-.393.505-.589.168-.196.224-.337.336-.562.112-.225.056-.421-.028-.589-.084-.168-.757-1.825-1.036-2.498-.273-.657-.55-.568-.757-.579l-.645-.011c-.224 0-.589.084-.897.421-.308.337-1.177 1.15-1.177 2.806 0 1.655 1.205 3.254 1.373 3.478.168.225 2.37 3.619 5.74 5.073.803.346 1.43.552 1.918.707.806.256 1.54.22 2.12.133.647-.097 1.99-.813 2.27-1.599.28-.785.28-1.458.196-1.599-.083-.14-.308-.225-.645-.393z"/>
  </svg>
</a>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<!-- CART DRAWER -->
<div class="cart-backdrop" id="cart-backdrop" onclick="closeCart()"></div>
<div class="cart-drawer" id="cart-drawer">
  <div class="drawer-header">
    <h2>Your Cart</h2>
    <button id="close-cart" onclick="closeCart()" aria-label="Close cart">&#x2715;</button>
  </div>
  <div class="drawer-body" id="cart-body">
    <div class="cart-empty" id="cart-empty">
      <svg width="64" height="64" fill="none" stroke="#999" stroke-width="1.5" viewBox="0 0 24 24"><path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/><line x1="3" y1="6" x2="21" y2="6"/><path d="M16 10a4 4 0 01-8 0"/></svg>
      <p>Your cart is empty.<br>Add some products!</p>
    </div>
    <div id="cart-items"></div>
  </div>
  <div class="drawer-footer">
    <div class="cart-total">
      <span>Total</span>
      <span id="cart-total">₹0.00</span>
    </div>
    <div class="order-form" id="order-form">
      <input type="text" id="cust-name" placeholder="Your name *" required>
      <input type="tel" id="cust-phone" placeholder="Phone number *" required>
    </div>
    <button id="send-order" onclick="sendOrder()">
      Send Order via Email 📧
    </button>
    <div class="send-status" id="send-status"></div>
  </div>
</div>

<script>
(function(){
  // EmailJS init — replace with your public key
  emailjs.init("YOUR_EMAILJS_PUBLIC_KEY");

  const PRODUCTS = [
    {
      id: 1,
      name: "Fresh Organic Basket",
      desc: "Locally sourced seasonal produce, hand-picked daily.",
      price: 299,
      img: "https://i.ibb.co/Lz3CVrxM/product1.jpg",
      fallback: "#4CAF50"
    },
    {
      id: 2,
      name: "Premium Spice Mix",
      desc: "Aromatic blend of handground Indian spices.",
      price: 149,
      img: "https://i.ibb.co/SkdTdRV/product2.jpg",
      fallback: "#FF7043"
    },
    {
      id: 3,
      name: "Artisan Snack Pack",
      desc: "Crunchy, delicious snacks — perfect for every occasion.",
      price: 199,
      img: "https://i.ibb.co/ymgjGx9J/product3.jpg",
      fallback: "#FFA726"
    },
    {
      id: 4,
      name: "Herbal Tea Collection",
      desc: "Soothing blends for morning calm and evening rest.",
      price: 249,
      img: "https://i.ibb.co/Lz3CVrxM/product4.jpg",
      fallback: "#26A69A"
    },
    {
      id: 5,
      name: "Natural Honey Jar",
      desc: "Pure raw honey from local beekeepers. 500g.",
      price: 349,
      img: "https://i.ibb.co/SkdTdRV/product5.jpg",
      fallback: "#FFC107"
    }
  ];

  let cart = [];

  // Render products
  const carousel = document.getElementById('product-carousel');
  PRODUCTS.forEach(p => {
    const card = document.createElement('div');
    card.className = 'product-card';
    card.innerHTML = `
      <div class="product-img-wrap">
        <img src="${p.img}" alt="${p.name}" loading="lazy" onerror="this.parentElement.style.background='${p.fallback}'; this.style.display='none'">
      </div>
      <div class="product-info">
        <div class="product-name">${p.name}</div>
        <div class="product-desc">${p.desc}</div>
        <div class="product-footer">
          <div class="product-price">₹${p.price}</div>
          <button class="add-btn" id="add-${p.id}" onclick="addToCart(${p.id})">Add to Cart</button>
        </div>
      </div>
    `;
    carousel.appendChild(card);
  });

  window.addToCart = function(id) {
    const prod = PRODUCTS.find(p => p.id === id);
    const existing = cart.find(c => c.id === id);
    if (existing) {
      existing.qty += 1;
    } else {
      cart.push({ ...prod, qty: 1 });
    }
    updateCartUI();
    const btn = document.getElementById('add-' + id);
    btn.textContent = 'Added ✓';
    btn.classList.add('added');
    setTimeout(() => { btn.textContent = 'Add to Cart'; btn.classList.remove('added'); }, 1200);
    showToast(`${prod.name} added to cart`);
  };

  window.changeQty = function(id, delta) {
    const item = cart.find(c => c.id === id);
    if (!item) return;
    item.qty += delta;
    if (item.qty <= 0) cart = cart.filter(c => c.id !== id);
    updateCartUI();
  };

  window.removeItem = function(id) {
    cart = cart.filter(c => c.id !== id);
    updateCartUI();
  };

  function updateCartUI() {
    const total = cart.reduce((s, c) => s + c.price * c.qty, 0);
    const count = cart.reduce((s, c) => s + c.qty, 0);
    document.getElementById('cart-count').textContent = count;
    document.getElementById('cart-total').textContent = '₹' + total.toFixed(2);

    const empty = document.getElementById('cart-empty');
    const itemsEl = document.getElementById('cart-items');

    if (cart.length === 0) {
      empty.style.display = 'block';
      itemsEl.innerHTML = '';
    } else {
      empty.style.display = 'none';
      itemsEl.innerHTML = cart.map(c => `
        <div class="cart-item">
          <img src="${c.img}" alt="${c.name}" onerror="this.style.background='${c.fallback}'; this.src='data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs='">
          <div class="cart-item-info">
            <div class="cart-item-name">${c.name}</div>
            <div class="cart-item-price">₹${c.price} each</div>
            <div class="cart-item-qty">
              <button class="qty-btn" onclick="changeQty(${c.id},-1)">−</button>
              <span class="qty-val">${c.qty}</span>
              <button class="qty-btn" onclick="changeQty(${c.id},1)">+</button>
            </div>
          </div>
          <button class="remove-btn" onclick="removeItem(${c.id})" title="Remove">✕</button>
        </div>
      `).join('');
    }
  }

  window.openCart = function() {
    document.getElementById('cart-backdrop').classList.add('open');
    document.getElementById('cart-drawer').classList.add('open');
    document.body.style.overflow = 'hidden';
  };

  window.closeCart = function() {
    document.getElementById('cart-backdrop').classList.remove('open');
    document.getElementById('cart-drawer').classList.remove('open');
    document.body.style.overflow = '';
  };

  window.sendOrder = async function() {
    const name = document.getElementById('cust-name').value.trim();
    const phone = document.getElementById('cust-phone').value.trim();
    const status = document.getElementById('send-status');

    if (!name || !phone) {
      status.textContent = 'Please enter your name and phone number.';
      status.className = 'send-status error';
      return;
    }
    if (cart.length === 0) {
      status.textContent = 'Your cart is empty!';
      status.className = 'send-status error';
      return;
    }

    const total = cart.reduce((s, c) => s + c.price * c.qty, 0);
    const itemLines = cart.map(c =>
      `• ${c.name} × ${c.qty} = ₹${(c.price * c.qty).toFixed(2)}`
    ).join('\n');

    const orderText = `New Order from The Local Shop Website

Customer: ${name}
Phone: ${phone}

Items Ordered:
${itemLines}

Total: ₹${total.toFixed(2)}

---
Sent from The Local Shop website`;

    const btn = document.getElementById('send-order');
    btn.disabled = true;
    btn.textContent = 'Sending…';
    status.textContent = '';
    status.className = 'send-status';

    // EmailJS send — update template_id and service_id after setup
    try {
      await emailjs.send(
        "YOUR_SERVICE_ID",
        "YOUR_TEMPLATE_ID",
        {
          to_email: "omargade489@gmail.com",
          from_name: name,
          customer_name: name,
          customer_phone: phone,
          order_details: orderText,
          order_total: "₹" + total.toFixed(2),
          items_list: itemLines
        }
      );
      status.textContent = '✓ Order sent! We\'ll contact you shortly.';
      status.className = 'send-status success';
      cart = [];
      updateCartUI();
      document.getElementById('cust-name').value = '';
      document.getElementById('cust-phone').value = '';
      setTimeout(closeCart, 2500);
    } catch (err) {
      // Fallback: open mailto
      const subject = encodeURIComponent(`New Order from ${name}`);
      const body = encodeURIComponent(orderText);
      window.open(`mailto:omargade489@gmail.com?subject=${subject}&body=${body}`);
      status.textContent = 'Email client opened. Please send the email!';
      status.className = 'send-status success';
    } finally {
      btn.disabled = false;
      btn.textContent = 'Send Order via Email 📧';
    }
  };

  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg;
    t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 2200);
  }

  window.smoothScroll = function(e, id) {
    e.preventDefault();
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
  };

  // Auto-carousel animation on desktop
  let autoScroll;
  const carouselEl = document.getElementById('product-carousel');
  function startAuto() {
    autoScroll = setInterval(() => {
      const maxScroll = carouselEl.scrollWidth - carouselEl.clientWidth;
      if (carouselEl.scrollLeft >= maxScroll - 10) {
        carouselEl.scrollTo({ left: 0, behavior: 'smooth' });
      } else {
        carouselEl.scrollBy({ left: 280, behavior: 'smooth' });
      }
    }, 3200);
  }
  startAuto();
  carouselEl.addEventListener('mouseenter', () => clearInterval(autoScroll));
  carouselEl.addEventListener('mouseleave', startAuto);
  carouselEl.addEventListener('touchstart', () => clearInterval(autoScroll), { passive: true });
  carouselEl.addEventListener('touchend', startAuto);
})();
</script>
</body>
</html>
