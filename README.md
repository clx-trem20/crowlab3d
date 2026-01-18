<!DOCTYPE html>
<html lang="pt-br" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CrowLab3D - Inovação em Impressão 3D</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            background-color: #020617;
            color: #f8fafc;
            overflow-x: hidden;
            width: 100vw; /* Garante que o corpo ocupa a largura total da viewport */
        }

        /* Removendo limites laterais para ocupar a tela toda */
        .full-width-container {
            width: 100%;
            padding-left: 2rem;
            padding-right: 2rem;
        }

        /* Hero Section ocupando 100% da largura e altura */
        .hero-section {
            width: 100%;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            background: radial-gradient(circle at center, #1e293b 0%, #020617 100%);
            overflow: hidden;
        }

        .hero-content {
            z-index: 10;
            text-align: center;
            width: 100%;
        }

        .hero-title {
            font-size: clamp(4rem, 15vw, 12rem); /* Escala gigante para telas grandes */
            font-weight: 900;
            line-height: 0.8;
            letter-spacing: -0.05em;
            text-transform: uppercase;
            background: linear-gradient(to bottom, #ffffff 40%, #3b82f6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 2rem;
        }

        /* Cards de Produto - Ajustados para preencher o grid */
        .product-card {
            background: #0f172a;
            border: 1px solid #1e293b;
            border-radius: 12px;
            overflow: hidden;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            width: 100%;
        }

        .product-card:hover {
            transform: translateY(-8px);
            border-color: #3b82f6;
            box-shadow: 0 20px 40px -10px rgba(59, 130, 246, 0.3);
        }

        .image-wrapper {
            position: relative;
            width: 100%;
            aspect-ratio: 1/1; /* Quadrado perfeito para o grid ficar uniforme */
            background: #000;
        }

        .image-wrapper img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Filtros horizontais ocupando a largura total */
        .filters-scroll {
            display: flex;
            gap: 1rem;
            overflow-x: auto;
            padding-bottom: 1rem;
            scrollbar-width: none; /* Firefox */
        }
        .filters-scroll::-webkit-scrollbar { display: none; } /* Chrome/Safari */

        .filter-btn {
            white-space: nowrap;
            padding: 0.8rem 2rem;
            border-radius: 8px;
            font-weight: 800;
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 0.1em;
            transition: all 0.3s;
            border: 1px solid #1e293b;
            background: #0f172a;
            color: #64748b;
        }

        .filter-btn.active {
            background: #3b82f6;
            color: white;
            border-color: #3b82f6;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
        }

        /* Carrinho Lateral */
        #cart-drawer {
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            max-width: 500px;
            height: 100vh;
            background: #020617;
            z-index: 1000;
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            border-left: 1px solid #1e293b;
            display: flex;
            flex-direction: column;
        }

        #cart-drawer.open {
            transform: translateX(0);
        }

        /* Navegação superior total */
        nav {
            width: 100%;
            padding: 0 2rem;
        }

        /* Footer total */
        footer {
            width: 100%;
            background: #020617;
            border-top: 1px solid #1e293b;
        }

        /* Ajuste do Grid para telas ultra-wide */
        @media (min-width: 2000px) {
            #product-grid {
                grid-template-columns: repeat(6, 1fr);
            }
        }
    </style>
</head>
<body>

    <!-- Header / Nav (Total) -->
    <nav class="fixed top-0 left-0 z-[100] bg-slate-950/90 backdrop-blur-xl border-b border-slate-800 h-24 flex items-center justify-between">
        <div class="flex items-center space-x-4">
            <i class="fas fa-cube text-blue-500 text-4xl"></i>
            <span class="text-3xl font-black tracking-tighter uppercase italic">CROWLAB<span class="text-blue-500">3D</span></span>
        </div>
        
        <div class="flex items-center space-x-10">
            <a href="#products" class="text-xs font-black uppercase tracking-[0.3em] text-slate-500 hover:text-white transition">Catálogo</a>
            <button onclick="toggleCart()" class="relative p-3 bg-slate-900 rounded-full text-slate-300 hover:text-white transition border border-slate-800">
                <i class="fas fa-shopping-bag text-xl"></i>
                <span id="cart-count" class="absolute -top-1 -right-1 bg-blue-600 text-[10px] font-black w-6 h-6 flex items-center justify-center rounded-full hidden border-2 border-slate-950">0</span>
            </button>
        </div>
    </nav>

    <!-- Hero Section (Tela Toda) -->
    <section class="hero-section">
        <div class="hero-content">
            <h1 class="hero-title italic">CROWLAB3D</h1>
            <p class="text-slate-400 text-2xl md:text-3xl font-light leading-relaxed mb-12 tracking-tight">
                Engenharia de precisão. Design sem limites.
            </p>
            <a href="#products" class="inline-block bg-blue-600 hover:bg-white hover:text-blue-600 text-white font-black py-6 px-16 rounded-sm transition-all shadow-2xl uppercase tracking-widest text-sm italic">
                Aceder ao Arsenal
            </a>
        </div>
        <!-- Background Decor -->
        <div class="absolute inset-0 pointer-events-none">
            <div class="absolute top-0 right-0 w-[50vw] h-[50vh] bg-blue-600/10 blur-[200px]"></div>
            <div class="absolute bottom-0 left-0 w-[50vw] h-[50vh] bg-purple-600/10 blur-[200px]"></div>
        </div>
    </section>

    <!-- Barra de Filtros (Tela Toda) -->
    <section class="py-8 bg-slate-950 border-b border-slate-900 sticky top-24 z-50">
        <div class="full-width-container">
            <div class="filters-scroll">
                <button onclick="filterProducts('Todos')" class="filter-btn active">Todos os Modelos</button>
                <button onclick="filterProducts('Colecionável')" class="filter-btn">Colecionáveis</button>
                <button onclick="filterProducts('Setup Gaming')" class="filter-btn">Setup Gaming</button>
                <button onclick="filterProducts('Decoração')" class="filter-btn">Decoração Interior</button>
                <button onclick="filterProducts('Técnico')" class="filter-btn">Componentes Técnicos</button>
            </div>
        </div>
    </section>

    <!-- Galeria de Produtos (Grid Total) -->
    <main id="products" class="py-12 bg-slate-950">
        <div class="full-width-container">
            <!-- Grid dinâmico que expande de acordo com a tela -->
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 2xl:grid-cols-6 gap-6" id="product-grid">
                <!-- Injetado por JS -->
            </div>
        </div>
    </main>

    <!-- Footer (Total) -->
    <footer class="py-24">
        <div class="full-width-container">
            <div class="flex flex-col lg:flex-row justify-between items-start gap-16">
                <div class="lg:w-1/3">
                    <div class="flex items-center space-x-3 mb-8">
                        <i class="fas fa-cube text-blue-500 text-4xl"></i>
                        <span class="text-3xl font-black uppercase italic tracking-tighter">CROWLAB<span class="text-blue-500">3D</span></span>
                    </div>
                    <p class="text-slate-500 leading-relaxed text-lg font-medium">
                        Líderes em manufatura aditiva de alta performance. Desenvolvemos soluções em polímeros para indústrias e colecionadores exigentes.
                    </p>
                </div>
                
                <div class="grid grid-cols-2 md:grid-cols-3 gap-16 flex-1 w-full lg:w-auto">
                    <div>
                        <h4 class="text-white font-black uppercase text-xs tracking-[0.4em] mb-8 italic">Explorar</h4>
                        <ul class="text-slate-500 space-y-4 text-sm font-bold">
                            <li><a href="#" class="hover:text-blue-500 transition">Catálogo Completo</a></li>
                            <li><a href="#" class="hover:text-blue-500 transition">Projetos Custom</a></li>
                            <li><a href="#" class="hover:text-blue-500 transition">Engenharia</a></li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="text-white font-black uppercase text-xs tracking-[0.4em] mb-8 italic">Suporte</h4>
                        <ul class="text-slate-500 space-y-4 text-sm font-bold">
                            <li><a href="#" class="hover:text-blue-500 transition">Envios</a></li>
                            <li><a href="#" class="hover:text-blue-500 transition">Garantia</a></li>
                            <li><a href="#" class="hover:text-blue-500 transition">Contacto</a></li>
                        </ul>
                    </div>
                    <div class="col-span-2 md:col-span-1">
                        <h4 class="text-white font-black uppercase text-xs tracking-[0.4em] mb-8 italic">Siga-nos</h4>
                        <div class="flex space-x-6 text-2xl">
                            <a href="#" class="text-slate-600 hover:text-white transition"><i class="fab fa-instagram"></i></a>
                            <a href="#" class="text-slate-600 hover:text-white transition"><i class="fab fa-whatsapp"></i></a>
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-24 pt-12 border-t border-slate-900 flex justify-between items-center text-slate-600 text-[10px] font-black uppercase tracking-[0.5em]">
                <span>© 2026 CrowLab3D Digital Arsenal</span>
                <span>Brasil / Global Shipping</span>
            </div>
        </div>
    </footer>

    <!-- Carrinho Lateral -->
    <div id="cart-drawer">
        <div class="p-10 border-b border-slate-900 flex justify-between items-center bg-slate-950">
            <h2 class="text-2xl font-black uppercase italic tracking-tighter">O Teu Arsenal</h2>
            <button onclick="toggleCart()" class="text-slate-500 hover:text-white transition"><i class="fas fa-times text-2xl"></i></button>
        </div>
        <div id="cart-content" class="flex-1 overflow-y-auto p-10 space-y-8 custom-scrollbar">
            <!-- Itens do carrinho -->
        </div>
        <div id="cart-footer" class="p-10 border-t border-slate-900 bg-slate-950 hidden">
            <div class="flex justify-between items-end mb-8">
                <span class="text-slate-500 font-black text-xs uppercase tracking-widest">Valor Total</span>
                <span id="cart-total" class="text-4xl font-black text-blue-500 tracking-tighter italic">R$ 0,00</span>
            </div>
            <button onclick="checkout()" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-black py-6 rounded-sm transition uppercase tracking-[0.2em] text-sm italic">
                Finalizar via WhatsApp
            </button>
        </div>
    </div>

    <!-- Floating Buttons -->
    <div class="fixed bottom-10 right-10 z-[100] flex flex-col gap-4">
        <a href="https://instagram.com/crowlab3d" target="_blank" class="w-16 h-16 bg-slate-900 border border-slate-800 rounded-full flex items-center justify-center text-2xl text-white shadow-2xl hover:bg-blue-600 transition-all"><i class="fab fa-instagram"></i></a>
        <button onclick="whatsappBudget()" class="w-16 h-16 bg-green-600 rounded-full flex items-center justify-center text-2xl text-white shadow-2xl hover:scale-110 transition-all"><i class="fab fa-whatsapp"></i></button>
    </div>

    <script>
        const products = [
            { id: 1, name: "Escultura Dragon V2", price: 185.00, cat: "Colecionável", imgs: ["https://images.unsplash.com/photo-1590233464442-553fd340cce9?q=80&w=800"] },
            { id: 2, name: "Suporte GPU Carbon", price: 89.90, cat: "Setup Gaming", imgs: ["https://images.unsplash.com/photo-1587202392411-e1b212879793?q=80&w=800"] },
            { id: 3, name: "Luminária Hexa-Cell", price: 145.00, cat: "Decoração", imgs: ["https://images.unsplash.com/photo-1534073828943-f801091bb18c?q=80&w=800"] },
            { id: 4, name: "Kit Mecânico V8", price: 210.00, cat: "Técnico", imgs: ["https://images.unsplash.com/photo-1537462715879-360eeb61a0ad?q=80&w=800"] },
            { id: 5, name: "Organizador Linear Pro", price: 55.00, cat: "Setup Gaming", imgs: ["https://images.unsplash.com/photo-1589254065878-42c9da997008?q=80&w=800"] },
            { id: 6, name: "Busto Cyber-Warrior", price: 120.00, cat: "Colecionável", imgs: ["https://images.unsplash.com/photo-1558655146-d09347e92766?q=80&w=800"] },
            { id: 7, name: "Vaso Low-Poly", price: 42.00, cat: "Decoração", imgs: ["https://images.unsplash.com/photo-1485955900006-10f4d324d411?q=80&w=800"] },
            { id: 8, name: "Peça Industrial Nylon", price: 75.00, cat: "Técnico", imgs: ["https://images.unsplash.com/photo-1581092160562-40aa08e78837?q=80&w=800"] },
            { id: 9, name: "Suporte Headset Stealth", price: 68.00, cat: "Setup Gaming", imgs: ["https://images.unsplash.com/photo-1594633312681-425c7b97ccd1?q=80&w=800"] },
            { id: 10, name: "Painel Acústico 3D", price: 299.00, cat: "Decoração", imgs: ["https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?q=80&w=800"] }
        ];

        let cart = [];

        function renderProducts(filter = 'Todos') {
            const grid = document.getElementById('product-grid');
            const filtered = filter === 'Todos' ? products : products.filter(p => p.cat === filter);
            
            grid.innerHTML = filtered.map(p => `
                <div class="product-card group">
                    <div class="image-wrapper">
                        <img src="${p.imgs[0]}" alt="${p.name}">
                        <div class="absolute inset-0 bg-blue-600/20 opacity-0 group-hover:opacity-100 transition-opacity"></div>
                    </div>
                    <div class="p-8">
                        <div class="text-[9px] font-black uppercase tracking-[0.3em] text-blue-500 mb-3 italic">${p.cat}</div>
                        <h3 class="text-lg font-black text-white mb-6 leading-tight group-hover:text-blue-400 transition uppercase italic">${p.name}</h3>
                        <div class="flex justify-between items-center">
                            <span class="text-2xl font-black italic tracking-tighter">R$ ${p.price.toFixed(2)}</span>
                            <button onclick="addToCart(${p.id})" class="bg-blue-600 hover:bg-white hover:text-blue-600 text-white w-12 h-12 rounded-sm flex items-center justify-center transition-all shadow-lg">
                                <i class="fas fa-plus text-sm"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function filterProducts(cat) {
            document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            renderProducts(cat);
        }

        function toggleCart() {
            document.getElementById('cart-drawer').classList.toggle('open');
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            const item = cart.find(i => i.id === id);
            if(item) item.qty++;
            else cart.push({...product, qty: 1});
            updateCart();
            if(!document.getElementById('cart-drawer').classList.contains('open')) toggleCart();
        }

        function updateCart() {
            const count = cart.reduce((a, b) => a + b.qty, 0);
            const total = cart.reduce((a, b) => a + (b.price * b.qty), 0);
            
            const badge = document.getElementById('cart-count');
            badge.innerText = count;
            badge.classList.toggle('hidden', count === 0);

            const container = document.getElementById('cart-content');
            if(cart.length === 0) {
                container.innerHTML = `<div class="text-center py-32 text-slate-700 font-black uppercase tracking-[0.3em] italic text-xs">O Arsenal está vazio</div>`;
                document.getElementById('cart-footer').classList.add('hidden');
            } else {
                document.getElementById('cart-footer').classList.remove('hidden');
                container.innerHTML = cart.map(i => `
                    <div class="flex gap-6 items-center bg-slate-900/30 p-6 border border-slate-900 hover:border-slate-800 transition">
                        <img src="${i.imgs[0]}" class="w-20 h-20 object-cover rounded-sm">
                        <div class="flex-1">
                            <h4 class="text-sm font-black text-white uppercase italic">${i.name}</h4>
                            <div class="text-blue-500 font-black text-xs mt-2 italic">${i.qty}x R$ ${i.price.toFixed(2)}</div>
                        </div>
                        <button onclick="removeFromCart(${i.id})" class="text-slate-700 hover:text-red-500 transition"><i class="fas fa-trash-alt"></i></button>
                    </div>
                `).join('');
            }
            document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
        }

        function removeFromCart(id) {
            cart = cart.filter(i => i.id !== id);
            updateCart();
        }

        function checkout() {
            let msg = `🛸 *NOVO PEDIDO CROWLAB3D*\n\n`;
            cart.forEach(i => msg += `▪️ ${i.qty}x ${i.name} - R$ ${(i.qty*i.price).toFixed(2)}\n`);
            msg += `\n*TOTAL: ${document.getElementById('cart-total').innerText}*`;
            window.open(`https://wa.me/5521990819172?text=${encodeURIComponent(msg)}`);
        }

        function whatsappBudget() {
            window.open(`https://wa.me/5521990819172?text=Olá! Gostaria de falar sobre um projeto 3D personalizado.`);
        }

        document.addEventListener('DOMContentLoaded', () => renderProducts());
    </script>
</body>
</html>
