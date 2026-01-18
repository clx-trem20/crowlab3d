<!DOCTYPE html>
<html lang="pt-br" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CROWLAB3D | Industrial Grade 3D Manufacturing</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Inter:wght@100;300;400;700;900&display=swap');

        :root {
            --neon-blue: #3b82f6;
            --deep-black: #020617;
            --slate-900: #0f172a;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            cursor: crosshair;
        }

        body {
            background-color: var(--deep-black);
            color: #f8fafc;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
        }

        h1, h2, h3, .font-sync {
            font-family: 'Syncopate', sans-serif;
        }

        /* Container padrão de tamanho normal */
        .main-container {
            max-width: 1440px;
            margin: 0 auto;
            width: 100%;
            padding: 0 2rem;
        }

        /* Layout */
        .viewport-section {
            width: 100%;
            min-height: 100vh;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        /* Background Grid */
        .bg-grid {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: linear-gradient(to right, #1e293b 1px, transparent 1px),
                              linear-gradient(to bottom, #1e293b 1px, transparent 1px);
            background-size: 50px 50px;
            mask-image: radial-gradient(circle at center, black, transparent 95%);
            opacity: 0.15;
            pointer-events: none;
            z-index: 0;
        }

        /* Hero */
        .hero-title {
            font-size: clamp(3rem, 10vw, 12rem);
            line-height: 0.8;
            letter-spacing: -0.05em;
            font-weight: 700;
            text-transform: uppercase;
            mix-blend-mode: difference;
            position: relative;
            z-index: 2;
            text-align: center;
        }

        /* Product Card */
        .complex-card {
            background: rgba(15, 23, 42, 0.4);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(59, 130, 246, 0.1);
            transition: all 0.5s cubic-bezier(0.19, 1, 0.22, 1);
            position: relative;
            overflow: hidden;
        }

        .complex-card:hover {
            background: rgba(15, 23, 42, 0.8);
            border-color: var(--neon-blue);
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5), inset 0 0 20px rgba(59, 130, 246, 0.1);
        }

        .card-image-display {
            width: 100%;
            aspect-ratio: 1/1;
            position: relative;
            overflow: hidden;
            background: #000;
        }

        .card-image-display img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0.7;
            transition: 1.2s cubic-bezier(0.19, 1, 0.22, 1);
        }

        .complex-card:hover img {
            opacity: 1;
            transform: scale(1.1);
        }

        /* Filtros */
        .filter-nav {
            display: flex;
            width: 100%;
            background: #020617;
            border-top: 1px solid #1e293b;
            border-bottom: 1px solid #1e293b;
            overflow-x: auto;
            scrollbar-width: none;
        }

        .filter-item {
            flex: 1;
            min-width: 180px;
            padding: 1.5rem 1rem;
            text-align: center;
            border-right: 1px solid #1e293b;
            text-transform: uppercase;
            font-weight: 700;
            letter-spacing: 0.2em;
            font-size: 0.65rem;
            transition: 0.3s;
            color: #475569;
        }

        .filter-item:hover {
            color: #fff;
            background: rgba(59, 130, 246, 0.05);
        }

        .filter-item.active {
            background: var(--neon-blue);
            color: white;
        }

        /* Carrinho Lateral */
        #cart-panel {
            position: fixed;
            top: 0;
            right: -100%;
            width: 100%;
            max-width: 500px;
            height: 100vh;
            background: #020617;
            z-index: 2000;
            transition: 0.5s cubic-bezier(0.85, 0, 0.15, 1);
            border-left: 1px solid #1e293b;
            display: flex;
            flex-direction: column;
        }

        #cart-panel.active {
            right: 0;
        }

        /* Scanline */
        @keyframes scan {
            0% { transform: translateY(-100%); }
            100% { transform: translateY(300%); }
        }

        .scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 50px;
            background: linear-gradient(to bottom, transparent, rgba(59, 130, 246, 0.1), transparent);
            z-index: 5;
            animation: scan 4s linear infinite;
            pointer-events: none;
        }

        #arsenal-grid {
            display: grid;
            gap: 1.5rem;
            grid-template-columns: repeat(1, 1fr);
            padding: 3rem 0;
        }

        @media (min-width: 640px) { #arsenal-grid { grid-template-columns: repeat(2, 1fr); } }
        @media (min-width: 1024px) { #arsenal-grid { grid-template-columns: repeat(3, 1fr); } }
        @media (min-width: 1280px) { #arsenal-grid { grid-template-columns: repeat(4, 1fr); } }

    </style>
</head>
<body>

    <div class="bg-grid"></div>

    <!-- Navegação -->
    <nav class="fixed top-0 left-0 w-full z-[100] border-b border-white/5 bg-black/80 backdrop-blur-xl">
        <div class="main-container flex justify-between items-center h-24">
            <div class="flex items-center space-x-6">
                <div class="w-12 h-12 border border-blue-600 flex items-center justify-center rotate-45 hover:rotate-[225deg] transition-all duration-700 bg-blue-600/10">
                    <i class="fas fa-microchip -rotate-45 text-blue-500 text-xl"></i>
                </div>
                <div class="flex flex-col">
                    <span class="text-2xl font-black tracking-tighter italic font-sync leading-none">CROWLAB<span class="text-blue-600">3D</span></span>
                    <span class="text-[7px] uppercase tracking-[0.8em] text-slate-500 mt-1 font-bold">Industrial Manufacturing</span>
                </div>
            </div>

            <div class="flex items-center space-x-12">
                <div class="hidden md:flex items-center space-x-8 text-[9px] font-bold uppercase tracking-widest text-slate-400">
                    <a href="#arsenal" class="hover:text-blue-500 transition-colors">Produtos</a>
                    <a href="#" class="hover:text-blue-500 transition-colors">Serviços</a>
                </div>
                
                <button onclick="toggleCart()" class="relative flex items-center group">
                    <div class="text-right mr-4 hidden sm:block">
                        <div class="text-[7px] font-bold text-slate-500 uppercase tracking-widest mb-1">Operação</div>
                        <div id="cart-total-top" class="text-lg font-bold font-sync">R$ 0,00</div>
                    </div>
                    <div class="w-14 h-14 bg-blue-600 flex items-center justify-center group-hover:bg-white group-hover:text-blue-600 transition-all rounded-sm shadow-lg shadow-blue-600/20">
                        <i class="fas fa-shopping-basket text-lg"></i>
                        <span id="cart-badge" class="absolute -top-1 -right-1 bg-white text-blue-600 w-6 h-6 text-[10px] font-black flex items-center justify-center rounded-full border-2 border-black hidden">0</span>
                    </div>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="viewport-section pt-24">
        <div class="main-container relative">
            <div class="absolute top-0 left-0 w-full h-full pointer-events-none -z-10 flex items-center justify-center opacity-5">
                <i class="fas fa-cube text-[40vw]"></i>
            </div>

            <div class="text-center">
                <h1 class="hero-title italic">CROWLAB</h1>
                <h1 class="hero-title italic text-blue-600" style="margin-top: -2vw;">3D CORE</h1>
                <p class="text-slate-500 text-sm md:text-lg max-w-2xl mx-auto mt-8 font-medium uppercase tracking-[0.3em] leading-relaxed">
                    Sistemas de precisão para manufatura aditiva e engenharia de polímeros avançados.
                </p>
            </div>

            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-20">
                <div class="p-8 border border-white/5 bg-white/5 backdrop-blur-sm rounded-lg text-center">
                    <div class="text-blue-500 text-xs font-bold uppercase tracking-widest mb-2">Status</div>
                    <div class="text-xl font-bold font-sync uppercase italic">Online</div>
                </div>
                <div class="p-8 border border-white/5 bg-white/5 backdrop-blur-sm rounded-lg text-center">
                    <div class="text-blue-500 text-xs font-bold uppercase tracking-widest mb-2">Resolução</div>
                    <div class="text-xl font-bold font-sync uppercase italic">20μm</div>
                </div>
                <div class="p-8 border border-white/5 bg-white/5 backdrop-blur-sm rounded-lg text-center">
                    <div class="text-blue-500 text-xs font-bold uppercase tracking-widest mb-2">Entrega</div>
                    <div class="text-xl font-bold font-sync uppercase italic">Brasil</div>
                </div>
                <div class="p-8 border border-white/5 bg-white/5 backdrop-blur-sm rounded-lg text-center">
                    <div class="text-blue-500 text-xs font-bold uppercase tracking-widest mb-2">Material</div>
                    <div class="text-xl font-bold font-sync uppercase italic">PETG+CF</div>
                </div>
            </div>
        </div>
    </section>

    <!-- Filtros Centralizados -->
    <div class="sticky top-24 z-50 bg-black/90 backdrop-blur-md">
        <div class="main-container">
            <div class="filter-nav">
                <button onclick="applyFilter('Todos', this)" class="filter-item active">Todos</button>
                <button onclick="applyFilter('Colecionável', this)" class="filter-item">Colecionáveis</button>
                <button onclick="applyFilter('Setup Gaming', this)" class="filter-item">Gaming</button>
                <button onclick="applyFilter('Decoração', this)" class="filter-item">Design</button>
                <button onclick="applyFilter('Técnico', this)" class="filter-item">Técnico</button>
            </div>
        </div>
    </div>

    <!-- Grid de Produtos -->
    <section id="arsenal" class="py-20">
        <div class="main-container">
            <div id="arsenal-grid">
                <!-- Render dinâmico -->
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-black border-t border-white/5 py-24">
        <div class="main-container">
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-16">
                <div class="col-span-1 lg:col-span-2">
                    <h2 class="text-4xl font-black italic font-sync mb-8">CROWLAB<span class="text-blue-600">3D</span></h2>
                    <p class="text-slate-500 max-w-md leading-loose uppercase text-xs tracking-wider">
                        Especialistas em transformar conceitos digitais em realidade física através de tecnologias de ponta em impressão 3D FDM e Resina.
                    </p>
                </div>
                <div>
                    <h5 class="text-white font-bold uppercase text-xs tracking-widest mb-8">Links</h5>
                    <ul class="space-y-4 text-slate-500 text-xs uppercase font-bold tracking-widest">
                        <li><a href="#" class="hover:text-blue-500 transition-colors">Termos de Uso</a></li>
                        <li><a href="#" class="hover:text-blue-500 transition-colors">Envios</a></li>
                        <li><a href="#" class="hover:text-blue-500 transition-colors">Privacidade</a></li>
                    </ul>
                </div>
                <div>
                    <h5 class="text-white font-bold uppercase text-xs tracking-widest mb-8">Redes</h5>
                    <div class="flex space-x-6 text-2xl">
                        <a href="#" class="text-slate-500 hover:text-white transition-colors"><i class="fab fa-instagram"></i></a>
                        <a href="#" class="text-slate-500 hover:text-white transition-colors"><i class="fab fa-whatsapp"></i></a>
                    </div>
                </div>
            </div>
            <div class="mt-24 pt-8 border-t border-white/5 text-center text-[9px] text-slate-700 uppercase tracking-[0.5em]">
                © 2026 CrowLab3D - Sistemas de Manufatura Avançada
            </div>
        </div>
    </footer>

    <!-- Carrinho -->
    <div id="cart-panel">
        <div class="p-8 border-b border-white/10 flex justify-between items-center">
            <h2 class="text-xl font-bold uppercase italic font-sync">Carrinho</h2>
            <button onclick="toggleCart()" class="w-10 h-10 flex items-center justify-center hover:bg-white/10 transition-colors">
                <i class="fas fa-times"></i>
            </button>
        </div>
        
        <div id="cart-items" class="flex-1 overflow-y-auto p-8 space-y-6">
            <!-- Render dinâmico -->
        </div>

        <div id="cart-summary" class="p-8 border-t border-white/10 bg-white/5">
            <div class="flex justify-between items-center mb-8">
                <div class="text-[10px] font-bold text-slate-500 uppercase">Subtotal</div>
                <div id="cart-grand-total" class="text-3xl font-bold italic font-sync text-blue-500">R$ 0,00</div>
            </div>
            <button onclick="processOrder()" class="w-full bg-blue-600 hover:bg-white hover:text-blue-600 text-white font-bold py-5 rounded-sm transition-all uppercase tracking-widest text-sm italic font-sync">
                Finalizar Pedido
            </button>
        </div>
    </div>

    <script>
        const arsenalData = [
            { id: 201, name: "Dragon Zero", price: 245.00, cat: "Colecionável", serial: "CL-9982", img: "https://images.unsplash.com/photo-1590233464442-553fd340cce9?q=80&w=800" },
            { id: 202, name: "GPU Vertical", price: 129.90, cat: "Setup Gaming", serial: "CL-1102", img: "https://images.unsplash.com/photo-1587202392411-e1b212879793?q=80&w=800" },
            { id: 203, name: "Node Hex Light", price: 185.00, cat: "Decoração", serial: "CL-5541", img: "https://images.unsplash.com/photo-1534073828943-f801091bb18c?q=80&w=800" },
            { id: 204, name: "Case Industrial", price: 340.00, cat: "Técnico", serial: "CL-0029", img: "https://images.unsplash.com/photo-1537462715879-360eeb61a0ad?q=80&w=800" },
            { id: 205, name: "Headset Stand", price: 95.00, cat: "Setup Gaming", serial: "CL-4432", img: "https://images.unsplash.com/photo-1589254065878-42c9da997008?q=80&w=800" },
            { id: 206, name: "Mecha Unit", price: 210.00, cat: "Colecionável", serial: "CL-8871", img: "https://images.unsplash.com/photo-1558655146-d09347e92766?q=80&w=800" },
            { id: 207, name: "Fractal Vase", price: 115.00, cat: "Decoração", serial: "CL-3321", img: "https://images.unsplash.com/photo-1485955900006-10f4d324d411?q=80&w=800" },
            { id: 208, name: "Engrenagem Pro", price: 155.00, cat: "Técnico", serial: "CL-1092", img: "https://images.unsplash.com/photo-1581092160562-40aa08e78837?q=80&w=800" }
        ];

        let cart = [];

        function renderArsenal(filter = 'Todos') {
            const grid = document.getElementById('arsenal-grid');
            const data = filter === 'Todos' ? arsenalData : arsenalData.filter(p => p.cat === filter);
            
            grid.innerHTML = data.map(p => `
                <div class="complex-card group rounded-xl">
                    <div class="card-image-display">
                        <div class="scan-line"></div>
                        <img src="${p.img}" alt="${p.name}">
                        <div class="absolute bottom-4 left-4 z-10 bg-blue-600 px-3 py-1 text-[8px] font-black uppercase italic rounded-sm">
                            ${p.serial}
                        </div>
                    </div>
                    <div class="p-8">
                        <div class="text-blue-500 text-[9px] font-bold uppercase tracking-widest mb-3 italic">${p.cat}</div>
                        <h3 class="text-xl font-bold text-white uppercase italic tracking-tighter mb-8 group-hover:text-blue-500 transition-colors font-sync">${p.name}</h3>
                        <div class="flex justify-between items-center">
                            <div>
                                <div class="text-[8px] text-slate-600 uppercase font-bold mb-1 tracking-widest">Unidade</div>
                                <div class="text-2xl font-black italic font-sync">R$ ${p.price.toFixed(2)}</div>
                            </div>
                            <button onclick="addToCart(${p.id})" class="w-12 h-12 bg-white/5 border border-white/10 flex items-center justify-center hover:bg-blue-600 hover:scale-110 transition-all rounded-lg">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function applyFilter(cat, btn) {
            document.querySelectorAll('.filter-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderArsenal(cat);
        }

        function toggleCart() {
            document.getElementById('cart-panel').classList.toggle('active');
        }

        function addToCart(id) {
            const prod = arsenalData.find(p => p.id === id);
            const exists = cart.find(i => i.id === id);
            if(exists) exists.qty++;
            else cart.push({...prod, qty: 1});
            updateCartUI();
        }

        function updateCartUI() {
            const count = cart.reduce((a, b) => a + b.qty, 0);
            const total = cart.reduce((a, b) => a + (b.price * b.qty), 0);
            
            const badge = document.getElementById('cart-badge');
            badge.innerText = count;
            badge.classList.toggle('hidden', count === 0);
            document.getElementById('cart-total-top').innerText = `R$ ${total.toFixed(2)}`;
            document.getElementById('cart-grand-total').innerText = `R$ ${total.toFixed(2)}`;

            const list = document.getElementById('cart-items');
            if(cart.length === 0) {
                list.innerHTML = `<div class="text-center py-20 text-slate-700 uppercase font-bold text-[10px] tracking-widest">Carrinho Vazio</div>`;
                document.getElementById('cart-summary').classList.add('hidden');
            } else {
                document.getElementById('cart-summary').classList.remove('hidden');
                list.innerHTML = cart.map(i => `
                    <div class="flex gap-4 items-center bg-white/5 p-4 rounded-lg border border-white/5">
                        <div class="w-20 h-20 rounded overflow-hidden">
                             <img src="${i.img}" class="w-full h-full object-cover">
                        </div>
                        <div class="flex-1">
                            <h4 class="text-sm font-bold uppercase italic font-sync leading-none">${i.name}</h4>
                            <div class="flex justify-between items-center mt-3">
                                <span class="text-[10px] text-slate-500">QTD: ${i.qty}</span>
                                <span class="text-sm font-bold text-blue-500">R$ ${(i.qty * i.price).toFixed(2)}</span>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${i.id})" class="text-slate-600 hover:text-red-500 px-2 transition-colors">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </div>
                `).join('');
            }
        }

        function removeFromCart(id) {
            cart = cart.filter(i => i.id !== id);
            updateCartUI();
        }

        function processOrder() {
            let msg = `🛸 *CROWLAB3D - NOVO PEDIDO*\n\n`;
            cart.forEach(i => msg += `- ${i.name} [${i.serial}] x${i.qty}\n`);
            msg += `\n*TOTAL: ${document.getElementById('cart-grand-total').innerText}*`;
            window.open(`https://wa.me/5521990819172?text=${encodeURIComponent(msg)}`);
        }

        document.addEventListener('DOMContentLoaded', () => renderArsenal());
    </script>
</body>
</html>
