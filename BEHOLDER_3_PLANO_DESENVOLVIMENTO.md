# Beholder 3.0 - Plano de Desenvolvimento

> Referencia: [BEHOLDER_3_DISCOVERY.md](./BEHOLDER_3_DISCOVERY.md)

---

## Visao Geral

| Item | Detalhe |
|---|---|
| **Projeto** | Beholder 3.0 - Bot de Trading Multi-Estrategia |
| **Duracao Total** | 16 sprints (16 semanas) |
| **Metodologia** | Sprints semanais com entregas incrementais |
| **Equipe minima** | 1 fullstack dev (pode ser feito solo) |
| **Premissa** | Cada sprint = 1 semana, ~40h de trabalho |

### Criterios de Pronto (Definition of Done)
- Codigo funcional e testado
- Sem erros no console (backend e frontend)
- Endpoints testados via REST client (Insomnia/Postman)
- Componentes renderizando sem quebra
- Commit com mensagem descritiva

---

## FASE 1 - FUNDACAO

### Sprint 1: Setup do Projeto e Infraestrutura

**Objetivo**: Ter o ambiente de desenvolvimento rodando com Docker e a estrutura base do backend e frontend criados.

#### Backend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 1.1 | Inicializar projeto Node.js (`npm init`) | `backend/package.json` | CRITICA |
| 1.2 | Instalar dependencias core: express, sequelize, mysql2, dotenv, cors, helmet, bcryptjs, jsonwebtoken | `backend/package.json` | CRITICA |
| 1.3 | Criar estrutura de pastas MVC (config, models, controllers, routes, services, repositories, middlewares, utils) | `backend/src/*` | CRITICA |
| 1.4 | Configurar Express app (middlewares globais: json, cors, helmet, error handler) | `backend/src/app.js` | CRITICA |
| 1.5 | Configurar entry point com HTTP server | `backend/src/server.js` | CRITICA |
| 1.6 | Criar arquivo `.env.example` com todas as variaveis necessarias | `backend/.env.example` | ALTA |
| 1.7 | Configurar Sequelize (connection, dialect, pool) | `backend/src/config/database.js`, `backend/.sequelizerc` | CRITICA |

#### Frontend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 1.8 | Criar projeto React com Create React App ou Vite | `frontend/` | CRITICA |
| 1.9 | Instalar dependencias: react-router-dom, axios, bootstrap, react-bootstrap, react-toastify | `frontend/package.json` | CRITICA |
| 1.10 | Configurar Bootstrap 5 e tema base (dark theme) | `frontend/src/index.js`, CSS | ALTA |
| 1.11 | Criar layout base: Header, Sidebar, Footer, Container | `frontend/src/components/common/` | ALTA |
| 1.12 | Configurar React Router com rotas placeholder | `frontend/src/Routes.js` | MEDIA |

#### Infraestrutura
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 1.13 | Criar `docker-compose.yml` (Node.js + MySQL 8 + Adminer) | `docker-compose.yml` | CRITICA |
| 1.14 | Criar Dockerfile backend e frontend | `backend/Dockerfile`, `frontend/Dockerfile` | ALTA |
| 1.15 | Configurar `.gitignore` (node_modules, .env, build) | `.gitignore` | CRITICA |

#### Entregavel Sprint 1:
> `docker-compose up` sobe o banco MySQL, backend rodando na porta 3001, frontend na porta 3000, com pagina inicial renderizando o layout base.

---

### Sprint 2: Banco de Dados, Autenticacao e Settings

**Objetivo**: Modelagem completa do banco, sistema de login funcionando, e CRUD de configuracoes.

#### Migrations e Models
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 2.1 | Migration: tabela `users` | `backend/migrations/` | CRITICA |
| 2.2 | Migration: tabela `settings` | `backend/migrations/` | CRITICA |
| 2.3 | Migration: tabela `symbols` | `backend/migrations/` | CRITICA |
| 2.4 | Migration: tabela `orders` | `backend/migrations/` | ALTA |
| 2.5 | Migration: tabela `monitors` | `backend/migrations/` | ALTA |
| 2.6 | Migration: tabela `automations` | `backend/migrations/` | ALTA |
| 2.7 | Migration: tabela `actions` | `backend/migrations/` | ALTA |
| 2.8 | Migration: tabela `orderTemplates` | `backend/migrations/` | ALTA |
| 2.9 | Migration: tabela `logs` | `backend/migrations/` | MEDIA |
| 2.10 | Criar todos os Models Sequelize com associations | `backend/src/models/` | CRITICA |
| 2.11 | Seeder: usuario admin padrao | `backend/seeders/` | CRITICA |
| 2.12 | Criar `models/index.js` com init e associations | `backend/src/models/index.js` | CRITICA |

#### Autenticacao
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 2.13 | Criar `usersRepository.js` (findByEmail, create, update) | `backend/src/repositories/` | CRITICA |
| 2.14 | Criar `authController.js` (login, logout, refresh) | `backend/src/controllers/` | CRITICA |
| 2.15 | Criar `authMiddleware.js` (verificacao JWT) | `backend/src/middlewares/` | CRITICA |
| 2.16 | Criar `authRoutes.js` (POST /login, /logout, /refresh) | `backend/src/routes/` | CRITICA |
| 2.17 | Implementar hash Bcrypt no registro/login | `authController.js` | CRITICA |
| 2.18 | Criar `crypto.js` utilitario (encrypt/decrypt AES-256 para API keys) | `backend/src/utils/crypto.js` | ALTA |

#### Settings e Symbols
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 2.19 | Criar `settingsRepository.js` e `settingsController.js` | `backend/src/repositories/`, `controllers/` | ALTA |
| 2.20 | Criar `symbolsRepository.js` e `symbolsController.js` | `backend/src/repositories/`, `controllers/` | ALTA |
| 2.21 | Rotas: GET/PATCH /settings, GET/PATCH /symbols, POST /symbols/sync | `backend/src/routes/` | ALTA |
| 2.22 | Registrar todas as rotas no `app.js` | `backend/src/app.js` | CRITICA |

#### Frontend - Login
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 2.23 | Criar `services/api.js` (Axios instance com interceptors JWT) | `frontend/src/services/api.js` | CRITICA |
| 2.24 | Criar `AuthContext.js` (estado de autenticacao global) | `frontend/src/context/AuthContext.js` | CRITICA |
| 2.25 | Criar tela de Login | `frontend/src/components/Login/` | CRITICA |
| 2.26 | Criar rota protegida (redirect para /login se nao autenticado) | `frontend/src/Routes.js` | CRITICA |
| 2.27 | Criar tela de Settings (formulario com API keys, email, Telegram) | `frontend/src/components/Settings/` | ALTA |

#### Entregavel Sprint 2:
> Usuario faz login, recebe JWT, acessa tela de settings, configura API keys da Binance (salvas com criptografia), e sincroniza symbols da exchange. Todas as migrations rodaram e o banco esta modelado.

---

## FASE 2 - DASHBOARD E REAL-TIME

### Sprint 3: Exchange Service e Conexao com Binance

**Objetivo**: Backend se conectando a Binance via REST API, buscando dados de mercado e carteira.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 3.1 | Instalar `node-binance-api` | `backend/package.json` | CRITICA |
| 3.2 | Criar `config/binance.js` (configuracao do client com API keys decriptadas) | `backend/src/config/binance.js` | CRITICA |
| 3.3 | Criar `exchangeService.js` - metodos REST: | `backend/src/services/exchangeService.js` | CRITICA |
|     | - `balance()` - saldo da carteira | | |
|     | - `exchangeInfo()` - info de todos os symbols | | |
|     | - `miniTickerStream()` - iniciar stream ticker | | |
|     | - `bookTickerStream()` - iniciar stream book | | |
|     | - `chartStream()` - iniciar stream klines | | |
|     | - `userDataStream()` - iniciar user data stream | | |
|     | - `buy/sell/cancel()` - operacoes de ordens | | |
| 3.4 | Implementar rate limiting interno (fila de requisicoes, max 1200/min) | `exchangeService.js` | ALTA |
| 3.5 | Implementar retry com exponential backoff para chamadas REST | `exchangeService.js` | ALTA |
| 3.6 | Criar endpoints dashboard: GET /dashboard, GET /wallet/balance | `backend/src/controllers/dashboardController.js` | ALTA |
| 3.7 | Criar `walletService.js` (logica de carteira, cache de saldos) | `backend/src/services/walletService.js` | ALTA |
| 3.8 | Testar conexao REST: buscar saldos, exchange info, symbols | Testes manuais | CRITICA |

#### Entregavel Sprint 3:
> Backend conecta na Binance, busca saldos e informacoes de mercado. Endpoints /dashboard e /wallet retornam dados reais.

---

### Sprint 4: Exchange Monitor (WebSocket Streams)

**Objetivo**: Backend recebendo dados em tempo real da Binance via WebSocket streams.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 4.1 | Criar `exchangeMonitor.js` (gerenciador de streams) | `backend/src/services/exchangeMonitor.js` | CRITICA |
| 4.2 | Implementar Mini Ticker stream (`!miniTicker@arr`) | `exchangeMonitor.js` | CRITICA |
| 4.3 | Implementar Book Ticker stream (`<symbol>@bookTicker`) | `exchangeMonitor.js` | CRITICA |
| 4.4 | Implementar Klines stream (`<symbol>@kline_<interval>`) | `exchangeMonitor.js` | CRITICA |
| 4.5 | Implementar User Data Stream (listen key lifecycle) | `exchangeMonitor.js` | ALTA |
| 4.6 | Reconnect automatico com exponential backoff | `exchangeMonitor.js` | ALTA |
| 4.7 | Criar sistema de broadcast interno (EventEmitter ou similar) | `exchangeMonitor.js` | ALTA |
| 4.8 | Inicializacao automatica dos monitores de sistema no startup | `server.js` | ALTA |
| 4.9 | Logs de conexao/desconexao/erro dos streams | `exchangeMonitor.js` | MEDIA |

#### Entregavel Sprint 4:
> Ao iniciar o backend, ele conecta automaticamente nos streams da Binance e recebe tickers, books e klines em real-time. Logs no console mostram dados chegando.

---

### Sprint 5: WebSocket Server (Backend -> Frontend)

**Objetivo**: Frontend recebendo dados em tempo real do backend via WebSocket proprio.

#### Backend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 5.1 | Criar `webSocketServer.js` (WS server integrado ao HTTP server) | `backend/src/services/webSocketServer.js` | CRITICA |
| 5.2 | Autenticacao no handshake (validar JWT no header/query) | `webSocketServer.js` | CRITICA |
| 5.3 | Implementar broadcast de eventos: `balance`, `miniTicker`, `bookTicker`, `kline` | `webSocketServer.js` | CRITICA |
| 5.4 | Conectar Exchange Monitor ao WS Server (pipe de dados) | `server.js` | CRITICA |
| 5.5 | Gerenciamento de conexoes (heartbeat, cleanup) | `webSocketServer.js` | ALTA |

#### Frontend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 5.6 | Instalar dependencia WS client (ou usar nativo WebSocket) | `frontend/package.json` | CRITICA |
| 5.7 | Criar `services/webSocketClient.js` (connection manager) | `frontend/src/services/` | CRITICA |
| 5.8 | Criar hook `useWebSocket.js` (connect, subscribe, reconnect) | `frontend/src/hooks/` | CRITICA |
| 5.9 | Criar `TickerContext.js` (distribuir dados ticker para componentes) | `frontend/src/context/` | ALTA |
| 5.10 | Criar hook `useTicker.js` (acesso facil aos dados do ticker) | `frontend/src/hooks/` | ALTA |

#### Entregavel Sprint 5:
> Frontend conecta ao WS do backend, recebe dados em real-time. Console do browser mostra tickers e books chegando continuamente.

---

### Sprint 6: Dashboard UI

**Objetivo**: Dashboard completo e funcional com dados real-time e graficos TradingView.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 6.1 | Instalar TradingView Lightweight Charts (`lightweight-charts`) | `frontend/package.json` | CRITICA |
| 6.2 | Criar `Dashboard.js` (layout principal com grid responsivo) | `frontend/src/components/Dashboard/` | CRITICA |
| 6.3 | Criar `MiniTicker.js` (cards com preco, variacao %, volume das top moedas) | `Dashboard/MiniTicker.js` | CRITICA |
| 6.4 | Criar `BookTicker.js` (tabela bid/ask em real-time) | `Dashboard/BookTicker.js` | ALTA |
| 6.5 | Criar `CandleChart.js` (grafico TradingView com candles real-time) | `Dashboard/CandleChart.js` | CRITICA |
| 6.6 | Criar `Wallet.js` (painel de saldos: moeda, disponivel, em ordens, equivalente USD) | `Dashboard/Wallet.js` | ALTA |
| 6.7 | Criar `SelectSymbol.js` (dropdown/autocomplete para trocar par de moedas) | `components/common/SelectSymbol.js` | ALTA |
| 6.8 | Integrar todos os componentes com TickerContext (dados real-time) | Todos os componentes Dashboard | CRITICA |
| 6.9 | Responsividade mobile (Bootstrap grid + media queries) | CSS | MEDIA |
| 6.10 | Animacoes de atualizacao (flash verde/vermelho nos precos) | CSS/JS | BAIXA |

#### Entregavel Sprint 6:
> Dashboard funcional: grafico TradingView com candles atualizando, cards de ticker com precos em real-time, book de ofertas, carteira com saldos. Tudo reativo e responsivo.

---

## FASE 3 - ORDENS

### Sprint 7: Orders Service (Backend)

**Objetivo**: Backend capaz de posicionar, cancelar e sincronizar os 6 tipos de ordens na Binance.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 7.1 | Criar `ordersRepository.js` (CRUD ordens no banco) | `backend/src/repositories/` | CRITICA |
| 7.2 | Criar `ordersService.js` com metodos para cada tipo: | `backend/src/services/ordersService.js` | CRITICA |
|     | - `placeMarketOrder(symbol, side, quantity)` | | |
|     | - `placeLimitOrder(symbol, side, quantity, price)` | | |
|     | - `placeStopLossOrder(symbol, side, quantity, stopPrice)` | | |
|     | - `placeStopLossLimitOrder(symbol, side, qty, price, stopPrice)` | | |
|     | - `placeTakeProfitOrder(symbol, side, quantity, stopPrice)` | | |
|     | - `placeTakeProfitLimitOrder(symbol, side, qty, price, stopPrice)` | | |
|     | - `cancelOrder(symbol, orderId)` | | |
|     | - `syncOrder(symbol, orderId)` | | |
| 7.3 | Validacao pre-ordem (minNotional, lotSize, stepSize, tickSize, saldo) | `ordersService.js` | CRITICA |
| 7.4 | Arredondamento de quantidade e preco conforme precision do symbol | `backend/src/utils/calcUtils.js` | CRITICA |
| 7.5 | Criar `ordersController.js` (endpoints REST) | `backend/src/controllers/` | CRITICA |
| 7.6 | Rotas: GET /orders, POST /orders, DELETE /orders/:symbol/:orderId | `backend/src/routes/ordersRoutes.js` | CRITICA |
| 7.7 | Paginacao e filtros no GET /orders (symbol, status, side, date range) | `ordersRepository.js` | ALTA |
| 7.8 | Gerar `clientOrderId` unico para cada ordem (idempotencia) | `ordersService.js` | ALTA |

#### Entregavel Sprint 7:
> Via REST client (Postman/Insomnia) e possivel posicionar os 6 tipos de ordens na Binance Testnet, cancelar ordens e listar historico.

---

### Sprint 8: User Data Stream e Orders UI

**Objetivo**: Ordens atualizando automaticamente via stream + interface visual completa.

#### Backend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 8.1 | Processar evento `executionReport` do User Data Stream | `exchangeMonitor.js` | CRITICA |
| 8.2 | Atualizar ordem no banco quando status muda (FILLED, CANCELED, etc) | `ordersService.js` | CRITICA |
| 8.3 | Calcular `net` (valor liquido) e `commission` em ordens FILLED | `ordersService.js` | ALTA |
| 8.4 | Broadcast evento `execution` via WS Server ao frontend | `webSocketServer.js` | CRITICA |
| 8.5 | Processar `outboundAccountPosition` (atualizar saldo em cache) | `exchangeMonitor.js` | ALTA |

#### Frontend
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 8.6 | Criar `OrdersPage.js` (tabela de ordens com filtros) | `frontend/src/components/Orders/` | CRITICA |
| 8.7 | Criar `NewOrderModal.js` (formulario com os 6 tipos, validacoes visuais) | `Orders/NewOrderModal.js` | CRITICA |
| 8.8 | Campos dinamicos no modal conforme tipo selecionado (MARKET so qty, LIMIT qty+price, etc) | `NewOrderModal.js` | ALTA |
| 8.9 | Criar `OrderRow.js` (linha da tabela com status badge, botao cancelar) | `Orders/OrderRow.js` | ALTA |
| 8.10 | Integrar com WS: atualizacao automatica da tabela ao receber `execution` | `OrdersPage.js` | CRITICA |
| 8.11 | Calculadora de quantidade (input de % do saldo disponivel) | `NewOrderModal.js` | MEDIA |
| 8.12 | Confirmacao antes de cancelar ordem (modal de confirmacao) | `OrderRow.js` | MEDIA |

#### Entregavel Sprint 8:
> Usuario posiciona ordens pelo frontend, ve status atualizando em real-time (NEW -> FILLED), cancela ordens com confirmacao. Saldo da carteira atualiza automaticamente.

---

### Sprint 9: Order Templates

**Objetivo**: Templates reutilizaveis de ordens com quantidade e preco dinamicos.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 9.1 | Criar `orderTemplatesRepository.js` (CRUD) | `backend/src/repositories/` | CRITICA |
| 9.2 | Criar `orderTemplatesController.js` e rotas | `backend/src/controllers/`, `routes/` | CRITICA |
| 9.3 | Implementar parser de expressoes dinamicas de quantidade: | `backend/src/utils/calcUtils.js` | ALTA |
|     | - `LAST_ORDER_QTY` (quantidade da ultima ordem) | | |
|     | - `WALLET_QTY` (saldo disponivel da moeda) | | |
|     | - `MIN_NOTIONAL` (quantidade minima permitida) | | |
|     | - Multiplicadores: `WALLET_QTY * 0.5` (50% do saldo) | | |
| 9.4 | Implementar parser de expressoes dinamicas de preco: | `calcUtils.js` | ALTA |
|     | - `LAST_PRICE` (ultimo preco do ticker) | | |
|     | - `BOOK_ASK` / `BOOK_BID` (melhor ask/bid) | | |
|     | - Multiplicadores: `LAST_PRICE * 1.02` (+2%) | | |
| 9.5 | Criar `TemplatesPage.js` (tabela CRUD de templates) | `frontend/src/components/OrderTemplates/` | ALTA |
| 9.6 | Criar `TemplateModal.js` (formulario com campos dinamicos) | `OrderTemplates/TemplateModal.js` | ALTA |
| 9.7 | Dropdown de variaveis disponíveis nos campos de qty/price | `TemplateModal.js` | MEDIA |

#### Entregavel Sprint 9:
> Usuario cria templates como "Comprar 50% do saldo de BTC a 2% abaixo do preco atual". Templates ficam salvos e prontos para uso nas automacoes.

---

## FASE 4 - MONITORES E INDICADORES

### Sprint 10: Monitor Manager e CRUD

**Objetivo**: Sistema de gerenciamento de monitores com CRUD completo e lifecycle.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 10.1 | Criar `monitorsRepository.js` (CRUD + query por tipo/status) | `backend/src/repositories/` | CRITICA |
| 10.2 | Criar `monitorsController.js` e rotas | `backend/src/controllers/`, `routes/` | CRITICA |
| 10.3 | Implementar start/stop de monitores individuais | `monitorsController.js` | CRITICA |
| 10.4 | Refatorar `exchangeMonitor.js` para aceitar monitores dinamicos | `exchangeMonitor.js` | CRITICA |
| 10.5 | Monitores de sistema (auto-start): miniTicker, userData | `exchangeMonitor.js` | ALTA |
| 10.6 | Monitores customizados: candles por symbol/interval | `exchangeMonitor.js` | ALTA |
| 10.7 | Criar `MonitorsPage.js` (tabela com status ativo/inativo, toggle) | `frontend/src/components/Monitors/` | ALTA |
| 10.8 | Criar `MonitorModal.js` (form: symbol, tipo, intervalo, indicadores) | `Monitors/MonitorModal.js` | ALTA |

#### Entregavel Sprint 10:
> Usuario cria monitores customizados (ex: "Monitorar BTCUSDT candles 15min"), liga/desliga monitores. Backend gerencia streams dinamicamente.

---

### Sprint 11: Indicadores Tecnicos (Parte 1)

**Objetivo**: Implementar os indicadores de tendencia, momentum e volatilidade.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 11.1 | Instalar `technicalindicators` lib | `backend/package.json` | CRITICA |
| 11.2 | Criar `indicatorsService.js` (facade para todos os indicadores) | `backend/src/services/indicatorsService.js` | CRITICA |
| 11.3 | Criar estrutura `BRAIN` (Map em memoria: `SYMBOL:INDICATOR_PERIOD` -> value) | `backend/src/utils/indexes.js` | CRITICA |
| 11.4 | Implementar indicadores de **Tendencia**: | `indicatorsService.js` | CRITICA |
|     | SMA, EMA, WEMA, MACD, ADX, PSAR, Ichimoku | | |
| 11.5 | Implementar indicadores de **Momentum**: | `indicatorsService.js` | CRITICA |
|     | RSI, Stochastic RSI, Stochastic, CCI, ROC, Williams %R, | | |
|     | Awesome Oscillator, KST, TRIX | | |
| 11.6 | Implementar indicadores de **Volatilidade**: | `indicatorsService.js` | ALTA |
|     | Bollinger Bands, ATR | | |
| 11.7 | Implementar indicadores de **Volume**: | `indicatorsService.js` | ALTA |
|     | OBV, ADL, MFI, Force Index, VWAP, Volume Profile | | |
| 11.8 | Pipeline: Kline chega -> calcula indicadores -> armazena no BRAIN | `exchangeMonitor.js` + `indicatorsService.js` | CRITICA |
| 11.9 | Broadcast de indexes atualizados via WS (evento `indexes`) | `webSocketServer.js` | ALTA |

#### Entregavel Sprint 11:
> Ao receber candles da Binance, o backend calcula automaticamente todos os indicadores configurados e armazena no BRAIN. Valores acessiveis via `MEMORY['BTCUSDT:RSI_14']`.

---

### Sprint 12: Padroes de Candlestick e UI de Indexes

**Objetivo**: Detectar padroes graficos em real-time e exibir todos os indexes no frontend.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 12.1 | Criar `patternsService.js` (facade para padroes de candles) | `backend/src/services/patternsService.js` | CRITICA |
| 12.2 | Implementar padroes **Altistas** (13 padroes): | `patternsService.js` | CRITICA |
|     | Hammer, Inverted Hammer, Bullish Engulfing, Bullish Harami, | | |
|     | Bullish Harami Cross, Morning Star, Morning Doji Star, | | |
|     | Piercing Line, Abandoned Baby Bull, Three White Soldiers, | | |
|     | Tweezer Bottom, Bull Pinbar | | |
| 12.3 | Implementar padroes **Baixistas** (13 padroes): | `patternsService.js` | CRITICA |
|     | Hanging Man, Shooting Star, Bearish Engulfing, Bearish Harami, | | |
|     | Bearish Harami Cross, Evening Star, Evening Doji Star, | | |
|     | Dark Cloud Cover, Abandoned Baby Bear, Three Black Crows, | | |
|     | Tweezer Top, Downside Tasuki Gap, Bear Pinbar | | |
| 12.4 | Implementar padroes **Neutros** (7 padroes): | `patternsService.js` | ALTA |
|     | Doji, Dragonfly Doji, Gravestone Doji, Spinning Top Bull/Bear, | | |
|     | Inside Candle, Marubozu Bull/Bear | | |
| 12.5 | Pipeline: Kline chega -> detecta padroes -> armazena no BRAIN | `exchangeMonitor.js` + `patternsService.js` | CRITICA |
| 12.6 | Criar `IndexesPanel.js` (painel com todos os valores calculados) | `frontend/src/components/Monitors/IndexesPanel.js` | ALTA |
| 12.7 | Exibir indicadores agrupados por categoria (tendencia, momentum, etc) | `IndexesPanel.js` | ALTA |
| 12.8 | Exibir padroes detectados com badge (altista=verde, baixista=vermelho) | `IndexesPanel.js` | MEDIA |
| 12.9 | Atualizacao real-time do painel via WS (evento `indexes`) | `IndexesPanel.js` | ALTA |

#### Entregavel Sprint 12:
> Todos os 60+ indicadores e padroes calculados em real-time. Frontend exibe painel com valores atualizando. Ex: "BTCUSDT: RSI=65.3, MACD=+125, Bullish Engulfing=true".

---

## FASE 5 - AUTOMACOES (CORE)

### Sprint 13: Motor de Automacoes (Engine)

**Objetivo**: Core do sistema - motor que avalia condicoes e executa acoes automaticamente.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 13.1 | Criar `automationsRepository.js` (CRUD com Actions eager loading) | `backend/src/repositories/` | CRITICA |
| 13.2 | Criar `automationsService.js` - o CEREBRO do bot: | `backend/src/services/automationsService.js` | CRITICA |
|     | - `evalConditions(conditions, indexes)` - avalia expressao logica | | |
|     | - `executeActions(actions, automation)` - executa acoes | | |
|     | - `processAutomations(indexes)` - loop principal | | |
| 13.3 | Parser de condicoes (converter string em expressao avaliavel): | `automationsService.js` | CRITICA |
|     | - Variaveis: `MEMORY['BTCUSDT:RSI_14']`, `MEMORY['BTCUSDT:MACD']` | | |
|     | - Operadores: `>`, `<`, `>=`, `<=`, `==`, `!=` | | |
|     | - Logicos: `&&`, `\|\|`, `(`, `)` | | |
|     | - Exemplo: `MEMORY['BTCUSDT:RSI_14'] < 30 && MEMORY['BTCUSDT:MACD'] > 0` | | |
| 13.4 | Sandboxed eval (usar `vm2` ou parser customizado, NUNCA eval() direto) | `automationsService.js` | CRITICA |
| 13.5 | Cruzamento de indicadores: detectar quando valor cruza threshold | `automationsService.js` | ALTA |
|     | - Ex: EMA9 cruza acima de EMA21 (crossover) | | |
|     | - Armazenar valor anterior para comparacao | | |
| 13.6 | Executar order template quando condicao e satisfeita | `automationsService.js` + `ordersService.js` | CRITICA |
| 13.7 | Cooldown entre execucoes da mesma automacao (evitar spam) | `automationsService.js` | ALTA |
| 13.8 | Integrar com Exchange Monitor: a cada atualizacao de indexes, rodar automacoes | `exchangeMonitor.js` | CRITICA |
| 13.9 | Criar `automationsController.js` e rotas CRUD + start/stop | `backend/src/controllers/`, `routes/` | CRITICA |
| 13.10 | Logging detalhado: qual automacao disparou, condicao avaliada, resultado | `automationsService.js` | ALTA |

#### Entregavel Sprint 13:
> Motor de automacoes funcionando: quando RSI < 30, o bot automaticamente posiciona uma ordem de compra usando o template configurado. Logs mostram avaliacao e execucao.

---

### Sprint 14: Grid Trading, Trailing Stop e Agendamento

**Objetivo**: Estrategias avancadas - grid, trailing stop movel e ordens agendadas.

| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 14.1 | Criar `gridService.js`: | `backend/src/services/gridService.js` | ALTA |
|     | - Definir faixa (upperPrice, lowerPrice) e numero de niveis | | |
|     | - Calcular precos de cada nivel automaticamente | | |
|     | - Posicionar ordens LIMIT em cada nivel | | |
|     | - Reposicionar apos execucao (comprou no nivel N -> vende no N+1) | | |
|     | - Controle de P&L acumulado do grid | | |
| 14.2 | Criar `trailingStopService.js`: | `backend/src/services/trailingStopService.js` | ALTA |
|     | - Monitorar preco em real-time | | |
|     | - Callback distance (% ou valor fixo) | | |
|     | - Mover stop automaticamente conforme preco sobe | | |
|     | - Disparar venda quando preco reverte alem do callback | | |
| 14.3 | Implementar agendamento via cron (node-cron): | `automationsService.js` | MEDIA |
|     | - DCA (Dollar Cost Averaging) - compra recorrente | | |
|     | - Ordens programadas para horario especifico | | |
| 14.4 | Implementar Wildcard automations (symbol = `*`): | `automationsService.js` | ALTA |
|     | - Mesma condicao aplicada a todos os pares monitorados | | |
|     | - Filtro de moedas elegiveis | | |
| 14.5 | Implementar condicoes de entrada + saida vinculadas: | `automationsService.js` | ALTA |
|     | - Automacao de compra dispara automacao de venda | | |
|     | - Par de automacoes (ida e volta) | | |

#### Entregavel Sprint 14:
> Grid trading funcional (posiciona ordens em niveis automaticamente), trailing stop acompanha tendencia, DCA executa compras periodicas. Automacoes wildcard aplicam estrategia em multiplas moedas.

---

### Sprint 15: Alertas e Automations UI

**Objetivo**: Sistema de alertas completo + interface visual para montar estrategias.

#### Backend - Alertas
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 15.1 | Criar `alertsService.js` (facade de notificacoes): | `backend/src/services/alertsService.js` | CRITICA |
| 15.2 | Implementar envio de email (Nodemailer/SendGrid) | `alertsService.js` | CRITICA |
| 15.3 | Implementar envio Telegram (node-telegram-bot-api) | `alertsService.js` | CRITICA |
| 15.4 | Implementar push notification (Web Push API) | `alertsService.js` | MEDIA |
| 15.5 | Templates de mensagem customizaveis (com variaveis: symbol, price, indicator) | `alertsService.js` | ALTA |
| 15.6 | Integrar alertas como tipo de Action nas automacoes | `automationsService.js` | CRITICA |

#### Frontend - Automations UI
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 15.7 | Criar `AutomationsPage.js` (tabela com status, toggle ativo/inativo) | `frontend/src/components/Automations/` | CRITICA |
| 15.8 | Criar `AutomationModal.js` (formulario principal) | `Automations/AutomationModal.js` | CRITICA |
| 15.9 | Criar `ConditionsBuilder.js` (builder visual de condicoes): | `Automations/ConditionsBuilder.js` | CRITICA |
|      | - Dropdown com todos os indicadores disponíveis do BRAIN | | |
|      | - Dropdown de operadores (>, <, ==, etc) | | |
|      | - Input de valor (fixo ou outro indicador para cruzamento) | | |
|      | - Botoes AND / OR para combinar condicoes | | |
|      | - Preview textual da expressao gerada | | |
| 15.10 | Criar `ActionsBuilder.js` (selecao de acoes): | `Automations/ActionsBuilder.js` | CRITICA |
|       | - Selecionar Order Template existente | | |
|       | - Selecionar tipo de alerta (email, Telegram, push) | | |
|       | - Multiplas acoes por automacao | | |
| 15.11 | Criar `GridModal.js` (configuracao visual do grid) | `Automations/GridModal.js` | ALTA |
| 15.12 | Criar `TrailingStopConfig.js` (configuracao do trailing) | `Automations/TrailingStopConfig.js` | ALTA |
| 15.13 | Logs de execucao por automacao (timeline visual) | `AutomationsPage.js` | MEDIA |

#### Entregavel Sprint 15:
> Interface completa para montar estrategias visualmente: usuario seleciona "RSI < 30 AND MACD > 0" -> "Comprar BTC (template)" + "Alerta Telegram". Alertas disparam por email e Telegram em producao.

---

## FASE 6 - FINALIZACAO

### Sprint 16: Agente IA, Relatorios e Deploy

**Objetivo**: Agente consultor com OpenAI, relatorios de performance e deploy profissional.

#### Agente IA
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 16.1 | Instalar `openai` SDK | `backend/package.json` | ALTA |
| 16.2 | Criar `config/openai.js` (configuracao do client) | `backend/src/config/openai.js` | ALTA |
| 16.3 | Criar `aiService.js`: | `backend/src/services/aiService.js` | ALTA |
|      | - System prompt especializado em crypto/trading/analise tecnica | | |
|      | - Injetar contexto: indicadores atuais do BRAIN, posicoes abertas | | |
|      | - Funcoes: analise de mercado, sugestao de estrategia, | | |
|      |   explicacao de padroes, auxilio em automacoes | | |
|      | - Historico de conversa (ultimas N mensagens) | | |
|      | - Rate limiting (max X requests/hora) | | |
| 16.4 | Criar `aiController.js` e rotas (POST /chat, GET /history) | `backend/src/controllers/`, `routes/` | ALTA |
| 16.5 | Criar `AIChat.js` (interface de chat com markdown rendering) | `frontend/src/components/AI/` | ALTA |
| 16.6 | Loading states e streaming de resposta (SSE ou chunked) | `AIChat.js` | MEDIA |

#### Relatorios
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 16.7 | Criar `reportsService.js` (queries agregadas): | `backend/src/services/reportsService.js` | ALTA |
|      | - P&L por periodo (dia, semana, mes) | | |
|      | - P&L por moeda | | |
|      | - P&L por estrategia/automacao | | |
|      | - Total de ordens executadas | | |
|      | - Win rate (% de trades lucrativos) | | |
| 16.8 | Criar `reportsController.js` e rotas | `backend/src/controllers/`, `routes/` | ALTA |
| 16.9 | Criar `ReportsPage.js` (dashboard de relatorios) | `frontend/src/components/Reports/` | ALTA |
| 16.10 | Graficos de evolucao: Chart.js ou Recharts (linha, barra, pizza) | `Reports/ProfitChart.js` | ALTA |
| 16.11 | Tabela de historico com exportacao CSV | `Reports/OrdersHistory.js` | MEDIA |

#### Deploy Producao
| # | Tarefa | Arquivo(s) | Prioridade |
|---|--------|------------|------------|
| 16.12 | Criar `docker-compose.prod.yml` (producao, sem volumes dev) | `docker-compose.prod.yml` | CRITICA |
| 16.13 | Configurar Nginx como reverse proxy | `nginx/default.conf` | CRITICA |
| 16.14 | Setup SSL/TLS com Let's Encrypt (Certbot) | `nginx/` | CRITICA |
| 16.15 | Configurar PM2 (ecosystem.config.js) para process management | `backend/ecosystem.config.js` | ALTA |
| 16.16 | Script de deploy automatizado | `scripts/deploy.sh` | ALTA |
| 16.17 | Configurar firewall (UFW: 22, 80, 443) | Servidor | ALTA |
| 16.18 | Script de backup do banco (mysqldump + cron) | `scripts/backup.sh` | ALTA |
| 16.19 | Criar `README.md` do projeto com instrucoes de setup | `README.md` | MEDIA |
| 16.20 | GitHub Actions CI/CD (lint, test, build, deploy) | `.github/workflows/ci.yml` | MEDIA |

#### Entregavel Sprint 16:
> Sistema completo em producao: agente IA respondendo sobre mercado com contexto real, relatorios mostrando P&L e performance, deploy na Digital Ocean com HTTPS, PM2 e backups automaticos.

---

## Resumo de Dependencias entre Sprints

```
Sprint 1 (Setup)
    |
    v
Sprint 2 (Auth + DB)
    |
    +---------------------+
    |                     |
    v                     v
Sprint 3 (Exchange)   Sprint 2.frontend (Login UI)
    |
    v
Sprint 4 (WS Streams)
    |
    v
Sprint 5 (WS Server)
    |
    v
Sprint 6 (Dashboard UI)
    |
    v
Sprint 7 (Orders Backend)
    |
    v
Sprint 8 (Orders UI + User Data)
    |
    v
Sprint 9 (Order Templates)
    |
    v
Sprint 10 (Monitor Manager)
    |
    v
Sprint 11 (Indicadores)
    |
    v
Sprint 12 (Padroes + Indexes UI)
    |
    v
Sprint 13 (Motor Automacoes) <<< CORE >>>
    |
    +-----------+-----------+
    |           |           |
    v           v           v
Sprint 14   Sprint 14   Sprint 14
(Grid)    (Trailing)    (Cron)
    |           |           |
    +-----------+-----------+
                |
                v
Sprint 15 (Alertas + Automations UI)
                |
                v
Sprint 16 (IA + Reports + Deploy)
```

---

## Checklist de Validacao por Fase

### Fase 1 - Fundacao (Sprints 1-2)
- [ ] `docker-compose up` sobe todo o ambiente
- [ ] Login retorna JWT valido
- [ ] Rotas protegidas rejeitam sem token
- [ ] API keys salvas com criptografia AES-256
- [ ] Symbols sincronizados da Binance
- [ ] Todas as tabelas criadas via migration

### Fase 2 - Dashboard (Sprints 3-6)
- [ ] Saldo da carteira retornado corretamente
- [ ] Mini Ticker atualizando em < 1 segundo
- [ ] Grafico TradingView renderizando candles real-time
- [ ] Book de ofertas atualizando bid/ask
- [ ] Frontend conectado via WebSocket
- [ ] Troca de symbol atualiza todos os componentes

### Fase 3 - Ordens (Sprints 7-9)
- [ ] 6 tipos de ordens posicionados com sucesso na Testnet
- [ ] Validacao rejeita ordens invalidas (qty, price, saldo)
- [ ] Status da ordem atualiza automaticamente via stream
- [ ] Cancelamento funciona corretamente
- [ ] Templates salvos com expressoes dinamicas
- [ ] Quantidade `WALLET_QTY * 0.5` calcula corretamente

### Fase 4 - Monitores (Sprints 10-12)
- [ ] Monitor CRUD funciona (criar, editar, deletar, start, stop)
- [ ] RSI calcula corretamente (comparar com TradingView)
- [ ] MACD calcula corretamente
- [ ] Bollinger Bands calcula corretamente
- [ ] Padroes de candle detectados corretamente
- [ ] Indexes exibidos no frontend em real-time
- [ ] BRAIN armazena 60+ indicadores por symbol

### Fase 5 - Automacoes (Sprints 13-15)
- [ ] Condicao `RSI < 30` dispara compra automatica
- [ ] Cruzamento `EMA9 > EMA21` detectado corretamente
- [ ] Grid trading posiciona ordens em todos os niveis
- [ ] Trailing stop move-se conforme preco sobe
- [ ] Alerta por email chega na caixa de entrada
- [ ] Alerta por Telegram chega no chat
- [ ] Conditions Builder gera expressao valida
- [ ] Wildcard aplica automacao em multiplos pares

### Fase 6 - Finalizacao (Sprint 16)
- [ ] Agente IA responde com contexto do mercado atual
- [ ] Relatorio de P&L mostra dados corretos
- [ ] Export CSV funciona
- [ ] Deploy em VPS rodando com HTTPS
- [ ] PM2 reinicia app apos crash
- [ ] Backup automatico do banco configurado
- [ ] CI/CD pipeline funcionando

---

## Riscos e Mitigacoes

| Risco | Impacto | Probabilidade | Mitigacao |
|-------|---------|---------------|-----------|
| Rate limit da Binance excedido | Bot bloqueado temporariamente | MEDIA | Queue interna + cache + respeitar weight das chamadas |
| Perda de conexao WebSocket | Dados desatualizados, ordens perdidas | ALTA | Reconnect automatico + heartbeat + recovery de estado |
| Erro de calculo em indicador | Decisao errada do bot, prejuizo | MEDIA | Validar contra TradingView + testes unitarios robustos |
| Eval de condicoes inseguro | Injecao de codigo, seguranca | ALTA | Usar vm2 sandbox ou parser AST, NUNCA eval() direto |
| Ordem duplicada por retry | Compra/venda duplicada, prejuizo | MEDIA | `clientOrderId` unico + verificacao pre-ordem |
| API keys vazadas | Acesso indevido a conta Binance | ALTA | AES-256 + env vars + nunca logar em plain text |
| VPS fora do ar | Bot para de operar | BAIXA | PM2 auto-restart + monitoramento + alertas |
| Banco de dados corrompido | Perda de dados/configuracoes | BAIXA | Backup automatico diario + replicacao |

---

## Metricas de Sucesso do Projeto

| Metrica | Alvo |
|---------|------|
| Tempo de atualizacao do ticker | < 500ms |
| Tempo de posicionamento de ordem | < 2s (fim-a-fim) |
| Uptime do bot em producao | > 99.5% |
| Indicadores calculados por segundo | > 100 |
| Consumo de memoria (100 moedas) | < 512MB |
| Numero de indicadores suportados | >= 60 |
| Tipos de ordens suportados | 6 |
| Alertas entregues com sucesso | > 99% |

---

*Plano gerado em: Fevereiro 2026*
*Referencia: [BEHOLDER_3_DISCOVERY.md](./BEHOLDER_3_DISCOVERY.md)*
