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

        /* Efeitos de Card - Ajustados para Grid Largo */
        .product-card {
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275), box-shadow 0.3s ease;
            width: 100%; /* Garante que o card ocupe o espaço do grid */
        }
        .product-card:hover {
            transform: translateY(-12px);
            box-shadow: 0 30px 45px -15px rgba(59, 130, 246, 0.4);
        }
        
        /* Carrossel de Imagens Interno */
        .carousel-container {
            position: relative;
            width: 100%;
            height: 500px; /* Aumentado para preencher melhor a tela */
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
            padding: 20px;
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

        /* Hero Section Full-Width */
        .hero-banner {
            position: relative;
            width: 100%;
            height: 95vh;
            background: linear-gradient(rgba(15, 23, 42, 0.5), rgba(15, 23, 42, 0.9)), url('https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&q=80&w=2070');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 5%;
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
            width: 75px;
            height: 75px;
            border-radius: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            color: white;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
        }
        .btn-whatsapp { background: linear-gradient(135deg, #25d366, #128c7e); }
        .btn-instagram { background: linear-gradient(135deg, #833ab4, #fd1d1d, #fcb045); }

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

    <!-- Navegação 100% de Largura -->
    <nav class="sticky top-0 z-[100] bg-slate-900/95 backdrop-blur-2xl border-b border-slate-800 w-full px-10 py-10">
        <div class="w-full flex justify-between items-center">
            <div class="flex items-center space-x-8">
                <i class="fas fa-cube text-blue-500 text-7xl"></i>
                <div class="flex flex-col">
                    <span class="text-6xl font-black tracking-tighter uppercase leading-none">CROWLAB<span class="text-blue-500">3D</span></span>
                    <span class="text-lg tracking-[0.5em] uppercase text-slate-500 font-black mt-2">Inovação em cada camada</span>
                </div>
            </div>
            
            <div class="flex items-center space-x-16">
                <button onclick="toggleCart()" class="p-8 bg-slate-800 hover:bg-slate-700 rounded-[2.5rem] transition-all relative group border border-slate-700">
                    <i class="fas fa-shopping-cart text-4xl text-slate-300 group-hover:text-blue-400"></i>
                    <span id="cart-count" class="absolute -top-4 -right-4 bg-blue-600 text-white text-lg w-10 h-10 flex items-center justify-center rounded-full hidden font-black border-4 border-slate-900">0</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section Expandida -->
    <header class="hero-banner text-center">
        <div class="animate-up w-full px-4">
            <h1 class="text-9xl md:text-[15rem] font-black mb-10 bg-gradient-to-b from-white via-slate-200 to-blue-600 bg-clip-text text-transparent uppercase tracking-tighter leading-none italic">
                CROWLAB3D
            </h1>
            <p class="text-slate-300 text-4xl md:text-5xl font-light leading-relaxed mb-20">
                Engenharia de precisão e design artístico fundidos em 3D.
            </p>
            <a href="#products-grid" class="group relative px-20 py-8 bg-blue-600 rounded-full font-black text-3xl transition-all hover:bg-blue-500 hover:scale-105 inline-flex items-center overflow-hidden shadow-2xl shadow-blue-900/40">
                <span class="relative z-10 uppercase tracking-tighter">Explorar o Arsenal</span>
                <i class="fas fa-arrow-right ml-6 relative z-10 transition-transform group-hover:translate-x-3"></i>
                <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-700"></div>
            </a>
        </div>
    </header>

    <!-- Filtros de Tela Inteira -->
    <div class="w-full bg-slate-900 py-16 px-10 border-b border-slate-800 flex flex-wrap justify-center gap-10" id="filters-container">
        <button onclick="filterByCategory('Todos')" class="filter-btn active px-12 py-5 bg-blue-600/10 border border-blue-500/30 rounded-3xl text-blue-400 font-black text-xl hover:bg-blue-600 hover:text-white transition">Todos</button>
        <button onclick="filterByCategory('Colecionável')" class="filter-btn px-12 py-5 bg-slate-800 border border-slate-700 rounded-3xl text-slate-400 font-black text-xl hover:bg-slate-700 transition">Colecionáveis</button>
        <button onclick="filterByCategory('Setup Gaming')" class="filter-btn px-12 py-5 bg-slate-800 border border-slate-700 rounded-3xl text-slate-400 font-black text-xl hover:bg-slate-700 transition">Setup Gaming</button>
        <button onclick="filterByCategory('Decoração')" class="filter-btn px-12 py-5 bg-slate-800 border border-slate-700 rounded-3xl text-slate-400 font-black text-xl hover:bg-slate-700 transition">Decoração</button>
        <button onclick="filterByCategory('Peças Técnicas')" class="filter-btn px-12 py-5 bg-slate-800 border border-slate-700 rounded-3xl text-slate-400 font-black text-xl hover:bg-slate-700 transition">Peças Técnicas</button>
    </div>

    <!-- Galeria de Produtos Grid Total -->
    <main class="w-full px-10 py-24 bg-slate-950">
        <!-- O grid agora ocupa 100% da largura disponível sem limites máximos -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5 gap-12" id="products-grid">
            <!-- Injetado por JS -->
        </div>
    </main>

    <!-- Footer Full-Width -->
    <footer class="w-full border-t border-slate-800 py-32 bg-slate-900 px-10">
        <div class="w-full grid grid-cols-1 lg:grid-cols-2 gap-32">
            <div class="space-y-12">
                <div class="flex items-center space-x-6">
                    <i class="fas fa-cube text-blue-500 text-7xl"></i>
                    <span class="text-6xl font-black tracking-tighter uppercase">CROWLAB<span class="text-blue-500">3D</span></span>
                </div>
                <p class="text-slate-400 text-3xl leading-relaxed">
                    Elevando o padrão da impressão 3D no Brasil. Projetos customizados, materiais de alta performance e acabamento premium.
                </p>
                <div class="flex space-x-12 pt-8">
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-5xl transition-all"><i class="fab fa-instagram"></i></a>
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-5xl transition-all"><i class="fab fa-whatsapp"></i></a>
                    <a href="#" class="text-slate-500 hover:text-blue-500 text-5xl transition-all"><i class="fab fa-youtube"></i></a>
                </div>
                <p class="text-slate-600 font-mono text-lg pt-12 italic">
                    © 2026 CrowLab3D – Made by CLX. Todos os direitos reservados.
                </p>
            </div>
            <div class="space-y-12">
                <h4 class="text-3xl font-black uppercase text-white tracking-[0.3em]">Navegação Rápida</h4>
                <ul class="grid grid-cols-1 sm:grid-cols-2 gap-10 text-slate-500 font-black text-2xl">
                    <li><a href="#" class="hover:text-blue-400 transition-all">Início</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all">Catálogo Completo</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all">Sobre a Engenharia</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all">Projetos Sob Medida</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all">Suporte Comercial</a></li>
                    <li><a href="#" class="hover:text-blue-400 transition-all">Galeria de Clientes</a></li>
                </ul>
            </div>
        </div>
    </footer>

    <!-- Carrinho Lateral Amplo -->
    <div id="cart-drawer" class="fixed inset-y-0 right-0 w-full md:w-[750px] bg-slate-900 shadow-[0_0_150px_rgba(0,0,0,1)] z-[200] transform translate-x-full transition-transform duration-500 ease-in-out border-l border-slate-800">
        <div class="h-full flex flex-col">
            <div class="p-16 border-b border-slate-800 flex justify-between items-center bg-slate-950">
                <h2 class="text-5xl font-black uppercase tracking-tight italic">Seu Arsenal</h2>
                <button onclick="toggleCart()" class="w-20 h-20 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white hover:bg-red-600 rounded-[2rem] transition-all border border-slate-700"><i class="fas fa-times text-4xl"></i></button>
            </div>
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-16 space-y-12 custom-scrollbar">
                <div id="cart-items-list" class="space-y-12"></div>
                <div id="empty-cart-msg" class="text-slate-500 text-center py-32">
                    <i class="fas fa-box-open text-[12rem] mb-12 opacity-10"></i>
                    <p class="text-4xl font-light">Seu arsenal está vazio.</p>
                </div>
            </div>
            <div id="cart-footer" class="p-16 border-t border-slate-800 bg-slate-950 hidden">
                <div class="flex justify-between items-end mb-16">
                    <span class="text-slate-500 text-xl uppercase font-black tracking-widest">Total:</span>
                    <span id="cart-total" class="text-7xl font-black text-blue-400 tracking-tighter italic">R$ 0,00</span>
                </div>
                <button onclick="showCheckout()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black py-10 rounded-[3rem] transition-all text-4xl uppercase italic tracking-tighter shadow-2xl">
                    Fechar Pedido
                </button>
            </div>
        </div>
    </div>

    <!-- Botões Sociais -->
    <div class="social-float-container">
        <a href="https://www.instagram.com/crowlab3d/" target="_blank" class="float-btn btn-instagram"><i class="fab fa-instagram"></i></a>
        <div onclick="customOrder()" class="float-btn btn-whatsapp"><i class="fab fa-whatsapp"></i></div>
    </div>

    <!-- Toast de Adição -->
    <div id="add-toast" class="fixed bottom-20 left-20 bg-slate-900 border-2 border-blue-500 text-white px-12 py-8 rounded-[2.5rem] shadow-2xl z-[500] transform translate-y-[300%] transition-transform duration-500 flex items-center space-x-8">
        <div class="bg-blue-600 w-12 h-12 rounded-full flex items-center justify-center"><i class="fas fa-check text-2xl"></i></div>
        <span class="font-black uppercase tracking-tighter text-2xl">Arsenal Atualizado!</span>
    </div>

    <!-- Modal de Checkout -->
    <div id="checkout-modal" class="fixed inset-0 z-[300] hidden items-center justify-center p-10 bg-slate-950/98 backdrop-blur-3xl">
        <div class="bg-slate-900 w-full max-w-6xl rounded-[5rem] border border-slate-800 max-h-[90vh] overflow-y-auto custom-scrollbar shadow-2xl">
            <div class="p-20">
                <div class="flex justify-between items-center mb-20">
                    <h2 class="text-7xl font-black uppercase tracking-tighter italic">Finalizar</h2>
                    <button onclick="hideCheckout()" class="w-24 h-24 flex items-center justify-center bg-slate-800 text-slate-400 hover:text-white rounded-[2.5rem] transition"><i class="fas fa-times text-4xl"></i></button>
                </div>
                <form id="checkout-form" onsubmit="handleOrder(event)" class="space-y-16">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-16">
                        <div>
                            <label class="block text-slate-500 text-lg font-black mb-6 uppercase tracking-widest">Nome Completo</label>
                            <input type="text" required name="nome" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2.5rem] p-8 focus:border-blue-500 outline-none text-white text-3xl font-black">
                        </div>
                        <div>
                            <label class="block text-slate-500 text-lg font-black mb-6 uppercase tracking-widest">Documento</label>
                            <input type="text" required name="cpf" placeholder="000.000.000-00" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2.5rem] p-8 focus:border-blue-500 outline-none text-white text-3xl font-black">
                        </div>
                    </div>
                    <div>
                        <label class="block text-slate-500 text-lg font-black mb-6 uppercase tracking-widest">Endereço de Entrega</label>
                        <textarea required name="endereco" rows="3" class="w-full bg-slate-800 border-2 border-slate-700 rounded-[2.5rem] p-8 focus:border-blue-500 outline-none text-white text-3xl font-black"></textarea>
                    </div>
                    <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-black py-12 rounded-[4rem] transition-all text-5xl uppercase italic shadow-2xl">
                        Concluir via WhatsApp
                    </button>
                </form>
            </div>
        </div>
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

        function renderProducts(filter = 'Todos') {
            const grid = document.getElementById('products-grid');
            const filteredProducts = filter === 'Todos' ? products : products.filter(p => p.category === filter);

            grid.innerHTML = filteredProducts.map((product, index) => {
                carouselStates[product.id] = 0;
                return `
                <div class="product-card bg-slate-900 border border-slate-800 rounded-[4rem] overflow-hidden flex flex-col group/card animate-up" style="animation-delay: ${index * 0.05}s">
                    <div class="carousel-container bg-black">
                        <div class="carousel-track" id="track-${product.id}">
                            ${product.images.map(img => `<div class="carousel-item"><img src="${img}" alt="${product.name}"></div>`).join('')}
                        </div>
                        ${product.images.length > 1 ? `
                            <button class="carousel-btn btn-prev" onclick="moveSlide(${product.id}, -1)"><i class="fas fa-chevron-left"></i></button>
                            <button class="carousel-btn btn-next" onclick="moveSlide(${product.id}, 1)"><i class="fas fa-chevron-right"></i></button>
                        ` : ''}
                    </div>
                    <div class="p-14 flex flex-col flex-1">
                        <h3 class="text-5xl font-black mb-8 tracking-tighter italic uppercase group-hover/card:text-blue-400 transition-colors">${product.name}</h3>
                        <div class="mt-auto flex justify-between items-center">
                            <span class="text-white font-black text-5xl italic tracking-tighter">R$ ${product.price.toFixed(2)}</span>
                            <button onclick="addToCart(${product.id})" class="bg-blue-600 hover:bg-white hover:text-blue-600 text-white w-24 h-24 rounded-[2.5rem] flex items-center justify-center transition-all shadow-xl"><i class="fas fa-cart-plus text-4xl"></i></button>
                        </div>
                    </div>
                </div>`;
            }).join('');
        }

        window.moveSlide = function(productId, direction) {
            const product = products.find(p => p.id === productId);
            const total = product.images.length;
            let current = (carouselStates[productId] + direction + total) % total;
            carouselStates[productId] = current;
            document.getElementById(`track-${productId}`).style.transform = `translateX(-${current * 100}%)`;
        };

        window.filterByCategory = function(category) {
            document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            renderProducts(category);
        };

        function toggleCart() { document.getElementById('cart-drawer').classList.toggle('translate-x-full'); }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existing = cart.find(item => item.id === productId);
            existing ? existing.quantity++ : cart.push({ ...product, quantity: 1 });
            updateCart();
            const toast = document.getElementById('add-toast');
            toast.classList.remove('translate-y-[300%]');
            setTimeout(() => toast.classList.add('translate-y-[300%]'), 3000);
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCart();
        }

        function updateCart() {
            const list = document.getElementById('cart-items-list');
            const totalItems = cart.reduce((acc, item) => acc + item.quantity, 0);
            document.getElementById('cart-count').innerText = totalItems;
            document.getElementById('cart-count').classList.toggle('hidden', totalItems === 0);
            
            if (cart.length === 0) {
                document.getElementById('empty-cart-msg').classList.remove('hidden');
                document.getElementById('cart-footer').classList.add('hidden');
                list.innerHTML = '';
            } else {
                document.getElementById('empty-cart-msg').classList.add('hidden');
                document.getElementById('cart-footer').classList.remove('hidden');
                list.innerHTML = cart.map(item => `
                    <div class="flex items-center justify-between bg-slate-950 p-10 rounded-[3rem] border border-slate-800">
                        <div class="flex items-center space-x-8">
                            <img src="${item.images[0]}" class="w-32 h-32 rounded-3xl object-cover">
                            <div>
                                <h4 class="font-black text-3xl italic uppercase tracking-tighter">${item.name}</h4>
                                <span class="text-blue-500 font-black text-2xl italic">${item.quantity}x R$ ${item.price.toFixed(2)}</span>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="text-slate-600 hover:text-red-500 text-3xl"><i class="fas fa-trash-alt"></i></button>
                    </div>`).join('');
            }
            const total = cart.reduce((acc, item) => acc + (item.price * item.quantity), 0);
            document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
        }

        function showCheckout() { document.getElementById('checkout-modal').classList.replace('hidden', 'flex'); }
        function hideCheckout() { document.getElementById('checkout-modal').classList.replace('flex', 'hidden'); }

        function handleOrder(e) {
            e.preventDefault();
            const cliente = Object.fromEntries(new FormData(e.target));
            let message = `🛸 *CROWLAB3D - NOVO PEDIDO*\n\n*CLIENTE:* ${cliente.nome}\n*DOC:* ${cliente.cpf}\n*ENDEREÇO:* ${cliente.endereco}\n\n*ITENS:*\n`;
            cart.forEach(item => message += `▪️ ${item.quantity}x ${item.name}\n`);
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
        }

        function customOrder() {
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=Olá! Gostaria de um orçamento personalizado.`, '_blank');
        }

        document.addEventListener('DOMContentLoaded', () => renderProducts());
    </script>
</body>
</html>
