<!DOCTYPE html>
<html lang="pt-br" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CrowLab3D - Loja de Objetos 3D</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        /* Reset e Garantia de Tela Toda */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        html, body {
            width: 100%;
            min-height: 100%;
            background-color: #0f172a;
            color: #f8fafc;
            overflow-x: hidden;
        }
        
        /* Scrollbar Personalizada */
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

        /* Efeitos de Card */
        .product-card {
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275), box-shadow 0.3s ease;
        }
        .product-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 20px 30px -10px rgba(59, 130, 246, 0.3);
        }
        
        /* Carrossel de Imagens Interno */
        .carousel-container {
            position: relative;
            width: 100%;
            height: 380px;
            overflow: hidden;
        }
        .carousel-track {
            display: flex;
            transition: transform 0.6s cubic-bezier(0.45, 0, 0.55, 1);
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
            background: rgba(15, 23, 42, 0.8);
            color: white;
            padding: 14px;
            cursor: pointer;
            border-radius: 50%;
            opacity: 0;
            transition: all 0.3s;
            z-index: 10;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .product-card:hover .carousel-btn {
            opacity: 1;
        }
        .btn-prev { left: 15px; }
        .btn-next { right: 15px; }
        .carousel-btn:hover { background: #3b82f6; transform: translateY(-50%) scale(1.1); }
        
        .carousel-dots {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 10px;
            z-index: 10;
        }
        .dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.2);
            transition: all 0.3s;
            cursor: pointer;
        }
        .dot.active {
            background: #3b82f6;
            width: 24px;
            border-radius: 6px;
        }

        /* Animações */
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-up {
            animation: fadeInUp 0.6s ease-out forwards;
        }

        /* Botões Flutuantes */
        .social-float-container {
            position: fixed;
            bottom: 40px;
            right: 40px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            z-index: 100;
        }
        .float-btn {
            width: 70px;
            height: 70px;
            border-radius: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            color: white;
            box-shadow: 0 10px 25px rgba(0,0,0,0.4);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
            position: relative;
        }
        .float-btn:hover { transform: scale(1.15) rotate(5deg); }
        .btn-whatsapp { background: linear-gradient(135deg, #25d366, #128c7e); }
        .btn-instagram { background: linear-gradient(135deg, #833ab4, #fd1d1d, #fcb045); }
        .float-tooltip {
            position: absolute;
            right: 90px;
            background: #1e293b;
            color: white;
            padding: 12px 20px;
            border-radius: 12px;
            font-size: 14px;
            font-weight: bold;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            border: 1px solid #334155;
            box-shadow: 0 10px 15px rgba(0,0,0,0.2);
        }
        .float-btn:hover .float-tooltip { opacity: 1; }

        .hero-banner {
            position: relative;
            width: 100%;
            height: 85vh;
            background: linear-gradient(rgba(15, 23, 42, 0.6), rgba(15, 23, 42, 0.95)), url('https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&q=80&w=2070');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 20px;
        }

        /* Estilo para botão de filtro ativo */
        .filter-btn.active {
            background-color: #3b82f6;
            color: white;
            border-color: #3b82f6;
        }
    </style>
</head>
<body class="font-sans antialiased">

    <!-- Navegação -->
    <nav class="sticky top-0 z-[100] bg-slate-900/95 backdrop-blur-xl border-b border-slate-800 w-full px-6 md:px-12 py-6">
        <div class="w-full flex justify-between items-center">
            <div class="flex items-center space-x-4">
                <i class="fas fa-cube text-blue-500 text-5xl"></i>
                <div class="flex flex-col">
                    <span class="text-4xl font-black tracking-tighter uppercase leading-none">CROWLAB<span class="text-blue-500">3D</span></span>
                    <span class="text-[10px] tracking-[0.3em] uppercase text-slate-500 font-bold">Inovação em cada camada</span>
                </div>
            </div>
            
            <div class="flex items-center space-x-8">
                <button onclick="toggleCart()" class="p-4 bg-slate-800 hover:bg-slate-700 rounded-2xl transition-all relative group">
                    <i class="fas fa-shopping-cart text-2xl text-slate-300 group-hover:text-blue-400"></i>
                    <span id="cart-count" class="absolute -top-2 -right-2 bg-blue-600 text-white text-xs w-7 h-7 flex items-center justify-center rounded-full hidden font-black border-4 border-slate-900">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <header class="hero-banner text-center">
        <div class="animate-up">
            <h1 class="text-7xl md:text-[10rem] font-black mb-4 bg-gradient-to-b from-white via-slate-300 to-blue-600 bg-clip-text text-transparent uppercase tracking-tighter leading-none italic">
                CROWLAB3D
            </h1>
            <p class="text-slate-300 text-2xl md:text-3xl max-w-5xl mx-auto font-light leading-relaxed mb-12">
                Engenharia de precisão e design artístico fundidos em 3D. <br>
                <span class="text-blue-400 font-bold uppercase tracking-widest text-lg">Modelagem • Impressão • Prototipagem</span>
            </p>
            <a href="#products-grid" class="group relative px-12 py-5 bg-blue-600 rounded-full font-black text-xl transition-all hover:bg-blue-500 hover:scale-105 inline-flex items-center overflow-hidden">
                <span class="relative z-10 uppercase tracking-tighter">Explorar o Arsenal</span>
                <i class="fas fa-arrow-right ml-3 relative z-10 transition-transform group-hover:translate-x-2"></i>
                <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-700"></div>
            </a>
        </div>
    </header>

    <!-- Filtros -->
    <div class="w-full bg-slate-900 py-10 px-6 md:px-12 border-b border-slate-800 flex flex-wrap justify-center gap-6" id="filters-container">
        <button onclick="filterByCategory('Todos')" class="filter-btn active px-8 py-3 bg-blue-600/10 border border-blue-500/30 rounded-xl text-blue-400 font-bold hover:bg-blue-600 hover:text-white transition">Todos</button>
        <button onclick="filterByCategory('Colecionável')" class="filter-btn px-8 py-3 bg-slate-800 border border-slate-700 rounded-xl text-slate-400 font-bold hover:bg-slate-700 transition">Colecionáveis</button>
        <button onclick="filterByCategory('Setup Gaming')" class="filter-btn px-8 py-3 bg-slate-800 border border-slate-700 rounded-xl text-slate-400 font-bold hover:bg-slate-700 transition">Setup Gaming</button>
        <button onclick="filterByCategory('Decoração')" class="filter-btn px-8 py-3 bg-slate-800 border border-slate-700 rounded-xl text-slate-400 font-bold hover:bg-slate-700 transition">Decoração</button>
        <button onclick="filterByCategory('Peças Técnicas')" class="filter-btn px-8 py-3 bg-slate-800 border border-slate-700 rounded-xl text-slate-400 font-bold hover:bg-slate-700 transition">Peças Técnicas</button>
    </div>

    <!-- Galeria de Produtos -->
    <main class="w-full px-4 md:px-10 py-20 bg-slate-950">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5 gap-12" id="products-grid">
            <!-- Injetado por JS -->
        </div>
    </main>

    <!-- Footer -->
    <footer class="w-full border-t border-slate-800 py-24 bg-slate-900">
        <div class="px-6 md:px-12 grid grid-cols-1 lg:grid-cols-2 gap-16">
            <div class="space-y-6">
                <div class="flex items-center space-x-3">
                    <i class="fas fa-cube text-blue-500 text-4xl"></i>
                    <span class="text-3xl font-black tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
                </div>
                <p class="text-slate-400 text-lg leading-relaxed max-w-xl">
                    Elevando o padrão da impressão 3D no Brasil. Projetos customizados, materiais de alta performance e acabamento premium.
                </p>
                <p class="text-slate-600 font-mono text-sm pt-4 italic">
                    © 2026 CrowLab3D – Made by CLX. Todos os direitos reservados.
                </p>
            </div>
            <div class="space-y-6">
                <h4 class="text-xl font-black uppercase text-white tracking-widest">Links Rápidos</h4>
                <ul class="grid grid-cols-2 gap-4 text-slate-500 font-bold">
                    <li><a href="#" class="hover:text-blue-400 transition">Início</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition">Catálogo</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition">Sobre Nós</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition">Suporte Comercial</a></li>
                </ul>
            </div>
        </div>
    </footer>

    <!-- Botões Flutuantes -->
    <div class="social-float-container">
        <a href="https://www.instagram.com/crowlab3d/" target="_blank" class="float-btn btn-instagram">
            <i class="fab fa-instagram"></i>
            <div class="float-tooltip">Acompanhe no Instagram</div>
        </a>
        <div onclick="customOrder()" class="float-btn btn-whatsapp">
            <i class="fab fa-whatsapp"></i>
            <div class="float-tooltip">Orçamento Personalizado</div>
        </div>
    </div>

    <!-- Carrinho -->
    <div id="cart-drawer" class="fixed inset-y-0 right-0 w-full md:w-[550px] bg-slate-900 shadow-[0_0_100px_rgba(0,0,0,0.8)] z-[200] transform translate-x-full transition-transform duration-500 ease-in-out border-l border-slate-800">
        <div class="h-full flex flex-col">
            <div class="p-10 border-b border-slate-800 flex justify-between items-center bg-slate-950">
                <div class="flex flex-col">
                    <h2 class="text-3xl font-black uppercase tracking-tight italic">O Teu Carrinho</h2>
                    <span id="cart-item-info" class="text-blue-500 font-bold text-xs uppercase tracking-widest mt-1">Nenhum item adicionado</span>
                </div>
                <button onclick="toggleCart()" class="w-12 h-12 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white hover:bg-red-600 rounded-xl transition-all"><i class="fas fa-times text-2xl"></i></button>
            </div>
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-10 space-y-8 custom-scrollbar bg-slate-900">
                <div id="cart-items-list" class="space-y-8"></div>
                <div id="empty-cart-msg" class="text-slate-500 text-center py-20 flex flex-col items-center">
                    <i class="fas fa-box-open text-8xl mb-8 opacity-10"></i>
                    <p class="text-2xl font-light">Seu arsenal está vazio.</p>
                    <button onclick="toggleCart()" class="mt-8 text-blue-500 font-bold uppercase tracking-widest hover:underline">Começar a comprar</button>
                </div>
            </div>
            <div id="cart-footer" class="p-10 border-t border-slate-800 bg-slate-950 hidden">
                <div class="flex justify-between items-end mb-10">
                    <span class="text-slate-500 text-sm uppercase font-black tracking-[0.2em]">Investimento Total:</span>
                    <span id="cart-total" class="text-5xl font-black text-blue-400 tracking-tighter italic">R$ 0,00</span>
                </div>
                <button onclick="showCheckout()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black py-7 rounded-3xl transition-all text-2xl uppercase italic tracking-tighter shadow-2xl shadow-blue-900/40 transform hover:scale-[1.02] active:scale-95">
                    Finalizar Encomenda
                </button>
            </div>
        </div>
    </div>

    <!-- Mensagem Flutuante de Adição ao Carrinho -->
    <div id="add-toast" class="fixed bottom-12 left-12 bg-slate-900 border border-blue-500 text-white px-8 py-4 rounded-2xl shadow-2xl z-[500] transform translate-y-[200%] transition-transform duration-500 flex items-center space-x-4">
        <i class="fas fa-check-circle text-blue-500 text-xl"></i>
        <span class="font-bold uppercase tracking-tighter text-sm">Item adicionado ao arsenal!</span>
    </div>

    <!-- Modal de Checkout -->
    <div id="checkout-modal" class="fixed inset-0 z-[300] hidden items-center justify-center p-6 bg-slate-950/98 backdrop-blur-xl">
        <div class="bg-slate-900 w-full max-w-4xl rounded-[3rem] border border-slate-800 max-h-[90vh] overflow-y-auto custom-scrollbar shadow-[0_0_150px_rgba(59,130,246,0.1)]">
            <div class="p-12">
                <div class="flex justify-between items-center mb-12">
                    <h2 class="text-5xl font-black uppercase tracking-tighter italic">Checkout Seguro</h2>
                    <button onclick="hideCheckout()" class="w-14 h-14 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white hover:bg-red-600 rounded-2xl transition"><i class="fas fa-times text-2xl"></i></button>
                </div>
                <form id="checkout-form" onsubmit="handleOrder(event)" class="space-y-10">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                        <div class="group">
                            <label class="block text-slate-500 text-xs font-black mb-3 uppercase tracking-[0.2em]">Nome Completo</label>
                            <input type="text" required name="nome" class="w-full bg-slate-800 border-2 border-slate-700 rounded-2xl p-5 focus:border-blue-500 outline-none text-white transition-all text-lg font-bold">
                        </div>
                        <div class="group">
                            <label class="block text-slate-500 text-xs font-black mb-3 uppercase tracking-[0.2em]">Documento (CPF/CNPJ)</label>
                            <input type="text" required name="cpf" placeholder="000.000.000-00" class="w-full bg-slate-800 border-2 border-slate-700 rounded-2xl p-5 focus:border-blue-500 outline-none text-white transition-all text-lg font-bold">
                        </div>
                    </div>
                    <div>
                        <label class="block text-slate-500 text-xs font-black mb-3 uppercase tracking-[0.2em]">Morada Completa para Envio</label>
                        <textarea required name="endereco" rows="3" class="w-full bg-slate-800 border-2 border-slate-700 rounded-2xl p-5 focus:border-blue-500 outline-none text-white transition-all text-lg font-bold" placeholder="Rua, Número, Complemento, Bairro, Cidade e CEP"></textarea>
                    </div>
                    <div>
                        <label class="block text-slate-500 text-xs font-black mb-6 uppercase tracking-[0.2em]">Escolha a Forma de Pagamento</label>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-8">
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="pix" class="hidden peer" checked>
                                <div class="p-8 border-2 border-slate-700 rounded-[2rem] flex items-center space-x-5 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800 group">
                                    <i class="fa-brands fa-pix text-cyan-400 text-5xl"></i>
                                    <div class="flex flex-col">
                                        <span class="font-black text-2xl italic">PIX</span>
                                        <span class="text-xs text-slate-500 uppercase">Aprovação Imediata</span>
                                    </div>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="boleto" class="hidden peer">
                                <div class="p-8 border-2 border-slate-700 rounded-[2rem] flex items-center space-x-5 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800 group">
                                    <i class="fas fa-barcode text-slate-300 text-5xl"></i>
                                    <div class="flex flex-col">
                                        <span class="font-black text-2xl italic">BOLETO</span>
                                        <span class="text-xs text-slate-500 uppercase">1 a 2 dias úteis</span>
                                    </div>
                                </div>
                            </label>
                        </div>
                    </div>
                    <div class="pt-10">
                        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-black py-8 rounded-[2rem] transition-all text-3xl uppercase italic shadow-[0_15px_40px_rgba(22,163,74,0.3)] hover:-translate-y-1">
                            <i class="fab fa-whatsapp mr-4"></i> Finalizar via WhatsApp
                        </button>
                        <p class="text-center text-slate-600 text-xs mt-6 uppercase tracking-widest">Processado com criptografia militar</p>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Sucesso -->
    <div id="success-message" class="fixed top-12 left-1/2 -translate-x-1/2 bg-blue-600 text-white px-12 py-6 rounded-3xl shadow-[0_20px_60px_rgba(59,130,246,0.5)] z-[1000] transform -translate-y-[200%] transition-transform duration-700 font-black text-xl italic uppercase tracking-tighter flex items-center">
        <div class="mr-4 w-10 h-10 bg-white/20 rounded-full flex items-center justify-center animate-pulse">
            <i class="fas fa-check"></i>
        </div>
        Sincronizando com WhatsApp...
    </div>

    <script>
        const WHATSAPP_NUMBER = "5521990819172"; 

        const products = [
            { id: 1, name: "Escultura Dragon V2", price: 185.00, category: "Colecionável", images: ["img/dragon.jpg", "img/dragon_v2.jpg", "img/dragon_v3.jpg"] },
            { id: 2, name: "Organizador Hexa-Core", price: 45.90, category: "Peças Técnicas", images: ["img/organizador.jpg", "img/organizador2.jpg"] },
            { id: 3, name: "Action Figure Titan", price: 120.00, category: "Colecionável", images: ["img/hero.jpg", "img/hero_detalhe.jpg"] },
            { id: 4, name: "Luminária Cyber-Art", price: 159.00, category: "Decoração", images: ["img/luminaria.jpg", "img/luminaria2.jpg"] },
            { id: 5, name: "Chaveiro Low-Poly", price: 15.00, category: "Decoração", images: ["img/chaveiro.jpg", "img/chaveiro2.jpg"] },
            { id: 6, name: "Master Set RPG", price: 89.90, category: "Colecionável", images: ["img/rpg.jpg", "img/rpg2.jpg"] },
            { id: 7, name: "Suporte Headset Pro", price: 65.00, category: "Setup Gaming", images: ["img/suporte.jpg", "img/suporte2.jpg"] },
            { id: 8, name: "Vaso Minimal Geométrico", price: 35.00, category: "Decoração", images: ["img/vaso.jpg", "img/vaso2.jpg"] },
            { id: 9, name: "Engrenagem Helicoidal", price: 110.00, category: "Peças Técnicas", images: ["img/ssd.jpg", "img/ssd2.jpg"] },
            { id: 10, name: "Suporte Dual Monitor", price: 195.00, category: "Setup Gaming", images: ["img/monitor.jpg", "img/monitor2.jpg"] },
        ];

        let cart = [];
        const carouselStates = {}; 
        let currentFilter = 'Todos';

        function renderProducts(filter = 'Todos') {
            const grid = document.getElementById('products-grid');
            if (!grid) return;
            
            const filteredProducts = filter === 'Todos' 
                ? products 
                : products.filter(p => p.category === filter);

            if (filteredProducts.length === 0) {
                grid.innerHTML = `
                    <div class="col-span-full py-20 text-center text-slate-500">
                        <i class="fas fa-search text-6xl mb-4 opacity-20"></i>
                        <p class="text-xl italic">Nenhum item encontrado nesta categoria.</p>
                    </div>
                `;
                return;
            }

            grid.innerHTML = filteredProducts.map((product, index) => {
                carouselStates[product.id] = 0;
                const hasMultiple = product.images.length > 1;

                return `
                <div class="product-card bg-slate-900 border border-slate-800 rounded-[2.5rem] overflow-hidden flex flex-col group/card animate-up" style="animation-delay: ${index * 0.05}s">
                    <div class="carousel-container bg-black">
                        <div class="carousel-track" id="track-${product.id}">
                            ${product.images.map(img => `
                                <div class="carousel-item">
                                    <img src="${img}" alt="${product.name}" class="group-hover/card:scale-105 transition-transform duration-700" onerror="this.src='https://via.placeholder.com/800x800?text=CrowLab3D+Gallery'">
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
                                ${product.images.map((_, i) => `<div class="dot ${i === 0 ? 'active' : ''}" onclick="goToSlide(${product.id}, ${i})"></div>`).join('')}
                            </div>
                        ` : ''}

                        <span class="absolute top-6 left-6 z-20 bg-blue-600 text-white text-[10px] font-black uppercase tracking-[0.3em] px-5 py-2 rounded-full shadow-xl shadow-blue-900/40 border border-white/20">
                            ${product.category}
                        </span>
                    </div>

                    <div class="p-10 flex flex-col flex-1">
                        <h3 class="text-3xl font-black mb-4 tracking-tighter leading-none italic uppercase group-hover/card:text-blue-400 transition-colors">${product.name}</h3>
                        <div class="mt-auto flex justify-between items-end pt-8">
                            <div class="flex flex-col">
                                <span class="text-slate-500 text-[10px] font-black uppercase tracking-widest mb-1">A partir de</span>
                                <span class="text-white font-black text-3xl tracking-tighter italic leading-none">R$ ${product.price.toFixed(2)}</span>
                            </div>
                            <button onclick="addToCart(${product.id})" class="bg-slate-800 hover:bg-blue-600 text-white w-16 h-16 rounded-[1.5rem] flex items-center justify-center transition-all active:scale-90 shadow-lg group">
                                <i class="fas fa-cart-plus text-2xl group-hover:scale-110 transition-transform"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `}).join('');
        }

        window.filterByCategory = function(category) {
            currentFilter = category;
            const buttons = document.querySelectorAll('.filter-btn');
            buttons.forEach(btn => {
                const btnText = btn.innerText.trim();
                const isActive = (category === 'Todos' && btnText === 'Todos') || 
                               (category === 'Colecionável' && btnText === 'Colecionáveis') ||
                               (category === 'Peças Técnicas' && btnText === 'Peças Técnicas') ||
                               (btnText === category);

                if (isActive) {
                    btn.classList.add('active', 'bg-blue-600/10', 'border-blue-500/30', 'text-blue-400');
                    btn.classList.remove('bg-slate-800', 'border-slate-700', 'text-slate-400');
                } else {
                    btn.classList.remove('active', 'bg-blue-600/10', 'border-blue-500/30', 'text-blue-400');
                    btn.classList.add('bg-slate-800', 'border-slate-700', 'text-slate-400');
                }
            });
            renderProducts(category);
            if(window.innerWidth < 768) {
                document.getElementById('products-grid').scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
        };

        window.moveSlide = function(productId, direction) {
            const product = products.find(p => p.id === productId);
            const total = product.images.length;
            let current = carouselStates[productId];
            current = (current + direction + total) % total;
            carouselStates[productId] = current;
            updateCarouselUI(productId, current);
        };

        window.goToSlide = function(productId, index) {
            carouselStates[productId] = index;
            updateCarouselUI(productId, index);
        };

        function updateCarouselUI(productId, index) {
            const track = document.getElementById(`track-${productId}`);
            if(!track) return;
            track.style.transform = `translateX(-${index * 100}%)`;
            const dotsContainer = document.getElementById(`dots-${productId}`);
            if (dotsContainer) {
                const dots = dotsContainer.querySelectorAll('.dot');
                dots.forEach((dot, idx) => {
                    dot.classList.toggle('active', idx === index);
                });
            }
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
            
            // FEEDBACK VISUAL SEM ABRIR O DRAWER
            const toast = document.getElementById('add-toast');
            toast.classList.remove('translate-y-[200%]');
            setTimeout(() => {
                toast.classList.add('translate-y-[200%]');
            }, 3000);
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
            const infoMsg = document.getElementById('cart-item-info');

            const totalItems = cart.reduce((acc, item) => acc + item.quantity, 0);

            if (cart.length === 0) {
                emptyMsg.classList.remove('hidden');
                footer.classList.add('hidden');
                countEl.classList.add('hidden');
                infoMsg.innerText = "Nenhum item adicionado";
                listContainer.innerHTML = '';
            } else {
                emptyMsg.classList.add('hidden');
                footer.classList.remove('hidden');
                countEl.classList.remove('hidden');
                countEl.innerText = totalItems;
                infoMsg.innerText = `${totalItems} item(s) pronto(s) para combate`;

                listContainer.innerHTML = cart.map(item => `
                    <div class="flex items-center justify-between bg-slate-950/50 p-6 rounded-[2rem] border border-slate-800 animate-up">
                        <div class="flex items-center space-x-6">
                            <div class="w-24 h-24 rounded-2xl overflow-hidden flex-shrink-0 border-2 border-slate-800">
                                <img src="${item.images[0]}" class="w-full h-full object-cover" onerror="this.src='https://via.placeholder.com/200x200'">
                            </div>
                            <div>
                                <h4 class="font-black text-2xl italic tracking-tighter uppercase leading-none mb-2">${item.name}</h4>
                                <div class="flex items-center space-x-4">
                                    <span class="text-blue-500 font-mono font-bold text-lg italic">${item.quantity}x</span>
                                    <span class="text-slate-500 font-bold tracking-widest text-sm">R$ ${item.price.toFixed(2)}</span>
                                </div>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="w-14 h-14 bg-slate-800 text-slate-500 hover:text-red-500 hover:bg-red-500/10 transition-all rounded-2xl">
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
            const message = "👋 Olá CrowLab3D! Tenho um projeto especial e gostaria de solicitar um orçamento personalizado.";
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
        }

        function handleOrder(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const cliente = Object.fromEntries(formData);
            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);

            let message = `🛸 *NOVA ENCOMENDA CROWLAB3D*\n`;
            message += `──────────────────\n`;
            message += `👤 *CLIENTE:* ${cliente.nome.toUpperCase()}\n`;
            message += `🆔 *DOC:* ${cliente.cpf}\n`;
            message += `📍 *MORADA:* ${cliente.endereco}\n`;
            message += `💳 *PAGAMENTO:* ${cliente.pagamento.toUpperCase()}\n`;
            message += `──────────────────\n`;
            message += `📦 *EQUIPAMENTOS:*\n`;
            cart.forEach(item => {
                message += `▪️ ${item.quantity}x ${item.name} - R$ ${(item.price * item.quantity).toFixed(2)}\n`;
            });
            message += `──────────────────\n`;
            message += `💰 *TOTAL: R$ ${total.toFixed(2)}*\n`;
            message += `──────────────────\n`;
            message += `_Aguardando instruções de pagamento._`;

            document.getElementById('success-message').classList.remove('-translate-y-[200%]');
            setTimeout(() => {
                window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
                hideCheckout();
                document.getElementById('success-message').classList.add('-translate-y-[200%]');
                cart = [];
                updateCart();
                e.target.reset();
            }, 1500);
        }

        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            updateCart();
        });
    </script>
</body>
</html>
