<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inovarte - Sistema de Ordens de Serviço</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --main-blue: #1e88e5;
            --dark-blue: #0d47a1;
            --light-blue: #bbdefb;
            --card-blue: #e3f2fd;
        }
        
        body {
            background-color: #f5f9fc;
            color: #333;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        .navbar, .btn-primary {
            background-color: var(--main-blue) !important;
            border-color: var(--main-blue) !important;
        }
        
        .navbar-brand, .nav-link {
            color: white !important;
        }
        
        .main-content {
            padding: 20px;
            margin-top: 80px;
        }
        
        .card {
            border: none;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        .card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
        
        .card-header {
            background-color: var(--card-blue);
            font-weight: bold;
            border-radius: 8px 8px 0 0 !important;
        }
        
        .kanban-container {
            display: flex;
            overflow-x: auto;
            gap: 15px;
            padding: 10px 0;
        }
        
        .kanban-column {
            min-width: 300px;
            min-height: 600px;
            background-color: #f0f8ff;
            border-radius: 8px;
            padding: 15px;
        }
        
        .kanban-card {
            margin-bottom: 15px;
            cursor: grab;
            border-left: 5px solid #ccc;
            transition: transform 0.2s;
            border-radius: 6px;
        }
        
        .kanban-card:active {
            cursor: grabbing;
            transform: rotate(3deg);
        }
        
        .priority-low {
            border-left-color: #4caf50 !important;
        }
        
        .priority-medium {
            border-left-color: #ff9800 !important;
        }
        
        .priority-high {
            border-left-color: #f44336 !important;
        }
        
        .calendar-day {
            cursor: pointer;
            transition: all 0.3s;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 2px auto;
        }
        
        .calendar-day:hover {
            background-color: var(--light-blue);
        }
        
        .has-orders {
            background-color: var(--main-blue);
            color: white;
            font-weight: bold;
        }
        
        .login-container {
            max-width: 400px;
            margin: 100px auto;
            padding: 30px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        }
        
        .dragging {
            opacity: 0.7;
            background-color: var(--light-blue);
        }
        
        .order-badge {
            font-size: 0.75rem;
        }
        
        .order-actions a {
            text-decoration: none;
            margin-right: 10px;
        }
        
        .image-thumbnail {
            width: 100%;
            height: 100px;
            object-fit: cover;
            border-radius: 5px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .image-thumbnail:hover {
            transform: scale(1.05);
        }
        
        .modal-image {
            width: 100%;
            border-radius: 5px;
        }
        
        .image-gallery {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 10px;
        }
        
        .gallery-item {
            width: 100px;
            height: 100px;
            position: relative;
        }
        
        .delete-image {
            position: absolute;
            top: 5px;
            right: 5px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        
        .dashboard-card {
            text-align: center;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .dashboard-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        
        .dashboard-icon {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .dashboard-number {
            font-size: 2rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .dashboard-label {
            font-size: 1rem;
            color: #6c757d;
        }
        
        .orders-table {
            width: 100%;
        }
        
        .orders-table th {
            background-color: var(--card-blue);
        }
        
        .status-badge {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 500;
        }
        
        .table-hover tbody tr:hover {
            background-color: rgba(30, 136, 229, 0.1);
        }
        
        .nav-link {
            color: rgba(255, 255, 255, 0.85);
            transition: color 0.3s;
        }
        
        .nav-link:hover, .nav-link.active {
            color: white;
        }
        
        .delete-order {
            color: #dc3545;
            cursor: pointer;
        }
        
        .delete-order:hover {
            color: #bd2130;
        }
        
        .toast-container {
            position: fixed;
            top: 100px;
            right: 20px;
            z-index: 9999;
        }
        
        .toast {
            box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
        }

        /* Correções específicas para o layout */
        .navbar {
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        .table thead th {
            border-top: none;
            font-weight: 600;
        }
        
        .btn {
            border-radius: 6px;
        }
        
        .form-control, .form-select {
            border-radius: 6px;
            padding: 10px 15px;
            height: auto;
        }
        
        .form-label {
            font-weight: 500;
            margin-bottom: 8px;
        }
        
        .kanban-column h5 {
            font-weight: 600;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--light-blue);
        }
    </style>
</head>
<body>
    <!-- Sistema de Login -->
    <div id="login-page" class="container-fluid">
        <div class="login-container">
            <h2 class="text-center mb-4" style="color: var(--main-blue);">
                <i class="fas fa-print me-2"></i>Inovarte
            </h2>
            <p class="text-center mb-4">Sistema de Gestão de Ordens de Serviço</p>
            <form id="login-form">
                <div class="mb-3">
                    <label for="username" class="form-label">Usuário</label>
                    <input type="text" class="form-control" id="username" required>
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">Senha</label>
                    <input type="password" class="form-control" id="password" required>
                </div>
                <button type="submit" class="btn btn-primary w-100">Entrar</button>
            </form>
            <div id="login-error" class="alert alert-danger mt-3 d-none">
                Usuário ou senha incorretos!
            </div>
            <p class="text-center mt-3 small">Use admin/admin para acessar como administrador</p>
        </div>
    </div>

    <!-- Sistema Principal (inicialmente oculto) -->
    <div id="app-container" class="d-none">
        <!-- Navbar Superior -->
        <nav class="navbar navbar-expand-lg navbar-dark fixed-top">
            <div class="container-fluid">
                <a class="navbar-brand" href="#">
                    <i class="fas fa-print me-2"></i>Inovarte
                </a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav me-auto">
                        <li class="nav-item">
                            <a class="nav-link active" href="#" id="home-link"><i class="fas fa-home me-1"></i> Dashboard</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="new-os-link"><i class="fas fa-plus-circle me-1"></i> Nova OS</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="calendar-link"><i class="fas fa-calendar-alt me-1"></i> Agenda</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="orders-link"><i class="fas fa-list me-1"></i> Todas as OS</a>
                        </li>
                        <li class="nav-item admin-only d-none">
                            <a class="nav-link" href="#" id="users-link"><i class="fas fa-users me-1"></i> Usuários</a>
                        </li>
                    </ul>
                    <ul class="navbar-nav">
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="logout-btn"><i class="fas fa-sign-out-alt me-1"></i> Sair</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>

        <!-- Container para mensagens toast -->
        <div class="toast-container">
            <div id="successToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-header bg-success text-white">
                    <strong class="me-auto">Sucesso</strong>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="toast" aria-label="Close"></button>
                </div>
                <div class="toast-body">
                    Operação realizada com sucesso!
                </div>
            </div>
        </div>

        <div class="container-fluid main-content">
            <!-- Página Inicial - Dashboard -->
            <div id="home-page">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2>Dashboard</h2>
                    <span id="welcome-user" style="color: var(--main-blue);"></span>
                </div>
                
                <div class="row mb-5">
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Aguardando">
                            <div class="card-body">
                                <div class="dashboard-icon text-primary">
                                    <i class="fas fa-clock"></i>
                                </div>
                                <div class="dashboard-number text-primary" id="count-aguardando">0</div>
                                <div class="dashboard-label">Aguardando</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Em execução">
                            <div class="card-body">
                                <div class="dashboard-icon text-warning">
                                    <i class="fas fa-cogs"></i>
                                </div>
                                <div class="dashboard-number text-warning" id="count-execucao">0</div>
                                <div class="dashboard-label">Em Execução</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Atrasado">
                            <div class="card-body">
                                <div class="dashboard-icon text-danger">
                                    <i class="fas fa-exclamation-triangle"></i>
                                </div>
                                <div class="dashboard-number text-danger" id="count-atrasado">0</div>
                                <div class="dashboard-label">Atrasados</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Concluído">
                            <div class="card-body">
                                <div class="dashboard-icon text-success">
                                    <i class="fas fa-check-circle"></i>
                                </div>
                                <div class="dashboard-number text-success" id="count-concluido">0</div>
                                <div class="dashboard-label">Concluídos</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="row">
                    <div class="col-md-8">
                        <div class="card">
                            <div class="card-header">
                                <h5 class="mb-0">Visão Geral das Ordens de Serviço</h5>
                            </div>
                            <div class="card-body">
                                <canvas id="orders-chart" height="250"></canvas>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="card">
                            <div class="card-header">
                                <h5 class="mb-0">Distribuição por Prioridade</h5>
                            </div>
                            <div class="card-body">
                                <canvas id="priority-chart" height="250"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Formulário de Nova OS -->
            <div id="new-os-page" class="d-none">
                <h2 class="mb-4" id="os-form-title">Nova Ordem de Serviço</h2>
                
                <div class="card">
                    <div class="card-header">
                        Dados do Serviço
                    </div>
                    <div class="card-body">
                        <form id="os-form">
                            <input type="hidden" id="edit-id" value="">
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label for="client" class="form-label">Cliente *</label>
                                    <input type="text" class="form-control" id="client" required>
                                </div>
                                <div class="col-md-6">
                                    <label for="whatsapp" class="form-label">WhatsApp *</label>
                                    <input type="tel" class="form-control" id="whatsapp" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="address" class="form-label">Endereço *</label>
                                    <input type="text" class="form-control" id="address" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="service" class="form-label">Descrição do Serviço *</label>
                                    <textarea class="form-control" id="service" rows="3" required></textarea>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="observation" class="form-label">Observação</label>
                                    <textarea class="form-control" id="observation" rows="2"></textarea>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label class="form-label">Situação *</label>
                                    <select class="form-select" id="status" required>
                                        <option value="Aguardando">Aguardando</option>
                                        <option value="Impresso">Impresso</option>
                                        <option value="Cortado">Cortado</option>
                                        <option value="Embalado">Embalado</option>
                                        <option value="Em execução">Em execução</option>
                                        <option value="Atrasado">Atrasado</option>
                                        <option value="Concluído">Concluído</option>
                                    </select>
                                </div>
                                <div class="col-md-6">
                                    <label class="form-label">Prioridade *</label>
                                    <div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-low" value="low" checked>
                                            <label class="form-check-label" for="priority-low">Verde (Normal)</label>
                                        </div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-medium" value="medium">
                                            <label class="form-check-label" for="priority-medium">Amarelo (Importante)</label>
                                        </div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-high" value="high">
                                            <label class="form-check-label" for="priority-high">Vermelho (Urgente)</label>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label for="layout" class="form-label">Imagens do Layout *</label>
                                    <input type="file" class="form-control" id="layout" accept="image/*" multiple required>
                                    <div class="form-text">Selecione uma ou mais imagens</div>
                                </div>
                                <div class="col-md-6">
                                    <label for="date" class="form-label">Data de Entrega *</label>
                                    <input type="date" class="form-control" id="date" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label class="form-label">Imagens Atuais</label>
                                    <div id="current-images" class="image-gallery">
                                        <!-- Imagens atuais serão exibidas aqui durante a edição -->
                                    </div>
                                </div>
                            </div>
                            
                            <div class="d-flex justify-content-end">
                                <button type="button" id="cancel-os" class="btn btn-secondary me-2">Cancelar</button>
                                <button type="submit" class="btn btn-primary" id="save-os-btn">Salvar OS</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>

            <!-- Calendário -->
            <div id="calendar-page" class="d-none">
                <h2 class="mb-4">Agenda</h2>
                
                <div class="card">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <div>
                            <button class="btn btn-sm btn-outline-primary me-2" id="prev-month">
                                <i class="fas fa-chevron-left"></i>
                            </button>
                            <span id="current-month">Junho 2023</span>
                            <button class="btn btn-sm btn-outline-primary ms-2" id="next-month">
                                <i class="fas fa-chevron-right"></i>
                            </button>
                        </div>
                        <button class="btn btn-sm btn-outline-secondary" id="today-btn">Hoje</button>
                    </div>
                    <div class="card-body">
                        <div id="calendar" class="table-responsive">
                            <!-- O calendário será gerado via JavaScript -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Quadro Kanban -->
            <div id="kanban-page" class="d-none">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2 id="kanban-date">Ordens de Serviço - 01/06/2023</h2>
                    <button class="btn btn-secondary" id="back-to-calendar">
                        <i class="fas fa-arrow-left me-1"></i> Voltar para Agenda
                    </button>
                </div>
                
                <div class="kanban-container">
                    <div class="kanban-column">
                        <h5 class="text-center">Aguardando</h5>
                        <div id="column-aguardando" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Impresso</h5>
                        <div id="column-impresso" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Cortado</h5>
                        <div id="column-cortado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Embalado</h5>
                        <div id="column-embalado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Em execução</h5>
                        <div id="column-emexecucao" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Atrasado</h5>
                        <div id="column-atrasado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Concluído</h5>
                        <div id="column-concluido" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Lista de Todas as OS -->
            <div id="orders-page" class="d-none">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2>Todas as Ordens de Serviço</h2>
                    <div>
                        <button class="btn btn-outline-primary me-2" id="filter-btn">
                            <i class="fas fa-filter me-1"></i> Filtrar
                        </button>
                        <button class="btn btn-primary" id="new-order-btn">
                            <i class="fas fa-plus me-1"></i> Nova OS
                        </button>
                    </div>
                </div>
                
                <div class="card mb-4 d-none" id="filter-panel">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-3">
                                <label for="filter-status" class="form-label">Status</label>
                                <select class="form-select" id="filter-status">
                                    <option value="">Todos</option>
                                    <option value="Aguardando">Aguardando</option>
                                    <option value="Impresso">Impresso</option>
                                    <option value="Cortado">Cortado</option>
                                    <option value="Embalado">Embalado</option>
                                    <option value="Em execução">Em execução</option>
                                    <option value="Atrasado">Atrasado</option>
                                    <option value="Concluído">Concluído</option>
                                </select>
                            </div>
                            <div class="col-md-3">
                                <label for="filter-priority" class="form-label">Prioridade</label>
                                <select class="form-select" id="filter-priority">
                                    <option value="">Todas</option>
                                    <option value="low">Normal (Verde)</option>
                                    <option value="medium">Importante (Amarelo)</option>
                                    <option value="high">Urgente (Vermelho)</option>
                                </select>
                            </div>
                            <div class="col-md-3">
                                <label for="filter-date" class="form-label">Data</label>
                                <input type="date" class="form-control" id="filter-date">
                            </div>
                            <div class="col-md-3">
                                <label class="form-label d-block">&nbsp;</label>
                                <button class="btn btn-secondary w-100" id="clear-filters">
                                    <i class="fas fa-times me-1"></i> Limpar
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-body">
                        <div class="table-responsive">
                            <table class="table table-hover orders-table">
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Cliente</th>
                                        <th>Serviço</th>
                                        <th>Data</th>
                                        <th>Status</th>
                                        <th>Prioridade</th>
                                        <th>Ações</th>
                                    </tr>
                                </thead>
                                <tbody id="orders-table-body">
                                    <!-- Ordens serão listadas aqui -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Gerenciamento de Usuários -->
            <div id="users-page" class="d-none">
                <h2 class="mb-4">Gerenciamento de Usuários</h2>
                
                <div class="card mb-4">
                    <div class="card-header">
                        Adicionar Novo Usuário
                    </div>
                    <div class="card-body">
                        <form id="user-form">
                            <div class="row mb-3">
                                <div class="col-md-4">
                                    <label for="new-username" class="form-label">Usuário *</label>
                                    <input type="text" class="form-control" id="new-username" required>
                                </div>
                                <div class="col-md-4">
                                    <label for
