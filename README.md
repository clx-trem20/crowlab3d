<html lang="pt-br" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CROWLAB3D | Real-time Manufacturing</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Inter:wght@100;300;400;700;900&display=swap');

        :root {
            --neon-blue: #3b82f6;
            --deep-black: #020617;
            --slate-900: #0f172a;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; cursor: default; }
        a, button, [onclick], .payment-option, .dot { cursor: pointer; }

        body {
            background-color: var(--deep-black);
            color: #f8fafc;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
        }

        h1, h2, h3, .font-sync { font-family: 'Syncopate', sans-serif; }

        .main-container {
            width: 100%;
            max-width: 1280px;
            margin: 0 auto;
            padding: 0 1.5rem;
        }

        .bg-grid {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-image: linear-gradient(to right, #1e293b 1px, transparent 1px),
                              linear-gradient(to bottom, #1e293b 1px, transparent 1px);
            background-size: 40px 40px;
            mask-image: radial-gradient(circle at center, black, transparent 80%);
            opacity: 0.1; pointer-events: none; z-index: 0;
        }

        .hero-title {
            font-size: clamp(3rem, 10vw, 10rem);
            line-height: 0.8; letter-spacing: -0.05em; font-weight: 700;
            text-transform: uppercase; position: relative; z-index: 2; text-align: center;
        }

        /* Estrutura do Carrossel */
        .carousel-container { position: relative; width: 100%; aspect-ratio: 1/1; overflow: hidden; background: #000; }
        .carousel-track { display: flex; transition: transform 0.6s cubic-bezier(0.19, 1, 0.22, 1); height: 100%; }
        .carousel-slide { min-width: 100%; height: 100%; }
        .carousel-slide img { width: 100%; height: 100%; object-fit: cover; opacity: 0.8; transition: 0.5s; }
        .complex-card:hover .carousel-slide img { opacity: 1; }
        
        .carousel-btn {
            position: absolute; top: 50%; transform: translateY(-50%);
            width: 35px; height: 35px; background: rgba(0,0,0,0.5); border: 1px solid rgba(255,255,255,0.1);
            color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center;
            opacity: 0; transition: 0.3s; z-index: 10;
        }
        .complex-card:hover .carousel-btn { opacity: 1; }
        .carousel-btn:hover { background: var(--neon-blue); border-color: var(--neon-blue); }
        .btn-prev { left: 10px; }
        .btn-next { right: 10px; }

        .carousel-dots {
            position: absolute; bottom: 15px; left: 50%; transform: translateX(-50%);
            display: flex; gap: 6px; z-index: 10;
        }
        .dot { width: 6px; height: 6px; border-radius: 50%; background: rgba(255,255,255,0.3); transition: 0.3s; }
        .dot.active { background: var(--neon-blue); width: 18px; border-radius: 10px; }

        .social-float { position: fixed; right: 1.5rem; bottom: 1.5rem; display: flex; flex-direction: column; gap: 0.75rem; z-index: 1000; }
        .float-btn { width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.4rem; color: white; box-shadow: 0 10px 25px rgba(0,0,0,0.4); transition: 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); text-decoration: none; }
        .btn-wa { background: #25d366; }
        .btn-ig { background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); }

        #arsenal-grid { display: grid; gap: 1.5rem; grid-template-columns: repeat(1, 1fr); padding: 3rem 0; }
        @media (min-width: 640px) { #arsenal-grid { grid-template-columns: repeat(2, 1fr); } }
        @media (min-width: 1024px) { #arsenal-grid { grid-template-columns: repeat(3, 1fr); } }

        .filter-item {
            padding: 1.5rem 1rem; text-align: center; border-right: 1px solid #1e293b;
            text-transform: uppercase; font-weight: 700; letter-spacing: 0.2em; font-size: 0.65rem;
            transition: 0.3s; background: transparent; color: #64748b; white-space: nowrap;
        }
        .filter-item.active { background: var(--neon-blue); color: white; }

        .no-scrollbar::-webkit-scrollbar { display: none; }

        .checkout-input {
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
            color: white; padding: 0.75rem 1rem; width: 100%; border-radius: 4px;
            outline: none; transition: 0.3s; font-size: 0.85rem;
        }
        .checkout-input:focus { border-color: var(--neon-blue); background: rgba(255,255,255,0.08); }
        .checkout-input:disabled { opacity: 0.5; cursor: not-allowed; }

        .payment-option {
            flex: 1; border: 1px solid rgba(255,255,255,0.1); padding: 1rem;
            text-align: center; transition: 0.3s; background: rgba(255,255,255,0.03); border-radius: 4px;
        }
        .payment-option.selected { border-color: var(--neon-blue); background: rgba(59, 130, 246, 0.1); color: var(--neon-blue); }

        .shipping-badge {
            background: rgba(59, 130, 246, 0.05); border: 1px solid var(--neon-blue);
            color: var(--neon-blue); padding: 12px; font-size: 11px; font-weight: bold;
            border-radius: 4px; margin-top: 10px; display: flex; justify-content: space-between; align-items: center;
        }

        .faq-item { border-bottom: 1px solid rgba(255,255,255,0.05); }
        .faq-trigger { width: 100%; display: flex; justify-content: space-between; align-items: center; padding: 2rem 0; text-align: left; }
        .faq-content { max-height: 0; overflow: hidden; transition: max-height 0.4s ease-out; color: #94a3b8; font-size: 0.9rem; line-height: 1.6; }
        
        #connection-info { font-size: 10px; color: #3b82f6; opacity: 0.6; }
    </style>
</head>
<body>

    <div class="bg-grid"></div>

    <!-- Flutuantes de Redes Sociais -->
    <div class="social-float">
        <a href="https://instagram.com/crowlab3d" target="_blank" class="float-btn btn-ig">
            <i class="fab fa-instagram"></i>
        </a>
        <a href="javascript:void(0)" onclick="customOrder()" class="float-btn btn-wa">
            <i class="fab fa-whatsapp"></i>
        </a>
    </div>

    <!-- Navegação -->
    <nav class="fixed top-0 left-0 w-full z-[100] border-b border-white/5 bg-black/90 backdrop-blur-md">
        <div class="main-container flex justify-between items-center h-20">
            <div class="flex flex-col">
                <div class="flex items-center space-x-6">
                    <div class="w-10 h-10 border border-blue-500/50 flex items-center justify-center rotate-45 bg-blue-600/5">
                        <i class="fas fa-microchip -rotate-45 text-blue-500 text-lg"></i>
                    </div>
                    <span class="text-2xl font-black italic font-sync">CROWLAB<span class="text-blue-600">3D</span></span>
                </div>
                <div id="connection-info" class="ml-16 font-mono text-green-500 uppercase tracking-tighter">Conectado ao Arsenal</div>
            </div>
            <button onclick="toggleCart()" class="relative flex items-center">
                <div id="cart-total-top" class="text-sm font-bold italic font-sync mr-4 hidden sm:block">R$ 0,00</div>
                <div class="w-12 h-12 bg-blue-600 flex items-center justify-center hover:bg-white hover:text-blue-600 transition-all">
                    <i class="fas fa-shopping-cart"></i>
                    <span id="cart-badge" class="absolute -top-1 -right-1 bg-white text-blue-600 w-5 h-5 text-[10px] font-black flex items-center justify-center rounded-full hidden">0</span>
                </div>
            </button>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="viewport-section pt-32 pb-20">
        <div class="main-container text-center">
            <h1 class="hero-title italic">CROWLAB</h1>
            <h1 class="hero-title italic text-blue-600" style="margin-top: -10px;">SYSTEMS</h1>
            <p class="text-slate-500 text-[10px] uppercase tracking-[0.6em] mt-8">Manufatura Real em 3D</p>
        </div>
    </section>

    <!-- Filtros -->
    <div class="sticky top-20 z-50 bg-black/95 border-y border-white/5">
        <div class="main-container">
            <div class="flex overflow-x-auto no-scrollbar">
                <button onclick="applyFilter('Todos', this)" class="filter-item active">Todos</button>
                <button onclick="applyFilter('Colecionável', this)" class="filter-item">Colecionáveis</button>
                <button onclick="applyFilter('Setup Gaming', this)" class="filter-item">Setup Gaming</button>
                <button onclick="applyFilter('Decoração', this)" class="filter-item">Decoração</button>
                <button onclick="applyFilter('Técnico', this)" class="filter-item">Técnico</button>
            </div>
        </div>
    </div>

    <!-- Grid de Produtos -->
    <section class="py-12" id="arsenal">
        <div class="main-container">
            <div id="arsenal-grid">
                <!-- Produtos sincronizados via Firebase aparecerão aqui -->
                <div class="col-span-full text-center py-20">
                    <div class="inline-block w-8 h-8 border-4 border-blue-600 border-t-transparent rounded-full animate-spin"></div>
                    <p class="text-[10px] uppercase font-sync mt-4 text-slate-500">Sincronizando Arsenal...</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Info Cards -->
    <section class="py-24 border-t border-white/5 bg-zinc-950/50">
        <div class="main-container">
            <div class="grid md:grid-cols-3 gap-12">
                <div>
                    <div class="text-blue-600 mb-6 text-3xl"><i class="fas fa-layer-group"></i></div>
                    <h4 class="font-sync text-sm font-bold uppercase mb-4">Materiais Premium</h4>
                    <p class="text-slate-400 text-sm leading-relaxed">Trabalhamos com filamentos de alta performance: PLA+, PETG e TPU para durabilidade extrema em todos os projetos.</p>
                </div>
                <div>
                    <div class="text-blue-600 mb-6 text-3xl"><i class="fas fa-microchip"></i></div>
                    <h4 class="font-sync text-sm font-bold uppercase mb-4">Precisão Micron</h4>
                    <p class="text-slate-400 text-sm leading-relaxed">Impressões com calibração rigorosa. Camadas imperceptíveis e encaixes técnicos perfeitos.</p>
                </div>
                <div>
                    <div class="text-blue-600 mb-6 text-3xl"><i class="fas fa-truck-fast"></i></div>
                    <h4 class="font-sync text-sm font-bold uppercase mb-4">Produção Ágil</h4>
                    <p class="text-slate-400 text-sm leading-relaxed">Sua ideia sai da tela para a mão em tempo recorde. Enviamos para todo o território nacional.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- FAQ -->
    <section class="py-24 border-t border-white/5">
        <div class="main-container max-w-3xl">
            <h2 class="font-sync text-2xl font-black italic uppercase mb-12 text-center">Protocolos & <span class="text-blue-600">Dúvidas</span></h2>
            <div class="space-y-2">
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold uppercase text-xs tracking-widest">Qual o prazo médio de produção?</span>
                        <i class="fas fa-plus text-[10px] text-blue-600"></i>
                    </button>
                    <div class="faq-content">
                        <p class="pb-8">O prazo varia de 2 a 7 dias úteis dependendo da complexidade e volume da peça. Projetos maiores exigem um tempo maior de impressão contínua.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold uppercase text-xs tracking-widest">Vocês fazem projetos sob medida?</span>
                        <i class="fas fa-plus text-[10px] text-blue-600"></i>
                    </button>
                    <div class="faq-content">
                        <p class="pb-8">Sim! Caso não encontre o que precisa no arsenal, entre em contato via WhatsApp para um orçamento personalizado enviando seu arquivo STL ou OBJ.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold uppercase text-xs tracking-widest">As peças já vêm pintadas?</span>
                        <i class="fas fa-plus text-[10px] text-blue-600"></i>
                    </button>
                    <div class="faq-content">
                        <p class="pb-8">Nossas peças são enviadas na cor natural do filamento escolhido. Algumas peças específicas de decoração podem ter acabamento premium opcional.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Rodapé -->
    <footer class="py-20 border-t border-white/5 bg-black">
        <div class="main-container">
            <div class="flex flex-col md:flex-row justify-between items-center gap-10">
                <div class="text-center md:text-left">
                    <span class="text-2xl font-black italic font-sync">CROWLAB<span class="text-blue-600">3D</span></span>
                    <p class="text-slate-600 text-[9px] uppercase tracking-widest mt-2">© 2025 – Criado por CLX</p>
                </div>
                <div class="flex gap-8 text-slate-400">
                    <a href="https://instagram.com/crowlab3d" class="hover:text-blue-600 transition-colors"><i class="fab fa-instagram text-xl"></i></a>
                    <a href="javascript:void(0)" onclick="customOrder()" class="hover:text-blue-600 transition-colors"><i class="fab fa-whatsapp text-xl"></i></a>
                    <a href="#" class="hover:text-blue-600 transition-colors"><i class="fab fa-tiktok text-xl"></i></a>
                </div>
            </div>
        </div>
    </footer>

    <!-- Painel Lateral do Carrinho -->
    <div id="cart-panel" class="fixed top-0 right-[-100%] w-full max-w-[500px] h-screen bg-zinc-950 z-[2000] transition-all duration-500 border-l border-white/10 flex flex-col">
        <div class="p-8 border-b border-white/10 flex justify-between items-center bg-black">
            <h2 id="panel-title" class="text-xl font-bold font-sync italic uppercase">Carrinho</h2>
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
                <button onclick="goToCheckout()" id="btn-next-step" class="w-full bg-blue-600 py-6 text-white font-sync font-black uppercase italic hover:bg-white hover:text-blue-600 transition-all disabled:opacity-50">Prosseguir para Entrega</button>
            </div>
        </div>

        <div id="view-checkout" class="flex-1 flex flex-col overflow-hidden hidden">
            <div class="flex-1 overflow-y-auto p-8 space-y-6">
                <div>
                    <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest block mb-2">Dados Pessoais</label>
                    <div class="space-y-3">
                        <input type="text" id="user-name" placeholder="Nome Completo" class="checkout-input">
                        <input type="text" id="user-cpf" placeholder="CPF (Apenas números)" class="checkout-input" maxlength="11" inputmode="numeric" oninput="onlyNumbers(this)">
                    </div>
                </div>

                <div>
                    <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest block mb-2">Endereço de Entrega</label>
                    <div class="grid grid-cols-2 gap-3">
                        <input type="text" id="user-cep" placeholder="CEP (Apenas números)" class="checkout-input" maxlength="8" inputmode="numeric" oninput="onlyNumbers(this)" onblur="fetchAddress(this.value)">
                        <input type="text" id="user-city" placeholder="Cidade / UF" class="checkout-input">
                        <input type="text" id="user-street" placeholder="Rua / Logradouro" class="checkout-input col-span-2">
                        <input type="text" id="user-num" placeholder="Nº (Apenas números)" class="checkout-input" inputmode="numeric" oninput="onlyNumbers(this)">
                        <input type="text" id="user-neighborhood" placeholder="Bairro" class="checkout-input">
                        <input type="text" id="user-complement" placeholder="Complemento (Opcional)" class="checkout-input col-span-2">
                    </div>
                    
                    <div id="shipping-area" class="mt-4 hidden">
                        <div class="shipping-badge">
                            <span>📦 Frete calculado</span>
                            <span id="shipping-value" class="font-black">R$ 0,00</span>
                        </div>
                    </div>
                </div>

                <div>
                    <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest block mb-2">Pagamento</label>
                    <div class="flex gap-4">
                        <div id="pay-pix" onclick="selectPayment('Pix')" class="payment-option font-bold uppercase text-xs">Pix</div>
                        <div id="pay-boleto" onclick="selectPayment('Boleto')" class="payment-option font-bold uppercase text-xs">Boleto</div>
                    </div>
                </div>
            </div>

            <div class="p-8 border-t border-white/10 bg-black">
                <div class="flex gap-4">
                    <button onclick="backToCart()" class="flex-1 border border-white/10 py-4 text-white font-sync font-bold uppercase italic text-[10px]">Voltar</button>
                    <button onclick="processOrder()" class="flex-[2] bg-blue-600 py-4 text-white font-sync font-black uppercase italic hover:bg-white hover:text-blue-600 transition-all">Finalizar Pedido</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Firebase Logic -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";

        // CONFIGURAÇÃO IGUAL AO ADMIN PANEL
        const firebaseConfig = {
            apiKey: "AIzaSyAlJuJGDfkccfind-bOdweBEb9G0Pe5ssg",
            authDomain: "loja-de-produtos-3d.firebaseapp.com",
            projectId: "loja-de-produtos-3d",
            storageBucket: "loja-de-produtos-3d.firebasestorage.app",
            messagingSenderId: "245685423208",
            appId: "1:245685423208:web:f136124fc0fc889f667fdd"
        };

        const appId = 'crowlab-prod';
        const WHATSAPP_NUMBER = "5521990819172";
        const ORIGIN_CEP = "26600000";

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // ESTADO GLOBAL
        let user = null;
        let cart = [];
        let selectedPayment = '';
        let shippingValue = 0;
        let allProducts = []; // Armazena os produtos do Firebase
        let productSlides = {}; 

        // 1. AUTENTICAÇÃO ANÔNIMA PARA O CLIENTE
        onAuthStateChanged(auth, (u) => {
            user = u;
            if (!user) {
                signInAnonymously(auth);
            } else {
                startProductSync();
            }
        });

        // 2. SINCRONIZAÇÃO EM TEMPO REAL COM O FIREBASE
        function startProductSync() {
            const productsCol = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            
            onSnapshot(productsCol, (snapshot) => {
                allProducts = [];
                snapshot.forEach(docSnap => {
                    allProducts.push({ id: docSnap.id, ...docSnap.data() });
                });
                
                // Ordenar por data de criação (mais novos primeiro)
                allProducts.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
                
                renderArsenal(); // Renderiza com os novos dados
            }, (error) => {
                console.error("Erro Firebase:", error);
                document.getElementById('arsenal-grid').innerHTML = `<p class="col-span-full text-center text-red-500 uppercase font-sync text-xs">Erro de conexão com o Arsenal.</p>`;
            });
        }

        // 3. RENDERIZAÇÃO DO ARSENAL
        window.renderArsenal = function(filter = 'Todos') {
            const grid = document.getElementById('arsenal-grid');
            const data = filter === 'Todos' ? allProducts : allProducts.filter(p => p.cat === filter);
            
            if (data.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-20 border-2 border-dashed border-white/5 rounded-2xl text-slate-600 italic text-sm uppercase tracking-widest">Nenhum item disponível nesta categoria.</div>`;
                return;
            }

            grid.innerHTML = data.map(p => {
                if(productSlides[p.id] === undefined) productSlides[p.id] = 0;
                
                const images = p.images || [];
                const slidesHtml = images.length > 0 
                    ? images.map(img => `<div class="carousel-slide"><img src="${img}" onerror="this.src='https://placehold.co/600x600/111/333?text=Imagem+Nao+Encontrada'" class="w-full"></div>`).join('')
                    : `<div class="carousel-slide"><img src="https://placehold.co/600x600/111/333?text=Sem+Imagem" class="w-full"></div>`;
                
                const dotsHtml = images.map((_, idx) => `<div class="dot ${idx === productSlides[p.id] ? 'active' : ''}" onclick="moveSlide('${p.id}', ${idx})"></div>`).join('');

                return `
                <div class="complex-card group border border-white/10 bg-zinc-900/20 rounded-xl overflow-hidden hover:border-blue-500/50 transition-all">
                    <div class="carousel-container" id="carousel-${p.id}">
                        <div class="carousel-track" style="transform: translateX(-${productSlides[p.id] * 100}%)">
                            ${slidesHtml}
                        </div>
                        ${images.length > 1 ? `
                            <button class="carousel-btn btn-prev" onclick="prevSlide('${p.id}')"><i class="fas fa-chevron-left"></i></button>
                            <button class="carousel-btn btn-next" onclick="nextSlide('${p.id}')"><i class="fas fa-chevron-right"></i></button>
                            <div class="carousel-dots">${dotsHtml}</div>
                        ` : ''}
                    </div>
                    <div class="p-6">
                        <div class="text-blue-500 text-[9px] font-bold uppercase italic font-sync">${p.cat || 'Geral'}</div>
                        <h3 class="text-lg font-bold uppercase italic font-sync mb-4 h-12 truncate">${p.name}</h3>
                        <div class="flex justify-between items-center">
                            <span class="text-xl font-black italic font-sync">R$ ${parseFloat(p.price).toFixed(2)}</span>
                            <button onclick="addToCart('${p.id}')" class="w-10 h-10 bg-blue-600 flex items-center justify-center hover:bg-white hover:text-blue-600 transition-all rounded"><i class="fas fa-plus"></i></button>
                        </div>
                    </div>
                </div>
                `;
            }).join('');
        };

        window.moveSlide = function(prodId, index) {
            const product = allProducts.find(p => p.id === prodId);
            if (!product) return;
            productSlides[prodId] = index;
            const track = document.querySelector(`#carousel-${prodId} .carousel-track`);
            const dots = document.querySelectorAll(`#carousel-${prodId} .dot`);
            if(track) track.style.transform = `translateX(-${index * 100}%)`;
            dots.forEach((d, i) => d.classList.toggle('active', i === index));
        };

        window.nextSlide = function(prodId) {
            const product = allProducts.find(p => p.id === prodId);
            let nextIndex = (productSlides[prodId] + 1) % product.images.length;
            moveSlide(prodId, nextIndex);
        };

        window.prevSlide = function(prodId) {
            const product = allProducts.find(p => p.id === prodId);
            let prevIndex = (productSlides[prodId] - 1 + product.images.length) % product.images.length;
            moveSlide(prodId, prevIndex);
        };

        window.addToCart = function(id) {
            const prod = allProducts.find(p => p.id === id);
            const item = cart.find(i => i.id === id);
            if(item) item.qty++;
            else cart.push({...prod, qty: 1});
            updateCartUI();
        };

        window.removeFromCart = function(id) {
            cart = cart.filter(i => i.id !== id);
            updateCartUI();
        };

        function updateCartUI() {
            const subtotal = cart.reduce((a, b) => a + (b.price * b.qty), 0);
            const total = subtotal + shippingValue;
            
            const badge = document.getElementById('cart-badge');
            badge.innerText = cart.reduce((a,b) => a+b.qty, 0);
            badge.classList.toggle('hidden', cart.length === 0);
            
            document.getElementById('cart-total-top').innerText = `R$ ${total.toFixed(2)}`;
            document.getElementById('cart-grand-total').innerText = `R$ ${total.toFixed(2)}`;
            document.getElementById('btn-next-step').disabled = cart.length === 0;

            const list = document.getElementById('cart-items');
            list.innerHTML = cart.length ? cart.map(i => `
                <div class="flex gap-4 items-center bg-white/5 p-4 border border-white/5 rounded">
                    <div class="flex-1">
                        <h4 class="text-[10px] font-bold uppercase font-sync truncate">${i.name}</h4>
                        <div class="text-blue-500 text-xs font-bold mt-1">R$ ${parseFloat(i.price).toFixed(2)} [x${i.qty}]</div>
                    </div>
                    <button onclick="removeFromCart('${i.id}')" class="text-slate-500 hover:text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('') : `<div class="text-center py-10 text-slate-600 text-[10px] font-sync uppercase">Carrinho Vazio</div>`;
        }

        window.toggleCart = function() {
            const p = document.getElementById('cart-panel');
            p.style.right = (p.style.right === '0px' || p.style.right === '0%') ? '-100%' : '0px';
        };

        window.applyFilter = function(cat, btn) {
            document.querySelectorAll('.filter-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderArsenal(cat);
        };

        window.onlyNumbers = function(input) {
            input.value = input.value.replace(/\D/g, '');
        };

        window.fetchAddress = async function(cep) {
            if (cep.length !== 8) return;
            const cityInput = document.getElementById('user-city');
            const streetInput = document.getElementById('user-street');
            const neighborhoodInput = document.getElementById('user-neighborhood');
            const shippingArea = document.getElementById('shipping-area');
            cityInput.value = "Buscando...";
            try {
                const res = await fetch(`https://viacep.com.br/ws/${cep}/json/`);
                const data = await res.json();
                if (data.erro) { alertBox("CEP não encontrado."); cityInput.value = ""; return; }
                streetInput.value = data.logradouro || '';
                neighborhoodInput.value = data.bairro || '';
                cityInput.value = `${data.localidade} / ${data.uf}`;
                calculateRealShipping(cep, data.uf);
                shippingArea.classList.remove('hidden');
            } catch (err) { alertBox("Erro ao buscar CEP."); }
        };

        function calculateRealShipping(cep, uf) {
            let price = (cep === ORIGIN_CEP) ? 0 : (uf === 'RJ' ? 18.50 : (['SP','MG','ES'].includes(uf) ? 28.90 : 45.00));
            shippingValue = price;
            const badge = document.getElementById('shipping-value');
            if (badge) badge.innerText = price === 0 ? "GRÁTIS (RETIRADA)" : `R$ ${price.toFixed(2)}`;
            updateCartUI();
        }

        window.selectPayment = function(mode) {
            selectedPayment = mode;
            document.querySelectorAll('.payment-option').forEach(el => el.classList.remove('selected'));
            document.getElementById(`pay-${mode.toLowerCase()}`).classList.add('selected');
        };

        window.processOrder = async function() {
            if (!user) return;
            const f = (id) => document.getElementById(id).value.trim();
            const customerData = { name: f('user-name'), cpf: f('user-cpf'), cep: f('user-cep'), rua: f('user-street'), num: f('user-num'), bairro: f('user-neighborhood'), cidade: f('user-city'), comp: f('user-complement') };
            if(!customerData.name || !customerData.cpf || !customerData.cep || !customerData.rua || !customerData.num || !selectedPayment) { alertBox("Preencha todos os campos."); return; }

            try {
                const ordersRef = collection(db, 'artifacts', appId, 'public', 'data', 'orders');
                const total = cart.reduce((a, b) => a + (b.price * b.qty), 0) + shippingValue;
                await addDoc(ordersRef, { customer: customerData, items: cart, payment: selectedPayment, shipping: shippingValue, total, createdAt: serverTimestamp(), status: 'PENDENTE', uid: user.uid });
                
                let text = `🛸 *NOVO PEDIDO CROWLAB3D*\n\n👤 *CLIENTE:* ${customerData.name}\n📍 *ENDEREÇO:* ${customerData.rua}, ${customerData.num}\n💳 *PAGAMENTO:* ${selectedPayment}\n📦 *ITENS:*\n`;
                cart.forEach(i => text += `• ${i.name} (${i.qty}x)\n`);
                text += `\n💰 *TOTAL: R$ ${total.toFixed(2)}*`;
                window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(text)}`);
            } catch (error) { alertBox("Erro ao registrar pedido."); }
        };

        window.toggleFaq = function(btn) {
            const content = btn.nextElementSibling;
            const isOpen = content.style.maxHeight;
            document.querySelectorAll('.faq-content').forEach(c => c.style.maxHeight = null);
            if (!isOpen) content.style.maxHeight = content.scrollHeight + "px";
        };

        window.goToCheckout = function() {
            document.getElementById('view-cart').classList.add('hidden');
            document.getElementById('view-checkout').classList.remove('hidden');
            document.getElementById('panel-title').innerText = "Entrega";
        };

        window.backToCart = function() {
            document.getElementById('view-cart').classList.remove('hidden');
            document.getElementById('view-checkout').classList.add('hidden');
            document.getElementById('panel-title').innerText = "Carrinho";
        };

        window.alertBox = function(m) {
            const b = document.createElement('div');
            b.className = "fixed bottom-5 left-5 right-5 bg-red-600 text-white p-4 z-[5000] text-[10px] font-bold uppercase tracking-widest text-center rounded";
            b.innerText = m;
            document.body.appendChild(b);
            setTimeout(() => b.remove(), 4000);
        };

        window.customOrder = function() { window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=Olá! Gostaria de um orçamento personalizado.`); };
    </script>
</body>
</html>
