# luiz-is-eduardo.github.io
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rei dos Gelatos & Açaí • Distribuidora B2B</title>
  <!-- Tailwind CSS & Lucide Icons -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Plus Jakarta Sans', sans-serif; }
    .custom-scrollbar::-webkit-scrollbar { width: 6px; height: 6px; }
    .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 9999px; }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 flex h-screen overflow-hidden">

  <!-- BARRA LATERAL (SIDEBAR) -->
  <aside class="w-64 bg-slate-900 text-slate-300 flex flex-col justify-between shrink-0 shadow-xl z-20">
    <div>
      <div class="h-20 flex items-center px-6 gap-3 border-b border-slate-800/80 bg-slate-950/40">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-purple-600 to-pink-500 flex items-center justify-center text-xl shadow-lg shadow-purple-500/20">
          🍧
        </div>
        <div>
          <h1 class="font-bold text-white tracking-wide text-base leading-tight">Rei dos Gelatos</h1>
          <span class="text-xs text-purple-400 font-medium">Distribuidora & Açaí</span>
        </div>
      </div>

      <nav class="p-4 space-y-1.5 font-medium text-sm">
        <button onclick="setTab('dashboard')" id="nav-dashboard" class="w-full flex items-center gap-3 px-3 py-2.5 rounded-lg bg-purple-600/10 text-purple-400 font-semibold transition">
          <i data-lucide="layout-dashboard" class="w-4 h-4"></i> Dashboard
        </button>
        <button onclick="setTab('pedidos')" id="nav-pedidos" class="w-full flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-slate-800/60 hover:text-white transition">
          <i data-lucide="shopping-bag" class="w-4 h-4"></i> Pedidos de Venda
        </button>
        <button onclick="setTab('estoque')" id="nav-estoque" class="w-full flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-slate-800/60 hover:text-white transition">
          <i data-lucide="package" class="w-4 h-4"></i> Produtos & Estoque
        </button>
        <button onclick="setTab('clientes')" id="nav-clientes" class="w-full flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-slate-800/60 hover:text-white transition">
          <i data-lucide="users" class="w-4 h-4"></i> Revendedores / Clientes
        </button>
      </nav>
    </div>

    <!-- Sincronização e Status -->
    <div class="p-4 border-t border-slate-800 bg-slate-950/30">
      <div class="flex items-center justify-between mb-2">
        <span class="text-xs text-slate-400 flex items-center gap-2">
          <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Sheets Conectado
        </span>
        <button onclick="carregarDados()" title="Atualizar agora" class="text-slate-400 hover:text-white">
          <i data-lucide="refresh-cw" class="w-3.5 h-3.5"></i>
        </button>
      </div>
      <button onclick="abrirModalPedido()" class="w-full bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white font-medium py-2 px-3 rounded-lg text-sm flex items-center justify-center gap-2 shadow-lg shadow-purple-600/30 transition">
        <i data-lucide="plus-circle" class="w-4 h-4"></i> Novo Pedido
      </button>
    </div>
  </aside>

  <!-- ÁREA DE CONTEÚDO PRINCIPAL -->
  <main class="flex-1 flex flex-col overflow-hidden">
    <!-- Topbar -->
    <header class="h-16 bg-white border-b border-slate-200 px-8 flex items-center justify-between shrink-0">
      <div class="flex items-center gap-3">
        <h2 id="page-title" class="text-xl font-bold text-slate-800">Dashboard Executivo</h2>
        <span class="text-xs bg-purple-50 text-purple-700 px-2.5 py-0.5 rounded-full font-semibold border border-purple-100">B2B Sistema Integrado</span>
      </div>
      <div class="flex items-center gap-4 text-sm text-slate-500">
        <span id="current-date">--/--/----</span>
      </div>
    </header>

    <!-- Conteúdo Dinâmico -->
    <div class="flex-1 overflow-y-auto p-8 custom-scrollbar space-y-6">
      
      <!-- ABA 1: DASHBOARD -->
      <section id="tab-dashboard" class="space-y-6">
        <!-- Cards de Métricas Principais -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-5">
          <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col justify-between">
            <div class="flex items-center justify-between text-slate-500 mb-2">
              <span class="text-xs font-semibold uppercase tracking-wider">Faturamento Total</span>
              <div class="p-2 bg-purple-50 text-purple-600 rounded-lg"><i data-lucide="dollar-sign" class="w-4 h-4"></i></div>
            </div>
            <div class="text-2xl font-bold text-slate-800" id="kpi-faturamento">R$ 0,00</div>
            <span class="text-xs text-emerald-600 mt-2 font-medium flex items-center gap-1">
              <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Vendas registradas
            </span>
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col justify-between">
            <div class="flex items-center justify-between text-slate-500 mb-2">
              <span class="text-xs font-semibold uppercase tracking-wider">Recebido (Pago)</span>
              <div class="p-2 bg-emerald-50 text-emerald-600 rounded-lg"><i data-lucide="check-circle" class="w-4 h-4"></i></div>
            </div>
            <div class="text-2xl font-bold text-emerald-700" id="kpi-recebido">R$ 0,00</div>
            <span class="text-xs text-slate-500 mt-2 font-medium">Liquidação confirmada</span>
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col justify-between">
            <div class="flex items-center justify-between text-slate-500 mb-2">
              <span class="text-xs font-semibold uppercase tracking-wider">A Receber (Pendente)</span>
              <div class="p-2 bg-amber-50 text-amber-600 rounded-lg"><i data-lucide="clock" class="w-4 h-4"></i></div>
            </div>
            <div class="text-2xl font-bold text-amber-600" id="kpi-pendente">R$ 0,00</div>
            <span class="text-xs text-amber-600 mt-2 font-medium">Fluxo em aberto</span>
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col justify-between">
            <div class="flex items-center justify-between text-slate-500 mb-2">
              <span class="text-xs font-semibold uppercase tracking-wider">Ticket Médio</span>
              <div class="p-2 bg-blue-50 text-blue-600 rounded-lg"><i data-lucide="shopping-cart" class="w-4 h-4"></i></div>
            </div>
            <div class="text-2xl font-bold text-slate-800" id="kpi-ticket">R$ 0,00</div>
            <span class="text-xs text-slate-500 mt-2 font-medium" id="kpi-qtd-pedidos">0 pedidos</span>
          </div>
        </div>

        <!-- Tabelas de Apoio do Dashboard -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <!-- Produtos em Baixa no Estoque -->
          <div class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm">
            <div class="flex items-center justify-between mb-4">
              <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                <i data-lucide="alert-triangle" class="w-4 h-4 text-amber-500"></i> Alerta de Estoque Crítico
              </h3>
            </div>
            <div class="overflow-x-auto">
              <table class="w-full text-left text-sm">
                <thead>
                  <tr class="text-xs font-semibold text-slate-400 uppercase border-b">
                    <th class="pb-3">SKU</th>
                    <th class="pb-3">Produto</th>
                    <th class="pb-3 text-right">Saldo</th>
                  </tr>
                </thead>
                <tbody id="lista-alerta-estoque" class="divide-y divide-slate-100 text-slate-600">
                  <!-- Injetado via JS -->
                </tbody>
              </table>
            </div>
          </div>

          <!-- Últimos Pedidos -->
          <div class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm">
            <div class="flex items-center justify-between mb-4">
              <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                <i data-lucide="history" class="w-4 h-4 text-purple-600"></i> Últimos Lançamentos
              </h3>
            </div>
            <div class="overflow-x-auto">
              <table class="w-full text-left text-sm">
                <thead>
                  <tr class="text-xs font-semibold text-slate-400 uppercase border-b">
                    <th class="pb-3">Pedido</th>
                    <th class="pb-3">Cliente</th>
                    <th class="pb-3">Valor</th>
                    <th class="pb-3 text-right">Status</th>
                  </tr>
                </thead>
                <tbody id="lista-ultimos-pedidos" class="divide-y divide-slate-100 text-slate-600">
                  <!-- Injetado via JS -->
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </section>

      <!-- ABA 2: HISTÓRICO DE PEDIDOS -->
      <section id="tab-pedidos" class="hidden space-y-4">
        <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col md:flex-row gap-4 items-center justify-between">
          <div class="relative w-full md:w-80">
            <i data-lucide="search" class="w-4 h-4 absolute left-3.5 top-3.5 text-slate-400"></i>
            <input type="text" id="filtro-pedido" oninput="filtrarPedidos()" placeholder="Buscar por cliente ou ID..." class="w-full pl-10 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-purple-500/20">
          </div>
          <button onclick="abrirModalPedido()" class="w-full md:w-auto px-4 py-2 bg-purple-600 text-white rounded-xl text-sm font-semibold flex items-center justify-center gap-2 hover:bg-purple-700">
            <i data-lucide="plus" class="w-4 h-4"></i> Criar Novo Pedido
          </button>
        </div>

        <div class="bg-white rounded-2xl border border-slate-200/80 shadow-sm overflow-hidden">
          <table class="w-full text-left text-sm">
            <thead class="bg-slate-50 border-b border-slate-100 text-xs text-slate-400 font-semibold uppercase">
              <tr>
                <th class="p-4">Nº Pedido</th>
                <th class="p-4">Data</th>
                <th class="p-4">Cliente</th>
                <th class="p-4">Vencimento</th>
                <th class="p-4">Total</th>
                <th class="p-4 text-center">Status</th>
              </tr>
            </thead>
            <tbody id="tabela-pedidos-body" class="divide-y divide-slate-100 text-slate-700">
              <!-- Injetado via JS -->
            </tbody>
          </table>
        </div>
      </section>

      <!-- ABA 3: PRODUTOS & ESTOQUE -->
      <section id="tab-estoque" class="hidden space-y-4">
        <div class="bg-white rounded-2xl border border-slate-200/80 shadow-sm overflow-hidden">
          <table class="w-full text-left text-sm">
            <thead class="bg-slate-50 border-b border-slate-100 text-xs text-slate-400 font-semibold uppercase">
              <tr>
                <th class="p-4">SKU</th>
                <th class="p-4">Descrição do Produto</th>
                <th class="p-4">Categoria</th>
                <th class="p-4 text-right">Preço Unitário</th>
                <th class="p-4 text-center">Estoque Atual</th>
                <th class="p-4 text-center">Condição</th>
              </tr>
            </thead>
            <tbody id="tabela-estoque-body" class="divide-y divide-slate-100 text-slate-700">
              <!-- Injetado via JS -->
            </tbody>
          </table>
        </div>
      </section>

      <!-- ABA 4: CLIENTES -->
      <section id="tab-clientes" class="hidden space-y-4">
        <div class="bg-white rounded-2xl border border-slate-200/80 shadow-sm overflow-hidden">
          <table class="w-full text-left text-sm">
            <thead class="bg-slate-50 border-b border-slate-100 text-xs text-slate-400 font-semibold uppercase">
              <tr>
                <th class="p-4">ID</th>
                <th class="p-4">Nome Completo</th>
                <th class="p-4">CPF / CNPJ</th>
                <th class="p-4">Contato / Telefone</th>
                <th class="p-4">Endereço / Cidade</th>
              </tr>
            </thead>
            <tbody id="tabela-clientes-body" class="divide-y divide-slate-100 text-slate-700">
              <!-- Injetado via JS -->
            </tbody>
          </table>
        </div>
      </section>

    </div>
  </main>

  <!-- MODAL: NOVO PEDIDO -->
  <div id="modal-pedido" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
    <div class="bg-white rounded-2xl w-full max-w-2xl shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
      <div class="p-5 border-b border-slate-100 flex items-center justify-between bg-slate-50/50">
        <div>
          <h3 class="font-bold text-slate-800 text-base">Emitir Novo Pedido de Venda</h3>
          <p class="text-xs text-slate-500">Gera histórico e faz baixa automática no estoque</p>
        </div>
        <button onclick="fecharModalPedido()" class="p-1 text-slate-400 hover:text-slate-700 rounded-lg">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <div class="p-6 overflow-y-auto space-y-5 custom-scrollbar">
        <!-- Seleção do Cliente -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Selecionar Revendedor *</label>
            <select id="modal-select-cliente" class="w-full bg-slate-50 border border-slate-200 p-2.5 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-purple-500/20">
              <option value="">Selecione o Cliente...</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Data de Vencimento</label>
            <input type="date" id="modal-vencimento" class="w-full bg-slate-50 border border-slate-200 p-2.5 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-purple-500/20">
          </div>
        </div>

        <!-- Adicionar Item ao Carrinho -->
        <div class="bg-slate-50/80 p-4 rounded-xl border border-slate-200/80 space-y-3">
          <span class="text-xs font-bold text-slate-700 uppercase tracking-wider block">Adicionar Produtos ao Pedido</span>
          <div class="grid grid-cols-12 gap-3 items-end">
            <div class="col-span-7">
              <label class="block text-xs text-slate-500 mb-1">Produto</label>
              <select id="modal-select-produto" class="w-full bg-white border border-slate-200 p-2 rounded-lg text-sm">
                <option value="">Escolha um item...</option>
              </select>
            </div>
            <div class="col-span-3">
              <label class="block text-xs text-slate-500 mb-1">Qtd</label>
              <input type="number" id="modal-qtd-item" value="1" min="1" class="w-full bg-white border border-slate-200 p-2 rounded-lg text-sm text-center">
            </div>
            <div class="col-span-2">
              <button type="button" onclick="adicionarItemAoPedido()" class="w-full bg-slate-800 hover:bg-slate-900 text-white p-2 rounded-lg text-sm font-semibold flex items-center justify-center">
                <i data-lucide="plus" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
        </div>

        <!-- Lista de Itens Adicionados -->
        <div>
          <table class="w-full text-left text-sm border border-slate-100 rounded-xl overflow-hidden">
            <thead class="bg-slate-100/70 text-xs text-slate-500">
              <tr>
                <th class="p-2.5">Item</th>
                <th class="p-2.5 text-center">Qtd</th>
                <th class="p-2.5 text-right">Unitário</th>
                <th class="p-2.5 text-right">Subtotal</th>
                <th class="p-2.5 text-center">Ação</th>
              </tr>
            </thead>
            <tbody id="modal-tabela-carrinho" class="divide-y divide-slate-100">
              <tr><td colspan="5" class="p-4 text-center text-xs text-slate-400">Nenhum produto adicionado ainda.</td></tr>
            </tbody>
          </table>
        </div>

        <div class="flex justify-between items-center pt-3 border-t">
          <span class="text-sm font-medium text-slate-500">Valor Total do Pedido:</span>
          <span id="modal-total-pedido" class="text-xl font-bold text-slate-900">R$ 0,00</span>
        </div>
      </div>

      <div class="p-4 bg-slate-50 border-t border-slate-100 flex items-center justify-end gap-3">
        <button onclick="fecharModalPedido()" class="px-4 py-2 text-slate-600 hover:bg-slate-200/60 rounded-xl text-sm font-medium">Cancelar</button>
        <button onclick="salvarPedido()" id="btn-salvar-pedido" class="px-5 py-2 bg-purple-600 hover:bg-purple-700 text-white rounded-xl text-sm font-semibold flex items-center gap-2">
          <i data-lucide="check" class="w-4 h-4"></i> Salvar e Registrar
        </button>
      </div>
    </div>
  </div>

  <!-- SCRIPT DE DADOS E LÓGICA -->
  <script>
    // Cole aqui o URL gerado na implantação do Apps Script
    const SCRIPT_URL = "[SUA_URL_DO_APPS_SCRIPT_AQUI](https://script.googleusercontent.com/macros/echo?user_content_key=AUkAhnR-Bgg3DD5HJmOBXZs9rZMXqAgeTX8R2eWPDyMjXyOWcnWp_O_nNilFDVUGI_mFUbe8M_SQagKNEUbx1oiBBJgf0y61TU12kmHTMFIQ76sm2d06NASuzBIYcPgzZPMVWkh_LSctmuhI3ms0lsGMv-szQjsYVW0fZEi8fXJRj4TQQwGtF7XR4X8IVi2U1_Yze2Wgg_cY1g2pXEYvuS_N563H9Y7-CZA879GOWaQuDIOFEKf4URsKwtasDhW7Ft5PJ1rv5YG4nj2m6WLCaZWBQjvmX169Mg&lib=M41R_BKdRNnoYWqAM89qBynb-DfI6kR2q)";

    // Dados iniciais (espelho da sua planilha para funcionamento imediato)
    let bancoDados = {
      clientes: [
        { id: "CLI-001", nome: "Mariana Silveira Costa", cpf: "123.456.789-01", contato: "(11) 98765-4321", endereco: "Av. Paulista, 1500", cidade: "São Paulo/SP" },
        { id: "CLI-002", nome: "Lucas Henrique de Oliveira", cpf: "234.567.890-12", contato: "(11) 97654-3210", endereco: "Rua Augusta, 780", cidade: "São Paulo/SP" },
        { id: "CLI-003", nome: "Beatriz Fernandes Santos", cpf: "345.678.901-23", contato: "(11) 96543-2109", endereco: "Rua Oscar Freire, 320", cidade: "São Paulo/SP" },
        { id: "CLI-004", nome: "Gabriel Souza Ribeiro", cpf: "456.789.012-34", contato: "(11) 95432-1098", endereco: "Alameda Santos, 1200", cidade: "São Paulo/SP" }
      ],
      produtos: [
        { sku: "SKU-AC01", nome: "Açaí Tradicional 300ml", categoria: "Açaí", preco: 16.00, estoque: 85, statusEstoque: "Estoque Normal" },
        { sku: "SKU-AC02", nome: "Açaí Tradicional 500ml", categoria: "Açaí", preco: 22.00, estoque: 110, statusEstoque: "Estoque Normal" },
        { sku: "SKU-AC03", nome: "Açaí Tradicional 700ml", categoria: "Açaí", preco: 28.00, estoque: 60, statusEstoque: "Estoque Normal" },
        { sku: "SKU-AC04", nome: "Açaí Trufado Especial 500ml", categoria: "Açaí", preco: 26.50, estoque: 45, statusEstoque: "Estoque Normal" },
        { sku: "SKU-AC06", nome: "Barca de Açaí Família 1.2L", categoria: "Açaí", preco: 55.00, estoque: 18, statusEstoque: "Estoque Baixo" },
        { sku: "SKU-SV01", nome: "Sorvete de Ninho Trufado 1L", categoria: "Sorvete", preco: 34.90, estoque: 25, statusEstoque: "Estoque Normal" },
        { sku: "SKU-SV02", nome: "Sorvete de Pistache Italiano 1L", categoria: "Sorvete", preco: 42.00, estoque: 20, statusEstoque: "Estoque Baixo" }
      ],
      pedidos: [
        { id: "PED-1001", data: "2026-04-05", idCliente: "CLI-001", cliente: "Mariana Silveira Costa", total: 517.70, vencimento: "2026-04-20", status: "Pago" },
        { id: "PED-1007", data: "2026-05-15", idCliente: "CLI-001", cliente: "Mariana Silveira Costa", total: 672.00, vencimento: "2026-05-30", status: "Pendente" },
        { id: "PED-1014", data: "2026-07-01", idCliente: "CLI-005", cliente: "Camila Martins Duarte", total: 630.00, vencimento: "2026-07-16", status: "Pendente" },
        { id: "PED-1020", data: "2026-09-22", idCliente: "CLI-010", cliente: "Felipe Nogueira Ramos", total: 227.90, vencimento: "2026-10-07", status: "Pago" }
      ]
    };

    let carrinhoItens = [];

    // Formatação de Moeda
    const fmtMoeda = (val) => Number(val).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });

    // Alternar Abas
    function setTab(tab) {
      ['dashboard', 'pedidos', 'estoque', 'clientes'].forEach(t => {
        document.getElementById(`tab-${t}`).classList.add('hidden');
        document.getElementById(`nav-${t}`).className = "w-full flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-slate-800/60 hover:text-white transition";
      });

      document.getElementById(`tab-${tab}`).classList.remove('hidden');
      document.getElementById(`nav-${tab}`).className = "w-full flex items-center gap-3 px-3 py-2.5 rounded-lg bg-purple-600/10 text-purple-400 font-semibold transition";

      const titulos = {
        dashboard: "Dashboard Executivo",
        pedidos: "Gestão e Lançamento de Pedidos",
        estoque: "Controle de Produtos & Estoque",
        clientes: "Carteira de Revendedores & Clientes"
      };
      document.getElementById('page-title').innerText = titulos[tab];
    }

    // Carregar dados da Planilha Real via Apps Script
    async function carregarDados() {
      if (!SCRIPT_URL || SCRIPT_URL === "SUA_URL_DO_APPS_SCRIPT_AQUI") {
        renderizarInterface();
        return;
      }
      try {
        const resp = await fetch(SCRIPT_URL);
        const data = await resp.json();
        if (data.pedidos && data.produtos) {
          bancoDados = data;
          renderizarInterface();
        }
      } catch (err) {
        console.error("Erro ao carregar dados do Sheets:", err);
      }
    }

    // Atualizar UI com os dados
    function renderizarInterface() {
      // 1. Métricas do Dashboard
      let totalFaturamento = 0;
      let totalRecebido = 0;
      let totalPendente = 0;

      bancoDados.pedidos.forEach(p => {
        totalFaturamento += p.total;
        if (p.status === "Pago") totalRecebido += p.total;
        if (p.status === "Pendente") totalPendente += p.total;
      });

      const ticketMedio = bancoDados.pedidos.length > 0 ? (totalFaturamento / bancoDados.pedidos.length) : 0;

      document.getElementById('kpi-faturamento').innerText = fmtMoeda(totalFaturamento);
      document.getElementById('kpi-recebido').innerText = fmtMoeda(totalRecebido);
      document.getElementById('kpi-pendente').innerText = fmtMoeda(totalPendente);
      document.getElementById('kpi-ticket').innerText = fmtMoeda(ticketMedio);
      document.getElementById('kpi-qtd-pedidos').innerText = `${bancoDados.pedidos.length} pedidos registrados`;

      // 2. Tabela de Alerta de Estoque
      const listaEstoque = document.getElementById('lista-alerta-estoque');
      listaEstoque.innerHTML = '';
      bancoDados.produtos
        .filter(prod => prod.estoque <= 25)
        .slice(0, 5)
        .forEach(prod => {
          listaEstoque.innerHTML += `
            <tr>
              <td class="py-2.5 font-mono text-xs text-slate-400">${prod.sku}</td>
              <td class="py-2.5 font-medium text-slate-700">${prod.nome}</td>
              <td class="py-2.5 text-right font-bold ${prod.estoque <= 20 ? 'text-rose-600' : 'text-amber-600'}">${prod.estoque} un</td>
            </tr>
          `;
        });

      // 3. Tabela de Últimos Pedidos
      const listaUltimos = document.getElementById('lista-ultimos-pedidos');
      listaUltimos.innerHTML = '';
      bancoDados.pedidos.slice(-5).reverse().forEach(ped => {
        listaUltimos.innerHTML += `
          <tr>
            <td class="py-2.5 font-mono text-xs font-semibold text-purple-600">${ped.id}</td>
            <td class="py-2.5 font-medium text-slate-700">${ped.cliente}</td>
            <td class="py-2.5 font-bold">${fmtMoeda(ped.total)}</td>
            <td class="py-2.5 text-right">
              <span class="text-xs px-2 py-0.5 rounded-full font-semibold ${ped.status === 'Pago' ? 'bg-emerald-50 text-emerald-700 border border-emerald-200' : 'bg-amber-50 text-amber-700 border border-amber-200'}">
                ${ped.status}
              </span>
            </td>
          </tr>
        `;
      });

      // 4. Tabela Completa de Pedidos
      filtrarPedidos();

      // 5. Tabela de Estoque
      const tabelaEstoque = document.getElementById('tabela-estoque-body');
      tabelaEstoque.innerHTML = '';
      bancoDados.produtos.forEach(prod => {
        tabelaEstoque.innerHTML += `
          <tr class="hover:bg-slate-50/80">
            <td class="p-4 font-mono text-xs text-slate-400">${prod.sku}</td>
            <td class="p-4 font-medium text-slate-800">${prod.nome}</td>
            <td class="p-4"><span class="px-2 py-0.5 text-xs rounded-md bg-slate-100 text-slate-600 font-medium">${prod.categoria}</span></td>
            <td class="p-4 text-right font-semibold">${fmtMoeda(prod.preco)}</td>
            <td class="p-4 text-center font-bold">${prod.estoque} un</td>
            <td class="p-4 text-center">
              <span class="text-xs px-2.5 py-1 rounded-full font-medium ${prod.estoque <= 20 ? 'bg-rose-50 text-rose-600 border border-rose-100' : 'bg-emerald-50 text-emerald-600 border border-emerald-100'}">
                ${prod.statusEstoque}
              </span>
            </td>
          </tr>
        `;
      });

      // 6. Tabela de Clientes
      const tabelaClientes = document.getElementById('tabela-clientes-body');
      tabelaClientes.innerHTML = '';
      bancoDados.clientes.forEach(cli => {
        tabelaClientes.innerHTML += `
          <tr class="hover:bg-slate-50/80">
            <td class="p-4 font-mono text-xs font-semibold text-purple-600">${cli.id}</td>
            <td class="p-4 font-medium text-slate-800">${cli.nome}</td>
            <td class="p-4 text-slate-500 font-mono text-xs">${cli.cpf}</td>
            <td class="p-4 text-slate-600">${cli.contato}</td>
            <td class="p-4 text-slate-500 text-xs">${cli.endereco}, ${cli.cidade}</td>
          </tr>
        `;
      });

      // Popula selects do Modal
      popularSelectsModal();
      lucide.createIcons();
    }

    function filtrarPedidos() {
      const q = (document.getElementById('filtro-pedido')?.value || '').toLowerCase();
      const body = document.getElementById('tabela-pedidos-body');
      body.innerHTML = '';
      
      bancoDados.pedidos
        .filter(p => p.cliente.toLowerCase().includes(q) || p.id.toLowerCase().includes(q))
        .reverse()
        .forEach(p => {
          body.innerHTML += `
            <tr class="hover:bg-slate-50/80">
              <td class="p-4 font-mono font-semibold text-purple-600">${p.id}</td>
              <td class="p-4 text-slate-500">${p.data}</td>
              <td class="p-4 font-medium text-slate-800">${p.cliente}</td>
              <td class="p-4 text-slate-500">${p.vencimento}</td>
              <td class="p-4 font-bold text-slate-900">${fmtMoeda(p.total)}</td>
              <td class="p-4 text-center">
                <span class="text-xs px-2.5 py-1 rounded-full font-semibold ${p.status === 'Pago' ? 'bg-emerald-50 text-emerald-700 border border-emerald-200' : 'bg-amber-50 text-amber-700 border border-amber-200'}">
                  ${p.status}
                </span>
              </td>
            </tr>
          `;
        });
    }

    function popularSelectsModal() {
      const selCli = document.getElementById('modal-select-cliente');
      selCli.innerHTML = '<option value="">Selecione o Cliente...</option>';
      bancoDados.clientes.forEach(c => {
        selCli.innerHTML += `<option value="${c.id}">${c.nome} (${c.id})</option>`;
      });

      const selProd = document.getElementById('modal-select-produto');
      selProd.innerHTML = '<option value="">Escolha um item...</option>';
      bancoDados.produtos.forEach(p => {
        selProd.innerHTML += `<option value="${p.sku}">${p.nome} - ${fmtMoeda(p.preco)} (Estoque: ${p.estoque})</option>`;
      });
    }

    // Modal de Criação de Pedido
    function abrirModalPedido() {
      carrinhoItens = [];
      atualizarCarrinhoModal();
      document.getElementById('modal-vencimento').valueAsDate = new Date(Date.now() + 15 * 86400000);
      document.getElementById('modal-pedido').classList.remove('hidden');
      lucide.createIcons();
    }

    function fecharModalPedido() {
      document.getElementById('modal-pedido').classList.add('hidden');
    }

    function adicionarItemAoPedido() {
      const sku = document.getElementById('modal-select-produto').value;
      const qtd = parseInt(document.getElementById('modal-qtd-item').value) || 1;
      if (!sku) return alert("Selecione um produto primeiro!");

      const prod = bancoDados.produtos.find(p => p.sku === sku);
      if (!prod) return;

      const itemExistente = carrinhoItens.find(i => i.sku === sku);
      if (itemExistente) {
        itemExistente.quantidade += qtd;
        itemExistente.subtotal = itemExistente.quantidade * itemExistente.precoUnitario;
      } else {
        carrinhoItens.push({
          sku: prod.sku,
          nome: prod.nome,
          categoria: prod.categoria,
          quantidade: qtd,
          precoUnitario: prod.preco,
          subtotal: prod.preco * qtd
        });
      }

      atualizarCarrinhoModal();
    }

    function removerItemCarrinho(index) {
      carrinhoItens.splice(index, 1);
      atualizarCarrinhoModal();
    }

    function atualizarCarrinhoModal() {
      const tbody = document.getElementById('modal-tabela-carrinho');
      tbody.innerHTML = '';
      let total = 0;

      if (carrinhoItens.length === 0) {
        tbody.innerHTML = '<tr><td colspan="5" class="p-4 text-center text-xs text-slate-400">Nenhum produto adicionado ainda.</td></tr>';
      } else {
        carrinhoItens.forEach((item, idx) => {
          total += item.subtotal;
          tbody.innerHTML += `
            <tr>
              <td class="p-2.5 font-medium text-slate-700">${item.nome}</td>
              <td class="p-2.5 text-center font-bold">${item.quantidade}</td>
              <td class="p-2.5 text-right">${fmtMoeda(item.precoUnitario)}</td>
              <td class="p-2.5 text-right font-bold">${fmtMoeda(item.subtotal)}</td>
              <td class="p-2.5 text-center">
                <button onclick="removerItemCarrinho(${idx})" class="text-rose-500 hover:text-rose-700">
                  <i data-lucide="trash-2" class="w-4 h-4"></i>
                </button>
              </td>
            </tr>
          `;
        });
      }

      document.getElementById('modal-total-pedido').innerText = fmtMoeda(total);
      lucide.createIcons();
    }

    async function salvarPedido() {
      const idCliente = document.getElementById('modal-select-cliente').value;
      if (!idCliente) return alert("Selecione um cliente para fechar o pedido!");
      if (carrinhoItens.length === 0) return alert("Adicione pelo menos um produto ao pedido!");

      const cliente = bancoDados.clientes.find(c => c.id === idCliente);
      const total = carrinhoItens.reduce((acc, cur) => acc + cur.subtotal, 0);
      const vencimento = document.getElementById('modal-vencimento').value;

      const payload = {
        action: "criar_pedido",
        idCliente: cliente.id,
        nomeCliente: cliente.nome,
        total: total,
        vencimento: vencimento,
        status: "Pendente",
        itens: carrinhoItens
      };

      const btn = document.getElementById('btn-salvar-pedido');
      btn.disabled = true;
      btn.innerText = "Salvando...";

      if (SCRIPT_URL && SCRIPT_URL !== "SUA_URL_DO_APPS_SCRIPT_AQUI") {
        try {
          await fetch(SCRIPT_URL, {
            method: "POST",
            body: JSON.stringify(payload)
          });
        } catch (e) {
          console.warn("Erro ao sincronizar online, gravando localmente:", e);
        }
      }

      // Registro local imediato
      const novoId = "PED-" + (1001 + bancoDados.pedidos.length);
      bancoDados.pedidos.push({
        id: novoId,
        data: new Date().toISOString().substring(0, 10),
        idCliente: cliente.id,
        cliente: cliente.nome,
        total: total,
        vencimento: vencimento,
        status: "Pendente"
      });

      // Baixa no estoque local
      carrinhoItens.forEach(item => {
        const p = bancoDados.produtos.find(prod => prod.sku === item.sku);
        if (p) p.estoque = Math.max(0, p.estoque - item.quantidade);
      });

      fecharModalPedido();
      btn.disabled = false;
      btn.innerHTML = '<i data-lucide="check" class="w-4 h-4"></i> Salvar e Registrar';
      renderizarInterface();
      alert(`Pedido ${novoId} gerado e lançado com sucesso!`);
    }

    // Inicialização
    document.getElementById('current-date').innerText = new Date().toLocaleDateString('pt-BR', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
    carregarDados();
  </script>
</body>
</html>
