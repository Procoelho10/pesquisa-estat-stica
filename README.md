<!DOCTYPE html>
<html lang="pt-BR" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pesquisa Teen (15-17 anos) - Análise Estatística Completa</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="h-full flex flex-col font-sans text-slate-800 antialiased selection:bg-indigo-500 selection:text-white">

    <!-- Header Navigation Bar -->
    <header class="bg-indigo-900 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo / Title -->
                <div class="flex items-center space-x-3">
                    <div class="p-2 bg-indigo-600 rounded-xl shadow">
                        <i class="fa-solid fa-chart-pie text-xl text-white"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg leading-tight">StatTeen 15-17</h1>
                        <p class="text-xs text-indigo-300">Análise Estatística para Projetos Escolares</p>
                    </div>
                </div>

                <!-- Navigation Tabs -->
                <nav class="flex space-x-1 sm:space-x-2">
                    <button id="tab-btn-dashboard" onclick="switchTab('dashboard')" class="px-3 py-2 text-sm font-medium rounded-lg transition-all flex items-center space-x-2 bg-indigo-700 text-white shadow">
                        <i class="fa-solid fa-chart-column"></i>
                        <span class="hidden sm:inline">Resultados & Gráficos</span>
                    </button>
                    <button id="tab-btn-import" onclick="switchTab('import')" class="px-3 py-2 text-sm font-medium rounded-lg transition-all flex items-center space-x-2 text-indigo-200 hover:text-white hover:bg-indigo-800">
                        <i class="fa-solid fa-file-import text-emerald-400"></i>
                        <span class="hidden sm:inline">Importar Planilha</span>
                    </button>
                    <button id="tab-btn-survey" onclick="switchTab('survey')" class="px-3 py-2 text-sm font-medium rounded-lg transition-all flex items-center space-x-2 text-indigo-200 hover:text-white hover:bg-indigo-800">
                        <i class="fa-solid fa-clipboard-question"></i>
                        <span class="hidden sm:inline">Responder Local</span>
                    </button>
                    <button id="tab-btn-gforms" onclick="switchTab('gforms')" class="px-3 py-2 text-sm font-medium rounded-lg transition-all flex items-center space-x-2 text-indigo-200 hover:text-white hover:bg-indigo-800">
                        <i class="fa-solid fa-link text-amber-300"></i>
                        <span class="hidden sm:inline">Google Forms / Link</span>
                    </button>
                    <button id="tab-btn-responses" onclick="switchTab('responses')" class="px-3 py-2 text-sm font-medium rounded-lg transition-all flex items-center space-x-2 text-indigo-200 hover:text-white hover:bg-indigo-800">
                        <i class="fa-solid fa-table"></i>
                        <span class="hidden sm:inline">Dados Brutos</span>
                    </button>
                </nav>
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

        <!-- Notification Toast Container -->
        <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2"></div>

        <!-- SECTION 1: DASHBOARD DE RESULTADOS (DEFAULT TAB) -->
        <section id="tab-dashboard" class="space-y-6">
            
            <!-- Global Overview & Controls Header -->
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 flex flex-col lg:flex-row items-start lg:items-center justify-between gap-6">
                <div>
                    <div class="flex items-center gap-2 flex-wrap">
                        <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-indigo-100 text-indigo-800">Universo: Adolescentes (15 a 17 anos)</span>
                        <span id="response-count-badge" class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-800">Total: 0 Respostas</span>
                    </div>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2">Painel de Estatísticas & Gráficos</h2>
                    <p class="text-sm text-slate-500">Resultados consolidados com tabelas de frequência absoluta/relativa e medidas descritivas.</p>
                </div>

                <!-- Action Controls -->
                <div class="flex flex-wrap gap-2 w-full lg:w-auto">
                    <button onclick="switchTab('import')" class="flex-1 lg:flex-none px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-semibold flex items-center justify-center gap-2 shadow-sm transition">
                        <i class="fa-solid fa-file-excel"></i> Importar Planilha
                    </button>
                    <button onclick="switchTab('gforms')" class="flex-1 lg:flex-none px-4 py-2 bg-amber-500 hover:bg-amber-600 text-white rounded-xl text-xs font-semibold flex items-center justify-center gap-2 shadow-sm transition">
                        <i class="fa-solid fa-paper-plane"></i> Google Forms
                    </button>
                    <button onclick="generateMockResponses(5)" class="flex-1 lg:flex-none px-3 py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 rounded-xl text-xs font-semibold flex items-center justify-center gap-1 transition" title="Adiciona 5 respostas de teste">
                        <i class="fa-solid fa-user-plus"></i> +5 Teste
                    </button>
                    <button onclick="exportDataJSON()" class="px-3 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-xl text-xs font-semibold flex items-center gap-1 transition" title="Exportar JSON">
                        <i class="fa-solid fa-download"></i> JSON
                    </button>
                    <button onclick="resetDataConfirmation()" class="px-3 py-2 bg-red-50 hover:bg-red-100 text-red-600 rounded-xl text-xs font-semibold flex items-center gap-1 transition" title="Zerar Banco">
                        <i class="fa-solid fa-trash-can"></i> Limpar
                    </button>
                </div>
            </div>

            <!-- Variable Types Breakdown Filter -->
            <div class="bg-slate-100 rounded-xl p-3 flex flex-wrap items-center justify-between gap-3 text-xs">
                <span class="font-semibold text-slate-600">Filtrar por Tipo de Variável:</span>
                <div class="flex flex-wrap gap-2">
                    <button onclick="filterDashboard('ALL')" id="filter-btn-ALL" class="px-3 py-1.5 rounded-lg font-semibold bg-indigo-600 text-white shadow-sm border border-indigo-600">Todas (20)</button>
                    <button onclick="filterDashboard('NOMINAL')" id="filter-btn-NOMINAL" class="px-3 py-1.5 rounded-lg font-semibold text-slate-600 hover:bg-white hover:shadow-sm">Qualitativa Nominal (5)</button>
                    <button onclick="filterDashboard('ORDINAL')" id="filter-btn-ORDINAL" class="px-3 py-1.5 rounded-lg font-semibold text-slate-600 hover:bg-white hover:shadow-sm">Qualitativa Ordinal (5)</button>
                    <button onclick="filterDashboard('DISCRETA')" id="filter-btn-DISCRETA" class="px-3 py-1.5 rounded-lg font-semibold text-slate-600 hover:bg-white hover:shadow-sm">Quantitativa Discreta (5)</button>
                    <button onclick="filterDashboard('CONTINUA')" id="filter-btn-CONTINUA" class="px-3 py-1.5 rounded-lg font-semibold text-slate-600 hover:bg-white hover:shadow-sm">Quantitativa Contínua (5)</button>
                </div>
            </div>

            <!-- Dynamic Dashboard Cards Container -->
            <div id="questions-dashboard-container" class="space-y-8">
                <!-- Cards render dynamically -->
            </div>

        </section>

        <!-- SECTION 2: IMPORTADOR DE PLANILHA (GOOGLE FORMS / EXCEL / CSV) -->
        <section id="tab-import" class="hidden space-y-6 max-w-4xl mx-auto">
            <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200 space-y-6">
                <div>
                    <span class="text-xs font-bold uppercase tracking-wider text-emerald-700 bg-emerald-50 px-3 py-1 rounded-full">Carregar Respostas dos Alunos</span>
                    <h2 class="text-2xl font-bold text-slate-900 mt-2">Importar Respostas do Google Forms / Excel</h2>
                    <p class="text-sm text-slate-500 mt-1">
                        Se os seus alunos responderam pelo Google Forms, baixe o arquivo <code>.csv</code> das respostas no Google Sheets ou cole as linhas diretamente na caixa abaixo.
                    </p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Option A: File Upload -->
                    <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 space-y-3 flex flex-col justify-between">
                        <div>
                            <div class="flex items-center gap-2 text-emerald-700 font-bold text-sm">
                                <i class="fa-solid fa-file-csv text-lg"></i>
                                <span>Opção A: Carregar Arquivo CSV</span>
                            </div>
                            <p class="text-xs text-slate-600 mt-2">
                                No Google Sheets/Forms, vá em <strong>Arquivo &gt; Fazer Download &gt; Valores separados por vírgulas (.csv)</strong>.
                            </p>
                        </div>
                        <label class="w-full cursor-pointer bg-white border-2 border-dashed border-emerald-400 hover:border-emerald-600 rounded-xl p-4 text-center block transition shadow-sm">
                            <i class="fa-solid fa-cloud-arrow-up text-2xl text-emerald-600 mb-1"></i>
                            <span class="block text-xs font-bold text-slate-700">Clique para Selecionar o Arquivo CSV</span>
                            <span class="text-[10px] text-slate-400">Suporta CSV exportado do Google Forms / Excel</span>
                            <input type="file" id="csv-file-input" accept=".csv, .tsv, .txt" class="hidden" onchange="handleFileUpload(event)">
                        </label>
                    </div>

                    <!-- Option B: Copy/Paste Raw Text -->
                    <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 space-y-3">
                        <div class="flex items-center gap-2 text-indigo-700 font-bold text-sm">
                            <i class="fa-solid fa-paste text-lg"></i>
                            <span>Opção B: Copiar e Colar Linhas</span>
                        </div>
                        <p class="text-xs text-slate-600">
                            Abra a planilha de respostas no Google Sheets, selecione todas as linhas e cole no campo abaixo:
                        </p>
                        <textarea id="paste-data-textarea" placeholder="Cole aqui o conteúdo da planilha do Google Forms..." class="w-full h-28 bg-white border border-slate-300 rounded-xl p-3 text-xs font-mono text-slate-700 focus:ring-2 focus:ring-indigo-500 focus:outline-none custom-scrollbar"></textarea>
                        <button onclick="processPastedText()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs rounded-xl shadow transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-check"></i> Processar Texto Colado
                        </button>
                    </div>
                </div>

                <div class="p-4 bg-indigo-50/70 border border-indigo-100 rounded-xl text-xs text-indigo-900 flex items-center gap-2">
                    <i class="fa-solid fa-circle-info text-base text-indigo-600"></i>
                    <p class="text-[11px] text-indigo-800">
                        O leitor mapeia automaticamente as 20 perguntas em ordem e calcula na hora as tabelas de frequência, médias, modas, medianas, variâncias, desvios padrão e gráficos.
                    </p>
                </div>
            </div>
        </section>

        <!-- SECTION 3: FORMULÁRIO DA PESQUISA LOCAL -->
        <section id="tab-survey" class="hidden space-y-6 max-w-4xl mx-auto">
            <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200">
                <div class="border-b border-slate-200 pb-4 mb-6">
                    <span class="text-xs font-bold uppercase tracking-wider text-indigo-600 bg-indigo-50 px-3 py-1 rounded-full">Questionário Jovem</span>
                    <h2 class="text-2xl font-bold text-slate-900 mt-2">Pesquisa de Opinião e Hábitos (15 a 17 Anos)</h2>
                    <p class="text-sm text-slate-500 mt-1">
                        Preenchimento direto de respostas no próprio navegador.
                    </p>
                </div>

                <form id="survey-form" onsubmit="handleFormSubmit(event)" class="space-y-8">
                    <div id="survey-questions-list" class="space-y-8"></div>

                    <div class="pt-6 border-t border-slate-200 flex items-center justify-between">
                        <button type="button" onclick="switchTab('dashboard')" class="px-5 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 text-sm font-semibold rounded-xl transition">
                            Cancelar
                        </button>
                        <button type="submit" class="px-8 py-3 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-bold rounded-xl shadow-lg shadow-indigo-200 hover:shadow-indigo-300 transition flex items-center gap-2">
                            <i class="fa-solid fa-paper-plane"></i> Enviar Resposta
                        </button>
                    </div>
                </form>
            </div>
        </section>

        <!-- SECTION 4: GERADOR DE LINK EXTERNO E GOOGLE FORMS -->
        <section id="tab-gforms" class="hidden space-y-6 max-w-4xl mx-auto">
            <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200 space-y-6">
                <div>
                    <span class="text-xs font-bold uppercase tracking-wider text-amber-700 bg-amber-50 px-3 py-1 rounded-full">Como Coletar Respostas dos Alunos</span>
                    <h2 class="text-2xl font-bold text-slate-900 mt-2">Como criar um formulário oficial em 2 minutos</h2>
                    <p class="text-sm text-slate-500 mt-1">
                        Siga o passo a passo para enviar o link aos alunos e importar os resultados:
                    </p>
                </div>

                <!-- Step-by-Step Instructions -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <div class="w-7 h-7 rounded-full bg-indigo-600 text-white font-bold flex items-center justify-center text-xs">1</div>
                        <h4 class="font-bold text-slate-800 text-xs">Copiar as Perguntas</h4>
                        <p class="text-[11px] text-slate-600">Clique no botão para copiar todas as 20 perguntas formatadas com suas opções.</p>
                        <button onclick="copyQuestionsToClipboard()" class="w-full py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-bold rounded-lg shadow transition flex items-center justify-center gap-1">
                            <i class="fa-solid fa-copy"></i> Copiar Texto das Perguntas
                        </button>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <div class="w-7 h-7 rounded-full bg-indigo-600 text-white font-bold flex items-center justify-center text-xs">2</div>
                        <h4 class="font-bold text-slate-800 text-xs">Criar Google Forms</h4>
                        <p class="text-[11px] text-slate-600">Abra o Google Formulários, cole as perguntas e envie o link para a turma.</p>
                        <a href="https://forms.new" target="_blank" class="w-full py-2 bg-amber-500 hover:bg-amber-600 text-white text-xs font-bold rounded-lg shadow transition flex items-center justify-center gap-1">
                            <i class="fa-solid fa-arrow-up-right-from-square"></i> Abrir Google Forms
                        </a>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <div class="w-7 h-7 rounded-full bg-indigo-600 text-white font-bold flex items-center justify-center text-xs">3</div>
                        <h4 class="font-bold text-slate-800 text-xs">Importar Respostas</h4>
                        <p class="text-[11px] text-slate-600">Quando responderem, baixe o CSV no Forms e carregue no StatTeen na aba "Importar Planilha".</p>
                        <button onclick="switchTab('import')" class="w-full py-2 bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold rounded-lg shadow transition flex items-center justify-center gap-1">
                            <i class="fa-solid fa-file-import"></i> Ir para Importação
                        </button>
                    </div>
                </div>

                <!-- Formatted Questions Preview Box -->
                <div class="space-y-2">
                    <label class="block text-xs font-bold text-slate-700 uppercase">Perguntas Prontas para Copiar e Colar:</label>
                    <textarea id="questions-formatted-text" readonly class="w-full h-56 bg-slate-50 border border-slate-300 rounded-xl p-4 text-xs font-mono text-slate-700 focus:outline-none custom-scrollbar"></textarea>
                </div>
            </div>
        </section>

        <!-- SECTION 5: BANCO DE DADOS BRUTOS (TABELA DE RESPOSTAS) -->
        <section id="tab-responses" class="hidden space-y-6">
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-6">
                    <div>
                        <h2 class="text-xl font-bold text-slate-900">Registro Individual de Respostas</h2>
                        <p class="text-xs text-slate-500">Tabela com todos os dados brutos armazenados nesta sessão.</p>
                    </div>
                    <button onclick="exportDataCSV()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-semibold rounded-xl flex items-center gap-2 shadow-sm transition">
                        <i class="fa-solid fa-file-csv"></i> Exportar CSV
                    </button>
                </div>

                <div class="overflow-x-auto custom-scrollbar border border-slate-200 rounded-xl">
                    <table class="w-full text-left text-xs text-slate-600">
                        <thead class="bg-slate-50 text-slate-700 uppercase font-bold border-b border-slate-200">
                            <tr id="responses-table-head">
                                <th class="p-3">#</th>
                                <th class="p-3">Data/Hora</th>
                            </tr>
                        </thead>
                        <tbody id="responses-table-body" class="divide-y divide-slate-100">
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-400 py-6 text-xs text-center border-t border-slate-800">
        <div class="max-w-7xl mx-auto px-4 space-y-2">
            <p class="font-semibold text-slate-300">StatTeen &copy; 2026 - Análise Estatística para Projetos Escolares</p>
            <p>Tabelas de Frequência Absoluta/Relativa, Média, Mediana, Moda, Desvio Padrão, Variância e Gráficos.</p>
        </div>
    </footer>

    <!-- JAVASCRIPT APP ENGINE -->
    <script>
        // 20 QUESTIONS CLASSIFIED INTO 4 STATISTICAL TYPES
        const QUESTIONS = [
            // QUALITATIVAS NOMINAIS (1-5)
            { id: 'q1', num: 1, type: 'NOMINAL', chartType: 'pie', title: 'Rede social mais utilizada no dia a dia', label: 'Rede Social Favorita', options: ['Instagram', 'TikTok', 'YouTube', 'WhatsApp', 'Discord', 'X (Twitter)'] },
            { id: 'q2', num: 2, type: 'NOMINAL', chartType: 'pie', title: 'Estilo musical preferido', label: 'Gênero Musical', options: ['Funk', 'Pop', 'Trap / Rap', 'Sertanejo', 'Rock / Indie', 'K-Pop', 'Eletrônica'] },
            { id: 'q3', num: 3, type: 'NOMINAL', chartType: 'pie', title: 'Principal dispositivo utilizado para estudos e tarefas', label: 'Dispositivo Principal', options: ['Smartphone', 'Notebook / PC', 'Tablet'] },
            { id: 'q4', num: 4, type: 'NOMINAL', chartType: 'pie', title: 'Principal meio de acesso a notícias e atualidades', label: 'Fonte de Notícias', options: ['Redes Sociais', 'Portais de Notícias', 'YouTube / Podcasts', 'Televisão', 'Conversas Família/Amigos'] },
            { id: 'q5', num: 5, type: 'NOMINAL', chartType: 'pie', title: 'Área do conhecimento que pretende seguir no futuro', label: 'Área Futura', options: ['Exatas e Tecnologia', 'Biológicas e Saúde', 'Humanas e Sociais', 'Artes e Comunicação', 'Ainda Não Sei'] },

            // QUALITATIVAS ORDINAIS (6-10)
            { id: 'q6', num: 6, type: 'ORDINAL', chartType: 'line', title: 'Nível habitual de estresse em relação aos estudos escolares', label: 'Nível de Estresse', options: ['1 - Muito Baixo', '2 - Baixo', '3 - Moderado', '4 - Alto', '5 - Muito Alto'] },
            { id: 'q7', num: 7, type: 'ORDINAL', chartType: 'line', title: 'Frequência de preocupação com o futuro profissional e vestibulares', label: 'Preocupação com Futuro', options: ['Nunca', 'Raramente', 'Às Vezes', 'Frequentemente', 'Sempre'] },
            { id: 'q8', num: 8, type: 'ORDINAL', chartType: 'line', title: 'Autoavaliação do desempenho escolar atual', label: 'Desempenho Escolar', options: ['Insuficiente', 'Regular', 'Bom', 'Muito Bom', 'Excelente'] },
            { id: 'q9', num: 9, type: 'ORDINAL', chartType: 'line', title: 'Grau de concordância: "A Inteligência Artificial trará impactos positivos para os jovens"', label: 'Impacto da IA', options: ['Discordo Totalmente', 'Discordo Parcialmente', 'Neutro', 'Concordo Parcialmente', 'Concordo Totalmente'] },
            { id: 'q10', num: 10, type: 'ORDINAL', chartType: 'line', title: 'Frequência de prática de hábitos de saúde (hidratação, alimentação equilibrada)', label: 'Hábitos Saudáveis', options: ['Raramente', 'Às Vezes', 'Frequência Moderada', 'Quase Sempre', 'Sempre'] },

            // QUANTITATIVAS DISCRETAS (11-15)
            { id: 'q11', num: 11, type: 'DISCRETA', chartType: 'bar', title: 'Quantidade de redes sociais diferentes acessadas diariamente', label: 'Nº de Redes Usadas', min: 1, max: 8, step: 1, unit: 'redes' },
            { id: 'q12', num: 12, type: 'DISCRETA', chartType: 'bar', title: 'Quantidade de livros lidos por ano (fora os de leitura obrigatória)', label: 'Livros por Ano', min: 0, max: 15, step: 1, unit: 'livros' },
            { id: 'q13', num: 13, type: 'DISCRETA', chartType: 'bar', title: 'Quantos dias por semana pratica esportes ou atividade física', label: 'Dias de Exercício/Semana', min: 0, max: 7, step: 1, unit: 'dias' },
            { id: 'q14', num: 14, type: 'DISCRETA', chartType: 'bar', title: 'Número de amigos próximos (de real confiança)', label: 'Nº de Amigos Próximos', min: 0, max: 10, step: 1, unit: 'amigos' },
            { id: 'q15', num: 15, type: 'DISCRETA', chartType: 'bar', title: 'Horas médias de sono por noite nos dias de aula', label: 'Horas de Sono/Noite', min: 4, max: 10, step: 1, unit: 'horas' },

            // QUANTITATIVAS CONTÍNUSA (16-20)
            { id: 'q16', num: 16, type: 'CONTINUA', chartType: 'histogram', title: 'Tempo médio diário de uso de tela no smartphone (horas)', label: 'Tempo de Tela Diário (h)', min: 1.0, max: 14.0, step: 0.5, unit: 'h' },
            { id: 'q17', num: 17, type: 'CONTINUA', chartType: 'histogram', title: 'Estimativa de gastos pessoais semanais com lazer/lanches (em R$)', label: 'Gasto Semanal (R$)', min: 0.0, max: 200.0, step: 5.0, unit: 'R$' },
            { id: 'q18', num: 18, type: 'CONTINUA', chartType: 'histogram', title: 'Altura aproximada (em metros)', label: 'Altura (m)', min: 1.45, max: 2.05, step: 0.01, unit: 'm' },
            { id: 'q19', num: 19, type: 'CONTINUA', chartType: 'histogram', title: 'Horas semanais dedicadas a hobbies fora das telas (esporte, música, desenho, etc.)', label: 'Hobbies Fora de Tela (h)', min: 0.0, max: 25.0, step: 0.5, unit: 'h' },
            { id: 'q20', num: 20, type: 'CONTINUA', chartType: 'histogram', title: 'Tempo de deslocamento diário até a escola (em minutos)', label: 'Deslocamento Escola (min)', min: 5.0, max: 90.0, step: 5.0, unit: 'min' }
        ];

        let responsesStore = [];
        let activeFilter = 'ALL';
        let chartInstances = {};

        // Bind handlers to global window object
        window.switchTab = switchTab;
        window.filterDashboard = filterDashboard;
        window.handleFormSubmit = handleFormSubmit;
        window.generateMockResponses = generateMockResponses;
        window.exportDataJSON = exportDataJSON;
        window.exportDataCSV = exportDataCSV;
        window.resetDataConfirmation = resetDataConfirmation;
        window.copyQuestionsToClipboard = copyQuestionsToClipboard;
        window.handleFileUpload = handleFileUpload;
        window.processPastedText = processPastedText;

        window.onload = function() {
            initLocalDataStore();
            renderSurveyForm();
            renderDashboard();
            populateFormattedQuestionsText();
        };

        function initLocalDataStore() {
            const saved = localStorage.getItem('stat_teen_responses_v1');
            if (saved) {
                try { responsesStore = JSON.parse(saved); } catch(e) { responsesStore = []; }
            }
            if (responsesStore.length === 0) {
                responsesStore = generateSeedResponses(15);
                saveDataStoreLocallyOnly();
            } else {
                updateBadgeCount();
            }
        }

        function saveDataStoreLocallyOnly() {
            localStorage.setItem('stat_teen_responses_v1', JSON.stringify(responsesStore));
            updateBadgeCount();
        }

        function updateBadgeCount() {
            const badge = document.getElementById('response-count-badge');
            if (badge) badge.innerText = `Total: ${responsesStore.length} Respostas`;
        }

        function generateSeedResponses(count) {
            const seeds = [];
            const now = new Date();
            for(let i=0; i<count; i++) {
                const respTime = new Date(now.getTime() - Math.random() * 86400000 * 7).toLocaleString('pt-BR');
                seeds.push({
                    id: 'resp_' + Math.random().toString(36).substring(2, 9),
                    timestamp: respTime,
                    age: Math.floor(Math.random() * 3) + 15,
                    q1: getRandomItem(['Instagram', 'TikTok', 'Instagram', 'TikTok', 'YouTube', 'WhatsApp', 'Discord', 'X (Twitter)']),
                    q2: getRandomItem(['Funk', 'Trap / Rap', 'Pop', 'Sertanejo', 'Rock / Indie', 'K-Pop', 'Funk']),
                    q3: getRandomItem(['Smartphone', 'Smartphone', 'Notebook / PC', 'Tablet']),
                    q4: getRandomItem(['Redes Sociais', 'Redes Sociais', 'YouTube / Podcasts', 'Portais de Notícias', 'Conversas Família/Amigos']),
                    q5: getRandomItem(['Exatas e Tecnologia', 'Biológicas e Saúde', 'Humanas e Sociais', 'Artes e Comunicação', 'Ainda Não Sei']),
                    q6: getRandomItem(['1 - Muito Baixo', '2 - Baixo', '3 - Moderado', '4 - Alto', '4 - Alto', '5 - Muito Alto']),
                    q7: getRandomItem(['Raramente', 'Às Vezes', 'Frequentemente', 'Frequentemente', 'Sempre']),
                    q8: getRandomItem(['Regular', 'Bom', 'Bom', 'Muito Bom', 'Excelente']),
                    q9: getRandomItem(['Discordo Parcialmente', 'Neutro', 'Concordo Parcialmente', 'Concordo Totalmente']),
                    q10: getRandomItem(['Raramente', 'Às Vezes', 'Frequência Moderada', 'Quase Sempre', 'Sempre']),
                    q11: Math.floor(Math.random() * 4) + 2,
                    q12: Math.floor(Math.random() * 6),
                    q13: Math.floor(Math.random() * 6),
                    q14: Math.floor(Math.random() * 6) + 1,
                    q15: Math.floor(Math.random() * 4) + 5,
                    q16: parseFloat((Math.random() * 6 + 3).toFixed(1)),
                    q17: parseFloat((Math.random() * 80 + 20).toFixed(2)),
                    q18: parseFloat((Math.random() * 0.35 + 1.55).toFixed(2)),
                    q19: parseFloat((Math.random() * 10 + 2).toFixed(1)),
                    q20: parseFloat((Math.random() * 45 + 10).toFixed(0))
                });
            }
            return seeds;
        }

        function getRandomItem(arr) {
            return arr[Math.floor(Math.random() * arr.length)];
        }

        function handleFileUpload(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                parseAndImportCSV(e.target.result);
            };
            reader.readAsText(file);
        }

        function processPastedText() {
            const text = document.getElementById('paste-data-textarea').value.trim();
            if (!text) { showToast('Por favor, cole algum texto da planilha.', 'error'); return; }
            parseAndImportCSV(text);
        }

        function parseAndImportCSV(rawText) {
            const lines = rawText.split(/\r?\n/).filter(line => line.trim() !== '');
            if (lines.length === 0) return;

            let addedCount = 0;

            lines.forEach((line, index) => {
                let delimiter = line.includes('\t') ? '\t' : ',';
                let cols = line.split(delimiter).map(c => c.replace(/^["']|["']$/g, '').trim());

                if (index === 0 && (cols[0].toLowerCase().includes('carimbo') || cols[0].toLowerCase().includes('timestamp') || cols[0].toLowerCase().includes('idade'))) {
                    return; // Skip header row
                }

                if (cols.length >= 20) {
                    let colOffset = cols.length > 20 ? (cols.length - 20) : 0;
                    let newResp = {
                        id: 'resp_imp_' + Math.random().toString(36).substring(2, 9),
                        timestamp: new Date().toLocaleString('pt-BR'),
                        age: 16
                    };

                    QUESTIONS.forEach((q, idx) => {
                        let rawVal = cols[colOffset + idx];
                        if (q.type === 'DISCRETA' || q.type === 'CONTINUA') {
                            let parsed = parseFloat(rawVal ? rawVal.replace(',', '.') : '0');
                            newResp[q.id] = isNaN(parsed) ? 0 : parsed;
                        } else {
                            newResp[q.id] = rawVal || q.options[0];
                        }
                    });

                    responsesStore.push(newResp);
                    addedCount++;
                }
            });

            if (addedCount > 0) {
                saveDataStoreLocallyOnly();
                renderDashboard();
                renderResponsesTable();
                showToast(`${addedCount} respostas importadas com sucesso!`);
                switchTab('dashboard');
            } else {
                showToast('Formato de dados não reconhecido. Certifique-se de colar 20 colunas de dados.', 'error');
            }
        }

        function generateMockResponses(num = 5) {
            const newSeeds = generateSeedResponses(num);
            for (const item of newSeeds) {
                responsesStore.push(item);
            }
            saveDataStoreLocallyOnly();
            renderDashboard();
            renderResponsesTable();
            showToast(`+${num} respostas adicionadas! Estatísticas atualizadas.`);
        }

        function resetDataConfirmation() {
            if (responsesStore.length === 0) return;
            responsesStore = [];
            saveDataStoreLocallyOnly();
            renderDashboard();
            renderResponsesTable();
            showToast('Banco de dados zerado com sucesso.', 'info');
        }

        function populateFormattedQuestionsText() {
            const textarea = document.getElementById('questions-formatted-text');
            if (!textarea) return;
            let text = "PESQUISA DE OPINIÃO - ADOLESCENTES (15 A 17 ANOS)\n\n";
            QUESTIONS.forEach(q => {
                text += `PERGUNTA ${q.num}: ${q.title}\n`;
                text += `Tipo de Variável: ${q.type}\n`;
                if (q.options) {
                    text += `Opções de Resposta: ${q.options.join(' | ')}\n`;
                } else {
                    text += `Unidade: ${q.unit} (Digite o valor numérico)\n`;
                }
                text += `----------------------------------------------------\n\n`;
            });
            textarea.value = text;
        }

        function copyQuestionsToClipboard() {
            const textarea = document.getElementById('questions-formatted-text');
            if (textarea) {
                textarea.select();
                document.execCommand('copy');
                showToast('Perguntas copiadas! Cole no seu Google Forms.');
            }
        }

        function switchTab(tabName) {
            ['dashboard', 'import', 'survey', 'gforms', 'responses'].forEach(t => {
                const el = document.getElementById(`tab-${t}`);
                const btn = document.getElementById(`tab-btn-${t}`);
                if (!el || !btn) return;
                if (t === tabName) {
                    el.classList.remove('hidden');
                    btn.classList.add('bg-indigo-700', 'text-white', 'shadow');
                    btn.classList.remove('text-indigo-200', 'hover:bg-indigo-800');
                } else {
                    el.classList.add('hidden');
                    btn.classList.remove('bg-indigo-700', 'text-white', 'shadow');
                    btn.classList.add('text-indigo-200', 'hover:bg-indigo-800');
                }
            });

            if (tabName === 'responses') renderResponsesTable();
            else if (tabName === 'dashboard') renderDashboard();
        }

        function renderSurveyForm() {
            const container = document.getElementById('survey-questions-list');
            if (!container) return;
            let html = `
                <div class="p-5 bg-indigo-50/60 border border-indigo-100 rounded-2xl">
                    <label class="block font-bold text-slate-800 text-sm mb-2">Qual é a sua idade atual?</label>
                    <div class="flex gap-4">
                        ${[15, 16, 17].map(age => `
                            <label class="flex items-center gap-2 bg-white px-4 py-2 border border-slate-300 rounded-xl cursor-pointer hover:border-indigo-500 font-semibold text-xs">
                                <input type="radio" name="age" value="${age}" required ${age===16?'checked':''}>
                                <span>${age} anos</span>
                            </label>
                        `).join('')}
                    </div>
                </div>
            `;

            QUESTIONS.forEach(q => {
                let badgeColor = '';
                let badgeText = '';
                if(q.type === 'NOMINAL') { badgeColor = 'bg-blue-100 text-blue-800'; badgeText = 'Qualitativa Nominal'; }
                else if(q.type === 'ORDINAL') { badgeColor = 'bg-purple-100 text-purple-800'; badgeText = 'Qualitativa Ordinal'; }
                else if(q.type === 'DISCRETA') { badgeColor = 'bg-amber-100 text-amber-800'; badgeText = 'Quantitativa Discreta'; }
                else { badgeColor = 'bg-emerald-100 text-emerald-800'; badgeText = 'Quantitativa Contínua'; }

                html += `
                    <div class="p-5 bg-white border border-slate-200 rounded-2xl shadow-sm space-y-3">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-bold text-slate-400">Pergunta ${q.num} de 20</span>
                            <span class="text-[10px] uppercase font-bold px-2 py-0.5 rounded-full ${badgeColor}">${badgeText}</span>
                        </div>
                        <h3 class="font-bold text-slate-800 text-sm">${q.title}</h3>
                `;

                if (q.type === 'NOMINAL' || q.type === 'ORDINAL') {
                    html += `<div class="grid grid-cols-1 sm:grid-cols-2 gap-2 pt-1">`;
                    q.options.forEach(opt => {
                        html += `
                            <label class="flex items-center gap-2 bg-slate-50 border border-slate-200 hover:border-indigo-400 hover:bg-indigo-50/30 px-3 py-2.5 rounded-xl cursor-pointer transition text-xs">
                                <input type="radio" name="${q.id}" value="${opt}" required class="text-indigo-600 focus:ring-indigo-500">
                                <span class="text-slate-700 font-medium">${opt}</span>
                            </label>
                        `;
                    });
                    html += `</div>`;
                } else {
                    html += `
                        <div class="flex items-center gap-3 pt-1">
                            <input type="number" name="${q.id}" min="${q.min}" max="${q.max}" step="${q.step}" required
                                class="w-36 bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm font-semibold text-slate-800 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            <span class="text-xs font-medium text-slate-500">${q.unit}</span>
                        </div>
                    `;
                }

                html += `</div>`;
            });

            container.innerHTML = html;
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const formData = new FormData(e.target);

            const newResp = {
                id: 'resp_' + Math.random().toString(36).substring(2, 9),
                timestamp: new Date().toLocaleString('pt-BR'),
                age: parseInt(formData.get('age'))
            };

            QUESTIONS.forEach(q => {
                const val = formData.get(q.id);
                newResp[q.id] = (q.type === 'DISCRETA' || q.type === 'CONTINUA') ? parseFloat(val) : val;
            });

            responsesStore.push(newResp);
            saveDataStoreLocallyOnly();
            e.target.reset();

            showToast('Resposta registrada com sucesso!');
            switchTab('dashboard');
        }

        function calculateStatistics(q, dataList) {
            const values = dataList.map(d => d[q.id]).filter(v => v !== undefined && v !== null);
            const n = values.length;

            if (n === 0) return { n: 0, frequencies: [], mean: 'N/A', median: 'N/A', mode: 'N/A', variance: 'N/A', stdDev: 'N/A' };

            if (q.type === 'NOMINAL' || q.type === 'ORDINAL') {
                const freqMap = {};
                q.options.forEach(opt => freqMap[opt] = 0);
                values.forEach(v => freqMap[v] = (freqMap[v] || 0) + 1);

                let accum = 0;
                const frequencies = q.options.map(opt => {
                    const fi = freqMap[opt] || 0;
                    const fr = n > 0 ? (fi / n) : 0;
                    accum += fi;
                    return { category: opt, fi, fr, pct: (fr * 100).toFixed(1), Fi: accum, FrPct: ((accum / n) * 100).toFixed(1) };
                });

                let maxFi = -1;
                let modes = [];
                frequencies.forEach(f => {
                    if (f.fi > maxFi) { maxFi = f.fi; modes = [f.category]; }
                    else if (f.fi === maxFi && maxFi > 0) modes.push(f.category);
                });

                let medianVal = 'N/A';
                if (q.type === 'ORDINAL') {
                    const half = Math.ceil(n / 2);
                    let runningSum = 0;
                    for (let f of frequencies) {
                        runningSum += f.fi;
                        if (runningSum >= half) { medianVal = f.category; break; }
                    }
                }

                return { n, frequencies, mode: modes.join(', '), median: medianVal, mean: 'N/A', variance: 'N/A', stdDev: 'N/A' };
            }

            const nums = values.map(v => Number(v)).sort((a, b) => a - b);
            const sum = nums.reduce((acc, curr) => acc + curr, 0);
            const mean = sum / n;

            const mid = Math.floor(n / 2);
            const median = (n % 2 !== 0) ? nums[mid] : (nums[mid - 1] + nums[mid]) / 2;

            const counts = {};
            nums.forEach(x => counts[x] = (counts[x] || 0) + 1);
            let maxCount = 0;
            let modes = [];
            for (let k in counts) {
                if (counts[k] > maxCount) { maxCount = counts[k]; modes = [Number(k)]; }
                else if (counts[k] === maxCount) modes.push(Number(k));
            }
            const modeStr = (maxCount === 1 && nums.length > 1) ? 'Amodal' : modes.join(', ');

            let sumSqDiff = 0;
            nums.forEach(x => sumSqDiff += Math.pow(x - mean, 2));
            const variance = n > 1 ? (sumSqDiff / (n - 1)) : 0;
            const stdDev = Math.sqrt(variance);

            let frequencies = [];
            if (q.type === 'DISCRETA') {
                const distValues = [...new Set(nums)].sort((a,b)=>a-b);
                let accum = 0;
                frequencies = distValues.map(val => {
                    const fi = counts[val];
                    accum += fi;
                    return { category: `${val} ${q.unit}`, fi, pct: ((fi/n)*100).toFixed(1), Fi: accum };
                });
            } else {
                const numClasses = 5;
                const minVal = nums[0];
                const maxVal = nums[n - 1];
                const range = (maxVal - minVal) || 1;
                const classWidth = range / numClasses;

                let accum = 0;
                for (let i = 0; i < numClasses; i++) {
                    const lower = minVal + i * classWidth;
                    const upper = lower + classWidth;
                    const isLast = (i === numClasses - 1);
                    const fi = nums.filter(x => isLast ? (x >= lower && x <= upper) : (x >= lower && x < upper)).length;
                    accum += fi;
                    frequencies.push({
                        category: `[${lower.toFixed(1)} |- ${upper.toFixed(1)})`,
                        fi,
                        pct: ((fi/n)*100).toFixed(1),
                        Fi: accum
                    });
                }
            }

            return {
                n, frequencies,
                mean: mean.toFixed(2),
                median: median.toFixed(2),
                mode: modeStr,
                variance: variance.toFixed(2),
                stdDev: stdDev.toFixed(2)
            };
        }

        function filterDashboard(type) {
            activeFilter = type;
            ['ALL', 'NOMINAL', 'ORDINAL', 'DISCRETA', 'CONTINUA'].forEach(t => {
                const btn = document.getElementById(`filter-btn-${t}`);
                if (!btn) return;
                btn.className = (t === type)
                    ? "px-3 py-1.5 rounded-lg font-semibold bg-indigo-600 text-white shadow-sm border border-indigo-600"
                    : "px-3 py-1.5 rounded-lg font-semibold text-slate-600 hover:bg-white hover:shadow-sm";
            });
            renderDashboard();
        }

        function renderDashboard() {
            const container = document.getElementById('questions-dashboard-container');
            if (!container) return;
            container.innerHTML = '';

            const filteredQuestions = QUESTIONS.filter(q => activeFilter === 'ALL' || q.type === activeFilter);

            filteredQuestions.forEach(q => {
                const stats = calculateStatistics(q, responsesStore);
                const card = document.createElement('div');
                card.className = "bg-white rounded-2xl p-6 shadow-sm border border-slate-200/90 space-y-6 transition-all hover:shadow-md";

                let typeBadge = '';
                if(q.type === 'NOMINAL') typeBadge = '<span class="bg-blue-100 text-blue-800 text-[11px] font-bold px-2.5 py-1 rounded-full uppercase">Qualitativa Nominal</span>';
                else if(q.type === 'ORDINAL') typeBadge = '<span class="bg-purple-100 text-purple-800 text-[11px] font-bold px-2.5 py-1 rounded-full uppercase">Qualitativa Ordinal</span>';
                else if(q.type === 'DISCRETA') typeBadge = '<span class="bg-amber-100 text-amber-800 text-[11px] font-bold px-2.5 py-1 rounded-full uppercase">Quantitativa Discreta</span>';
                else typeBadge = '<span class="bg-emerald-100 text-emerald-800 text-[11px] font-bold px-2.5 py-1 rounded-full uppercase">Quantitativa Contínua</span>';

                card.innerHTML = `
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2 border-b border-slate-100 pb-4">
                        <div>
                            <div class="flex items-center gap-2">
                                <span class="font-bold text-slate-400 text-xs">Pergunta #${q.num}</span>
                                ${typeBadge}
                            </div>
                            <h3 class="text-lg font-bold text-slate-900 mt-1">${q.title}</h3>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3">
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Amostra (n)</span>
                            <span class="text-base font-extrabold text-slate-800">${stats.n}</span>
                        </div>
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Média (x̅)</span>
                            <span class="text-base font-extrabold ${stats.mean !== 'N/A' ? 'text-indigo-600' : 'text-slate-400'}">${stats.mean !== 'N/A' ? stats.mean + ' ' + (q.unit||'') : 'N/A'}</span>
                        </div>
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Mediana (Md)</span>
                            <span class="text-base font-extrabold text-slate-800 truncate block">${stats.median}</span>
                        </div>
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Moda (Mo)</span>
                            <span class="text-base font-extrabold text-slate-800 truncate block">${stats.mode}</span>
                        </div>
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Variância (s²)</span>
                            <span class="text-base font-extrabold ${stats.variance !== 'N/A' ? 'text-purple-600' : 'text-slate-400'}">${stats.variance}</span>
                        </div>
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-100">
                            <span class="text-[10px] font-bold text-slate-400 uppercase block">Desvio Padrão (s)</span>
                            <span class="text-base font-extrabold ${stats.stdDev !== 'N/A' ? 'text-emerald-600' : 'text-slate-400'}">${stats.stdDev}</span>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-center">
                        <div class="lg:col-span-6 overflow-x-auto custom-scrollbar">
                            <span class="text-xs font-bold text-slate-500 mb-2 block">Tabela de Frequência Absoluta e Relativa</span>
                            <table class="w-full text-left text-xs border border-slate-200 rounded-xl overflow-hidden">
                                <thead class="bg-slate-100 text-slate-700 font-bold border-b border-slate-200 uppercase text-[10px]">
                                    <tr>
                                        <th class="p-2.5">Categoria / Classe</th>
                                        <th class="p-2.5 text-center">Freq. Absoluta (fᵢ)</th>
                                        <th class="p-2.5 text-center">Freq. Relativa (fᵣ%)</th>
                                        <th class="p-2.5 text-center">Acumulada (Fᵢ)</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-slate-100">
                                    ${stats.frequencies.map(f => `
                                        <tr class="hover:bg-slate-50/80 transition">
                                            <td class="p-2.5 font-semibold text-slate-700">${f.category}</td>
                                            <td class="p-2.5 text-center font-extrabold text-indigo-700">${f.fi}</td>
                                            <td class="p-2.5 text-center font-bold text-slate-600">${f.pct}%</td>
                                            <td class="p-2.5 text-center text-slate-500">${f.Fi}</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>

                        <div class="lg:col-span-6 flex flex-col items-center justify-center min-h-[260px] bg-slate-50/50 p-4 rounded-2xl border border-slate-100 relative">
                            <div class="w-full h-64 relative flex items-center justify-center">
                                <canvas id="chart-canvas-${q.id}"></canvas>
                            </div>
                        </div>
                    </div>
                `;

                container.appendChild(card);
                setTimeout(() => buildQuestionChart(q, stats), 30);
            });
        }

        function buildQuestionChart(q, stats) {
            const canvas = document.getElementById(`chart-canvas-${q.id}`);
            if (!canvas) return;
            if (chartInstances[q.id]) chartInstances[q.id].destroy();

            const ctx = canvas.getContext('2d');
            const labels = stats.frequencies.map(f => f.category);
            const dataValues = stats.frequencies.map(f => f.fi);

            const colors = ['#6366f1', '#3b82f6', '#10b981', '#f59e0b', '#ec4899', '#8b5cf6', '#14b8a6', '#f97316'];
            let chartConfig = {};

            if (q.chartType === 'pie') {
                chartConfig = {
                    type: 'doughnut',
                    data: { labels, datasets: [{ data: dataValues, backgroundColor: colors.slice(0, labels.length) }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom' } } }
                };
            } else if (q.chartType === 'line') {
                chartConfig = {
                    type: 'line',
                    data: { labels, datasets: [{ label: 'Frequência (fᵢ)', data: dataValues, borderColor: '#4f46e5', backgroundColor: 'rgba(79, 70, 229, 0.15)', fill: true, tension: 0.35 }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true } } }
                };
            } else if (q.chartType === 'histogram') {
                chartConfig = {
                    type: 'bar',
                    data: { labels, datasets: [{ label: 'Frequência (fᵢ)', data: dataValues, backgroundColor: 'rgba(16, 185, 129, 0.75)', barPercentage: 1.0, categoryPercentage: 1.0 }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true } } }
                };
            } else {
                chartConfig = {
                    type: 'bar',
                    data: { labels, datasets: [{ label: 'Quantidade', data: dataValues, backgroundColor: 'rgba(245, 158, 11, 0.75)', borderRadius: 6 }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true } } }
                };
            }

            chartInstances[q.id] = new Chart(ctx, chartConfig);
        }

        function renderResponsesTable() {
            const head = document.getElementById('responses-table-head');
            const body = document.getElementById('responses-table-body');
            if (!head || !body) return;

            head.innerHTML = `
                <th class="p-3">#</th>
                <th class="p-3">Data/Hora</th>
                <th class="p-3">Idade</th>
                ${QUESTIONS.map(q => `<th class="p-3 whitespace-nowrap">Q${q.num} (${q.label})</th>`).join('')}
            `;

            if (responsesStore.length === 0) {
                body.innerHTML = `<tr><td colspan="${QUESTIONS.length + 3}" class="p-6 text-center text-slate-400">Nenhuma resposta registrada ainda.</td></tr>`;
                return;
            }

            body.innerHTML = responsesStore.map((r, idx) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-3 font-bold text-slate-400">${idx + 1}</td>
                    <td class="p-3 whitespace-nowrap text-slate-500">${r.timestamp}</td>
                    <td class="p-3 font-semibold text-indigo-600">${r.age} anos</td>
                    ${QUESTIONS.map(q => `<td class="p-3 whitespace-nowrap text-slate-700">${r[q.id] !== undefined ? r[q.id] : '-'}</td>`).join('')}
                </tr>
            `).join('');
        }

        function exportDataJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(responsesStore, null, 2));
            const a = document.createElement('a');
            a.setAttribute("href", dataStr);
            a.setAttribute("download", `pesquisa_dados_${new Date().toISOString().slice(0,10)}.json`);
            a.click();
            showToast('Dados baixados em JSON!');
        }

        function exportDataCSV() {
            if (responsesStore.length === 0) return;
            let csv = 'ID,DataHora,Idade,' + QUESTIONS.map(q => `Q${q.num}_${q.label}`).join(',') + '\n';
            responsesStore.forEach(r => {
                let row = `"${r.id}","${r.timestamp}",${r.age},`;
                row += QUESTIONS.map(q => `"${r[q.id]}"`).join(',');
                csv += row + '\n';
            });
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `respostas_pesquisa_${new Date().toISOString().slice(0,10)}.csv`;
            a.click();
            showToast('CSV Exportado!');
        }

        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            if (!container) return;
            const toast = document.createElement('div');
            const icon = type === 'error' ? 'fa-circle-exclamation text-red-400' : 'fa-check-circle text-emerald-400';
            toast.className = `bg-slate-900 text-white px-4 py-3 rounded-xl shadow-xl flex items-center space-x-3 text-xs font-semibold transform transition-all duration-300 opacity-0 translate-y-2`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-base"></i><span>${message}</span>`;
            container.appendChild(toast);
            setTimeout(() => toast.classList.remove('opacity-0', 'translate-y-2'), 10);
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3500);
        }
    </script>
</body>
</html>
