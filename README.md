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
            <h3 class="text-xl font-semibold mb-4 text-black">Оформление заказа</h3>
            <p id="order-form-size-info" class="text-sm text-gray-600 mb-4 hidden">Выбранный размер: <span id="selected-size-display" class="font-bold"></span></p>
            <form id="order-form" class="space-y-4">
                <div>
                    <label for="order-name" class="block text-sm font-medium text-black">Имя</label>
                    <input type="text" id="order-name" required class="mt-1 block w-full px-3 py-2 input-field rounded-md focus:ring-black focus:border-black">
                </div>
                <div>
                    <label for="order-address" class="block text-sm font-medium text-black">Адрес доставки</label>
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
                <button type="submit" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                    Готово
                </button>
            </form>
            <button id="order-form-close-btn" class="absolute top-2 right-2 text-black hover:text-gray-400 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
        </div>
    </div>

    <!-- Модальное окно для оплаты -->
    <div id="payment-modal" class="modal hidden">
        <div class="bg-white p-6 rounded-box shadow-2xl border-2 border-black max-w-md w-full relative text-center">
            <h3 class="text-xl font-semibold mb-4 text-black">Реквизиты для оплаты</h3>
            <p class="text-sm font-medium text-gray-700">Номер карты:</p>
            <p id="card-number" class="text-lg font-bold text-black mb-4 select-all">22023940450239032</p>
            <p class="text-sm font-medium text-gray-700">Сумма к оплате:</p>
            <p id="payment-amount" class="text-2xl font-extrabold text-black mb-4"></p>
            <p id="payment-size" class="text-sm font-medium text-gray-700 mb-6"></p>
            <button id="payment-completed-btn" class="w-full bg-white text-black font-semibold py-2 px-4 rounded-btn transition duration-300 transform hover:scale-105">
                Я оплатил
            </button>
            <button id="payment-close-btn" class="absolute top-2 right-2 text-black hover:text-gray-400 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
        </div>
    </div>

    <script>
        // Инициализация элементов DOM
        const adminLoginBtn = document.getElementById('admin-login-btn');
        const adminPanel = document.getElementById('admin-panel');
        const addProductForm = document.getElementById('add-product-form');
        const productList = document.getElementById('product-list');
        const emptyMessage = document.getElementById('empty-message');
        const modalMessage = document.getElementById('modal-message');
        const modalText = document.getElementById('modal-text');
        const modalCloseBtn = document.getElementById('modal-close-btn');

        const passwordModal = document.getElementById('password-modal');
        const passwordInput = document.getElementById('password-input');
        const passwordSubmitBtn = document.getElementById('password-submit-btn');
        const passwordError = document.getElementById('password-error');
        const passwordCloseBtn = document.getElementById('password-close-btn');
        
        const detailsView = document.getElementById('details-view');
        const detailsTitle = document.getElementById('details-title');
        const detailsImage = document.getElementById('details-image');
        const detailsDescription = document.getElementById('details-description');
        const detailsText = document.getElementById('details-text');
        const detailsPrice = document.getElementById('details-price');
        const detailsCloseBtn = document.getElementById('details-close-btn');
        const mainContent = document.getElementById('main-content');
        const buyBtn = document.getElementById('buy-btn');

        const productImageFile = document.getElementById('product-image-file');
        const imagePreview = document.getElementById('image-preview');
        const fileInputLabel = document.querySelector('.file-input-label span');
        const productDetailsInput = document.getElementById('product-details');

        const genderButtons = document.querySelectorAll('.gender-btn');
        const catalogButtons = document.querySelectorAll('.catalog-btn');

        const sizeSelectionContainer = document.getElementById('size-selection-container');
        const sizeButtonsContainer = document.getElementById('size-buttons');
        
        // Элементы для новых модальных окон
        const orderFormModal = document.getElementById('order-form-modal');
        const orderForm = document.getElementById('order-form');
        const orderFormCloseBtn = document.getElementById('order-form-close-btn');
        const orderNameInput = document.getElementById('order-name');
        const orderAddressInput = document.getElementById('order-address');
        const orderEmailInput = document.getElementById('order-email');
        const orderPhoneInput = document.getElementById('order-phone');
        
        const paymentModal = document.getElementById('payment-modal');
        const paymentAmount = document.getElementById('payment-amount');
        const paymentSize = document.getElementById('payment-size');
        const paymentCompletedBtn = document.getElementById('payment-completed-btn');
        const paymentCloseBtn = document.getElementById('payment-close-btn');
        const orderFormSizeInfo = document.getElementById('order-form-size-info');
        const selectedSizeDisplay = document.getElementById('selected-size-display');
        
        let isAdmin = false;
        let currentGender = 'all';
        let currentCategory = 'all';
        let currentProduct = null; 
        let currentOrderData = {};
        let selectedSize = null;

        // Инициализируем Telegram Web App
        if (typeof Telegram !== 'undefined' && Telegram.WebApp) {
            Telegram.WebApp.ready();
        }

        // Показывает модальное окно с сообщением
        function showMessageModal(message) {
            modalText.textContent = message;
            modalMessage.classList.remove('hidden');
        }

        // Скрывает модальное окно сообщений
        modalCloseBtn.addEventListener('click', () => {
            modalMessage.classList.add('hidden');
        });
        
        // Скрывает полноэкранное представление
        detailsCloseBtn.addEventListener('click', () => {
            detailsView.classList.add('hidden');
            mainContent.classList.remove('hidden'); 
            currentProduct = null;
            selectedSize = null;
        });

        // Скрывает модальное окно анкеты
        orderFormCloseBtn.addEventListener('click', () => {
            orderFormModal.classList.add('hidden');
        });

        // Скрывает модальное окно оплаты
        paymentCloseBtn.addEventListener('click', () => {
            paymentModal.classList.add('hidden');
        });

        // Переключает режим администратора
        function toggleAdminMode() {
            isAdmin = !isAdmin;
            adminLoginBtn.textContent = isAdmin ? 'Выйти' : 'Войти как админ';
            adminPanel.classList.toggle('hidden', !isAdmin);
            renderProducts(); 
        }

        // Получает товары из localStorage
        function getProducts() {
            try {
                const products = JSON.parse(localStorage.getItem('products')) || [];
                return products;
            } catch (e) {
                console.error("Не удалось разобрать данные из localStorage:", e);
                return [];
            }
        }

        // Сохраняет товары в localStorage
        function saveProducts(products) {
            localStorage.setItem('products', JSON.stringify(products));
        }

        // Отрисовывает список товаров
        function renderProducts() {
            const products = getProducts();
            productList.innerHTML = ''; 

            const filteredProducts = products.filter(product => 
                (currentGender === 'all' || product.gender === currentGender) &&
                (currentCategory === 'all' || product.category === currentCategory)
            );

            if (filteredProducts.length === 0) {
                emptyMessage.classList.remove('hidden');
            } else {
                emptyMessage.classList.add('hidden');
                filteredProducts.forEach(product => {
                    const productDiv = document.createElement('div');
                    productDiv.className = 'product-card bg-white rounded-box overflow-hidden shadow-lg flex flex-col md:flex-row transition duration-300 transform hover:scale-[1.02] hover:shadow-2xl cursor-pointer';
                    productDiv.dataset.id = product.id; 

                    productDiv.innerHTML = `
                        <div class="product-image-container md:w-1/3 md:h-auto overflow-hidden">
                            <img src="${product.image}" onerror="this.src='https://placehold.co/400x225/ffffff/000000?text=LINKKORN'" alt="${product.name}" class="w-full h-full object-cover">
                        </div>
                        <div class="flex-1 p-4 flex flex-col justify-between">
                            <div>
                                <h3 class="text-xl font-bold text-black mb-1">${product.name}</h3>
                                <p class="text-gray-700 text-sm mb-2">Пол: ${product.gender === 'men' ? 'Мужской' : 'Женский'}</p>
                                <p class="text-gray-700 text-sm mb-2">${product.description}</p>
                                <p class="text-lg font-semibold text-black">${product.price} ₽</p>
                            </div>
                            <div class="flex items-center justify-end">
                                <button data-id="${product.id}" class="delete-btn bg-white text-black font-semibold py-1 px-3 rounded-btn mt-2 transition duration-300 ${isAdmin ? '' : 'hidden'}">Удалить</button>
                            </div>
                        </div>
                    `;
                    productList.appendChild(productDiv);
                });

                document.querySelectorAll('.delete-btn').forEach(button => {
                    button.addEventListener('click', (e) => {
                        e.stopPropagation(); 
                        const productId = e.target.dataset.id;
                        deleteProduct(productId);
                    });
                });
                
                document.querySelectorAll('.product-card').forEach(card => {
                    card.addEventListener('click', (e) => {
                        const productId = e.currentTarget.dataset.id;
                        const product = getProducts().find(p => p.id === productId);
                        if (product) {
                            showProductDetails(product);
                        }
                    });
                });
            }
        }

        function showProductDetails(product) {
            detailsTitle.textContent = product.name;
            detailsImage.src = product.image;
            detailsDescription.textContent = `Пол: ${product.gender === 'men' ? 'Мужской' : 'Женский'}\n${product.description}`;
            detailsText.textContent = product.details;
            detailsPrice.textContent = `${product.price} ₽`;
            
            currentProduct = product; 
            
            // Отрисовываем кнопки размеров
            sizeButtonsContainer.innerHTML = '';
            selectedSize = null; 
            if (product.sizes && product.sizes.length > 0) {
                sizeSelectionContainer.classList.remove('hidden');
                product.sizes.forEach(size => {
                    const button = document.createElement('button');
                    button.textContent = size;
                    button.className = 'size-btn py-2 px-4 rounded-btn text-sm font-semibold transition duration-300';
                    button.dataset.size = size;
                    button.addEventListener('click', (e) => {
                        document.querySelectorAll('.size-btn').forEach(btn => btn.classList.remove('active'));
                        e.target.classList.add('active');
                        selectedSize = e.target.dataset.size;
                    });
                    sizeButtonsContainer.appendChild(button);
                });
            } else {
                sizeSelectionContainer.classList.add('hidden');
            }

            mainContent.classList.add('hidden');
            detailsView.classList.remove('hidden');
        }

        function addProduct(event) {
            event.preventDefault();

            if (!productImageFile.files.length) {
                showMessageModal('Пожалуйста, выберите файл с фото!');
                return;
            }
            
            // Получаем выбранные размеры
            const selectedSizes = Array.from(document.querySelectorAll('input[name="product-sizes"]:checked')).map(checkbox => checkbox.value);
            if (selectedSizes.length === 0) {
                showMessageModal('Пожалуйста, выберите хотя бы один размер!');
                return;
            }

            const gender = document.getElementById('product-gender').value;
            const category = document.getElementById('product-category').value;
            const name = document.getElementById('product-name').value;
            const description = document.getElementById('product-description').value;
            const details = productDetailsInput.value; 
            const price = document.getElementById('product-price').value;
            const file = productImageFile.files[0];

            if (!category) {
                showMessageModal('Пожалуйста, выберите категорию для товара!');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const newProduct = {
                    id: Date.now().toString(), 
                    gender,
                    category,
                    name,
                    description,
                    details,
                    price: parseFloat(price),
                    sizes: selectedSizes, // Сохраняем выбранные размеры
                    image: e.target.result 
                };

                const products = getProducts();
                products.push(newProduct);
                saveProducts(products);
                renderProducts();
                addProductForm.reset();
                imagePreview.classList.add('hidden');
                fileInputLabel.textContent = 'Выберите файл...';
                showMessageModal('Товар успешно добавлен!');
            };
            reader.readAsDataURL(file);
        }

        function deleteProduct(productId) {
            let products = getProducts();
            products = products.filter(product => product.id !== productId);
            saveProducts(products);
            renderProducts();
            showMessageModal('Товар успешно удален!');
        }

        function changeCategory(category) {
            currentCategory = category;
            catalogButtons.forEach(btn => {
                if (btn.dataset.category === category) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            renderProducts();
        }

        function changeGender(gender) {
            currentGender = gender;
            genderButtons.forEach(btn => {
                if (btn.dataset.gender === gender) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            renderProducts();
        }

        // ОБНОВЛЕННЫЙ ОБРАБОТЧИК ДЛЯ КНОПКИ "КУПИТЬ"
        buyBtn.addEventListener('click', () => {
            if (!currentProduct) {
                showMessageModal('Произошла ошибка. Пожалуйста, выберите товар для покупки.');
                return;
            }
            if (!selectedSize) {
                showMessageModal('Пожалуйста, выберите размер!');
                return;
            }
            // Показываем модальное окно с анкетой
            orderFormModal.classList.remove('hidden');
            selectedSizeDisplay.textContent = selectedSize;
            orderFormSizeInfo.classList.remove('hidden');
        });

        // ОБРАБОТЧИК ОТПРАВКИ ФОРМЫ
        orderForm.addEventListener('submit', (e) => {
            e.preventDefault();
            
            // Сохраняем данные из формы
            currentOrderData = {
                name: orderNameInput.value,
                address: orderAddressInput.value,
                size: selectedSize, // Добавляем выбранный размер
                email: orderEmailInput.value,
                phone: orderPhoneInput.value,
                product_id: currentProduct.id,
                product_name: currentProduct.name,
                product_price: currentProduct.price,
                product_image: currentProduct.image
            };

            // Скрываем анкету
            orderFormModal.classList.add('hidden');
            
            // Заполняем и показываем модальное окно оплаты
            paymentAmount.textContent = `${currentProduct.price} ₽`;
            paymentSize.textContent = `Размер: ${currentOrderData.size}`;
            paymentModal.classList.remove('hidden');
        });
        
        // ОБРАБОТЧИК КНОПКИ "Я оплатил"
        paymentCompletedBtn.addEventListener('click', () => {
            // Выводим данные в консоль (имитация отправки админу)
            console.log("Заказ успешно оплачен! Детали:");
            console.log(currentOrderData);
            
            // Можно отправить эти данные в Telegram Bot API
            // Пример: Telegram.WebApp.sendData(JSON.stringify(currentOrderData));

            // Скрываем окно оплаты
            paymentModal.classList.add('hidden');
            
            // Показываем сообщение с благодарностью
            showMessageModal('Спасибо за покупку! Информация о заказе отправлена администратору.');
            
            // Очищаем данные
            currentProduct = null;
            currentOrderData = {};
            selectedSize = null;
        });

        // Обработчики событий
        window.addEventListener('load', renderProducts);
        
        adminLoginBtn.addEventListener('click', () => {
            if (isAdmin) {
                toggleAdminMode();
            } else {
                passwordModal.classList.remove('hidden');
                passwordInput.value = '';
                passwordError.classList.add('hidden');
                passwordInput.focus();
            }
        });

        passwordSubmitBtn.addEventListener('click', () => {
            if (passwordInput.value === '8920666') {
                passwordModal.classList.add('hidden');
                toggleAdminMode();
            } else {
                passwordError.classList.remove('hidden');
            }
        });

        passwordCloseBtn.addEventListener('click', () => {
            passwordModal.classList.add('hidden');
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
    </script>
</body>
</html>
