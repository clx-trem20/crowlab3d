<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CrowLab3D - Loja de Objetos 3D</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #0f172a;
            color: #f8fafc;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #1e293b;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        .product-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px -5px rgba(59, 130, 246, 0.2);
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.3s ease-out forwards;
        }

        /* Container para os botões flutuantes */
        .social-float-container {
            position: fixed;
            bottom: 30px;
            right: 30px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            z-index: 100;
        }

        /* Estilo base dos botões flutuantes */
        .float-btn {
            width: 60px;
            height: 60px;
            border-radius: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            color: white;
            box-shadow: 0 4px 15px rgba(0,0,0,0.4);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .float-btn:hover {
            transform: scale(1.1);
        }

        /* Cores específicas */
        .btn-whatsapp {
            background-color: #25d366;
        }
        .btn-whatsapp:hover {
            background-color: #128c7e;
        }

        .btn-instagram {
            background: radial-gradient(circle at 30% 107%, #fdf497 0%, #fdf497 5%, #fd5949 45%,#d6249f 60%,#285AEB 90%);
        }

        /* Tooltips */
        .float-tooltip {
            position: absolute;
            right: 75px;
            background: #1e293b;
            color: white;
            padding: 8px 15px;
            border-radius: 8px;
            font-size: 14px;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            border: 1px solid #334155;
        }

        .float-btn:hover .float-tooltip {
            opacity: 1;
        }
    </style>
</head>
<body class="font-sans antialiased">

    <!-- Navegação -->
    <nav class="sticky top-0 z-50 bg-slate-900/80 backdrop-blur-md border-b border-slate-800 px-6 py-4">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i class="fas fa-cube text-blue-500 text-3xl"></i>
                <span class="text-2xl font-bold tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
            </div>
            
            <div class="relative">
                <button onclick="toggleCart()" class="p-2 hover:bg-slate-800 rounded-full transition relative">
                    <i class="fas fa-shopping-cart text-xl text-slate-300"></i>
                    <span id="cart-count" class="absolute -top-1 -right-1 bg-blue-600 text-white text-xs w-5 h-5 flex items-center justify-center rounded-full hidden">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <header class="py-20 px-6 text-center">
        <h1 class="text-5xl md:text-7xl font-extrabold mb-6 bg-gradient-to-r from-blue-400 to-indigo-500 bg-clip-text text-transparent uppercase">
            CROWLAB3D
        </h1>
        <p class="text-slate-400 text-lg max-w-2xl mx-auto">
            Explora a vanguarda da impressão e modelação 3D. Peças exclusivas e protótipos de alta precisão.
        </p>
    </header>

    <!-- Galeria de Produtos -->
    <main class="max-w-7xl mx-auto px-6 pb-20">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8" id="products-grid">
            <!-- Os produtos serão injetados aqui via JS -->
        </div>
    </main>

    <!-- Rodapé -->
    <footer class="border-t border-slate-800 py-10 text-center">
        <div class="max-w-7xl mx-auto px-6">
            <div class="flex items-center justify-center space-x-2 mb-4">
                <i class="fas fa-cube text-blue-500 text-xl"></i>
                <span class="text-lg font-bold tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
            </div>
            <p class="text-slate-500 text-sm">
                © 2026 – Criado por CLX
            </p>
        </div>
    </footer>

    <!-- Container de Botões Flutuantes -->
    <div class="social-float-container">
        <!-- Botão Instagram -->
        <a href="https://www.instagram.com/crowlab3d/" target="_blank" class="float-btn btn-instagram">
            <i class="fab fa-instagram"></i>
            <div class="float-tooltip">Siga-nos no Instagram</div>
        </a>

        <!-- Botão WhatsApp -->
        <div onclick="customOrder()" class="float-btn btn-whatsapp">
            <i class="fab fa-whatsapp"></i>
            <div class="float-tooltip">Pedido Personalizado</div>
        </div>
    </div>

    <!-- Carrinho Lateral -->
    <div id="cart-drawer" class="fixed inset-y-0 right-0 w-full md:w-96 bg-slate-900 shadow-2xl z-[60] transform translate-x-full transition-transform duration-300 ease-in-out border-l border-slate-800">
        <div class="h-full flex flex-col">
            <div class="p-6 border-b border-slate-800 flex justify-between items-center">
                <h2 class="text-xl font-bold">Seu Carrinho</h2>
                <button onclick="toggleCart()" class="text-slate-400 hover:text-white text-xl"><i class="fas fa-times"></i></button>
            </div>

            <!-- Lista de Itens -->
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-6 space-y-4 custom-scrollbar">
                <div id="cart-items-list" class="space-y-4"></div>
                <p id="empty-cart-msg" class="text-slate-500 text-center py-10">O carrinho está vazio.</p>
            </div>

            <!-- Resumo e Botão Finalizar -->
            <div id="cart-footer" class="p-6 border-t border-slate-800 hidden">
                <div class="flex justify-between items-center mb-6">
                    <span class="text-slate-400">Total:</span>
                    <span id="cart-total" class="text-2xl font-bold text-blue-400">R$ 0,00</span>
                </div>
                <button onclick="showCheckout()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 rounded-xl transition">
                    Finalizar Compra
                </button>
            </div>
        </div>
    </div>

    <!-- Modal de Checkout -->
    <div id="checkout-modal" class="fixed inset-0 z-[70] hidden items-center justify-center p-4 bg-slate-950/90 backdrop-blur-sm">
        <div class="bg-slate-900 w-full max-w-2xl rounded-2xl border border-slate-800 max-h-[90vh] overflow-y-auto custom-scrollbar">
            <div class="p-8">
                <div class="flex justify-between items-center mb-8">
                    <h2 class="text-3xl font-bold">Finalizar Encomenda</h2>
                    <button onclick="hideCheckout()" class="text-slate-400 hover:text-white text-2xl"><i class="fas fa-times"></i></button>
                </div>

                <form id="checkout-form" onsubmit="handleOrder(event)" class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-slate-400 mb-2">Nome Completo</label>
                            <input type="text" required name="nome" class="w-full bg-slate-800 border border-slate-700 rounded-lg p-3 focus:ring-2 focus:ring-blue-500 outline-none text-white">
                        </div>
                        <div>
                            <label class="block text-slate-400 mb-2">CPF</label>
                            <input type="text" required name="cpf" placeholder="000.000.000-00" class="w-full bg-slate-800 border border-slate-700 rounded-lg p-3 focus:ring-2 focus:ring-blue-500 outline-none text-white">
                        </div>
                    </div>

                    <div>
                        <label class="block text-slate-400 mb-2">Endereço Completo (Rua, Nº, Bairro, CEP)</label>
                        <textarea required name="endereco" rows="3" class="w-full bg-slate-800 border border-slate-700 rounded-lg p-3 focus:ring-2 focus:ring-blue-500 outline-none text-white"></textarea>
                    </div>

                    <div>
                        <label class="block text-slate-400 mb-4">Forma de Pagamento</label>
                        <div class="grid grid-cols-2 gap-4">
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="pix" class="hidden peer" checked>
                                <div class="p-4 border border-slate-700 rounded-xl flex items-center justify-center space-x-2 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition">
                                    <i class="fa-brands fa-pix text-cyan-400"></i>
                                    <span>Pix</span>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="boleto" class="hidden peer">
                                <div class="p-4 border border-slate-700 rounded-xl flex items-center justify-center space-x-2 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition">
                                    <i class="fas fa-barcode"></i>
                                    <span>Boleto</span>
                                </div>
                            </label>
                        </div>
                    </div>

                    <div class="pt-6">
                        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-4 rounded-xl transition shadow-lg shadow-green-900/20">
                            Confirmar e Enviar via WhatsApp
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Mensagem de Sucesso -->
    <div id="success-message" class="fixed top-5 left-1/2 -translate-x-1/2 bg-green-500 text-white px-8 py-4 rounded-full shadow-2xl z-[100] transform -translate-y-24 transition-transform duration-500">
        <i class="fas fa-check-circle mr-2"></i> Redirecionando para o WhatsApp...
    </div>

    <script>
        const WHATSAPP_NUMBER = "5521990819172"; 

        /**
         * IMPORTANTE: No GitHub, salve suas fotos na pasta 'img'
         * Exemplo: img/dragon.jpg, img/organizador.jpg, etc.
         */
        const products = [
            { id: 1, name: "Escultura Dragon", price: 185.00, category: "Arte", image: "img/dragon.jpg" },
            { id: 2, name: "Organizador Hexa", price: 45.90, category: "Utilitário", image: "img/organizador.jpg" },
            { id: 3, name: "Action Figure Hero", price: 120.00, category: "Colecionável", image: "img/hero.jpg" },
            { id: 4, name: "Luminária Articulada", price: 159.00, category: "Decoração", image: "img/luminaria.jpg" },
            { id: 5, name: "Chaveiro Personalizado", price: 15.00, category: "Acessório", image: "img/chaveiro.jpg" },
            { id: 6, name: "Miniatura RPG Set", price: 89.90, category: "Jogos", image: "img/rpg.jpg" },
            { id: 7, name: "Suporte de Headset", price: 65.00, category: "Setup", image: "img/suporte.jpg" },
            { id: 8, name: "Vaso Geométrico", price: 35.00, category: "Casa", image: "img/vaso.jpg" },
        ];

        let cart = [];

        function renderProducts() {
            const grid = document.getElementById('products-grid');
            if (!grid) return;
            
            grid.innerHTML = products.map(product => `
                <div class="product-card bg-slate-800/50 border border-slate-700 rounded-2xl overflow-hidden flex flex-col">
                    <div class="relative h-64 overflow-hidden bg-slate-900">
                        <img src="${product.image}" alt="${product.name}" class="w-full h-full object-cover opacity-80 hover:opacity-100 transition duration-500" onerror="this.src='https://via.placeholder.com/400x400?text=Adicione+Foto+na+Pasta+Img'">
                        <span class="absolute top-3 left-3 bg-blue-600/20 backdrop-blur-md text-blue-400 text-xs font-bold px-3 py-1 rounded-full border border-blue-500/30">
                            ${product.category}
                        </span>
                    </div>
                    <div class="p-6 flex flex-col flex-1">
                        <h3 class="text-xl font-bold mb-2">${product.name}</h3>
                        <div class="mt-auto flex justify-between items-center">
                            <span class="text-blue-400 font-mono text-lg font-bold">R$ ${product.price.toFixed(2)}</span>
                            <button onclick="addToCart(${product.id})" class="bg-blue-600 hover:bg-blue-500 text-white p-2 rounded-lg transition active:scale-95">
                                <i class="fas fa-cart-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function toggleCart() {
            const drawer = document.getElementById('cart-drawer');
            if (drawer) drawer.classList.toggle('translate-x-full');
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existing = cart.find(item => item.id === productId);
            
            if (existing) {
                existing.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            
            updateCart();
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCart();
        }

        function updateCart() {
            const listContainer = document.getElementById('cart-items-list');
            const totalEl = document.getElementById('cart-total');
            const countEl = document.getElementById('cart-count');
            const footer = document.getElementById('cart-footer');
            const emptyMsg = document.getElementById('empty-cart-msg');

            if (!listContainer || !totalEl || !countEl || !footer || !emptyMsg) return;

            if (cart.length === 0) {
                emptyMsg.classList.remove('hidden');
                footer.classList.add('hidden');
                countEl.classList.add('hidden');
                listContainer.innerHTML = '';
            } else {
                emptyMsg.classList.add('hidden');
                footer.classList.remove('hidden');
                countEl.classList.remove('hidden');
                countEl.innerText = cart.reduce((acc, item) => acc + item.quantity, 0);

                listContainer.innerHTML = cart.map(item => `
                    <div class="flex items-center justify-between bg-slate-800/50 p-4 rounded-xl border border-slate-700 animate-fade-in">
                        <div class="flex items-center space-x-4">
                            <div class="w-12 h-12 rounded-lg overflow-hidden flex-shrink-0">
                                <img src="${item.image}" class="w-full h-full object-cover" onerror="this.src='https://via.placeholder.com/100x100?text=Img'">
                            </div>
                            <div>
                                <h4 class="font-bold text-sm">${item.name}</h4>
                                <p class="text-xs text-slate-400">${item.quantity}x R$ ${item.price.toFixed(2)}</p>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="text-slate-500 hover:text-red-400 transition-colors">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </div>
                `).join('');
            }

            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);
            totalEl.innerText = `R$ ${total.toFixed(2)}`;
        }

        function showCheckout() {
            const modal = document.getElementById('checkout-modal');
            if (modal) {
                modal.classList.remove('hidden');
                modal.classList.add('flex');
            }
        }

        function hideCheckout() {
            const modal = document.getElementById('checkout-modal');
            if (modal) {
                modal.classList.add('hidden');
                modal.classList.remove('flex');
            }
        }

        function customOrder() {
            const message = "Olá! Gostaria de fazer um orçamento para um pedido personalizado na CrowLab3D.";
            const encodedMessage = encodeURIComponent(message);
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodedMessage}`, '_blank');
        }

        function handleOrder(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const cliente = Object.fromEntries(formData);
            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);

            let message = `🚀 *NOVA ENCOMENDA CROWLAB3D*\n\n`;
            message += `👤 *Cliente:* ${cliente.nome}\n`;
            message += `🆔 *CPF:* ${cliente.cpf}\n`;
            message += `📍 *Endereço:* ${cliente.endereco}\n`;
            message += `💳 *Pagamento:* ${cliente.pagamento.toUpperCase()}\n\n`;
            message += `📦 *Produtos:*\n`;
            
            cart.forEach(item => {
                message += `- ${item.quantity}x ${item.name} (R$ ${(item.price * item.quantity).toFixed(2)})\n`;
            });

            message += `\n💰 *VALOR TOTAL: R$ ${total.toFixed(2)}*`;

            const encodedMessage = encodeURIComponent(message);
            const whatsappUrl = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodedMessage}`;

            const msgStatus = document.getElementById('success-message');
            if (msgStatus) {
                msgStatus.classList.remove('-translate-y-24');
            }

            setTimeout(() => {
                window.open(whatsappUrl, '_blank');
                hideCheckout();
                if (msgStatus) msgStatus.classList.add('-translate-y-24');
                cart = [];
                updateCart();
                e.target.reset();
            }, 1000);
        }

        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            updateCart();
        });
    </script>
</body>
</html>
