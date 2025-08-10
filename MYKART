<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Kart</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }

        .container {
            max-width: 900px;
            margin: 20px auto;
            padding: 20px;
            background: #fff;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }

        .page {
            display: none;
        }

        .page.active {
            display: block;
        }

        h1, h2, h3 {
            color: #0056b3;
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
            margin-top: 0;
        }

        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 20px;
        }

        label {
            font-weight: bold;
        }

        input,
        textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        
        input[type="file"] {
            padding: 0;
            border: none;
        }
        
        input[type="file"]::-webkit-file-upload-button {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #eee;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        input[type="file"]::-webkit-file-upload-button:hover {
            background-color: #ddd;
        }

        button {
            padding: 10px 15px;
            border: none;
            background-color: #007bff;
            color: white;
            cursor: pointer;
            border-radius: 4px;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #0056b3;
        }
        
        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .product-card {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            background: #f9f9f9;
        }

        .product-card img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: 4px;
            margin-bottom: 10px;
        }

        .product-card h3 {
            font-size: 1.2em;
            margin: 10px 0;
        }

        .product-price {
            font-weight: bold;
            color: #e94e77;
            font-size: 1.1em;
        }

        .cart-item {
            display: flex;
            gap: 15px;
            align-items: center;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
            margin-bottom: 15px;
        }

        .cart-item img {
            width: 80px;
            height: 80px;
            object-fit: cover;
            border-radius: 4px;
        }

        .cart-summary {
            margin-top: 20px;
            text-align: right;
            border-top: 2px solid #eee;
            padding-top: 15px;
        }

        .order-details {
            border: 1px solid #ddd;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
            background-color: #f9f9f9;
        }

        .order-details h3 {
            margin-top: 0;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
        }

        .order-details ul {
            list-style: none;
            padding: 0;
            margin: 10px 0;
        }

        .order-details li {
            padding: 5px 0;
        }
    </style>
</head>
<body>

    <div class="container">
        <div id="login-view" class="page active">
            <h1>Welcome to My Kart</h1>
            <form id="login-form">
                <label for="username">Username:</label>
                <input type="text" id="username" required>
                <label for="password">Password:</label>
                <input type="password" id="password" required>
                <button type="submit">Login</button>
            </form>
        </div>

        <div id="admin-view" class="page">
            <h1>Admin Dashboard</h1>
            <button onclick="app.logout()">Logout</button>

            <hr>
            <h2>Add New Product</h2>
            <form id="add-product-form">
                <label for="product-name">Product Name:</label>
                <input type="text" id="product-name" required>
                <label for="product-price">Price (INR):</label>
                <input type="number" id="product-price" required>
                <label for="product-details">Details:</label>
                <textarea id="product-details" required></textarea>
                <label for="product-image">Product Image:</label>
                <input type="file" id="product-image" accept="image/*" required>
                <button type="submit">Add Product</button>
            </form>

            <hr>
            <h2>Customer Orders</h2>
            <div id="orders-list"></div>
        </div>

        <div id="customer-view" class="page">
            <h1>Welcome, Customer!</h1>
            <button onclick="app.logout()">Logout</button>
            <button onclick="app.showCart()">View My Cart</button>

            <hr>
            <h2>Our Products</h2>
            <div id="products-list" class="product-grid"></div>
        </div>

        <div id="cart-view" class="page">
            <h1>My Cart</h1>
            <div id="cart-items"></div>
            <div id="cart-summary">
                <h3>Total: ₹<span id="cart-total">0</span></h3>
                <button onclick="app.checkout()">Buy Now</button>
            </div>
            <button onclick="app.showView('customer-view')">Continue Shopping</button>
        </div>

        <div id="checkout-view" class="page">
            <h1>Checkout</h1>
            <form id="checkout-form">
                <label for="customer-name">Your Name:</label>
                <input type="text" id="customer-name" required>
                <label for="customer-address">Your Address:</label>
                <textarea id="customer-address" required></textarea>
                <label for="customer-contact">Contact Number:</label>
                <input type="text" id="customer-contact" required>
                <h3>Payment Method: Cash on Delivery</h3>
                <button type="submit">Place Order</button>
            </form>
        </div>
    </div>

    <script>
        const app = (function() {
            // --- State Management ---
            const state = {
                products: [],
                cart: [],
                orders: [],
                // Simple hardcoded credentials
                credentials: {
                    admin: 'a123',
                    customer: 'c123'
                }
            };

            // --- DOM Element Caching ---
            const dom = {
                loginForm: document.getElementById('login-form'),
                loginView: document.getElementById('login-view'),
                adminView: document.getElementById('admin-view'),
                customerView: document.getElementById('customer-view'),
                cartView: document.getElementById('cart-view'),
                checkoutView: document.getElementById('checkout-view'),
                addProductForm: document.getElementById('add-product-form'),
                productImageInput: document.getElementById('product-image'),
                productsList: document.getElementById('products-list'),
                cartItems: document.getElementById('cart-items'),
                cartTotal: document.getElementById('cart-total'),
                checkoutForm: document.getElementById('checkout-form'),
                ordersList: document.getElementById('orders-list'),
            };

            // --- View Management ---
            function showView(viewId) {
                document.querySelectorAll('.page').forEach(page => {
                    page.classList.remove('active');
                });
                document.getElementById(viewId).classList.add('active');
            }

            // --- Admin Functions ---
            function renderOrders() {
                if (state.orders.length === 0) {
                    dom.ordersList.innerHTML = '<p>No new orders.</p>';
                    return;
                }
                dom.ordersList.innerHTML = state.orders.map(order => {
                    const itemsHtml = order.items.map(item =>
                        `<li>${item.product.name} (x${item.quantity}) - ₹${(item.product.price * item.quantity).toFixed(2)}</li>`
                    ).join('');
                    return `
                        <div class="order-details">
                            <h3>Order #${order.id}</h3>
                            <p><strong>Customer:</strong> ${order.customer.name}</p>
                            <p><strong>Address:</strong> ${order.customer.address}</p>
                            <p><strong>Contact:</strong> ${order.customer.contact}</p>
                            <p><strong>Total:</strong> ₹${order.total.toFixed(2)}</p>
                            <p><strong>Items:</strong></p>
                            <ul>${itemsHtml}</ul>
                        </div>
                    `;
                }).join('');
            }

            function handleAddProduct(e) {
                e.preventDefault();
                const imageFile = dom.productImageInput.files[0];
                if (!imageFile) {
                    alert('Please select an image file.');
                    return;
                }

                const reader = new FileReader();
                reader.onload = function(event) {
                    const newProduct = {
                        id: state.products.length + 1,
                        name: dom.addProductForm['product-name'].value,
                        price: parseFloat(dom.addProductForm['product-price'].value),
                        details: dom.addProductForm['product-details'].value,
                        image: event.target.result // Use the data URL from the file reader
                    };
                    state.products.push(newProduct);
                    dom.addProductForm.reset();
                    alert(`Product "${newProduct.name}" added successfully!`);
                };
                reader.readAsDataURL(imageFile);
            }

            // --- Customer Functions ---
            function renderProductCards() {
                if (state.products.length === 0) {
                    dom.productsList.innerHTML = '<p>No products available yet.</p>';
                    return;
                }
                dom.productsList.innerHTML = state.products.map(product => `
                    <div class="product-card">
                        <img src="${product.image}" alt="${product.name}">
                        <h3>${product.name}</h3>
                        <p class="product-price">₹${product.price.toFixed(2)}</p>
                        <p>${product.details}</p>
                        <button onclick="app.addToCart(${product.id})">Add to Cart</button>
                    </div>
                `).join('');
            }

            function addToCart(productId) {
                const product = state.products.find(p => p.id === productId);
                if (product) {
                    const existingItem = state.cart.find(item => item.product.id === productId);
                    if (existingItem) {
                        existingItem.quantity += 1;
                    } else {
                        state.cart.push({ product: product, quantity: 1 });
                    }
                    alert(`${product.name} added to cart!`);
                }
            }

            function renderCartItems() {
                if (state.cart.length === 0) {
                    dom.cartItems.innerHTML = '<p>Your cart is empty.</p>';
                    dom.cartTotal.textContent = '0.00';
                    return;
                }

                const total = state.cart.reduce((sum, item) => sum + item.product.price * item.quantity, 0);
                dom.cartTotal.textContent = total.toFixed(2);

                dom.cartItems.innerHTML = state.cart.map(item => `
                    <div class="cart-item">
                        <img src="${item.product.image}" alt="${item.product.name}">
                        <div>
                            <h4>${item.product.name}</h4>
                            <p>Price: ₹${item.product.price.toFixed(2)}</p>
                            <p>Quantity: ${item.quantity}</p>
                        </div>
                    </div>
                `).join('');
            }

            function checkout() {
                if (state.cart.length > 0) {
                    showView('checkout-view');
                } else {
                    alert('Your cart is empty. Please add some products.');
                }
            }

            function handlePlaceOrder(e) {
                e.preventDefault();
                const customerDetails = {
                    name: dom.checkoutForm['customer-name'].value,
                    address: dom.checkoutForm['customer-address'].value,
                    contact: dom.checkoutForm['customer-contact'].value
                };
                const newOrder = {
                    id: state.orders.length + 1,
                    customer: customerDetails,
                    items: [...state.cart], // A copy of the cart
                    total: parseFloat(dom.cartTotal.textContent),
                    status: 'Pending'
                };
                state.orders.push(newOrder);
                state.cart = []; // Clear the cart
                dom.checkoutForm.reset();
                alert('Order is successfully placed!');
                showView('customer-view');
                renderProductCards();
            }

            // --- General Handlers ---
            function handleLogin(e) {
                e.preventDefault();
                const password = dom.loginForm['password'].value;
                if (password === state.credentials.admin) {
                    showView('admin-view');
                    renderOrders();
                } else if (password === state.credentials.customer) {
                    showView('customer-view');
                    renderProductCards();
                } else {
                    alert('Invalid username or password.');
                }
                dom.loginForm.reset();
            }

            function logout() {
                showView('login-view');
            }

            // --- Initialization ---
            function setupEventListeners() {
                dom.loginForm.addEventListener('submit', handleLogin);
                dom.addProductForm.addEventListener('submit', handleAddProduct);
                dom.checkoutForm.addEventListener('submit', handlePlaceOrder);
            }

            function init() {
                setupEventListeners();
                showView('login-view');
            }

            init();

            // Expose public methods
            return {
                logout,
                showView,
                addToCart,
                showCart: () => {
                    showView('cart-view');
                    renderCartItems();
                },
                checkout
            };
        })();
    </script>
</body>
</html>
