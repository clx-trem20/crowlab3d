<!DOCTYPE html>
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
        
        /* Skeleton Loader */
        .skeleton { background: linear-gradient(90deg, #0f172a 25%, #1e293b 50%, #0f172a 75%); background-size: 200% 100%; animation: skeleton-loading 1.5s infinite; }
        @keyframes skeleton-loading { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }
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
            <div class="flex items-center space-x-6">
                <div class="w-10 h-10 border border-blue-500/50 flex items-center justify-center rotate-45 bg-blue-600/5">
                    <i class="fas fa-microchip -rotate-45 text-blue-500 text-lg"></i>
                </div>
                <span class="text-2xl font-black italic font-sync">CROWLAB<span class="text-blue-600">3D</span></span>
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
                <!-- Skeletons enquanto carrega do Firebase -->
                <div class="skeleton h-80 rounded-xl"></div>
                <div class="skeleton h-80 rounded-xl"></div>
                <div class="skeleton h-80 rounded-xl"></div>
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

        <!-- Vista: Lista do Carrinho -->
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

        <!-- Vista: Checkout -->
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

    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, query } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- CONFIGURAÇÃO FIREBASE ---
        const firebaseConfig = {
            apiKey: "AIzaSyAlJuJGDfkccfind-bOdweBEb9G0Pe5ssg",
            authDomain: "loja-de-produtos-3d.firebaseapp.com",
            projectId: "loja-de-produtos-3d",
            storageBucket: "loja-de-produtos-3d.firebasestorage.app",
            messagingSenderId: "245685423208",
            appId: "1:245685423208:web:f136124fc0fc889f667fdd"
        };

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'loja-de-produtos-3d';
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Globais do Arsenal
        window.arsenalData = [];
        window.currentFilter = 'Todos';

        // Autenticação Inicial
        const initFirebase = async () => {
            try {
                await signInAnonymously(auth);
            } catch (err) { console.error("Erro Auth:", err); }
        };

        // Listener em tempo real para os produtos
        onAuthStateChanged(auth, (user) => {
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

        /* Fundo Dinâmico */
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

        /* Carrossel de Imagens */
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

        /* Floating UI */
        .social-float { position: fixed; right: 1.5rem; bottom: 1.5rem; display: flex; flex-direction: column; gap: 0.75rem; z-index: 1000; }
        .float-btn { width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.4rem; color: white; box-shadow: 0 10px 25px rgba(0,0,0,0.4); transition: 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); text-decoration: none; }
        .btn-wa { background: #25d366; }
        .btn-ig { background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); }

        /* Grid de Produtos */
        #arsenal-grid { display: grid; gap: 1.5rem; grid-template-columns: repeat(1, 1fr); padding: 3rem 0; }
        @media (min-width: 640px) { #arsenal-grid { grid-template-columns: repeat(2, 1fr); } }
        @media (min-width: 1024px) { #arsenal-grid { grid-template-columns: repeat(3, 1fr); } }

        /* Filtros */
        .filter-item {
            padding: 1.5rem 1rem; text-align: center; border-right: 1px solid #1e293b;
            text-transform: uppercase; font-weight: 700; letter-spacing: 0.2em; font-size: 0.65rem;
            transition: 0.3s; background: transparent; color: #64748b; white-space: nowrap;
        }
        .filter-item.active { background: var(--neon-blue); color: white; }
        .no-scrollbar::-webkit-scrollbar { display: none; }

        /* Checkout Inputs */
        .checkout-input {
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
            color: white; padding: 0.75rem 1rem; width: 100%; border-radius: 4px;
            outline: none; transition: 0.3s; font-size: 0.85rem;
        }
        .checkout-input:focus { border-color: var(--neon-blue); background: rgba(255,255,255,0.08); }

        /* FAQ */
        .faq-item { border-bottom: 1px solid rgba(255,255,255,0.05); }
        .faq-trigger { width: 100%; display: flex; justify-content: space-between; align-items: center; padding: 1.5rem 0; text-align: left; }
        .faq-content { max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out; color: #94a3b8; font-size: 0.85rem; }
        .faq-item.active .faq-content { max-height: 200px; padding-bottom: 1.5rem; }
        .faq-item.active i { transform: rotate(180deg); color: var(--neon-blue); }

        /* Skeletons */
        .skeleton { background: linear-gradient(90deg, #0f172a 25%, #1e293b 50%, #0f172a 75%); background-size: 200% 100%; animation: skeleton-loading 1.5s infinite; }
        @keyframes skeleton-loading { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }

        /* Vidro Neon */
        .glass-card { background: rgba(15, 23, 42, 0.6); backdrop-filter: blur(12px); border: 1px solid rgba(255,255,255,0.05); }
    </style>
</head>
<body>

    <div class="bg-grid"></div>

    <!-- Redes Sociais Flutuantes -->
    <div class="social-float">
        <a href="https://instagram.com/crowlab3d" target="_blank" class="float-btn btn-ig"><i class="fab fa-instagram"></i></a>
        <a href="javascript:void(0)" onclick="customOrder()" class="float-btn btn-wa"><i class="fab fa-whatsapp"></i></a>
    </div>

    <!-- Navegação -->
    <nav class="fixed top-0 left-0 w-full z-[100] border-b border-white/5 bg-black/90 backdrop-blur-md">
        <div class="main-container flex justify-between items-center h-20">
            <div class="flex items-center space-x-6">
                <div class="w-10 h-10 border border-blue-500/50 flex items-center justify-center rotate-45 bg-blue-600/5">
                    <i class="fas fa-microchip -rotate-45 text-blue-500 text-lg"></i>
                </div>
                <span class="text-2xl font-black italic font-sync tracking-tighter">CROWLAB<span class="text-blue-600">3D</span></span>
            </div>
            
            <div class="hidden md:flex space-x-8 text-[10px] uppercase font-bold tracking-[0.3em] text-slate-400">
                <a href="#arsenal" class="hover:text-blue-500 transition-colors">Arsenal</a>
                <a href="#servicos" class="hover:text-blue-500 transition-colors">Serviços</a>
                <a href="#faq" class="hover:text-blue-500 transition-colors">Suporte</a>
            </div>

            <button onclick="toggleCart()" class="relative flex items-center group">
                <div id="cart-total-top" class="text-sm font-bold italic font-sync mr-4 hidden sm:block group-hover:text-blue-500 transition-colors">R$ 0,00</div>
                <div class="w-12 h-12 bg-blue-600 flex items-center justify-center group-hover:bg-white group-hover:text-blue-600 transition-all">
                    <i class="fas fa-shopping-cart"></i>
                    <span id="cart-badge" class="absolute -top-1 -right-1 bg-white text-blue-600 w-5 h-5 text-[10px] font-black flex items-center justify-center rounded-full hidden">0</span>
                </div>
            </button>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="viewport-section pt-40 pb-20">
        <div class="main-container text-center">
            <div class="inline-block px-4 py-1 border border-blue-500/30 bg-blue-500/5 text-blue-500 text-[9px] uppercase font-bold tracking-[0.4em] mb-6 rounded-full">
                Próxima Geração de Manufatura
            </div>
            <h1 class="hero-title italic">CROWLAB</h1>
            <h1 class="hero-title italic text-blue-600" style="margin-top: -10px;">SYSTEMS</h1>
            <p class="max-w-xl mx-auto text-slate-500 text-sm leading-relaxed mt-8 font-light">
                Transformamos designs digitais complexos em objetos físicos de alta performance. Precisão absoluta, materiais industriais e acabamento premium.
            </p>
            <div class="mt-10 flex flex-col sm:flex-row justify-center gap-4">
                <a href="#arsenal" class="bg-white text-black px-10 py-4 font-sync font-bold text-[10px] uppercase italic hover:bg-blue-600 hover:text-white transition-all">Explorar Arsenal</a>
                <button onclick="customOrder()" class="border border-white/10 px-10 py-4 font-sync font-bold text-[10px] uppercase italic hover:bg-white/5 transition-all">Peça Personalizada</button>
            </div>
        </div>
    </section>

    <!-- Filtros Fixos -->
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

    <!-- Grid de Produtos (Arsenal) -->
    <section class="py-20" id="arsenal">
        <div class="main-container">
            <div class="flex justify-between items-end mb-12">
                <div>
                    <h2 class="text-3xl font-black font-sync italic uppercase">Arsenal <span class="text-blue-600">Disponível</span></h2>
                    <p class="text-slate-500 text-[10px] uppercase tracking-widest mt-2">Pronto para manufatura imediata</p>
                </div>
            </div>
            <div id="arsenal-grid">
                <!-- Skeletons enquanto carrega -->
                <div class="skeleton h-[450px] rounded-xl"></div>
                <div class="skeleton h-[450px] rounded-xl"></div>
                <div class="skeleton h-[450px] rounded-xl"></div>
            </div>
        </div>
    </section>

    <!-- Secção de Serviços -->
    <section class="py-24 bg-zinc-950/50" id="servicos">
        <div class="main-container">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-12">
                <div class="glass-card p-10 border-l-4 border-l-blue-600">
                    <i class="fas fa-cube text-3xl text-blue-600 mb-6"></i>
                    <h3 class="font-sync text-sm font-bold mb-4 uppercase">Impressão FDM/SLA</h3>
                    <p class="text-slate-400 text-sm leading-relaxed">Produzimos desde protótipos rápidos até peças finais com resolução de mícron em resina ou termoplásticos.</p>
                </div>
                <div class="glass-card p-10 border-l-4 border-l-blue-600">
                    <i class="fas fa-paint-brush text-3xl text-blue-600 mb-6"></i>
                    <h3 class="font-sync text-sm font-bold mb-4 uppercase">Pós-Processamento</h3>
                    <p class="text-slate-400 text-sm leading-relaxed">Lixamento profissional, pintura personalizada e verniz automotivo para um acabamento de nível industrial.</p>
                </div>
                <div class="glass-card p-10 border-l-4 border-l-blue-600">
                    <i class="fas fa-drafting-compass text-3xl text-blue-600 mb-6"></i>
                    <h3 class="font-sync text-sm font-bold mb-4 uppercase">Modelagem 3D</h3>
                    <p class="text-slate-400 text-sm leading-relaxed">Não tem o arquivo? A nossa equipa de engenharia cria o modelo 3D baseado na sua ideia ou rascunho.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- FAQ -->
    <section class="py-24" id="faq">
        <div class="main-container max-w-3xl">
            <h2 class="text-2xl font-black font-sync italic uppercase mb-12 text-center">Questões <span class="text-blue-600">Frequentes</span></h2>
            <div class="space-y-2">
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold text-sm uppercase font-sync">Qual o prazo de produção?</span>
                        <i class="fas fa-chevron-down transition-transform"></i>
                    </button>
                    <div class="faq-content">O prazo médio de manufatura varia entre 3 a 7 dias úteis, dependendo da complexidade da peça e da fila de impressão atual.</div>
                </div>
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold text-sm uppercase font-sync">Quais materiais utilizam?</span>
                        <i class="fas fa-chevron-down transition-transform"></i>
                    </button>
                    <div class="faq-content">Trabalhamos com PLA Premium, PETG, ABS, TPU (flexível) e Resinas de alta precisão (Standard e Tough).</div>
                </div>
                <div class="faq-item">
                    <button class="faq-trigger" onclick="toggleFaq(this)">
                        <span class="font-bold text-sm uppercase font-sync">Fazem envios para todo o país?</span>
                        <i class="fas fa-chevron-down transition-transform"></i>
                    </button>
                    <div class="faq-content">Sim. Enviamos via transportadora ou correios com embalagem reforçada anti-impacto para garantir a integridade da sua peça.</div>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="py-20 border-t border-white/5 bg-black">
        <div class="main-container">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-12 mb-16">
                <div class="col-span-2">
                    <span class="text-2xl font-black italic font-sync">CROWLAB<span class="text-blue-600">3D</span></span>
                    <p class="text-slate-500 mt-6 max-w-sm text-sm">Laboratório especializado em soluções de manufatura aditiva e design de produtos personalizados.</p>
                </div>
                <div>
                    <h4 class="text-[10px] uppercase font-bold tracking-widest text-blue-600 mb-6">Links</h4>
                    <ul class="space-y-4 text-sm text-slate-400">
                        <li><a href="#arsenal" class="hover:text-white">Arsenal</a></li>
                        <li><a href="#servicos" class="hover:text-white">Serviços</a></li>
                        <li><a href="#faq" class="hover:text-white">Suporte</a></li>
                    </ul>
                </div>
                <div>
                    <h4 class="text-[10px] uppercase font-bold tracking-widest text-blue-600 mb-6">Social</h4>
                    <div class="flex gap-4">
                        <a href="https://instagram.com/crowlab3d" class="w-10 h-10 border border-white/10 flex items-center justify-center hover:bg-blue-600 hover:border-blue-600 transition-all"><i class="fab fa-instagram"></i></a>
                        <a href="javascript:void(0)" onclick="customOrder()" class="w-10 h-10 border border-white/10 flex items-center justify-center hover:bg-blue-600 hover:border-blue-600 transition-all"><i class="fab fa-whatsapp"></i></a>
                    </div>
                </div>
            </div>
            <div class="pt-8 border-t border-white/5 flex flex-col md:flex-row justify-between items-center gap-6">
                <p class="text-slate-600 text-[9px] uppercase tracking-widest">© 2025 – CROWLAB3D – Todos os direitos reservados</p>
                <div class="flex items-center gap-4 text-slate-600 text-[9px] uppercase tracking-widest font-bold">
                    <span>Brasil</span>
                    <span class="w-1 h-1 bg-slate-800 rounded-full"></span>
                    <span>Powered by CLX Systems</span>
                </div>
            </div>
        </div>
    </footer>

    <!-- Carrinho & Checkout Panel -->
    <div id="cart-panel" class="fixed top-0 right-[-100%] w-full max-w-[500px] h-screen bg-zinc-950 z-[2000] transition-all duration-500 border-l border-white/10 flex flex-col shadow-2xl">
        <div class="p-8 border-b border-white/10 flex justify-between items-center bg-black">
            <h2 id="panel-title" class="text-xl font-bold font-sync italic uppercase">Carrinho</h2>
            <button onclick="toggleCart()" class="w-10 h-10 flex items-center justify-center text-white hover:bg-red-500 transition-colors"><i class="fas fa-times text-xl"></i></button>
        </div>

        <!-- View: Cart Items -->
        <div id="view-cart" class="flex-1 flex flex-col overflow-hidden">
            <div id="cart-items" class="flex-1 overflow-y-auto p-8 space-y-6">
                <!-- Inserido via JS -->
            </div>
            <div class="p-8 border-t border-white/10 bg-black">
                <div class="flex justify-between items-center mb-6">
                    <span class="text-xs uppercase font-bold text-slate-500">Subtotal</span>
                    <span id="cart-grand-total" class="text-3xl font-black font-sync italic text-blue-600">R$ 0,00</span>
                </div>
                <button onclick="goToCheckout()" id="btn-next-step" class="w-full bg-blue-600 py-6 text-white font-sync font-black uppercase italic hover:bg-white hover:text-blue-600 transition-all disabled:opacity-50">Prosseguir para Entrega</button>
            </div>
        </div>

        <!-- View: Checkout Details -->
        <div id="view-checkout" class="flex-1 flex flex-col overflow-hidden hidden">
            <div class="flex-1 overflow-y-auto p-8 space-y-8">
                <div>
                    <label class="text-[10px] uppercase font-bold text-slate-500 tracking-widest block mb-4">Informações de Envio</label>
                    <div class="space-y-4">
                        <input type="text" id="user-name" placeholder="Nome Completo" class="checkout-input">
                        <div class="grid grid-cols-2 gap-4">
                            <input type="text" id="user-cep" placeholder="CEP" class="checkout-input" maxlength="8" onblur="fetchAddress(this.value)">
                            <input type="text" id="user-phone" placeholder="WhatsApp" class="checkout-input">
                        </div>
                        <input type="text" id="user-street" placeholder="Rua / Avenida" class="checkout-input">
                        <div class="grid grid-cols-3 gap-4">
                            <input type="text" id="user-num" placeholder="Nº" class="checkout-input">
                            <input type="text" id="user-city" placeholder="Cidade/UF" class="checkout-input col-span-2">
                        </div>
                    </div>
                </div>
                <div id="shipping-area" class="hidden">
                    <div class="bg-blue-600/5 border border-blue-600/30 p-6 rounded flex justify-between items-center">
                        <div class="flex items-center gap-4">
                            <i class="fas fa-truck text-blue-500"></i>
                            <div>
                                <div class="text-[10px] uppercase font-bold text-blue-500">Frete Calculado</div>
                                <div class="text-xs text-slate-400">Entrega via Transportadora</div>
                            </div>
                        </div>
                        <span id="shipping-value" class="font-sync font-bold text-sm">R$ 0,00</span>
                    </div>
                </div>
            </div>
            <div class="p-8 border-t border-white/10 bg-black flex gap-4">
                <button onclick="backToCart()" class="w-20 border border-white/10 flex items-center justify-center hover:bg-white/5"><i class="fas fa-arrow-left"></i></button>
                <button onclick="processOrder()" class="flex-1 bg-blue-600 py-4 text-white font-sync font-black uppercase italic hover:bg-blue-700 transition-all">Finalizar no WhatsApp</button>
            </div>
        </div>
    </div>

    <!-- FIREBASE & LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';

        window.arsenalData = [];
        window.currentFilter = 'Todos';

        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (e) { console.error("Erro Auth:", e); }
        };

        onAuthStateChanged(auth, (user) => {
            if (user) {
                // Caminho rigoroso conforme solicitado pelas regras de ambiente
                const path = ['artifacts', appId, 'public', 'data', 'produtos'];
                const productsRef = collection(db, ...path);
                
                onSnapshot(productsRef, (snap) => {
                    window.arsenalData = snap.docs.map(doc => ({
                        id: doc.id,
                        ...doc.data(),
                        images: Array.isArray(doc.data().images) ? doc.data().images : (doc.data().image ? [doc.data().image] : ["https://placehold.co/600x600/111/333?text=Crowlab3D"]),
                        price: parseFloat(doc.data().price || 0)
                    }));
                    renderArsenal(window.currentFilter);
                }, (err) => {
                    console.error("Firestore Error:", err);
                    document.getElementById('arsenal-grid').innerHTML = `<div class='col-span-full py-20 text-center text-red-500 font-sync text-[10px] uppercase'>Erro de Permissão: Contacte o Administrador</div>`;
                });
            }
        });

        initAuth();
    </script>

    <script>
        const WHATSAPP_NUMBER = "5521990819172";
        let cart = [];
        let shippingValue = 0;
        let productSlides = {};

        function renderArsenal(filter = 'Todos') {
            window.currentFilter = filter;
            const grid = document.getElementById('arsenal-grid');
            const data = filter === 'Todos' ? window.arsenalData : window.arsenalData.filter(p => p.cat === filter);

            if (!data.length && window.arsenalData.length > 0) {
                grid.innerHTML = `<div class="col-span-full py-20 text-center text-slate-500 uppercase text-[10px] font-sync">Nenhum item nesta categoria...</div>`;
                return;
            } else if (!window.arsenalData.length) return;

            grid.innerHTML = data.map(p => {
                if(productSlides[p.id] === undefined) productSlides[p.id] = 0;
                return `
                <div class="complex-card group border border-white/10 bg-zinc-900/20 rounded-xl overflow-hidden hover:border-blue-500/50 transition-all flex flex-col">
                    <div class="carousel-container" id="carousel-${p.id}">
                        <div class="carousel-track" style="transform: translateX(-${productSlides[p.id] * 100}%)">
                            ${p.images.map(img => `<div class="carousel-slide"><img src="${img}" class="w-full h-full object-cover"></div>`).join('')}
                        </div>
                        ${p.images.length > 1 ? `
                            <button class="carousel-btn btn-prev" onclick="prevSlide('${p.id}')"><i class="fas fa-chevron-left"></i></button>
                            <button class="carousel-btn btn-next" onclick="nextSlide('${p.id}')"><i class="fas fa-chevron-right"></i></button>
                        ` : ''}
                    </div>
                    <div class="p-8 flex-1 flex flex-col">
                        <div class="text-blue-500 text-[9px] font-bold uppercase italic font-sync tracking-widest">${p.cat || 'Arsenal'}</div>
                        <h3 class="text-lg font-bold uppercase italic font-sync mt-2 mb-6 h-12 line-clamp-2">${p.name}</h3>
                        <div class="mt-auto flex justify-between items-center pt-6 border-t border-white/5">
                            <div class="flex flex-col">
                                <span class="text-[10px] text-slate-500 uppercase font-bold tracking-tighter">Valor Unitário</span>
                                <span class="text-2xl font-black italic font-sync">R$ ${p.price.toFixed(2)}</span>
                            </div>
                            <button onclick="addToCart('${p.id}')" class="w-12 h-12 bg-blue-600 flex items-center justify-center hover:bg-white hover:text-blue-600 transition-all rounded shadow-lg shadow-blue-600/20">
                                <i class="fas fa-cart-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>`;
            }).join('');
        }

        window.moveSlide = function(id, idx) {
            const track = document.querySelector(`#carousel-${id} .carousel-track`);
            if(track) track.style.transform = `translateX(-${idx * 100}%)`;
            productSlides[id] = idx;
        };

        window.nextSlide = function(id) {
            const p = window.arsenalData.find(x => x.id === id);
            let next = (productSlides[id] + 1) % p.images.length;
            moveSlide(id, next);
        };

        window.prevSlide = function(id) {
            const p = window.arsenalData.find(x => x.id === id);
            let prev = (productSlides[id] - 1 + p.images.length) % p.images.length;
            moveSlide(id, prev);
        };

        function addToCart(id) {
            const prod = window.arsenalData.find(p => p.id === id);
            const item = cart.find(i => i.id === id);
            if(item) item.qty++; else cart.push({...prod, qty: 1});
            updateCartUI();
            if(window.innerWidth > 768) {
                const panel = document.getElementById('cart-panel');
                if(panel.style.right !== '0px') toggleCart();
            }
        }

        function removeFromCart(id) {
            cart = cart.filter(i => i.id !== id);
            updateCartUI();
        }

        function updateCartUI() {
            const subtotal = cart.reduce((a, b) => a + (b.price * b.qty), 0);
            const total = subtotal + shippingValue;
            
            document.getElementById('cart-badge').innerText = cart.reduce((a,b) => a+b.qty, 0);
            document.getElementById('cart-badge').classList.toggle('hidden', !cart.length);
            document.getElementById('cart-total-top').innerText = `R$ ${total.toFixed(2)}`;
            document.getElementById('cart-grand-total').innerText = `R$ ${total.toFixed(2)}`;
            
            const itemsContainer = document.getElementById('cart-items');
            if(!cart.length) {
                itemsContainer.innerHTML = `<div class="h-full flex flex-col items-center justify-center text-slate-600">
                    <i class="fas fa-shopping-basket text-4xl mb-4 opacity-20"></i>
                    <p class="uppercase font-sync text-[10px] tracking-widest">Carrinho Vazio</p>
                </div>`;
                document.getElementById('btn-next-step').disabled = true;
                return;
            }
            
            document.getElementById('btn-next-step').disabled = false;
            itemsContainer.innerHTML = cart.map(i => `
                <div class="flex gap-6 items-center bg-white/5 p-4 border border-white/5 rounded group relative">
                    <img src="${i.images[0]}" class="w-16 h-16 object-cover rounded grayscale group-hover:grayscale-0 transition-all">
                    <div class="flex-1">
                        <h4 class="text-[10px] font-bold uppercase font-sync leading-tight">${i.name}</h4>
                        <div class="flex items-center gap-4 mt-2">
                            <span class="text-blue-500 text-xs font-black">R$ ${i.price.toFixed(2)}</span>
                            <span class="text-slate-500 text-[9px] uppercase font-bold">Qtd: ${i.qty}</span>
                        </div>
                    </div>
                    <button onclick="removeFromCart('${i.id}')" class="text-slate-700 hover:text-red-500 transition-colors"><i class="fas fa-trash-alt"></i></button>
                </div>`).join('');
        }

        function toggleCart() {
            const p = document.getElementById('cart-panel');
            const isOpen = p.style.right === '0px';
            p.style.right = isOpen ? '-100%' : '0px';
            if(isOpen) backToCart(); // Reset views when closing
        }

        function goToCheckout() {
            document.getElementById('view-cart').classList.add('hidden');
            document.getElementById('view-checkout').classList.remove('hidden');
            document.getElementById('panel-title').innerText = "Entrega";
        }

        function backToCart() {
            document.getElementById('view-checkout').classList.add('hidden');
            document.getElementById('view-cart').classList.remove('hidden');
            document.getElementById('panel-title').innerText = "Carrinho";
        }

        function applyFilter(cat, btn) {
            document.querySelectorAll('.filter-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderArsenal(cat);
        }

        function toggleFaq(btn) {
            const item = btn.parentElement;
            const wasActive = item.classList.contains('active');
            document.querySelectorAll('.faq-item').forEach(i => i.classList.remove('active'));
            if(!wasActive) item.classList.add('active');
        }

        async function fetchAddress(cep) {
            const cleanCep = cep.replace(/\D/g, '');
            if(cleanCep.length !== 8) return;
            try {
                const res = await fetch(`https://viacep.com.br/ws/${cleanCep}/json/`);
                const data = await res.json();
                if(!data.erro) {
                    document.getElementById('user-street').value = data.logradouro;
                    document.getElementById('user-city').value = `${data.localidade}/${data.uf}`;
                    shippingValue = data.uf === 'RJ' ? 19.90 : 38.50;
                    document.getElementById('shipping-value').innerText = `R$ ${shippingValue.toFixed(2)}`;
                    document.getElementById('shipping-area').classList.remove('hidden');
                    updateCartUI();
                }
            } catch (e) { console.error(e); }
        }

        function processOrder() {
            const name = document.getElementById('user-name').value;
            const city = document.getElementById('user-city').value;
            if(!name || !city) { 
                alert("Por favor, preencha os dados de entrega."); 
                return; 
            }

            let text = `🛸 *NOVO PEDIDO CROWLAB3D*\n\n`;
            text += `*Cliente:* ${name}\n`;
            text += `*Local:* ${city}\n\n`;
            text += `*ARSENAL SELECIONADO:*\n`;
            cart.forEach(i => text += `• ${i.name} (x${i.qty}) - R$ ${(i.price * i.qty).toFixed(2)}\n`);
            text += `\n📦 *Frete:* R$ ${shippingValue.toFixed(2)}`;
            text += `\n💰 *TOTAL FINAL: ${document.getElementById('cart-grand-total').innerText}*`;
            text += `\n\n_A aguardar confirmação de manufatura..._`;

            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(text)}`);
        }

        function customOrder() {
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent("Olá! Tenho um projeto personalizado e gostaria de um orçamento da CROWLAB3D.")}`);
        }
    </script>
</body>
</html>
