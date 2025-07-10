<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kanban - Pedidos de Fornecedores - Alfa Mix</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #000000;
            color: #ffffff;
            min-height: 100vh;
            overflow-x: auto;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            position: relative;
        }

        .logo {
            width: 120px;
            height: 120px;
            margin: 0 auto 20px;
            filter: drop-shadow(0 4px 8px rgba(255, 255, 255, 0.3));
            transition: transform 0.3s ease;
        }

        .logo:hover {
            transform: scale(1.05);
        }

        .company-name {
            font-size: 2.5rem;
            font-weight: bold;
            background: linear-gradient(45deg, #ff6b6b, #feca57, #48dbfb, #ff9ff3, #54a0ff);
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: gradientShift 3s ease-in-out infinite;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(255, 255, 255, 0.1);
        }

        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .title {
            font-size: 2rem;
            color: #ffffff;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        .subtitle {
            color: #cccccc;
            font-size: 1.1rem;
            margin-bottom: 5px;
        }

        .last-modified {
            color: #888888;
            font-size: 0.9rem;
        }

        .save-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #28a745;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9rem;
            z-index: 1000;
            box-shadow: 0 2px 10px rgba(40, 167, 69, 0.3);
        }

        .controls {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .control-btn {
            background: linear-gradient(135deg, #333333, #555555);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            border: 1px solid #666666;
        }

        .control-btn:hover {
            background: linear-gradient(135deg, #555555, #777777);
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(255, 255, 255, 0.1);
        }

        .control-btn.danger {
            background: linear-gradient(135deg, #dc3545, #c82333);
            border: 1px solid #dc3545;
        }

        .control-btn.danger:hover {
            background: linear-gradient(135deg, #c82333, #a71e2a);
        }

        .control-btn.info {
            background: linear-gradient(135deg, #17a2b8, #138496);
            border: 1px solid #17a2b8;
        }

        .control-btn.info:hover {
            background: linear-gradient(135deg, #138496, #0f6674);
        }

        .control-btn.success {
            background: linear-gradient(135deg, #28a745, #1e7e34);
            border: 1px solid #28a745;
        }

        .control-btn.success:hover {
            background: linear-gradient(135deg, #1e7e34, #155724);
        }

        .week-selector {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .week-btn {
            background: linear-gradient(135deg, #444444, #666666);
            color: white;
            border: none;
            padding: 10px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            border: 1px solid #777777;
        }

        .week-btn.active {
            background: linear-gradient(135deg, #007bff, #0056b3);
            border: 1px solid #007bff;
        }

        .week-btn:hover {
            background: linear-gradient(135deg, #666666, #888888);
            transform: translateY(-1px);
        }

        .stats {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .stat-card {
            background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            min-width: 120px;
            border: 1px solid #444444;
            box-shadow: 0 4px 12px rgba(255, 255, 255, 0.05);
        }

        .stat-number {
            font-size: 2rem;
            font-weight: bold;
            color: #ffffff;
            margin-bottom: 5px;
        }

        .stat-label {
            color: #cccccc;
            font-size: 0.9rem;
        }

        .filters {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
            align-items: center;
        }

        .search-input {
            padding: 10px 15px;
            border: 1px solid #555555;
            border-radius: 8px;
            background: #222222;
            color: #ffffff;
            font-size: 1rem;
            width: 250px;
        }

        .search-input::placeholder {
            color: #888888;
        }

        .filter-btn {
            background: linear-gradient(135deg, #333333, #555555);
            color: white;
            border: none;
            padding: 10px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            border: 1px solid #666666;
        }

        .filter-btn.active {
            background: linear-gradient(135deg, #28a745, #1e7e34);
            border: 1px solid #28a745;
        }

        .filter-btn:hover {
            background: linear-gradient(135deg, #555555, #777777);
            transform: translateY(-1px);
        }

        .responsaveis-manager {
            background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 30px;
            border: 1px solid #444444;
        }

        .responsaveis-title {
            font-size: 1.2rem;
            color: #ffffff;
            margin-bottom: 15px;
            text-align: center;
        }

        .responsavel-input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .responsavel-input {
            padding: 8px 12px;
            border: 1px solid #555555;
            border-radius: 6px;
            background: #222222;
            color: #ffffff;
            font-size: 0.9rem;
            width: 200px;
        }

        .responsavel-input::placeholder {
            color: #888888;
        }

        .add-responsavel-btn {
            background: linear-gradient(135deg, #28a745, #1e7e34);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
        }

        .add-responsavel-btn:hover {
            background: linear-gradient(135deg, #1e7e34, #155724);
            transform: translateY(-1px);
        }

        .responsaveis-list {
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .responsavel-tag {
            background: linear-gradient(135deg, #444444, #666666);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 8px;
            border: 1px solid #777777;
        }

        .remove-responsavel {
            background: #dc3545;
            color: white;
            border: none;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            cursor: pointer;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .kanban-board {
            display: flex;
            gap: 20px;
            overflow-x: auto;
            padding-bottom: 20px;
        }

        .column {
            min-width: 300px;
            background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #444444;
            box-shadow: 0 4px 12px rgba(255, 255, 255, 0.05);
        }

        .column-header {
            background: linear-gradient(135deg, #333333, #555555);
            color: white;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            margin-bottom: 20px;
            font-weight: bold;
            font-size: 1.1rem;
            border: 1px solid #666666;
        }

        .column-header.segunda { background: linear-gradient(135deg, #6f42c1, #5a2d91); }
        .column-header.terca { background: linear-gradient(135deg, #e83e8c, #d91a72); }
        .column-header.quarta { background: linear-gradient(135deg, #20c997, #17a085); }
        .column-header.quinta { background: linear-gradient(135deg, #fd7e14, #e8590c); }
        .column-header.sexta { background: linear-gradient(135deg, #6610f2, #520dc2); }

        .pedido-count {
            font-size: 0.9rem;
            opacity: 0.8;
            margin-top: 5px;
        }

        .card {
            background: linear-gradient(135deg, #2d2d2d, #404040);
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            border: 1px solid #555555;
            transition: all 0.3s ease;
            box-shadow: 0 2px 8px rgba(255, 255, 255, 0.05);
        }

        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 16px rgba(255, 255, 255, 0.1);
            border-color: #777777;
        }

        .card-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: #ffffff;
            font-size: 1rem;
        }

        .card-info {
            display: flex;
            flex-direction: column;
            gap: 5px;
            margin-bottom: 15px;
        }

        .card-info-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.85rem;
            color: #cccccc;
        }

        .card-info-item.observacoes {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            padding: 8px 12px;
            border-radius: 6px;
            border-left: 3px solid #ffd700;
            margin-top: 8px;
            font-style: italic;
            color: #ffd700;
            font-size: 0.8rem;
            line-height: 1.4;
        }

        .card-controls {
            display: flex;
            gap: 10px;
            align-items: center;
            flex-wrap: wrap;
        }

        .status-select {
            background: #333333;
            color: white;
            border: 1px solid #555555;
            border-radius: 4px;
            padding: 6px 10px;
            font-size: 0.8rem;
            cursor: pointer;
        }

        .status-select option {
            background: #333333;
            color: white;
        }

        .card-btn {
            background: linear-gradient(135deg, #444444, #666666);
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.8rem;
            transition: all 0.3s ease;
        }

        .card-btn:hover {
            background: linear-gradient(135deg, #666666, #888888);
            transform: translateY(-1px);
        }

        .card-btn.edit {
            background: linear-gradient(135deg, #28a745, #1e7e34);
        }

        .card-btn.edit:hover {
            background: linear-gradient(135deg, #1e7e34, #155724);
        }

        .card-btn.delete {
            background: linear-gradient(135deg, #dc3545, #c82333);
        }

        .card-btn.delete:hover {
            background: linear-gradient(135deg, #c82333, #a71e2a);
        }

        .add-card-btn {
            background: linear-gradient(135deg, #007bff, #0056b3);
            color: white;
            border: none;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.9rem;
            width: 100%;
            transition: all 0.3s ease;
            border: 1px solid #007bff;
        }

        .add-card-btn:hover {
            background: linear-gradient(135deg, #0056b3, #004085);
            transform: translateY(-1px);
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
        }

        .modal-content {
            background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
            margin: 5% auto;
            padding: 30px;
            border-radius: 12px;
            width: 90%;
            max-width: 500px;
            border: 1px solid #444444;
            box-shadow: 0 8px 32px rgba(255, 255, 255, 0.1);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 1.3rem;
            color: #ffffff;
        }

        .close {
            color: #cccccc;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.3s ease;
        }

        .close:hover {
            color: #ffffff;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            color: #ffffff;
            font-weight: bold;
        }

        .form-input, .form-select, .form-textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #555555;
            border-radius: 6px;
            background: #222222;
            color: #ffffff;
            font-size: 1rem;
        }

        .form-input::placeholder, .form-textarea::placeholder {
            color: #888888;
        }

        .form-textarea {
            resize: vertical;
            min-height: 80px;
        }

        .form-select option {
            background: #222222;
            color: #ffffff;
        }

        .modal-buttons {
            display: flex;
            gap: 15px;
            justify-content: flex-end;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(135deg, #007bff, #0056b3);
            color: white;
        }

        .btn-primary:hover {
            background: linear-gradient(135deg, #0056b3, #004085);
            transform: translateY(-1px);
        }

        .btn-secondary {
            background: linear-gradient(135deg, #6c757d, #545b62);
            color: white;
        }

        .btn-secondary:hover {
            background: linear-gradient(135deg, #545b62, #3d4142);
            transform: translateY(-1px);
        }

        .notification {
            position: fixed;
            top: 80px;
            right: 20px;
            background: #28a745;
            color: white;
            padding: 15px 20px;
            border-radius: 8px;
            z-index: 1001;
            transform: translateX(400px);
            transition: transform 0.3s ease;
            box-shadow: 0 4px 12px rgba(40, 167, 69, 0.3);
        }

        .notification.show {
            transform: translateX(0);
        }

        .logs-content {
            max-height: 400px;
            overflow-y: auto;
            background: #222222;
            border-radius: 8px;
            padding: 15px;
            border: 1px solid #444444;
        }

        .log-entry {
            margin-bottom: 15px;
            padding: 10px;
            background: #333333;
            border-radius: 6px;
            border-left: 4px solid #007bff;
        }

        .log-entry.status { border-left-color: #28a745; }
        .log-entry.add { border-left-color: #17a2b8; }
        .log-entry.edit { border-left-color: #ffc107; }
        .log-entry.delete { border-left-color: #dc3545; }
        .log-entry.reset { border-left-color: #6f42c1; }

        .log-time {
            font-size: 0.8rem;
            color: #888888;
            margin-bottom: 5px;
        }

        .log-action {
            font-weight: bold;
            color: #ffffff;
            margin-bottom: 3px;
        }

        .log-details {
            font-size: 0.9rem;
            color: #cccccc;
        }

        @media (max-width: 768px) {
            .company-name {
                font-size: 2rem;
            }

            .title {
                font-size: 1.5rem;
            }

            .logo {
                width: 100px;
                height: 100px;
            }

            .controls, .filters, .week-selector {
                flex-direction: column;
                align-items: center;
            }

            .search-input {
                width: 100%;
                max-width: 300px;
            }

            .kanban-board {
                flex-direction: column;
            }

            .column {
                min-width: auto;
            }

            .modal-content {
                width: 95%;
                margin: 10% auto;
                padding: 20px;
            }

            .responsavel-input-group {
                flex-direction: column;
                align-items: center;
            }

            .responsavel-input {
                width: 100%;
                max-width: 250px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="save-indicator" id="saveIndicator">💾 Dados salvos automaticamente</div>
        
        <div class="header">
            <img src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTIwIiBoZWlnaHQ9IjEyMCIgdmlld0JveD0iMCAwIDEyMCAxMjAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIxMjAiIGhlaWdodD0iMTIwIiBmaWxsPSIjZmZmZmZmIi8+Cjx0ZXh0IHg9IjYwIiB5PSI2NSIgZm9udC1mYW1pbHk9IkFyaWFsLCBzYW5zLXNlcmlmIiBmb250LXNpemU9IjE2IiBmaWxsPSIjMzMzMzMzIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5MT0dPPC90ZXh0Pgo8L3N2Zz4K" alt="Logo Alfa Mix" class="logo">
            <h1 class="company-name">ALFA MIX</h1>
            <h2 class="title">Pedidos de Fornecedores</h2>
            <p class="subtitle">Sistema com logs e persistência automática</p>
            <p class="last-modified" id="lastModified">Última modificação: <span id="lastModifiedTime"></span></p>
        </div>

        <div class="controls">
            <button class="control-btn info" onclick="mostrarLogs()">📋 Ver Logs</button>
            <button class="control-btn" onclick="exportarDados()">💾 Exportar Backup</button>
            <button class="control-btn" onclick="importarDados()">📁 Importar Backup</button>
            <button class="control-btn danger" onclick="zerarSistema()">🔄 Resetar Status</button>
            <button class="control-btn success" onclick="toggleResponsaveisManager()">👥 Gerenciar Responsáveis</button>
        </div>

        <div class="week-selector">
            <button class="week-btn active" onclick="selecionarSemana('atual')">📅 Semana Atual</button>
            <button class="week-btn" onclick="selecionarSemana('anterior1')">📅 Semana Passada</button>
            <button class="week-btn" onclick="selecionarSemana('anterior2')">📅 2 Semanas Atrás</button>
            <button class="week-btn" onclick="selecionarSemana('anterior3')">📅 3 Semanas Atrás</button>
        </div>

        <div class="responsaveis-manager" id="responsaveisManager" style="display: none;">
            <h3 class="responsaveis-title">👥 Gerenciar Responsáveis</h3>
            <div class="responsavel-input-group">
                <input type="text" class="responsavel-input" id="novoResponsavel" placeholder="Nome do responsável" maxlength="30">
                <button class="add-responsavel-btn" onclick="adicionarResponsavel()">➕ Adicionar</button>
            </div>
            <div class="responsaveis-list" id="responsaveisList"></div>
        </div>

        <div class="stats" id="stats"></div>

        <div class="filters">
            <input type="text" class="search-input" id="searchInput" placeholder="🔍 Buscar pedidos..." oninput="filtrarPedidos()">
            <button class="filter-btn" onclick="filtrarPedidos('todos')">Todos</button>n>
            <div id="filtrosResponsaveis"></div>
        </div>

        <div class="kanban-board" id="kanbanBoard"></div>
    </div>

    <!-- Modal para adicionar/editar pedidos -->
    <div id="pedidoModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title" id="modalTitle">Adicionar Pedido</h3>
                <span class="close" onclick="fecharModal()">&times;</span>
            </div>
            <form id="pedidoForm">
                <div class="form-group">
                    <label class="form-label">Fornecedor:</label>
                    <input type="text" class="form-input" id="fornecedor" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Loja:</label>
                    <select class="form-select" id="loja" required>
                        <option value="">Selecione a loja</option>
                        <option value="Loja 1 - Iperó">Loja 1 - Iperó</option>
                        <option value="Loja 2 - Av. Brasil">Loja 2 - Av. Brasil</option>
                        <option value="Loja 3 - CT">Loja 3 - CT</option>
                        <option value="Loja 4 - Av. Zélia">Loja 4 - Av. Zélia</option>
                        <option value="TODAS AS LOJAS">TODAS AS LOJAS</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="form-label">Dia da Semana:</label>
                    <select class="form-select" id="diaSemana" required>
                        <option value="">Selecione o dia</option>
                        <option value="segunda">Segunda-feira</option>
                        <option value="terca">Terça-feira</option>
                        <option value="quarta">Quarta-feira</option>
                        <option value="quinta">Quinta-feira</option>
                        <option value="sexta">Sexta-feira</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="form-label">Horário:</label>
                    <input type="text" class="form-input" id="horario" placeholder="Ex: 08:00 às 16:00" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Entrega:</label>
                    <input type="text" class="form-input" id="entrega" placeholder="Ex: 5º Feira" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Responsável:</label>
                    <select class="form-select" id="responsavel" required>
                        <option value="">Selecione o responsável</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="form-label">Status:</label>
                    <select class="form-select" id="status">
                        <option value="todo">A Fazer</option>
                        <option value="progress">Em Andamento</option>
                        <option value="waiting">Aguardando</option>
                        <option value="done">Concluído</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="form-label">Observações:</label>
                    <textarea class="form-textarea" id="observacoes" placeholder="Digite observações importantes, notas especiais ou detalhes específicos do pedido..." rows="3"></textarea>
                </div>
                <div class="modal-buttons">
                    <button type="button" class="btn btn-secondary" onclick="fecharModal()">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Salvar</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal para logs -->
    <div id="logsModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">📋 Histórico de Atividades</h3>
                <span class="close" onclick="fecharLogsModal()">&times;</span>
            </div>
            <div class="logs-content" id="logsContent"></div>
            <div class="modal-buttons">
                <button type="button" class="btn btn-secondary" onclick="fecharLogsModal()">Fechar</button>
                <button type="button" class="btn btn-primary" onclick="limparLogs()">Limpar Logs</button>
            </div>
        </div>
    </div>

    <div class="notification" id="notification"></div>

    <script>
        // Dados iniciais
        let pedidos = [
            // Segunda-feira
            { id: 1, fornecedor: 'Nestlé Chocolates', loja: 'Loja 1 - Iperó', dia: 'segunda', horario: '08:00 às 16:00', entrega: 'Semana Seguinte', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 2, fornecedor: 'Nestlé Chocolates', loja: 'Loja 2 - Av. Brasil', dia: 'segunda', horario: '08:00 às 16:00', entrega: 'Semana Seguinte', responsavel: 'Responsável Loja 2', status: 'todo', semana: 'atual' },
            { id: 3, fornecedor: 'Nestlé Chocolates', loja: 'Loja 4 - Av. Zélia', dia: 'segunda', horario: '08:00 às 16:00', entrega: 'Semana Seguinte', responsavel: 'Responsável Loja 4', status: 'todo', semana: 'atual' },
            { id: 4, fornecedor: 'Cigaax', loja: 'TODAS AS LOJAS', dia: 'segunda', horario: '08:00 às 16:00', entrega: '5º Feira', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 5, fornecedor: 'Pepsico do Brasil - Elma Chips', loja: 'Loja 1 - Iperó', dia: 'segunda', horario: '08:00 às 16:30', entrega: '5º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 6, fornecedor: 'Pepsico do Brasil - Elma Chips', loja: 'Loja 2 - Av. Brasil', dia: 'segunda', horario: '08:00 às 16:30', entrega: '5º Feira', responsavel: 'Responsável Loja 2', status: 'todo', semana: 'atual' },
            
            // Terça-feira
            { id: 7, fornecedor: 'Morumbi Distribuidora President', loja: 'TODAS AS LOJAS', dia: 'terca', horario: '08:00 às 16:00', entrega: '5º Feira', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 8, fornecedor: 'RKM Salgados', loja: 'TODAS AS LOJAS', dia: 'terca', horario: '08:00 às 12:00', entrega: '5º Feira', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 9, fornecedor: 'Paulispan Pão Francês', loja: 'Loja 2 - Av. Brasil', dia: 'terca', horario: '08:00 às 12:00', entrega: '5º Feira', responsavel: 'Responsável Loja 2', status: 'todo', semana: 'atual' },
            { id: 10, fornecedor: 'Paulispan Pão Francês', loja: 'Loja 4 - Av. Zélia', dia: 'terca', horario: '08:00 às 12:00', entrega: '5º Feira', responsavel: 'Responsável Loja 4', status: 'todo', semana: 'atual' },
            { id: 11, fornecedor: 'Grupo Petrópolis', loja: 'Loja 1 - Iperó', dia: 'terca', horario: '08:00 às 16:30', entrega: '6º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            
            // Quarta-feira
            { id: 12, fornecedor: 'Grupo Petrópolis', loja: 'Loja 3 - CT', dia: 'quarta', horario: '08:00 às 16:30', entrega: '6º Feira', responsavel: 'Responsável Loja 3', status: 'todo', semana: 'atual' },
            { id: 13, fornecedor: 'Lourenço Distribuidora', loja: 'TODAS AS LOJAS', dia: 'quarta', horario: '08:00 às 16:30', entrega: 'Sábado', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 14, fornecedor: 'Phillip Morris (Cigarros)', loja: 'TODAS AS LOJAS', dia: 'quarta', horario: '08:00 às 16:30', entrega: 'Sábado', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 15, fornecedor: 'Bertin Bebidas (Chopp Vinho Grape Cool)', loja: 'TODAS AS LOJAS', dia: 'quarta', horario: '08:00 às 16:30', entrega: 'Sábado', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            
            // Quinta-feira
            { id: 16, fornecedor: 'Granado Doces', loja: 'Loja 1 - Iperó', dia: 'quinta', horario: '08:00 às 16:30', entrega: '2º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 17, fornecedor: 'DNA Distribuidora', loja: 'TODAS AS LOJAS', dia: 'quinta', horario: '08:00 às 16:30', entrega: '2º Feira', responsavel: 'Responsável Geral', status: 'todo', semana: 'atual' },
            { id: 18, fornecedor: 'Sorocaba Refrescos (Coca-Cola)', loja: 'Loja 1 - Iperó', dia: 'quinta', horario: '08:00 às 17:00', entrega: '2º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 19, fornecedor: 'Nestlé Insumos Máquinas Café', loja: 'Loja 2 - Av. Brasil', dia: 'quinta', horario: '08:00 às 17:00', entrega: 'Próxima Semana', responsavel: 'Responsável Loja 2', status: 'todo', semana: 'atual' },
            
            // Sexta-feira
            { id: 20, fornecedor: 'Paulispan Pão Francês', loja: 'Loja 1 - Iperó', dia: 'sexta', horario: '08:00 às 12:00', entrega: '2º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 21, fornecedor: 'Arcom S/A', loja: 'Loja 1 - Iperó', dia: 'sexta', horario: '08:00 às 12:00', entrega: '2º Feira', responsavel: 'Responsável Loja 1', status: 'todo', semana: 'atual' },
            { id: 22, fornecedor: 'Arcom S/A', loja: 'Loja 2 - Av. Brasil', dia: 'sexta', horario: '08:00 às 12:00', entrega: '2º Feira', responsavel: 'Responsável Loja 2', status: 'todo', semana: 'atual' },
            { id: 23, fornecedor: 'Pepsico do Brasil - Elma Chips', loja: 'Loja 4 - Av. Zélia', dia: 'sexta', horario: '08:00 às 12:00', entrega: '2º Feira', responsavel: 'Responsável Loja 4', status: 'todo', semana: 'atual' }
        ];

        let logs = [];
        let responsaveis = ['Responsável Loja 1', 'Responsável Loja 2', 'Responsável Loja 3', 'Responsável Loja 4', 'Responsável Geral'];
        let semanaAtual = 'atual';
        let filtroAtual = 'todos';
        let buscaAtual = '';
        let editandoPedido = null;

        // Inicialização
        document.addEventListener('DOMContentLoaded', function() {
            carregarDados();
            renderizarResponsaveis();
            renderizarFiltrosResponsaveis();
            renderizarCards();
            atualizarEstatisticas();
            atualizarUltimaModificacao();
            
            adicionarLog('init', 'Sistema inicializado com dados padrão', `${pedidos.length} pedidos carregados`);
        });

        // Gerenciamento de responsáveis
        function toggleResponsaveisManager() {
            const manager = document.getElementById('responsaveisManager');
            manager.style.display = manager.style.display === 'none' ? 'block' : 'none';
        }

        function adicionarResponsavel() {
            const input = document.getElementById('novoResponsavel');
            const nome = input.value.trim();
            
            if (nome && !responsaveis.includes(nome)) {
                responsaveis.push(nome);
                input.value = '';
                renderizarResponsaveis();
                renderizarFiltrosResponsaveis();
                atualizarSelectResponsaveis();
                salvarDados();
                mostrarNotificacao(`Responsável "${nome}" adicionado!`);
                adicionarLog('add', 'Responsável adicionado', nome);
            } else if (responsaveis.includes(nome)) {
                mostrarNotificacao('Este responsável já existe!');
            }
        }

        function removerResponsavel(nome) {
            if (confirm(`Remover responsável "${nome}"?\n\nAtenção: Pedidos atribuídos a este responsável ficarão sem responsável.`)) {
                responsaveis = responsaveis.filter(r => r !== nome);
                
                // Atualizar pedidos que usavam este responsável
                pedidos.forEach(pedido => {
                    if (pedido.responsavel === nome) {
                        pedido.responsavel = 'Sem responsável';
                    }
                });
                
                renderizarResponsaveis();
                renderizarFiltrosResponsaveis();
                atualizarSelectResponsaveis();
                renderizarCards();
                salvarDados();
                mostrarNotificacao(`Responsável "${nome}" removido!`);
                adicionarLog('delete', 'Responsável removido', nome);
            }
        }

        function renderizarResponsaveis() {
            const lista = document.getElementById('responsaveisList');
            lista.innerHTML = responsaveis.map(nome => `
                <div class="responsavel-tag">
                    <span>${nome}</span>
                    <button class="remove-responsavel" onclick="removerResponsavel('${nome}')" title="Remover">×</button>
                </div>
            `).join('');
        }

        function renderizarFiltrosResponsaveis() {
            const container = document.getElementById('filtrosResponsaveis');
            container.innerHTML = responsaveis.map(nome => `
                <button class="filter-btn" onclick="filtrarPorResponsavel('${nome}')">${nome}</button>
            `).join('');
        }

        function atualizarSelectResponsaveis() {
            const select = document.getElementById('responsavel');
            select.innerHTML = '<option value="">Selecione o responsável</option>' +
                responsaveis.map(nome => `<option value="${nome}">${nome}</option>`).join('');
        }

        // Seleção de semana
        function selecionarSemana(semana) {
            semanaAtual = semana;
            
            // Atualizar botões
            document.querySelectorAll('.week-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            renderizarCards();
            atualizarEstatisticas();
            
            const nomesSemanas = {
                'atual': 'Semana Atual',
                'anterior1': 'Semana Passada',
                'anterior2': '2 Semanas Atrás',
                'anterior3': '3 Semanas Atrás'
            };
            
            mostrarNotificacao(`Visualizando: ${nomesSemanas[semana]}`);
        }

        // Filtros
        function filtrarPorStatus(status) {
            filtroAtual = status;
            
            // Atualizar botões
            document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            renderizarCards();
        }

        function filtrarPorResponsavel(responsavel) {
            filtroAtual = responsavel;
            
            // Atualizar botões
            document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            renderizarCards();
        }

        function filtrarPedidos() {
            buscaAtual = document.getElementById('searchInput').value.toLowerCase();
            renderizarCards();
        }

        // Função para obter pedidos filtrados
        function obterPedidosFiltrados() {
            return pedidos.filter(pedido => {
                // Filtro por semana
                if (pedido.semana !== semanaAtual) return false;
                
                // Filtro por busca
                if (buscaAtual && !pedido.fornecedor.toLowerCase().includes(buscaAtual) && 
                    !pedido.loja.toLowerCase().includes(buscaAtual) &&
                    !pedido.responsavel.toLowerCase().includes(buscaAtual)) {
                    return false;
                }
                
                // Filtro por responsável
                if (filtroAtual === 'todos') return true;
                if (responsaveis.includes(filtroAtual)) return pedido.responsavel === filtroAtual;
                
                return true;
            });
        }

        // Renderização do Kanban
        function renderizarCards() {
            const board = document.getElementById('kanbanBoard');
            const dias = ['segunda', 'terca', 'quarta', 'quinta', 'sexta'];
            const nomesDias = {
                'segunda': 'Segunda-feira',
                'terca': 'Terça-feira', 
                'quarta': 'Quarta-feira',
                'quinta': 'Quinta-feira',
                'sexta': 'Sexta-feira'
            };

            const pedidosFiltrados = obterPedidosFiltrados();

            board.innerHTML = dias.map(dia => {
                const pedidosDia = pedidosFiltrados.filter(p => p.dia === dia);
                
                return `
                    <div class="column">
                        <div class="column-header ${dia}">
                            📅 ${nomesDias[dia]}
                            <div class="pedido-count">${pedidosDia.length} pedidos</div>
                        </div>
                        ${pedidosDia.map(pedido => criarCardHTML(pedido)).join('')}
                        <button class="add-card-btn" onclick="abrirModalAdicionar('${dia}')">➕ Adicionar Pedido</button>
                    </div>
                `;
            }).join('');

            salvarDados();
        }

        function criarCardHTML(pedido) {
            return `
                <div class="card">
                    <div class="card-title">${pedido.fornecedor}</div>
                    <div class="card-info">
                        <div class="card-info-item">🏪 ${pedido.loja}</div>
                        <div class="card-info-item">⏰ ${pedido.horario}</div>
                        <div class="card-info-item">🚚 Entrega: ${pedido.entrega}</div>
                        <div class="card-info-item">👤 ${pedido.responsavel}</div>
                        ${pedido.observacoes ? `<div class="card-info-item observacoes">📝 ${pedido.observacoes}</div>` : ''}
                    </div>
                    <div class="card-controls">
                        <select class="status-select" onchange="alterarStatus(${pedido.id}, this.value)">
                            <option value="todo" ${pedido.status === 'todo' ? 'selected' : ''}>A Fazer</option>
                            <option value="progress" ${pedido.status === 'progress' ? 'selected' : ''}>Em Andamento</option>
                            <option value="waiting" ${pedido.status === 'waiting' ? 'selected' : ''}>Aguardando</option>
                            <option value="done" ${pedido.status === 'done' ? 'selected' : ''}>Concluído</option>
                        </select>
                        <button class="card-btn edit" onclick="editarPedido(${pedido.id})">✏️ Editar</button>
                        <button class="card-btn delete" onclick="excluirPedido(${pedido.id})">🗑️ Excluir</button>
                    </div>
                </div>
            `;
        }

        // Estatísticas
        function atualizarEstatisticas() {
            const pedidosFiltrados = obterPedidosFiltrados();
            const stats = {
                total: pedidosFiltrados.length,
                todo: pedidosFiltrados.filter(p => p.status === 'todo').length,
                progress: pedidosFiltrados.filter(p => p.status === 'progress').length,
                waiting: pedidosFiltrados.filter(p => p.status === 'waiting').length,
                done: pedidosFiltrados.filter(p => p.status === 'done').length
            };

            document.getElementById('stats').innerHTML = `
                <div class="stat-card">
                    <div class="stat-number">${stats.total}</div>
                    <div class="stat-label">Total de Pedidos</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.todo}</div>
                    <div class="stat-label">A Fazer</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.progress}</div>
                    <div class="stat-label">Em Andamento</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.waiting}</div>
                    <div class="stat-label">Aguardando</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.done}</div>
                    <div class="stat-label">Concluído</div>
                </div>
            `;
        }

        // Funções de pedidos
        function alterarStatus(id, novoStatus) {
            const pedido = pedidos.find(p => p.id === id);
            if (pedido) {
                const statusAnterior = pedido.status;
                pedido.status = novoStatus;
                
                const statusNomes = {
                    'todo': 'A Fazer',
                    'progress': 'Em Andamento', 
                    'waiting': 'Aguardando',
                    'done': 'Concluído'
                };
                
                adicionarLog(
                    'status',
                    `Status alterado: ${pedido.fornecedor}`,
                    `${statusNomes[statusAnterior]} → ${statusNomes[novoStatus]} (${pedido.loja})`
                );

                atualizarEstatisticas();
                atualizarUltimaModificacao();
                mostrarNotificacao('Status alterado com sucesso!');
            }
        }

        function abrirModalAdicionar(dia = '') {
            editandoPedido = null;
            document.getElementById('modalTitle').textContent = 'Adicionar Pedido';
            document.getElementById('pedidoForm').reset();
            if (dia) document.getElementById('diaSemana').value = dia;
            atualizarSelectResponsaveis();
            document.getElementById('pedidoModal').style.display = 'block';
        }

        function editarPedido(id) {
            const pedido = pedidos.find(p => p.id === id);
            if (pedido) {
                editandoPedido = id;
                document.getElementById('modalTitle').textContent = 'Editar Pedido';
                document.getElementById('fornecedor').value = pedido.fornecedor;
                document.getElementById('loja').value = pedido.loja;
                document.getElementById('diaSemana').value = pedido.dia;
                document.getElementById('horario').value = pedido.horario;
                document.getElementById('entrega').value = pedido.entrega;
                document.getElementById('responsavel').value = pedido.responsavel;
                document.getElementById('status').value = pedido.status;
                document.getElementById('observacoes').value = pedido.observacoes || '';
                atualizarSelectResponsaveis();
                document.getElementById('pedidoModal').style.display = 'block';
            }
        }

        function excluirPedido(id) {
            const pedido = pedidos.find(p => p.id === id);
            if (pedido && confirm(`Excluir pedido "${pedido.fornecedor}"?`)) {
                pedidos = pedidos.filter(p => p.id !== id);
                
                adicionarLog('delete', 'Pedido excluído', `${pedido.fornecedor} (${pedido.loja})`);
                
                renderizarCards();
                atualizarEstatisticas();
                atualizarUltimaModificacao();
                mostrarNotificacao('Pedido excluído com sucesso!');
            }
        }

        function fecharModal() {
            document.getElementById('pedidoModal').style.display = 'none';
            editandoPedido = null;
        }

        // Submissão do formulário
        document.getElementById('pedidoForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const dados = {
                fornecedor: document.getElementById('fornecedor').value,
                loja: document.getElementById('loja').value,
                dia: document.getElementById('diaSemana').value,
                horario: document.getElementById('horario').value,
                entrega: document.getElementById('entrega').value,
                responsavel: document.getElementById('responsavel').value,
                status: document.getElementById('status').value,
                observacoes: document.getElementById('observacoes').value,
                semana: semanaAtual
            };

            if (editandoPedido) {
                // Editar pedido existente
                const pedido = pedidos.find(p => p.id === editandoPedido);
                Object.assign(pedido, dados);
                
                adicionarLog('edit', 'Pedido editado', `${dados.fornecedor} (${dados.loja})`);
                mostrarNotificacao('Pedido editado com sucesso!');
            } else {
                // Adicionar novo pedido
                const novoId = Math.max(...pedidos.map(p => p.id), 0) + 1;
                pedidos.push({ id: novoId, ...dados });
                
                adicionarLog('add', 'Pedido adicionado', `${dados.fornecedor} (${dados.loja})`);
                mostrarNotificacao('Pedido adicionado com sucesso!');
            }

            renderizarCards();
            atualizarEstatisticas();
            atualizarUltimaModificacao();
            fecharModal();
        });

        // Sistema de logs
        function adicionarLog(tipo, acao, detalhes) {
            const agora = new Date();
            logs.unshift({
                timestamp: agora.toLocaleString('pt-BR'),
                tipo: tipo,
                acao: acao,
                detalhes: detalhes
            });
            
            // Manter apenas os últimos 100 logs
            if (logs.length > 100) {
                logs = logs.slice(0, 100);
            }
            
            salvarDados();
        }

        function mostrarLogs() {
            const content = document.getElementById('logsContent');
            content.innerHTML = logs.map(log => `
                <div class="log-entry ${log.tipo}">
                    <div class="log-time">${log.timestamp}</div>
                    <div class="log-action">${log.acao}</div>
                    <div class="log-details">${log.detalhes}</div>
                </div>
            `).join('') || '<p style="text-align: center; color: #888;">Nenhum log encontrado.</p>';
            
            document.getElementById('logsModal').style.display = 'block';
        }

        function fecharLogsModal() {
            document.getElementById('logsModal').style.display = 'none';
        }

        function limparLogs() {
            if (confirm('Limpar todos os logs? Esta ação não pode ser desfeita.')) {
                logs = [];
                salvarDados();
                mostrarLogs();
                mostrarNotificacao('Logs limpos com sucesso!');
            }
        }

        // Função para zerar sistema (resetar status)
        function zerarSistema() {
            const pedidosConcluidos = pedidos.filter(p => p.status === 'done' && p.semana === semanaAtual);
            
            if (pedidosConcluidos.length === 0) {
                alert('Não há pedidos concluídos para resetar na semana selecionada.');
                return;
            }
            
            const confirmacao = confirm(`🔄 RESETAR STATUS DOS PEDIDOS\n\nIsso irá voltar ${pedidosConcluidos.length} pedido(s) concluído(s) para "A Fazer".\n\nOs pedidos não serão deletados, apenas o status será resetado.\n\nTem certeza que deseja continuar?`);
            
            if (confirmacao) {
                let pedidosResetados = 0;
                
                pedidos.forEach(pedido => {
                    if (pedido.status === 'done' && pedido.semana === semanaAtual) {
                        pedido.status = 'todo';
                        pedidosResetados++;
                    }
                });
                
                adicionarLog(
                    'reset',
                    'Status dos pedidos resetado pelo usuário',
                    `${pedidosResetados} pedido(s) voltaram de "Concluído" para "A Fazer"`
                );

                renderizarCards();
                atualizarEstatisticas();
                atualizarUltimaModificacao();
                mostrarNotificacao(`${pedidosResetados} pedido(s) resetado(s) para "A Fazer"!`);
            }
        }

        // Funções de backup
        function exportarDados() {
            const dados = {
                pedidos: pedidos,
                logs: logs,
                responsaveis: responsaveis,
                exportadoEm: new Date().toLocaleString('pt-BR')
            };
            
            const blob = new Blob([JSON.stringify(dados, null, 2)], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `kanban-backup-${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            URL.revokeObjectURL(url);
            
            mostrarNotificacao('Backup exportado com sucesso!');
        }

        function importarDados() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = '.json';
            input.onchange = function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        try {
                            const dados = JSON.parse(e.target.result);
                            
                            if (dados.pedidos && Array.isArray(dados.pedidos)) {
                                pedidos = dados.pedidos;
                            }
                            if (dados.logs && Array.isArray(dados.logs)) {
                                logs = dados.logs;
                            }
                            if (dados.responsaveis && Array.isArray(dados.responsaveis)) {
                                responsaveis = dados.responsaveis;
                            }
                            
                            renderizarResponsaveis();
                            renderizarFiltrosResponsaveis();
                            atualizarSelectResponsaveis();
                            renderizarCards();
                            atualizarEstatisticas();
                            atualizarUltimaModificacao();
                            salvarDados();
                            
                            mostrarNotificacao('Backup importado com sucesso!');
                            adicionarLog('import', 'Dados importados de backup', `${pedidos.length} pedidos carregados`);
                        } catch (error) {
                            alert('Erro ao importar arquivo. Verifique se é um backup válido.');
                        }
                    };
                    reader.readAsText(file);
                }
            };
            input.click();
        }

        // Persistência de dados
        function salvarDados() {
            const dados = {
                pedidos: pedidos,
                logs: logs,
                responsaveis: responsaveis
            };
            localStorage.setItem('kanbanData', JSON.stringify(dados));
            
            // Atualizar indicador
            const indicator = document.getElementById('saveIndicator');
            indicator.textContent = '✅ Dados salvos';
            setTimeout(() => {
                indicator.textContent = '💾 Dados salvos automaticamente';
            }, 2000);
        }

        function carregarDados() {
            const dados = localStorage.getItem('kanbanData');
            if (dados) {
                try {
                    const parsed = JSON.parse(dados);
                    if (parsed.pedidos) pedidos = parsed.pedidos;
                    if (parsed.logs) logs = parsed.logs;
                    if (parsed.responsaveis) responsaveis = parsed.responsaveis;
                } catch (error) {
                    console.error('Erro ao carregar dados:', error);
                }
            }
        }

        function atualizarUltimaModificacao() {
            document.getElementById('lastModifiedTime').textContent = new Date().toLocaleString('pt-BR');
        }

        // Notificações
        function mostrarNotificacao(mensagem) {
            const notification = document.getElementById('notification');
            notification.textContent = mensagem;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Fechar modais ao clicar fora
        window.onclick = function(event) {
            const pedidoModal = document.getElementById('pedidoModal');
            const logsModal = document.getElementById('logsModal');
            
            if (event.target === pedidoModal) {
                fecharModal();
            }
            if (event.target === logsModal) {
                fecharLogsModal();
            }
        }

        // Teclas de atalho
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                fecharModal();
                fecharLogsModal();
            }
        });
    </script>
</body>
</html>

