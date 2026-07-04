Markdown
# Banco API Performance

Este repositório é dedicado à automação e execução de testes de performance, carga e estresse da API bancária, utilizando **Javascript** e **Grafana K6**. O objetivo principal é garantir a resiliência, escalabilidade e conformidade com os SLAs estabelecidos.

* **URL do Repositório:** [https://github.com/DanielCanto10/banco-api-performance](https://github.com/DanielCanto10/banco-api-performance)

---

## 1. Introdução

Este projeto simula o comportamento de múltiplos usuários virtuais (`VUs`) realizando requisições simultâneas em fluxos críticos do sistema. Através das métricas coletadas, torna-se possível identificar vazamentos de memória, tempos excessivos de resposta da base de dados e o comportamento sob estresse severo antes que os serviços entrem em produção.

## 2. Tecnologias Utilizadas

* **JavaScript (ES6+):** Linguagem padrão para a escrita dos fluxos e comportamentos dos usuários virtuais.
* **Grafana K6:** Ferramenta open-source para testes de carga, altamente otimizada, escrita em Go, de baixo consumo de recursos de hardware.
* **Node.js:** Gerenciador de pacotes e scripts utilitários auxiliares.

## 3. Estrutura do Repositório

Abaixo está o mapeamento conceitual da organização de pastas sugerida para o projeto:

```text
banco-api-performance/
├── src/
│   ├── config/          # Perfis de carga, estágios de VU e regras de thresholds
│   ├── fixtures/        # Massas de dados e payloads simulados (JSON, CSV)
│   ├── scenarios/       # Fluxos de negócios reais e cenários de testes executáveis
│   ├── support/         # Bibliotecas auxiliares, utilitários e geradores de dados
├── .gitignore           # Bloqueio de subida de arquivos locais/relatórios
├── package.json         # Manifesto e scripts utilitários locais
└── README.md            # Documentação principal do projeto
4. Objetivo de Cada Grupo de Arquivos
src/config/: Contém as opções globais (options) do K6. Separa a lógica de quanto testar (VUs, duração, falhas toleráveis) da lógica de negócio.

src/data/: Centraliza os arquivos que fornecem insumos para as requisições (massa de dados), evitando que múltiplos VUs enviem exatamente os mesmos dados de transação.

src/scenarios/: Reúne os fluxos que simulam o comportamento do usuário final (ex: login, transferência Pix, extratos). São os pontos de entrada do comando k6 run.

src/support/: Consolida funções comuns, interceptores de requisição, tratamento de cabeçalhos e validações customizadas aplicadas a todos os cenários.

5. Modo de Instalação
Pré-requisito: Instalação do K6
Caso precise instalar o K6 em sua estação de trabalho, use o gerenciador de pacotes correspondente:

Bash
# macOS
brew install k6

# Windows (via Chocolatey)
choco install k6

# Linux (Debian/Ubuntu)
sudo gpg -k
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] [https://dl.k6.io/deb](https://dl.k6.io/deb) stable main" | sudo tee /etc/apt/sources.list.d/k6.list
sudo apt-get update
sudo apt-get install k6
Clonando e Instalando o Repositório
Bash
git clone [https://github.com/DanielCanto10/banco-api-performance.git](https://github.com/DanielCanto10/banco-api-performance.git)
cd banco-api-performance
npm install
6. Modo de Execução do Projeto
Todas as execuções exigem que o endpoint do ambiente seja informado através da variável de ambiente BASE_URL.

Execução Simples (Via Linha de Comando)
Substitua pela URL do ambiente desejado (Homologação, Staging, etc.):

Bash
BASE_URL=[https://api.homolog.seubanco.com.br](https://api.homolog.seubanco.com.br) k6 run src/scenarios/seu_cenario.js
Execução Avançada (Acompanhamento em Tempo Real e Exportação de Relatório)
Para visualizar as métricas sendo plotadas graficamente em tempo real através do navegador e forçar o salvamento do relatório estatístico em arquivo HTML próprio ao término, execute o K6 injetando as variáveis internas conforme o modelo abaixo:

Bash
BASE_URL=[https://api.homolog.seubanco.com.br](https://api.homolog.seubanco.com.br) K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=html-report.html k6 run src/scenarios/seu_cenario.js
Benefícios adicionais deste comando:

Interface Gráfica: Durante o teste, abra o navegador e acesse http://localhost:5665 para visualizar gráficos vivos e de fácil leitura.

Relatório Físico: Um arquivo denominado html-report.html será compilado na pasta raiz do projeto de forma automática ao término do teste, pronto para compartilhamento com a equipe de desenvolvimento.
