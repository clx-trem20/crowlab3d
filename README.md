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
            margin: 0;
            padding: 0;
            overflow-x: hidden;
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
        
        /* Estilos do Carrossel */
        .carousel-container {
            position: relative;
            width: 100%;
            height: 320px; /* Aumentado para preencher melhor a tela */
            overflow: hidden;
        }
        .carousel-track {
            display: flex;
            transition: transform 0.5s ease-in-out;
            height: 100%;
        }
        .carousel-item {
            min-width: 100%;
            height: 100%;
        }
        .carousel-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .carousel-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(15, 23, 42, 0.6);
            color: white;
            padding: 12px;
            cursor: pointer;
            border-radius: 50%;
            opacity: 0;
            transition: opacity 0.3s;
            z-index: 10;
        }
        .product-card:hover .carousel-btn {
            opacity: 1;
        }
        .btn-prev { left: 15px; }
        .btn-next { right: 15px; }
        
        .carousel-dots {
            position: absolute;
            bottom: 15px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 8px;
            z-index: 10;
        }
        .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transition: background 0.3s;
        }
        .dot.active {
            background: #3b82f6;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.3s ease-out forwards;
        }

        /* Botões Flutuantes */
        .social-float-container {
            position: fixed;
            bottom: 30px;
            right: 30px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            z-index: 100;
        }

        .float-btn {
            width: 65px;
            height: 65px;
            border-radius: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            color: white;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .float-btn:hover { transform: scale(1.1); }
        .btn-whatsapp { background-color: #25d366; }
        .btn-instagram { background: radial-gradient(circle at 30% 107%, #fdf497 0%, #fdf497 5%, #fd5949 45%,#d6249f 60%,#285AEB 90%); }

        .float-tooltip {
            position: absolute;
            right: 80px;
            background: #1e293b;
            color: white;
            padding: 10px 18px;
            border-radius: 8px;
            font-size: 14px;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            border: 1px solid #334155;
        }
        .float-btn:hover .float-tooltip { opacity: 1; }

        /* Estilo da Seção Hero Full Width */
        .hero-banner {
            background: linear-gradient(rgba(15, 23, 42, 0.7), rgba(15, 23, 42, 0.9)), url('https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&q=80&w=2070');
            background-size: cover;
            background-position: center;
            min-height: 60vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
    </style>
</head>
<body class="font-sans antialiased">

    <!-- Navegação Full Width -->
    <nav class="sticky top-0 z-50 bg-slate-900/90 backdrop-blur-lg border-b border-slate-800 w-full px-8 py-5">
        <div class="w-full flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <i class="fas fa-cube text-blue-500 text-4xl"></i>
                <span class="text-3xl font-black tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
            </div>
            
            <div class="flex items-center space-x-6">
                <button onclick="toggleCart()" class="p-3 hover:bg-slate-800 rounded-full transition relative">
                    <i class="fas fa-shopping-cart text-2xl text-slate-300"></i>
                    <span id="cart-count" class="absolute top-1 right-1 bg-blue-600 text-white text-xs w-6 h-6 flex items-center justify-center rounded-full hidden font-bold">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section Full Width -->
    <header class="hero-banner px-6 text-center w-full">
        <h1 class="text-6xl md:text-8xl font-black mb-6 bg-gradient-to-r from-blue-400 via-indigo-400 to-blue-600 bg-clip-text text-transparent uppercase tracking-tight">
            CROWLAB3D
        </h1>
        <p class="text-slate-300 text-xl md:text-2xl max-w-4xl mx-auto font-light leading-relaxed">
            A nova era da fabricação digital. Impressões de alta definição, prototipagem rápida e designs exclusivos para o teu setup ou coleção.
        </p>
        <div class="mt-10">
            <a href="#products-grid" class="bg-blue-600 hover:bg-blue-700 text-white px-10 py-4 rounded-full font-bold text-lg transition-all hover:scale-105 inline-block">
                Explorar Catálogo
            </a>
        </div>
    </header>

    <!-- Galeria de Produtos Grid Responsivo -->
    <main class="w-full px-4 md:px-8 py-16">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 2xl:grid-cols-4 gap-10" id="products-grid">
            <!-- Os produtos serão injetados aqui via JS -->
        </div>
    </main>

    <!-- Rodapé Full Width -->
    <footer class="border-t border-slate-800 py-16 text-center w-full bg-slate-950">
        <div class="px-8">
            <div class="flex items-center justify-center space-x-3 mb-6">
                <i class="fas fa-cube text-blue-500 text-3xl"></i>
                <span class="text-2xl font-black tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
            </div>
            <p class="text-slate-400 text-lg mb-8 max-w-xl mx-auto">
                Transformamos ideias digitais em realidade física com a melhor tecnologia 3D do mercado.
            </p>
            <div class="flex justify-center space-x-6 mb-10">
                <a href="#" class="text-slate-500 hover:text-blue-500 text-2xl transition"><i class="fab fa-instagram"></i></a>
                <a href="#" class="text-slate-500 hover:text-blue-500 text-2xl transition"><i class="fab fa-facebook"></i></a>
                <a href="#" class="text-slate-500 hover:text-blue-500 text-2xl transition"><i class="fab fa-whatsapp"></i></a>
            </div>
            <p class="text-slate-600 text-sm">
                © 2026 CrowLab3D – Todos os direitos reservados. Criado por CLX
            </p>
        </div>
    </footer>

    <!-- Botões Flutuantes -->
    <div class="social-float-container">
        <a href="https://www.instagram.com/crowlab3d/" target="_blank" class="float-btn btn-instagram">
            <i class="fab fa-instagram"></i>
            <div class="float-tooltip">Siga-nos no Instagram</div>
        </a>
        <div onclick="customOrder()" class="float-btn btn-whatsapp">
            <i class="fab fa-whatsapp"></i>
            <div class="float-tooltip">Pedido Personalizado</div>
        </div>
    </div>

    <!-- Carrinho Lateral -->
    <div id="cart-drawer" class="fixed inset-y-0 right-0 w-full md:w-[450px] bg-slate-900 shadow-2xl z-[60] transform translate-x-full transition-transform duration-300 ease-in-out border-l border-slate-800">
        <div class="h-full flex flex-col">
            <div class="p-8 border-b border-slate-800 flex justify-between items-center bg-slate-900/50">
                <h2 class="text-2xl font-black uppercase tracking-tight">O Teu Carrinho</h2>
                <button onclick="toggleCart()" class="text-slate-400 hover:text-white text-2xl transition"><i class="fas fa-times"></i></button>
            </div>
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-8 space-y-6 custom-scrollbar">
                <div id="cart-items-list" class="space-y-6"></div>
                <div id="empty-cart-msg" class="text-slate-500 text-center py-20">
                    <i class="fas fa-shopping-basket text-5xl mb-4 block opacity-20"></i>
                    <p class="text-xl">O carrinho está vazio.</p>
                </div>
            </div>
            <div id="cart-footer" class="p-8 border-t border-slate-800 bg-slate-950 hidden">
                <div class="flex justify-between items-center mb-8">
                    <span class="text-slate-400 text-lg uppercase tracking-wider">Subtotal:</span>
                    <span id="cart-total" class="text-3xl font-black text-blue-400">R$ 0,00</span>
                </div>
                <button onclick="showCheckout()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black py-5 rounded-2xl transition-all text-xl uppercase tracking-tighter shadow-lg shadow-blue-900/20">
                    Finalizar Compra
                </button>
            </div>
        </div>
    </div>

    <!-- Modal de Checkout -->
    <div id="checkout-modal" class="fixed inset-0 z-[70] hidden items-center justify-center p-4 bg-slate-950/95 backdrop-blur-md">
        <div class="bg-slate-900 w-full max-w-3xl rounded-3xl border border-slate-800 max-h-[95vh] overflow-y-auto custom-scrollbar shadow-2xl">
            <div class="p-10">
                <div class="flex justify-between items-center mb-10">
                    <h2 class="text-4xl font-black uppercase tracking-tight italic">Checkout</h2>
                    <button onclick="hideCheckout()" class="text-slate-400 hover:text-white text-3xl"><i class="fas fa-times"></i></button>
                </div>
                <form id="checkout-form" onsubmit="handleOrder(event)" class="space-y-8">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                        <div>
                            <label class="block text-slate-400 text-sm font-bold mb-3 uppercase tracking-wider">Nome Completo</label>
                            <input type="text" required name="nome" class="w-full bg-slate-800 border-2 border-slate-700 rounded-xl p-4 focus:border-blue-500 outline-none text-white transition-all">
                        </div>
                        <div>
                            <label class="block text-slate-400 text-sm font-bold mb-3 uppercase tracking-wider">CPF / NIF</label>
                            <input type="text" required name="cpf" placeholder="000.000.000-00" class="w-full bg-slate-800 border-2 border-slate-700 rounded-xl p-4 focus:border-blue-500 outline-none text-white transition-all">
                        </div>
                    </div>
                    <div>
                        <label class="block text-slate-400 text-sm font-bold mb-3 uppercase tracking-wider">Morada de Entrega</label>
                        <textarea required name="endereco" rows="3" class="w-full bg-slate-800 border-2 border-slate-700 rounded-xl p-4 focus:border-blue-500 outline-none text-white transition-all"></textarea>
                    </div>
                    <div>
                        <label class="block text-slate-400 text-sm font-bold mb-5 uppercase tracking-wider">Método de Pagamento</label>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="pix" class="hidden peer" checked>
                                <div class="p-5 border-2 border-slate-700 rounded-2xl flex items-center justify-center space-x-3 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800">
                                    <i class="fa-brands fa-pix text-cyan-400 text-2xl"></i>
                                    <span class="font-bold text-lg">Pix</span>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="boleto" class="hidden peer">
                                <div class="p-5 border-2 border-slate-700 rounded-2xl flex items-center justify-center space-x-3 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800">
                                    <i class="fas fa-barcode text-2xl"></i>
                                    <span class="font-bold text-lg">Boleto</span>
                                </div>
                            </label>
                        </div>
                    </div>
                    <div class="pt-10">
                        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-black py-6 rounded-2xl transition-all text-2xl uppercase shadow-lg shadow-green-900/20">
                            Confirmar Pedido via WhatsApp
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Mensagem de Sucesso -->
    <div id="success-message" class="fixed top-8 left-1/2 -translate-x-1/2 bg-green-500 text-white px-10 py-5 rounded-full shadow-2xl z-[100] transform -translate-y-40 transition-transform duration-500 font-bold text-lg">
        <i class="fas fa-check-circle mr-3"></i> A redirecionar para o WhatsApp...
    </div>

    <script>
        const WHATSAPP_NUMBER = "5521990819172"; 

        const products = [
            { id: 1, name: "Escultura Dragon", price: 185.00, category: "Arte", images: ["img/dragon.jpg", "img/dragon_v2.jpg", "img/dragon_v3.jpg"] },
            { id: 2, name: "Organizador Hexa", price: 45.90, category: "Utilitário", images: ["img/organizador.jpg", "img/organizador2.jpg"] },
            { id: 3, name: "Action Figure Hero", price: 120.00, category: "Colecionável", images: ["img/hero.jpg", "img/hero_detalhe.jpg"] },
            { id: 4, name: "Luminária Articulada", price: 159.00, category: "Decoração", images: ["img/luminaria.jpg", "img/luminaria2.jpg"] },
            { id: 5, name: "Chaveiro Personalizado", price: 15.00, category: "Acessório", images: ["img/chaveiro.jpg", "img/chaveiro2.jpg"] },
            { id: 6, name: "Miniatura RPG Set", price: 89.90, category: "Jogos", images: ["img/rpg.jpg", "img/rpg2.jpg"] },
            { id: 7, name: "Suporte de Headset", price: 65.00, category: "Setup", images: ["img/suporte.jpg", "img/suporte2.jpg"] },
            { id: 8, name: "Vaso Geométrico", price: 35.00, category: "Casa", images: ["img/vaso.jpg", "img/vaso2.jpg"] },
        ];

        let cart = [];
        const carouselStates = {}; 

        function renderProducts() {
            const grid = document.getElementById('products-grid');
            if (!grid) return;
            
            grid.innerHTML = products.map(product => {
                carouselStates[product.id] = 0;
                const hasMultiple = product.images.length > 1;

                return `
                <div class="product-card bg-slate-800/40 border border-slate-700/50 rounded-3xl overflow-hidden flex flex-col backdrop-blur-sm">
                    <div class="carousel-container bg-slate-950">
                        <div class="carousel-track" id="track-${product.id}">
                            ${product.images.map(img => `
                                <div class="carousel-item">
                                    <img src="${img}" alt="${product.name}" onerror="this.src='https://via.placeholder.com/600x600?text=Sem+Imagem'">
                                </div>
                            `).join('')}
                        </div>
                        
                        ${hasMultiple ? `
                            <button class="carousel-btn btn-prev" onclick="moveSlide(${product.id}, -1)">
                                <i class="fas fa-chevron-left"></i>
                            </button>
                            <button class="carousel-btn btn-next" onclick="moveSlide(${product.id}, 1)">
                                <i class="fas fa-chevron-right"></i>
                            </button>
                            <div class="carousel-dots" id="dots-${product.id}">
                                ${product.images.map((_, i) => `<div class="dot ${i === 0 ? 'active' : ''}"></div>`).join('')}
                            </div>
                        ` : ''}

                        <span class="absolute top-4 left-4 z-20 bg-blue-600 text-white text-[10px] font-black uppercase tracking-widest px-4 py-1.5 rounded-full shadow-lg shadow-blue-900/40">
                            ${product.category}
                        </span>
                    </div>

                    <div class="p-8 flex flex-col flex-1">
                        <h3 class="text-2xl font-black mb-3 tracking-tight">${product.name}</h3>
                        <div class="mt-auto flex justify-between items-center pt-6">
                            <span class="text-blue-400 font-mono text-2xl font-black italic tracking-tighter">R$ ${product.price.toFixed(2)}</span>
                            <button onclick="addToCart(${product.id})" class="bg-blue-600 hover:bg-blue-500 text-white w-14 h-14 rounded-2xl flex items-center justify-center transition active:scale-90 shadow-lg shadow-blue-900/30">
                                <i class="fas fa-plus text-xl"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `}).join('');
        }

        window.moveSlide = function(productId, direction) {
            const product = products.find(p => p.id === productId);
            const total = product.images.length;
            let current = carouselStates[productId];

            current = (current + direction + total) % total;
            carouselStates[productId] = current;

            const track = document.getElementById(`track-${productId}`);
            track.style.transform = `translateX(-${current * 100}%)`;

            const dotsContainer = document.getElementById(`dots-${productId}`);
            if (dotsContainer) {
                const dots = dotsContainer.querySelectorAll('.dot');
                dots.forEach((dot, idx) => {
                    dot.classList.toggle('active', idx === current);
                });
            }
        };

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
            // Abrir carrinho ao adicionar
            const drawer = document.getElementById('cart-drawer');
            if (drawer.classList.contains('translate-x-full')) toggleCart();
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
                    <div class="flex items-center justify-between bg-slate-800/40 p-5 rounded-2xl border border-slate-700/50 animate-fade-in">
                        <div class="flex items-center space-x-5">
                            <div class="w-16 h-16 rounded-xl overflow-hidden flex-shrink-0 border border-slate-700">
                                <img src="${item.images[0]}" class="w-full h-full object-cover" onerror="this.src='https://via.placeholder.com/150x150'">
                            </div>
                            <div>
                                <h4 class="font-black text-lg leading-tight">${item.name}</h4>
                                <p class="text-blue-400 font-mono font-bold">${item.quantity}x R$ ${item.price.toFixed(2)}</p>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="text-slate-600 hover:text-red-500 transition-colors p-2">
                            <i class="fas fa-trash-alt text-xl"></i>
                        </button>
                    </div>
                `).join('');
            }
            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);
            totalEl.innerText = `R$ ${total.toFixed(2)}`;
        }

        function showCheckout() {
            document.getElementById('checkout-modal').classList.replace('hidden', 'flex');
        }

        function hideCheckout() {
            document.getElementById('checkout-modal').classList.replace('flex', 'hidden');
        }

        function customOrder() {
            const message = "Olá! Gostaria de fazer um orçamento para um pedido personalizado na CrowLab3D.";
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
        }

        function handleOrder(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const cliente = Object.fromEntries(formData);
            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);

            let message = `🚀 *NOVA ENCOMENDA CROWLAB3D*\n\n`;
            message += `👤 *Cliente:* ${cliente.nome}\n`;
            message += `🆔 *Identificação:* ${cliente.cpf}\n`;
            message += `📍 *Morada:* ${cliente.endereco}\n`;
            message += `💳 *Pagamento:* ${cliente.pagamento.toUpperCase()}\n\n`;
            message += `📦 *Produtos:*\n`;
            cart.forEach(item => {
                message += `- ${item.quantity}x ${item.name} (R$ ${(item.price * item.quantity).toFixed(2)})\n`;
            });
            message += `\n💰 *VALOR TOTAL: R$ ${total.toFixed(2)}*`;

            document.getElementById('success-message').classList.remove('-translate-y-40');

            setTimeout(() => {
                window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
                hideCheckout();
                document.getElementById('success-message').classList.add('-translate-y-40');
                cart = [];
                updateCart();
                e.target.reset();
            }, 1200);
        }

        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            updateCart();
        });
    </script>
</body>
</html>
