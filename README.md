<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Магазин парфюма</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
    }
    header {
      background-color: #fff;
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid #ddd;
    }
    header h1 {
      font-size: 20px;
      margin: 0;
    }
    .cart-icon {
      font-size: 24px;
      cursor: pointer;
    }
    .catalog {
      padding: 20px;
    }
    .product-card {
      background: white;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      display: flex;
      gap: 15px;
    }
    .product-image {
      width: 80px;
      height: 80px;
      object-fit: cover;
      border-radius: 6px;
    }
    .product-info {
      flex: 1;
    }
    .product-name {
      font-size: 18px;
      font-weight: bold;
      margin-bottom: 5px;
    }
    .product-price {
      color: #333;
      margin-bottom: 10px;
    }
    .add-to-cart {
      background-color: #e0b074;
      border: none;
      padding: 8px 12px;
      border-radius: 4px;
      color: white;
      cursor: pointer;
    }
    .cart-modal {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.5);
      display: none;
      justify-content: center;
      align-items: center;
    }
    .cart-content {
      background: white;
      padding: 20px;
      border-radius: 8px;
      width: 90%;
      max-width: 400px;
      max-height: 80vh;
      overflow-y: auto;
    }
    .cart-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 15px;
    }
    .checkout-btn {
      background-color: #d4a05d;
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 4px;
      color: white;
      margin-top: 15px;
      cursor: pointer;
    }
    footer {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #fff;
      text-align: center;
      padding: 10px;
      border-top: 1px solid #ddd;
      font-size: 12px;
      color: #666;
    }
  </style>
</head>
<body>
  <!-- Шапка -->
  <header>
    <h1>Parfum Shop</h1>
    <div class="cart-icon" id="cartIcon">🛒 <span id="cartCount">0</span></div>
  </header>

  <!-- Каталог товаров -->
  <div class="catalog" id="catalog">
    <!-- Пример карточки товара -->
    <div class="product-card">
      <img src="https://via.placeholder.com/80x80" alt="Парфюм" class="product-image" />
      <div class="product-info">
        <div class="product-name">Цветочное вдохновение</div>
        <div class="product-price">1 500 ₽</div>
        <button class="add-to-cart" onclick="addToCart('Цветочное вдохновение', 1500)">Добавить в корзину</button>
      </div>
    </div>
    <!-- Добавьте другие карточки аналогично -->
  </div>

  <!-- Модальное окно корзины -->
  <div class="cart-modal" id="cartModal">
    <div class="cart-content">
      <h2>Корзина</h2>
      <div id="cartItems"></div>
      <button class="checkout-btn" onclick="sendOrder()">Оформить заказ</button>
    </div>
  </div>

  <!-- Подвал -->
  <footer>
    © 2025 Parfum Shop. Все права защищены.
  </footer>

  <!-- Скрипт Telegram Web App -->
  <script>
    // Инициализация Telegram Web App
    if (window.Telegram && window.Telegram.WebApp) {
      const tg = window.Telegram.WebApp;
      tg.expand(); // Расширяем окно до полного экрана
      tg.MainButton.setText("Заказать").show(); // Показываем кнопку "Заказать"
    }

    // Логика корзины
    let cart = [];
    const cartIcon = document.getElementById('cartIcon');
    const cartModal = document.getElementById('cartModal');
    const cartItems = document.getElementById('cartItems');
    const cartCount = document.getElementById('cartCount');

    function addToCart(name, price) {
      cart.push({ name, price });
      updateCart();
    }

    function updateCart() {
      cartCount.textContent = cart.length;
      cartItems.innerHTML = '';
      cart.forEach((item, index) => {
        cartItems.innerHTML += `
          <div class="cart-item">
            <span>${item.name}</span>
            <span>${item.price} ₽</span>
          </div>
        `;
      });
    }

    cartIcon.addEventListener('click', () => {
      cartModal.style.display = 'flex';
    });

    function sendOrder() {
      if (cart.length === 0) return;

      // Отправка заказа через Telegram API
      if (window.Telegram && window.Telegram.WebApp) {
        const total = cart.reduce((sum, item) => sum + item.price, 0);
        const message = `📦 Заказ:\n${cart.map(i => `- ${i.name} (${i.price} ₽)`).join('\n')}\n\nИтого: ${total} ₽`;
        window.Telegram.WebApp.sendData(message); // Отправляем данные обратно в бота
      }
      alert("Заказ оформлен! Ожидайте подтверждения.");
      cart = [];
      updateCart();
      cartModal.style.display = 'none';
    }
  </script>
</body>
</html>
