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
        /* Стиль для кнопки "Купить" */
        #buy-btn {
            background-color: var(--tg-theme-button-color, #2481cc);
            color: var(--tg-theme-button-text-color, #ffffff);
            border: none;
        }
        /* Стиль для кнопки "Вернуться в каталог" */
        #details-close-btn {
            background-color: var(--tg-theme-button-color, #2481cc);
            color: var(--tg-theme-button-text-color, #ffffff);
            border: none;
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
        .payment-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(5px);
        }
        .payment-modal-content {
            background-color: white;
            padding: 2rem;
            border-radius: 1rem;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .payment-qr {
            width: 100%;
            height: auto;
            max-width: 300px;
            margin: 1rem auto;
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
                        <button id="buy-btn" class="bg-white text-black font-semibold py-3 px-8 rounded-btn transition duration-300 transform hover:scale-105">Купить</button>
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
            <button id="order-form-close-btn" class="absolute top-2 right-2 text-black hover:text-gray-400 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
            <h3 class="text-xl font-semibold mb-4 text-black">Оформление заказа</h3>
            <p id="order-form-size-info" class="text-sm text-gray-600 mb-4 hidden">Выбранный размер: <span id="selected-size-display" class="font-bold"></span></p>
            <form id="order-form" class="space-y-4">
                <div>
                    <label for="order-name" class="block text-sm font-medium text-black">Ваше имя</label>
                    <input type="text" id="order-name" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-phone" class="block text-sm font-medium text-black">Номер телефона</label>
                    <input type="tel" id="order-phone" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-email" class="block text-sm font-medium text-black">Email</label>
                    <input type="email" id="order-email" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-address" class="block text-sm font-medium text-black">Адрес доставки</label>
                    <textarea id="order-address" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black"></textarea>
                </div>
                <button type="submit" id="submit-order-btn" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                    Перейти к оплате
                </button>
            </form>
        </div>
    </div>

    <!-- Модальное окно для оплаты -->
    <div id="payment-modal" class="hidden payment-modal">
        <div class="payment-modal-content rounded-box border-2 border-black text-center relative">
            <button id="payment-close-btn" class="absolute top-2 right-2 text-black hover:text-gray-400 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
            <h3 class="text-2xl font-bold mb-4 text-black">Оплата заказа</h3>
            <p class="text-gray-700 mb-2">Отсканируйте QR-код для оплаты.</p>
            <img id="payment-qr" class="payment-qr border-2 border-black" src="https://placehold.co/300x300/000000/ffffff?text=QR+Code" alt="QR-код для оплаты">
            <p class="text-sm text-gray-500 mb-4">Сумма к оплате: <span id="payment-amount" class="font-bold text-black"></span></p>
            <p class="text-sm font-bold text-black mt-4">Или переведите по номеру:</p>
            <p id="payment-phone" class="text-black mb-4"></p>
            <button id="paid-btn" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                Я оплатил
            </button>
        </div>
    </div>

    <script>
        const API_URL = 'https://linkkorn.github.io/linkkorn-web-app/'; // Замени на свою ссылку GitHub Pages
        const ADMIN_PASSWORD = 'admin'; // Замени на свой пароль админа
        const products = JSON.parse(localStorage.getItem('products')) || [];

        // UI-элементы
        const adminLoginBtn = document.getElementById('admin-login-btn');
        const adminPanel = document.getElementById('admin-panel');
        const passwordModal = document.getElementById('password-modal');
        const passwordInput = document.getElementById('password-input');
        const passwordSubmitBtn = document.getElementById('password-submit-btn');
        const passwordCloseBtn = document.getElementById('password-close-btn');
        const passwordError = document.getElementById('password-error');
        const addProductForm = document.getElementById('add-product-form');
        const productList = document.getElementById('product-list');
        const emptyMessage = document.getElementById('empty-message');
        const productImageFile = document.getElementById('product-image-file');
        const imagePreview = document.getElementById('image-preview');
        const fileInputLabel = document.querySelector('.file-input-label span');
        const catalogButtons = document.querySelectorAll('.catalog-btn');
        const genderButtons = document.querySelectorAll('.gender-btn');
        
        // Модальное окно сообщений
        const modalMessage = document.getElementById('modal-message');
        const modalText = document.getElementById('modal-text');
        const modalCloseBtn = document.getElementById('modal-close-btn');

        // Модальное окно деталей товара
        const detailsView = document.getElementById('details-view');
        const detailsImage = document.getElementById('details-image');
        const detailsTitle = document.getElementById('details-title');
        const detailsDescription = document.getElementById('details-description');
        const detailsText = document.getElementById('details-text');
        const detailsPrice = document.getElementById('details-price');
        const detailsCloseBtn = document.getElementById('details-close-btn');
        const buyBtn = document.getElementById('buy-btn');
        const sizeButtonsContainer = document.getElementById('size-buttons');
        const sizeSelectionContainer = document.getElementById('size-selection-container');
        
        // Модальное окно формы заказа
        const orderFormModal = document.getElementById('order-form-modal');
        const orderFormCloseBtn = document.getElementById('order-form-close-btn');
        const orderForm = document.getElementById('order-form');
        const orderFormSizeInfo = document.getElementById('order-form-size-info');
        const selectedSizeDisplay = document.getElementById('selected-size-display');
        const submitOrderBtn = document.getElementById('submit-order-btn');

        // Модальное окно оплаты
        const paymentModal = document.getElementById('payment-modal');
        const paymentCloseBtn = document.getElementById('payment-close-btn');
        const paidBtn = document.getElementById('paid-btn');
        const paymentAmount = document.getElementById('payment-amount');
        const paymentPhone = document.getElementById('payment-phone');

        let currentCategory = 'all';
        let currentGender = 'all';
        let currentProductData = {};
        let selectedSize = '';
        let isAdmin = false;

        // Инициализация Telegram Web App
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();

        // Функции
        function showMessage(message) {
            modalText.textContent = message;
            modalMessage.classList.remove('hidden');
        }

        function renderProducts(filterCategory, filterGender) {
            productList.innerHTML = '';
            const filteredProducts = products.filter(product => {
                const categoryMatch = filterCategory === 'all' || product.category === filterCategory;
                const genderMatch = filterGender === 'all' || product.gender === filterGender;
                return categoryMatch && genderMatch;
            });

            if (filteredProducts.length === 0) {
                emptyMessage.classList.remove('hidden');
            } else {
                emptyMessage.classList.add('hidden');
                filteredProducts.forEach(product => {
                    const productElement = document.createElement('div');
                    productElement.className = 'product-card bg-white p-4 rounded-box shadow-lg border-2 border-black transition-transform duration-300 transform hover:scale-[1.02] cursor-pointer';
                    productElement.innerHTML = `
                        <div class="product-image-container rounded-md overflow-hidden mb-4 border-2 border-black">
                            <img src="${product.image}" alt="${product.name}" class="object-cover w-full h-full">
                        </div>
                        <h3 class="text-lg font-bold text-black">${product.name}</h3>
                        <p class="text-gray-700 text-sm mb-2">${product.description}</p>
                        <p class="text-xl font-extrabold text-black">${product.price} ₽</p>
                    `;
                    productElement.addEventListener('click', () => showProductDetails(product));
                    productList.appendChild(productElement);
                });
            }
        }

        function saveProducts() {
            localStorage.setItem('products', JSON.stringify(products));
            renderProducts(currentCategory, currentGender);
        }

        function addProduct(event) {
            event.preventDefault();

            const gender = document.getElementById('product-gender').value;
            const category = document.getElementById('product-category').value;
            const name = document.getElementById('product-name').value;
            const description = document.getElementById('product-description').value;
            const details = document.getElementById('product-details').value;
            const price = parseFloat(document.getElementById('product-price').value);
            const sizes = Array.from(document.querySelectorAll('input[name="product-sizes"]:checked')).map(cb => cb.value);

            const file = productImageFile.files[0];
            if (!file) {
                showMessage('Пожалуйста, выберите изображение!');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const newProduct = {
                    id: Date.now(),
                    gender,
                    category,
                    name,
                    description,
                    details,
                    price,
                    sizes,
                    image: e.target.result // Base64 image
                };
                products.push(newProduct);
                saveProducts();
                addProductForm.reset();
                imagePreview.classList.add('hidden');
                fileInputLabel.textContent = 'Выберите файл...';
                showMessage('Товар успешно добавлен!');
            };
            reader.readAsDataURL(file);
        }

        function showProductDetails(product) {
            currentProductData = product;
            detailsImage.src = product.image;
            detailsTitle.textContent = product.name;
            detailsDescription.textContent = product.description;
            detailsText.textContent = product.details;
            detailsPrice.textContent = `${product.price} ₽`;
            
            // Render sizes dynamically
            sizeButtonsContainer.innerHTML = '';
            if (product.sizes && product.sizes.length > 0) {
                sizeSelectionContainer.classList.remove('hidden');
                product.sizes.forEach(size => {
                    const sizeButton = document.createElement('button');
                    sizeButton.className = 'size-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300';
                    sizeButton.textContent = size;
                    sizeButton.addEventListener('click', () => {
                        document.querySelectorAll('.size-btn').forEach(btn => btn.classList.remove('active'));
                        sizeButton.classList.add('active');
                        selectedSize = size;
                    });
                    sizeButtonsContainer.appendChild(sizeButton);
                });
            } else {
                sizeSelectionContainer.classList.add('hidden');
            }

            detailsView.classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }

        function showOrderForm() {
            if (!selectedSize && sizeSelectionContainer.classList.contains('hidden') === false) {
                showMessage('Пожалуйста, выберите размер!');
                return;
            }
            orderFormModal.classList.remove('hidden');
            detailsView.classList.add('hidden');
            selectedSizeDisplay.textContent = selectedSize;
            if (selectedSize) {
                orderFormSizeInfo.classList.remove('hidden');
            } else {
                orderFormSizeInfo.classList.add('hidden');
            }
        }
        
        function showPaymentModal(orderInfo) {
            paymentModal.classList.remove('hidden');
            orderFormModal.classList.add('hidden');
            paymentAmount.textContent = `${orderInfo.product_price} ₽`;
            paymentPhone.textContent = '8-999-999-99-99'; // Замени на свой номер телефона
        }

        function sendOrderToBot(orderInfo) {
            if (Telegram.WebApp.isExpanded) {
                Telegram.WebApp.sendData(JSON.stringify(orderInfo));
                showMessage("Ваш заказ отправлен! Мы свяжемся с вами в ближайшее время.");
                // Закрываем модальное окно оплаты
                paymentModal.classList.add('hidden');
                document.body.style.overflow = 'auto';
            } else {
                showMessage("Ошибка: Мини-приложение Telegram недоступно. Пожалуйста, откройте его в Telegram.");
            }
        }

        function changeCategory(category) {
            currentCategory = category;
            catalogButtons.forEach(btn => {
                btn.classList.remove('active');
                if (btn.dataset.category === category) {
                    btn.classList.add('active');
                }
            });
            renderProducts(currentCategory, currentGender);
        }

        function changeGender(gender) {
            currentGender = gender;
            genderButtons.forEach(btn => {
                btn.classList.remove('active');
                if (btn.dataset.gender === gender) {
                    btn.classList.add('active');
                }
            });
            renderProducts(currentCategory, currentGender);
        }

        // Обработчики событий
        adminLoginBtn.addEventListener('click', () => {
            passwordModal.classList.remove('hidden');
            passwordInput.focus();
        });

        passwordSubmitBtn.addEventListener('click', () => {
            if (passwordInput.value === ADMIN_PASSWORD) {
                adminPanel.classList.remove('hidden');
                passwordModal.classList.add('hidden');
                isAdmin = true;
                showMessage('Вы вошли как администратор!');
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
        orderForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const orderInfo = {
                product_id: currentProductData.id,
                product_name: currentProductData.name,
                product_price: currentProductData.price,
                size: selectedSize,
                name: document.getElementById('order-name').value,
                phone: document.getElementById('order-phone').value,
                email: document.getElementById('order-email').value,
                address: document.getElementById('order-address').value
            };
            showPaymentModal(orderInfo);
            // Обработчик кнопки "Я оплатил"
            paidBtn.addEventListener('click', () => {
                sendOrderToBot(orderInfo);
            }, { once: true }); // Использование { once: true } гарантирует, что обработчик будет вызван только один раз
        });

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

        buyBtn.addEventListener('click', showOrderForm);
        detailsCloseBtn.addEventListener('click', () => {
            detailsView.classList.add('hidden');
            document.body.style.overflow = 'auto';
        });

        orderFormCloseBtn.addEventListener('click', () => {
            orderFormModal.classList.add('hidden');
            document.body.style.overflow = 'auto';
            // Возвращаемся к деталям, чтобы пользователь мог выбрать другой размер
            showProductDetails(currentProductData);
        });

        paymentCloseBtn.addEventListener('click', () => {
            paymentModal.classList.add('hidden');
            document.body.style.overflow = 'auto';
        });

        modalCloseBtn.addEventListener('click', () => {
            modalMessage.classList.add('hidden');
        });

        // Инициализация
        renderProducts(currentCategory, currentGender);
    </script>
</body>
</html>
