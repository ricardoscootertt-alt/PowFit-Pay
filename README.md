<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pay - Gerenciamento de Academia</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            400: '#60a5fa',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            900: '#1e3a8a',
                            950: '#172554',
                        },
                        dark: {
                            bg: '#0f172a',
                            card: '#1e293b',
                            border: '#334155'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Estilos adicionais e configurações de impressão */
        body { font-family: 'Inter', sans-serif; }
        
        @media print {
            body { background-color: white !important; color: black !important; }
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            .print-container { width: 100% !important; margin: 0 !important; padding: 0 !important; }
            .dark\:bg-dark-card { background-color: white !important; border: 1px solid #ccc !important; }
            .text-white, .dark\:text-gray-300 { color: black !important; }
            * { box-shadow: none !important; }
        }

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }
    </style>
    <!-- Ícones -->
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="bg-dark-bg text-gray-200 min-h-screen flex antialiased">

    <!-- TELA DE LOGIN -->
    <div id="login-screen" class="fixed inset-0 bg-dark-bg z-50 flex flex-col items-center justify-center transition-opacity duration-300">
        <div class="bg-dark-card p-8 rounded-2xl shadow-2xl border border-dark-border max-w-md w-full text-center">
            <div class="flex justify-center mb-6">
                <div class="w-16 h-16 bg-brand-600 rounded-xl flex items-center justify-center shadow-lg shadow-brand-600/30">
                    <i data-lucide="dumbbell" class="text-white w-8 h-8"></i>
                </div>
            </div>
            <h1 class="text-3xl font-bold text-white mb-2">PowFit Pay</h1>
            <p class="text-gray-400 mb-8">Gerenciamento financeiro inteligente para sua academia.</p>
            
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
                    <i data-lucide="dumbbell" class="text-white w-6 h-6"></i>
                </div>
                <div>
                    <h2 class="text-xl font-bold text-white tracking-tight">PowFit</h2>
                    <p class="text-xs text-brand-400 font-medium">PAY SYSTEM</p>
                </div>
            </div>

            <nav class="flex-1 p-4 space-y-2 overflow-y-auto">
                <button onclick="navigate('dashboard')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="dashboard">
                    <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
                    <span class="font-medium">Dashboard</span>
                </button>
                <button onclick="navigate('receitas')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="receitas">
                    <i data-lucide="arrow-up-circle" class="w-5 h-5 text-emerald-400"></i>
                    <span class="font-medium">Receitas</span>
                </button>
                <button onclick="navigate('despesas')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 text-gray-400 hover:text-white hover:bg-dark-border rounded-xl transition-all" data-target="despesas">
                    <i data-lucide="arrow-down-circle" class="w-5 h-5 text-rose-400"></i>
                    <span class="font-medium">Despesas</span>
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
                        <p id="user-email" class="text-xs text-gray-400 truncate">Sincronizando...</p>
                    </div>
                </div>
                <button id="btn-logout" class="w-full flex items-center justify-center gap-2 px-4 py-2 bg-dark-border hover:bg-rose-500/20 hover:text-rose-400 text-gray-300 rounded-lg transition-colors text-sm font-medium">
                    <i data-lucide="log-out" class="w-4 h-4"></i> Sair
                </button>
            </div>
        </aside>

        <!-- MAIN CONTENT -->
        <main class="flex-1 ml-64 p-8 print-container">
            
            <header class="flex justify-between items-center mb-8 no-print">
                <div>
                    <h1 id="page-title" class="text-2xl font-bold text-white">Dashboard</h1>
                    <p id="gym-header-name" class="text-sm text-brand-400 mt-1">Academia não definida</p>
                </div>
                <div id="current-date-display" class="bg-dark-card px-4 py-2 rounded-lg border border-dark-border text-sm font-medium">
                    ...
                </div>
            </header>

            <!-- DASHBOARD SECTION -->
            <section id="sec-dashboard" class="page-section">
                
                <!-- Cards de Resumo -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <!-- Faturamento -->
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Faturamento Bruto</p>
                                <h3 id="dash-faturamento" class="text-2xl font-bold text-emerald-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-emerald-400/10 rounded-lg"><i data-lucide="trending-up" class="w-5 h-5 text-emerald-400"></i></div>
                        </div>
                    </div>
                    <!-- Despesas -->
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Gastos Totais</p>
                                <h3 id="dash-despesas" class="text-2xl font-bold text-rose-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-rose-400/10 rounded-lg"><i data-lucide="trending-down" class="w-5 h-5 text-rose-400"></i></div>
                        </div>
                    </div>
                    <!-- Sobra / Lucro -->
                    <div class="bg-dark-card p-6 rounded-2xl border border-brand-500/30 shadow-[0_0_15px_rgba(59,130,246,0.1)]">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Lucro Líquido (Sobra)</p>
                                <h3 id="dash-lucro" class="text-2xl font-bold text-brand-400">R$ 0,00</h3>
                            </div>
                            <div class="p-2 bg-brand-500/10 rounded-lg"><i data-lucide="wallet" class="w-5 h-5 text-brand-400"></i></div>
                        </div>
                    </div>
                    <!-- Margem -->
                    <div class="bg-dark-card p-6 rounded-2xl border border-dark-border">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-gray-400 text-sm font-medium mb-1">Margem de Lucro</p>
                                <h3 id="dash-margem" class="text-2xl font-bold text-white">0%</h3>
                            </div>
                            <div class="p-2 bg-gray-700/50 rounded-lg"><i data-lucide="pie-chart" class="w-5 h-5 text-gray-300"></i></div>
                        </div>
                    </div>
                </div>

                <!-- Divisão de Sócios -->
                <div class="bg-gradient-to-r from-dark-card to-brand-950/20 p-6 rounded-2xl border border-dark-border mb-8">
                    <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                        <i data-lucide="users" class="w-5 h-5 text-brand-400"></i> Pró-labore / Divisão Sócios (50/50)
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
                    <p class="text-xs text-gray-500 mt-3">* A divisão é calculada com base no Lucro Líquido do mês atual.</p>
                </div>

                <!-- Resumo Rápido Tabelas -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4">Últimas Receitas</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-sm text-left">
                                <thead class="text-xs text-gray-400 uppercase bg-dark-bg rounded-lg">
                                    <tr>
                                        <th class="px-4 py-3 rounded-l-lg">Associado</th>
                                        <th class="px-4 py-3">Pacote</th>
                                        <th class="px-4 py-3 rounded-r-lg text-right">Valor</th>
                                    </tr>
                                </thead>
                                <tbody id="dash-table-receitas" class="divide-y divide-dark-border">
                                    <!-- Preenchido via JS -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                    <div class="bg-dark-card rounded-2xl border border-dark-border p-6">
                        <h3 class="text-lg font-bold text-white mb-4">Últimos Gastos</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-sm text-left">
                                <thead class="text-xs text-gray-400 uppercase bg-dark-bg rounded-lg">
                                    <tr>
                                        <th class="px-4 py-3 rounded-l-lg">Descrição</th>
                                        <th class="px-4 py-3">Categoria</th>
                                        <th class="px-4 py-3 rounded-r-lg text-right">Valor</th>
                                    </tr>
                                </thead>
                                <tbody id="dash-table-despesas" class="divide-y divide-dark-border">
                                    <!-- Preenchido via JS -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </section>

            <!-- RECEITAS SECTION -->
            <section id="sec-receitas" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <h3 class="text-lg font-bold text-white mb-4">Anotar Mensalidade / Diária</h3>
                    <form id="form-receita" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4 items-end">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Nome do Associado(a)</label>
                            <input type="text" id="rec-nome" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none placeholder-gray-600" placeholder="Ex: João Silva">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria / Pacote</label>
                            <select id="rec-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none">
                                <option value="Associado 3 meses">3 Meses</option>
                                <option value="Associado 1 mês">1 Mês</option>
                                <option value="Temporário (Diária)">Diária</option>
                                <option value="Outros">Outros</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Valor (R$)</label>
                            <input type="number" id="rec-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="0.00">
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2">
                                <i data-lucide="plus" class="w-4 h-4"></i> Adicionar
                            </button>
                        </div>
                    </form>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border flex justify-between items-center">
                        <h3 class="font-bold text-white">Histórico de Receitas (Este Mês)</h3>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg">
                                <tr>
                                    <th class="px-6 py-4">Data</th>
                                    <th class="px-6 py-4">Associado</th>
                                    <th class="px-6 py-4">Pacote</th>
                                    <th class="px-6 py-4 text-right">Valor</th>
                                    <th class="px-6 py-4 text-center">Ação</th>
                                </tr>
                            </thead>
                            <tbody id="lista-receitas" class="divide-y divide-dark-border">
                                <!-- Preenchido via JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- DESPESAS SECTION -->
            <section id="sec-despesas" class="page-section hidden">
                <div class="bg-dark-card rounded-2xl border border-dark-border p-6 mb-8">
                    <h3 class="text-lg font-bold text-white mb-4">Anotar Gastos</h3>
                    <form id="form-despesa" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4 items-end">
                        <div class="lg:col-span-2">
                            <label class="block text-sm font-medium text-gray-400 mb-1">Descrição do Gasto</label>
                            <input type="text" id="desp-desc" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none placeholder-gray-600" placeholder="Ex: Conta de Luz ref. Maio">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Categoria</label>
                            <select id="desp-categoria" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none">
                                <option value="Limpeza">Limpeza</option>
                                <option value="Colchão">Colchão/Equipamentos</option>
                                <option value="Consultor RT">Consultor RT</option>
                                <option value="Energia">Energia</option>
                                <option value="Internet">Internet</option>
                                <option value="Outros">Outros</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-1">Valor (R$)</label>
                            <input type="number" id="desp-valor" step="0.01" required class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="0.00">
                        </div>
                        <div>
                            <button type="submit" class="w-full bg-rose-500 hover:bg-rose-600 text-white font-medium py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2">
                                <i data-lucide="plus" class="w-4 h-4"></i> Adicionar
                            </button>
                        </div>
                    </form>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border overflow-hidden">
                    <div class="p-4 border-b border-dark-border flex justify-between items-center">
                        <h3 class="font-bold text-white">Histórico de Gastos (Este Mês)</h3>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-400 uppercase bg-dark-bg">
                                <tr>
                                    <th class="px-6 py-4">Data</th>
                                    <th class="px-6 py-4">Descrição</th>
                                    <th class="px-6 py-4">Categoria</th>
                                    <th class="px-6 py-4 text-right">Valor</th>
                                    <th class="px-6 py-4 text-center">Ação</th>
                                </tr>
                            </thead>
                            <tbody id="lista-despesas" class="divide-y divide-dark-border">
                                <!-- Preenchido via JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- RELATÓRIOS SECTION -->
            <section id="sec-relatorios" class="page-section hidden">
                
                <div class="flex justify-between items-center mb-6 no-print">
                    <div class="flex gap-4 items-center">
                        <label class="text-gray-400 text-sm">Mês de Referência:</label>
                        <input type="month" id="filtro-mes" class="bg-dark-bg border border-dark-border text-white rounded-lg px-3 py-1.5 focus:outline-none">
                    </div>
                    <button onclick="window.print()" class="bg-brand-600 hover:bg-brand-500 text-white px-4 py-2 rounded-lg flex items-center gap-2 transition-colors">
                        <i data-lucide="printer" class="w-4 h-4"></i> Imprimir Relatório
                    </button>
                </div>

                <div class="bg-dark-card rounded-2xl border border-dark-border p-8 print:border-none print:shadow-none mb-8" id="print-area">
                    
                    <div class="text-center mb-8 border-b border-dark-border pb-6">
                        <h2 id="rel-gym-name" class="text-2xl font-bold text-white print:text-black">NOME DA ACADEMIA</h2>
                        <p id="rel-gym-cnpj" class="text-gray-400 print:text-gray-700">CNPJ: 00.000.000/0000-00</p>
                        <h3 class="text-lg font-semibold text-brand-400 mt-4 print:text-black">Relatório Mensal de Faturamento - <span id="rel-mes-texto"></span></h3>
                    </div>

                    <div class="grid grid-cols-2 gap-8 mb-8">
                        <div>
                            <h4 class="font-bold text-gray-300 print:text-black mb-3 border-b border-dark-border pb-2">Resumo de Receitas</h4>
                            <ul id="rel-resumo-receitas" class="space-y-2 text-sm text-gray-400 print:text-gray-800">
                                <!-- Preenchido via JS -->
                            </ul>
                            <div class="mt-4 pt-2 border-t border-dark-border flex justify-between font-bold text-emerald-400 print:text-black">
                                <span>Total Receitas:</span>
                                <span id="rel-total-receitas">R$ 0,00</span>
                            </div>
                        </div>
                        <div>
                            <h4 class="font-bold text-gray-300 print:text-black mb-3 border-b border-dark-border pb-2">Resumo de Gastos</h4>
                            <ul id="rel-resumo-despesas" class="space-y-2 text-sm text-gray-400 print:text-gray-800">
                                <!-- Preenchido via JS -->
                            </ul>
                            <div class="mt-4 pt-2 border-t border-dark-border flex justify-between font-bold text-rose-400 print:text-black">
                                <span>Total Gastos:</span>
                                <span id="rel-total-despesas">R$ 0,00</span>
                            </div>
                        </div>
                    </div>

                    <div class="bg-dark-bg print:bg-white border border-dark-border p-6 rounded-xl">
                        <h4 class="text-lg font-bold text-white print:text-black mb-4 text-center">Resultado Final</h4>
                        
                        <div class="flex justify-between items-center mb-2 text-gray-300 print:text-black">
                            <span>Faturamento Bruto:</span>
                            <span id="rel-fim-fat">R$ 0,00</span>
                        </div>
                        <div class="flex justify-between items-center mb-2 text-gray-300 print:text-black">
                            <span>(-) Custos Totais:</span>
                            <span id="rel-fim-custos">R$ 0,00</span>
                        </div>
                        <div class="flex justify-between items-center mb-4 text-brand-400 font-bold text-xl print:text-black border-b border-dark-border pb-4">
                            <span>(=) Lucro Líquido (Sobra):</span>
                            <span id="rel-fim-lucro">R$ 0,00</span>
                        </div>

                        <div class="grid grid-cols-2 gap-4 mt-4">
                            <div class="bg-dark-card print:bg-gray-100 p-4 rounded-lg text-center">
                                <p class="text-sm text-gray-400 print:text-gray-600 mb-1">Pró-labore Sócio 1 (50%)</p>
                                <p id="rel-socio1" class="font-bold text-white print:text-black text-lg">R$ 0,00</p>
                            </div>
                            <div class="bg-dark-card print:bg-gray-100 p-4 rounded-lg text-center">
                                <p class="text-sm text-gray-400 print:text-gray-600 mb-1">Pró-labore Sócio 2 (50%)</p>
                                <p id="rel-socio2" class="font-bold text-white print:text-black text-lg">R$ 0,00</p>
                            </div>
                        </div>
                        <div class="mt-4 text-center">
                            <span class="text-sm text-gray-400 print:text-gray-600">Margem de Lucro: <strong id="rel-margem" class="text-white print:text-black">0%</strong></span>
                        </div>
                    </div>

                </div>
            </section>

            <!-- CONFIGURAÇÕES SECTION -->
            <section id="sec-configuracoes" class="page-section hidden no-print">
                <div class="max-w-2xl bg-dark-card rounded-2xl border border-dark-border p-8">
                    <h3 class="text-xl font-bold text-white mb-6 flex items-center gap-2">
                        <i data-lucide="building-2" class="w-6 h-6 text-brand-400"></i> Dados da Academia
                    </h3>
                    
                    <form id="form-config" class="space-y-6">
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-2">Nome da Academia</label>
                            <input type="text" id="conf-nome" class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="Ex: PowFit Centro de Treinamento">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-400 mb-2">CNPJ</label>
                            <input type="text" id="conf-cnpj" class="w-full bg-dark-bg border border-dark-border text-white rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:outline-none" placeholder="00.000.000/0000-00">
                        </div>
                        <div class="pt-4 border-t border-dark-border">
                            <button type="submit" class="w-full md:w-auto bg-brand-600 hover:bg-brand-500 text-white font-medium py-3 px-8 rounded-xl transition-colors shadow-lg shadow-brand-600/20">
                                Salvar Configurações
                            </button>
                            <span id="config-save-msg" class="ml-4 text-emerald-400 text-sm hidden">Salvo com sucesso!</span>
                        </div>
                    </form>
                </div>
            </section>

        </main>
    </div>

    <!-- Scripts e Lógica (Firebase + App) -->
    <script type="module">
        // Importações do Firebase SDK v11
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithCustomToken, signInAnonymously, GoogleAuthProvider, signInWithPopup, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, addDoc, deleteDoc, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Configuração do Firebase
        // Prioriza a configuração injetada pelo ambiente (se existir), caso contrário usa a fornecida.
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "AIzaSyDOGZGAKNvAUiD7BfETTTXJz-aQ54AAweM",
            authDomain: "pow-fit-pay.firebaseapp.com",
            projectId: "pow-fit-pay",
            storageBucket: "pow-fit-pay.firebasestorage.app",
            messagingSenderId: "899659807167",
            appId: "1:899659807167:web:b8ca11726bff6cbcd6090a"
        };

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'pow-fit-pay';

        // Inicializa Firebase
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Estado Global da Aplicação
        let currentUser = null;
        let transactions = [];
        let gymSettings = { name: 'Nome da Academia', cnpj: '' };
        let unsubTransactions = null;
        let unsubSettings = null;
        let currentMonthFilter = new Date().toISOString().slice(0, 7); // YYYY-MM

        // Elementos da UI
        const loginScreen = document.getElementById('login-screen');
        const appContainer = document.getElementById('app-container');
        const loginLoading = document.getElementById('login-loading');
        
        // Formatar Moeda
        const formatMoney = (value) => {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
        };
        // Formatar Data
        const formatDate = (dateStr) => {
            const [y, m, d] = dateStr.split('-');
            return `${d}/${m}/${y}`;
        };

        // Inicializa Lucide Icons
        lucide.createIcons();

        // Data Atual no Header
        document.getElementById('current-date-display').textContent = new Intl.DateTimeFormat('pt-BR', { dateStyle: 'full' }).format(new Date());
        document.getElementById('filtro-mes').value = currentMonthFilter;
        document.getElementById('filtro-mes').addEventListener('change', (e) => {
            currentMonthFilter = e.target.value;
            updateDashboard();
        });

        // -------------------------
        // SISTEMA DE NAVEGAÇÃO
        // -------------------------
        window.navigate = (targetId) => {
            document.querySelectorAll('.page-section').forEach(sec => sec.classList.add('hidden'));
            document.getElementById(`sec-${targetId}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(btn => {
                if(btn.dataset.target === targetId) {
                    btn.classList.add('bg-dark-border', 'text-white');
                    btn.classList.remove('text-gray-400');
                } else {
                    btn.classList.remove('bg-dark-border', 'text-white');
                    btn.classList.add('text-gray-400');
                }
            });

            const titles = {
                'dashboard': 'Dashboard Geral',
                'receitas': 'Gerenciar Receitas',
                'despesas': 'Gerenciar Despesas',
                'relatorios': 'Relatório Financeiro',
                'configuracoes': 'Configurações'
            };
            document.getElementById('page-title').textContent = titles[targetId];
            
            // Re-renderizar dependendo da aba
            updateDashboard();
        };

        // -------------------------
        // AUTENTICAÇÃO E INICIALIZAÇÃO (Regra 3 do Firebase)
        // -------------------------
        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                console.error("Erro na autenticação inicial:", error);
            }
        };

        // Login com Google Acionado pelo usuário
        document.getElementById('btn-google-login').addEventListener('click', async () => {
            const provider = new GoogleAuthProvider();
            try {
                loginLoading.classList.remove('hidden');
                await signInWithPopup(auth, provider);
            } catch (error) {
                console.error("Erro no login com Google:", error);
                alert("Erro ao fazer login com o Google.");
            } finally {
                loginLoading.classList.add('hidden');
            }
        });

        document.getElementById('btn-logout').addEventListener('click', async () => {
            await signOut(auth);
            window.location.reload();
        });

        // Listener de Estado de Autenticação
        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                
                // Atualiza UI de Perfil
                if(user.isAnonymous) {
                    document.getElementById('user-name').textContent = "Usuário Anônimo";
                    document.getElementById('user-email').textContent = "Dados locais temporários";
                    // Se for anônimo, continua mostrando a tela de login para forçar o Google (ou oculta se quisermos usar anon)
                    // Para cumprir o prompt, se for anônimo num ambiente normal, mostramos a tela para forçar o Google.
                    // Em canvas, podemos liberar.
                } else {
                    document.getElementById('user-name').textContent = user.displayName || "Usuário";
                    document.getElementById('user-email').textContent = user.email || "";
                    if(user.photoURL) {
                        const img = document.getElementById('user-avatar');
                        img.src = user.photoURL;
                        img.classList.remove('hidden');
                        document.getElementById('user-avatar-fallback').classList.add('hidden');
                    }
                    // Esconde tela de login e mostra app
                    loginScreen.classList.add('opacity-0', 'pointer-events-none');
                    setTimeout(() => loginScreen.classList.add('hidden'), 300);
                    appContainer.classList.remove('hidden');
                }

                // Inicia escuta de dados APÓS auth
                loadUserData();
            } else {
                currentUser = null;
                loginScreen.classList.remove('hidden', 'opacity-0', 'pointer-events-none');
                appContainer.classList.add('hidden');
            }
        });

        // Chamada inicial obrigatória
        initAuth();

        // -------------------------
        // CARREGAMENTO DE DADOS (FIRESTORE)
        // -------------------------
        const loadUserData = () => {
            if (!currentUser) return;
            
            const uid = currentUser.uid;
            
            // Limpa listeners antigos
            if (unsubTransactions) unsubTransactions();
            if (unsubSettings) unsubSettings();

            // Referências seguindo a Regra 1: strict paths
            const transRef = collection(db, 'artifacts', appId, 'users', uid, 'transactions');
            const settingsRef = doc(db, 'artifacts', appId, 'users', uid, 'settings', 'geral');

            // Escuta Configurações
            unsubSettings = onSnapshot(settingsRef, (docSnap) => {
                if (docSnap.exists()) {
                    gymSettings = docSnap.data();
                    atualizarTextosAcademia();
                }
            }, (error) => console.error("Erro config:", error));

            // Escuta Transações (Regra 2: sem queries complexas, traz tudo e filtra no JS)
            unsubTransactions = onSnapshot(transRef, (snapshot) => {
                transactions = [];
                snapshot.forEach(doc => {
                    transactions.push({ id: doc.id, ...doc.data() });
                });
                // Ordenar por data mais recente
                transactions.sort((a, b) => new Date(b.date) - new Date(a.date) || b.timestamp - a.timestamp);
                updateDashboard();
            }, (error) => console.error("Erro transactions:", error));
        };

        const atualizarTextosAcademia = () => {
            const nome = gymSettings.name || 'Academia não definida';
            const cnpj = gymSettings.cnpj || '';
            
            document.getElementById('gym-header-name').textContent = nome;
            document.getElementById('rel-gym-name').textContent = nome;
            document.getElementById('rel-gym-cnpj').textContent = cnpj ? `CNPJ: ${cnpj}` : '';
            
            document.getElementById('conf-nome').value = gymSettings.name || '';
            document.getElementById('conf-cnpj').value = gymSettings.cnpj || '';
        };

        // -------------------------
        // LÓGICA DE NEGÓCIO E RENDERIZAÇÃO
        // -------------------------
        
        // Filtra transações pelo mês selecionado
        const getTransacoesDoMes = () => {
            return transactions.filter(t => t.date.startsWith(currentMonthFilter));
        };

        const updateDashboard = () => {
            const currentTx = getTransacoesDoMes();
            
            let totalReceitas = 0;
            let totalDespesas = 0;
            
            const receitasPorCat = {};
            const despesasPorCat = {};

            const listasReceitasHTML = [];
            const listasDespesasHTML = [];

            currentTx.forEach(t => {
                if (t.type === 'income') {
                    totalReceitas += t.amount;
                    receitasPorCat[t.category] = (receitasPorCat[t.category] || 0) + t.amount;
                    
                    listasReceitasHTML.push(`
                        <tr class="hover:bg-dark-border/50 transition-colors">
                            <td class="px-6 py-4 whitespace-nowrap text-gray-400">${formatDate(t.date)}</td>
                            <td class="px-6 py-4 font-medium text-white">${t.description}</td>
                            <td class="px-6 py-4 text-brand-400">${t.category}</td>
                            <td class="px-6 py-4 text-right font-bold text-emerald-400">${formatMoney(t.amount)}</td>
                            <td class="px-6 py-4 text-center">
                                <button onclick="deletarTransacao('${t.id}')" class="text-rose-400 hover:text-rose-300 p-1"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                            </td>
                        </tr>
                    `);
                } else if (t.type === 'expense') {
                    totalDespesas += t.amount;
                    despesasPorCat[t.category] = (despesasPorCat[t.category] || 0) + t.amount;

                    listasDespesasHTML.push(`
                        <tr class="hover:bg-dark-border/50 transition-colors">
                            <td class="px-6 py-4 whitespace-nowrap text-gray-400">${formatDate(t.date)}</td>
                            <td class="px-6 py-4 font-medium text-white">${t.description}</td>
                            <td class="px-6 py-4 text-gray-400">${t.category}</td>
                            <td class="px-6 py-4 text-right font-bold text-rose-400">${formatMoney(t.amount)}</td>
                            <td class="px-6 py-4 text-center">
                                <button onclick="deletarTransacao('${t.id}')" class="text-rose-400 hover:text-rose-300 p-1"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                            </td>
                        </tr>
                    `);
                }
            });

            // Cálculos Finais
            const lucroLiquido = totalReceitas - totalDespesas;
            const proLaboreSocio = lucroLiquido > 0 ? lucroLiquido / 2 : 0;
            const margem = totalReceitas > 0 ? (lucroLiquido / totalReceitas) * 100 : 0;

            // Atualiza Dashboard Cards
            document.getElementById('dash-faturamento').textContent = formatMoney(totalReceitas);
            document.getElementById('dash-despesas').textContent = formatMoney(totalDespesas);
            document.getElementById('dash-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('dash-margem').textContent = `${margem.toFixed(1)}%`;
            
            document.getElementById('dash-lucro').className = `text-2xl font-bold ${lucroLiquido >= 0 ? 'text-brand-400' : 'text-rose-400'}`;
            
            document.getElementById('dash-socio1').textContent = formatMoney(proLaboreSocio);
            document.getElementById('dash-socio2').textContent = formatMoney(proLaboreSocio);

            // Atualiza Tabelas nas abas de input
            document.getElementById('lista-receitas').innerHTML = listasReceitasHTML.length ? listasReceitasHTML.join('') : '<tr><td colspan="5" class="px-6 py-4 text-center text-gray-500">Nenhuma receita registrada neste mês.</td></tr>';
            document.getElementById('lista-despesas').innerHTML = listasDespesasHTML.length ? listasDespesasHTML.join('') : '<tr><td colspan="5" class="px-6 py-4 text-center text-gray-500">Nenhum gasto registrado neste mês.</td></tr>';

            // Atualiza Resumos na Dash (Top 5)
            const recDashHTML = currentTx.filter(t=>t.type==='income').slice(0,5).map(t => `
                <tr>
                    <td class="px-4 py-3 font-medium text-white">${t.description}</td>
                    <td class="px-4 py-3 text-brand-400">${t.category}</td>
                    <td class="px-4 py-3 text-right text-emerald-400 font-bold">${formatMoney(t.amount)}</td>
                </tr>
            `).join('');
            document.getElementById('dash-table-receitas').innerHTML = recDashHTML || '<tr><td colspan="3" class="px-4 py-3 text-center text-gray-500 text-xs">Vazio</td></tr>';

            const despDashHTML = currentTx.filter(t=>t.type==='expense').slice(0,5).map(t => `
                <tr>
                    <td class="px-4 py-3 font-medium text-white">${t.description}</td>
                    <td class="px-4 py-3 text-gray-400">${t.category}</td>
                    <td class="px-4 py-3 text-right text-rose-400 font-bold">${formatMoney(t.amount)}</td>
                </tr>
            `).join('');
            document.getElementById('dash-table-despesas').innerHTML = despDashHTML || '<tr><td colspan="3" class="px-4 py-3 text-center text-gray-500 text-xs">Vazio</td></tr>';

            // Atualiza Seção de Relatórios
            document.getElementById('rel-mes-texto').textContent = currentMonthFilter.split('-').reverse().join('/');
            
            let htmlResumoRec = '';
            for(let cat in receitasPorCat) {
                htmlResumoRec += `<li class="flex justify-between border-b border-dark-border/50 print:border-gray-200 pb-1"><span>${cat}</span> <strong>${formatMoney(receitasPorCat[cat])}</strong></li>`;
            }
            document.getElementById('rel-resumo-receitas').innerHTML = htmlResumoRec || '<li>Sem receitas</li>';
            document.getElementById('rel-total-receitas').textContent = formatMoney(totalReceitas);

            let htmlResumoDesp = '';
            for(let cat in despesasPorCat) {
                htmlResumoDesp += `<li class="flex justify-between border-b border-dark-border/50 print:border-gray-200 pb-1"><span>${cat}</span> <strong>${formatMoney(despesasPorCat[cat])}</strong></li>`;
            }
            document.getElementById('rel-resumo-despesas').innerHTML = htmlResumoDesp || '<li>Sem gastos</li>';
            document.getElementById('rel-total-despesas').textContent = formatMoney(totalDespesas);

            document.getElementById('rel-fim-fat').textContent = formatMoney(totalReceitas);
            document.getElementById('rel-fim-custos').textContent = formatMoney(totalDespesas);
            document.getElementById('rel-fim-lucro').textContent = formatMoney(lucroLiquido);
            document.getElementById('rel-socio1').textContent = formatMoney(proLaboreSocio);
            document.getElementById('rel-socio2').textContent = formatMoney(proLaboreSocio);
            document.getElementById('rel-margem').textContent = `${margem.toFixed(1)}%`;

            lucide.createIcons();
        };

        // -------------------------
        // AÇÕES DO USUÁRIO (CRIAR/DELETAR/SALVAR)
        // -------------------------
        
        // Adicionar Receita
        document.getElementById('form-receita').addEventListener('submit', async (e) => {
            e.preventDefault();
            if (!currentUser) return;

            const nome = document.getElementById('rec-nome').value;
            const cat = document.getElementById('rec-categoria').value;
            const val = parseFloat(document.getElementById('rec-valor').value);
            const hoje = new Date().toISOString().split('T')[0];

            try {
                const transRef = collection(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions');
                await addDoc(transRef, {
                    type: 'income',
                    description: nome,
                    category: cat,
                    amount: val,
                    date: hoje,
                    timestamp: Date.now()
                });
                document.getElementById('form-receita').reset();
            } catch (error) {
                console.error("Erro ao adicionar receita", error);
            }
        });

        // Adicionar Despesa
        document.getElementById('form-despesa').addEventListener('submit', async (e) => {
            e.preventDefault();
            if (!currentUser) return;

            const desc = document.getElementById('desp-desc').value;
            const cat = document.getElementById('desp-categoria').value;
            const val = parseFloat(document.getElementById('desp-valor').value);
            const hoje = new Date().toISOString().split('T')[0];

            try {
                const transRef = collection(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions');
                await addDoc(transRef, {
                    type: 'expense',
                    description: desc,
                    category: cat,
                    amount: val,
                    date: hoje,
                    timestamp: Date.now()
                });
                document.getElementById('form-despesa').reset();
            } catch (error) {
                console.error("Erro ao adicionar despesa", error);
            }
        });

        // Deletar Transação
        window.deletarTransacao = async (id) => {
            if (!currentUser) return;
            if(confirm('Tem certeza que deseja apagar este registro?')) {
                try {
                    await deleteDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'transactions', id));
                } catch (error) {
                    console.error("Erro ao deletar", error);
                }
            }
        };

        // Salvar Configurações
        document.getElementById('form-config').addEventListener('submit', async (e) => {
            e.preventDefault();
            if (!currentUser) return;

            const nome = document.getElementById('conf-nome').value;
            const cnpj = document.getElementById('conf-cnpj').value;

            try {
                const settingsRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'settings', 'geral');
                await setDoc(settingsRef, { name: nome, cnpj: cnpj });
                
                const msg = document.getElementById('config-save-msg');
                msg.classList.remove('hidden');
                setTimeout(() => msg.classList.add('hidden'), 3000);
            } catch (error) {
                console.error("Erro ao salvar config", error);
            }
        });

        // Inicializa aba ativa
        navigate('dashboard');

    </script>
</body>
</html>
