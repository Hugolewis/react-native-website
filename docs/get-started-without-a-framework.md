<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gaúcho Delivery - A melhor comida do RS na sua casa</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Cores inspiradas na bandeira do RS */
        :root {
            --gaucho-green: #0C3B2E;
            --gaucho-red: #A51C30;
            --gaucho-yellow: #F9A01B;
        }
        
        .bg-gaucho-green {
            background-color: var(--gaucho-green);
        }
        
        .text-gaucho-green {
            color: var(--gaucho-green);
        }
        
        .bg-gaucho-red {
            background-color: var(--gaucho-red);
        }
        
        .text-gaucho-red {
            color: var(--gaucho-red);
        }
        
        .bg-gaucho-yellow {
            background-color: var(--gaucho-yellow);
        }
        
        .text-gaucho-yellow {
            color: var(--gaucho-yellow);
        }
        
        .border-gaucho-yellow {
            border-color: var(--gaucho-yellow);
        }
        
        /* Animação para o carrinho */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        
        .animate-pulse {
            animation: pulse 1s infinite;
        }
        
        /* Estilo para o mapa simplificado */
        .simple-map {
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M30,10 L70,10 L80,30 L90,50 L70,70 L50,90 L30,70 L10,50 L20,30 Z" fill="none" stroke="%230C3B2E" stroke-width="2"/><circle cx="50" cy="50" r="5" fill="%23A51C30"/></svg>');
            background-repeat: no-repeat;
            background-position: center;
            background-size: contain;
        }
    </style>
</head>
<body class="bg-gray-50 font-sans">
    <!-- Barra de navegação superior -->
    <nav class="bg-gaucho-green text-white shadow-md fixed top-0 left-0 right-0 z-50">
        <div class="container mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i class="fas fa-hamburger text-gaucho-yellow text-2xl"></i>
                <h1 class="text-xl font-bold">Gaúcho Delivery</h1>
            </div>
            <div class="flex items-center space-x-4">
                <button id="location-btn" class="flex items-center space-x-1 text-sm">
                    <i class="fas fa-map-marker-alt"></i>
                    <span id="current-city">Porto Alegre</span>
                </button>
                <button id="cart-btn" class="relative">
                    <i class="fas fa-shopping-cart text-xl"></i>
                    <span id="cart-count" class="absolute -top-2 -right-2 bg-gaucho-red text-white text-xs rounded-full h-5 w-5 flex items-center justify-center hidden">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Conteúdo principal -->
    <main class="mt-16 pb-20">
        <!-- Seção de busca -->
        <section class="bg-gradient-to-r from-gaucho-green to-gaucho-red text-white py-6 px-4">
            <div class="container mx-auto">
                <h2 class="text-xl font-bold mb-4">O que você deseja hoje?</h2>
                <div class="relative">
                    <input type="text" placeholder="Buscar restaurantes ou pratos..." class="w-full py-3 px-4 rounded-full text-gray-800 focus:outline-none focus:ring-2 focus:ring-gaucho-yellow">
                    <button class="absolute right-3 top-3 bg-gaucho-yellow text-gaucho-green p-1 rounded-full">
                        <i class="fas fa-search"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- Modal de seleção de cidade -->
        <div id="city-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden flex items-center justify-center">
            <div class="bg-white rounded-lg w-11/12 max-w-md max-h-[80vh] overflow-y-auto">
                <div class="p-4 border-b">
                    <h3 class="text-lg font-bold text-gaucho-green">Selecione sua cidade</h3>
                </div>
                <div class="p-4">
                    <input type="text" placeholder="Digite o nome da cidade..." class="w-full py-2 px-3 border rounded mb-4">
                    <div class="space-y-2">
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Porto Alegre">Porto Alegre</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Caxias do Sul">Caxias do Sul</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Pelotas">Pelotas</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Santa Maria">Santa Maria</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Passo Fundo">Passo Fundo</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Rio Grande">Rio Grande</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Canela">Canela</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Gramado">Gramado</button>
                        <button class="city-option w-full text-left py-2 px-3 hover:bg-gray-100 rounded" data-city="Novo Hamburgo">Novo Hamburgo</button>
                    </div>
                </div>
                <div class="p-4 border-t flex justify-end">
                    <button id="close-city-modal" class="px-4 py-2 bg-gray-200 rounded">Cancelar</button>
                </div>
            </div>
        </div>

        <!-- Modal do carrinho -->
        <div id="cart-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden flex items-center justify-center">
            <div class="bg-white rounded-lg w-11/12 max-w-md max-h-[80vh] overflow-y-auto">
                <div class="p-4 border-b flex justify-between items-center">
                    <h3 class="text-lg font-bold text-gaucho-green">Seu Carrinho</h3>
                    <button id="close-cart-modal" class="text-gray-500">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                <div id="cart-items" class="p-4">
                    <!-- Itens do carrinho serão adicionados aqui via JavaScript -->
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-shopping-cart text-4xl mb-2 text-gray-300"></i>
                        <p>Seu carrinho está vazio</p>
                    </div>
                </div>
                <div class="p-4 border-t">
                    <div class="flex justify-between mb-4">
                        <span class="font-bold">Total:</span>
                        <span id="cart-total" class="font-bold">R$ 0,00</span>
                    </div>
                    <button id="checkout-btn" class="w-full bg-gaucho-green text-white py-3 rounded-lg font-bold disabled:opacity-50" disabled>
                        Finalizar Pedido
                    </button>
                </div>
            </div>
        </div>

        <!-- Modal de login/cadastro -->
        <div id="auth-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden flex items-center justify-center">
            <div class="bg-white rounded-lg w-11/12 max-w-md">
                <div class="p-4 border-b flex justify-between items-center">
                    <h3 class="text-lg font-bold text-gaucho-green">Entrar ou Cadastrar</h3>
                    <button id="close-auth-modal" class="text-gray-500">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                <div class="p-4">
                    <div class="mb-4">
                        <label class="block text-gray-700 mb-2">E-mail</label>
                        <input type="email" class="w-full px-3 py-2 border rounded">
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 mb-2">Senha</label>
                        <input type="password" class="w-full px-3 py-2 border rounded">
                    </div>
                    <button class="w-full bg-gaucho-green text-white py-2 rounded-lg mb-4">
                        Entrar
                    </button>
                    <div class="text-center mb-4">
                        <span class="text-gray-500">ou</span>
                    </div>
                    <button class="w-full bg-gaucho-yellow text-gaucho-green py-2 rounded-lg font-bold">
                        Cadastrar com e-mail
                    </button>
                </div>
            </div>
        </div>

        <!-- Seção de promoções -->
        <section class="container mx-auto px-4 py-6">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-bold text-gaucho-green">Promoções perto de você</h2>
                <button class="text-sm text-gaucho-red font-bold">Ver todas</button>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <div class="bg-white rounded-lg shadow-md overflow-hidden border border-gaucho-yellow">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1559844484-e3b708a742f5?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Churrasco" class="w-full h-32 object-cover">
                        <div class="absolute top-2 left-2 bg-gaucho-red text-white text-xs px-2 py-1 rounded">
                            30% OFF
                        </div>
                    </div>
                    <div class="p-3">
                        <h3 class="font-bold">Churrascaria Gaúcha</h3>
                        <p class="text-sm text-gray-600">Rodízio de carnes com 30% de desconto</p>
                        <div class="flex justify-between items-center mt-2">
                            <div class="flex items-center text-sm">
                                <i class="fas fa-star text-gaucho-yellow mr-1"></i>
                                <span>4.8</span>
                            </div>
                            <span class="text-xs bg-gray-100 px-2 py-1 rounded">30-40 min</span>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md overflow-hidden border border-gaucho-yellow">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1601050690597-df0568f70950?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Café colonial" class="w-full h-32 object-cover">
                        <div class="absolute top-2 left-2 bg-gaucho-red text-white text-xs px-2 py-1 rounded">
                            2 por 1
                        </div>
                    </div>
                    <div class="p-3">
                        <h3 class="font-bold">Café Colonial Serra</h3>
                        <p class="text-sm text-gray-600">Leve 2 e pague 1 em cafés coloniais</p>
                        <div class="flex justify-between items-center mt-2">
                            <div class="flex items-center text-sm">
                                <i class="fas fa-star text-gaucho-yellow mr-1"></i>
                                <span>4.9</span>
                            </div>
                            <span class="text-xs bg-gray-100 px-2 py-1 rounded">20-30 min</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Seção de categorias -->
        <section class="container mx-auto px-4 py-6">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-bold text-gaucho-green">Categorias</h2>
                <button class="text-sm text-gaucho-red font-bold">Ver todas</button>
            </div>
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                <button class="category-btn flex flex-col items-center p-3 bg-white rounded-lg shadow-sm hover:bg-gray-50">
                    <div class="w-12 h-12 bg-gaucho-green rounded-full flex items-center justify-center text-white mb-2">
                        <i class="fas fa-drumstick-bite"></i>
                    </div>
                    <span class="text-sm">Churrasco</span>
                </button>
                <button class="category-btn flex flex-col items-center p-3 bg-white rounded-lg shadow-sm hover:bg-gray-50">
                    <div class="w-12 h-12 bg-gaucho-red rounded-full flex items-center justify-center text-white mb-2">
                        <i class="fas fa-burger"></i>
                    </div>
                    <span class="text-sm">Lanches</span>
                </button>
                <button class="category-btn flex flex-col items-center p-3 bg-white rounded-lg shadow-sm hover:bg-gray-50">
                    <div class="w-12 h-12 bg-gaucho-yellow rounded-full flex items-center justify-center text-gaucho-green mb-2">
                        <i class="fas fa-pizza-slice"></i>
                    </div>
                    <span class="text-sm">Massas</span>
                </button>
                <button class="category-btn flex flex-col items-center p-3 bg-white rounded-lg shadow-sm hover:bg-gray-50">
                    <div class="w-12 h-12 bg-gaucho-green rounded-full flex items-center justify-center text-white mb-2">
                        <i class="fas fa-bread-slice"></i>
                    </div>
                    <span class="text-sm">Colonial</span>
                </button>
            </div>
        </section>

        <!-- Seção de restaurantes -->
        <section class="container mx-auto px-4 py-6">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-bold text-gaucho-green">Restaurantes perto de você</h2>
                <button class="text-sm text-gaucho-red font-bold">Ver todos</button>
            </div>
            <div class="space-y-4">
                <!-- Restaurante 1 -->
                <div class="restaurant-card bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1559844484-e3b708a742f5?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Churrascaria Gaúcha" class="w-full h-40 object-cover">
                        <div class="absolute top-2 right-2 bg-white p-1 rounded-full shadow">
                            <i class="far fa-heart text-gray-400"></i>
                        </div>
                    </div>
                    <div class="p-3">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="font-bold">Churrascaria Gaúcha</h3>
                                <div class="flex items-center text-sm text-gray-600 mb-1">
                                    <i class="fas fa-map-marker-alt mr-1 text-xs"></i>
                                    <span>Centro - 1.2km</span>
                                </div>
                            </div>
                            <div class="flex items-center bg-green-100 text-green-800 px-2 py-1 rounded text-xs">
                                <i class="fas fa-star mr-1"></i>
                                <span>4.8</span>
                            </div>
                        </div>
                        <div class="flex justify-between items-center mt-2">
                            <div class="flex flex-wrap gap-1">
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Churrasco</span>
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Rodízio</span>
                            </div>
                            <span class="text-xs bg-gray-100 px-2 py-1 rounded">30-40 min</span>
                        </div>
                        <div class="mt-3 pt-3 border-t">
                            <button class="w-full bg-gaucho-yellow text-gaucho-green py-2 rounded-lg font-bold">
                                Ver cardápio
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Restaurante 2 -->
                <div class="restaurant-card bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1601050690597-df0568f70950?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Café Colonial Serra" class="w-full h-40 object-cover">
                        <div class="absolute top-2 right-2 bg-white p-1 rounded-full shadow">
                            <i class="far fa-heart text-gray-400"></i>
                        </div>
                    </div>
                    <div class="p-3">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="font-bold">Café Colonial Serra</h3>
                                <div class="flex items-center text-sm text-gray-600 mb-1">
                                    <i class="fas fa-map-marker-alt mr-1 text-xs"></i>
                                    <span>Bela Vista - 0.8km</span>
                                </div>
                            </div>
                            <div class="flex items-center bg-green-100 text-green-800 px-2 py-1 rounded text-xs">
                                <i class="fas fa-star mr-1"></i>
                                <span>4.9</span>
                            </div>
                        </div>
                        <div class="flex justify-between items-center mt-2">
                            <div class="flex flex-wrap gap-1">
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Colonial</span>
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Café da manhã</span>
                            </div>
                            <span class="text-xs bg-gray-100 px-2 py-1 rounded">20-30 min</span>
                        </div>
                        <div class="mt-3 pt-3 border-t">
                            <button class="w-full bg-gaucho-yellow text-gaucho-green py-2 rounded-lg font-bold">
                                Ver cardápio
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Restaurante 3 -->
                <div class="restaurant-card bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Pizzaria Napolitana" class="w-full h-40 object-cover">
                        <div class="absolute top-2 right-2 bg-white p-1 rounded-full shadow">
                            <i class="far fa-heart text-gray-400"></i>
                        </div>
                    </div>
                    <div class="p-3">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="font-bold">Pizzaria Napolitana</h3>
                                <div class="flex items-center text-sm text-gray-600 mb-1">
                                    <i class="fas fa-map-marker-alt mr-1 text-xs"></i>
                                    <span>Moinhos - 1.5km</span>
                                </div>
                            </div>
                            <div class="flex items-center bg-green-100 text-green-800 px-2 py-1 rounded text-xs">
                                <i class="fas fa-star mr-1"></i>
                                <span>4.7</span>
                            </div>
                        </div>
                        <div class="flex justify-between items-center mt-2">
                            <div class="flex flex-wrap gap-1">
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Pizza</span>
                                <span class="text-xs bg-gray-100 px-2 py-1 rounded">Massas</span>
                            </div>
                            <span class="text-xs bg-gray-100 px-2 py-1 rounded">25-35 min</span>
                        </div>
                        <div class="mt-3 pt-3 border-t">
                            <button class="w-full bg-gaucho-yellow text-gaucho-green py-2 rounded-lg font-bold">
                                Ver cardápio
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Seção de mapa simplificado -->
        <section class="container mx-auto px-4 py-6">
            <div class="bg-white rounded-lg shadow-md p-4">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-lg font-bold text-gaucho-green">Área de entrega</h2>
                    <button class="text-sm text-gaucho-red font-bold">Detalhes</button>
                </div>
                <div class="simple-map w-full h-40 rounded-lg bg-gray-100 flex items-center justify-center">
                    <div class="text-center">
                        <i class="fas fa-map-marker-alt text-3xl text-gaucho-red mb-2"></i>
                        <p class="text-sm font-bold text-gaucho-green">Porto Alegre - RS</p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Menu inferior fixo -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white shadow-lg border-t z-50">
        <div class="container mx-auto px-4">
            <div class="flex justify-around">
                <button class="menu-btn flex flex-col items-center py-3 px-4 text-gaucho-green border-b-2 border-transparent hover:border-gaucho-green">
                    <i class="fas fa-home text-xl mb-1"></i>
                    <span class="text-xs">Início</span>
                </button>
                <button class="menu-btn flex flex-col items-center py-3 px-4 text-gray-500 border-b-2 border-transparent hover:border-gaucho-green">
                    <i class="fas fa-clipboard-list text-xl mb-1"></i>
                    <span class="text-xs">Pedidos</span>
                </button>
                <button id="profile-btn" class="menu-btn flex flex-col items-center py-3 px-4 text-gray-500 border-b-2 border-transparent hover:border-gaucho-green">
                    <i class="fas fa-user text-xl mb-1"></i>
                    <span class="text-xs">Perfil</span>
                </button>
            </div>
        </div>
    </nav>

    <script>
        // Variáveis globais para simular o carrinho
        let cart = [];
        let currentCity = "Porto Alegre";
        
        // Elementos DOM
        const cityModal = document.getElementById('city-modal');
        const cartModal = document.getElementById('cart-modal');
        const authModal = document.getElementById('auth-modal');
        const cityOptions = document.querySelectorAll('.city-option');
        const currentCityElement = document.getElementById('current-city');
        const locationBtn = document.getElementById('location-btn');
        const closeCityModal = document.getElementById('close-city-modal');
        const cartBtn = document.getElementById('cart-btn');
        const closeCartModal = document.getElementById('close-cart-modal');
        const cartItems = document.getElementById('cart-items');
        const cartTotal = document.getElementById('cart-total');
        const cartCount = document.getElementById('cart-count');
        const checkoutBtn = document.getElementById('checkout-btn');
        const profileBtn = document.getElementById('profile-btn');
        const closeAuthModal = document.getElementById('close-auth-modal');
        
        // Event Listeners
        locationBtn.addEventListener('click', () => cityModal.classList.remove('hidden'));
        closeCityModal.addEventListener('click', () => cityModal.classList.add('hidden'));
        cartBtn.addEventListener('click', () => {
            updateCartModal();
            cartModal.classList.remove('hidden');
        });
        closeCartModal.addEventListener('click', () => cartModal.classList.add('hidden'));
        profileBtn.addEventListener('click', () => authModal.classList.remove('hidden'));
        closeAuthModal.addEventListener('click', () => authModal.classList.add('hidden'));
        
        // Selecionar cidade
        cityOptions.forEach(option => {
            option.addEventListener('click', () => {
                currentCity = option.dataset.city;
                currentCityElement.textContent = currentCity;
                cityModal.classList.add('hidden');
                
                // Aqui seria feita uma requisição para buscar restaurantes da cidade selecionada
                // fetch(`/api/restaurants?city=${currentCity}`)
                //   .then(response => response.json())
                //   .then(data => updateRestaurants(data));
                
                // Simulação de atualização
                alert(`Cidade alterada para ${currentCity}. Em uma aplicação real, os restaurantes seriam atualizados aqui.`);
            });
        });
        
        // Adicionar itens ao carrinho (simulação)
        document.querySelectorAll('.restaurant-card button').forEach(button => {
            button.addEventListener('click', () => {
                // Simular adição de itens ao carrinho
                const restaurantCard = button.closest('.restaurant-card');
                const restaurantName = restaurantCard.querySelector('h3').textContent;
                
                // Adicionar alguns itens fictícios
                cart = [
                    {
                        id: 1,
                        name: "Picanha (500g)",
                        price: 45.90,
                        quantity: 1,
                        restaurant: restaurantName
                    },
                    {
                        id: 2,
                        name: "Arroz de carreteiro",
                        price: 18.50,
                        quantity: 1,
                        restaurant: restaurantName
                    },
                    {
                        id: 3,
                        name: "Salada verde",
                        price: 12.00,
                        quantity: 1,
                        restaurant: restaurantName
                    }
                ];
                
                updateCartCount();
                cartBtn.classList.add('animate-pulse');
                setTimeout(() => cartBtn.classList.remove('animate-pulse'), 2000);
                
                // Mostrar o carrinho automaticamente
                updateCartModal();
                cartModal.classList.remove('hidden');
            });
        });
        
        // Atualizar contador do carrinho
        function updateCartCount() {
            const count = cart.reduce((total, item) => total + item.quantity, 0);
            cartCount.textContent = count;
            
            if (count > 0) {
                cartCount.classList.remove('hidden');
            } else {
                cartCount.classList.add('hidden');
            }
        }
        
        // Atualizar modal do carrinho
        function updateCartModal() {
            if (cart.length === 0) {
                cartItems.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-shopping-cart text-4xl mb-2 text-gray-300"></i>
                        <p>Seu carrinho está vazio</p>
                    </div>
                `;
                cartTotal.textContent = "R$ 0,00";
                checkoutBtn.disabled = true;
                return;
            }
            
            let itemsHTML = '';
            let total = 0;
            
            // Agrupar por restaurante
            const restaurants = {};
            cart.forEach(item => {
                if (!restaurants[item.restaurant]) {
                    restaurants[item.restaurant] = [];
                }
                restaurants[item.restaurant].push(item);
                total += item.price * item.quantity;
            });
            
            for (const [restaurantName, items] of Object.entries(restaurants)) {
                itemsHTML += `
                    <div class="mb-4">
                        <div class="flex justify-between items-center mb-2">
                            <h4 class="font-bold text-gaucho-green">${restaurantName}</h4>
                            <button class="text-xs text-gaucho-red">Remover todos</button>
                        </div>
                        <div class="space-y-3">
                `;
                
                items.forEach(item => {
                    itemsHTML += `
                        <div class="flex justify-between items-center">
                            <div>
                                <p class="text-sm">${item.quantity}x ${item.name}</p>
                                <p class="text-xs text-gray-500">R$ ${item.price.toFixed(2)}</p>
                            </div>
                            <div class="flex items-center">
                                <span class="font-bold mr-4">R$ ${(item.price * item.quantity).toFixed(2)}</span>
                                <div class="flex items-center border rounded">
                                    <button class="px-2 py-1 decrease-item" data-id="${item.id}">-</button>
                                    <span class="px-2 text-sm">${item.quantity}</span>
                                    <button class="px-2 py-1 increase-item" data-id="${item.id}">+</button>
                                </div>
                            </div>
                        </div>
                    `;
                });
                
                itemsHTML += `
                        </div>
                    </div>
                    <div class="border-b my-3"></div>
                `;
            }
            
            itemsHTML += `
                <div class="flex items-center mb-4">
                    <input type="text" placeholder="Cupom de desconto" class="flex-1 px-3 py-2 border rounded-l">
                    <button class="bg-gaucho-green text-white px-3 py-2 rounded-r">Aplicar</button>
                </div>
            `;
            
            cartItems.innerHTML = itemsHTML;
            cartTotal.textContent = `R$ ${total.toFixed(2)}`;
            checkoutBtn.disabled = false;
            
            // Adicionar eventos para aumentar/diminuir quantidade
            document.querySelectorAll('.increase-item').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const itemId = parseInt(e.target.dataset.id);
                    const item = cart.find(i => i.id === itemId);
                    if (item) {
                        item.quantity++;
                        updateCartModal();
                        updateCartCount();
                    }
                });
            });
            
            document.querySelectorAll('.decrease-item').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const itemId = parseInt(e.target.dataset.id);
                    const itemIndex = cart.findIndex(i => i.id === itemId);
                    if (itemIndex !== -1) {
                        if (cart[itemIndex].quantity > 1) {
                            cart[itemIndex].quantity--;
                        } else {
                            cart.splice(itemIndex, 1);
                        }
                        updateCartModal();
                        updateCartCount();
                    }
                });
            });
        }
        
        // Finalizar compra
        checkoutBtn.addEventListener('click', () => {
            alert('Pedido finalizado com sucesso! Em uma aplicação real, isso enviaria o pedido para o backend.');
            cart = [];
            updateCartCount();
            updateCartModal();
        });
        
        // Inicialização
        updateCartCount();
        
        /* 
        COMENTÁRIOS SOBRE INTEGRAÇÃO COM BACKEND:
        
        1. Busca de restaurantes por cidade:
           - GET /api/restaurants?city={cidade}
           - Retornaria lista de restaurantes com informações como nome, imagem, avaliação, categorias, etc.
        
        2. Busca de cardápio de um restaurante:
           - GET /api/restaurants/{id}/menu
           - Retornaria categorias de itens e os itens do menu com preços
        
        3. Autenticação de usuário:
           - POST /api/auth/login (para login)
           - POST /api/auth/register (para cadastro)
           - Usaria JWT para manter a sessão
        
        4. Envio de pedido:
           - POST /api/orders
           - Enviaria os itens do carrinho, endereço de entrega, forma de pagamento, etc.
        
        5. Histórico de pedidos:
           - GET /api/users/{id}/orders
           - Retornaria lista de pedidos anteriores do usuário
        
        6. Integração com serviços de mapa:
           - Google Maps API ou similar para mostrar localização exata e rota de entrega
        */
    </script>
</body>
</html>
