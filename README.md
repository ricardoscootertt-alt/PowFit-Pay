<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="color-scheme" content="dark only"> <!-- Bloqueia navegadores de inverterem o tema -->
    <meta name="theme-color" content="#0f172a">
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
        :root { color-scheme: dark only !important; }
        body { font-family: 'Inter', sans-serif; background-color: #0f172a !important; color: #e2e8f0 !important; }
        
        /* ---------------------------------------------------
           CORREÇÃO DEFINITIVA PARA TABELAS NO MOBILE 
           Força o fundo escuro contra qualquer navegador
           --------------------------------------------------- */
        table, .overflow-x-auto { background-color: #0f172a !important; border-color: #334155 !important; }
        thead, th { background-color: #1e293b !important; color: #94a3b8 !important; border-color: #334155 !important; }
        tbody, tr { background-color: #0f172a !important; border-color: #334155 !important; }
        td { background-color: #0f172a !important; border-color: #334155 !important; color: #e2e8f0 !important; }
        
        /* Efeito de hover nas linhas */
        tr:hover, tr:hover td { background-color: #1e293b !important; }

        /* Proteção das cores dos textos para não ficarem brancas/invisíveis */
        td.text-emerald-400, .text-emerald-400 { color: #34d399 !important; }
        td.text-indigo-400, .text-indigo-400 { color: #818cf8 !important; }
        td.text-rose-400, .text-rose-400 { color: #fb7185 !important; }
        td.text-blue-400, .text-blue-400 { color: #60a5fa !important; }
        td.text-white, .text-white { color: #ffffff !important; }
        td.text-gray-400, .text-gray-400 { color: #94a3b8 !important; }
        
        @media print {
            body { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; background-color: #0f172a !important; }
            .no-print { display: none !important; }
            .print-container { width: 100% !important; margin: 0 !important; padding: 0 !important; }
            * { box-shadow: none !important; }
            #print-area { background-color: #0f172a !important; border-color: #334155 !important; }
        }

        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }
    </style>
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="bg-dark-bg text-gray-200 min-h-screen flex antialiased">

    <!-- MODAL DARK DE CONFIRMAÇÃO (Para excluir ou alertas) -->
    <div id="custom-confirm-modal" class="hidden fixed inset-0 bg-dark-bg/90 backdrop-blur-sm z-50 flex items-center justify-center transition-opacity duration-300">
        <div class="bg-dark-card p-6 rounded-2xl shadow-2xl border border-dark-border max-w-sm w-full mx-4">
            <h3 id="custom-confirm-title" class="text-lg font-bold text-white mb-2">Confirmar</h3>
            <p id="custom-confirm-msg" class="text-gray-400 mb-6 text-sm">Tem certeza disso?</p>
            <div class="flex justify-end gap-3">
                <button id="custom-confirm-btn-cancel" class="px-4 py-2 rounded-lg text-gray-400 hover:text-white hover:bg-dark-border transition-colors text-sm font-medium">Cancelar</button>
                <button id="custom-confirm-btn-ok" class="px-4 py-2 rounded-lg bg-rose-600 hover:bg-rose-500 text-white transition-colors text-sm font-medium">Confirmar</button>
            </div>
        </div>
    </div>

    <!-- MODAL DARK DE PROMPT (Para criar categorias/setores) -->
    <div id="custom-prompt-modal" class="hidden fixed inset-0 bg-dark-bg/90 backdrop-blur-sm z-50 flex items-center justify-center transition-opacity duration-300">
        <div class="bg-dark-card p-6 rounded-2xl shadow-2xl border border-dark-border max-w-sm w-full mx-4">
            <h3 id="custom-prompt-title" class="text-lg font-bold text-white mb-4">Digite o valor:</h3>
            <input type="text" id="custom-prompt-input" class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none mb-6" placeholder="...">
            <div class="flex justify-end gap-3">
                <button id="custom-prompt-btn-cancel" class="px-4 py-2 rounded-lg text-gray-400 hover:text-white hover:bg-dark-border transition-colors text-sm font-medium">Cancelar</button>
                <button id="custom-prompt-btn-ok" class="px-4 py-2 rounded-lg bg-brand-600 hover:bg-brand-500 text-white transition-colors text-sm font-medium">Confirmar</button>
            </div>
        </div>
    </div>

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
            
            <button id="btn-google-login" class="w-full flex items-center justify-center gap-3 bg-dark-bg border border-dark-border text-white font-semibold py-3 px-4 rounded-xl hover:bg-dark-border transition-colors shadow-md">
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
                                <p class="text-gray-400 text-sm font-medium mb-1">Venda Produtos Bruta</p>
                                <h3 id="dash-faturamento-produtos" class="text-2xl font-bold text-indigo-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-indigo-400/10 rounded-lg"><i data-lucide="shopping-bag" class="w-5 h-5 text-indigo-400"></i></div>
                        </div>
                    </div>
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border relative group">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1 cursor-help border-b border-dashed border-gray-500 inline-block">Gastos Totais <span class="text-xs text-gray-500">(?)</span></p>
                                <h3 id="dash-despesas" class="text-2xl font-bold text-rose-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-rose-400/10 rounded-lg"><i data-lucide="trending-down" class="w-5 h-5 text-rose-400"></i></div>
                        </div>
                        <div class="absolute bottom-full left-0 mb-2 w-64 p-3 bg-gray-800 text-xs text-gray-200 rounded-lg shadow-xl opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all z-10 border border-gray-700">
                            Inclui Despesas Gerais + Custo de aquisição dos Produtos Vendidos.
                        </div>
                    </div>
                    <div class="bg-dark-card p-6 rounded-2xl border border-brand-500/30 shadow-[0_0_15px_rgba(59,130,246,0.1)]">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Lucro Líquido Real</p>
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
                    <div class="overflow-x-auto rounded-lg border border-dark-border">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs uppercase">
                                <tr>
                                    <th class="px-4 py-3 rounded-tl-lg">Tipo</th>
                                    <th class="px-4 py-3">Descrição</th>
                                    <th class="px-4 py-3">Categoria</th>
                                    <th class="px-4 py-3">Setor</th>
                                    <th class="px-4 py-3 rounded-tr-lg text-right">Valor Final</th>
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
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-bold text-white" id="rec-form-title">Registrar Pagamento de Sócio</h3>
                        <button type="button" id="btn-cancel-edit-rec" class="hidden text-gray-400 hover:text-white text-sm" onclick="cancelEdit('rec')">Cancelar Edição</button>
                    </div>
                    <form id="form-receita" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-4 items-end">
                        <input type="hidden" id="rec-id">
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
                            <button type="submit" id="btn-submit-rec" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Mensalidades do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs uppercase">
                                <tr><th class="px-6 py-3">Data</th><th class="px-6 py-3">Sócio</th><th class="px-6 py-3">Setor</th><th class="px-6 py-3">Categoria</th><th class="px-6 py-3 text-right">Valor</th><th class="px-6 py-3 text-center">Ações</th></tr>
                            </thead>
                            <tbody id="lista-receitas" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- PRODUTOS SECTION -->
            <section id="sec-produtos" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-bold text-white" id="prod-form-title">Registrar Venda de Produto</h3>
                        <button type="button" id="btn-cancel-edit-prod" class="hidden text-gray-400 hover:text-white text-sm" onclick="cancelEdit('prod')">Cancelar Edição</button>
                    </div>
                    <form id="form-produto" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-7 gap-4 items-end">
                        <input type="hidden" id="prod-id">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Descrição do Produto</label>
                            <input type="text" id="prod-nome" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Ex: Camiseta Fitness M">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Setor</label>
                            <div class="flex gap-2">
                                <select id="prod-setor" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('sectors', 'Digite o nome do novo Setor:', 'prod-setor')" class="bg-brand-600 hover:bg-brand-500 text-white px-2 text-xs rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria</label>
                            <div class="flex gap-2">
                                <select id="prod-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-2 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none"></select>
                                <button type="button" onclick="quickAddConfig('catProduct', 'Digite a nova Categoria de Produto:', 'prod-categoria')" class="bg-brand-600 hover:bg-brand-500 text-white px-2 text-xs rounded-lg font-bold transition-colors whitespace-nowrap">+ Novo</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Custo (R$)</label>
                            <input type="number" id="prod-custo" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Custo Pago">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Venda (R$)</label>
                            <input type="number" id="prod-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Valor Cobrado">
                        </div>
                        <div>
                            <button type="submit" id="btn-submit-prod" class="w-full bg-indigo-500 hover:bg-indigo-600 text-white font-medium py-2 px-2 rounded-lg transition-colors flex items-center justify-center gap-1 shadow-lg text-sm">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>
                
                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Vendas do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs uppercase">
                                <tr>
                                    <th class="px-6 py-3">Data</th>
                                    <th class="px-6 py-3">Produto</th>
                                    <th class="px-6 py-3">Setor</th>
                                    <th class="px-6 py-3">Categoria</th>
                                    <th class="px-6 py-3 text-right">Custo</th>
                                    <th class="px-6 py-3 text-right">Venda</th>
                                    <th class="px-6 py-3 text-right">Lucro Un.</th>
                                    <th class="px-6 py-3 text-center">Ações</th>
                                </tr>
                            </thead>
                            <tbody id="lista-produtos" class="divide-y divide-dark-border"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- DESPESAS SECTION -->
            <section id="sec-despesas" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-bold text-white" id="desp-form-title">Registrar Gastos Gerais/Despesas</h3>
                        <button type="button" id="btn-cancel-edit-desp" class="hidden text-gray-400 hover:text-white text-sm" onclick="cancelEdit('desp')">Cancelar Edição</button>
                    </div>
                    <form id="form-despesa" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-4 items-end">
                        <input type="hidden" id="desp-id">
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
                            <button type="submit" id="btn-submit-desp" class="w-full bg-rose-500 hover:bg-rose-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg">
                                <i data-lucide="check" class="w-4 h-4"></i> Salvar
                            </button>
                        </div>
                    </form>
                </div>
                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border"><h3 class="font-bold text-white">Gastos do Mês</h3></div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs uppercase">
                                <tr><th class="px-6 py-3">Data</th><th class="px-6 py-3">Descrição</th><th class="px-6 py-3">Setor</th><th class="px-6 py-3">Categoria</th><th class="px-6 py-3 text-right">Valor</th><th class="px-6 py-3 text-center">Ações</th></tr>
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

                <div class="bg-dark-card rounded-2xl border border-dark-border p-8" id="print-area">
                    <div class="text-center mb-8 border-b border-dark-border pb-6">
                        <h2 id="rel-club-name" class="text-3xl font-bold text-white uppercase">NOME DO CLUBE</h2>
                        <p id="rel-club-cnpj" class="text-gray-400 mt-1">CNPJ: 00.000.000/0000-00</p>
                        
                        <div class="inline-block mt-4 px-6 py-2 bg-dark-bg border border-dark-border rounded-xl">
                            <h3 class="text-lg font-semibold text-brand-400">
                                Relatório Financeiro: <span id="rel-mes-texto" class="text-white font-bold"></span>
                            </h3>
                            <p class="text-sm text-gray-400 mt-1">
                                Setor Selecionado: <strong id="rel-setor-texto" class="text-white">Todos</strong>
                            </p>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-8">
                        <div>
                            <h4 class="font-bold text-emerald-400 mb-3 border-b border-dark-border pb-2 flex items-center gap-2"><i data-lucide="users" class="w-4 h-4"></i> Receitas de Sócios</h4>
                            <ul id="rel-resumo-receitas" class="space-y-2 text-sm text-gray-400 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border flex justify-between font-bold text-white">
                                <span>Total Sócios:</span><span id="rel-total-receitas" class="text-emerald-400">R$ 0,00</span>
                            </div>
                        </div>
                        <div>
                            <h4 class="font-bold text-indigo-400 mb-3 border-b border-dark-border pb-2 flex items-center gap-2"><i data-lucide="shopping-bag" class="w-4 h-4"></i> Venda Bruta de Produtos</h4>
                            <ul id="rel-resumo-produtos" class="space-y-2 text-sm text-gray-400 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border flex justify-between font-bold text-white">
                                <span>Total Produtos:</span><span id="rel-total-produtos" class="text-indigo-400">R$ 0,00</span>
                            </div>
                        </div>
                        <div>
                            <h4 class="font-bold text-rose-400 mb-3 border-b border-dark-border pb-2 flex items-center gap-2"><i data-lucide="trending-down" class="w-4 h-4"></i> Despesas Gerais</h4>
                            <ul id="rel-resumo-despesas" class="space-y-2 text-sm text-gray-400 mb-4"></ul>
                            <div class="pt-2 border-t border-dark-border flex justify-between font-bold text-white">
                                <span>Total Gastos:</span><span id="rel-total-despesas" class="text-rose-400">R$ 0,00</span>
                            </div>
                        </div>
                    </div>

                    <div class="bg-dark-bg border border-dark-border p-6 rounded-xl max-w-2xl mx-auto">
                        <h4 class="text-xl font-bold text-white mb-6 text-center uppercase tracking-wider">Cálculo Completo de Lucro</h4>
                        <div class="space-y-3 mb-6">
                            <div class="flex justify-between items-center text-gray-300">
                                <span>(+) Receitas de Sócios:</span><span id="rel-fim-socios" class="font-medium text-emerald-400">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300">
                                <span>(+) Venda de Produtos (Bruto):</span><span id="rel-fim-prod" class="font-medium text-indigo-400">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300 font-bold border-t border-dark-border pt-2 pb-2">
                                <span>(=) Faturamento Bruto Total:</span><span id="rel-fim-fat" class="text-white">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300">
                                <span>(-) Despesas Gerais:</span><span id="rel-fim-custos" class="font-medium text-rose-400">R$ 0,00</span>
                            </div>
                            <div class="flex justify-between items-center text-gray-300 border-b border-dark-border pb-2">
                                <span>(-) Custo dos Produtos Vendidos (CMV):</span><span id="rel-fim-custos-prod" class="font-medium text-rose-400">R$ 0,00</span>
                            </div>
                        </div>
                        <div class="flex justify-between items-center mb-6 text-brand-400 font-bold text-2xl border-y border-dark-border py-4">
                            <span>(=) Lucro Líquido Real:</span>
                            <span id="rel-fim-lucro">R$ 0,00</span>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div class="bg-dark-card p-4 rounded-lg text-center border border-dark-border">
                                <p class="text-sm text-gray-400 mb-1">Pró-labore Sócio 1 (50%)</p>
                                <p id="rel-socio1" class="font-bold text-white text-xl">R$ 0,00</p>
                            </div>
                            <div class="bg-dark-card p-4 rounded-lg text-center border border-dark-border">
                                <p class="text-sm text-gray-400 mb-1">Pró-labore Sócio 2 (50%)</p>
                                <p id="rel-socio2" class="font-bold text-white text-xl">R$ 0,00</p>
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
        import { getFirestore, collection, onSnapshot, addDoc, updateDoc, deleteDoc, doc, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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

        window.customPrompt = (message) => {
            return new Promise((resolve) => {
                const modal = document.getElementById('custom-prompt-modal');
                const title = document.getElementById('custom-prompt-title');
                const input = document.getElementById('custom-prompt-input');
                const btnOk = document.getElementById('custom-prompt-btn-ok');
                const btnCancel = document.getElementById('custom-prompt-btn-cancel');

                title.textContent = message;
                input.value = '';
                modal.classList.remove('hidden');
                input.focus();

                const cleanup = () => {
                    btnOk.onclick = null; btnCancel.onclick = null; input.onkeydown = null;
                    modal.classList.add('hidden');
                };

                btnOk.onclick = () => { resolve(input.value); cleanup(); };
                btnCancel.onclick = () => { resolve(null); cleanup(); };
                input.onkeydown = (e) => {
                    if (e.key === 'Enter') { resolve(input.value); cleanup(); }
                    if (e.key === 'Escape') { resolve(null); cleanup(); }
                };
            });
        };

        window.customConfirm = (titleText, msgText) => {
            return new Promise((resolve) => {
                const modal = document.getElementById('custom-confirm-modal');
                const title = document.getElementById('custom-confirm-title');
                const msg = document.getElementById('custom-confirm-msg');
                const btnOk = document.getElementById('custom-confirm-btn-ok');
                const btnCancel = document.getElementById('custom-confirm-btn-cancel');

                title.textContent = titleText;
                msg.textContent = msgText;
                modal.classList.remove('hidden');

                const cleanup = () => {
                    btnOk.onclick = null; btnCancel.onclick = null;
                    modal.classList.add('hidden');
                };

                btnOk.onclick = () => { resolve(true); cleanup(); };
                btnCancel.onclick = () => { resolve(false); cleanup(); };
            });
        };

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
            
            let totRec = 0, totProdVenda = 0, totProdCusto = 0, totDesp = 0;
            const resRec = {}, resProd = {}, resDesp = {};
            const htmlRec = [], htmlProd = [], htmlDesp = [], htmlDash = [];

            txFiltradas.forEach(t => {
                const actionButtons = `
                    <div class="flex justify-center gap-2">
                        <button onclick="editarTransacao('${t.id}')" class="text-blue-400 hover:text-blue-300 p-1" title="Editar"><i data-lucide="edit" class="w-4 h-4"></i></button>
                        <button onclick="deletarTransacao('${t.id}')" class="text-rose-400 hover:text-rose-300 p-1" title="Excluir"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                    </div>`;

                if (t.type === 'income') { 
                    totRec += t.amount; resRec[t.category] = (resRec[t.category] || 0) + t.amount; 
                    htmlRec.push(`<tr><td class="px-6 py-3 text-gray-400 whitespace-nowrap">${formatDate(t.date)}</td><td class="px-6 py-3 font-medium text-white">${t.description}</td><td class="px-6 py-3 text-gray-400">${t.sector || '-'}</td><td class="px-6 py-3 text-emerald-400">${t.category}</td><td class="px-6 py-3 text-right font-bold text-emerald-400">${formatMoney(t.amount)}</td><td class="px-6 py-3">${actionButtons}</td></tr>`); 
                }
                else if (t.type === 'product') { 
                    const custo = t.costAmount || 0; const lucroItem = t.amount - custo;
                    totProdVenda += t.amount; totProdCusto += custo;
                    resProd[t.category] = (resProd[t.category] || 0) + t.amount; 
                    htmlProd.push(`<tr><td class="px-6 py-3 text-gray-400 whitespace-nowrap">${formatDate(t.date)}</td><td class="px-6 py-3 font-medium text-white">${t.description}</td><td class="px-6 py-3 text-gray-400">${t.sector || '-'}</td><td class="px-6 py-3 text-indigo-400">${t.category}</td><td class="px-6 py-3 text-right text-rose-400">${formatMoney(custo)}</td><td class="px-6 py-3 text-right font-bold text-indigo-400">${formatMoney(t.amount)}</td><td class="px-6 py-3 text-right font-bold text-emerald-400">${formatMoney(lucroItem)}</td><td class="px-6 py-3">${actionButtons}</td></tr>`); 
                }
                else if (t.type === 'expense') { 
                    totDesp += t.amount; resDesp[t.category] = (resDesp[t.category] || 0) + t.amount; 
                    htmlDesp.push(`<tr><td class="px-6 py-3 text-gray-400 whitespace-nowrap">${formatDate(t.date)}</td><td class="px-6 py-3 font-medium text-white">${t.description}</td><td class="px-6 py-3 text-gray-400">${t.sector || '-'}</td><td class="px-6 py-3 text-rose-400">${t.category}</td><td class="px-6 py-3 text-right font-bold text-rose-400">${formatMoney(t.amount)}</td><td class="px-6 py-3">${actionButtons}</td></tr>`); 
                }
            });

            txFiltradas.slice(0, 8).forEach(t => {
                const colors = { income: 'emerald', product: 'indigo', expense: 'rose' };
                const c = colors[t.type];
                const typeName = t.type === 'income' ? 'Sócio' : t.type === 'product' ? 'Produto' : 'Despesa';
                htmlDash.push(`<tr><td class="px-4 py-3 text-gray-400 text-xs uppercase">${typeName}</td><td class="px-4 py-3 font-medium text-white">${t.description}</td><td class="px-4 py-3 text-gray-400">${t.category}</td><td class="px-4 py-3 text-gray-400 text-xs">${t.sector || '-'}</td><td class="px-4 py-3 text-right font-bold text-${c}-400">${formatMoney(t.amount)}</td></tr>`);
            });

            const faturamentoBruto = totRec + totProdVenda;
            const custosTotais = totDesp + totProdCusto;
            const lucroLiquido = faturamentoBruto - custosTotais;
            const proLabore = lucroLiquido > 0 ? lucroLiquido / 2 : 0;

            document.getElementById('dash-faturamento-socios').textContent = formatMoney(totRec);
            document.getElementById('dash-faturamento-produtos').textContent = formatMoney(totProdVenda);
            document.getElementById('dash-despesas').textContent = formatMoney(custosTotais); 
            document.getElementById('dash-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('dash-lucro').className = `text-2xl font-bold ${lucroLiquido >= 0 ? 'text-brand-400' : 'text-rose-400'}`;
            document.getElementById('dash-socio1').textContent = formatMoney(proLabore);
            document.getElementById('dash-socio2').textContent = formatMoney(proLabore);

            document.getElementById('dash-table-recentes').innerHTML = htmlDash.join('') || '<tr><td colspan="5" class="px-4 py-3 text-center text-gray-500">Nenhum registro encontrado.</td></tr>';
            document.getElementById('lista-receitas').innerHTML = htmlRec.join('') || '<tr><td colspan="6" class="px-6 py-4 text-center text-gray-500">Nenhum registro encontrado.</td></tr>';
            document.getElementById('lista-produtos').innerHTML = htmlProd.join('') || '<tr><td colspan="8" class="px-6 py-4 text-center text-gray-500">Nenhum registro encontrado.</td></tr>';
            document.getElementById('lista-despesas').innerHTML = htmlDesp.join('') || '<tr><td colspan="6" class="px-6 py-4 text-center text-gray-500">Nenhum registro encontrado.</td></tr>';

            document.getElementById('rel-mes-texto').textContent = currentMonthFilter.split('-').reverse().join('/');
            document.getElementById('rel-setor-texto').textContent = currentSectorFilter === 'Todos' ? 'Todos os Setores' : currentSectorFilter;
            
            const renderResumo = (obj, idList, idTotal, totalVal) => {
                let h = '';
                for(let k in obj) h += `<li class="flex justify-between border-b border-dark-border/30 pb-1 text-gray-400"><span>${k}</span> <strong class="text-white">${formatMoney(obj[k])}</strong></li>`;
                document.getElementById(idList).innerHTML = h || '<li class="text-gray-500">Sem dados</li>';
                document.getElementById(idTotal).textContent = formatMoney(totalVal);
            };

            renderResumo(resRec, 'rel-resumo-receitas', 'rel-total-receitas', totRec);
            renderResumo(resProd, 'rel-resumo-produtos', 'rel-total-produtos', totProdVenda);
            renderResumo(resDesp, 'rel-resumo-despesas', 'rel-total-despesas', totDesp);

            document.getElementById('rel-fim-socios').textContent = formatMoney(totRec);
            document.getElementById('rel-fim-prod').textContent = formatMoney(totProdVenda);
            document.getElementById('rel-fim-fat').textContent = formatMoney(faturamentoBruto);
            document.getElementById('rel-fim-custos').textContent = formatMoney(totDesp);
            document.getElementById('rel-fim-custos-prod').textContent = formatMoney(totProdCusto);
            document.getElementById('rel-fim-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('rel-socio1').textContent = formatMoney(proLabore);
            document.getElementById('rel-socio2').textContent = formatMoney(proLabore);

            lucide.createIcons();
        };

        const handleFormSubmit = async (e, type, prefix) => {
            e.preventDefault();
            if (!currentUser) return;
            
            const idToEdit = document.getElementById(`${prefix}-id`).value;
            const desc = document.getElementById(`${prefix}-nome`)?.value || document.getElementById(`${prefix}-desc`)?.value;
            const catEl = document.getElementById(`${prefix}-categoria`);
            const setEl = document.getElementById(`${prefix}-setor`);
            
            if(!catEl.value || !setEl.value) { 
                await window.customConfirm("Atenção", "Por favor, selecione a Categoria e o Setor."); 
                return; 
            }

            const payload = {
                type: type, 
                description: desc, 
                category: catEl.value, 
                sector: setEl.value, 
                amount: parseFloat(document.getElementById(`${prefix}-valor`).value)
            };

            if (type === 'product') {
                const custo = parseFloat(document.getElementById(`${prefix}-custo`).value);
                payload.costAmount = isNaN(custo) ? 0 : custo;
            }

            try {
                if (idToEdit) {
                    const ref = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions', idToEdit);
                    await updateDoc(ref, payload);
                    cancelEdit(prefix); 
                } else {
                    payload.date = new Date().toISOString().split('T')[0];
                    payload.timestamp = Date.now();
                    await addDoc(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions'), payload);
                    e.target.reset();
                }
            } catch (err) { console.error("Erro submit:", err); }
        };

        document.getElementById('form-receita').addEventListener('submit', e => handleFormSubmit(e, 'income', 'rec'));
        document.getElementById('form-produto').addEventListener('submit', e => handleFormSubmit(e, 'product', 'prod'));
        document.getElementById('form-despesa').addEventListener('submit', e => handleFormSubmit(e, 'expense', 'desp'));

        window.editarTransacao = (id) => {
            const t = transactions.find(x => x.id === id);
            if(!t) return;

            let prefix = '';
            if(t.type === 'income') prefix = 'rec';
            else if(t.type === 'product') prefix = 'prod';
            else if(t.type === 'expense') prefix = 'desp';

            document.getElementById(`${prefix}-form-title`).innerHTML = `<span class="text-blue-400">Editando Registro</span>`;
            document.getElementById(`btn-cancel-edit-${prefix}`).classList.remove('hidden');
            const btnSubmit = document.getElementById(`btn-submit-${prefix}`);
            btnSubmit.innerHTML = `<i data-lucide="save" class="w-4 h-4"></i> Atualizar`;
            btnSubmit.className = "w-full bg-blue-600 hover:bg-blue-500 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg";

            document.getElementById(`${prefix}-id`).value = t.id;
            const descEl = document.getElementById(`${prefix}-nome`) || document.getElementById(`${prefix}-desc`);
            descEl.value = t.description;
            document.getElementById(`${prefix}-setor`).value = t.sector;
            document.getElementById(`${prefix}-categoria`).value = t.category;
            document.getElementById(`${prefix}-valor`).value = t.amount;
            
            if(t.type === 'product') {
                document.getElementById(`${prefix}-custo`).value = t.costAmount || 0;
            }

            lucide.createIcons();
            document.getElementById(`sec-${prefix === 'rec' ? 'receitas' : prefix === 'prod' ? 'produtos' : 'despesas'}`).scrollIntoView({ behavior: 'smooth' });
        };

        window.cancelEdit = (prefix) => {
            document.getElementById(`form-${prefix === 'rec' ? 'receita' : prefix === 'prod' ? 'produto' : 'despesa'}`).reset();
            document.getElementById(`${prefix}-id`).value = '';
            
            const titles = { rec: 'Registrar Pagamento de Sócio', prod: 'Registrar Venda de Produto', desp: 'Registrar Gastos Gerais/Despesas' };
            const btnColors = { rec: 'emerald', prod: 'indigo', desp: 'rose' };
            const c = btnColors[prefix];

            document.getElementById(`${prefix}-form-title`).textContent = titles[prefix];
            document.getElementById(`btn-cancel-edit-${prefix}`).classList.add('hidden');
            
            const btnSubmit = document.getElementById(`btn-submit-${prefix}`);
            btnSubmit.innerHTML = `<i data-lucide="check" class="w-4 h-4"></i> Salvar`;
            btnSubmit.className = `w-full bg-${c}-500 hover:bg-${c}-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2 shadow-lg text-sm`;
            lucide.createIcons();
        };

        window.deletarTransacao = async (id) => {
            if (!currentUser) return;
            const confirmou = await window.customConfirm("Excluir Registro", "Esta ação não pode ser desfeita. Deseja realmente excluir este lançamento?");
            if (confirmou) {
                await deleteDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions', id));
            }
        };

        const saveSettingsToFirebase = async () => {
            await setDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'settings', 'geral'), appSettings);
        };

        window.salvarConfigGeral = async () => {
            if(!currentUser) return;
            appSettings.name = document.getElementById('conf-nome').value;
            appSettings.cnpj = document.getElementById('conf-cnpj').value;
            await saveSettingsToFirebase();
        };

        window.quickAddConfig = async (listKey, promptText, selectId) => {
            if(!currentUser) return;
            const val = await window.customPrompt(promptText);
            if(val && val.trim() !== '') {
                const item = val.trim();
                if(!appSettings[listKey]) appSettings[listKey] = [];
                
                if(!appSettings[listKey].includes(item)) {
                    appSettings[listKey].push(item);
                    await saveSettingsToFirebase();
                    
                    let tentativas = 0;
                    let checkInterval = setInterval(() => {
                        const el = document.getElementById(selectId);
                        if(el && Array.from(el.options).some(opt => opt.value === item)) {
                            el.value = item;
                            clearInterval(checkInterval);
                        }
                        tentativas++;
                        if(tentativas > 20) clearInterval(checkInterval);
                    }, 100);

                } else {
                    await window.customConfirm("Aviso", "Esta opção já existe na lista.");
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
            const confirmou = await window.customConfirm("Remover Categoria", "Deseja remover esta configuração?");
            if(confirmou) {
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
