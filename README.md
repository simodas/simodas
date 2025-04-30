<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>سوق داس</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <style>
    body {
      background: linear-gradient(to left, #fdfcfb, #e2d1c3);
    }
    .ad-area {
      background: linear-gradient(to right, #f59e0b, #facc15);
      animation: flicker 3s infinite;
    }
    @keyframes flicker {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.6; }
    }
  </style>
</head>
<body class="font-sans">
  <header class="bg-yellow-400 p-4 shadow flex justify-between items-center">
    <h1 class="text-2xl font-extrabold text-purple-800">سوق داس</h1>
    <nav class="flex gap-4 text-sm">
      <a href="#" class="hover:underline">الرئيسية</a>
      <a href="#categories" class="hover:underline">التصنيفات</a>
      <a href="#cart" class="hover:underline">السلة</a>
      <a href="#login" class="hover:underline">تسجيل الدخول</a>
      <a href="#add-product" class="hover:underline">إضافة منتج</a>
    </nav>
    <button class="bg-purple-700 text-white px-4 py-2 rounded" onclick="showCart()">عربة التسوق (<span id="cart-count">0</span>)</button>
  </header>

  <div class="ad-area text-center text-black p-3 font-bold">عرض خاص: توصيل مجاني للطلبات فوق 100 ريال!</div>

  <section id="login" class="p-6 text-center">
    <h2 class="text-xl font-bold mb-4">تسجيل حساب جديد</h2>
    <input id="regEmail" type="email" placeholder="البريد الإلكتروني" class="p-2 border rounded w-1/2 mb-2">
    <input id="regPass" type="password" placeholder="كلمة المرور" class="p-2 border rounded w-1/2 mb-2">
    <button onclick="registerUser()" class="bg-green-600 text-white px-4 py-2 rounded">تسجيل</button>

    <h2 class="text-xl font-bold mt-6 mb-2">تسجيل الدخول</h2>
    <input id="loginEmail" type="email" placeholder="البريد الإلكتروني" class="p-2 border rounded w-1/2 mb-2">
    <input id="loginPass" type="password" placeholder="كلمة المرور" class="p-2 border rounded w-1/2 mb-2">
    <button onclick="loginUser()" class="bg-blue-600 text-white px-4 py-2 rounded">دخول</button>
    <p id="loginMessage" class="mt-2 text-red-600 font-bold"></p>
  </section>

  <section id="add-product" class="hidden p-6">
    <h2 class="text-xl font-bold mb-4">إضافة منتج جديد</h2>
    <input id="productName" type="text" placeholder="اسم المنتج" class="p-2 border rounded w-1/2 mb-2">
    <input id="productDescription" type="text" placeholder="وصف المنتج" class="p-2 border rounded w-1/2 mb-2">
    <input id="productPrice" type="number" placeholder="السعر" class="p-2 border rounded w-1/2 mb-2">
    <input id="productImage" type="url" placeholder="رابط صورة المنتج" class="p-2 border rounded w-1/2 mb-2">
    <button onclick="addProduct()" class="bg-green-600 text-white px-4 py-2 rounded">إضافة المنتج</button>
  </section>

  <section id="categories" class="px-6 py-4">
    <h2 class="text-xl font-bold mb-4 text-center">تصنيفات المنتجات</h2>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <div class="bg-white p-4 rounded shadow text-center">إلكترونيات</div>
      <div class="bg-white p-4 rounded shadow text-center">ملابس</div>
      <div class="bg-white p-4 rounded shadow text-center">منزلية</div>
      <div class="bg-white p-4 rounded shadow text-center">ألعاب</div>
    </div>
  </section>

  <section id="product-list" class="px-6 py-4">
    <h2 class="text-xl font-bold mb-4 text-center text-purple-800">منتجات المستخدمين</h2>
    <div id="products" class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <!-- سيتم إضافة المنتجات هنا عبر JavaScript -->
    </div>
  </section>

  <footer class="bg-black text-white text-center p-4 mt-10">
    &copy; 2025 سوق داس - جميع الحقوق محفوظة
  </footer>

  <script>
    const cart = JSON.parse(localStorage.getItem('cart')) || [];
    const products = JSON.parse(localStorage.getItem('products')) || [];

    function updateCartUI() {
      document.getElementById('cart-count').textContent = cart.length;
    }

    function showCart() {
      document.getElementById('cart').scrollIntoView({ behavior: 'smooth' });
      updateCartUI();
    }

    function registerUser() {
      const email = document.getElementById('regEmail').value;
      const pass = document.getElementById('regPass').value;
      if (email && pass) {
        localStorage.setItem('user_' + email, pass);
        alert('تم التسجيل بنجاح!');
      }
    }

    function loginUser() {
      const email = document.getElementById('loginEmail').value;
      const pass = document.getElementById('loginPass').value;
      const savedPass = localStorage.getItem('user_' + email);
      if (savedPass && savedPass === pass) {
        document.getElementById('loginMessage').textContent = 'تم تسجيل الدخول بنجاح!';
        document.getElementById('add-product').classList.remove('hidden');
      } else {
        document.getElementById('loginMessage').textContent = 'بيانات الدخول غير صحيحة!';
      }
    }

    function addProduct() {
      const name = document.getElementById('productName').value;
      const description = document.getElementById('productDescription').value;
      const price = document.getElementById('productPrice').value;
      const image = document.getElementById('productImage').value;

      if (name && description && price && image) {
        const product = { name, description, price, image };
        products.push(product);
        localStorage.setItem('products', JSON.stringify(products));
        displayProducts();
      }
    }

    function displayProducts() {
      const productContainer = document.getElementById('products');
      productContainer.innerHTML = '';
      products.forEach((product, index) => {
        const productDiv = document.createElement('div');
        productDiv.classList.add('bg-white', 'p-4', 'rounded-2xl', 'shadow', 'flex', 'flex-col', 'items-center');
        productDiv.innerHTML = `
          <img src="${product.image}" alt="${product.name}" class="mb-4">
          <h2 class="text-lg font-bold">${product.name}</h2>
          <p class="text-sm text-gray-600">${product.description}</p>
          <p class="text-sm text-gray-600">${product.price} ريال</p>
          <button onclick="addToCart('${product.name}', ${product.price})" class="bg-yellow-500 px-4 py-2 mt-2 rounded hover:bg-yellow-400">أضف إلى السلة</button>
        `;
        productContainer.appendChild(productDiv);
      });
    }

    displayProducts();
  </script>
</body>
</html>
