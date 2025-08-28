<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Каталог LINKKORN</title>
    <!-- Мета-тег для Telegram Web Apps -->
    <meta name="telegram-web-app-init-data" content="YOUR_WEB_APP_INIT_DATA">
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
            background-color: var(--tg-theme-bg-color, #ffffff);
            color: var(--tg-theme-text-color, #000000);
        }
        .rounded-box {
            border-radius: 1rem;
            border: 3px solid black; /* Жирная черная обводка */
        }
        .rounded-btn {
            border-radius: 0.5rem;
            border: 2px solid black; /* Жирная черная обводка */
        }
        .product-image-container {
            width: 100%;
            padding-top: 56.25%; /* 16:9 Aspect Ratio */
            position: relative;
        }
        .product-image-container img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-right: 3px solid black;
        }
        .file-input-label {
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            padding: 0.5rem 1rem;
            border-radius: 0.375rem;
            transition: all 0.3s ease-in-out;
            border: 2px dashed black; /* Черный пунктир */
        }
        .file-input-label:hover {
            background-color: rgba(0, 0, 0, 0.1);
            border-color: black;
        }
        .glow-shadow {
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }
        .full-screen-details {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(var(--tg-theme-bg-color-rgb, 255, 255, 255), 0.95);
            backdrop-filter: blur(10px);
            z-index: 50;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1rem;
            overflow-y: auto;
        }
        .input-field {
            border: 2px solid black;
            background-color: white;
            color: black;
        }
        .input-field:focus {
            outline: none;
            box-shadow: 0 0 0 2px black;
        }
        .catalog-btn.active, .gender-btn.active {
            background-color: black;
            color: white;
        }
        .catalog-btn, .gender-btn {
            border: 2px solid black;
        }
        /* Стиль для кнопки "Купить" в полноэкранном режиме */
        #buy-btn {
            background-color: white;
            color: black;
            border: 2px solid black;
        }
        /* Стиль для кнопки "Вернуться в каталог" в полноэкранном режиме */
        #details-close-btn {
            background-color: white;
            color: black;
            border: 2px solid black;
        }
        /* Стиль для кнопок-фильтров */
        .catalog-btn, .gender-btn {
            background-color: var(--tg-theme-secondary-bg-color, #efeff3);
            color: var(--tg-theme-text-color, #000000);
            border: 2px solid var(--tg-theme-text-color, #000000);
        }
        .modal {
            position: fixed;
            inset: 0;
            z-index: 50;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: rgba(var(--tg-theme-bg-color-rgb, 255, 255, 255), 0.75);
            backdrop-filter: blur(10px);
        }
        .size-btn {
            background-color: white;
            color: black;
            border: 2px solid black;
        }
        .size-btn.active {
            background-color: black;
            color: white;
        }
    </style>
</head>
<body class="bg-white text-black min-h-screen p-4 flex flex-col items-center">

    <!-- Основной контейнер приложения -->
    <div id="main-content" class="w-full max-w-2xl bg-white rounded-box shadow-2xl p-6 relative">
        
        <!-- Верхняя панель: название бренда и кнопка админа -->
        <header class="flex justify-between items-center mb-6 border-b border-black pb-4">
            <h1 class="text-3xl md:text-4xl font-extrabold text-black tracking-wider flex-grow">LINKKORN</h1>
            <button id="admin-login-btn" class="bg-white text-black font-semibold py-1 px-3 rounded-btn text-sm transition duration-300 transform hover:scale-105">
                Войти как админ
            </button>
        </header>

        <!-- Меню выбора пола -->
        <div id="gender-menu" class="flex flex-wrap justify-center md:justify-start gap-2 mb-4">
            <button class="gender-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300 active" data-gender="all">Все</button>
            <button class="gender-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-gender="men">Мужское</button>
            <button class="gender-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-gender="women">Женское</button>
        </div>
        <!-- Меню каталогов -->
        <div id="catalog-menu" class="flex flex-wrap justify-center md:justify-start gap-2 mb-6">
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300 active" data-category="all">Все</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="bombers">Бомберы</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="raincoats">Плащи</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="jackets">Куртки</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="coats">Пальто</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="bags">Сумки</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="hats">Шапки</button>
            <button class="catalog-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300" data-category="skirts">Юбки</button>
        </div>

        <!-- Форма для добавления товара (только для админа) -->
        <div id="admin-panel" class="hidden mb-6 p-4 border-2 border-black rounded-box bg-white">
            <h2 class="text-xl font-semibold mb-4 text-black">Добавить новый товар</h2>
            <form id="add-product-form" class="space-y-4">
                <div>
                    <label for="product-gender" class="block text-sm font-medium text-black">Пол</label>
                    <select id="product-gender" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                        <option value="">Выберите пол</option>
                        <option value="men">Мужское</option>
                        <option value="women">Женское</option>
                    </select>
                </div>
                <div>
                    <label for="product-category" class="block text-sm font-medium text-black">Категория</label>
                    <select id="product-category" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                        <option value="">Выберите категорию</option>
                        <option value="bombers">Бомберы</option>
                        <option value="raincoats">Плащи</option>
                        <option value="jackets">Куртки</option>
                        <option value="coats">Пальто</option>
                        <option value="bags">Сумки</option>
                        <option value="hats">Шапки</option>
                        <option value="skirts">Юбки</option>
                    </select>
                </div>
                <div>
                    <label for="product-name" class="block text-sm font-medium text-black">Название товара</label>
                    <input type="text" id="product-name" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="product-description" class="block text-sm font-medium text-black">Краткое описание</label>
                    <textarea id="product-description" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black"></textarea>
                </div>
                <div>
                    <label for="product-details" class="block text-sm font-medium text-black">Подробное описание</label>
                    <textarea id="product-details" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black"></textarea>
                </div>
                <div>
                    <label for="product-price" class="block text-sm font-medium text-black">Цена</label>
                    <input type="number" id="product-price" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label class="block text-sm font-medium text-black">Размеры</label>
                    <div class="mt-1 flex flex-wrap gap-2">
                        <label class="flex items-center space-x-2">
                            <input type="checkbox" name="product-sizes" value="XS" class="rounded text-black focus:ring-black">
                            <span class="text-sm">XS</span>
                        </label>
                        <label class="flex items-center space-x-2">
                            <input type="checkbox" name="product-sizes" value="S" class="rounded text-black focus:ring-black">
                            <span class="text-sm">S</span>
                        </label>
                        <label class="flex items-center space-x-2">
                            <input type="checkbox" name="product-sizes" value="M" class="rounded text-black focus:ring-black">
                            <span class="text-sm">M</span>
                        </label>
                        <label class="flex items-center space-x-2">
                            <input type="checkbox" name="product-sizes" value="L" class="rounded text-black focus:ring-black">
                            <span class="text-sm">L</span>
                        </label>
                        <label class="flex items-center space-x-2">
                            <input type="checkbox" name="product-sizes" value="XL" class="rounded text-black focus:ring-black">
                            <span class="text-sm">XL</span>
                        </label>
                    </div>
                </div>
                <div>
                    <label for="product-image" class="block text-sm font-medium text-black">Файл с фото</label>
                    <label for="product-image-file" class="file-input-label mt-1 w-full text-black">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2 text-black" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                        </svg>
                        <span>Выберите файл...</span>
                    </label>
                    <input type="file" id="product-image-file" accept="image/*" class="hidden" required>
                    <img id="image-preview" src="#" alt="Предварительный просмотр" class="hidden mt-2 rounded-md object-cover w-full h-48 border-2 border-black">
                </div>
                <button type="submit" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-md transition duration-300 transform hover:scale-105 rounded-btn">
                    Добавить товар
                </button>
            </form>
        </div>

        <!-- Список товаров -->
        <main>
            <div id="product-list" class="space-y-6">
                <!-- Товары будут отображаться здесь -->
                <p id="empty-message" class="text-center text-gray-500 italic">Товары пока отсутствуют.</p>
            </div>
        </main>

    </div>

    <!-- Модальное окно для ввода пароля -->
    <div id="password-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-white bg-opacity-75 backdrop-blur-sm">
        <div class="bg-white p-6 rounded-box shadow-2xl border-2 border-black max-w-sm w-full text-center relative">
            <button id="password-close-btn" class="absolute top-2 right-2 text-black hover:text-gray-400 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
            <h3 class="text-xl font-semibold mb-4 text-black">Вход для администратора</h3>
            <input type="password" id="password-input" class="w-full px-3 py-2 mb-4 input-field rounded-md focus:ring-black focus:border-black" placeholder="Введите пароль">
            <button id="password-submit-btn" class="w-full bg-white text-black font-semibold py-2 px-6 rounded-btn transition duration-300">Войти</button>
            <p id="password-error" class="text-red-500 text-sm mt-2 hidden">Неверный пароль!</p>
        </div>
    </div>

    <!-- Модальное окно для сообщений -->
    <div id="modal-message" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-white bg-opacity-75 backdrop-blur-sm">
        <div class="bg-white p-6 rounded-box shadow-2xl border-2 border-black max-w-sm w-full text-center">
            <p id="modal-text" class="text-lg font-semibold text-black mb-4"></p>
            <button id="modal-close-btn" class="bg-white text-black font-semibold py-2 px-6 rounded-btn transition duration-300">OK</button>
        </div>
    </div>

    <!-- Полноэкранное представление товара -->
    <div id="details-view" class="hidden full-screen-details">
        <div class="bg-white rounded-box p-6 w-full max-w-4xl max-h-[90%] overflow-y-auto">
            <div class="flex flex-col md:flex-row gap-6">
                <div class="md:w-1/2 flex-shrink-0">
                    <img id="details-image" src="#" alt="Изображение товара" class="w-full rounded-md object-cover glow-shadow border-2 border-black mb-4">
                </div>
                <div class="md:w-1/2 flex flex-col justify-between">
                    <div>
                        <h2 id="details-title" class="text-3xl font-bold text-black mb-2"></h2>
                        <p id="details-description" class="text-gray-700 mb-4"></p>
                        <p id="details-text" class="text-black whitespace-pre-wrap mb-4 leading-relaxed"></p>
                        <!-- Блок для выбора размера -->
                        <div id="size-selection-container" class="mt-4 hidden">
                            <h4 class="text-sm font-bold text-black mb-2">Выберите размер:</h4>
                            <div id="size-buttons" class="flex flex-wrap gap-2">
                                <!-- Кнопки размеров будут здесь -->
                            </div>
                        </div>
                    </div>
                    <div class="flex items-center justify-between mt-4">
                        <p id="details-price" class="text-3xl font-extrabold text-black"></p>
                        <button id="buy-btn" class="bg-black text-white font-semibold py-3 px-8 rounded-btn transition duration-300 transform hover:scale-105">Купить</button>
                    </div>
                </div>
            </div>
            <div class="flex justify-center mt-6">
                <button id="details-close-btn" class="bg-white text-black font-semibold py-2 px-6 rounded-btn transition duration-300">Вернуться в каталог</button>
            </div>
        </div>
    </div>
    <!-- Модальное окно для анкеты клиента -->
    <div id="order-form-modal" class="modal hidden">
        <div class="bg-white p-6 rounded-box shadow-2xl border-2 border-black max-w-lg w-full relative">
            <h3 class="text-xl font-semibold mb-4 text-black">Оформление заказа</h3>
            <p id="order-form-size-info" class="text-sm text-gray-600 mb-4 hidden">Выбранный размер: <span id="selected-size-display" class="font-bold"></span></p>
            <form id="order-form" class="space-y-4">
                <div>
                    <label for="order-name" class="block text-sm font-medium text-black">Имя</label>
                    <input type="text" id="order-name" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-address" class="block text-sm font-medium text-black">Адрес</label>
                    <input type="text" id="order-address" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-email" class="block text-sm font-medium text-black">Email</label>
                    <input type="email" id="order-email" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-phone" class="block text-sm font-medium text-black">Номер телефона</label>
                    <input type="tel" id="order-phone" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <button type="submit" id="submit-order-btn" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                    Перейти к оплате
                </button>
                <p id="form-error" class="text-red-500 text-sm mt-2 hidden">Заполните все поля!</p>
                <button type="button" id="order-form-close-btn" class="mt-2 w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                    Отмена
                </button>
            </form>
        </div>
    </div>

    <!-- Модальное окно для оплаты -->
    <div id="payment-modal" class="modal hidden">
        <div class="bg-white p-6 rounded-box shadow-2xl border-2 border-black max-w-lg w-full relative">
            <h3 class="text-xl font-semibold mb-4 text-black text-center">Оплата заказа</h3>
            <p class="text-sm text-gray-600 text-center mb-4">Для оплаты переведите средства на указанную карту, затем нажмите "Я оплатил".</p>
            
            <div class="bg-gray-100 p-4 rounded-box border-2 border-black mb-4">
                <div class="flex justify-between items-center mb-2">
                    <span class="font-semibold text-black">Номер карты:</span>
                    <span id="card-number" class="text-black">1234 5678 9012 3456</span>
                    <button id="copy-card-btn" class="bg-white text-black py-1 px-3 rounded-btn text-xs hover:bg-gray-200 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline-block mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M8 5H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-2M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6 4h.01" />
                        </svg>
                        Копировать
                    </button>
                </div>
                <div class="flex justify-between items-center">
                    <span class="font-semibold text-black">Сумма к оплате:</span>
                    <span id="payment-price" class="font-bold text-black"></span>
                </div>
            </div>

            <button id="payment-confirm-btn" class="w-full bg-black text-white font-semibold py-3 px-6 rounded-btn transition duration-300 transform hover:scale-105">
                Я оплатил
            </button>
            <button id="payment-close-btn" class="mt-2 w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                Отмена
            </button>
            <p id="payment-error" class="text-red-500 text-sm mt-2 hidden text-center">Не удалось отправить данные заказа. Пожалуйста, попробуйте еще раз.</p>
        </div>
    </div>


    <script>
        // Имитация базы данных
        const productsDB = JSON.parse(localStorage.getItem('products')) || [];

        // Пароль администратора (можно изменить)
        const ADMIN_PASSWORD = "admin";

        // DOM элементы
        const mainContent = document.getElementById('main-content');
        const productList = document.getElementById('product-list');
        const emptyMessage = document.getElementById('empty-message');
        const adminLoginBtn = document.getElementById('admin-login-btn');
        const passwordModal = document.getElementById('password-modal');
        const passwordInput = document.getElementById('password-input');
        const passwordSubmitBtn = document.getElementById('password-submit-btn');
        const passwordCloseBtn = document.getElementById('password-close-btn');
        const passwordError = document.getElementById('password-error');
        const adminPanel = document.getElementById('admin-panel');
        const addProductForm = document.getElementById('add-product-form');
        const productImageFile = document.getElementById('product-image-file');
        const imagePreview = document.getElementById('image-preview');
        const fileInputLabel = document.querySelector('.file-input-label span');
        const catalogButtons = document.querySelectorAll('#catalog-menu .catalog-btn');
        const genderButtons = document.querySelectorAll('#gender-menu .gender-btn');

        // Добавляем DOM-элементы для модальных окон
        const detailsView = document.getElementById('details-view');
        const detailsCloseBtn = document.getElementById('details-close-btn');
        const buyBtn = document.getElementById('buy-btn');
        const orderFormModal = document.getElementById('order-form-modal');
        const orderForm = document.getElementById('order-form');
        const orderFormCloseBtn = document.getElementById('order-form-close-btn');
        const paymentModal = document.getElementById('payment-modal');
        const paymentConfirmBtn = document.getElementById('payment-confirm-btn');
        const paymentCloseBtn = document.getElementById('payment-close-btn');
        const copyCardBtn = document.getElementById('copy-card-btn');
        const modalMessage = document.getElementById('modal-message');
        const modalText = document.getElementById('modal-text');
        const modalCloseBtn = document.getElementById('modal-close-btn');

        // Переменные состояния
        let currentCategory = 'all';
        let currentGender = 'all';
        let selectedProduct = null;
        let selectedSize = null;
        let orderData = {};

        // ----------------- Функции для работы с данными и отображением -----------------

        // Функция для скрытия всех модальных окон и возврата на главную
        function hideAllModals() {
            passwordModal.classList.add('hidden');
            detailsView.classList.add('hidden');
            orderFormModal.classList.add('hidden');
            paymentModal.classList.add('hidden');
            modalMessage.classList.add('hidden');
        }

        // Функция для отображения каталога
        function displayProducts(products) {
            productList.innerHTML = '';
            if (products.length === 0) {
                emptyMessage.classList.remove('hidden');
            } else {
                emptyMessage.classList.add('hidden');
                products.forEach(product => {
                    const productCard = createProductCard(product);
                    productList.appendChild(productCard);
                });
            }
        }

        // Функция для фильтрации товаров
        function filterProducts() {
            const filtered = productsDB.filter(product => {
                const categoryMatch = currentCategory === 'all' || product.category === currentCategory;
                const genderMatch = currentGender === 'all' || product.gender === currentGender;
                return categoryMatch && genderMatch;
            });
            displayProducts(filtered);
        }

        // Функция для смены категории
        function changeCategory(category) {
            currentCategory = category;
            catalogButtons.forEach(button => {
                button.classList.remove('active');
                if (button.dataset.category === category) {
                    button.classList.add('active');
                }
            });
            filterProducts();
        }

        // Функция для смены пола
        function changeGender(gender) {
            currentGender = gender;
            genderButtons.forEach(button => {
                button.classList.remove('active');
                if (button.dataset.gender === gender) {
                    button.classList.add('active');
                }
            });
            filterProducts();
        }

        // Функция для создания карточки товара
        function createProductCard(product) {
            const card = document.createElement('div');
            card.className = 'product-card rounded-box p-4 flex flex-col items-center justify-between glow-shadow transition transform hover:scale-105 duration-300 cursor-pointer';
            card.dataset.id = product.id;
            card.innerHTML = `
                <div class="product-image-container mb-4 rounded-md overflow-hidden border-2 border-black">
                    <img src="${product.imageUrl}" alt="${product.name}" class="product-image">
                </div>
                <h3 class="product-name text-center text-lg md:text-xl font-bold mb-2">${product.name}</h3>
                <p class="product-description text-center text-sm text-gray-700 mb-4 truncate w-full">${product.description}</p>
                <div class="w-full flex justify-between items-center mt-auto">
                    <span class="product-price text-xl md:text-2xl font-extrabold">${product.price} ₽</span>
                </div>
            `;
            card.addEventListener('click', () => showProductDetails(product));
            return card;
        }

        // Функция для отображения деталей товара
        function showProductDetails(product) {
            selectedProduct = product;
            document.getElementById('details-image').src = product.imageUrl;
            document.getElementById('details-title').textContent = product.name;
            document.getElementById('details-description').textContent = product.description;
            document.getElementById('details-text').textContent = product.details;
            document.getElementById('details-price').textContent = `${product.price} ₽`;

            const sizeButtonsContainer = document.getElementById('size-buttons');
            const sizeSelectionContainer = document.getElementById('size-selection-container');
            sizeButtonsContainer.innerHTML = '';
            selectedSize = null;

            if (product.sizes && product.sizes.length > 0) {
                sizeSelectionContainer.classList.remove('hidden');
                product.sizes.forEach(size => {
                    const btn = document.createElement('button');
                    btn.className = 'size-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300';
                    btn.textContent = size;
                    btn.dataset.size = size;
                    btn.addEventListener('click', () => {
                        document.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
                        btn.classList.add('active');
                        selectedSize = size;
                    });
                    sizeButtonsContainer.appendChild(btn);
                });
            } else {
                sizeSelectionContainer.classList.add('hidden');
            }

            detailsView.classList.remove('hidden');
        }

        // Функция для очистки формы добавления товара
        function clearAddProductForm() {
            addProductForm.reset();
            imagePreview.src = '#';
            imagePreview.classList.add('hidden');
            fileInputLabel.textContent = 'Выберите файл...';
        }

        // Функция для сохранения товара в localStorage
        function saveProducts() {
            localStorage.setItem('products', JSON.stringify(productsDB));
        }

        // Функция для добавления нового товара
        function addProduct(e) {
            e.preventDefault();
            const gender = document.getElementById('product-gender').value;
            const category = document.getElementById('product-category').value;
            const name = document.getElementById('product-name').value;
            const description = document.getElementById('product-description').value;
            const details = document.getElementById('product-details').value;
            const price = document.getElementById('product-price').value;
            const sizes = Array.from(document.querySelectorAll('input[name="product-sizes"]:checked')).map(el => el.value);

            const file = productImageFile.files[0];
            const reader = new FileReader();

            reader.onload = function(e) {
                const imageUrl = e.target.result;
                const newProduct = {
                    id: Date.now(),
                    gender: gender,
                    category: category,
                    name: name,
                    description: description,
                    details: details,
                    price: price,
                    sizes: sizes,
                    imageUrl: imageUrl
                };
                productsDB.push(newProduct);
                saveProducts();
                filterProducts();
                clearAddProductForm();
                showMessageModal('Товар успешно добавлен!');
            };

            reader.readAsDataURL(file);
        }

        // Функция для отображения модального окна с сообщением
        function showMessageModal(message) {
            modalText.textContent = message;
            modalMessage.classList.remove('hidden');
        }

        // ----------------- Инициализация и обработка событий -----------------

        // При загрузке страницы, убеждаемся, что все модальные окна скрыты
        // Это решает проблему с открытием не той страницы
        window.onload = function() {
            hideAllModals();
            filterProducts(); // Отображаем товары при загрузке
        };

        adminLoginBtn.addEventListener('click', () => {
            passwordModal.classList.remove('hidden');
        });

        passwordSubmitBtn.addEventListener('click', () => {
            if (passwordInput.value === ADMIN_PASSWORD) {
                passwordModal.classList.add('hidden');
                adminPanel.classList.remove('hidden');
                showMessageModal('Добро пожаловать, администратор!');
            } else {
                passwordError.classList.remove('hidden');
            }
        });

        passwordCloseBtn.addEventListener('click', () => {
            passwordModal.classList.add('hidden');
            passwordError.classList.add('hidden');
        });

        passwordInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                passwordSubmitBtn.click();
            }
        });

        addProductForm.addEventListener('submit', addProduct);

        productImageFile.addEventListener('change', function() {
            if (this.files && this.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    imagePreview.src = e.target.result;
                    imagePreview.classList.remove('hidden');
                    fileInputLabel.textContent = this.files[0].name;
                }.bind(this);
                reader.readAsDataURL(this.files[0]);
            } else {
                imagePreview.classList.add('hidden');
                fileInputLabel.textContent = 'Выберите файл...';
            }
        });

        catalogButtons.forEach(button => {
            button.addEventListener('click', (e) => {
                changeCategory(e.target.dataset.category);
            });
        });

        genderButtons.forEach(button => {
            button.addEventListener('click', (e) => {
                changeGender(e.target.dataset.gender);
            });
        });

        // Обработчики событий для модальных окон
        detailsCloseBtn.addEventListener('click', () => {
            detailsView.classList.add('hidden');
        });

        buyBtn.addEventListener('click', () => {
            if (!selectedSize && selectedProduct.sizes.length > 0) {
                showMessageModal('Пожалуйста, выберите размер.');
                return;
            }
            detailsView.classList.add('hidden');
            document.getElementById('selected-size-display').textContent = selectedSize || 'N/A';
            document.getElementById('order-form-size-info').classList.remove('hidden');
            orderFormModal.classList.remove('hidden');
        });

        orderFormCloseBtn.addEventListener('click', () => {
            orderFormModal.classList.add('hidden');
        });

        orderForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const name = document.getElementById('order-name').value;
            const address = document.getElementById('order-address').value;
            const email = document.getElementById('order-email').value;
            const phone = document.getElementById('order-phone').value;

            if (name && address && email && phone) {
                orderData = {
                    product_id: selectedProduct.id,
                    product_name: selectedProduct.name,
                    size: selectedSize || 'N/A',
                    product_price: selectedProduct.price,
                    name: name,
                    address: address,
                    email: email,
                    phone: phone
                };
                orderFormModal.classList.add('hidden');
                document.getElementById('payment-price').textContent = `${orderData.product_price} ₽`;
                paymentModal.classList.remove('hidden');
            } else {
                document.getElementById('form-error').classList.remove('hidden');
            }
        });

        paymentCloseBtn.addEventListener('click', () => {
            paymentModal.classList.add('hidden');
        });

        paymentConfirmBtn.addEventListener('click', async () => {
            const dataToSend = {
                action: 'order_completed',
                payload: orderData
            };
            try {
                // Отправляем данные в бота
                Telegram.WebApp.sendData(JSON.stringify(dataToSend));
                showMessageModal('Ваш заказ отправлен! Вскоре с вами свяжется администратор.');
                paymentModal.classList.add('hidden');
            } catch (error) {
                console.error("Failed to send data to Telegram bot:", error);
                document.getElementById('payment-error').classList.remove('hidden');
            }
        });

        copyCardBtn.addEventListener('click', () => {
            const cardNumber = document.getElementById('card-number').textContent;
            navigator.clipboard.writeText(cardNumber).then(() => {
                showMessageModal('Номер карты скопирован!');
            }).catch(err => {
                console.error('Не удалось скопировать номер карты:', err);
                showMessageModal('Не удалось скопировать. Пожалуйста, скопируйте вручную.');
            });
        });

        modalCloseBtn.addEventListener('click', () => {
            modalMessage.classList.add('hidden');
        });
    </script>
</body>
</html>
