<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hub: Partidas de Futebol</title>
  <style>
    /* Reset & Variáveis */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    :root {
      --bg-color: #0f172a;
      --card-bg: #1e293b;
      --card-border: #334155;
      --primary: #22c55e;
      --primary-hover: #16a34a;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --youtube-tag: #ef4444;
      --sheets-tag: #10b981;
      --link-tag: #3b82f6;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      display: flex;
      justify-content: center;
      min-height: 100vh;
      padding-bottom: 80px;
    }

    .app-container {
      width: 100%;
      max-width: 480px;
      background-color: #0f172a;
      min-height: 100vh;
      position: relative;
      display: flex;
      flex-direction: column;
      border-left: 1px solid var(--card-border);
      border-right: 1px solid var(--card-border);
    }

    /* Header */
    .header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px;
      background-color: #1e293b;
      border-bottom: 1px solid var(--card-border);
      position: sticky;
      top: 0;
      z-index: 10;
    }

    .header-title-container {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .btn-icon {
      background: none;
      border: none;
      color: var(--text-main);
      font-size: 1.2rem;
      cursor: pointer;
      padding: 4px;
      border-radius: 4px;
    }

    .btn-icon:hover {
      background-color: var(--card-border);
    }

    .header-title {
      font-size: 1.1rem;
      font-weight: 700;
      letter-spacing: 0.5px;
    }

    .header-actions {
      display: flex;
      gap: 8px;
    }

    /* Search Bar Overlay */
    .search-bar {
      display: none;
      padding: 12px 16px;
      background-color: #1e293b;
      border-bottom: 1px solid var(--card-border);
    }

    .search-bar.active {
      display: block;
    }

    .search-input {
      width: 100%;
      padding: 8px 12px;
      border-radius: 8px;
      border: 1px solid var(--card-border);
      background-color: var(--bg-color);
      color: var(--text-main);
      outline: none;
    }

    /* Filtros Rápidos */
    .filters-section {
      padding: 16px;
      border-bottom: 1px solid var(--card-border);
    }

    .section-subtitle {
      font-size: 0.8rem;
      color: var(--text-muted);
      text-transform: uppercase;
      margin-bottom: 10px;
      font-weight: 600;
    }

    .filter-pills {
      display: flex;
      gap: 8px;
      overflow-x: auto;
      padding-bottom: 4px;
    }

    .filter-pills::-webkit-scrollbar {
      display: none;
    }

    .pill {
      background-color: var(--card-bg);
      border: 1px solid var(--card-border);
      color: var(--text-muted);
      padding: 8px 14px;
      border-radius: 20px;
      font-size: 0.85rem;
      white-space: nowrap;
      cursor: pointer;
      transition: all 0.2s;
    }

    .pill.active {
      background-color: var(--primary);
      color: #000;
      font-weight: 600;
      border-color: var(--primary);
    }

    /* Lista de Conteúdos */
    .content-section {
      padding: 16px;
      flex: 1;
    }

    .cards-container {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-top: 12px;
    }

    .card {
      background-color: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 12px;
      padding: 14px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      transition: transform 0.2s, border-color 0.2s;
    }

    .card:hover {
      border-color: var(--text-muted);
    }

    .card-header {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.75rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    .tag-youtube { color: var(--youtube-tag); }
    .tag-sheets { color: var(--sheets-tag); }
    .tag-link { color: var(--link-tag); }

    .card-title {
      font-size: 0.95rem;
      font-weight: 600;
      line-height: 1.3;
    }

    .card-meta {
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    .card-action {
      align-self: flex-end;
      background-color: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--card-border);
      color: var(--text-main);
      padding: 6px 14px;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      text-decoration: none;
      transition: background-color 0.2s;
    }

    .card-action:hover {
      background-color: var(--primary);
      color: #000;
      border-color: var(--primary);
    }

    /* Floating Action Button (FAB) */
    .fab-container {
      position: fixed;
      bottom: 75px;
      left: 50%;
      transform: translateX(-50%);
      width: 100%;
      max-width: 480px;
      display: flex;
      justify-content: flex-end;
      padding-right: 16px;
      pointer-events: none;
    }

    .btn-fab {
      pointer-events: auto;
      background-color: var(--primary);
      color: #000;
      border: none;
      padding: 12px 20px;
      border-radius: 30px;
      font-weight: 700;
      font-size: 0.9rem;
      box-shadow: 0 4px 12px rgba(34, 197, 94, 0.3);
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 6px;
      transition: transform 0.2s, background-color 0.2s;
    }

    .btn-fab:hover {
      background-color: var(--primary-hover);
      transform: scale(1.03);
    }

    /* Bottom Navigation */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      width: 100%;
      max-width: 480px;
      background-color: #1e293b;
      border-top: 1px solid var(--card-border);
      display: flex;
      justify-content: space-around;
      padding: 10px 0;
      z-index: 10;
    }

    .nav-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      color: var(--text-muted);
      text-decoration: none;
      font-size: 0.75rem;
      cursor: pointer;
    }

    .nav-item.active {
      color: var(--primary);
      font-weight: 600;
    }

    .nav-icon {
      font-size: 1.2rem;
    }

    /* Modal Simples */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background-color: rgba(0,0,0,0.8);
      z-index: 100;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .modal.active {
      display: flex;
    }

    .modal-content {
      background-color: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 12px;
      padding: 20px;
      width: 100%;
      max-width: 400px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .modal-title {
      font-size: 1.1rem;
      font-weight: 700;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .form-group label {
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    .form-group input, .form-group select {
      padding: 10px;
      border-radius: 6px;
      border: 1px solid var(--card-border);
      background-color: var(--bg-color);
      color: var(--text-main);
      outline: none;
    }

    .modal-actions {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
    }

    .btn-secondary {
      background-color: transparent;
      border: 1px solid var(--card-border);
      color: var(--text-main);
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
    }

    .btn-primary {
      background-color: var(--primary);
      border: none;
      color: #000;
      font-weight: 600;
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <div class="app-container">

    <!-- Header -->
    <header class="header">
      <div class="header-title-container">
        <button class="btn-icon" title="Voltar">←</button>
        <h1 class="header-title">HUB: PARTIDAS DE FUTEBOL</h1>
      </div>
      <div class="header-actions">
        <button class="btn-icon" id="btn-search" title="Buscar">🔍</button>
        <button class="btn-icon" title="Configurações">⚙️</button>
      </div>
    </header>

    <!-- Search Bar -->
    <div class="search-bar" id="search-bar">
      <input type="text" class="search-input" id="search-input" placeholder="Buscar por palavra-chave...">
    </div>

    <!-- Filtros Rápidos -->
    <section class="filters-section">
      <div class="section-subtitle">Filtros Rápidos (Formato)</div>
      <div class="filter-pills">
        <button class="pill active" data-filter="todos">Todos (<span id="count-todos">5</span>)</button>
        <button class="pill" data-filter="video">🎥 Vídeos (<span id="count-video">2</span>)</button>
        <button class="pill" data-filter="sheet">📊 Planilhas (<span id="count-sheet">2</span>)</button>
        <button class="pill" data-filter="link">🔗 Links (<span id="count-link">1</span>)</button>
      </div>
    </section>

    <!-- Conteúdos Centralizados -->
    <main class="content-section">
      <div class="section-subtitle">Conteúdos Centralizados</div>
      <div class="cards-container" id="cards-container">
        
        <!-- Card 1: YouTube -->
        <div class="card" data-type="video">
          <div class="card-header tag-youtube">🎥 YouTube</div>
          <div class="card-title">Melhores Momentos: Flamengo x Palmeiras</div>
          <div class="card-meta">⏱️ 12 min • Canal Oficial GE</div>
          <a href="https://youtube.com" target="_blank" class="card-action">▶ Assistir</a>
        </div>

        <!-- Card 2: Google Sheets -->
        <div class="card" data-type="sheet">
          <div class="card-header tag-sheets">📊 Google Sheets</div>
          <div class="card-title">Tabela do Campeonato & Classificação</div>
          <div class="card-meta">🗓️ Atualizado hoje • Google Drive</div>
          <a href="https://docs.google.com/spreadsheets" target="_blank" class="card-action">↗ Abrir</a>
        </div>

        <!-- Card 3: Google Sheets -->
        <div class="card" data-type="sheet">
          <div class="card-header tag-sheets">📊 Google Sheets</div>
          <div class="card-title">Controle Financeiro: Ingressos e Viagens</div>
          <div class="card-meta">💵 Planilha Pessoal • Google Drive</div>
          <a href="https://docs.google.com/spreadsheets" target="_blank" class="card-action">↗ Abrir</a>
        </div>

        <!-- Card 4: YouTube -->
        <div class="card" data-type="video">
          <div class="card-header tag-youtube">🎥 YouTube</div>
          <div class="card-title">Análise Tática do Próximo Adversário</div>
          <div class="card-meta">⏱️ 18 min • Análise Tática FC</div>
          <a href="https://youtube.com" target="_blank" class="card-action">▶ Assistir</a>
        </div>

        <!-- Card 5: Link Externo -->
        <div class="card" data-type="link">
          <div class="card-header tag-link">🔗 Link Externo</div>
          <div class="card-title">Escalação Provável e Notícias do Dia</div>
          <div class="card-meta">🌐 ge.globo.com</div>
          <a href="https://ge.globo.com" target="_blank" class="card-action">↗ Acessar</a>
        </div>

      </div>
    </main>

    <!-- FAB -->
    <div class="fab-container">
      <button class="btn-fab" id="btn-add">+ Adicionar Conteúdo</button>
    </div>

    <!-- Bottom Navigation -->
    <nav class="bottom-nav">
      <a href="#" class="nav-item">
        <span class="nav-icon">🏠</span>
        <span>Início</span>
      </a>
      <a href="#" class="nav-item active">
        <span class="nav-icon">⚽</span>
        <span>Hubs</span>
      </a>
      <a href="#" class="nav-item">
        <span class="nav-icon">⭐</span>
        <span>Salvos</span>
      </a>
      <a href="#" class="nav-item">
        <span class="nav-icon">👤</span>
        <span>Perfil</span>
      </a>
    </nav>

  </div>

  <!-- Modal para Adicionar Conteúdo -->
  <div class="modal" id="modal-add">
    <div class="modal-content">
      <h2 class="modal-title">Adicionar Novo Conteúdo</h2>
      <div class="form-group">
        <label for="input-title">Título</label>
        <input type="text" id="input-title" placeholder="Ex: Análise pós-jogo">
      </div>
      <div class="form-group">
        <label for="select-type">Formato / Origem</label>
        <select id="select-type">
          <option value="video">🎥 YouTube (Vídeo)</option>
          <option value="sheet">📊 Google Sheets (Planilha)</option>
          <option value="link">🔗 Link Externo (Notícia/Artigo)</option>
        </select>
      </div>
      <div class="form-group">
        <label for="input-meta">Descrição / Detalhes</label>
        <input type="text" id="input-meta" placeholder="Ex: 15 min • Canal X ou Link original">
      </div>
      <div class="modal-actions">
        <button class="btn-secondary" id="btn-modal-cancel">Cancelar</button>
        <button class="btn-primary" id="btn-modal-save">Salvar</button>
      </div>
    </div>
  </div>

  <script>
    // Toggle Search Bar
    const btnSearch = document.getElementById('btn-search');
    const searchBar = document.getElementById('search-bar');
    const searchInput = document.getElementById('search-input');

    btnSearch.addEventListener('click', () => {
      searchBar.classList.toggle('active');
      if (searchBar.classList.contains('active')) {
        searchInput.focus();
      }
    });

    // Filtros por Tipo
    const pills = document.querySelectorAll('.pill');
    const cards = document.querySelectorAll('.card');

    pills.forEach(pill => {
      pill.addEventListener('click', () => {
        pills.forEach(p => p.classList.remove('active'));
        pill.classList.add('active');

        const filter = pill.getAttribute('data-filter');

        cards.forEach(card => {
          if (filter === 'todos' || card.getAttribute('data-type') === filter) {
            card.style.display = 'flex';
          } else {
            card.style.display = 'none';
          }
        });
      });
    });

    // Busca por Texto
    searchInput.addEventListener('input', (e) => {
      const query = e.target.value.toLowerCase();
      cards.forEach(card => {
        const title = card.querySelector('.card-title').textContent.toLowerCase();
        const meta = card.querySelector('.card-meta').textContent.toLowerCase();
        if (title.includes(query) || meta.includes(query)) {
          card.style.display = 'flex';
        } else {
          card.style.display = 'none';
        }
      });
    });

    // Modal
    const btnAdd = document.getElementById('btn-add');
    const modalAdd = document.getElementById('modal-add');
    const btnModalCancel = document.getElementById('btn-modal-cancel');
    const btnModalSave = document.getElementById('btn-modal-save');

    btnAdd.addEventListener('click', () => modalAdd.classList.add('active'));
    btnModalCancel.addEventListener('click', () => modalAdd.classList.remove('active'));

    btnModalSave.addEventListener('click', () => {
      const title = document.getElementById('input-title').value;
      const type = document.getElementById('select-type').value;
      const meta = document.getElementById('input-meta').value;

      if (!title) return alert('Por favor, informe um título.');

      const container = document.getElementById('cards-container');
      const newCard = document.createElement('div');
      newCard.className = 'card';
      newCard.setAttribute('data-type', type);

      let tagClass = 'tag-link';
      let tagText = '🔗 Link Externo';
      let actionText = '↗ Acessar';

      if (type === 'video') {
        tagClass = 'tag-youtube';
        tagText = '🎥 YouTube';
        actionText = '▶ Assistir';
      } else if (type === 'sheet') {
        tagClass = 'tag-sheets';
        tagText = '📊 Google Sheets';
        actionText = '↗ Abrir';
      }

      newCard.innerHTML = `
        <div class="card-header ${tagClass}">${tagText}</div>
        <div class="card-title">${title}</div>
        <div class="card-meta">${meta || 'Adicionado recentemente'}</div>
        <a href="#" class="card-action">${actionText}</a>
      `;

      container.prepend(newCard);
      modalAdd.classList.remove('active');

      // Limpar formulário
      document.getElementById('input-title').value = '';
      document.getElementById('input-meta').value = '';
    });
  </script>
</body>
</html>
