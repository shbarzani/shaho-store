<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Shaho Store | Login & Shop</title>
  <meta name="description" content="Shaho Store: Shop mobiles, laptops, clothes, and accessories. Fast shipping, secure payments, and great deals.">
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  <script src="https://www.paypal.com/sdk/js?client-id=sb&currency=USD"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Poppins', Arial, sans-serif; background: #f7f8fa; color: #222; min-height: 100vh; }
    .hidden { display: none !important; }
    /* Auth screen */
    .auth-bg { min-height: 100vh; width: 100vw; background: linear-gradient(120deg, #0a58ca 0%, #6dd5ed 100%); display: flex; align-items: center; justify-content: center; position: fixed; top: 0; left: 0; right: 0; bottom: 0; z-index: 9999; }
    .auth-container { background: rgba(255,255,255,0.18); box-shadow: 0 8px 32px rgba(10,88,202,0.14); border-radius: 22px; padding: 44px 32px 32px 32px; max-width: 370px; width: 100%; backdrop-filter: blur(12px); display: flex; flex-direction: column; align-items: stretch; }
    .auth-tabs { display: flex; justify-content: center; margin-bottom: 24px; }
    .auth-tab { flex: 1 1 50%; background: none; border: none; padding: 14px 0; font-size: 1.1rem; font-weight: 600; color: #0a58ca; border-bottom: 2.5px solid transparent; cursor: pointer; transition: color 0.2s, border 0.2s; }
    .auth-tab.active { color: #084298; border-bottom: 2.5px solid #ffe066; background: rgba(255,255,255,0.13); }
    .auth-form { display: flex; flex-direction: column; gap: 18px; }
    .auth-form.hidden { display: none; }
    .auth-title { text-align: center; color: #0a58ca; font-size: 1.4rem; margin-bottom: 12px; font-weight: 700; letter-spacing: 1px; }
    .auth-field { position: relative; }
    .auth-field input { width: 100%; padding: 12px 44px 12px 14px; border-radius: 12px; border: 1.5px solid #e3e8f0; font-size: 1rem; background: rgba(255,255,255,0.75); outline: none; transition: border 0.2s; }
    .auth-field input:focus { border: 1.5px solid #0a58ca; }
    .password-field .toggle-password { position: absolute; right: 14px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #0a58ca; font-size: 1.15rem; opacity: 0.7; user-select: none; }
    .password-field .toggle-password:hover { opacity: 1; }
    .auth-row { display: flex; justify-content: space-between; align-items: center; font-size: 0.97rem; }
    .forgot-link { color: #0a58ca; text-decoration: underline; cursor: pointer; font-size: 0.97rem; }
    .forgot-link:hover { color: #084298; }
    .auth-btn { background: #0a58ca; color: #fff; border: none; border-radius: 22px; padding: 12px 0; font-size: 1.1rem; font-weight: 700; cursor: pointer; margin-top: 8px; transition: background 0.2s; }
    .auth-btn:hover { background: #084298; }
    .auth-error { min-height: 22px; color: #e84118; text-align: center; font-size: 0.98rem; }
    @media (max-width: 500px) { .auth-container { padding: 28px 5px 18px 5px; max-width: 99vw; } .auth-title { font-size: 1.1rem; } }
    /* Store styles */
    .container { max-width: 1200px; margin: 0 auto; padding: 0 18px; }
    .main-header { background: #fff; box-shadow: 0 2px 8px rgba(0,0,0,0.04); padding: 0.5rem 0; position: sticky; top: 0; z-index: 10; }
    .header-flex { display: flex; align-items: center; justify-content: space-between; }
    .logo { display: flex; align-items: center; gap: 14px; }
    .logo-img { width: 54px; height: 54px; border-radius: 12px; object-fit: cover; border: 2px solid #0a58ca; background: #fff; }
    .logo-title { display: flex; flex-direction: column; line-height: 1; }
    .logo-main { font-size: 2rem; font-weight: 700; color: #0a58ca; }
    .logo-sub { font-size: 2rem; font-weight: 700; color: #222; margin-top: -2px; }
    .main-nav ul { list-style: none; display: flex; gap: 24px; }
    .main-nav a { text-decoration: none; color: #222; font-weight: 600; font-size: 1rem; transition: color 0.2s; }
    .main-nav a:hover { color: #0a58ca; }
    .header-right { display: flex; align-items: center; gap: 16px; }
    .search-bar { padding: 7px 14px; border-radius: 18px; border: 1px solid #eee; font-size: 1rem; outline: none; transition: border 0.2s; }
    .search-bar:focus { border: 1.5px solid #0a58ca; }
    .cart-icon { font-size: 1.4rem; cursor: pointer; position: relative; background: #f1f3f6; border-radius: 30px; padding: 7px 18px; transition: background 0.2s; user-select: none; }
    .cart-icon:hover { background: #e3e8f0; }
    .user-box { display: flex; align-items: center; gap: 10px; background: #eaf1fb; border-radius: 30px; padding: 7px 18px; font-size: 1rem; font-weight: 500; color: #0a58ca; margin-left: 8px; }
    .logout-btn { background: #e84118; color: #fff; border: none; border-radius: 18px; padding: 5px 14px; font-size: 1rem; cursor: pointer; transition: background 0.2s; }
    .logout-btn:hover { background: #c23616; }
    .account-btn { background: #ffe066; color: #0a58ca; border: none; border-radius: 18px; padding: 5px 14px; font-size: 1rem; cursor: pointer; transition: background 0.2s; font-weight: 600;}
    .account-btn:hover { background: #ffd700; }
    .hero-section { background: linear-gradient(120deg, #0a58ca 0%, #6dd5ed 100%); color: #fff; padding: 60px 0 40px 0; text-align: center; }
    .hero-glass { background: rgba(255,255,255,0.10); border-radius: 18px; padding: 40px 18px; margin: 0 auto; max-width: 540px; box-shadow: 0 4px 32px rgba(0,0,0,0.09); }
    .hero-section h2 { font-size: 2.5rem; margin-bottom: 0.5rem; }
    .hero-section .accent { color: #ffe066; font-weight: 700; }
    .cta-button { background: #ffe066; color: #0a58ca; border: none; border-radius: 24px; padding: 12px 38px; font-size: 1.1rem; font-weight: 700; margin-top: 22px; cursor: pointer; transition: background 0.2s, color 0.2s; }
    .cta-button:hover { background: #ffd700; color: #084298; }
    .products-section { padding: 48px 0; }
    .section-title { font-size: 2rem; font-weight: 700; text-align: center; margin-bottom: 32px; color: #0a58ca; }
    .products-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 28px; }
    .product-card { background: #fff; border-radius: 16px; box-shadow: 0 2px 16px rgba(0,0,0,0.05); overflow: hidden; display: flex; flex-direction: column; transition: box-shadow 0.2s; }
    .product-card:hover { box-shadow: 0 6px 24px rgba(0,0,0,0.10); }
    .product-image { position: relative; background: #eaf1fb; padding: 18px; display: flex; align-items: center; justify-content: center; min-height: 170px; }
    .product-image img { max-width: 100%; max-height: 120px; object-fit: contain; border-radius: 10px; }
    .product-badge { position: absolute; top: 10px; left: 10px; background: #0a58ca; color: #fff; font-size: 0.85rem; padding: 4px 12px; border-radius: 14px; font-weight: 600; letter-spacing: 1px; }
    .product-info { padding: 18px 16px; flex: 1; display: flex; flex-direction: column; }
    .product-info h3 { font-size: 1.2rem; font-weight: 700; margin-bottom: 8px; color: #222; }
    .product-description { font-size: 0.97rem; color: #555; margin-bottom: 14px; }
    .price-section { margin-bottom: 16px; }
    .price { font-size: 1.2rem; font-weight: 700; color: #0a58ca; }
    .buy-button { background: #0a58ca; color: #fff; border: none; border-radius: 22px; padding: 10px 0; font-size: 1rem; font-weight: 600; cursor: pointer; transition: background 0.2s; width: 100%; margin-bottom: 0; }
    .buy-button:hover { background: #084298; }
    .cart-modal, .payment-modal { position: fixed; z-index: 1000; left: 0; top: 0; right: 0; bottom: 0; background: rgba(30,40,60,0.18); display: flex; align-items: center; justify-content: center; }
    .cart-modal.hidden, .payment-modal.hidden { display: none; }
    .modal-content { background: #fff; border-radius: 18px; box-shadow: 0 8px 48px rgba(0,0,0,0.12); width: 95%; max-width: 420px; padding: 0; overflow: hidden; }
    .modal-header { display: flex; align-items: center; justify-content: space-between; background: #0a58ca; color: #fff; padding: 18px 24px; }
    .modal-header h3 { font-size: 1.3rem; }
    .close-button { font-size: 1.6rem; cursor: pointer; color: #fff; transition: color 0.2s; }
    .close-button:hover { color: #ffe066; }
    .modal-body { padding: 22px 24px 24px 24px; }
    .cart-total, .order-total { margin-top: 18px; font-size: 1.1rem; color: #0a58ca; font-weight: 700; }
    .checkout-btn { background: #0a58ca; color: #fff; border: none; border-radius: 20px; padding: 10px 0; font-size: 1rem; font-weight: 600; cursor: pointer; width: 100%; margin-top: 16px; transition: background 0.2s; }
    .checkout-btn:disabled { background: #aaa; cursor: not-allowed; }
    .checkout-btn:hover:not(:disabled) { background: #084298; }
    #cartItems, #orderSummary { list-style: none; margin: 0; padding: 0; }
    #cartItems li, #orderSummary li { display: flex; align-items: center; justify-content: space-between; margin-bottom: 9px; font-size: 1rem; }
    .cart-item-name { flex: 2; }
    .cart-item-qty { flex: 1; text-align: center; }
    .cart-item-price { flex: 1; text-align: right; color: #0a58ca; }
    .remove-btn { background: #e84118; color: #fff; border: none; border-radius: 12px; padding: 4px 10px; font-size: 0.92rem; margin-left: 8px; cursor: pointer; transition: background 0.2s; }
    .remove-btn:hover { background: #c23616; }
    .payment-methods { margin-top: 22px; }
    .payment-methods h4 { margin-bottom: 6px; font-size: 1.1rem; color: #0a58ca; }
    .about-section, .contact-section { background: #fff; padding: 44px 0; margin-top: 22px; }
    .about-section h2, .contact-section h2 { color: #0a58ca; font-size: 2rem; text-align: center; margin-bottom: 18px; }
    .about-section p, .contact-section p { max-width: 700px; margin: 0 auto 10px auto; font-size: 1.05rem; color: #444; text-align: center; }
    .about-section strong { color: #0a58ca; }
    .main-footer { background: #0a58ca; color: #fff; padding: 36px 0 0 0; margin-top: 38px; }
    .footer-content { display: flex; flex-wrap: wrap; gap: 36px; justify-content: space-between; max-width: 1200px; margin: 0 auto; }
    .footer-section { flex: 1 1 200px; margin-bottom: 18px; }
    .footer-section h4 { font-size: 1.1rem; margin-bottom: 10px; color: #ffe066; }
    .footer-section ul { list-style: none; padding: 0; }
    .footer-section ul li { margin-bottom: 6px; }
    .footer-section ul a { color: #fff; text-decoration: none; transition: color 0.2s; }
    .footer-section ul a:hover { color: #ffe066; }
    .payment-icons span { background: #fff; color: #0a58ca; border-radius: 6px; padding: 3px 10px; margin-right: 8px; font-weight: 600; font-size: 1rem; }
    .footer-bottom { text-align: center; padding: 14px 0; background: #09408c; color: #ffe066; font-size: 0.97rem; }
    @media (max-width: 900px) { .footer-content { flex-direction: column; gap: 18px; } .products-grid { grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); } }
    @media (max-width: 600px) { .main-header .container, .about-section .container, .contact-section .container, .footer-content { padding: 0 8px; } .hero-glass { padding: 20px 5px; } .modal-content { max-width: 98vw; } .products-section { padding: 18px 0; } }
  </style>
</head>
<body>
  <!-- LOGIN/REGISTER SCREEN -->
  <div id="authScreen" class="auth-bg">
    <div class="auth-container">
      <div class="auth-tabs">
        <button id="loginTab" class="auth-tab active">Login</button>
        <button id="registerTab" class="auth-tab">Register</button>
      </div>
      <!-- Login Form -->
      <form id="loginForm" class="auth-form">
        <h2 class="auth-title">Welcome Back</h2>
        <div class="auth-field">
          <input type="text" id="loginEmail" placeholder="Email" autocomplete="username" required>
        </div>
        <div class="auth-field password-field">
          <input type="password" id="loginPassword" placeholder="Password" autocomplete="current-password" required>
          <span class="toggle-password" data-toggle="loginPassword">&#128065;</span>
        </div>
        <div class="auth-row">
          <label><input type="checkbox" id="rememberMe"> Remember Me</label>
          <a href="#" id="forgotLink" class="forgot-link">Forgot?</a>
        </div>
        <button type="submit" class="auth-btn">Login</button>
        <div id="loginError" class="auth-error"></div>
      </form>
      <!-- Register Form -->
      <form id="registerForm" class="auth-form hidden">
        <h2 class="auth-title">Create Account</h2>
        <div class="auth-field">
          <input type="text" id="registerName" placeholder="Full Name" autocomplete="name" required>
        </div>
        <div class="auth-field">
          <input type="email" id="registerEmail" placeholder="Email" autocomplete="email" required>
        </div>
        <div class="auth-field password-field">
          <input type="password" id="registerPassword" placeholder="Password (min 8 chars)" autocomplete="new-password" required>
          <span class="toggle-password" data-toggle="registerPassword">&#128065;</span>
        </div>
        <div class="auth-field password-field">
          <input type="password" id="registerConfirm" placeholder="Confirm Password" autocomplete="new-password" required>
          <span class="toggle-password" data-toggle="registerConfirm">&#128065;</span>
        </div>
        <button type="submit" class="auth-btn">Register</button>
        <div id="registerError" class="auth-error"></div>
      </form>
      <!-- Forgot Password Form -->
      <form id="forgotForm" class="auth-form hidden">
        <h2 class="auth-title">Reset Password</h2>
        <div class="auth-field">
          <input type="email" id="forgotEmail" placeholder="Enter your email" required>
        </div>
        <button type="submit" class="auth-btn">Send Reset Link</button>
        <div id="forgotMsg" class="auth-error"></div>
      </form>
    </div>
  </div>

  <!-- MAIN WEBSITE (hidden until login) -->
  <div id="siteScreen" class="hidden">
    <!-- Header Section -->
    <header class="main-header">
      <div class="container header-flex">
        <div class="logo">
          <img src="logo.jpg" alt="Shaho Store Logo" class="logo-img">
          <div class="logo-title">
            <span class="logo-main">Shaho</span>
            <span class="logo-sub">Store</span>
          </div>
        </div>
        <nav class="main-nav">
          <ul>
            <li><a href="#home">Home</a></li>
            <li><a href="#products">Products</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
            <li><a href="#faq">FAQ</a></li>
            <li><a href="#shipping">Shipping </a></li>
          </ul>
        </nav>
        <div class="header-right">
          <input type="text" id="searchInput" class="search-bar" placeholder="Search products...">
          <div class="cart-icon" id="cartBtn" tabindex="0" aria-label="View cart">
            🛒 <span id="cart-count">0</span>
          </div>
          <div id="userBox" class="user-box">
            <span id="userWelcome"></span>
            <button id="logoutBtn" class="logout-btn">Logout</button>
            <button id="accountBtn" class="account-btn">My Account</button>
          </div>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section id="home" class="hero-section">
      <div class="hero-glass">
        <h2>Welcome to <span class="accent">Shaho Store</span></h2>
        <p>Find your perfect product in every category</p>
        <button class="cta-button" onclick="scrollToProducts()">Shop Now</button>
      </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="products-section">
      <div class="container">
        <h2 class="section-title">Our Products</h2>
        <div class="products-grid" id="productsGrid"></div>
      </div>
    </section>

    <!-- Product Reviews Modal -->
    <div id="reviewsModal" class="cart-modal hidden">
      <div class="modal-content">
        <div class="modal-header">
          <h3>Product Reviews</h3>
          <span class="close-button" id="closeReviewsBtn">&times;</span>
        </div>
        <div class="modal-body" id="reviewsBody"></div>
      </div>
    </div>

    <!-- Cart Modal -->
    <div id="cartModal" class="cart-modal hidden">
      <div class="modal-content">
        <div class="modal-header">
          <h3>Your Cart</h3>
          <span class="close-button" id="closeCartBtn">&times;</span>
        </div>
        <div class="modal-body">
          <ul id="cartItems"></ul>
          <div class="cart-total">
            <strong>Total: $<span id="cartTotal">0.00</span></strong>
          </div>
          <button class="checkout-btn" id="checkoutBtn">Checkout</button>
        </div>
      </div>
    </div>

    <!-- Payment Modal -->
    <div id="paymentModal" class="payment-modal hidden">
      <div class="modal-content">
        <div class="modal-header">
          <h3>Complete Your Purchase</h3>
          <span class="close-button" id="closePaymentBtn">&times;</span>
        </div>
        <div class="modal-body">
          <div class="order-summary">
            <h4>Order Summary</h4>
            <ul id="orderSummary"></ul>
            <div class="order-total">
              <strong>Total: $<span id="orderTotal">0.00</span></strong>
            </div>
          </div>
          <div class="payment-methods">
            <h4>Payment Methods</h4>
            <p>Pay securely with PayPal, Credit Card, or Debit Card</p>
            <div id="paypal-button-container"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Account/Order History Modal -->
    <div id="accountModal" class="cart-modal hidden">
      <div class="modal-content">
        <div class="modal-header">
          <h3>My Account</h3>
          <span class="close-button" id="closeAccountBtn">&times;</span>
        </div>
        <div class="modal-body" id="accountBody"></div>
      </div>
    </div>

    <!-- About Section -->
    <section id="about" class="about-section">
      <div class="container">
        <h2>About Shaho Store</h2>
        <p>
          Shaho Store is your one-stop shop for mobiles, laptops, clothes, and accessories. We offer top brands and the latest products at great prices. Our mission is to deliver quality and satisfaction to every customer.
        </p>
      </div>
    </section>

    <!-- FAQ Section -->
    <section id="faq" class="about-section">
      <div class="container">
        <h2>Frequently Asked Questions</h2>
        <p><strong>How do I place an order?</strong> <br>Browse products, add to cart, and complete checkout using PayPal or credit card.</p>
        <p><strong>Can I return products?</strong> <br>Yes, see our Shipping & Returns section for details.</p>
        <p><strong>How can I contact support?</strong> <br>Email us at <a href="mailto:shahinbarzanii@gmail.com">shahinbarzanii@gmail.com</a> or call <a href="tel:+9647503121203">+964-750 3121203</a>.</p>
      </div>
    </section>

    <!-- Shipping & Returns Section -->
    <section id="shipping" class="about-section">
      <div class="container">
        <h2>Shipping & Returns</h2>
        <p><strong>Shipping:</strong> We offer fast shipping to all regions. Orders are processed within 1-2 business days. Delivery time is 3-7 days.</p>
        <p><strong>Returns:</strong> You can return products within 14 days of delivery for a full refund. Items must be unused and in original packaging.</p>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="contact-section">
      <div class="container">
        <h2>Contact Us</h2>
        <p>Email: <a href="mailto:shahinbarzanii@gmail.com">shahinbarzanii@gmail.com</a></p>
        <p>Phone: <a href="tel:+9647503121203">+964-750 3121203</a></p>
        <p>Location: Erbil-mergasor</p>
      </div>
    </section>
    <!-- Footer -->
    <footer class="main-footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-section">
            <h4>Shaho Store</h4>
            <p>Your trusted online shop for all your needs.</p>
          </div>
          <div class="footer-section">
            <h4>Quick Links</h4>
            <ul>
              <li><a href="#home">Home</a></li>
              <li><a href="#products">Products</a></li>
              <li><a href="#about">About</a></li>
              <li><a href="#faq">FAQ</a></li>
              <li><a href="#shipping">Shipping & Returns</a></li>
            </ul>
          </div>
          <div class="footer-section">
            <h4>Payment Methods</h4>
            <div class="payment-icons">
              <span>PayPal</span>
              <span>Visa</span>
              <span>Mastercard</span>
            </div>
          </div>
        </div>
        <div class="footer-bottom">
          <p>&copy; 2025 Shaho Store. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>

  <script>
    // --- LOGIN/REGISTER LOGIC ---
    function getUsers() {
      return JSON.parse(localStorage.getItem("users") || "[]");
    }
    function saveUsers(users) {
      localStorage.setItem("users", JSON.stringify(users));
    }
    function setTab(tab) {
      if (tab === "login") {
        loginTab.classList.add("active");
        registerTab.classList.remove("active");
        loginForm.classList.remove("hidden");
        registerForm.classList.add("hidden");
        forgotForm.classList.add("hidden");
        clearErrors();
      } else {
        loginTab.classList.remove("active");
        registerTab.classList.add("active");
        loginForm.classList.add("hidden");
        registerForm.classList.remove("hidden");
        forgotForm.classList.add("hidden");
        clearErrors();
      }
    }
    function clearErrors() {
      document.getElementById("loginError").textContent = "";
      document.getElementById("registerError").textContent = "";
      document.getElementById("forgotMsg").textContent = "";
    }
    function showSiteScreen() {
      document.getElementById("authScreen").classList.add("hidden");
      document.getElementById("siteScreen").classList.remove("hidden");
      renderUserBox();
      renderProducts();
      updateCartCount();
    }
    function showAuthScreen() {
      document.getElementById("authScreen").classList.remove("hidden");
      document.getElementById("siteScreen").classList.add("hidden");
      clearErrors();
      setTab("login");
      loginForm.reset();
      registerForm.reset();
      forgotForm.reset();
    }
    // Tab switching
    const loginTab = document.getElementById("loginTab");
    const registerTab = document.getElementById("registerTab");
    const loginForm = document.getElementById("loginForm");
    const registerForm = document.getElementById("registerForm");
    const forgotForm = document.getElementById("forgotForm");
    loginTab.onclick = () => setTab("login");
    registerTab.onclick = () => setTab("register");
    // Show/hide password
    document.querySelectorAll('.toggle-password').forEach(icon => {
      icon.addEventListener('click', function() {
        const field = document.getElementById(this.dataset.toggle);
        if (field.type === "password") {
          field.type = "text";
          this.style.opacity = 1;
        } else {
          field.type = "password";
          this.style.opacity = 0.7;
        }
      });
    });
    // Remember Me
    window.addEventListener("DOMContentLoaded", () => {
      const saved = JSON.parse(localStorage.getItem("shaho_remember") || "null");
      if (saved) {
        document.getElementById("loginEmail").value = saved.email;
        document.getElementById("loginPassword").value = saved.password;
        document.getElementById("rememberMe").checked = true;
      }
      // Auto-login if already logged in
      if (localStorage.getItem("shaho_user")) {
        showSiteScreen();
      }
    });
    // Login logic
    loginForm.onsubmit = function(e) {
      e.preventDefault();
      const email = document.getElementById("loginEmail").value.trim().toLowerCase();
      const password = document.getElementById("loginPassword").value;
      const remember = document.getElementById("rememberMe").checked;
      const users = getUsers();
      const user = users.find(u => u.email === email && u.password === password);
      if (user) {
        localStorage.setItem("shaho_user", user.name || user.email);
        if (remember) {
          localStorage.setItem("shaho_remember", JSON.stringify({ email, password }));
        } else {
          localStorage.removeItem("shaho_remember");
        }
        showSiteScreen();
      } else {
        document.getElementById("loginError").textContent = "Invalid email or password.";
      }
    };
    // Register logic
    registerForm.onsubmit = function(e) {
      e.preventDefault();
      const name = document.getElementById("registerName").value.trim();
      const email = document.getElementById("registerEmail").value.trim().toLowerCase();
      const password = document.getElementById("registerPassword").value;
      const confirm = document.getElementById("registerConfirm").value;
      let users = getUsers();
      if (!name || !email || !password || !confirm) {
        document.getElementById("registerError").textContent = "All fields are required.";
        return;
      }
      if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
        document.getElementById("registerError").textContent = "Invalid email address.";
        return;
      }
      if (password.length < 8) {
        document.getElementById("registerError").textContent = "Password must be at least 8 characters.";
        return;
      }
      if (password !== confirm) {
        document.getElementById("registerError").textContent = "Passwords do not match.";
        return;
      }
      if (users.some(u => u.email === email)) {
        document.getElementById("registerError").textContent = "Email already registered.";
        return;
      }
      users.push({ name, email, password });
      saveUsers(users);
      document.getElementById("registerError").style.color = "#0a58ca";
      document.getElementById("registerError").textContent = "Registration successful! You can now login.";
      setTimeout(() => setTab("login"), 1300);
    };
    // Forgot password logic
    document.getElementById("forgotLink").onclick = (e) => {
      e.preventDefault();
      loginForm.classList.add("hidden");
      registerForm.classList.add("hidden");
      forgotForm.classList.remove("hidden");
      clearErrors();
    };
    forgotForm.onsubmit = function(e) {
      e.preventDefault();
      const email = document.getElementById("forgotEmail").value.trim().toLowerCase();
      const users = getUsers();
      if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
        document.getElementById("forgotMsg").textContent = "Enter a valid email address.";
        return;
      }
      if (!users.some(u => u.email === email)) {
        document.getElementById("forgotMsg").textContent = "Email not found.";
        return;
      }
      document.getElementById("forgotMsg").style.color = "#0a58ca";
      document.getElementById("forgotMsg").textContent = "Reset link sent! (Simulated)";
      setTimeout(() => setTab("login"), 1500);
    };

    // --- STORE LOGIC ---
    const products = [
      { name: "iPhone 14 Pro", price: 999, image: "mobile1.jpg", category: "Mobiles", desc: "Powerful chip and top camera system." },
      { name: "s24 Ultra", price: 980, image: "s242.jpg", category: "Mobiles", desc: "Powerful chip and top camera system." },
      { name: "Samsung Galaxy S23", price: 849, image: "galaxy1.jpg", category: "Mobiles", desc: "Great Android experience with fast performance." },
      { name: "HP Pro M2", price: 1230,  image: "laptop1.jpg", category: "Laptops", desc: "Super powerful and sleek HP laptop. (Discounted!)" },
      { name: "Dell XPS 15", price: 1299, image: "laptop2.jpg", category: "Laptops", desc: "Great for work and design professionals." },
      { name: "Cool T-Shirt", price: 25, image: "t-shirt.jpg", category: "Clothes", desc: "100% cotton, stylish and comfortable." },
      { name: "Winter Jacket", price: 99, image: "t-shirt2.jpg", category: "Clothes", desc: "Warm and cozy jacket for cold weather." },
      { name: "Wireless Headphones", price: 199, image: "head.jpg", category: "Accessories", desc: "Noise-cancelling, long battery life." },
      { name: "Smart Watch", price: 149, image: "smartwatch.jpg", category: "Accessories", desc: "Track your fitness and notifications." },
      { name: "Huawei Nova 11", price: 699, image: "mob3.jpg", category: "Mobiles", desc: "Huawei Nova 11: Stunning design, advanced camera, and long-lasting battery." },
      { name: "Huawei P60 Pro", price: 899, image: "mob4.jpg", category: "Mobiles", desc: "Huawei P60 Pro: Flagship performance, ultra-clear night shots, and fast charging." },
      { name: "Huawei Mate 50", price: 1099, image: "mob5.jpg", category: "Mobiles", desc: "Huawei Mate 50: Premium build, 5G connectivity, " }
    ];
    const productReviews = {
      "iPhone 14 Pro": [
        { user: "Ali", rating: 5, text: "Amazing phone, super fast!" },
        { user: "Sara", rating: 4, text: "Great camera and battery." }
      ],
      "Samsung Galaxy S23": [
        { user: "Ahmed", rating: 5, text: "Best Android experience!" }
      ]
    };
    let cart = [];
    function getOrderHistory() {
      return JSON.parse(localStorage.getItem("shaho_orders") || "[]");
    }
    function saveOrderHistory(history) {
      localStorage.setItem("shaho_orders", JSON.stringify(history));
    }
    function renderProducts(filter = "") {
      const grid = document.getElementById("productsGrid");
      grid.innerHTML = "";
      products
        .filter(product =>
          product.name.toLowerCase().includes(filter) ||
          product.desc.toLowerCase().includes(filter)
        )
        .forEach((product, idx) => {
          const card = document.createElement("div");
          card.className = "product-card";
          let priceHTML = `<span class="price">$${product.price}</span>`;
          card.innerHTML = `
            <div class="product-image">
              <img src="${product.image}" alt="${product.name}" />
              <div class="product-badge">${product.category}</div>
            </div>
            <div class="product-info">
              <h3>${product.name}</h3>
              <p class="product-description">${product.desc}</p>
              <div class="price-section">${priceHTML}</div>
              <button class="buy-button" onclick="addToCart(${idx})">Add to Cart</button>
              <button class="buy-button" style="background:#ffe066;color:#0a58ca;margin-top:8px;" onclick="showReviews('${product.name}')">Reviews</button>
            </div>
          `;
          grid.appendChild(card);
        });
    }
    window.addToCart = function(idx) {
      if (!isLoggedIn()) {
        showAuthScreen();
        return;
      }
      const product = products[idx];
      const existing = cart.find(item => item.name === product.name);
      if (existing) {
        existing.qty += 1;
      } else {
        cart.push({ ...product, qty: 1 });
      }
      updateCartCount();
      showCartModal();
    };
    function updateCartCount() {
      document.getElementById("cart-count").textContent = cart.reduce((sum, item) => sum + item.qty, 0);
    }
    function showCartModal() {
      if (!isLoggedIn()) {
        showAuthScreen();
        return;
      }
      renderCart();
      document.getElementById("cartModal").classList.remove("hidden");
      document.body.style.overflow = "hidden";
    }
    function hideCartModal() {
      document.getElementById("cartModal").classList.add("hidden");
      document.body.style.overflow = "auto";
    }
    function renderCart() {
      const cartItems = document.getElementById("cartItems");
      cartItems.innerHTML = "";
      if (cart.length === 0) {
        cartItems.innerHTML = '<li>Your cart is empty.</li>';
        document.getElementById("cartTotal").textContent = "0.00";
        document.getElementById("checkoutBtn").disabled = true;
        return;
      }
      cart.forEach((item, idx) => {
        const li = document.createElement("li");
        li.innerHTML = `
          <span class="cart-item-name">${item.name}</span>
          <span class="cart-item-qty">x${item.qty}</span>
          <span class="cart-item-price">$${(item.price * item.qty).toFixed(2)}</span>
          <button class="remove-btn" onclick="removeFromCart(${idx})">Delete</button>
        `;
        cartItems.appendChild(li);
      });
      document.getElementById("cartTotal").textContent = cartTotal().toFixed(2);
      document.getElementById("checkoutBtn").disabled = false;
    }
    window.removeFromCart = function(idx) {
      cart.splice(idx, 1);
      updateCartCount();
      renderCart();
    };
    function cartTotal() {
      return cart.reduce((sum, item) => sum + item.price * item.qty, 0);
    }
    function showPaymentModal() {
      if (!isLoggedIn()) {
        showAuthScreen();
        return;
      }
      renderOrderSummary();
      document.getElementById("paymentModal").classList.remove("hidden");
      document.body.style.overflow = "hidden";
      renderPayPalButton();
    }
    function hidePaymentModal() {
      document.getElementById("paymentModal").classList.add("hidden");
      document.body.style.overflow = "auto";
      document.getElementById("paypal-button-container").innerHTML = "";
    }
    function renderOrderSummary() {
      const orderSummary = document.getElementById("orderSummary");
      orderSummary.innerHTML = "";
      cart.forEach(item => {
        const li = document.createElement("li");
        li.textContent = `${item.name} x${item.qty} - $${(item.price * item.qty).toFixed(2)}`;
        orderSummary.appendChild(li);
      });
      document.getElementById("orderTotal").textContent = cartTotal().toFixed(2);
    }
    function renderPayPalButton() {
      if (window.paypal) {
        paypal.Buttons({
          createOrder: function(data, actions) {
            return actions.order.create({
              purchase_units: [{
                amount: { value: cartTotal().toFixed(2) },
                description: 'Shaho Store Order'
              }]
            });
          },
          onApprove: function(data, actions) {
            return actions.order.capture().then(function(details) {
              alert('Thank you for your purchase, ' + details.payer.name.given_name + '!');
              // Save order to order history for logged-in user
              if (isLoggedIn() && cart.length > 0) {
                const history = getOrderHistory();
                history.push({
                  user: localStorage.getItem("shaho_user"),
                  date: new Date().toLocaleString(),
                  items: cart.map(item => ({ name: item.name, qty: item.qty, price: item.price }))
                });
                saveOrderHistory(history);
              }
              cart = [];
              updateCartCount();
              hidePaymentModal();
              hideCartModal();
            });
          },
          onError: function(err) {
            alert('Payment failed. Please try again.');
          }
        }).render('#paypal-button-container');
      }
    }
    window.scrollToProducts = function() {
      document.getElementById("products").scrollIntoView({ behavior: "smooth" });
    };
    function isLoggedIn() {
      return !!localStorage.getItem("shaho_user");
    }
    function renderUserBox() {
      const userBox = document.getElementById("userBox");
      if (isLoggedIn()) {
        userBox.style.display = "flex";
        document.getElementById("userWelcome").textContent = "Welcome, " + localStorage.getItem("shaho_user");
      } else {
        userBox.style.display = "none";
      }
    }
    document.getElementById("logoutBtn").onclick = function() {
      localStorage.removeItem("shaho_user");
      showAuthScreen();
    };
    document.getElementById("cartBtn").onclick = function() {
      if (!isLoggedIn()) {
        showAuthScreen();
        return;
      }
      showCartModal();
    };
    document.getElementById("checkoutBtn").onclick = function() {
      if (!isLoggedIn()) {
        showAuthScreen();
        return;
      }
      hideCartModal();
      showPaymentModal();
    };
    document.getElementById("searchInput").oninput = function(e) {
      renderProducts(e.target.value.trim().toLowerCase());
    };
    window.showReviews = function(productName) {
      const reviews = productReviews[productName] || [];
      let html = `<h4>${productName}</h4>`;
      if (reviews.length === 0) {
        html += "<p>No reviews yet.</p>";
      } else {
        html += reviews.map(r =>
          `<div style="margin:8px 0;"><strong>${r.user}</strong> <span style="color:#ffe066;">${"★".repeat(r.rating)}</span><br>${r.text}</div>`
        ).join('');
      }
      document.getElementById("reviewsBody").innerHTML = html;
      document.getElementById("reviewsModal").classList.remove("hidden");
      document.body.style.overflow = "hidden";
    };
    document.getElementById("closeReviewsBtn").onclick = function() {
      document.getElementById("reviewsModal").classList.add("hidden");
      document.body.style.overflow = "auto";
    };
    function renderAccount() {
      let html = "<h4 style='margin-bottom:12px;'>Order History</h4>";
      const user = localStorage.getItem("shaho_user");
      const userOrders = getOrderHistory().filter(o => o.user === user);
      if (userOrders.length === 0) {
        html += "<p>No orders yet.</p>";
      } else {
        userOrders.reverse().forEach(order => {
          html += `<div style="margin:12px 0;border-bottom:1px solid #eee;padding-bottom:8px;">
            <strong>Date:</strong> ${order.date}<br>
            <strong>Items:</strong>
            <ul style="margin:0 0 0 18px;">${
              order.items.map(i => `<li>${i.name} x${i.qty} - $${(i.price * i.qty).toFixed(2)}</li>`).join('')
            }</ul>
          </div>`;
        });
      }
      document.getElementById("accountBody").innerHTML = html;
    }
    document.getElementById("accountBtn").onclick = function() {
      renderAccount();
      document.getElementById("accountModal").classList.remove("hidden");
      document.body.style.overflow = "hidden";
    };
    document.getElementById("closeAccountBtn").onclick = function() {
      document.getElementById("accountModal").classList.add("hidden");
      document.body.style.overflow = "auto";
    };
    document.getElementById("closeCartBtn").onclick = hideCartModal;
    document.getElementById("closePaymentBtn").onclick = hidePaymentModal;
    window.onclick = function(event) {
      if (event.target === document.getElementById("cartModal")) hideCartModal();
      if (event.target === document.getElementById("paymentModal")) hidePaymentModal();
      if (event.target === document.getElementById("reviewsModal")) document.getElementById("reviewsModal").classList.add("hidden");
      if (event.target === document.getElementById("accountModal")) document.getElementById("accountModal").classList.add("hidden");
    };
  </script>
</body>
</html>
