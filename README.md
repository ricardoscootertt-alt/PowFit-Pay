<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pay - Gestão de Clubes</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: { 50: '#eff6ff', 100: '#dbeafe', 400: '#60a5fa', 500: '#3b82f6', 600: '#2563eb', 700: '#1d4ed8', 900: '#1e3a8a', 950: '#172554' },
                        dark: { bg: '#0f172a', card: '#1e293b', border: '#334155' }
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        @media print {
            body { background-color: white !important; color: black !important; }
            .no-print { display: none !important; }
            .print-container { width: 100% !important; margin: 0 !important; padding: 0 !important; }
            .dark\:bg-dark-card { background-color: white !important; border: 1px solid #e5e7eb !important; }
            .text-white, .dark\:text-gray-300, .text-gray-400 { color: black !important; }
            .border-dark-border { border-color: #e5e7eb !important; }
            * { box-shadow: none !important; }
        }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }
    </style>
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="bg-dark-bg text-gray-200 min-h-screen flex antialiased">

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="fixed inset-0 bg-dark-bg z-50 flex flex-col items-center justify-center transition-opacity duration-300">
        <div class="bg-dark-card p-8 rounded-2xl shadow-2xl border border-dark-border max-w-md w-full text-center">
            <div class="flex justify-center mb-6">
                <div class="w-16 h-16 bg-brand-600 rounded-xl flex items-center justify-center shadow-lg shadow-brand-600/30">
                    <i data-lucide="shield-half" class="text-white w-8 h-8"></i>
                </div>
            </div>
            <h1 class="text-3xl font-bold text-white mb-2">PowFit Pay</h1>
            <p class="text-gray-400 mb-8">Gestão Financeira para Clubes.</p>
            
            <button id="btn-google-login" class="w-full flex items-center justify-center gap-3 bg-white text-gray-900 font-semibold py-3 px-4 rounded-xl hover:bg-gray-100 transition-colors shadow-md">
                <svg class="w-5 h-5" viewBox="0 0 24 24">
                    <path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
                    <path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
                    <path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
                    <path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
                </svg>
                Entrar com Google
            </button>
            <div id="login-loading" class="hidden mt-4 text-brand-400 text-sm">Autenticando...</div>
        </div>
    </div>

    <!-- APLICATIVO PRINCIPAL -->
    <div id="app-container" class="hidden w-full flex min-h-screen">
        
        <!-- SIDEBAR -->
        <aside class="w-64 bg-dark-card border-r border-dark-border flex flex-col no-print fixed h-full z-40">
            <div class="p-6 border-b border-dark-border flex items-center gap-3">
                <div class="w-10 h-10 bg-brand-600 rounded-lg flex items-center justify-center">
                    <i data-lucide="shield-half" class="text-white w-6 h-6"></i>
                </div>
                <div>
                    <h2 class="text-xl font-bold text-white tracking-tight">PowFit</h2>
                    <p class="text-xs text-brand-400 font-medium">CLUB SYSTEM</p>
                </div>
            </div>

            <nav class="flex-1 p-4 space-y-2 overflow-y-auto">
                <button onclick="navigate('dashboard')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="dashboard">
                    <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
                    <span class="font-medium">Dashboard</span>
                </button>
                <button onclick="navigate('receitas')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="receitas">
                    <i data-lucide="users" class="w-5 h-5 text-emerald-400"></i>
                    <span class="font-medium">Sócios/Mensalidades</span>
                </button>
                <button onclick="navigate('produtos')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="produtos">
                    <i data-lucide="shopping-bag" class="w-5 h-5 text-indigo-400"></i>
                    <span class="font-medium">Venda de Produtos</span>
                </button>
                <button onclick="navigate('despesas')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="despesas">
                    <i data-lucide="arrow-down-circle" class="w-5 h-5 text-rose-400"></i>
                    <span class="font-medium">Despesas/Gastos</span>
                </button>
                <button onclick="navigate('relatorios')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="relatorios">
                    <i data-lucide="file-bar-chart" class="w-5 h-5"></i>
                    <span class="font-medium">Relatórios</span>
                </button>
                <button onclick="navigate('configuracoes')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="configuracoes">
                    <i data-lucide="settings" class="w-5 h-5"></i>
                    <span class="font-medium">Configurações</span>
                </button>
            </nav>

            <div class="p-4 border-t border-dark-border">
                <div class="flex items-center gap-3 mb-4">
                    <img id="user-avatar" src="" alt="Avatar" class="w-10 h-10 rounded-full bg-dark-border hidden">
                    <div id="user-avatar-fallback" class="w-10 h-10 rounded-full bg-brand-600 flex items-center justify-center text-white font-bold">U</div>
                    <div class="overflow-hidden">
                        <p id="user-name" class="text-sm font-medium text-white truncate">Usuário</p>
                    </div>
                </div>
                <button id="btn-logout" class="w-full flex items-center justify-center gap-2 px-4 py-2 bg-dark-border hover:bg-rose-500/20 hover:text-rose-400 text-gray-300 rounded-lg transition-colors text-sm font-medium">
                    <i data-lucide="log-out" class="w-4 h-4"></i> Sair
                </button>
            </div>
        </aside>

        <!-- MAIN CONTENT -->
        <main class="flex-1 ml-64 p-8 print-container bg-dark-bg min-h-screen">
            
            <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 no-print gap-4">
                <div>
                    <h1 id="page-title" class="text-2xl font-bold text-white">Dashboard</h1>
                    <p id="club-header-name" class="text-sm text-brand-400 mt-1">Clube não definido</p>
                </div>
                
                <div class="flex items-center gap-3">
                    <div class="flex items-center gap-4 bg-dark-card p-2 rounded-xl border border-dark-border">
                        <div class="flex items-center gap-2 px-3 border-r border-dark-border">
                            <i data-lucide="calendar" class="w-4 h-4 text-gray-400"></i>
                            <input type="month" id="filtro-mes" class="bg-transparent text-white font-medium focus:outline-none cursor-pointer">
                        </div>
                        <div class="flex items-center gap-2 px-3">
                            <i data-lucide="map-pin" class="w-4 h-4 text-gray-400"></i>
                            <select id="filtro-setor-global" class="bg-transparent text-white font-medium focus:outline-none cursor-pointer">
                                <option value="Todos">Todos os Setores</option>
                            </select>
                        </div>
                    </div>
                    
                    <button id="btn-logout-header" title="Sair da Conta" class="bg-dark-card border border-dark-border p-2.5 rounded-xl text-rose-400 hover:bg-rose-500 hover:text-white transition-colors shadow-sm flex items-center justify-center">
                        <i data-lucide="log-out" class="w-5 h-5"></i>
                    </button>
                </div>
            </header>

            <!-- AVISO DE CONFIGURAÇÃO PENDENTE -->
            <div id="aviso-config" class="hidden bg-rose-500/10 border border-rose-500/50 text-rose-400 p-4 rounded-xl mb-6 flex items-center justify-between no-print">
                <div class="flex items-center gap-3">
                    <i data-lucide="alert-triangle" class="w-5 h-5"></i>
                    <span>Você precisa cadastrar pelo menos um <strong>Setor</strong> nas configurações antes de registrar finanças.</span>
                </div>
                <button onclick="navigate('configuracoes')" class="bg-rose-500 text-white px-4 py-1.5 rounded-lg text-sm font-medium hover:bg-rose-600 transition-colors">
                    Configurar Agora
                </button>
            </div>

            <!-- DASHBOARD SECTION -->
            <section id="sec-dashboard" class="page-section">
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Sócios (Receitas)</p>
                                <h3 id="dash-faturamento-socios" class="text-2xl font-bold text-emerald-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-emerald-400/10 rounded-lg"><i data-lucide="users" class="w-5 h-5 text-emerald-400"></i></div>
                        </div>
                    </div>
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Venda Produtos</p>
                                <h3 id="dash-faturamento-produtos" class="text-2xl font-bold text-indigo-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-indigo-400/10 rounded-lg"><i data-lucide="shopping-bag" class="w-5 h-5 text-indigo-400"></i></div>
                        </div>
                    </div>
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Gastos Totais</p>
                                <h3 id="dash-despesas" class="text-2xl font-bold text-rose-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-rose-400/10 rounded-lg"><i data-lucide="trending-down" class="w-5 h-5 text-rose-400"></i></div>
                        </div>
                    </div>
                    <div class="bg-dark-card p-6 rounded-2xl border border-brand-500/30 shadow-[0_0_15px_rgba(59,130,246,0.1)]">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Lucro Líquido</p>
                                <h3 id="dash-lucro" class="text-2xl font-bold text-brand-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-brand-500/10 rounded-lg"><i data-lucide="wallet" class="w-5 h-5 text-brand-400"></i></div>
                        </div>
                    </div>
                </div>

                <div class="bg-gradient-to-r from-dark-card to-brand-950/20 p-6 rounded-2xl border border-dark-border mb-8">
                    <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                        <i data-lucide="pie-chart" class="w-5 h-5 text-brand-400"></i> Pró-labore Sócios do Clube (50/50)
                    </h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="bg-dark-bg p-4 rounded-xl border border-dark-border flex items-center justify-between">
                            <span class="text-gray-400">Sócio 1 (50%)</span>
                            <span id="dash-socio1" class="text-xl font-bold text-white">R$ 0,00</span>
                        </div>
                        <div class="bg-dark-bg p-4 rounded-xl border border-dark-border flex items-center justify-between">
                            <span class="text-gray-400">Sócio 2 (50%)</span>
                            <span id="dash-socio2" class="text-xl font-bold text-white">R$ 0,00</span>
                        </div>
                    </div>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                    <h3 class="text-lg font-bold text-white mb-4">Transações Recentes</h3>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg rounded-lg">
                                <tr>
                                    <th class="px-4 py-3 rounded-l-lg">Tipo</th>
                                    <th class="px-4 py-3">Descrição</th>
                                    <th class="px-4 py-3">Categoria</th>
                                    <th class="px-4 py-3">Setor</th>
                                    <th class="px-4 py-3 rounded-r-lg text-right">Valor</th>
                                </tr>
                            </thead>
                            <tbody id="dash-table-recentes" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- RECEITAS (SÓCIOS) SECTION -->
            <section id="sec-receitas" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <h3 class="text-lg font-bold text-white mb-4">Registrar Pagamento de Sócio/Mensalidade</h3>
                    <form id="form-receita" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-4 items-end">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Nome do Sócio</label>
                            <input type="text" id="rec-nome" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Ex: João Silva">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Setor</label>
                            <div class="flex gap-2">
                                <select id="rec-setor" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('sectors', 'Digite o nome do novo Setor:', 'rec-setor')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap" title="Adicionar novo setor">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria (Pacote)</label>
                            <div class="flex gap-2">
                                <select id="rec-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('catIncome', 'Digite a nova Categoria de Sócio:', 'rec-categoria')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap" title="Adicionar nova categoria">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Valor (R$)</label>
                            <input type="number" id="rec-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="0.00">
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Mensalidades do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg">
                                <tr><th class="px-6 py-3">Data</th><th class="px-6 py-3">Sócio</th><th class="px-6 py-3">Setor</th><th class="px-6 py-3">Categoria</th><th class="px-6 py-3 text-right">Valor</th><th class="px-6 py-3 text-center">Ação</th></tr>
                            </thead>
                            <tbody id="lista-receitas" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- PRODUTOS SECTION -->
            <section id="sec-produtos" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <h3 class="text-lg font-bold text-white mb-4">Registrar Venda de Produto</h3>
                    <form id="form-produto" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-4 items-end">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Descrição do Produto</label>
                            <input type="text" id="prod-nome" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Ex: Camiseta Fitness M">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Setor</label>
                            <div class="flex gap-2">
                                <select id="prod-setor" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('sectors', 'Digite o nome do novo Setor:', 'prod-setor')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria (Produto)</label>
                            <div class="flex gap-2">
                                <select id="prod-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('catProduct', 'Digite a nova Categoria de Produto:', 'prod-categoria')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Valor Total (R$)</label>
                            <input type="number" id="prod-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="0.00">
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-indigo-500 hover:bg-indigo-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>
                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Vendas do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg">
                                <tr><th class="px-6 py-3">Data</th><th class="px-6 py-3">Produto</th><th class="px-6 py-3">Setor</th><th class="px-6 py-3">Categoria</th><th class="px-6 py-3 text-right">Valor</th><th class="px-6 py-3 text-center">Ação</th></tr>
                            </thead>
                            <tbody id="lista-produtos" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- DESPESAS SECTION -->
            <section id="sec-despesas" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <h3 class="text-lg font-bold text-white mb-4">Registrar Gastos/Despesas</h3>
                    <form id="form-despesa" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-4 items-end">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Descrição</label>
                            <input type="text" id="desp-desc" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Ex: Conta de Energia">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Setor</label>
                            <div class="flex gap-2">
                                <select id="desp-setor" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('sectors', 'Digite o nome do novo Setor:', 'desp-setor')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria (Despesa)</label>
                            <div class="flex gap-2">
                                <select id="desp-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('catExpense', 'Digite a nova Categoria de Despesa:', 'desp-categoria')" class="bg-brand-600 hover:bg-brand-500 text-white px-3 text-sm rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Valor (R$)</label>
                            <input type="number" id="desp-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="0.00">
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-rose-500 hover:bg-rose-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>
                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Gastos do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg">
                                <tr><th class="px-6 py-3">Data</th><th class="px-6 py-3">Descrição</th><th class="px-6 py-3">Setor</th><th class="px-6 py-3">Categoria</th><th class="px-6 py-3 text-right">Valor</th><th class="px-6 py-3 text-center">Ação</th></tr>
                            </thead>
                            <tbody id="lista-despesas" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- RELATÓRIOS SECTION -->
            <section id="sec-relatorios" class="page-section hidden">
                <div class="flex justify-end mb-6 no-print">
                    <button onclick="window.print()" class="bg-brand-600 hover:bg-brand-500 text-white px-6 py-2 rounded-lg flex items-center gap-2 transition-colors font-medium shadow-lg">
                        <i data-lucide="printer" class="w-5 h-5"></i> Imprimir Relatório
                    </button>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border p-8 print:border-none print:shadow-none print:bg-white" id="print-area">
                    <div class="text-center mb-8 border-b border-dark-border print:border-gray-300 pb-6">
                        <h2 id="rel-club-name" class="text-3xl font-bold text-white print:text-black uppercase">NOME DO CLUBE</h2>
                        <p id="rel-club-cnpj" class="text-gray-400 print:text-gray-600 mt-1">CNPJ: 00.000.000/0000-00</p>
                        
                        <div class="inline-block mt-4 px-6 py-2 bg-dark-bg print:bg-gray-100 border border-dark-border print:border-gray-300 rounded-xl">
                            <h3 class="text-lg font-semibold text-brand-400 print:text-black">
                                Relatório Financeiro: <span id="rel-mes-texto" class="text-white print:text-black font-bold"></span>
                            </h3>
                            <p class="text-sm text-gray-400 print:text-gray-700 mt-1">
                                Setor Selecionado: <strong id="rel-setor-texto" class="text-white print:text-black">Todos</strong>
                            </p>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-8">
                        <div>
                            <h4 class="font-bold text-emerald-400 print:text-black mb-3 border-b border-dark-border print:border-gray-300 pb-2 flex items-center gap-2"><i data-lucide="users" class="w-4 h-4"></i> Receitas de Sócios</h4>
                            <ul id="rel-resumo-receitas" class="space-y-2 text-sm text-gray-400 print:text-gray-700 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border print:border-gray-300 flex justify-between font-bold text-white print:text-black">
                                <span>Total Sócios:</span><span id="rel-total-receitas" class="text-emerald-400 print:text-black">R$ 0,00</span>
                            </div>
                        </div>
                        <div>
                            <h4 class="font-bold text-indigo-400 print:text-black mb-3 border-b border-dark-border print:border-gray-300 pb-2 flex items-center gap-2"><i data-lucide="shopping-bag" class="w-4 h-4"></i> Venda de Produtos</h4>
                            <ul id="rel-resumo-produtos" class="space-y-2 text-sm text-gray-400 print:text-gray-700 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border print:border-gray-300 flex justify-between font-bold text-white print:text-black">
                                <span>Total Produtos:</span><span id="rel-total-produtos" class="text-indigo-400 print:text-black">R$ 0,00</span>
                            </div>
                        </div>
                        <div>
                            <h4 class="font-bold text-rose-400 print:text-black mb-3 border-b border-dark-border print:border-gray-300 pb-2 flex items-center gap-2"><i data-lucide="trending-down" class="w-4 h-4"></i> Despesas</h4>
                            <ul id="rel-resumo-despesas" class="space-y-2 text-sm text-gray-400 print:text-gray-700 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border print:border-gray-300 flex justify-between font-bold text-white print:text-black">
                                <span>Total Gastos:</span><span id="rel-total-despesas" class="text-rose-400 print:text-black">R$ 0,00</span>
                            </div>
                        </div>
                    </div>

                    <div class="bg-dark-bg print:bg-white border border-dark-border print:border-gray-300 p-6 rounded-xl max-w-2xl mx-auto">
                        <h4 class="text-xl font-bold text-white print:text-black mb-6 text-center uppercase tracking-wider">Fechamento do Período</h4>
                        <div class="space-y-3 mb-6">
                            <div class="flex justify-between items-center text-gray-300 print:text-gray-800">
                                <span>(+) Receitas de Sócios:</span><span id="rel-fim-socios" class="font-medium">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300 print:text-gray-800">
                                <span>(+) Venda de Produtos:</span><span id="rel-fim-prod" class="font-medium">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300 print:text-gray-800 font-bold border-t border-dark-border print:border-gray-200 pt-2">
                                <span>(=) Faturamento Bruto Total:</span><span id="rel-fim-fat" class="text-white print:text-black">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300 print:text-gray-800 text-rose-400 print:text-red-600">
                                <span>(-) Custos Totais (Despesas):</span><span id="rel-fim-custos" class="font-medium">R$ 0,00</span>
                            </div>
                        </div>
                        <div class="flex justify-between items-center mb-6 text-brand-400 print:text-black font-bold text-2xl border-y border-dark-border print:border-gray-400 py-4">
                            <span>(=) Lucro Líquido Final:</span>
                            <span id="rel-fim-lucro">R$ 0,00</span>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div class="bg-dark-card print:bg-gray-100 p-4 rounded-lg text-center border border-dark-border print:border-gray-300">
                                <p class="text-sm text-gray-400 print:text-gray-600 mb-1">Pró-labore Sócio 1 (50%)</p>
                                <p id="rel-socio1" class="font-bold text-white print:text-black text-xl">R$ 0,00</p>
                            </div>
                            <div class="bg-dark-card print:bg-gray-100 p-4 rounded-lg text-center border border-dark-border print:border-gray-300">
                                <p class="text-sm text-gray-400 print:text-gray-600 mb-1">Pró-labore Sócio 2 (50%)</p>
                                <p id="rel-socio2" class="font-bold text-white print:text-black text-xl">R$ 0,00</p>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- CONFIGURAÇÕES SECTION -->
            <section id="sec-configuracoes" class="page-section hidden no-print">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    
                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2"><i data-lucide="building" class="w-5 h-5 text-brand-400"></i> Dados do Clube</h3>
                        <div class="space-y-4 mb-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-400 mb-1">Nome do Clube</label>
                                <input type="text" id="conf-nome" class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-brand-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-400 mb-1">CNPJ</label>
                                <input type="text" id="conf-cnpj" class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-brand-500 focus:outline-none">
                            </div>
                        </div>
                        <button onclick="salvarConfigGeral()" class="w-full bg-brand-600 hover:bg-brand-500 text-white font-medium py-2 rounded-lg transition-colors">Salvar Dados</button>
                    </div>

                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2"><i data-lucide="map-pin" class="w-5 h-5 text-amber-400"></i> Gerenciar Setores</h3>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="novo-setor" class="flex-1 bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:outline-none" placeholder="Ex: Sede Principal, Quadra">
                            <button onclick="addConfigItem('sectors', 'novo-setor')" class="bg-amber-500 text-white px-4 rounded-lg hover:bg-amber-600 font-bold">+</button>
                        </div>
                        <ul id="lista-conf-setores" class="space-y-2 max-h-40 overflow-y-auto"></ul>
                    </div>

                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2"><i data-lucide="users" class="w-5 h-5 text-emerald-400"></i> Categorias: Sócios</h3>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="nova-cat-rec" class="flex-1 bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:outline-none" placeholder="Ex: Associado Mensal">
                            <button onclick="addConfigItem('catIncome', 'nova-cat-rec')" class="bg-emerald-500 text-white px-4 rounded-lg hover:bg-emerald-600 font-bold">+</button>
                        </div>
                        <ul id="lista-conf-rec" class="space-y-2 max-h-40 overflow-y-auto"></ul>
                    </div>

                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2"><i data-lucide="shopping-bag" class="w-5 h-5 text-indigo-400"></i> Categorias: Produtos</h3>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="nova-cat-prod" class="flex-1 bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:outline-none" placeholder="Ex: Roupas, Bar">
                            <button onclick="addConfigItem('catProduct', 'nova-cat-prod')" class="bg-indigo-500 text-white px-4 rounded-lg hover:bg-indigo-600 font-bold">+</button>
                        </div>
                        <ul id="lista-conf-prod" class="space-y-2 max-h-40 overflow-y-auto"></ul>
                    </div>

                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6 md:col-span-2">
                        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2"><i data-lucide="trending-down" class="w-5 h-5 text-rose-400"></i> Categorias: Despesas</h3>
                        <div class="flex gap-2 mb-4 max-w-md">
                            <input type="text" id="nova-cat-desp" class="flex-1 bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:outline-none" placeholder="Ex: Manutenção, Água">
                            <button onclick="addConfigItem('catExpense', 'nova-cat-desp')" class="bg-rose-500 text-white px-4 rounded-lg hover:bg-rose-600 font-bold">+</button>
                        </div>
                        <ul id="lista-conf-desp" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-2 max-h-40 overflow-y-auto"></ul>
                    </div>
                </div>
            </section>
        </main>
    </div>

    <!-- Scripts (Firebase + Lógica) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithCustomToken, signInAnonymously, GoogleAuthProvider, signInWithPopup, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, addDoc, deleteDoc, doc, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "AIzaSyDOGZGAKNvAUiD7BfETTTXJz-aQ54AAweM",
            authDomain: "pow-fit-pay.firebaseapp.com",
            projectId: "pow-fit-pay",
            storageBucket: "pow-fit-pay.firebasestorage.app",
            messagingSenderId: "899659807167",
            appId: "1:899659807167:web:b8ca11726bff6cbcd6090a"
        };
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'pow-fit-pay';

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        let currentUser = null;
        let transactions = [];
        
        // Estado INICIAL exato pedido por você
        let appSettings = { 
            name: 'Nome do Clube', 
            cnpj: '',
            sectors: ['Sede Principal'],
            catIncome: ['Associado(a)', 'Associado Temporário (Visitante)'],
            catProduct: [], 
            catExpense: []  
        };

        let unsubTransactions = null;
        let unsubSettings = null;
        let currentMonthFilter = new Date().toISOString().slice(0, 7);
        let currentSectorFilter = 'Todos';

        const formatMoney = (val) => new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(val);
        const formatDate = (dateStr) => { const [y, m, d] = dateStr.split('-'); return `${d}/${m}/${y}`; };

        lucide.createIcons();

        document.getElementById('filtro-mes').value = currentMonthFilter;
        document.getElementById('filtro-mes').addEventListener('change', (e) => { currentMonthFilter = e.target.value; updateUI(); });
        document.getElementById('filtro-setor-global').addEventListener('change', (e) => { currentSectorFilter = e.target.value; updateUI(); });

        window.navigate = (targetId) => {
            document.querySelectorAll('.page-section').forEach(sec => sec.classList.add('hidden'));
            document.getElementById(`sec-${targetId}`).classList.remove('hidden');
            document.querySelectorAll('.nav-btn').forEach(btn => {
                if(btn.dataset.target === targetId) { btn.classList.add('bg-dark-border', 'text-white'); btn.classList.remove('text-gray-400'); } 
                else { btn.classList.remove('bg-dark-border', 'text-white'); btn.classList.add('text-gray-400'); }
            });
            const titles = { 'dashboard': 'Dashboard', 'receitas': 'Sócios / Mensalidades', 'produtos': 'Venda de Produtos', 'despesas': 'Despesas / Gastos', 'relatorios': 'Relatórios', 'configuracoes': 'Configurações' };
            document.getElementById('page-title').textContent = titles[targetId];
        };

        const initAuth = async () => {
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                await signInWithCustomToken(auth, __initial_auth_token);
            } else { await signInAnonymously(auth); }
        };

        document.getElementById('btn-google-login').addEventListener('click', async () => {
            const provider = new GoogleAuthProvider();
            try {
                document.getElementById('login-loading').classList.remove('hidden');
                await signInWithPopup(auth, provider);
            } catch (error) { console.error("Erro:", error); } 
            finally { document.getElementById('login-loading').classList.add('hidden'); }
        });

        document.getElementById('btn-logout').addEventListener('click', async () => { await signOut(auth); window.location.reload(); });
        document.getElementById('btn-logout-header').addEventListener('click', async () => { await signOut(auth); window.location.reload(); });

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                if(!user.isAnonymous) {
                    document.getElementById('user-name').textContent = user.displayName || "Usuário";
                    if(user.photoURL) {
                        const img = document.getElementById('user-avatar');
                        img.src = user.photoURL; img.classList.remove('hidden');
                        document.getElementById('user-avatar-fallback').classList.add('hidden');
                    }
                    const ls = document.getElementById('login-screen');
                    ls.classList.add('opacity-0', 'pointer-events-none');
                    setTimeout(() => ls.classList.add('hidden'), 300);
                    document.getElementById('app-container').classList.remove('hidden');
                }
                loadUserData();
            } else {
                currentUser = null;
                document.getElementById('login-screen').classList.remove('hidden', 'opacity-0', 'pointer-events-none');
                document.getElementById('app-container').classList.add('hidden');
            }
        });

        initAuth();

        const loadUserData = () => {
            if (!currentUser) return;
            const uid = currentUser.uid;
            
            if (unsubTransactions) unsubTransactions();
            if (unsubSettings) unsubSettings();

            const transRef = collection(db, 'artifacts', appId, 'users', uid, 'transactions');
            const settingsRef = doc(db, 'artifacts', appId, 'users', uid, 'settings', 'geral');

            unsubSettings = onSnapshot(settingsRef, (docSnap) => {
                if (docSnap.exists()) { appSettings = { ...appSettings, ...docSnap.data() }; }
                applySettingsToUI();
            }, (err) => console.error(err));

            unsubTransactions = onSnapshot(transRef, (snapshot) => {
                transactions = [];
                snapshot.forEach(doc => transactions.push({ id: doc.id, ...doc.data() }));
                transactions.sort((a, b) => new Date(b.date) - new Date(a.date) || b.timestamp - a.timestamp);
                updateUI();
            }, (err) => console.error(err));
        };

        const applySettingsToUI = () => {
            document.getElementById('club-header-name').textContent = appSettings.name;
            document.getElementById('rel-club-name').textContent = appSettings.name;
            document.getElementById('rel-club-cnpj').textContent = appSettings.cnpj ? `CNPJ: ${appSettings.cnpj}` : '';
            document.getElementById('conf-nome').value = appSettings.name || '';
            document.getElementById('conf-cnpj').value = appSettings.cnpj || '';

            const aviso = document.getElementById('aviso-config');
            if(!appSettings.sectors || appSettings.sectors.length === 0) aviso.classList.remove('hidden');
            else aviso.classList.add('hidden');

            updateSelectOptions('filtro-setor-global', ['Todos', ...(appSettings.sectors || [])]);
            updateSelectOptions('rec-setor', appSettings.sectors || []);
            updateSelectOptions('prod-setor', appSettings.sectors || []);
            updateSelectOptions('desp-setor', appSettings.sectors || []);

            updateSelectOptions('rec-categoria', appSettings.catIncome || []);
            updateSelectOptions('prod-categoria', appSettings.catProduct || []);
            updateSelectOptions('desp-categoria', appSettings.catExpense || []);

            document.getElementById('filtro-setor-global').value = currentSectorFilter;
            renderConfigLists();
            updateUI();
        };

        const updateSelectOptions = (elementId, optionsArray) => {
            const el = document.getElementById(elementId);
            const val = el.value; 
            el.innerHTML = optionsArray.length === 0 ? '<option value="" disabled selected>Clique em + Novo</option>' : '';
            optionsArray.forEach(opt => {
                if(opt === 'Todos') el.innerHTML += `<option value="${opt}">Todos os Setores</option>`;
                else el.innerHTML += `<option value="${opt}">${opt}</option>`;
            });
            if(optionsArray.includes(val)) el.value = val;
        };

        const updateUI = () => {
            const txFiltradas = transactions.filter(t => t.date.startsWith(currentMonthFilter) && (currentSectorFilter === 'Todos' || t.sector === currentSectorFilter));
            let totRec = 0, totProd = 0, totDesp = 0;
            const resRec = {}, resProd = {}, resDesp = {};
            const htmlRec = [], htmlProd = [], htmlDesp = [], htmlDash = [];

            txFiltradas.forEach(t => {
                const trBase = (color) => `<tr class="hover:bg-dark-border/50"><td class="px-6 py-3 text-gray-400 whitespace-nowrap">${formatDate(t.date)}</td><td class="px-6 py-3 font-medium text-white">${t.description}</td><td class="px-6 py-3 text-gray-400">${t.sector || '-'}</td><td class="px-6 py-3 text-${color}-400">${t.category}</td><td class="px-6 py-3 text-right font-bold text-${color}-400">${formatMoney(t.amount)}</td><td class="px-6 py-3 text-center"><button onclick="deletarTransacao('${t.id}')" class="text-rose-400 hover:text-rose-300"><i data-lucide="trash-2" class="w-4 h-4"></i></button></td></tr>`;
                if (t.type === 'income') { totRec += t.amount; resRec[t.category] = (resRec[t.category] || 0) + t.amount; htmlRec.push(trBase('emerald')); }
                else if (t.type === 'product') { totProd += t.amount; resProd[t.category] = (resProd[t.category] || 0) + t.amount; htmlProd.push(trBase('indigo')); }
                else if (t.type === 'expense') { totDesp += t.amount; resDesp[t.category] = (resDesp[t.category] || 0) + t.amount; htmlDesp.push(trBase('rose')); }
            });

            txFiltradas.slice(0, 8).forEach(t => {
                const colors = { income: 'emerald', product: 'indigo', expense: 'rose' };
                const c = colors[t.type];
                const typeName = t.type === 'income' ? 'Sócio' : t.type === 'product' ? 'Produto' : 'Despesa';
                htmlDash.push(`<tr class="border-b border-dark-border/50 last:border-0"><td class="px-4 py-3 text-gray-400 text-xs uppercase">${typeName}</td><td class="px-4 py-3 font-medium text-white">${t.description}</td><td class="px-4 py-3 text-gray-400">${t.category}</td><td class="px-4 py-3 text-gray-400 text-xs">${t.sector || '-'}</td><td class="px-4 py-3 text-right font-bold text-${c}-400">${formatMoney(t.amount)}</td></tr>`);
            });

            const lucroLiquido = (totRec + totProd) - totDesp;
            const proLabore = lucroLiquido > 0 ? lucroLiquido / 2 : 0;

            document.getElementById('dash-faturamento-socios').textContent = formatMoney(totRec);
            document.getElementById('dash-faturamento-produtos').textContent = formatMoney(totProd);
            document.getElementById('dash-despesas').textContent = formatMoney(totDesp);
            document.getElementById('dash-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('dash-lucro').className = `text-2xl font-bold ${lucroLiquido >= 0 ? 'text-brand-400' : 'text-rose-400'}`;
            document.getElementById('dash-socio1').textContent = formatMoney(proLabore);
            document.getElementById('dash-socio2').textContent = formatMoney(proLabore);

            document.getElementById('dash-table-recentes').innerHTML = htmlDash.join('') || '<tr><td colspan="5" class="px-4 py-3 text-center text-gray-500">Vazio.</td></tr>';
            document.getElementById('lista-receitas').innerHTML = htmlRec.join('') || '<tr><td colspan="6" class="px-6 py-4 text-center text-gray-500">Vazio.</td></tr>';
            document.getElementById('lista-produtos').innerHTML = htmlProd.join('') || '<tr><td colspan="6" class="px-6 py-4 text-center text-gray-500">Vazio.</td></tr>';
            document.getElementById('lista-despesas').innerHTML = htmlDesp.join('') || '<tr><td colspan="6" class="px-6 py-4 text-center text-gray-500">Vazio.</td></tr>';

            document.getElementById('rel-mes-texto').textContent = currentMonthFilter.split('-').reverse().join('/');
            document.getElementById('rel-setor-texto').textContent = currentSectorFilter === 'Todos' ? 'Todos os Setores' : currentSectorFilter;
            
            const renderResumo = (obj, idList, idTotal, totalVal) => {
                let h = '';
                for(let k in obj) h += `<li class="flex justify-between border-b border-dark-border/30 print:border-gray-200 pb-1"><span>${k}</span> <strong>${formatMoney(obj[k])}</strong></li>`;
                document.getElementById(idList).innerHTML = h || '<li>Sem dados</li>';
                document.getElementById(idTotal).textContent = formatMoney(totalVal);
            };

            renderResumo(resRec, 'rel-resumo-receitas', 'rel-total-receitas', totRec);
            renderResumo(resProd, 'rel-resumo-produtos', 'rel-total-produtos', totProd);
            renderResumo(resDesp, 'rel-resumo-despesas', 'rel-total-despesas', totDesp);

            document.getElementById('rel-fim-socios').textContent = formatMoney(totRec);
            document.getElementById('rel-fim-prod').textContent = formatMoney(totProd);
            document.getElementById('rel-fim-fat').textContent = formatMoney(totRec + totProd);
            document.getElementById('rel-fim-custos').textContent = formatMoney(totDesp);
            document.getElementById('rel-fim-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('rel-socio1').textContent = formatMoney(proLabore);
            document.getElementById('rel-socio2').textContent = formatMoney(proLabore);
        };

        const handleFormSubmit = async (e, type, prefix) => {
            e.preventDefault();
            if (!currentUser) return;
            
            const desc = document.getElementById(`${prefix}-nome`)?.value || document.getElementById(`${prefix}-desc`)?.value;
            const catEl = document.getElementById(`${prefix}-categoria`);
            const setEl = document.getElementById(`${prefix}-setor`);
            
            if(!catEl.value || !setEl.value) { 
                alert("Por favor, selecione (ou crie usando o botão + Novo) a Categoria e o Setor."); 
                return; 
            }

            try {
                await addDoc(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions'), {
                    type: type, description: desc, category: catEl.value, sector: setEl.value, amount: parseFloat(document.getElementById(`${prefix}-valor`).value),
                    date: new Date().toISOString().split('T')[0], timestamp: Date.now()
                });
                e.target.reset();
            } catch (err) { console.error("Erro add:", err); }
        };

        document.getElementById('form-receita').addEventListener('submit', e => handleFormSubmit(e, 'income', 'rec'));
        document.getElementById('form-produto').addEventListener('submit', e => handleFormSubmit(e, 'product', 'prod'));
        document.getElementById('form-despesa').addEventListener('submit', e => handleFormSubmit(e, 'expense', 'desp'));

        window.deletarTransacao = async (id) => {
            if (currentUser && confirm('Apagar este registro?')) await deleteDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions', id));
        };

        const saveSettingsToFirebase = async () => {
            await setDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'settings', 'geral'), appSettings);
        };

        window.salvarConfigGeral = async () => {
            if(!currentUser) return;
            appSettings.name = document.getElementById('conf-nome').value;
            appSettings.cnpj = document.getElementById('conf-cnpj').value;
            await saveSettingsToFirebase();
            alert("Dados salvos!");
        };

        // Função Global de Adição Rápida com Seleção Automática
        window.quickAddConfig = async (listKey, promptText, selectId) => {
            if(!currentUser) return;
            const val = prompt(promptText);
            if(val && val.trim() !== '') {
                const item = val.trim();
                if(!appSettings[listKey]) appSettings[listKey] = [];
                
                if(!appSettings[listKey].includes(item)) {
                    appSettings[listKey].push(item);
                    await saveSettingsToFirebase();
                    
                    // Tenta selecionar automaticamente o novo item no menu dropdown
                    let tentativas = 0;
                    let checkInterval = setInterval(() => {
                        const el = document.getElementById(selectId);
                        if(el && Array.from(el.options).some(opt => opt.value === item)) {
                            el.value = item;
                            clearInterval(checkInterval);
                        }
                        tentativas++;
                        if(tentativas > 20) clearInterval(checkInterval); // Cancela após 2 segundos
                    }, 100);

                } else {
                    alert('Esta opção já existe na lista.');
                }
            }
        };

        window.addConfigItem = async (listKey, inputId) => {
            const val = document.getElementById(inputId).value.trim();
            if(val) {
                if(!appSettings[listKey]) appSettings[listKey] = [];
                if(!appSettings[listKey].includes(val)) {
                    appSettings[listKey].push(val);
                    document.getElementById(inputId).value = '';
                    await saveSettingsToFirebase();
                }
            }
        };

        window.remConfigItem = async (listKey, index) => {
            if(confirm('Remover este item?')) {
                appSettings[listKey].splice(index, 1);
                await saveSettingsToFirebase();
            }
        };

        const renderConfigLists = () => {
            const render = (listKey, elementId) => {
                const arr = appSettings[listKey] || [];
                document.getElementById(elementId).innerHTML = arr.map((item, idx) => `<li class="flex justify-between items-center bg-dark-bg px-3 py-2 rounded-lg border border-dark-border"><span class="text-gray-300 text-sm">${item}</span><button onclick="remConfigItem('${listKey}', ${idx})" class="text-rose-400 hover:text-rose-500"><i data-lucide="x" class="w-4 h-4"></i></button></li>`).join('');
            };
            render('sectors', 'lista-conf-setores');
            render('catIncome', 'lista-conf-rec');
            render('catProduct', 'lista-conf-prod');
            render('catExpense', 'lista-conf-desp');
            lucide.createIcons();
        };

        navigate('dashboard');
    </script>
</body>
</html>
