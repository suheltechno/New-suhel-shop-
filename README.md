# New-suhel-shop-
This is new version of our website 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Suhel Pro Shop | Premium Tech</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lucide/0.263.0/umd/lucide.min.js"></script>
    <style>
        :root {
            --primary-blue: #0077b6;
            --primary-green: #20bf6b;
            --dark-bg: #0f172a;
            --light-text: #f8fafc;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }

        body { background-color: #f1f5f9; color: #333; }

        /* Header */
        header {
            background: linear-gradient(135deg, var(--primary-blue), var(--primary-green));
            color: white;
            padding: 1rem 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .logo { font-size: 1.5rem; font-weight: bold; letter-spacing: 1px; }

        .cart-icon { cursor: pointer; position: relative; }
        #cart-count {
            position: absolute;
            top: -8px;
            right: -8px;
            background: red;
            font-size: 12px;
            padding: 2px 6px;
            border-radius: 50%;
        }

        /* Hero Section */
        .hero {
            height: 300px;
            background: var(--dark-bg);
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
        }

        .hero h1 { font-size: 3rem; margin-bottom: 10px; }
        .hero p { color: var(--primary-green); font-weight: bold; }

        /* Products Grid */
        .container { padding: 40px 5%; }
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 25px;
        }

        .product-card {
            background: white;
            border-radius: 15px;
            overflow: hidden;
            transition: transform 0.3s;
            border: 1px solid #e2e8f0;
            text-align: center;
            padding-bottom: 20px;
        }

        .product-card:hover { transform: translateY(-10px); }
        .product-img { height: 200px; background: #eee; width: 100%; display: flex; align-items: center; justify-content: center; font-size: 50px; }
        
        .product-card h3 { margin: 15px 0; }
        .price { color: var(--primary-blue); font-weight: bold; font-size: 1.2rem; }
        
        .btn-add {
            background: var(--primary-green);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 15px;
            transition: opacity 0.2s;
        }

        /* Modal / Checkout */
        #payment-modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 2000;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 20px;
            max-width: 400px;
            width: 90%;
            text-align: center;
        }

        .qr-code { width: 100%; max-width: 250px; margin: 20px 0; border: 5px solid var(--primary-blue); border-radius: 10px; }

        footer {
            background: var(--dark-bg);
            color: white;
            padding: 40px 5%;
            text-align: center;
            margin-top: 50px;
        }

        .footer-credit { color: var(--primary-green); margin-top: 10px; }

    </style>
</head>
<body>

<header>
    <div class="logo">SUHEL PRO SHOP</div>
    <div class="cart-icon" onclick="openCheckout()">
        <i data-lucide="shopping-cart"></i>
        <span id="cart-count">0</span>
    </div>
</header>

<section class="hero">
    <h1>Welcome to Suhel Pro Shop</h1>
    <p>Premium Quality | Fast Delivery | Secure Payments</p>
</section>

<div class="container">
    <div class="grid" id="product-grid">
        </div>
</div>

<div id="payment-modal">
    <div class="modal-content">
        <h2>Complete Payment</h2>
        <p>Total Amount: <span id="total-amt" style="font-weight:bold; color:var(--primary-blue)">₹0</span></p>
        <img src="AccountQRCodeIndia Post Payment Bank - 1861_DARK_THEME.png" alt="PhonePe QR" class="qr-code">
        <p>Scan the QR above using <b>PhonePe</b> to pay.</p>
        <p style="font-size: 0.8rem; margin-top:10px;">Account: SUHEL (IPPB)</p>
        <button class="btn-add" style="background:var(--primary-blue); margin-top:20px; width:100%" onclick="closeCheckout()">I Have Paid</button>
    </div>
</div>

<footer>
    <p>&copy; 2026 Suhel Pro Shop. All rights reserved.</p>
    <p class="footer-credit">Developed by: <b>Suhel Choudhary</b></p>
</footer>

<script>
    // Initialize Icons
    lucide.createIcons();

    const products = [
        { id: 1, name: "Pro Gaming Mouse", price: 1200, icon: "mouse" },
        { id: 2, name: "Mechanical Keyboard", price: 3500, icon: "keyboard" },
        { id: 3, name: "Wireless Headphones", price: 2800, icon: "headphones" },
        { id: 4, name: "Smart Watch Elite", price: 4999, icon: "watch" }
    ];

    let cartCount = 0;
    let totalPrice = 0;

    // Load Products
    const grid = document.getElementById('product-grid');
    products.forEach(p => {
        grid.innerHTML += `
            <div class="product-card">
                <div class="product-img"><i data-lucide="${p.icon}" size="48"></i></div>
                <h3>${p.name}</h3>
                <p class="price">₹${p.price}</p>
                <button class="btn-add" onclick="addToCart(${p.price})">Add to Cart</button>
            </div>
        `;
    });
    lucide.createIcons(); // Refresh icons for dynamic content

    function addToCart(price) {
        cartCount++;
        totalPrice += price;
        document.getElementById('cart-count').innerText = cartCount;
        alert("Item added to cart!");
    }

    function openCheckout() {
        if(cartCount === 0) {
            alert("Your cart is empty!");
            return;
        }
        document.getElementById('total-amt').innerText = "₹" + totalPrice;
        document.getElementById('payment-modal').style.display = "flex";
    }

    function closeCheckout() {
        document.getElementById('payment-modal').style.display = "none";
        alert("Thank you for your order! Suhel Choudhary will verify your payment soon.");
        cartCount = 0;
        totalPrice = 0;
        document.getElementById('cart-count').innerText = 0;
    }
</script>

</body>
</html>
