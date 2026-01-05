# Menu-card
This repository is for a menu card
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Food Menu</title>
    <style>
        :root {
            --primary: #e67e22; 
            --dark-bg: #2d1b15;
            --card-bg: #3d261e;
            --text-light: #f4f4f4;
            --success: #27ae60;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent; /* Removes blue tap box on mobile */
        }

        body {
            background-color: var(--dark-bg);
            color: var(--text-light);
            padding-bottom: 90px; /* Increased space for fixed cart */
        }

        /* Header */
        header {
            background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url('https://images.unsplash.com/photo-1555396273-367ea4eb4db5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80');
            background-size: cover;
            background-position: center;
            padding: 40px 20px;
            text-align: center;
            border-bottom: 4px solid var(--primary);
        }

        h1 {
            color: var(--primary);
            font-size: 2.2rem;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.9);
            margin: 0;
        }

        /* Navigation */
        .nav-scroller {
            overflow-x: auto;
            white-space: nowrap;
            padding: 15px;
            background: rgba(45, 27, 21, 0.95);
            position: sticky;
            top: 0;
            z-index: 500;
            backdrop-filter: blur(5px);
            border-bottom: 1px solid #444;
            -webkit-overflow-scrolling: touch; /* Smooth scroll on iOS */
        }

        .nav-scroller::-webkit-scrollbar {
            display: none; /* Hide scrollbar */
        }

        .nav-btn {
            background: transparent;
            border: 1px solid var(--primary);
            color: var(--primary);
            padding: 8px 18px;
            border-radius: 20px;
            margin-right: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.95rem;
        }

        .nav-btn.active {
            background: var(--primary);
            color: white;
            box-shadow: 0 2px 10px rgba(230, 126, 34, 0.4);
        }

        /* Menu Grid */
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }

        .section-title {
            color: var(--primary);
            border-bottom: 1px solid #555;
            padding-bottom: 8px;
            margin: 30px 0 15px 0;
            font-size: 1.3rem;
            font-weight: 600;
        }

        .menu-item {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            transition: transform 0.2s;
        }

        .item-info {
            flex: 1;
            padding-right: 15px;
        }

        .item-info h3 {
            font-size: 1.1rem;
            margin-bottom: 6px;
            font-weight: 500;
            line-height: 1.3;
        }

        .price {
            color: #ddd;
            font-weight: bold;
            font-size: 1.0rem;
        }

        .add-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            min-width: 80px;
            transition: background 0.2s;
        }

        /* Animation for clicking add */
        .add-btn.clicked {
            background-color: var(--success);
            transform: scale(0.95);
        }

        /* Floating Cart Bar */
        .cart-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: #1a1a1a;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-top: 2px solid var(--primary);
            box-shadow: 0 -4px 20px rgba(0,0,0,0.6);
            transform: translateY(110%); /* Hidden by default */
            transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 900;
        }

        .cart-bar.visible {
            transform: translateY(0);
        }

        .cart-total {
            font-size: 1.2rem;
            font-weight: bold;
            color: white;
        }

        .view-cart-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 30px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(230, 126, 34, 0.3);
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85);
            justify-content: center;
            align-items: center;
            z-index: 2000; /* Ensure on top of everything */
            backdrop-filter: blur(5px);
            padding: 20px;
        }

        .modal-content {
            background: #2d1b15;
            padding: 30px;
            border-radius: 16px;
            text-align: center;
            width: 100%;
            max-width: 400px;
            border: 1px solid #555;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .bill-row {
            display: flex; 
            justify-content: space-between; 
            margin-bottom: 12px;
            border-bottom: 1px dotted #444;
            padding-bottom: 4px;
        }

        .close-modal {
            margin-top: 20px;
            background: transparent;
            border: 1px solid #888;
            color: #ccc;
            padding: 10px 30px;
            border-radius: 20px;
            font-size: 0.9rem;
            cursor: pointer;
        }

    </style>
</head>
<body>

<header>
    <h1>Food Menu</h1>
</header>

<div class="nav-scroller">
    <button class="nav-btn active" onclick="filterMenu('all', this)">All</button>
    <button class="nav-btn" onclick="filterMenu('Starters', this)">Starters</button>
    <button class="nav-btn" onclick="filterMenu('Main Course', this)">Main Course</button>
    <button class="nav-btn" onclick="filterMenu('Beverages', this)">Beverages</button>
    <button class="nav-btn" onclick="filterMenu('Dessert', this)">Dessert</button>
</div>

<div class="container" id="menu-container">
    </div>

<div class="cart-bar" id="cart-bar">
    <div class="cart-total">Total: ₹<span id="total-price">0</span></div>
    <button class="view-cart-btn" onclick="checkout()">View Bill</button>
</div>

<div class="modal" id="orderModal">
    <div class="modal-content">
        <h2 style="color: var(--primary); margin-bottom: 20px;">Order Summary</h2>
        <div id="bill-details" style="margin: 20px 0; text-align: left; color: #ddd;"></div>
        <div style="display:flex; justify-content:space-between; margin-top:20px; font-size: 1.3rem; font-weight:bold; color: var(--primary);">
            <span>Grand Total</span>
            <span>₹<span id="modal-total">0</span></span>
        </div>
        <p style="margin-top: 15px; font-size: 0.85rem; color: #888;">Show this screen to your waiter</p>
        <button class="close-modal" onclick="closeModal()">Close</button>
    </div>
</div>

<script>
    // Data from your image
    const menuItems = [
        { category: "Starters", name: "Nachos with Cheese", price: 150 },
        { category: "Starters", name: "Mexican Fries", price: 120 },
        { category: "Starters", name: "Corn Cheese Balls", price: 140 },
        
        { category: "Main Course", name: "Veg Mexican Pizza", price: 220 },
        { category: "Main Course", name: "Paneer Mexican Pizza", price: 250 },
        
        { category: "Beverages", name: "Cold Coffee", price: 140 },
        { category: "Beverages", name: "Iced Tea", price: 100 },
        { category: "Beverages", name: "Classic Lemonade", price: 80 },
        
        { category: "Dessert", name: "Brownie with Ice Cream", price: 190 },
        { category: "Dessert", name: "Churros with Chocolate Dip", price: 160 }
    ];

    let cart = [];

    // Render Menu
    function renderMenu(filter = 'all') {
        const container = document.getElementById('menu-container');
        container.innerHTML = '';
        
        const categories = [...new Set(menuItems.map(item => item.category))];
        
        categories.forEach(cat => {
            if (filter !== 'all' && filter !== cat) return;

            const sectionDiv = document.createElement('div');
            sectionDiv.innerHTML = `<h2 class="section-title">${cat}</h2>`;
            
            menuItems.filter(item => item.category === cat).forEach(item => {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'menu-item';
                itemDiv.innerHTML = `
                    <div class="item-info">
                        <h3>${item.name}</h3>
                        <div class="price">₹${item.price}</div>
                    </div>
                    <button class="add-btn" onclick="addToCart('${item.name}', ${item.price}, this)">ADD +</button>
                `;
                sectionDiv.appendChild(itemDiv);
            });
            
            container.appendChild(sectionDiv);
        });
    }

    // Filter Logic - Fixed with 'el' parameter
    function filterMenu(category, el) {
        // Remove active class from all buttons
        document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
        // Add to clicked button
        el.classList.add('active');
        renderMenu(category);
    }

    // Cart Logic
    function addToCart(name, price, btnElement) {
        cart.push({ name, price });
        
        // Visual Feedback
        const originalText = btnElement.innerText;
        btnElement.innerText = "ADDED";
        btnElement.classList.add("clicked");
        
        // Vibration for mobile (Android)
        if (navigator.vibrate) navigator.vibrate(50);

        setTimeout(() => {
            btnElement.innerText = originalText;
            btnElement.classList.remove("clicked");
        }, 500);

        updateCartUI();
    }

    function updateCartUI() {
        const total = cart.reduce((sum, item) => sum + item.price, 0);
        document.getElementById('total-price').innerText = total;
        
        const cartBar = document.getElementById('cart-bar');
        if (cart.length > 0) {
            cartBar.classList.add('visible');
        } else {
            cartBar.classList.remove('visible');
        }
    }

    function checkout() {
        const modal = document.getElementById('orderModal');
        const billDetails = document.getElementById('bill-details');
        const modalTotal = document.getElementById('modal-total');
        
        // Count items
        const counts = {};
        cart.forEach(x => { counts[x.name] = (counts[x.name] || 0) + 1; });
        
        let html = '';
        for (const [name, count] of Object.entries(counts)) {
            // Find price safely
            const itemObj = menuItems.find(i => i.name === name);
            if (!itemObj) continue; 
            
            html += `<div class="bill-row">
                        <span>${count} x ${name}</span>
                        <span>₹${itemObj.price * count}</span>
                     </div>`;
        }
        
        billDetails.innerHTML = html;
        modalTotal.innerText = cart.reduce((sum, item) => sum + item.price, 0);
        modal.style.display = 'flex';
    }

    function closeModal() {
        document.getElementById('orderModal').style.display = 'none';
    }

    // Initialize
    renderMenu();
</script>

</body>
</html>
