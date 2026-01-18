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
            /* Cursor alterado para o padrão (default) */
            cursor: default;
        }

        /* Garantir que elementos clicáveis mostrem a "mãozinha" */
        a, button, [onclick], .payment-option, .dot {
            cursor: pointer;
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

        .main-container {
            width: 100%;
            max-width: 1280px;
            margin: 0 auto;
            padding: 0 1.5rem;
        }

        .viewport-section {
            width: 100%;
            min-height: 80vh;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .bg-grid {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: linear-gradient(to right, #1e293b 1px, transparent 1px),
                              linear-gradient(to bottom, #1e293b 1px, transparent 1px);
            background-size: 40px 40px;
            mask-image: radial-gradient(circle at center, black, transparent 80%);
            opacity: 0.1;
            pointer-events: none;
            z-index: 0;
        }

        .hero-title {
            font-size: clamp(3rem, 10vw, 10rem);
            line-height: 0.8;
            letter-spacing: -0.05em;
            font-weight: 700;
            text-transform: uppercase;
            position: relative;
            z-index: 2;
            text-align: center;
        }

        .carousel-container {
            position: relative;
            width: 100%;
            aspect-ratio: 1/1;
            overflow: hidden;
            background: #000;
        }

        .carousel-track {
            display: flex;
            transition: transform 0.6s cubic-bezier(0.19, 1, 0.22, 1);
            height: 100%;
        }

        .carousel-slide {
            min-width: 100%;
            height: 100%;
        }

        .carousel-slide img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0.8;
            transition: 0.5s;
        }

        .complex-card:hover .carousel-slide img {
            opacity: 1;
        }

        .carousel-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(0,0,0,0.7);
            color: white;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 20;
            opacity: 0;
            transition: 0.3s;
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 4px;
        }

        .complex-card:hover .carousel-btn {
            opacity: 1;
        }

        .social-float {
            position: fixed;
            right: 1.5rem;
            bottom: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
            z-index: 1000;
        }

        .float-btn {
            width: 55px;
            height: 55px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.4rem;
            color: white;
            box-shadow: 0 10px 25px rgba(0,0,0,0.4);
            transition: 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-decoration: none;
        }

        .float-btn:hover {
            transform: scale(1.1) translateY(-5px);
        }

        .btn-wa { background: #25d366; }
        .btn-ig { background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); }

        #arsenal-grid {
            display: grid;
            gap: 1.5rem;
            grid-template-columns: repeat(1, 1fr);
            padding: 3rem 0;
        }

        @media (min-width: 640px) { #arsenal-grid { grid-template-columns: repeat(2, 1fr); } }
        @media (min-width: 1024px) { #arsenal-grid { grid-template-columns: repeat(3, 1fr); } }

        .filter-item {
            padding: 1.5rem 1rem;
            text-align: center;
            border-right: 1px solid #1e293b;
            text-transform: uppercase;
            font-weight: 700;
            letter-spacing: 0.2em;
            font-size: 0.65rem;
            transition: 0.3s;
            background: transparent;
            color: #64748b;
            white-space: nowrap;
        }

        .filter-item.active {
            background: var(--neon-blue);
            color: white;
        }

        .no-scrollbar::-webkit-scrollbar { display: none; }

        .checkout-input {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(255,255,255,0.1);
            color: white;
            padding: 1rem;
            width: 100%;
            border-radius: 4px;
            margin-bottom: 1rem;
            outline: none;
            transition: 0.3s;
        }

        .checkout-input:focus {
            border-color: var(--neon-blue);
            background: rgba(255,255,255,0.08);
        }

        .payment-option {
            flex: 1;
            border: 1px solid rgba(255,255,255,0.1);
            padding: 1rem;
            text-align: center;
            transition: 0.3s;
            background: rgba(255,255,255,0.03);
            border-radius: 4px;
        }

        .payment-option.selected {
            border-color: var(--neon-blue);
            background: rgba(59, 130, 246, 0.1);
            color: var(--neon-blue);
        }

        .btn-custom-order {
            background: transparent;
            border: 2px solid var(--neon-blue);
            color: var(--neon-blue);
            padding: 1rem 2rem;
            font-family: 'Syncopate', sans-serif;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.1em;
            transition: 0.4s;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            margin-top: 2rem;
        }

        .btn-custom-order:hover {
            background: var(--neon-blue);
            color: white;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
        }

        .carousel-dots {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 6px;
            z-index: 25;
        }

        .dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: rgba(255,255,255,0.3);
            transition: 0.3s;
        }

        .dot.active {
            background: var(--neon-blue);
            width: 12px;
            border-radius: 10px;
        }

    </style>
</head>
<body>

    <div class="bg-grid"></div>

    <div class="social-float">
        <a href="https://instagram.com/crowlab3d" target="_blank" class="float-btn btn-ig">
            <i class="fab fa-instagram"></i>
        </a>
        <a href="javascript:void(0)" onclick="customOrder()" class="float-btn btn-wa">
            <i class="fab fa-whatsapp"></i>
        </a>
    </div>

    <nav class="fixed top-0 left-0 w-full z-[100] border-b border-white/5 bg-black/90 backdrop-blur-md">
        <div class="main-container flex justify-between items-center h-20">
            <div class="flex items-center space-x-6">
                <div class="w-10 h-10 border border-blue-500/50 flex items-center justify-center rotate-45 bg-blue-600/5">
                    <i class="fas fa-microchip -rotate-45 text-blue-500 text-lg"></i>
                </div>
                <div class="flex flex-col">
                    <span class="text-2xl font-black italic font-sync">CROWLAB<span class="text-blue-600">3D</span></span>
                </div>
            </div>

            <div class="flex items-center space-x-8">
                <button onclick="toggleCart()" class="relative flex items-center">
                    <div class="text-right mr-4 hidden sm:block">
                        <div id="cart-total-top" class="text-sm font-bold italic font-sync">R$ 0,00</div>
                    </div>
                    <div class="w-12 h-12 bg-blue-600 flex items-center justify-center hover:bg-white hover:text-blue-600 transition-all">
                        <i class="fas fa-shopping-cart"></i>
                        <span id="cart-badge" class="absolute -top-1 -right-1 bg-white text-blue-600 w-5 h-5 text-[10px] font-black flex items-center justify-center rounded-full hidden">0</span>
                    </div>
                </button>
            </div>
        </div>
    </nav>

    <section class="viewport-section pt-32">
        <div class="main-container text-center">
            <div class="mb-16">
                <h1 class="hero-title italic">CROWLAB</h1>
                <h1 class="hero-title italic text-blue-600" style="margin-top: -10px;">SYSTEMS</h1>
                <p class="text-slate-500 text-[10px] uppercase tracking-[0.6em] mt-8">Manufatura de Alta Performance</p>
                
                <button onclick="customOrder()" class="btn-custom-order">
                    <i class="fab fa-whatsapp text-xl"></i>
                    Projeto Personalizado
                </button>
            </div>

            <div class="grid grid-cols-2 lg:grid-cols-4 border border-white/5 bg-zinc-900/40 rounded-lg overflow-hidden">
                <div class="p-6 border-r border-b lg:border-b-0 border-white/5 text-center">
                    <span class="text-blue-500 text-[9px] font-bold uppercase tracking-widest">Precisão</span>
                    <h4 class="text-lg font-bold font-sync italic mt-1">20 MICRONS</h4>
                </div>
                <div class="p-6 border-r border-b lg:border-b-0 border-white/5 text-center">
                    <span class="text-blue-500 text-[9px] font-bold uppercase tracking-widest">Capacidade</span>
                    <h4 class="text-lg font-bold font-sync italic mt-1">INDUSTRIAL</h4>
                </div>
                <div class="p-6 border-r border-white/5 text-center">
                    <span class="text-blue-500 text-[9px] font-bold uppercase tracking-widest">SLA/FDM</span>
                    <h4 class="text-lg font-bold font-sync italic mt-1">HYBRID</h4>
                </div>
                <div class="p-6 text-center">
                    <span class="text-blue-500 text-[9px] font-bold uppercase tracking-widest">Entrega</span>
                    <h4 class="text-lg font-bold font-sync italic mt-1">LOGÍSTICA</h4>
                </div>
            </div>
        </div>
    </section>

    <div class="sticky top-20 z-50 bg-black/95 border-y border-white/5">
        <div class="main-container">
            <div class="flex overflow-x-auto no-scrollbar">
                <button onclick="applyFilter('Todos', this)" class="filter-item active">Todos</button>
                <button onclick="applyFilter('Colecionável', this)" class="filter-item">Colecionáveis</button>
                <button onclick="applyFilter('Setup Gaming', this)" class="filter-item">Setup Gaming</button>
                <button onclick="applyFilter('Decoração', this)" class="filter-item">Decoração</button>
                <button onclick="applyFilter('Técnico', this)" class="filter-item">Peças Técnicas</button>
            </div>
        </div>
    </div>

    <section class="py-12">
        <div class="main-container">
            <div id="arsenal-grid"></div>
        </div>
    </section>

    <footer class="bg-black border-t border-white/5 py-20">
        <div class="main-container text-center">
            <h2 class="text-4xl font-black italic font-sync uppercase mb-6">CROWLAB<span class="text-blue-600">3D</span></h2>
            <p class="text-slate-600 text-xs tracking-widest uppercase mb-10">Tecnologia em constante evolução.</p>
            
            <div class="flex flex-col sm:flex-row justify-center items-center gap-6 mb-12">
                <button onclick="customOrder()" class="bg-zinc-900 border border-white/10 px-8 py-4 text-[10px] font-bold uppercase tracking-[0.3em] hover:bg-white hover:text-black transition-all">
                    Solicitar Orçamento Personalizado
                </button>
            </div>

            <div class="pt-8 border-t border-white/5">
                <p class="text-[10px] text-slate-700 font-sync uppercase tracking-[0.4em]">
                    © 2025 – Criado por CLX
                </p>
            </div>
        </div>
    </footer>

    <div id="cart-panel" class="fixed top-0 right-[-100%] w-full max-w-[500px] h-screen bg-zinc-950 z-[2000] transition-all duration-500 border-l border-white/10 flex flex-col">
        <div class="p-8 border-b border-white/10 flex justify-between items-center bg-black">
            <h2 id="panel-title" class="text-xl font-bold font-sync italic">CARRINHO</h2>
            <button onclick="toggleCart()" class="text-white hover:text-red-500 transition-colors">
                <i class="fas fa-times text-xl"></i>
            </button>
        </div>

        <div id="view-cart" class="flex-1 flex flex-col overflow-hidden">
            <div id="cart-items" class="flex-1 overflow-y-auto p-8 space-y-6"></div>
            <div class="p-8 border-t border-white/10 bg-black">
                <div class="flex justify-between items-center mb-6">
                    <span class="text-xs uppercase font-bold text-slate-500">Subtotal</span>
                    <span id="cart-grand-total" class="text-3xl font-black font-sync italic text-blue-600">R$ 0,00</span>
                </div>
                <button onclick="goToCheckout()" id="btn-next-step" class="w-full bg-blue-600 py-6 text-white font-sync font-black uppercase italic hover:bg-white hover:text-blue-600 transition-all disabled:opacity-50 disabled:pointer-events-none">Prosseguir para Dados</button>
            </div>
        </div>

        <div id="view-checkout" class="flex-1 flex flex-col overflow-hidden hidden">
            <div class="flex-1 overflow-y-auto p-8 space-y-4">
                <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest">Identificação</label>
                <input type="text" id="user-name" placeholder="Nome Completo" class="checkout-input">
                <input type="text" id="user-cpf" placeholder="CPF" class="checkout-input">
                
                <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest mt-4 block">Logística de Entrega</label>
                <textarea id="user-address" placeholder="Endereço Completo (Rua, Nº, Complemento, Bairro, Cidade, CEP)" class="checkout-input h-32"></textarea>
                
                <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest mt-4 block">Método de Pagamento</label>
                <div class="flex gap-4">
                    <div id="pay-pix" onclick="selectPayment('Pix')" class="payment-option font-bold">PIX</div>
                    <div id="pay-boleto" onclick="selectPayment('Boleto')" class="payment-option font-bold">BOLETO</div>
                </div>
            </div>
            <div class="p-8 border-t border-white/10 bg-black">
                <div class="flex gap-4">
                    <button onclick="backToCart()" class="flex-1 border border-white/10 py-6 text-white font-sync font-bold uppercase italic text-xs">Voltar</button>
                    <button onclick="processOrder()" class="flex-[2] bg-blue-600 py-6 text-white font-sync font-black uppercase italic hover:bg-white hover:text-blue-600 transition-all">Finalizar no WhatsApp</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const WHATSAPP_NUMBER = "5521990819172";

        const arsenalData = [
            { 
                id: 201, 
                name: "Escultura Dragon Zero", 
                price: 245.00, 
                cat: "Colecionável", 
                serial: "CL-9982", 
                imgs: [
                    "https://images.unsplash.com/photo-1542641728-6ca359b085f4?q=80&w=600&h=600&auto=format&fit=crop", 
                    "https://images.unsplash.com/photo-1518709268805-4e9042af9f23?q=80&w=600&h=600&auto=format&fit=crop"
                ] 
            },
            { 
                id: 202, 
                name: "Vertical GPU Mount", 
                price: 129.90, 
                cat: "Setup Gaming", 
                serial: "CL-1102", 
                imgs: [
                    "https://images.unsplash.com/photo-1591488320449-011701bb6704?q=80&w=600&h=600&auto=format&fit=crop",
                    "https://images.unsplash.com/photo-1587202372775-e229f172b9d7?q=80&w=600&h=600&auto=format&fit=crop"
                ] 
            },
            { 
                id: 203, 
                name: "Node Light Hex", 
                price: 185.00, 
                cat: "Decoração", 
                serial: "CL-5541", 
                imgs: ["https://images.unsplash.com/photo-1534073828943-f801091bb18c?q=80&w=600&h=600&auto=format&fit=crop"] 
            },
            { 
                id: 204, 
                name: "Carenagem Industrial", 
                price: 340.00, 
                cat: "Técnico", 
                serial: "CL-0029", 
                imgs: [
                    "https://images.unsplash.com/photo-1581092160562-40aa08e78837?q=80&w=600&h=600&auto=format&fit=crop",
                    "https://images.unsplash.com/photo-1504917595217-d4dc5ebe6122?q=80&w=600&h=600&auto=format&fit=crop"
                ] 
            },
            { 
                id: 205, 
                name: "Headset Cradle V3", 
                price: 95.00, 
                cat: "Setup Gaming", 
                serial: "CL-4432", 
                imgs: ["https://images.unsplash.com/photo-1505740420928-5e560c06d30e?q=80&w=600&h=600&auto=format&fit=crop"] 
            },
            { 
                id: 206, 
                name: "Busto Mecha-Unit", 
                price: 210.00, 
                cat: "Colecionável", 
                serial: "CL-8871", 
                imgs: ["https://images.unsplash.com/photo-1563089145-599997674d42?q=80&w=600&h=600&auto=format&fit=crop"] 
            }
        ];

        let cart = [];
        let selectedPayment = '';
        const carouselPositions = {};

        function renderArsenal(filter = 'Todos') {
            const grid = document.getElementById('arsenal-grid');
            const data = filter === 'Todos' ? arsenalData : arsenalData.filter(p => p.cat === filter);
            
            grid.innerHTML = data.map(p => {
                if (carouselPositions[p.id] === undefined) carouselPositions[p.id] = 0;

                return `
                <div class="complex-card group border border-white/10 bg-zinc-900/20 rounded-xl overflow-hidden hover:border-blue-500/50 transition-all duration-300">
                    <div class="carousel-container" id="carousel-${p.id}">
                        <div class="carousel-track" style="transform: translateX(-${carouselPositions[p.id] * 100}%)">
                            ${p.imgs.map(img => `
                                <div class="carousel-slide">
                                    <img src="${img}" onerror="this.src='https://via.placeholder.com/600x600/020617/3b82f6?text=CROWLAB+3D'">
                                </div>`).join('')}
                        </div>
                        
                        ${p.imgs.length > 1 ? `
                            <button onclick="moveCarousel(${p.id}, -1)" class="carousel-btn left-2"><i class="fas fa-chevron-left"></i></button>
                            <button onclick="moveCarousel(${p.id}, 1)" class="carousel-btn right-2"><i class="fas fa-chevron-right"></i></button>
                            <div class="carousel-dots">
                                ${p.imgs.map((_, idx) => `<div class="dot ${idx === carouselPositions[p.id] ? 'active' : ''}"></div>`).join('')}
                            </div>
                        ` : ''}
                    </div>
                    <div class="p-6">
                        <div class="text-blue-500 text-[9px] font-bold uppercase tracking-widest mb-1 italic font-sync">${p.cat}</div>
                        <h3 class="text-lg font-bold uppercase italic font-sync mb-4 h-12 overflow-hidden">${p.name}</h3>
                        <div class="flex justify-between items-center">
                            <span class="text-xl font-black italic font-sync">R$ ${p.price.toFixed(2)}</span>
                            <button onclick="addToCart(${p.id})" class="w-10 h-10 bg-blue-600 flex items-center justify-center hover:bg-white hover:text-blue-600 transition-all rounded">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `}).join('');
        }

        function moveCarousel(id, direction) {
            const prod = arsenalData.find(p => p.id === id);
            const total = prod.imgs.length;
            carouselPositions[id] = (carouselPositions[id] + direction + total) % total;
            const track = document.querySelector(`#carousel-${id} .carousel-track`);
            if (track) track.style.transform = `translateX(-${carouselPositions[id] * 100}%)`;
            const dots = document.querySelectorAll(`#carousel-${id} .dot`);
            dots.forEach((dot, idx) => {
                dot.classList.toggle('active', idx === carouselPositions[id]);
            });
        }

        function applyFilter(cat, btn) {
            document.querySelectorAll('.filter-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderArsenal(cat);
        }

        function toggleCart() {
            const panel = document.getElementById('cart-panel');
            const isClosing = panel.style.right === '0px';
            panel.style.right = isClosing ? '-100%' : '0px';
            if(isClosing) setTimeout(backToCart, 500);
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
            document.getElementById('btn-next-step').disabled = count === 0;

            const list = document.getElementById('cart-items');
            if(cart.length === 0) {
                list.innerHTML = `<div class="text-center py-20 text-slate-600 uppercase text-xs font-bold tracking-widest">Nenhum item detectado</div>`;
                return;
            }

            list.innerHTML = cart.map(i => `
                <div class="flex gap-4 items-center bg-zinc-900/50 p-4 border border-white/5 rounded">
                    <img src="${i.imgs[0]}" onerror="this.src='https://via.placeholder.com/100x100/020617/3b82f6?text=3D'" class="w-16 h-16 object-cover rounded">
                    <div class="flex-1">
                        <h4 class="text-xs font-bold uppercase italic font-sync">${i.name}</h4>
                        <div class="text-blue-500 text-xs font-bold mt-1">R$ ${(i.qty * i.price).toFixed(2)} [x${i.qty}]</div>
                    </div>
                    <button onclick="removeFromCart(${i.id})" class="text-slate-600 hover:text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }

        function removeFromCart(id) {
            cart = cart.filter(i => i.id !== id);
            updateCartUI();
        }

        function goToCheckout() {
            document.getElementById('view-cart').classList.add('hidden');
            document.getElementById('view-checkout').classList.remove('hidden');
            document.getElementById('panel-title').innerText = 'CHECKOUT';
        }

        function backToCart() {
            document.getElementById('view-cart').classList.remove('hidden');
            document.getElementById('view-checkout').classList.add('hidden');
            document.getElementById('panel-title').innerText = 'CARRINHO';
        }

        function selectPayment(mode) {
            selectedPayment = mode;
            document.getElementById('pay-pix').classList.toggle('selected', mode === 'Pix');
            document.getElementById('pay-boleto').classList.toggle('selected', mode === 'Boleto');
        }

        function customOrder() {
            const msg = `🛸 *CROWLAB3D - SOLICITAÇÃO DE PROJETO*\n\nOlá! Gostaria de solicitar um *ORÇAMENTO PERSONALIZADO* para um projeto em 3D.`;
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(msg)}`);
        }

        function processOrder() {
            const name = document.getElementById('user-name').value.trim();
            const cpf = document.getElementById('user-cpf').value.trim();
            const address = document.getElementById('user-address').value.trim();

            if(!name || !cpf || !address || !selectedPayment) {
                alertBox("Por favor, preencha todos os campos e selecione a forma de pagamento.");
                return;
            }

            let msg = `🛸 *CROWLAB3D - NOVO PROTOCOLO DE COMPRA*\n\n`;
            msg += `👤 *CLIENTE:* ${name}\n`;
            msg += `📄 *CPF:* ${cpf}\n`;
            msg += `📍 *ENDEREÇO:* ${address}\n`;
            msg += `💳 *PAGAMENTO:* ${selectedPayment}\n\n`;
            
            msg += `📦 *ITENS DO PEDIDO:*\n`;
            cart.forEach(i => msg += `- ${i.name} [x${i.qty}] - R$ ${(i.qty * i.price).toFixed(2)}\n`);
            
            msg += `\n💰 *VALOR TOTAL: ${document.getElementById('cart-grand-total').innerText}*`;
            
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(msg)}`);
        }

        function alertBox(text) {
            const box = document.createElement('div');
            box.className = "fixed top-10 left-1/2 -translate-x-1/2 bg-red-600 text-white px-8 py-4 z-[3000] font-bold uppercase text-[10px] tracking-widest rounded shadow-2xl";
            box.innerText = text;
            document.body.appendChild(box);
            setTimeout(() => box.remove(), 3000);
        }

        document.addEventListener('DOMContentLoaded', () => renderArsenal());
    </script>
</body>
</html>
