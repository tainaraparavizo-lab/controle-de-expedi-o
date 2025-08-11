<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel de ON TIME expedição CD VAR</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* --- SPLASH SCREEN ATUALIZADA --- */
        #splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            background-image: url('https://marcasmais.com.br/wp-content/uploads/2021/05/O-BOTICARIO-DIA-DOS-NAMORADOS-ALMAPBBDO-04.jpg');
            background-size: cover;
            background-position: center;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            transition: opacity 0.8s ease-out, visibility 0.8s ease-out;
        }
        #splash-screen::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5); /* Overlay escuro para legibilidade */
        }
        #splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }
        .splash-content {
            text-align: center;
            animation: fadeInContent 1s 0.2s ease-out forwards;
            opacity: 0;
            position: relative; /* Para ficar acima do overlay */
            z-index: 1;
            padding: 20px;
        }
        @keyframes fadeInContent {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .splash-content img {
           display: none; /* A imagem agora é o background */
        }
        .splash-content h1 {
            font-size: 2.5rem;
            font-weight: 700;
            color: #ffffff;
            text-shadow: 2px 2px 8px rgba(0,0,0,0.7);
            margin: 0 0 10px 0;
        }
        .splash-content p {
            font-size: 1.2rem;
            color: #f0f0f0;
            text-shadow: 1px 1px 4px rgba(0,0,0,0.7);
            margin-bottom: 35px;
            max-width: 600px;
        }
        .btn-splash {
            background: var(--laranja);
            color: white;
            border: none;
            padding: 14px 35px;
            font-size: 1.1rem;
            font-weight: 500;
            font-family: 'Poppins', sans-serif;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(243, 156, 18, 0.4);
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }
        .btn-splash:hover {
            transform: translateY(-4px);
            box-shadow: 0 8px 25px rgba(243, 156, 18, 0.5);
        }

        /* --- GERAL E MODO NOTURNO --- */
        :root {
            --laranja: #f39c12; --laranja-secundario: #e67e22; --verde: #2ecc71; --vermelho: #e74c3c; --azul: #3498db;
            --fundo-gradiente: linear-gradient(120deg, #5dade2, #f39c12);
            --fundo-painel: #ffffff;
            --fundo-card: #f4f6f8;
            --cor-texto: #34495e;
            --cor-texto-secundario: #555;
            --borda-cor: #e0e0e0;
            --sombra-cor: rgba(0, 0, 0, 0.1);
            --borda-input: #ccc;
        }
        body.dark-mode {
            --fundo-gradiente: linear-gradient(120deg, #2c3e50, #8e44ad);
            --fundo-painel: #2c3e50;
            --fundo-card: #34495e;
            --cor-texto: #ecf0f1;
            --cor-texto-secundario: #bdc3c7;
            --borda-cor: #4a6278;
            --sombra-cor: rgba(0, 0, 0, 0.4);
            --borda-input: #566573;
        }
        body {
            font-family: 'Poppins', 'Segoe UI', sans-serif;
            background: var(--fundo-gradiente);
            padding: 20px; margin: 0; color: var(--cor-texto); display: flex;
            justify-content: flex-start; align-items: flex-start; min-height: 100vh;
            transition: background 0.3s, color 0.3s;
        }
        .painel {
            max-width: 1100px; width: 100%; background: var(--fundo-painel);
            padding: 30px; border-radius: 20px;
            box-shadow: 0 15px 35px var(--sombra-cor);
            animation: fadeIn 0.8s ease-out; border: 3px solid var(--cor-texto);
            position: relative; overflow: hidden; margin: auto; transition: background 0.3s, border-color 0.3s;
        }
        /* --- ANIMAÇÕES E CABEÇALHO --- */
        @keyframes fadeIn { from { transform: translateY(30px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        @keyframes slideUpFadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .painel-header { display: flex; align-items: center; justify-content: space-between; gap: 15px; margin-bottom: 25px; flex-wrap: wrap; }
        .titulo-logo { display: flex; align-items: center; gap: 15px; }
        #logoEmpresa { max-height: 50px; object-fit: contain; background-color: #fff; padding: 5px; border-radius: 8px; }
        h2 { font-size: 2rem; font-weight: 700; margin: 0; background: linear-gradient(45deg, var(--laranja), var(--laranja-secundario)); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .header-controls { display: flex; align-items: center; gap: 15px; }
        #relogio { font-size: 1.5rem; font-weight: 700; color: var(--cor-texto); background-color: var(--fundo-card); padding: 8px 16px; border-radius: 12px; box-shadow: inset 0 2px 4px rgba(0,0,0,0.06); min-width: 110px; text-align: center; }
        /* --- INPUTS E BOTÕES --- */
        .upload-container { display: flex; gap: 10px; margin-bottom: 10px; align-items: center; }
        input, select, .btn { padding: 10px 15px; border-radius: 12px; border: 1px solid var(--borda-input); background-color: var(--fundo-painel); color: var(--cor-texto); transition: all 0.3s ease; font-family: 'Poppins', sans-serif; font-size: 14px; }
        input:hover, select:hover, .btn:hover { border-color: var(--laranja); box-shadow: 0 0 8px rgba(243, 156, 18, 0.6); transform: translateY(-2px); }
        .btn { cursor: pointer; background-color: var(--cor-texto); color: var(--fundo-painel); display: flex; align-items: center; gap: 8px; font-weight: 500;}
        .btn-icon { padding: 10px; }
        /* --- FILTROS --- */
        .filtros-container { display: flex; flex-wrap: wrap; gap: 15px; align-items: center; margin-bottom: 20px; padding: 15px; background-color: var(--fundo-card); border-radius: 15px; }
        .filtros-container label { display: flex; align-items: center; gap: 5px; font-weight: 500;}
        /* --- DASHBOARD VIEW --- */
        .dashboard-content { display: none; animation: slideUpFadeIn 0.7s ease-out; }
        #estado-inicial { text-align: center; padding: 40px 20px; border: 2px dashed var(--borda-input); border-radius: 15px; margin: 20px 0; }
        #estado-inicial img { max-width: 220px; margin-bottom: 20px; opacity: 0.9; }
        #estado-inicial h3 { margin-top: 0; font-size: 1.2rem; }
        /* --- LOADER E NOTIFICAÇÃO --- */
        .loader { position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.5); backdrop-filter: blur(4px); z-index: 10; display: flex; justify-content: center; align-items: center; visibility: hidden; opacity: 0; transition: opacity 0.5s, visibility 0.5s; }
        .loader.visible { visibility: visible; opacity: 1; }
        .spinner { border: 6px solid var(--fundo-card); border-top: 6px solid var(--laranja); border-radius: 50%; width: 50px; height: 50px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        #notificacao { position: fixed; top: -100px; left: 50%; transform: translateX(-50%); padding: 15px 25px; border-radius: 12px; color: white; box-shadow: 0 4px 15px rgba(0,0,0,0.2); font-weight: bold; transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 20; }
        #notificacao.success { background-color: var(--verde); }
        #notificacao.error { background-color: var(--vermelho); }
        #notificacao.info { background-color: var(--laranja-secundario); }
        #notificacao.visible { top: 20px; }
        /* --- CARDS E GRÁFICOS --- */
        .resumo { margin-top: 20px; display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 20px; }
        .card-numero { background-color: var(--fundo-painel); border-left: 6px solid var(--laranja); border-radius: 12px; box-shadow: 0 6px 12px var(--sombra-cor); padding: 20px; transition: all 0.3s ease; animation: slideUpFadeIn 0.5s ease-out forwards; opacity: 0; cursor: pointer; }
        .card-numero:hover { transform: translateY(-5px) scale(1.03); box-shadow: 0 10px 20px var(--sombra-cor); }
        .card-numero h3 { margin: 0; font-size: 1rem; color: var(--cor-texto); display: flex; align-items: center; gap: 8px;}
        .card-numero p { margin: 5px 0 0; font-size: 2rem; font-weight: bold; }
        .container-grafico-resumo { display: flex; gap: 30px; margin-top: 20px; flex-wrap: wrap; align-items: stretch; }
        .grafico-wrapper { flex: 2; min-width: 300px; margin: auto; animation: slideUpFadeIn 0.6s ease-out forwards; opacity: 0; }
        #graficoCarretas { cursor: pointer; }
        .resumo-wrapper { flex: 3; min-width: 320px; }

        .cards-duplos-container { margin-top: 30px; display: grid; grid-template-columns: 1fr; gap: 30px; animation: slideUpFadeIn 0.7s ease-out forwards; opacity: 0; }
        .card-extra { background-color: var(--fundo-card); padding: 25px; border-radius: 15px; border-top: 4px solid var(--laranja); }
        .card-extra h3 { margin-top: 0; margin-bottom: 15px; color: var(--cor-texto); border-bottom: 1px solid var(--borda-cor); padding-bottom: 10px; display: flex; align-items: center; gap: 8px; }

        /* --- Destaques e Expedição --- */
        .lista-expedicao { list-style: none; padding: 0; margin: 0; font-size: 15px; line-height: 1.6; }
        .item-expedicao:last-child { border-bottom: none; }

        #destaque-expedicao { font-weight: 700; color: var(--laranja-secundario); padding-bottom: 10px; margin-bottom: 10px; border-bottom: 1px solid var(--borda-cor); }
        .item-expedicao { display: flex; flex-direction: column; gap: 8px; padding: 12px 5px; border-bottom: 1px solid var(--borda-cor); }
        .item-info-superior { display: flex; justify-content: space-between; align-items: flex-start; }
        .exp-placa { font-size: 1.2rem; font-weight: 700; color: var(--cor-texto); } /* Destaque na Placa */
        .exp-atendimento { font-size: 0.9rem; color: var(--cor-texto-secundario); }
        .exp-status { font-size: 0.8rem; font-weight: 700; color: white; padding: 3px 10px; border-radius: 12px; text-transform: uppercase; }
        /* Cores dos badges de status */
        .status-liberada { background-color: var(--verde); }
        .status-em-faturamento { background-color: var(--azul); }
        .status-chamado-aberto { background-color: var(--vermelho); }
        .status-default { background-color: var(--cor-texto-secundario); }

        .progress-bar-container { width: 100%; background-color: var(--borda-cor); border-radius: 10px; height: 16px; overflow: hidden; margin-top: 4px; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--laranja-secundario), var(--laranja)); border-radius: 10px; transition: width 0.5s ease-in-out; text-align: right; color: white; font-size: 11px; line-height: 16px; font-weight: 700; padding-right: 5px; box-sizing: border-box; }

        /* --- TABELA DE DETALHES --- */
        #container-tabela { margin-top: 30px; }
        .tabela-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; flex-wrap: wrap; gap: 10px; }
        #tabelaBusca { max-width: 250px; }
        .tabela-wrapper { overflow-x: auto; }
        #tabelaDetalhes { width: 100%; border-collapse: collapse; }
        #tabelaDetalhes th, #tabelaDetalhes td { padding: 12px 15px; text-align: left; border-bottom: 1px solid var(--borda-cor); }
        #tabelaDetalhes th { background-color: var(--fundo-card); font-weight: 700; }
        #tabelaDetalhes tbody tr:hover { background-color: var(--fundo-card); }
        #tabelaDetalhes th.sortable { cursor: pointer; }
        #tabelaDetalhes th.sortable::after { content: ' \f0dc'; font-family: "Font Awesome 6 Free"; font-weight: 900; opacity: 0.3; }
        #tabelaDetalhes th.asc::after { content: ' \f0de'; opacity: 1; }
        #tabelaDetalhes th.desc::after { content: ' \f0dd'; opacity: 1; }
        #tabelaPaginacao { display: flex; justify-content: center; align-items: center; gap: 10px; margin-top: 20px; font-weight: 500; }
        /* --- MEDIA QUERIES --- */
        @media (max-width: 900px) { .cards-duplos-container { grid-template-columns: 1fr; } }
        @media (max-width: 768px) { body { padding: 10px; } .painel { padding: 20px; margin-top: 10px; } h2 { font-size: 1.5rem; } .painel-header, .filtros-container, .upload-container, .container-grafico-resumo { flex-direction: column; align-items: stretch; } .splash-content h1 {font-size: 1.8rem;} .splash-content p {font-size: 1rem;} }
    </style>
</head>
<body>

<div id="splash-screen">
    <div class="splash-content">
        <img src="" alt=""> <h1>Bem-vindo ao Painel On Time CD VAR 2025</h1>
        <p></p>
        <button id="btn-enter-splash" class="btn-splash">
            Entrar
            <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>

<div class="painel">
    <div id="loader" class="loader"><div class="spinner"></div></div>
    <div id="notificacao"></div>

    <div class="painel-header">
        <div class="titulo-logo">
            <img id="logoEmpresa" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAyMAAACyCAYAAABRARC+AAABN2lDQ1BBZG9iZSBSR0IgKDE5OTgpAAAokZWPv0rDUBSHvxtFxaFWCOLgcCdRUGzVwYxJW4ogWKtDkq1JQ5ViEm6uf/oQjm4dXNx9AidHwUHxCXwDxamDQ4QMBYvf9J3fORzOAaNi152GUYbzWKt205Gu58vZF2aYAoBOmKV2q3UAECdxxBjf7wiA10277jTG+38yH6ZKAyNguxtlIYgK0L/SqQYxBMygn2oQD4CpTto1EE9AqZf7G1AKcv8ASsr1fBBfgNlzPR+MOcAMcl8BTB1da4Bakg7UWe9Uy6plWdLuJkEkjweZjs4zuR+HiUoT1dFRF8jvA2AxH2w3HblWtay99X/+PRHX82Vun0cIQCw9F1lBeKEuf1UYO5PrYsdwGQ7vYXpUZLs3cLcBC7dFtlqF8hY8Dn8AwMZP/fNTP8gAAAAJcEhZcwAACxMAAAsTAQCanBgAAATwaVRYdFhNTDpjb20uYWRvYmUueG1wAAAAAAA8P3hwYWNrZXQgYmVnaW49Iu+7vyIgaWQ9Ilc1TTBNcENlaGlIenJlU3pOVGN6a2M5ZCI/PiA8eDp4bXBtZXRhIHhtbG5zOng9ImFkb2JlOm5zOm1ldGEvIiB4OnhtcHRrPSJBZG9iZSBYTVAgQ29yZSA5LjEtYzAwMiA3OS5hNmE2Mzk2LCAyMDI0LzAzLzEyLTA3OjQ4OjIzICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIgeG1sbnM6cGhvdG9zaG9wPSJodHRwOi8vbnMuYWRvYmUuY29tL3Bob3Rvc2hvcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RFdnQ9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZUV2ZW50IyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgMjUuMTIgKFdpbmRvd3MpIiB4bXA6Q3JlYXRlRGF0ZT0iMjAyNS0wMy0wN1QxMzo1Mjo1NC0wMzowMCIgeG1wOk1vZGlmeURhdGU9IjIwMjUtMDMtMDdUMTQ6MDg6MDAtMDM6MDAiIHhtcDpNZXRhZGF0YURhdGU9IjIwMjUtMDMtMDdUMTQ6MDg6MDAtMDM6MDAiIGRjOmZvcm1hdD0iaW1hZ2UvcG5nIiBwaG90b3Nob3A6Q29sb3JNb2RlPSIzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOmI5ZmRhZmJjLTAyNDUtOWI0My1hMDUzLTk5YmM3NWE0NjNmOSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpiOWZkYWZiYy0wMjQ1LTliNDMtYTA1My05OWJjNzVhNDYzZjkiIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpiOWZkYWZiYy0wMjQ1LTliNDMtYTA1My05OWJjNzVhNDYzZjkiPiA8eG1wTU06SGlzdG9yeT4gPHJkZjpTZXE+IDxyZGY6bGkgc3RFdnQ6YWN0aW9uPSJjcmVhdGVkIiBzdEV2dDppbnN0YW5jZUlEPSJ4bXAuaWlkOmI5ZmRhZmJjLTAyNDUtOWI0My1hMDUzLTk5YmM3NWE0NjNmOSIgc3RFdnQ6d2hlbj0iMjAyNS0wMy0wN1QxMzo1Mjo1NC0wMzowMCIgc3RFdnQ6c29mdHdhcmVBZ2VudD0iQWRvYmUgUGhvdG9zaG9wIDI1LjEyIChXaW5kb3dzKSIvPiA8L3JkZjpTZXE+IDwveG1wTU06SGlzdG9yeT4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7r/HvoAAA+CklEQVR4nO3dfZAbZ34f+G83ZjgvGhLgcEazK4kCtKaWXibrgVzr3UNihVCOfsmLI2zVZW1fWBYYh65K2ReBjFN2UgkF8Tbl7OWOGjqO7Y3iEPTRLp/jWEPfnu/WoU+gualxbr1LzGrFLLlcClhK1FIzGgLkzGBegO77ox+QIIiXfhr9BuD7qZqSOHj6eZ5B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/FG0v8/l8OA85rwIjuuuhSE0r2cRDz5nWT8F8NRZXZsrjs7XkiKMz2HW8YrRff/+1C/98N9e+7/+3qi28RNQ8KQXddA3lJtaSZnS7iljbeaEFGA8iHA8CKlxLBghkuXHHe+JiIiI7PThL3z8mDKu/5g6pn8SATzhWEE6yvqmsmwiAAFEL5ldK2TJYDBCvsFghIiIiAbJ0uH9cXWX/gvKmL5PGcYEhvUhZdjCEF4dZb2CD/SyMqSXlUl9A2NtluatuT9Mz61ekGYYjJBvMBghIiKiQbV0eH8ID+ZbxZVx/VMAHmt70LaCNvM+6hUghooCyDo1Gd0KBiPkGwxGiIiIiB62dHh/XPxv7b8htF4gIgdjXtD9//dT4NHMkNcVICIiIiKi5uqCiWybZD2rq31GiIiIiIiIrGIwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnmAwQkREREREnhjyugJEdV6VSJt3qhJERERE5I7/H3S9JP5VjNohAAAAAElFTkSuQmCC " alt="Logo da Empresa">
            <h2>Painel On Time 2025 - Status de Expedição</h2>
        </div>
        <div class="header-controls">
            <div id="relogio">00:00:00</div>
            <button id="btnThemeToggle" class="btn btn-icon" title="Alternar Tema"><i class="fa-solid fa-moon"></i></button>
        </div>
    </div>

    <div class="upload-container">
        <input type="text" id="googleSheetUrl" placeholder="Cole a URL da planilha Google aqui" style="flex: 1;">
        <button id="btnLoadSheet" class="btn"><i class="fa-solid fa-cloud-arrow-down"></i> Carregar Planilha</button>
        <button id="btnLimpar" class="btn"><i class="fa-solid fa-eraser"></i> Limpar Dados</button>
    </div>
    <div id="last-updated-container" style="margin-bottom: 20px; text-align: right; font-size: 12px; color: var(--cor-texto-secundario);"></div>

    <div id="dashboard-content" class="dashboard-content">
        <div class="filtros-container">
            <label><i class="fa-solid fa-calendar-days"></i> De:</label><input type="date" id="dataInicial" />
            <label><i class="fa-solid fa-calendar-check"></i> Até:</label><input type="date" id="dataFinal" />
            <label><i class="fa-solid fa-truck"></i> Transportadora:</label><select id="filtroTransportadora"><option value="">Todas</option></select>
            <label><i class="fa-solid fa-clipboard-list"></i> Atendimento:</label><select id="filtroAtendimento"><option value="">Todos</option></select>
            <label><i class="fa-solid fa-flag-checkered"></i> Status:</label><select id="filtroStatus"><option value="">Todos</option><option value="On Time">On Time</option><option value="Fora do Prazo">Fora do Prazo</option></select>
        </div>

        <div class="container-grafico-resumo">
            <div class="grafico-wrapper"><canvas id="graficoCarretas"></canvas></div>
            <div class="resumo-wrapper"><div id="resumo" class="resumo"></div></div>
        </div>
        
        <div class="cards-duplos-container">
            <div class="card-extra" id="card-expedicao">
                <h3><i class="fa-solid fa-truck-ramp-box"></i> Carretas em Expedição</h3>
                <p id="destaque-expedicao"></p>
                <ul id="lista-expedicao" class="lista-expedicao"></ul>
            </div>
        </div>

        <div id="container-tabela" class="card-extra">
            <div class="tabela-header">
                <h3><i class="fa-solid fa-table-list"></i> Detalhes das Entregas</h3>
                <input type="text" id="tabelaBusca" placeholder="Buscar nos detalhes...">
            </div>
            <div class="tabela-wrapper"><table id="tabelaDetalhes">
                <thead></thead>
                <tbody></tbody>
            </table></div>
            <div id="tabelaPaginacao"></div>
        </div>
    </div>

    <div id="estado-inicial">
        <img src="C:\Users\tainarap\Documents\APRESENTAÇÃO PARA O DECOL\image (3).png" alt="Imagem Inicial">
        <h3>Nossa meta raiz é ter o produto certo, na hora certa, no custo certo, na qualidade certa, respeitando o meio ambiente, sem acidentes e com alto engajamento dos times.</h3>
        <p>Cole a URL da sua planilha Google para monitorar entregas e o status da expedição.</p>
    </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', () => {
    // --- LÓGICA DA SPLASH SCREEN ---
    const splash = document.getElementById('splash-screen');
    const enterButton = document.getElementById('btn-enter-splash');
    const hideSplash = () => {
        splash.classList.add('hidden');
        setTimeout(() => { if (splash.parentNode) { splash.parentNode.removeChild(splash); } }, 800);
    };
    if (splash && enterButton) {
        enterButton.addEventListener('click', hideSplash);
    }

    // --- STATE MANAGEMENT ---
    const appState = {
        deliveryData: [], expeditionData: [], chartInstance: null,
        deliveryColumns: ['Data', 'Transportadora', 'Atendimento', 'TU', 'Status'],
        table: { page: 1, rowsPerPage: 5, sortColumn: 'Data', sortDirection: 'desc', searchTerm: '' }
    };

    // --- DOM ELEMENTS ---
    const dom = {
        body: document.body, loader: document.getElementById('loader'), notificacao: document.getElementById('notificacao'),
        googleSheetUrl: document.getElementById('googleSheetUrl'), btnLoadSheet: document.getElementById('btnLoadSheet'), btnLimpar: document.getElementById('btnLimpar'),
        dashboardContent: document.getElementById('dashboard-content'), estadoInicial: document.getElementById('estado-inicial'),
        dataInicial: document.getElementById('dataInicial'), dataFinal: document.getElementById('dataFinal'),
        filtroTransportadora: document.getElementById('filtroTransportadora'), 
        filtroAtendimento: document.getElementById('filtroAtendimento'),
        filtroStatus: document.getElementById('filtroStatus'), canvas: document.getElementById('graficoCarretas'),
        resumoContainer: document.getElementById('resumo'), relogio: document.getElementById('relogio'),
        btnThemeToggle: document.getElementById('btnThemeToggle'),
        listaExpedicao: document.getElementById('lista-expedicao'),
        destaqueExpedicao: document.getElementById('destaque-expedicao'),
        tabelaBusca: document.getElementById('tabelaBusca'), tabelaHead: document.querySelector('#tabelaDetalhes thead'),
        tabelaBody: document.querySelector('#tabelaDetalhes tbody'), tabelaPaginacao: document.getElementById('tabelaPaginacao'),
        lastUpdatedContainer: document.getElementById('last-updated-container')
    };
    const ctx = dom.canvas.getContext('2d');

    // --- UI LOGIC ---
    const ui = {
        showLoader: (show) => dom.loader.classList.toggle('visible', show),
        renderExpeditionCard: (data) => {
            dom.listaExpedicao.innerHTML = '';
            dom.destaqueExpedicao.innerHTML = '';
            if (data.length === 0) {
                dom.listaExpedicao.innerHTML = '<li class="item-expedicao">Nenhuma carreta na expedição.</li>';
                return;
            }
            const topTruck = data.reduce((prev, current) => (prev.Percentual > current.Percentual) ? prev : current);
            dom.destaqueExpedicao.innerHTML = `<i class="fa-solid fa-star"></i> <strong>Destaque:</strong> ${topTruck.Placa} com ${topTruck.Percentual}%`;
            
            data.sort((a,b) => b.Percentual - a.Percentual).forEach(item => {
                const li = document.createElement('li');
                li.className = 'item-expedicao';
                const percent = Math.min(Math.max(item.Percentual || 0, 0), 100);
                
                const finalStatus = (percent === 100) ? 'Liberada' : item.StatusExpedicao;
                const normalizeStatusClass = (status) => status ? status.toLowerCase().replace(/\s+/g, '-') : 'default';
                const statusClass = normalizeStatusClass(finalStatus);

                li.innerHTML = `
                    <div class="item-info-superior">
                        <div>
                            <div class="exp-placa">${item.Placa} (${item.TU || 'N/A'})</div>
                            <div class="exp-atendimento">${item.Atendimento}</div>
                        </div>
                        <span class="exp-status status-${statusClass}">${finalStatus || 'N/A'}</span>
                    </div>
                    <div class="progress-bar-container">
                        <div class="progress-bar-fill" style="width: ${percent}%;">${percent}%</div>
                    </div>`;
                dom.listaExpedicao.appendChild(li);
            });
        },
        showNotification: (message, type = 'success', duration = 3000) => {
            dom.notificacao.innerHTML = `<i class="fa-solid fa-check-circle"></i> ${message}`;
            dom.notificacao.className = type;
            dom.notificacao.classList.add('visible');
            setTimeout(() => dom.notificacao.classList.remove('visible'), duration);
        },
        toggleDashboardView: (show) => {
            dom.dashboardContent.style.display = show ? 'block' : 'none';
            dom.estadoInicial.style.display = show ? 'none' : 'flex';
            dom.estadoInicial.style.flexDirection = 'column';
            dom.estadoInicial.style.alignItems = 'center';
            dom.estadoInicial.style.justifyContent = 'center';
        },
        updateSummaryCards: (onTime, foraPrazo) => {
            dom.resumoContainer.innerHTML = `
                <div class="card-numero" data-status="On Time" style="border-left-color: var(--verde);"><h3 style="color:var(--verde);"><i class="fa-solid fa-circle-check"></i> On Time</h3><p>${onTime}</p></div>
                <div class="card-numero" data-status="Fora do Prazo" style="border-left-color: var(--vermelho);"><h3 style="color:var(--vermelho);"><i class="fa-solid fa-circle-xmark"></i> Fora do Prazo</h3><p>${foraPrazo}</p></div>
                <div class="card-numero" data-status="" style="border-left-color: var(--laranja);"><h3><i class="fa-solid fa-chart-pie"></i> Total</h3><p>${onTime + foraPrazo}</p></div>`;
        },
        renderTable: (data) => {
            dom.tabelaHead.innerHTML = '';
            const headerRow = document.createElement('tr');
            appState.deliveryColumns.forEach(col => {
                const th = document.createElement('th');
                th.className = 'sortable';
                th.dataset.column = col;
                if (col === appState.table.sortColumn) th.classList.add(appState.table.sortDirection);
                th.textContent = col;
                headerRow.appendChild(th);
            });
            dom.tabelaHead.appendChild(headerRow);
            dom.tabelaBody.innerHTML = '';
            data.forEach(row => {
                const tr = document.createElement('tr');
                appState.deliveryColumns.forEach(col => {
                    const td = document.createElement('td');
                    let value = row[col];
                    if (col === 'Data' && value instanceof Date) value = value.toLocaleDateString('pt-BR');
                    td.textContent = value || '';
                    tr.appendChild(td);
                });
                dom.tabelaBody.appendChild(tr);
            });
        },
        renderPagination: (totalRows) => {
            dom.tabelaPaginacao.innerHTML = '';
            const totalPages = Math.ceil(totalRows / appState.table.rowsPerPage);
            if (totalPages <= 1) return;
            const createBtn = (text, page) => {
                const btn = document.createElement('button');
                btn.className = 'btn btn-icon';
                btn.innerHTML = text;
                btn.disabled = appState.table.page === page || !page;
                if (appState.table.page === page) btn.style.borderColor = 'var(--laranja)';
                btn.onclick = () => { appState.table.page = page; logic.renderDashboard(); };
                return btn;
            };
            dom.tabelaPaginacao.appendChild(createBtn('«', 1));
            dom.tabelaPaginacao.appendChild(createBtn('‹', appState.table.page > 1 ? appState.table.page - 1 : null));
            const pageInfo = document.createElement('span');
            pageInfo.textContent = `Página ${appState.table.page} de ${totalPages}`;
            dom.tabelaPaginacao.appendChild(pageInfo);
            dom.tabelaPaginacao.appendChild(createBtn('›', appState.table.page < totalPages ? appState.table.page + 1 : null));
            dom.tabelaPaginacao.appendChild(createBtn('»', totalPages));
        }
    };
    
    // --- MAIN LOGIC ---
    const logic = {
        fetchGoogleSheet: async (url) => {
            if (!url || !url.includes('docs.google.com/spreadsheets')) {
                ui.showNotification('URL da planilha Google inválida.', 'error');
                return;
            }
            ui.showLoader(true);
            try {
                const response = await fetch(url);
                if (!response.ok) throw new Error('Falha ao carregar a planilha.');
                const csvText = await response.text();
                const rows = csvText.split('\n').map(row => row.split(','));
                const headers = rows[0].map(h => h.trim());
                const jsonData = rows.slice(1).map(row => {
                    const obj = {};
                    headers.forEach((header, i) => {
                        obj[header] = row[i] ? row[i].trim() : '';
                    });
                    return obj;
                });

                if (jsonData.length === 0) throw new Error(`Planilha vazia.`);

                appState.deliveryData = [];
                appState.expeditionData = [];

                jsonData.forEach(row => {
                    if (row.Data) {
                       const dateParts = row.Data.split('/');
                       if(dateParts.length === 3) {
                          row.Data = new Date(dateParts[2], dateParts[1] - 1, dateParts[0]);
                       }
                    }

                    const status = row.Status || '';
                    // ### CORREÇÃO APLICADA AQUI ###
                    // A verificação agora é mais flexível para funcionar com "Em carregamento", "Em carregamen", etc.
                    if (status.toLowerCase().includes('em carre')) {
                        row.Percentual = parseFloat(row.Percentual) || 0;
                        appState.expeditionData.push(row);
                    } else if (status) {
                        row.Status = status.toLowerCase().includes('fora') ? 'Fora do Prazo' : 'On Time';
                        appState.deliveryData.push(row);
                    }
                });

                if(appState.deliveryData.length === 0 && appState.expeditionData.length === 0) throw new Error(`Nenhum dado válido encontrado na planilha.`);
                
                logic.resetFilters();
                logic.updateFiltersUI();
                logic.renderDashboard();
                ui.renderExpeditionCard(appState.expeditionData);

                const now = new Date();
                const formattedDateTime = now.toLocaleDateString('pt-BR') + ' às ' + now.toLocaleTimeString('pt-BR');
                dom.lastUpdatedContainer.innerHTML = `<i class="fa-solid fa-clock-rotate-left"></i> Última atualização: <strong>${formattedDateTime}</strong>`;

                ui.showNotification("Planilha carregada com sucesso!", 'success');
                ui.toggleDashboardView(true);
            } catch (error) {
                console.error("Erro:", error);
                ui.showNotification(error.message, 'error');
                logic.resetApp();
            } finally {
                ui.showLoader(false);
            }
        },
        renderDashboard: () => {
            if (appState.deliveryData.length === 0 && appState.expeditionData.length > 0) {
                ui.updateSummaryCards(0, 0);
                logic.renderChart(0, 0);
                logic.renderTableWithLogic([]);
                return;
            }
            if(appState.deliveryData.length === 0) return;
            const filteredData = logic.applyMainFilters();
            const onTime = filteredData.filter(l => l.Status === 'On Time').length;
            const foraPrazo = filteredData.length - onTime;
            ui.updateSummaryCards(onTime, foraPrazo);
            logic.renderChart(onTime, foraPrazo);
            logic.renderTableWithLogic(filteredData);
        },
        resetApp: () => {
            appState.deliveryData = [];
            appState.expeditionData = [];
            if (appState.chartInstance) appState.chartInstance.destroy();
            logic.resetFilters();
            ui.toggleDashboardView(false);
            ui.renderExpeditionCard([]);
            dom.lastUpdatedContainer.innerHTML = '';
        },
        updateFiltersUI: () => {
            const createOptions = (element, items) => {
                const selectedValue = element.value;
                element.innerHTML = '<option value="">Todos</option>';
                [...new Set(items)].sort().forEach(item => {
                    if (item) element.innerHTML += `<option value="${item}" ${item === selectedValue ? 'selected' : ''}>${item}</option>`;
                });
            };
            createOptions(dom.filtroTransportadora, appState.deliveryData.map(l => l.Transportadora));
            createOptions(dom.filtroAtendimento, appState.deliveryData.map(l => l.Atendimento));
        },
        applyMainFilters: () => {
            let filteredData = [...appState.deliveryData];
            const formatDate = (d) => d ? new Date(d).toISOString().split('T')[0] : null;
            const inicio = formatDate(dom.dataInicial.value);
            const fim = formatDate(dom.dataFinal.value);
            if (inicio && fim) filteredData = filteredData.filter(l => { const d = formatDate(l.Data); return d && d >= inicio && d <= fim; });
            if (dom.filtroTransportadora.value) filteredData = filteredData.filter(l => l.Transportadora === dom.filtroTransportadora.value);
            if (dom.filtroAtendimento.value) filteredData = filteredData.filter(l => l.Atendimento === dom.filtroAtendimento.value);
            if (dom.filtroStatus.value) filteredData = filteredData.filter(l => l.Status === dom.filtroStatus.value);
            return filteredData;
        },
        renderTableWithLogic: (data) => {
            let tableData = [...data];
            if (appState.table.searchTerm) {
                const term = appState.table.searchTerm.toLowerCase();
                tableData = tableData.filter(row => Object.values(row).some(val => String(val).toLowerCase().includes(term)));
            }
            tableData.sort((a, b) => {
                let valA = a[appState.table.sortColumn];
                let valB = b[appState.table.sortColumn];
                if (valA instanceof Date && valB instanceof Date) {
                    valA = valA.getTime();
                    valB = valB.getTime();
                }
                if (valA < valB) return appState.table.sortDirection === 'asc' ? -1 : 1;
                if (valA > valB) return appState.table.sortDirection === 'asc' ? 1 : -1;
                return 0;
            });
            const start = (appState.table.page - 1) * appState.table.rowsPerPage;
            const end = start + appState.table.rowsPerPage;
            ui.renderTable(tableData.slice(start, end));
            ui.renderPagination(tableData.length);
        },
        renderChart: (onTime, foraPrazo) => {
            if (appState.chartInstance) appState.chartInstance.destroy();
            appState.chartInstance = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['On Time', 'Fora do Prazo'],
                    datasets: [{
                        data: [onTime, foraPrazo],
                        backgroundColor: [getComputedStyle(document.documentElement).getPropertyValue('--verde'), getComputedStyle(document.documentElement).getPropertyValue('--vermelho')],
                        borderWidth: 4,
                        borderColor: getComputedStyle(document.documentElement).getPropertyValue('--fundo-painel')
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '70%',
                    onClick: (e, elements) => {
                        if (elements.length > 0) {
                            const label = appState.chartInstance.data.labels[elements[0].index];
                            dom.filtroStatus.value = label;
                            logic.renderDashboard();
                            ui.showNotification(`Filtro aplicado: Status "${label}"`, 'info', 2000);
                        }
                    },
                    plugins: { legend: { labels: { color: getComputedStyle(document.documentElement).getPropertyValue('--cor-texto') } } }
                }
            });
        },
        resetFilters: () => {
            dom.dataInicial.value = '';
            dom.dataFinal.value = '';
            dom.filtroTransportadora.value = '';
            dom.filtroAtendimento.value = '';
            dom.filtroStatus.value = '';
            appState.table.page = 1;
            appState.table.searchTerm = '';
            dom.tabelaBusca.value = '';
        }
    };

    // --- UTILITIES ---
    const utils = {
        updateClock: () => dom.relogio.textContent = new Date().toLocaleTimeString('pt-BR'),
        toggleTheme: () => {
            const isDark = dom.body.classList.toggle('dark-mode');
            dom.btnThemeToggle.innerHTML = `<i class="fa-solid ${isDark ? 'fa-sun' : 'fa-moon'}"></i>`;
            localStorage.setItem('theme', isDark ? 'dark' : 'light');
            if (appState.chartInstance) logic.renderDashboard();
        },
        loadTheme: () => { if (localStorage.getItem('theme') === 'dark') utils.toggleTheme(); }
    };

    // --- EVENT LISTENERS ---
    const addEventListeners = () => {
        dom.btnLoadSheet.addEventListener('click', () => logic.fetchGoogleSheet(dom.googleSheetUrl.value));
        dom.btnLimpar.addEventListener('click', () => { logic.resetApp(); ui.showNotification("Dados limpos.", 'success'); });
        dom.btnThemeToggle.addEventListener('click', utils.toggleTheme);
        [dom.dataInicial, dom.dataFinal, dom.filtroTransportadora, dom.filtroAtendimento, dom.filtroStatus].forEach(el => el.addEventListener('change', () => { appState.table.page = 1; logic.renderDashboard(); }));
        dom.resumoContainer.addEventListener('click', (e) => {
            const card = e.target.closest('.card-numero');
            if (card && card.dataset.status) {
                dom.filtroStatus.value = card.dataset.status;
                logic.renderDashboard();
                if (card.dataset.status) ui.showNotification(`Filtro aplicado: Status "${card.dataset.status}"`, 'info', 2000);
            }
        });
        dom.tabelaBusca.addEventListener('input', (e) => {
            appState.table.searchTerm = e.target.value;
            appState.table.page = 1;
            logic.renderDashboard();
        });
        dom.tabelaHead.addEventListener('click', (e) => {
            const th = e.target.closest('.sortable');
            if (th) {
                const column = th.dataset.column;
                if (appState.table.sortColumn === column) {
                    appState.table.sortDirection = appState.table.sortDirection === 'asc' ? 'desc' : 'asc';
                } else {
                    appState.table.sortColumn = column;
                    appState.table.sortDirection = 'asc';
                }
                appState.table.page = 1;
                logic.renderDashboard();
            }
        });
    };
    
    // --- INITIALIZATION ---
    const init = () => {
        addEventListeners();
        ui.toggleDashboardView(false);
        utils.updateClock();
        setInterval(utils.updateClock, 1000);
        utils.loadTheme();
    };
    init();
});
</script>
</body>
</html>
