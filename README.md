<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f7fa;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #007B8A;
    }
    #login-container {
      max-width: 400px;
      margin: 100px auto;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      text-align: center;
    }
    #login-container input {
      display: block;
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    #login-container button {
      padding: 10px 20px;
      background-color: #007B8A;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #main-container {
      display: none;
    }
    #cliente-form {
      text-align: center;
      margin: 20px 0;
    }
    #cliente-form input {
      padding: 10px;
      width: 200px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    #cliente-form button {
      padding: 10px 15px;
      background-color: #007B8A;
      color: white;
      border: none;
      border-radius: 5px;
      margin-left: 10px;
      cursor: pointer;
    }
    #clientes-dropdown-container {
      text-align: center;
      margin-bottom: 20px;
    }
    #clientes-dropdown {
      padding: 10px;
      width: 300px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: #fff;
      font-size: 16px;
      cursor: pointer;
      outline: none;
    }
    #clientes-dropdown:focus {
      border-color: #007B8A;
      box-shadow: 0 0 5px rgba(0, 123, 138, 0.3);
    }
    .controle-aba {
      display: flex;
      gap: 10px;
      align-items: center;
      justify-content: center;
      background: #fff;
      padding: 5px 10px;
      border-radius: 5px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
      margin: 10px auto;
      max-width: 300px;
    }
    .controle-aba input[type="checkbox"] {
      margin: 0;
      width: 14px;
      height: 14px;
      cursor: pointer;
    }
    .controle-aba label {
      font-size: 12px;
      margin-left: 4px;
      cursor: pointer;
    }
    .controle-aba span {
      cursor: pointer;
      font-size: 14px;
      padding: 3px 6px;
      border-radius: 3px;
      background-color: #e8e8e8;
      transition: background-color 0.2s;
    }
    .controle-aba span:hover {
      background-color: #d8d8d8;
    }
    .tarefas {
      max-width: 700px;
      margin: 0 auto;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    }
    .categoria h3 {
      color: #007B8A;
      margin-top: 20px;
    }
    .btn-adicionar-tarefa {
      display: inline-block;
      padding: 8px 15px;
      background-color: #007B8A;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 12px;
      margin-bottom: 10px;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      margin: 10px 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .item-tarefa {
      display: flex;
      align-items: center;
      flex: 1;
      gap: 10px;
    }
    input[type="checkbox"] {
      margin-right: 10px;
      accent-color: #007B8A;
    }
    .checked {
      text-decoration: line-through;
      color: blue;
    }
    .botoes-tarefa span {
      margin-left: 10px;
      cursor: pointer;
      font-size: 14px;
    }
    .chart-container {
      max-width: 400px;
      margin: 40px auto 10px;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    }
    #botoes-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      margin: 20px auto;
      max-width: 300px;
    }
    #btn-desmarcar, #btn-balance, #btn-clientes-com-pendencia {
      display: block;
      padding: 10px 20px;
      background-color: #00BFA6;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      width: 100%;
      text-align: center;
    }
    #btn-clientes-com-pendencia {
      background-color: #FF6B6B;
    }
    #btn-desmarcar[disabled] {
      background-color: #ccc;
      cursor: not-allowed;
    }
    .pendente {
      font-weight: bold;
      color: red;
    }
    .status-tag {
      padding: 2px 8px;
      border-radius: 3px;
      font-size: 12px;
      color: white;
    }
    .status-atrasado {
      background-color: #FF6B6B;
    }
    .status-em-dia {
      background-color: #00BFA6;
    }
    .status-feito {
      background-color: #007B8A;
    }
    .prazo-input {
      padding: 5px;
      width: 60px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 12px;
    }
    textarea {
      width: 100%;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    #anotacoes-container {
      max-width: 700px;
      margin: 0 auto 30px;
    }
    #filtros {
      text-align: center;
      margin: 10px 0;
      display: flex;
      justify-content: center;
      gap: 10px;
    }
    .btn-filtro {
      display: inline-block;
      padding: 10px 20px;
      background-color: #00BFA6;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    @media (max-width: 600px) {
      .tarefas { padding: 10px; }
      .chart-container { max-width: 100%; }
      #cliente-form input { width: 150px; }
      .btn-filtro { padding: 8px 15px; font-size: 14px; }
      #botoes-container { max-width: 100%; padding: 0 10px; }
      #filtros { flex-wrap: wrap; }
      #login-container { margin: 50px auto; padding: 15px; }
      #clientes-dropdown { width: 100%; max-width: 250px; }
      .controle-aba {
        padding: 3px 5px;
      }
      .controle-aba label { font-size: 10px; }
      .controle-aba span { font-size: 12px; padding: 2px 4px; }
      .controle-aba input[type="checkbox"] { width: 12px; height: 12px; }
      .prazo-input { width: 50px; font-size: 10px; }
      .item-tarefa { flex-wrap: wrap; gap: 5px; }
      .status-tag { font-size: 10px; padding: 2px 6px; }
      .btn-adicionar-tarefa { padding: 6px 12px; font-size: 10px; }
    }
  </style>
</head>
<body>
  <div id="login-container">
    <h2>Gestão de Tarefas - FORTTE</h2>
    <input type="text" id="usuario" placeholder="Usuário">
    <input type="password" id="senha" placeholder="Senha">
    <button onclick="fazerLogin()">Entrar</button>
  </div>

  <div id="main-container">
    <div style="display: flex; justify-content: center; align-items: center; flex-direction: column;">
      <h1>Gestão de Tarefas - FORTTE</h1>
    </div>

    <div id="cliente-form">
      <input type="text" id="novoCliente" placeholder="Novo Cliente">
      <button onclick="adicionarCliente()">Adicionar Cliente</button>
    </div>

    <div id="clientes-dropdown-container">
      <select id="clientes-dropdown" onchange="selecionarCliente(this.value)"></select>
      <div id="controles-cliente"></div>
    </div>

    <div id="filtros">
      <button class="btn-filtro" onclick="renderizarTarefas('todos')">Todas</button>
      <button class="btn-filtro" onclick="renderizarTarefas('pendentes')">Pendentes</button>
      <button class="btn-filtro" onclick="renderizarTarefas('concluidas')">Concluídas</button>
    </div>

    <div class="tarefas" id="tarefas-container"></div>
    <div id="anotacoes-container">
      <h2>Anotações Gerais</h2>
      <textarea id="anotacoes" rows="5" placeholder="Digite suas observações aqui..."></textarea>
    </div>

    <div class="chart-container">
      <h2>Status Geral</h2>
      <canvas id="graficoGeral"></canvas>
    </div>

    <div id="botoes-container">
      <button id="btn-desmarcar" onclick="desmarcarTodas()">Desmarcar todas as tarefas</button>
      <button id="btn-balance" onclick="gerarBalancoPDF()">Imprimir Balanço Mensal</button>
      <button id="btn-clientes-com-pendencia" onclick="mostrarClientesComPendencia()">Clientes com Pendência</button>
    </div>
  </div>

  <script>
    let tarefasModelo = JSON.parse(localStorage.getItem('tarefasModelo')) || [
      { categoria: 'Departamento Fiscal', tarefas: [
        { nome: 'Emissão de notas fiscais', prazoDia: null },
        { nome: 'Apuração de impostos', prazoDia: null },
        { nome: 'Entrega de obrigações acessórias', prazoDia: null },
        { nome: 'Geração de DAS', prazoDia: null }
      ] },
      { categoria: 'Departamento Contábil', tarefas: [
        { nome: 'Escrituração contábil', prazoDia: null },
        { nome: 'Emissão de balancetes', prazoDia: null },
        { nome: 'DRE', prazoDia: null },
        { nome: 'SPED Contábil', prazoDia: null }
      ] },
      { categoria: 'Departamento Pessoal', tarefas: [
        { nome: 'Admissão e demissão', prazoDia: null },
        { nome: 'Folha de pagamento', prazoDia: null },
        { nome: 'Guias INSS/FGTS', prazoDia: null },
        { nome: 'eSocial', prazoDia: null }
      ] },
      { categoria: 'Departamento Financeiro', tarefas: [
        { nome: 'Contas a pagar/receber', prazoDia: null },
        { nome: 'Relatórios gerenciais', prazoDia: null },
        { nome: 'Planejamento tributário', prazoDia: null },
        { nome: 'Análise de lucratividade', prazoDia: null }
      ] }
    ];

    const usuarios = {
      'Elvis': { senha: '12345', tipo: 'admin' },
      'Adriano': { senha: '654321', tipo: 'restrito' }
    };

    let clientes = JSON.parse(localStorage.getItem('clientes')) || {};
    let prazosGlobais = JSON.parse(localStorage.getItem('prazosGlobais')) || {};
    let clienteAtual = null;
    let usuarioAtual = null;

    // Migrar chaves de prazosGlobais para novos nomes de categorias
    const categoriasAntigasParaNovas = {
      'Fiscal': 'Departamento Fiscal',
      'Contábil': 'Departamento Contábil',
      'DP': 'Departamento Pessoal',
      'Financeiro': 'Departamento Financeiro'
    };
    Object.keys(prazosGlobais).forEach(chave => {
      const [categoriaAntiga, nomeTarefa] = chave.split(':');
      if (categoriasAntigasParaNovas[categoriaAntiga]) {
        const novaChave = `${categoriasAntigasParaNovas[categoriaAntiga]}:${nomeTarefa}`;
        prazosGlobais[novaChave] = prazosGlobais[chave];
        delete prazosGlobais[chave];
      }
    });
    salvarPrazosGlobais();

    const grafico = new Chart(document.getElementById('graficoGeral'), {
      type: 'doughnut',
      data: {
        labels: ['Concluídas', 'Pendentes'],
        datasets: [{
          data: [0, 100],
          backgroundColor: ['#00BFA6', '#FF6B6B']
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: { position: 'bottom' }
        }
      }
    });

    function fazerLogin() {
      const usuario = document.getElementById('usuario').value.trim();
      const senha = document.getElementById('senha').value;
      if (usuarios[usuario] && usuarios[usuario].senha === senha) {
        usuarioAtual = usuario;
        document.getElementById('login-container').style.display = 'none';
        document.getElementById('main-container').style.display = 'block';
        ajustarInterface();
        atualizarDropdown();
        renderizarTarefas();
        atualizarGrafico();
      } else {
        alert('Usuário ou senha incorretos.');
      }
    }

    function ajustarInterface() {
      const clienteForm = document.getElementById('cliente-form');
      const btnDesmarcar = document.getElementById('btn-desmarcar');
      const btnBalance = document.getElementById('btn-balance');
      const btnClientesPendencia = document.getElementById('btn-clientes-com-pendencia');

      if (clienteForm && btnDesmarcar && btnBalance && btnClientesPendencia) {
        if (usuarioAtual === 'Adriano') {
          clienteForm.style.display = 'none';
          btnDesmarcar.disabled = true;
        } else {
          clienteForm.style.display = 'block';
          btnDesmarcar.disabled = false;
        }
      } else {
        console.error('Um ou mais elementos não foram encontrados:', {
          clienteForm, btnDesmarcar, btnBalance, btnClientesPendencia
        });
      }
    }

    function isTarefaAtrasada(tarefa) {
      if (!tarefa.prazoDia || tarefa.feito) return false;
      const hoje = new Date();
      const diaAtual = hoje.getDate();
      return diaAtual > tarefa.prazoDia;
    }

    function sincronizarTarefasEPrazos() {
      Object.values(clientes).forEach(cliente => {
        const novasTarefasPorCategoria = tarefasModelo.map(c => ({
          categoria: c.categoria,
          tarefas: c.tarefas.map(t => {
            const tarefaExistente = cliente.tarefas
              .find(cat => cat.categoria === c.categoria)?.tarefas
              .find(tarefa => tarefa.nome === t.nome);
            return {
              nome: t.nome,
              feito: tarefaExistente ? tarefaExistente.feito : false,
              prazoDia: prazosGlobais[`${c.categoria}:${t.nome}`] || null
            };
          })
        }));
        cliente.tarefas = novasTarefasPorCategoria;
      });
      salvarClientes();
    }

    function salvarTarefasModelo() {
      try {
        localStorage.setItem('tarefasModelo', JSON.stringify(tarefasModelo));
      } catch (e) {
        console.error('Erro ao salvar tarefasModelo no localStorage:', e);
        alert('Não foi possível salvar o modelo de tarefas.');
      }
    }

    function salvarPrazosGlobais() {
      try {
        localStorage.setItem('prazosGlobais', JSON.stringify(prazosGlobais));
      } catch (e) {
        console.error('Erro ao salvar prazosGlobais no localStorage:', e);
        alert('Não foi possível salvar os prazos globais.');
      }
    }

    function adicionarTarefaGlobal(categoria, nomeTarefa, prazoDia) {
      if (!nomeTarefa) {
        alert('O nome da tarefa não pode ser vazio.');
        return;
      }
      const categoriaModelo = tarefasModelo.find(c => c.categoria === categoria);
      if (categoriaModelo.tarefas.some(t => t.nome === nomeTarefa)) {
        alert('Tarefa já existe nesta categoria.');
        return;
      }
      categoriaModelo.tarefas.push({ nome: nomeTarefa, prazoDia: prazoDia });
      const chave = `${categoria}:${nomeTarefa}`;
      if (prazoDia !== null) {
        prazosGlobais[chave] = prazoDia;
      }
      sincronizarTarefasEPrazos();
      salvarTarefasModelo();
      salvarPrazosGlobais();
      renderizarTarefas();
      atualizarGrafico();
    }

    function adicionarCliente() {
      if (usuarioAtual !== 'Elvis') return;
      const nome = document.getElementById('novoCliente').value.trim();
      if (!nome) {
        alert('Informe o nome do cliente.');
        return;
      }
      if (clientes[nome]) {
        alert('Cliente já cadastrado.');
        return;
      }
      clientes[nome] = {
        tarefas: tarefasModelo.map(c => ({
          categoria: c.categoria,
          tarefas: c.tarefas.map(t => ({ nome: t.nome, feito: false, prazoDia: prazosGlobais[`${c.categoria}:${t.nome}`] || null }))
        })),
        notes: '',
        adrianoPermitido: false
      };
      document.getElementById('novoCliente').value = '';
      clienteAtual = nome;
      salvarClientes();
      atualizarDropdown();
      renderizarTarefas();
      atualizarGrafico();
    }

    function selecionarCliente(nome) {
      clienteAtual = nome || null;
      atualizarDropdown();
      renderizarTarefas();
    }

    function atualizarDropdown() {
      const dropdown = document.getElementById('clientes-dropdown');
      const controlesContainer = document.getElementById('controles-cliente');
      if (!dropdown || !controlesContainer) {
        console.error('Elementos de dropdown ou controles não encontrados.');
        return;
      }
      dropdown.innerHTML = '';
      controlesContainer.innerHTML = '';

      const clientesVisiveis = usuarioAtual === 'Elvis' 
        ? Object.keys(clientes) 
        : Object.keys(clientes).filter(nome => clientes[nome].adrianoPermitido);

      if (clientesVisiveis.length === 0) {
        const option = document.createElement('option');
        option.value = '';
        option.textContent = usuarioAtual === 'Elvis' ? 'Nenhum cliente cadastrado' : 'Nenhum cliente liberado';
        dropdown.appendChild(option);
        return;
      }

      const defaultOption = document.createElement('option');
      defaultOption.value = '';
      defaultOption.textContent = 'Selecione um cliente';
      dropdown.appendChild(defaultOption);

      clientesVisiveis.forEach(nome => {
        const option = document.createElement('option');
        option.value = nome;
        option.textContent = nome;
        if (nome === clienteAtual) {
          option.selected = true;
        }
        dropdown.appendChild(option);
      });

      if (usuarioAtual === 'Elvis' && clienteAtual && clientes[clienteAtual]) {
        const controleAba = document.createElement('div');
        controleAba.className = 'controle-aba';
        controleAba.setAttribute('data-cliente-controle', clienteAtual);

        const permissoes = document.createElement('div');
        const checkboxPerm = document.createElement('input');
        checkboxPerm.type = 'checkbox';
        checkboxPerm.id = `perm-adriano-${clienteAtual}`;
        checkboxPerm.checked = clientes[clienteAtual].adrianoPermitido || false;
        checkboxPerm.setAttribute('aria-label', `Permitir acesso de Adriano ao cliente ${clienteAtual}`);
        checkboxPerm.title = 'Permitir acesso para Adriano';
        checkboxPerm.onchange = () => {
          clientes[clienteAtual].adrianoPermitido = checkboxPerm.checked;
          salvarClientes();
          if (!checkboxPerm.checked && clienteAtual === clienteAtual) {
            clienteAtual = null;
            renderizarTarefas();
          }
          atualizarDropdown();
        };
        const labelPerm = document.createElement('label');
        labelPerm.setAttribute('for', `perm-adriano-${clienteAtual}`);
        labelPerm.textContent = 'Adriano';
        permissoes.appendChild(checkboxPerm);
        permissoes.appendChild(labelPerm);

        const editar = document.createElement('span');
        editar.textContent = '✏️';
        editar.setAttribute('aria-label', `Editar cliente ${clienteAtual}`);
        editar.title = 'Editar cliente';
        editar.tabIndex = 0;
        editar.onclick = (e) => {
          e.stopPropagation();
          const novoNome = prompt('Editar nome do cliente:', clienteAtual);
          if (novoNome && !clientes[novoNome]) {
            clientes[novoNome] = clientes[clienteAtual];
            delete clientes[clienteAtual];
            clienteAtual = novoNome;
            salvarClientes();
            atualizarDropdown();
            renderizarTarefas();
            atualizarGrafico();
          } else if (novoNome) {
            alert('Nome já existe.');
          }
        };
        editar.onkeydown = (e) => {
          if (e.key === 'Enter' || e.key === ' ') {
            e.stopPropagation();
            const novoNome = prompt('Editar nome do cliente:', clienteAtual);
            if (novoNome && !clientes[novoNome]) {
              clientes[novoNome] = clientes[clienteAtual];
              delete clientes[clienteAtual];
              clienteAtual = novoNome;
              salvarClientes();
              atualizarDropdown();
              renderizarTarefas();
              atualizarGrafico();
            } else if (novoNome) {
              alert('Nome já existe.');
            }
          }
        };

        const excluir = document.createElement('span');
        excluir.textContent = '❌';
        excluir.setAttribute('aria-label', `Excluir cliente ${clienteAtual}`);
        excluir.title = 'Excluir cliente';
        excluir.tabIndex = 0;
        excluir.onclick = (e) => {
          e.stopPropagation();
          if (confirm(`Deseja excluir o cliente "${clienteAtual}"?`)) {
            delete clientes[clienteAtual];
            salvarClientes();
            const nomes = Object.keys(clientes);
            clienteAtual = nomes.length ? nomes[0] : null;
            atualizarDropdown();
            renderizarTarefas();
            atualizarGrafico();
          }
        };
        excluir.onkeydown = (e) => {
          if (e.key === 'Enter' || e.key === ' ') {
            e.stopPropagation();
            if (confirm(`Deseja excluir o cliente "${clienteAtual}"?`)) {
              delete clientes[clienteAtual];
              salvarClientes();
              const nomes = Object.keys(clientes);
              clienteAtual = nomes.length ? nomes[0] : null;
              atualizarDropdown();
              renderizarTarefas();
              atualizarGrafico();
            }
          }
        };

        controleAba.appendChild(permissoes);
        controleAba.appendChild(editar);
        controleAba.appendChild(excluir);
        controlesContainer.appendChild(controleAba);
      }
    }

    function renderizarTarefas(filtro = 'todos') {
      const container = document.getElementById('tarefas-container');
      container.innerHTML = '';
      if (!clienteAtual || !clientes[clienteAtual]) {
        container.innerHTML = '<p>Selecione um cliente para visualizar as tarefas.</p>';
        document.getElementById('anotacoes').value = '';
        return;
      }
      if (usuarioAtual === 'Adriano' && !clientes[clienteAtual].adrianoPermitido) {
        container.innerHTML = '<p>Acesso não autorizado a este cliente.</p>';
        document.getElementById('anotacoes').value = '';
        return;
      }

      clientes[clienteAtual].tarefas.forEach((cat, ci) => {
        const div = document.createElement('div');
        div.className = 'categoria';
        const h3 = document.createElement('h3');
        h3.textContent = cat.categoria;
        div.appendChild(h3);

        if (usuarioAtual === 'Elvis') {
          const btnAdicionar = document.createElement('button');
          btnAdicionar.className = 'btn-adicionar-tarefa';
          btnAdicionar.textContent = 'Adicionar Tarefa';
          btnAdicionar.onclick = () => {
            const nomeTarefa = prompt('Nome da nova tarefa:');
            if (!nomeTarefa) return;
            const prazoDiaInput = prompt('Dia do prazo (1-31, deixe vazio para sem prazo):');
            let prazoDia = null;
            if (prazoDiaInput) {
              const dia = parseInt(prazoDiaInput);
              if (isNaN(dia) || dia < 1 || dia > 31) {
                alert('Por favor, insira um dia válido entre 1 e 31.');
                return;
              }
              prazoDia = dia;
            }
            adicionarTarefaGlobal(cat.categoria, nomeTarefa, prazoDia);
          };
          div.appendChild(btnAdicionar);
        }

        const ul = document.createElement('ul');
        const tarefasOrdenadas = [...cat.tarefas].sort((a, b) => {
          if (a.prazoDia === null && b.prazoDia === null) return 0;
          if (a.prazoDia === null) return 1;
          if (b.prazoDia === null) return -1;
          return a.prazoDia - b.prazoDia;
        });

        tarefasOrdenadas.forEach((tarefa, ti) => {
          if (filtro === 'pendentes' && tarefa.feito) return;
          if (filtro === 'concluidas' && !tarefa.feito) return;

          const li = document.createElement('li');
          const tarefaDiv = document.createElement('div');
          tarefaDiv.className = 'item-tarefa';
          if (tarefa.feito) tarefaDiv.classList.add('checked');

          const checkbox = document.createElement('input');
          checkbox.type = 'checkbox';
          checkbox.checked = tarefa.feito;
          checkbox.onchange = () => {
            tarefa.feito = checkbox.checked;
            salvarClientes();
            renderizarTarefas(filtro);
            atualizarGrafico();
          };

          const label = document.createElement('span');
          label.textContent = tarefa.nome;
          if (!tarefa.feito) label.classList.add('pendente');

          tarefaDiv.appendChild(checkbox);
          tarefaDiv.appendChild(label);

          if (tarefa.prazoDia || tarefa.feito) {
            const statusTag = document.createElement('span');
            statusTag.className = 'status-tag';
            if (tarefa.feito) {
              statusTag.classList.add('status-feito');
              statusTag.textContent = 'Feito';
            } else {
              statusTag.classList.add(isTarefaAtrasada(tarefa) ? 'status-atrasado' : 'status-em-dia');
              statusTag.textContent = isTarefaAtrasada(tarefa) ? 'Atrasado' : 'Em dia';
            }
            tarefaDiv.appendChild(statusTag);
          }

          if (usuarioAtual === 'Elvis') {
            const prazoInput = document.createElement('input');
            prazoInput.type = 'number';
            prazoInput.className = 'prazo-input';
            prazoInput.min = '1';
            prazoInput.max = '31';
            prazoInput.value = tarefa.prazoDia || '';
            prazoInput.placeholder = 'Dia';
            prazoInput.onchange = () => {
              const dia = parseInt(prazoInput.value);
              if (isNaN(dia) || dia < 1 || dia > 31) {
                alert('Por favor, insira um dia válido entre 1 e 31.');
                prazoInput.value = tarefa.prazoDia || '';
                return;
              }
              const chave = `${cat.categoria}:${tarefa.nome}`;
              prazosGlobais[chave] = dia;
              sincronizarTarefasEPrazos();
              salvarPrazosGlobais();
              renderizarTarefas(filtro);
            };
            tarefaDiv.appendChild(prazoInput);
          }

          const botoes = document.createElement('div');
          botoes.className = 'botoes-tarefa';

          if (usuarioAtual === 'Elvis') {
            const editarTarefa = document.createElement('span');
            editarTarefa.textContent = '✏️';
            editarTarefa.setAttribute('aria-label', 'Editar tarefa ' + tarefa.nome);
            editarTarefa.title = 'Editar tarefa';
            editarTarefa.onclick = () => {
              const novoNome = prompt('Editar tarefa:', tarefa.nome);
              if (novoNome) {
                const chaveAntiga = `${cat.categoria}:${tarefa.nome}`;
                tarefa.nome = novoNome;
                const chaveNova = `${cat.categoria}:${novoNome}`;
                if (prazosGlobais[chaveAntiga] !== undefined) {
                  prazosGlobais[chaveNova] = prazosGlobais[chaveAntiga];
                  delete prazosGlobais[chaveAntiga];
                }
                const tarefaModelo = tarefasModelo.find(c => c.categoria === cat.categoria).tarefas.find(t => t.nome === tarefa.nome);
                if (tarefaModelo) tarefaModelo.nome = novoNome;
                salvarTarefasModelo();
                salvarPrazosGlobais();
                salvarClientes();
                sincronizarTarefasEPrazos();
                renderizarTarefas(filtro);
              }
            };

            const excluirTarefa = document.createElement('span');
            excluirTarefa.textContent = '❌';
            excluirTarefa.setAttribute('aria-label', 'Excluir tarefa ' + tarefa.nome);
            excluirTarefa.title = 'Excluir tarefa';
            excluirTarefa.onclick = () => {
              if (confirm('Deseja excluir esta tarefa?')) {
                const chave = `${cat.categoria}:${tarefa.nome}`;
                delete prazosGlobais[chave];
                clientes[clienteAtual].tarefas[ci].tarefas.splice(ti, 1);
                tarefasModelo.find(c => c.categoria === cat.categoria).tarefas = tarefasModelo.find(c => c.categoria === cat.categoria).tarefas.filter(t => t.nome !== tarefa.nome);
                salvarTarefasModelo();
                salvarPrazosGlobais();
                sincronizarTarefasEPrazos();
                renderizarTarefas(filtro);
                atualizarGrafico();
              }
            };

            botoes.appendChild(editarTarefa);
            botoes.appendChild(excluirTarefa);
          }

          li.appendChild(tarefaDiv);
          li.appendChild(botoes);
          ul.appendChild(li);
        });

        div.appendChild(ul);
        container.appendChild(div);
      });

      const anotacoes = document.getElementById('anotacoes');
      anotacoes.value = clientes[clienteAtual]?.notes || '';
    }

    function salvarClientes() {
      try {
        localStorage.setItem('clientes', JSON.stringify(clientes));
      } catch (e) {
        console.error('Erro ao salvar no localStorage:', e);
        alert('Não foi possível salvar os dados.');
      }
    }

    function atualizarGrafico() {
      let total = 0, feitas = 0;
      const clientesVisiveis = usuarioAtual === 'Elvis' 
        ? Object.values(clientes) 
        : Object.values(clientes).filter(cli => cli.adrianoPermitido);
      clientesVisiveis.forEach(cli => {
        cli.tarefas.forEach(cat => {
          cat.tarefas.forEach(t => {
            total++;
            if (t.feito) feitas++;
          });
        });
      });

      const percFeitas = total ? Math.round((feitas / total) * 100) : 0;
      grafico.data.datasets[0].data = [percFeitas, 100 - percFeitas];
      grafico.update();
    }

    function desmarcarTodas() {
      if (usuarioAtual !== 'Elvis') {
        alert('Apenas o administrador pode desmarcar todas as tarefas.');
        return;
      }
      if (!confirm('Deseja realmente desmarcar todas as tarefas?')) return;
      Object.values(clientes).forEach(cli => {
        cli.tarefas.forEach(cat => {
          cat.tarefas.forEach(t => {
            t.feito = false;
          });
        });
      });
      salvarClientes();
      renderizarTarefas();
      atualizarGrafico();
    }

    function gerarBalancoPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      const dataAtual = new Date().toLocaleDateString('pt-BR');
      let y = 10;

      doc.setFontSize(18);
      doc.text('Balanço Mensal de Tarefas - FORTTE', 10, y);
      y += 10;

      doc.setFontSize(12);
      doc.text(`Data: ${dataAtual}`, 10, y);
      y += 10;

      let totalTarefas = 0;
      let totalFeitas = 0;

      Object.keys(clientes).forEach(cliente => {
        if (y > 270) {
          doc.addPage();
          y = 10;
        }
        doc.setFontSize(16);
        doc.setTextColor(0, 0, 0);
        doc.text(cliente, 10, y);
        y += 10;

        clientes[cliente].tarefas.forEach(categoria => {
          if (y > 270) {
            doc.addPage();
            y = 10;
          }
          doc.setFontSize(14);
          doc.text(categoria.categoria, 10, y);
          y += 10;

          const tarefasOrdenadas = [...categoria.tarefas].sort((a, b) => {
            if (a.prazoDia === null && b.prazoDia === null) return 0;
            if (a.prazoDia === null) return 1;
            if (b.prazoDia === null) return -1;
            return a.prazoDia - b.prazoDia;
          });

          tarefasOrdenadas.forEach(tarefa => {
            if (y > 270) {
              doc.addPage();
              y = 10;
            }
            totalTarefas++;
            const status = tarefa.feito ? 'Concluída' : 'Pendente';
            if (tarefa.feito) totalFeitas++;

            doc.setFontSize(12);
            let tarefaText = `${tarefa.nome} - ${status}`;
            if (tarefa.prazoDia) {
              tarefaText += ` (Prazo: Dia ${tarefa.prazoDia}, ${tarefa.feito ? 'Feito' : (isTarefaAtrasada(tarefa) ? 'Atrasado' : 'Em dia')})`;
            }
            if (!tarefa.feito) {
              doc.setTextColor(255, 0, 0);
            } else {
              doc.setTextColor(0, 0, 255);
            }
            doc.text(tarefaText, 10, y);
            y += 8;
          });
        });

        if (y > 270) {
          doc.addPage();
          y = 10;
        }
        doc.setFontSize(12);
        doc.setTextColor(0, 0, 0);
        doc.text(`Anotações: ${clientes[cliente].notes || 'Nenhuma'}`, 10, y);
        y += 10;
      });

      if (y > 270) {
        doc.addPage();
        y = 10;
      }
      doc.setFontSize(14);
      doc.setTextColor(0, 0, 0);
      doc.text(`Tarefas Concluídas: ${totalFeitas} de ${totalTarefas}`, 10, y);
      y += 8;
      const perc = totalTarefas ? Math.round((totalFeitas / totalTarefas) * 100) : 0;
      doc.text(`Porcentagem Concluída: ${perc}%`, 10, y);

      doc.save('Balanço_Mensal.pdf');
    }

    function mostrarClientesComPendencia() {
      const pendentes = [];
      Object.entries(clientes).forEach(([nome, cliente]) => {
        const temPendencias = cliente.tarefas.some(cat => cat.tarefas.some(t => !t.feito));
        if (temPendencias) pendentes.push(nome);
      });

      alert(pendentes.length ? 'Clientes com Pendência:\n' + pendentes.join('\n') : 'Nenhum cliente com pendências.');
    }

    document.getElementById('anotacoes').addEventListener('input', () => {
      if (!clienteAtual) return;
      clientes[clienteAtual].notes = document.getElementById('anotacoes').value;
      salvarClientes();
    });
  </script>
</body>
</html>
