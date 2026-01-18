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
            width: 10px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #1e293b;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 5px;
        }

        /* Efeitos de Card */
        .product-card {
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275), box-shadow 0.3s ease;
        }
        .product-card:hover {
            transform: translateY(-12px);
            box-shadow: 0 30px 45px -15px rgba(59, 130, 246, 0.4);
        }
        
        /* Carrossel de Imagens Interno */
        .carousel-container {
            position: relative;
            width: 100%;
            height: 450px; /* Aumentado */
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
            background: rgba(15, 23, 42, 0.9);
            color: white;
            padding: 18px; /* Aumentado */
            cursor: pointer;
            border-radius: 50%;
            opacity: 0;
            transition: all 0.3s;
            z-index: 10;
            border: 1px solid rgba(255,255,255,0.2);
        }
        .product-card:hover .carousel-btn {
            opacity: 1;
        }
        .btn-prev { left: 20px; }
        .btn-next { right: 20px; }
        .carousel-btn:hover { background: #3b82f6; transform: translateY(-50%) scale(1.2); }
        
        .carousel-dots {
            position: absolute;
            bottom: 25px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 12px;
            z-index: 10;
        }
        .dot {
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transition: all 0.3s;
            cursor: pointer;
        }
        .dot.active {
            background: #3b82f6;
            width: 30px;
            border-radius: 8px;
        }

        /* Animações */
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(40px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-up {
            animation: fadeInUp 0.7s ease-out forwards;
        }

        /* Botões Flutuantes */
        .social-float-container {
            position: fixed;
            bottom: 50px;
            right: 50px;
            display: flex;
            flex-direction: column;
            gap: 25px;
            z-index: 100;
        }
        .float-btn {
            width: 85px; /* Aumentado */
            height: 85px; /* Aumentado */
            border-radius: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 38px;
            color: white;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
            position: relative;
        }
        .float-btn:hover { transform: scale(1.15) rotate(5deg); }
        .btn-whatsapp { background: linear-gradient(135deg, #25d366, #128c7e); }
        .btn-instagram { background: linear-gradient(135deg, #833ab4, #fd1d1d, #fcb045); }
        .float-tooltip {
            position: absolute;
            right: 110px;
            background: #1e293b;
            color: white;
            padding: 15px 25px;
            border-radius: 15px;
            font-size: 16px;
            font-weight: bold;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            border: 1px solid #334155;
            box-shadow: 0 15px 25px rgba(0,0,0,0.3);
        }
        .float-btn:hover .float-tooltip { opacity: 1; }

        .hero-banner {
            position: relative;
            width: 100%;
            height: 90vh; /* Aumentado */
            background: linear-gradient(rgba(15, 23, 42, 0.6), rgba(15, 23, 42, 0.95)), url('https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&q=80&w=2070');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 40px;
        }

        /* Estilo para botão de filtro ativo */
        .filter-btn.active {
            background-color: #3b82f6;
            color: white;
            border-color: #3b82f6;
            box-shadow: 0 10px 20px -5px rgba(59, 130, 246, 0.5);
        }
    </style>
</head>
<body class="font-sans antialiased">

    <!-- Navegação Ampliada -->
    <nav class="sticky top-0 z-[100] bg-slate-900/95 backdrop-blur-2xl border-b border-slate-800 w-full px-8 md:px-20 py-8">
        <div class="w-full flex justify-between items-center">
            <div class="flex items-center space-x-6">
                <i class="fas fa-cube text-blue-500 text-6xl"></i>
                <div class="flex flex-col">
                    <span class="text-5xl font-black tracking-tighter uppercase leading-none">CROWLAB<span class="text-blue-500">3D</span></span>
                    <span class="text-sm tracking-[0.4em] uppercase text-slate-500 font-black mt-1">Inovação em cada camada</span>
                </div>
            </div>
            
            <div class="flex items-center space-x-12">
                <button onclick="toggleCart()" class="p-6 bg-slate-800 hover:bg-slate-700 rounded-3xl transition-all relative group border border-slate-700">
                    <i class="fas fa-shopping-cart text-3xl text-slate-300 group-hover:text-blue-400"></i>
                    <span id="cart-count" class="absolute -top-3 -right-3 bg-blue-600 text-white text-sm w-9 h-9 flex items-center justify-center rounded-full hidden font-black border-4 border-slate-900">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section Ampliada -->
    <header class="hero-banner text-center">
        <div class="animate-up">
            <h1 class="text-8xl md:text-[12rem] font-black mb-6 bg-gradient-to-b from-white via-slate-300 to-blue-600 bg-clip-text text-transparent uppercase tracking-tighter leading-none italic">
                CROWLAB3D
            </h1>
            <p class="text-slate-300 text-3xl md:text-4xl max-w-6xl mx-auto font-light leading-relaxed mb-16">
                Engenharia de precisão e design artístico fundidos em 3D. <br>
                <span class="text-blue-400 font-bold uppercase tracking-widest text-xl">Modelagem • Impressão • Prototipagem</span>
            </p>
            <a href="#products-grid" class="group relative px-16 py-7 bg-blue-600 rounded-full font-black text-2xl transition-all hover:bg-blue-500 hover:scale-105 inline-flex items-center overflow-hidden shadow-2xl shadow-blue-900/40">
                <span class="relative z-10 uppercase tracking-tighter">Explorar o Arsenal</span>
                <i class="fas fa-arrow-right ml-4 relative z-10 transition-transform group-hover:translate-x-3"></i>
                <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-700"></div>
            </a>
        </div>
    </header>

    <!-- Filtros Ampliados -->
    <div class="w-full bg-slate-900 py-14 px-8 md:px-20 border-b border-slate-800 flex flex-wrap justify-center gap-8" id="filters-container">
        <button onclick="filterByCategory('Todos')" class="filter-btn active px-10 py-4 bg-blue-600/10 border border-blue-500/30 rounded-2xl text-blue-400 font-black text-lg hover:bg-blue-600 hover:text-white transition">Todos</button>
        <button onclick="filterByCategory('Colecionável')" class="filter-btn px-10 py-4 bg-slate-800 border border-slate-700 rounded-2xl text-slate-400 font-black text-lg hover:bg-slate-700 transition">Colecionáveis</button>
        <button onclick="filterByCategory('Setup Gaming')" class="filter-btn px-10 py-4 bg-slate-800 border border-slate-700 rounded-2xl text-slate-400 font-black text-lg hover:bg-slate-700 transition">Setup Gaming</button>
        <button onclick="filterByCategory('Decoração')" class="filter-btn px-10 py-4 bg-slate-800 border border-slate-700 rounded-2xl text-slate-400 font-black text-lg hover:bg-slate-700 transition">Decoração</button>
        <button onclick="filterByCategory('Peças Técnicas')" class="filter-btn px-10 py-4 bg-slate-800 border border-slate-700 rounded-2xl text-slate-400 font-black text-lg hover:bg-slate-700 transition">Peças Técnicas</button>
    </div>

    <!-- Galeria de Produtos -->
    <main class="w-full px-6 md:px-20 py-24 bg-slate-950">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-16" id="products-grid">
            <!-- Injetado por JS -->
        </div>
    </main>

    <!-- Footer Ampliado -->
    <footer class="w-full border-t border-slate-800 py-32 bg-slate-900">
        <div class="px-8 md:px-20 grid grid-cols-1 lg:grid-cols-2 gap-24">
            <div class="space-y-10">
                <div class="flex items-center space-x-5">
                    <i class="fas fa-cube text-blue-500 text-6xl"></i>
                    <span class="text-5xl font-black tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
                </div>
                <p class="text-slate-400 text-2xl leading-relaxed max-w-2xl">
                    Elevando o padrão da impressão 3D no Brasil. Projetos customizados, materiais de alta performance e acabamento premium.
                </p>
                <div class="flex space-x-8 pt-6">
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-4xl transition-all"><i class="fab fa-instagram"></i></a>
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-4xl transition-all"><i class="fab fa-whatsapp"></i></a>
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-4xl transition-all"><i class="fab fa-youtube"></i></a>
                </div>
                <p class="text-slate-600 font-mono text-base pt-10 italic">
                    © 2026 CrowLab3D – Made by CLX. Todos os direitos reservados.
                </p>
            </div>
            <div class="space-y-10">
                <h4 class="text-2xl font-black uppercase text-white tracking-[0.2em]">Links de Acesso</h4>
                <ul class="grid grid-cols-1 sm:grid-cols-2 gap-8 text-slate-500 font-black text-xl">
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Início</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Catálogo</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Sobre Nós</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Projetos Sob Medida</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Suporte Comercial</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all flex items-center"><i class="fas fa-chevron-right text-xs mr-3 text-blue-500"></i>Galeria de Clientes</a></li>
                </ul>
            </div>
        </div>
    </footer>

    <!-- Botões Flutuantes Ampliados -->
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

    <!-- Carrinho Ampliado -->
    <div id="cart-drawer" class="fixed inset-y-0 right-0 w-full md:w-[650px] bg-slate-900 shadow-[0_0_120px_rgba(0,0,0,0.9)] z-[200] transform translate-x-full transition-transform duration-500 ease-in-out border-l border-slate-800">
        <div class="h-full flex flex-col">
            <div class="p-12 border-b border-slate-800 flex justify-between items-center bg-slate-950">
                <div class="flex flex-col">
                    <h2 class="text-4xl font-black uppercase tracking-tight italic">O Teu Carrinho</h2>
                    <span id="cart-item-info" class="text-blue-500 font-black text-sm uppercase tracking-widest mt-2">Nenhum item adicionado</span>
                </div>
                <button onclick="toggleCart()" class="w-16 h-16 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white hover:bg-red-600 rounded-2xl transition-all border border-slate-700"><i class="fas fa-times text-3xl"></i></button>
            </div>
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-12 space-y-10 custom-scrollbar bg-slate-900">
                <div id="cart-items-list" class="space-y-10"></div>
                <div id="empty-cart-msg" class="text-slate-500 text-center py-24 flex flex-col items-center">
                    <i class="fas fa-box-open text-9xl mb-10 opacity-10"></i>
                    <p class="text-3xl font-light">Seu arsenal está vazio.</p>
                    <button onclick="toggleCart()" class="mt-10 text-blue-500 font-black uppercase tracking-[0.2em] text-lg hover:underline">Começar a comprar</button>
                </div>
            </div>
            <div id="cart-footer" class="p-12 border-t border-slate-800 bg-slate-950 hidden">
                <div class="flex justify-between items-end mb-12">
                    <span class="text-slate-500 text-base uppercase font-black tracking-[0.3em]">Investimento Total:</span>
                    <span id="cart-total" class="text-6xl font-black text-blue-400 tracking-tighter italic">R$ 0,00</span>
                </div>
                <button onclick="showCheckout()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black py-8 rounded-[2.5rem] transition-all text-3xl uppercase italic tracking-tighter shadow-2xl shadow-blue-900/50 transform hover:scale-[1.02] active:scale-95">
                    Finalizar Encomenda
                </button>
            </div>
        </div>
    </div>

    <!-- Mensagem Flutuante Ampliada -->
    <div id="add-toast" class="fixed bottom-16 left-16 bg-slate-900 border-2 border-blue-500 text-white px-10 py-6 rounded-3xl shadow-[0_30px_60px_rgba(0,0,0,0.6)] z-[500] transform translate-y-[250%] transition-transform duration-500 flex items-center space-x-6">
        <div class="bg-blue-600 w-10 h-10 rounded-full flex items-center justify-center">
            <i class="fas fa-check text-xl"></i>
        </div>
        <span class="font-black uppercase tracking-tighter text-lg">Item adicionado ao arsenal!</span>
    </div>

    <!-- Modal de Checkout Ampliado -->
    <div id="checkout-modal" class="fixed inset-0 z-[300] hidden items-center justify-center p-8 bg-slate-950/98 backdrop-blur-2xl">
        <div class="bg-slate-900 w-full max-w-5xl rounded-[4rem] border border-slate-800 max-h-[95vh] overflow-y-auto custom-scrollbar shadow-[0_0_200px_rgba(59,130,246,0.15)]">
            <div class="p-16">
                <div class="flex justify-between items-center mb-16">
                    <h2 class="text-6xl font-black uppercase tracking-tighter italic">Checkout Seguro</h2>
                    <button onclick="hideCheckout()" class="w-20 h-20 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white hover:bg-red-600 rounded-[2rem] transition border border-slate-700"><i class="fas fa-times text-3xl"></i></button>
                </div>
                <form id="checkout-form" onsubmit="handleOrder(event)" class="space-y-12">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
                        <div class="group">
                            <label class="block text-slate-500 text-sm font-black mb-4 uppercase tracking-[0.3em]">Nome Completo</label>
                            <input type="text" required name="nome" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2rem] p-7 focus:border-blue-500 outline-none text-white transition-all text-2xl font-black italic">
                        </div>
                        <div class="group">
                            <label class="block text-slate-500 text-sm font-black mb-4 uppercase tracking-[0.3em]">Documento (CPF/CNPJ)</label>
                            <input type="text" required name="cpf" placeholder="000.000.000-00" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2rem] p-7 focus:border-blue-500 outline-none text-white transition-all text-2xl font-black italic">
                        </div>
                    </div>
                    <div>
                        <label class="block text-slate-500 text-sm font-black mb-4 uppercase tracking-[0.3em]">Morada Completa para Envio</label>
                        <textarea required name="endereco" rows="4" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2rem] p-7 focus:border-blue-500 outline-none text-white transition-all text-2xl font-black italic" placeholder="Rua, Número, Complemento, Bairro, Cidade e CEP"></textarea>
                    </div>
                    <div>
                        <label class="block text-slate-500 text-sm font-black mb-8 uppercase tracking-[0.3em]">Escolha a Forma de Pagamento</label>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-10">
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="pix" class="hidden peer" checked>
                                <div class="p-10 border-2 border-slate-700 rounded-[3rem] flex items-center space-x-7 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800 group">
                                    <i class="fa-brands fa-pix text-cyan-400 text-6xl"></i>
                                    <div class="flex flex-col">
                                        <span class="font-black text-3xl italic">PIX</span>
                                        <span class="text-sm text-slate-500 uppercase font-bold tracking-widest mt-1">Aprovação Imediata</span>
                                    </div>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="pagamento" value="boleto" class="hidden peer">
                                <div class="p-10 border-2 border-slate-700 rounded-[3rem] flex items-center space-x-7 peer-checked:border-blue-500 peer-checked:bg-blue-500/10 transition-all hover:bg-slate-800 group">
                                    <i class="fas fa-barcode text-slate-300 text-6xl"></i>
                                    <div class="flex flex-col">
                                        <span class="font-black text-3xl italic">BOLETO</span>
                                        <span class="text-sm text-slate-500 uppercase font-bold tracking-widest mt-1">1 a 2 dias úteis</span>
                                    </div>
                                </div>
                            </label>
                        </div>
                    </div>
                    <div class="pt-12">
                        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-black py-10 rounded-[3rem] transition-all text-4xl uppercase italic shadow-[0_20px_50px_rgba(22,163,74,0.4)] hover:-translate-y-2">
                            <i class="fab fa-whatsapp mr-6"></i> Finalizar via WhatsApp
                        </button>
                        <p class="text-center text-slate-600 text-sm mt-8 uppercase tracking-[0.4em] font-black">Processado com Criptografia Nível Militar</p>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Mensagem de Sucesso Ampliada -->
    <div id="success-message" class="fixed top-16 left-1/2 -translate-x-1/2 bg-blue-600 text-white px-16 py-8 rounded-[2.5rem] shadow-[0_30px_80px_rgba(59,130,246,0.6)] z-[1000] transform -translate-y-[250%] transition-transform duration-700 font-black text-2xl italic uppercase tracking-tighter flex items-center">
        <div class="mr-6 w-14 h-14 bg-white/20 rounded-full flex items-center justify-center animate-pulse">
            <i class="fas fa-sync-alt fa-spin"></i>
        </div>
        Sincronizando com WhatsApp...
    </div>

    <script>
        const WHATSAPP_NUMBER = "5521990819172"; 

        const products = [
            { id: 1, name: "Escultura Dragon V2", price: 185.00, category: "Colecionável", images: ["https://images.unsplash.com/photo-1590233464442-553fd340cce9?auto=format&fit=crop&q=80&w=800", "https://images.unsplash.com/photo-1612036782180-6f0b6cd846fe?auto=format&fit=crop&q=80&w=800"] },
            { id: 2, name: "Organizador Hexa-Core", price: 45.90, category: "Peças Técnicas", images: ["https://images.unsplash.com/photo-1589254065878-42c9da997008?auto=format&fit=crop&q=80&w=800"] },
            { id: 3, name: "Action Figure Titan", price: 120.00, category: "Colecionável", images: ["https://images.unsplash.com/photo-1558655146-d09347e92766?auto=format&fit=crop&q=80&w=800"] },
            { id: 4, name: "Luminária Cyber-Art", price: 159.00, category: "Decoração", images: ["https://images.unsplash.com/photo-1534073828943-f801091bb18c?auto=format&fit=crop&q=80&w=800"] },
            { id: 5, name: "Chaveiro Low-Poly", price: 15.00, category: "Decoração", images: ["https://images.unsplash.com/photo-1544816155-12df9643f363?auto=format&fit=crop&q=80&w=800"] },
            { id: 6, name: "Master Set RPG", price: 89.90, category: "Colecionável", images: ["https://images.unsplash.com/photo-1611996575749-79a3a250f948?auto=format&fit=crop&q=80&w=800"] },
            { id: 7, name: "Suporte Headset Pro", price: 65.00, category: "Setup Gaming", images: ["https://images.unsplash.com/photo-1594633312681-425c7b97ccd1?auto=format&fit=crop&q=80&w=800"] },
            { id: 8, name: "Vaso Minimal Geométrico", price: 35.00, category: "Decoração", images: ["https://images.unsplash.com/photo-1485955900006-10f4d324d411?auto=format&fit=crop&q=80&w=800"] },
            { id: 9, name: "Engrenagem Helicoidal", price: 110.00, category: "Peças Técnicas", images: ["https://images.unsplash.com/photo-1537462715879-360eeb61a0ad?auto=format&fit=crop&q=80&w=800"] },
            { id: 10, name: "Suporte Dual Monitor", price: 195.00, category: "Setup Gaming", images: ["https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?auto=format&fit=crop&q=80&w=800"] },
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

            grid.innerHTML = filteredProducts.map((product, index) => {
                carouselStates[product.id] = 0;
                const hasMultiple = product.images.length > 1;

                return `
                <div class="product-card bg-slate-900 border border-slate-800 rounded-[3.5rem] overflow-hidden flex flex-col group/card animate-up" style="animation-delay: ${index * 0.05}s">
                    <div class="carousel-container bg-black">
                        <div class="carousel-track" id="track-${product.id}">
                            ${product.images.map(img => `
                                <div class="carousel-item">
                                    <img src="${img}" alt="${product.name}" class="group-hover/card:scale-110 transition-transform duration-1000" onerror="this.src='https://via.placeholder.com/800x800?text=CrowLab3D+Object'">
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

                        <span class="absolute top-8 left-8 z-20 bg-blue-600 text-white text-xs font-black uppercase tracking-[0.4em] px-7 py-3 rounded-full shadow-2xl shadow-blue-900/50 border border-white/20">
                            ${product.category}
                        </span>
                    </div>

                    <div class="p-12 flex flex-col flex-1">
                        <h3 class="text-4xl font-black mb-6 tracking-tighter leading-none italic uppercase group-hover/card:text-blue-400 transition-colors">${product.name}</h3>
                        <div class="mt-auto flex justify-between items-end pt-10">
                            <div class="flex flex-col">
                                <span class="text-slate-500 text-xs font-black uppercase tracking-[0.2em] mb-2">Preço Arsenal</span>
                                <span class="text-white font-black text-4xl tracking-tighter italic leading-none">R$ ${product.price.toFixed(2)}</span>
                            </div>
                            <button onclick="addToCart(${product.id})" class="bg-slate-800 hover:bg-blue-600 text-white w-20 h-20 rounded-[2rem] flex items-center justify-center transition-all active:scale-90 shadow-2xl group border border-slate-700">
                                <i class="fas fa-cart-plus text-3xl group-hover:scale-125 transition-transform"></i>
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
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            renderProducts(category);
            document.getElementById('products-grid').scrollIntoView({ behavior: 'smooth', block: 'start' });
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
            
            const toast = document.getElementById('add-toast');
            toast.classList.remove('translate-y-[250%]');
            setTimeout(() => {
                toast.classList.add('translate-y-[250%]');
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
                infoMsg.innerText = `${totalItems} EQUIPAMENTO(S) SINCRONIZADO(S)`;

                listContainer.innerHTML = cart.map(item => `
                    <div class="flex items-center justify-between bg-slate-950/60 p-8 rounded-[2.5rem] border border-slate-800 animate-up">
                        <div class="flex items-center space-x-8">
                            <div class="w-32 h-32 rounded-3xl overflow-hidden flex-shrink-0 border-2 border-slate-800 shadow-xl">
                                <img src="${item.images[0]}" class="w-full h-full object-cover" onerror="this.src='https://via.placeholder.com/400x400'">
                            </div>
                            <div>
                                <h4 class="font-black text-3xl italic tracking-tighter uppercase leading-none mb-3">${item.name}</h4>
                                <div class="flex items-center space-x-6">
                                    <span class="text-blue-500 font-black text-2xl italic">${item.quantity}x</span>
                                    <span class="text-slate-500 font-black tracking-widest text-base">R$ ${item.price.toFixed(2)}</span>
                                </div>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="w-16 h-16 bg-slate-800 text-slate-500 hover:text-red-500 hover:bg-red-500/10 transition-all rounded-[1.5rem] border border-slate-700">
                            <i class="fas fa-trash-alt text-2xl"></i>
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

            document.getElementById('success-message').classList.remove('-translate-y-[250%]');
            setTimeout(() => {
                window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
                hideCheckout();
                document.getElementById('success-message').classList.add('-translate-y-[250%]');
                cart = [];
                updateCart();
                e.target.reset();
            }, 1800);
        }

        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            updateCart();
        });
    </script>
</body>
</html>
