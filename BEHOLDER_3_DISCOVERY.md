# Beholder 3.0 - Discovery Document

## 1. Visao Geral do Projeto

O **Beholder 3.0** e uma plataforma de trading bot multi-estrategias e multi-moedas em tempo real, integrada com a **Binance Spot**. O sistema monitora indicadores de mercado em real-time, executa estrategias automatizadas de compra/venda 24/7 e oferece um dashboard completo para acompanhamento.

### 1.1. Problema que Resolve

- Monitoramento manual de mercado cripto e humanamente impossivel 24/7
- Execucao de estrategias requer velocidade e disciplina emocional
- Calculos de indicadores tecnicos em tempo real para multiplas moedas simultaneamente
- Necessidade de posicionamento rapido em oportunidades de mercado

### 1.2. Proposta de Valor

- Bot autonomo monitorando centenas de pares de moedas simultaneamente
- +60 indicadores tecnicos e padroes graficos calculados em real-time
- Estrategias visuais modeladas pelo usuario sem necessidade de codigo
- Seguranca robusta com criptografia ponta a ponta
- Agente de IA (OpenAI) como consultor de mercado

---

## 2. Arquitetura Tecnica

### 2.1. Visao Macro da Arquitetura

```
+------------------+     +-----------------+     +------------------+
|                  |     |                 |     |                  |
|  Frontend (SPA)  |<--->|  Backend (API)  |<--->|  Binance APIs    |
|  React + BS5     |     |  Express MVC    |     |  REST + WS       |
|                  |     |                 |     |                  |
+------------------+     +--------+--------+     +------------------+
                                  |
                    +-------------+-------------+
                    |             |              |
              +-----+----+ +-----+-----+ +-----+-----+
              |          | |           | |           |
              | Database | | WebSocket | |  Services |
              | SQL ORM  | | Server    | |  (Email,  |
              |          | |           | |  Telegram,|
              +----------+ +-----------+ |  OpenAI)  |
                                         +-----------+
```

### 2.2. Padrao Arquitetural

- **Backend**: MVC RESTful com Express.js
- **Frontend**: SPA reativa com React.js
- **Comunicacao**: REST API + WebSocket bidirecional
- **Dados**: Repository Pattern + ORM Sequelize (multi-banco)
- **Seguranca**: JWT + CORS + Bcrypt + SSL/TLS + HMAC/SHA256

### 2.3. Fluxo de Dados em Tempo Real

```
Binance WS Streams
       |
       v
+------------------+
| Exchange Monitor |  <-- Recebe Book, Ticker, Klines, User Data
+--------+---------+
         |
         v
+------------------+
| Calculo de       |  <-- RSI, MACD, Bollinger, Padroes de Candles...
| Indicadores (60+)|
+--------+---------+
         |
         v
+------------------+
| Motor de         |  <-- Avalia condicoes das automacoes
| Automacoes       |
+--------+---------+
         |
    +----+----+
    |         |
    v         v
+-------+ +--------+
| Ordem | | Alerta |  <-- Email, Telegram, Push Notification
+-------+ +--------+
```

---

## 3. Stack Tecnologica

### 3.1. Backend

| Tecnologia | Finalidade | Versao Sugerida |
|---|---|---|
| **Node.js** | Runtime JavaScript server-side | >= 20 LTS |
| **Express.js** | Framework web MVC RESTful | ^4.18 |
| **Sequelize** | ORM SQL multi-banco | ^6.35 |
| **jsonwebtoken** | Autenticacao JWT | ^9.0 |
| **bcryptjs** | Hash de senhas | ^2.4 |
| **ws** | WebSocket server | ^8.16 |
| **cors** | Cross-Origin Resource Sharing | ^2.8 |
| **dotenv** | Variaveis de ambiente | ^16.3 |
| **helmet** | Security headers HTTP | ^7.1 |
| **node-binance-api** | SDK Binance (REST + WS) | ^0.13 |
| **technicalindicators** | Biblioteca de indicadores tecnicos | ^3.1 |
| **nodemailer** | Envio de emails (alertas) | ^6.9 |
| **node-telegram-bot-api** | Integracao Telegram | ^0.64 |
| **openai** | SDK OpenAI (agente IA) | ^4.x |

### 3.2. Frontend

| Tecnologia | Finalidade | Versao Sugerida |
|---|---|---|
| **React.js** | Biblioteca UI SPA | ^18.2 |
| **React Router DOM** | Roteamento SPA | ^6.x |
| **Bootstrap 5** | Framework CSS responsivo | ^5.3 |
| **React Bootstrap** | Componentes BS5 para React | ^2.10 |
| **Axios** | Cliente HTTP | ^1.6 |
| **TradingView Widget** | Graficos de mercado | Lightweight Charts |
| **React Toastify** | Notificacoes na UI | ^9.x |
| **Socket.io-client** | WebSocket client | ^4.7 |

### 3.3. Banco de Dados

| Banco | Uso |
|---|---|
| **MySQL / MariaDB** | Principal (producao) |
| **PostgreSQL** | Alternativa suportada |
| **SQLite** | Desenvolvimento local |

### 3.4. Infraestrutura / DevOps

| Ferramenta | Finalidade |
|---|---|
| **Docker + Docker Compose** | Containerizacao e dev environment |
| **Nginx** | Reverse proxy + SSL termination |
| **PM2** | Process manager Node.js em producao |
| **Let's Encrypt** | Certificados SSL gratuitos |
| **Digital Ocean** | Cloud hosting (VPS) |
| **GitHub Actions** | CI/CD pipeline |

---

## 4. Estrutura de Pastas Proposta

```
beholder-3/
|
|-- backend/
|   |-- src/
|   |   |-- config/
|   |   |   |-- database.js          # Config Sequelize
|   |   |   |-- binance.js           # Config API Binance
|   |   |   |-- auth.js              # Config JWT/Bcrypt
|   |   |   |-- telegram.js          # Config Telegram Bot
|   |   |   |-- email.js             # Config SMTP/Nodemailer
|   |   |   |-- openai.js            # Config OpenAI
|   |   |
|   |   |-- models/
|   |   |   |-- index.js             # Sequelize init + associations
|   |   |   |-- userModel.js         # Usuario do sistema
|   |   |   |-- settingsModel.js     # Configuracoes globais
|   |   |   |-- symbolModel.js       # Pares de moedas (symbols)
|   |   |   |-- orderModel.js        # Ordens (6 tipos)
|   |   |   |-- monitorModel.js      # Monitores de streams
|   |   |   |-- automationModel.js   # Automacoes/Estrategias
|   |   |   |-- gridModel.js         # Configuracoes de grid
|   |   |   |-- orderTemplateModel.js# Templates de ordens
|   |   |   |-- actionModel.js       # Acoes das automacoes
|   |   |   |-- logModel.js          # Logs de auditoria
|   |   |   |-- walletModel.js       # Carteira do usuario
|   |   |
|   |   |-- repositories/
|   |   |   |-- usersRepository.js
|   |   |   |-- settingsRepository.js
|   |   |   |-- symbolsRepository.js
|   |   |   |-- ordersRepository.js
|   |   |   |-- monitorsRepository.js
|   |   |   |-- automationsRepository.js
|   |   |   |-- orderTemplatesRepository.js
|   |   |   |-- logsRepository.js
|   |   |
|   |   |-- controllers/
|   |   |   |-- authController.js       # Login, Logout, Token refresh
|   |   |   |-- settingsController.js   # CRUD configuracoes
|   |   |   |-- symbolsController.js    # CRUD pares de moedas
|   |   |   |-- ordersController.js     # CRUD + posicionamento ordens
|   |   |   |-- monitorsController.js   # CRUD monitores
|   |   |   |-- automationsController.js# CRUD automacoes
|   |   |   |-- orderTemplatesController.js
|   |   |   |-- dashboardController.js  # Dados do dashboard
|   |   |   |-- walletController.js     # Dados da carteira
|   |   |   |-- reportsController.js    # Relatorios
|   |   |   |-- aiController.js         # Agente IA (OpenAI)
|   |   |
|   |   |-- middlewares/
|   |   |   |-- authMiddleware.js     # Verificacao JWT
|   |   |   |-- errorMiddleware.js    # Error handler global
|   |   |   |-- rateLimitMiddleware.js# Rate limiting
|   |   |   |-- profileMiddleware.js  # Verificacao de perfil
|   |   |
|   |   |-- routes/
|   |   |   |-- authRoutes.js
|   |   |   |-- settingsRoutes.js
|   |   |   |-- symbolsRoutes.js
|   |   |   |-- ordersRoutes.js
|   |   |   |-- monitorsRoutes.js
|   |   |   |-- automationsRoutes.js
|   |   |   |-- orderTemplatesRoutes.js
|   |   |   |-- dashboardRoutes.js
|   |   |   |-- walletRoutes.js
|   |   |   |-- reportsRoutes.js
|   |   |   |-- aiRoutes.js
|   |   |
|   |   |-- services/
|   |   |   |-- exchangeService.js       # Abstrai chamadas Binance REST
|   |   |   |-- exchangeMonitor.js       # Gerencia streams WebSocket
|   |   |   |-- automationsService.js    # Motor de logica/automacoes
|   |   |   |-- indicatorsService.js     # Calculo dos 60+ indicadores
|   |   |   |-- patternsService.js       # Deteccao de padroes graficos
|   |   |   |-- ordersService.js         # Logica de ordens
|   |   |   |-- gridService.js           # Logica de grid trading
|   |   |   |-- trailingStopService.js   # Logica de trailing stop
|   |   |   |-- alertsService.js         # Email + Telegram + Push
|   |   |   |-- aiService.js             # Integracao OpenAI
|   |   |   |-- webSocketServer.js       # WS Server para o frontend
|   |   |   |-- walletService.js         # Logica de carteira
|   |   |   |-- reportsService.js        # Geracao de relatorios
|   |   |
|   |   |-- utils/
|   |   |   |-- crypto.js              # Encrypt/Decrypt keys
|   |   |   |-- indexes.js             # Logica de indexacao de dados
|   |   |   |-- calcUtils.js           # Utilidades de calculo
|   |   |   |-- constants.js           # Constantes do sistema
|   |   |
|   |   |-- app.js                     # Express app setup
|   |   |-- server.js                  # Entry point (HTTP + WS)
|   |
|   |-- migrations/                    # Sequelize migrations
|   |-- seeders/                       # Dados iniciais
|   |-- tests/                         # Testes unitarios/integracao
|   |-- package.json
|   |-- .env.example
|   |-- .sequelizerc
|
|-- frontend/
|   |-- public/
|   |-- src/
|   |   |-- components/
|   |   |   |-- Dashboard/
|   |   |   |   |-- Dashboard.js
|   |   |   |   |-- MiniTicker.js      # Ticker resumido moedas
|   |   |   |   |-- BookTicker.js      # Book de ofertas
|   |   |   |   |-- CandleChart.js     # Grafico TradingView
|   |   |   |   |-- Wallet.js          # Visao da carteira
|   |   |   |
|   |   |   |-- Orders/
|   |   |   |   |-- OrdersPage.js
|   |   |   |   |-- NewOrderModal.js
|   |   |   |   |-- OrderRow.js
|   |   |   |   |-- SelectSymbol.js
|   |   |   |
|   |   |   |-- Monitors/
|   |   |   |   |-- MonitorsPage.js
|   |   |   |   |-- MonitorModal.js
|   |   |   |   |-- MonitorRow.js
|   |   |   |   |-- IndexesPanel.js
|   |   |   |
|   |   |   |-- Automations/
|   |   |   |   |-- AutomationsPage.js
|   |   |   |   |-- AutomationModal.js
|   |   |   |   |-- ConditionsBuilder.js  # Builder visual
|   |   |   |   |-- ActionsBuilder.js
|   |   |   |   |-- GridModal.js
|   |   |   |   |-- TrailingStopConfig.js
|   |   |   |
|   |   |   |-- OrderTemplates/
|   |   |   |   |-- TemplatesPage.js
|   |   |   |   |-- TemplateModal.js
|   |   |   |
|   |   |   |-- Settings/
|   |   |   |   |-- SettingsPage.js
|   |   |   |   |-- SymbolsSettings.js
|   |   |   |   |-- AlertsSettings.js
|   |   |   |
|   |   |   |-- Reports/
|   |   |   |   |-- ReportsPage.js
|   |   |   |   |-- ProfitChart.js
|   |   |   |   |-- OrdersHistory.js
|   |   |   |
|   |   |   |-- AI/
|   |   |   |   |-- AIChat.js           # Chat com agente IA
|   |   |   |   |-- StrategyAdvisor.js   # Sugestoes de estrategia
|   |   |   |
|   |   |   |-- common/
|   |   |       |-- Header.js
|   |   |       |-- Sidebar.js
|   |   |       |-- Footer.js
|   |   |       |-- Toast.js
|   |   |       |-- Pagination.js
|   |   |       |-- Loading.js
|   |   |
|   |   |-- services/
|   |   |   |-- api.js                 # Axios instance + interceptors
|   |   |   |-- authService.js
|   |   |   |-- ordersService.js
|   |   |   |-- monitorsService.js
|   |   |   |-- automationsService.js
|   |   |   |-- dashboardService.js
|   |   |   |-- settingsService.js
|   |   |   |-- webSocketClient.js     # WS connection manager
|   |   |
|   |   |-- hooks/
|   |   |   |-- useWebSocket.js
|   |   |   |-- useAuth.js
|   |   |   |-- useTicker.js
|   |   |
|   |   |-- context/
|   |   |   |-- AuthContext.js
|   |   |   |-- TickerContext.js
|   |   |
|   |   |-- App.js
|   |   |-- index.js
|   |   |-- Routes.js
|   |
|   |-- package.json
|
|-- docker-compose.yml
|-- docker-compose.prod.yml
|-- nginx/
|   |-- default.conf
|-- .github/
|   |-- workflows/
|       |-- ci.yml
|-- README.md
```

---

## 5. Modelagem de Dados (Entidades Principais)

### 5.1. Diagrama ER Simplificado

```
+----------+     +-----------+     +---------+
|  Users   |---->| Settings  |     | Symbols |
+----------+     +-----------+     +----+----+
     |                                   |
     |           +------------+          |
     +---------->|   Orders   |<---------+
     |           +-----+------+
     |                 |
     |           +-----+------+
     |           | OrderTempl |
     |           +-----+------+
     |                 |
     |          +------+-------+
     +--------->| Automations  |
     |          +------+-------+
     |                 |
     |          +------+-------+
     |          |   Actions    |
     |          +--------------+
     |
     |          +--------------+
     +--------->|   Monitors   |
     |          +--------------+
     |
     |          +--------------+
     +--------->|    Logs      |
     |          +--------------+
     |
     |          +--------------+
     +--------->|   Wallet     |
                +--------------+
```

### 5.2. Descricao das Entidades

#### Users
```
- id            : INTEGER, PK, AUTO_INCREMENT
- name          : STRING(100), NOT NULL
- email         : STRING(150), UNIQUE, NOT NULL
- password      : STRING(255), NOT NULL (Bcrypt hash)
- apiUrl        : STRING(255)         # Binance API URL
- apiKey        : STRING(255)         # Encrypted (AES-256)
- secretKey     : STRING(255)         # Encrypted (AES-256)
- streamUrl     : STRING(255)         # Binance WS URL
- accessToken   : STRING(500)         # JWT atual
- pushToken     : STRING(500)         # Token push notification
- telegramChat  : STRING(100)         # Chat ID Telegram
- limitApiCalls : INTEGER, DEFAULT 10 # Rate limit pessoal
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Settings
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- email         : STRING(150)         # Email para alertas
- phone         : STRING(20)
- telegramBot   : STRING(255)         # Token do bot Telegram
- telegramChat  : STRING(100)         # Chat ID
- sendGridKey   : STRING(255)         # API key email service
- openAiKey     : STRING(255)         # API key OpenAI
- alertsEnabled : BOOLEAN, DEFAULT true
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Symbols (Pares de Moedas)
```
- symbol        : STRING(20), PK      # Ex: BTCUSDT
- basePrecision : INTEGER              # Casas decimais base
- quotePrecision: INTEGER              # Casas decimais cotacao
- minNotional   : DECIMAL(18,8)        # Valor minimo da ordem
- minLotSize    : DECIMAL(18,8)        # Quantidade minima
- stepSize      : DECIMAL(18,8)        # Incremento de quantidade
- tickSize      : DECIMAL(18,8)        # Incremento de preco
- isFavorite    : BOOLEAN, DEFAULT false
- base          : STRING(10)           # Ex: BTC
- quote         : STRING(10)           # Ex: USDT
- status        : STRING(20)           # TRADING, HALT, etc.
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Orders
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- automationId  : INTEGER, FK -> Automations (nullable)
- symbol        : STRING(20), FK -> Symbols
- orderId       : BIGINT               # ID retornado pela Binance
- clientOrderId : STRING(50)           # ID customizado
- transactTime  : BIGINT               # Timestamp Binance
- type          : ENUM('LIMIT','MARKET','STOP_LOSS',
                       'STOP_LOSS_LIMIT','TAKE_PROFIT',
                       'TAKE_PROFIT_LIMIT')
- side          : ENUM('BUY','SELL')
- status        : STRING(20)           # NEW, FILLED, CANCELED...
- quantity      : DECIMAL(18,8)
- limitPrice    : DECIMAL(18,8)        # Preco limite
- stopPrice     : DECIMAL(18,8)        # Preco de stop
- icebergQty    : DECIMAL(18,8)        # Ordem iceberg
- avgPrice      : DECIMAL(18,8)        # Preco medio executado
- commission    : DECIMAL(18,8)        # Taxa paga
- net           : DECIMAL(18,8)        # Valor liquido
- isMaker       : BOOLEAN
- obs           : STRING(255)          # Observacoes
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Monitors
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- symbol        : STRING(20)           # '*' para todos
- type          : ENUM('MINI_TICKER','BOOK','CANDLES',
                       'USER_DATA','TICKER')
- interval      : STRING(5)            # 1m, 5m, 15m, 1h, 1d...
- broadcastLabel: STRING(50)           # Label para broadcast
- indexes       : STRING(500)          # Indicadores configurados
- isActive      : BOOLEAN, DEFAULT true
- isSystemMon   : BOOLEAN, DEFAULT false
- logs          : BOOLEAN, DEFAULT false
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Automations
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- name          : STRING(100), NOT NULL
- symbol        : STRING(20)           # '*' para wildcard
- conditions    : TEXT                  # Expressao logica (JSON)
- indexes       : STRING(500)          # Indexes usados
- schedule      : STRING(50)           # Cron expression (agendamento)
- isActive      : BOOLEAN, DEFAULT true
- logs          : BOOLEAN, DEFAULT false
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Actions
```
- id            : INTEGER, PK, AUTO_INCREMENT
- automationId  : INTEGER, FK -> Automations
- orderTemplateId: INTEGER, FK -> OrderTemplates (nullable)
- type          : ENUM('ORDER','ALERT_EMAIL','ALERT_TELEGRAM',
                       'ALERT_PUSH','WITHDRAW')
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### OrderTemplates
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- name          : STRING(100), NOT NULL
- symbol        : STRING(20)
- type          : ENUM('LIMIT','MARKET','STOP_LOSS',
                       'STOP_LOSS_LIMIT','TAKE_PROFIT',
                       'TAKE_PROFIT_LIMIT')
- side          : ENUM('BUY','SELL')
- quantity      : STRING(50)           # Pode ser expressao dinamica
- quantityMultiplier: DECIMAL(10,4)
- limitPrice    : STRING(50)           # Pode ser expressao dinamica
- limitPriceMultiplier: DECIMAL(10,4)
- stopPrice     : STRING(50)
- stopPriceMultiplier: DECIMAL(10,4)
- icebergQty    : DECIMAL(18,8)
- createdAt     : DATETIME
- updatedAt     : DATETIME
```

#### Logs
```
- id            : INTEGER, PK, AUTO_INCREMENT
- userId        : INTEGER, FK -> Users
- automationId  : INTEGER, FK -> Automations (nullable)
- orderId       : INTEGER, FK -> Orders (nullable)
- text          : TEXT
- level         : ENUM('INFO','WARNING','ERROR','DEBUG')
- createdAt     : DATETIME
```

---

## 6. Detalhamento dos Modulos

### MODULO 1 - Introducao e Setup

**Objetivo**: Configurar todo o ambiente de desenvolvimento e a base do projeto.

#### Tarefas de Desenvolvimento:
1. **Inicializacao do projeto**
   - Setup monorepo (backend + frontend)
   - Configuracao Docker Compose (Node.js + MySQL)
   - Estrutura de pastas MVC
   - Variaveis de ambiente (.env)

2. **Setup do Banco de Dados**
   - Configuracao Sequelize (connection, dialects)
   - Criacao das migrations (todas as tabelas)
   - Seeders para dados iniciais (usuario admin, symbols)
   - Indices de performance

3. **Autenticacao e Seguranca**
   - Registro e login com Bcrypt
   - Middleware JWT (access + refresh token)
   - CORS configuravel
   - Helmet para headers de seguranca
   - Criptografia AES-256 para API keys da Binance

4. **Configuracoes do Usuario**
   - CRUD de settings
   - Validacao e armazenamento seguro das API keys
   - Configuracao dos pares de moedas favoritos
   - Sync inicial de symbols com a Binance

#### Estimativa de Complexidade: MEDIA

---

### MODULO 2 - Dashboard

**Objetivo**: Dashboard real-time integrado com TradingView e streams da Binance.

#### Tarefas de Desenvolvimento:

1. **Exchange Service (Backend)**
   - Wrapper da API REST da Binance
   - Rate limiting inteligente (respeitar 1200 req/min)
   - Retry com exponential backoff
   - Cache de dados frequentes (exchange info)

2. **WebSocket Server (Backend -> Frontend)**
   - Setup do servidor WS (ws library)
   - Autenticacao WS via token
   - Canais/rooms por usuario
   - Broadcast de dados (ticker, book, wallet)

3. **Exchange Monitor (Backend)**
   - Connection Manager para streams Binance WS
   - Mini Ticker stream (todas as moedas)
   - Book Ticker stream
   - Klines/Candlestick stream (multiplos intervalos)
   - User Data Stream (balance updates, order updates)
   - Reconnect automatico com backoff

4. **Dashboard UI (Frontend)**
   - Layout responsivo com grid Bootstrap 5
   - Integracao TradingView Lightweight Charts
   - Mini Ticker cards (top moedas + favoritas)
   - Book de ofertas (bid/ask em real-time)
   - Wallet panel (saldos disponivel/em ordens)
   - Selector de pares de moedas

5. **WebSocket Client (Frontend)**
   - Hook `useWebSocket` para gerenciar conexao
   - Context `TickerContext` para dados globais
   - Reconexao automatica
   - Atualizacao reativa dos componentes

#### Estimativa de Complexidade: ALTA

---

### MODULO 3 - Orders

**Objetivo**: Sistema completo de posicionamento dos 6 tipos de ordens.

#### Tarefas de Desenvolvimento:

1. **Orders Service (Backend)**
   - Posicionamento dos 6 tipos de ordem:
     - `MARKET` (compra/venda a mercado)
     - `LIMIT` (compra/venda com preco limite)
     - `STOP_LOSS` (venda automatica em queda)
     - `STOP_LOSS_LIMIT` (stop com preco limite)
     - `TAKE_PROFIT` (venda automatica em alta)
     - `TAKE_PROFIT_LIMIT` (take profit com limite)
   - Cancelamento de ordens
   - Sync de ordens com Binance (reconciliacao)
   - Validacao de quantidade (lotSize, minNotional)

2. **User Data Stream (Backend)**
   - Listen Key management (create, keepalive, close)
   - Processamento de `executionReport` (atualizacao de ordens)
   - Processamento de `outboundAccountPosition` (saldo)
   - Atualizacao automatica no banco e broadcast WS

3. **Orders UI (Frontend)**
   - Tabela de ordens com filtros e paginacao
   - Modal de nova ordem (todos os 6 tipos)
   - Selector de symbol com autocomplete
   - Calculadora de quantidade (% do saldo)
   - Botao de cancelamento com confirmacao
   - Status em real-time via WebSocket

4. **Order Templates (Backend + Frontend)**
   - CRUD de templates reutilizaveis
   - Suporte a quantidade dinamica (expressoes)
   - Multiplicadores de preco (ex: lastPrice * 0.98)
   - Vinculacao com automacoes

#### Estimativa de Complexidade: ALTA

---

### MODULO 4 - Monitors

**Objetivo**: Barramento de monitoria com calculo de 60+ indicadores em real-time.

#### Tarefas de Desenvolvimento:

1. **Monitor Manager (Backend)**
   - CRUD de monitores customizados
   - Ativacao/desativacao de monitores
   - Monitores de sistema (pre-configurados)
   - Gerenciamento de streams por monitor

2. **Indicators Service (Backend)**
   - Integracao com `technicalindicators` lib
   - **Indicadores de Tendencia**:
     - EMA, SMA, WEMA (Medias Moveis)
     - MACD (Moving Average Convergence Divergence)
     - ADX (Average Directional Index)
     - PSAR (Parabolic SAR)
     - Ichimoku Cloud
   - **Indicadores de Momentum**:
     - RSI (Relative Strength Index)
     - Stochastic RSI
     - Stochastic Oscillator
     - CCI (Commodity Channel Index)
     - ROC (Rate of Change)
     - Williams %R
     - Awesome Oscillator
     - KST (Know Sure Thing)
     - TRIX
   - **Indicadores de Volume**:
     - OBV (On Balance Volume)
     - ADL (Accumulation Distribution Line)
     - MFI (Money Flow Index)
     - Force Index
     - VWAP (Volume Weighted Average Price)
     - Volume Profile
   - **Indicadores de Volatilidade**:
     - Bollinger Bands
     - ATR (Average True Range)

3. **Patterns Service (Backend)**
   - Integracao com `technicalindicators` (candlestick patterns)
   - **Padroes de Reversao Altista**:
     - Hammer, Inverted Hammer
     - Bullish Engulfing, Bullish Harami
     - Morning Star, Morning Doji Star
     - Piercing Line, Abandoned Baby (Bull)
     - Three White Soldiers
     - Tweezer Bottom
   - **Padroes de Reversao Baixista**:
     - Hanging Man, Shooting Star
     - Bearish Engulfing, Bearish Harami
     - Evening Star, Evening Doji Star
     - Dark Cloud Cover, Abandoned Baby (Bear)
     - Three Black Crows
     - Tweezer Top
   - **Padroes Neutros**:
     - Doji, Dragonfly Doji, Gravestone Doji
     - Spinning Top (Bull/Bear)
     - Inside Candle
     - Marubozu (Bull/Bear)
     - Downside Tasuki Gap

4. **Indexes/Brain (Backend)**
   - Armazenamento em memoria dos indicadores calculados
   - Estrutura de dados otimizada (Map por symbol + interval)
   - Broadcast de atualizacoes para o motor de automacoes
   - Persistencia periodica para recovery

5. **Monitors UI (Frontend)**
   - Tabela de monitores com status (ativo/inativo)
   - Modal de criacao/edicao de monitor
   - Painel de indexes (valores calculados em real-time)
   - Visualizacao dos indicadores por symbol
   - Logs de monitoria

#### Estimativa de Complexidade: MUITO ALTA

---

### MODULO 5 - Automations

**Objetivo**: Motor de logica para automacoes com estrategias visuais.

#### Tarefas de Desenvolvimento:

1. **Automations Engine (Backend)** - CORE do sistema
   - Parser de condicoes (expressoes logicas)
   - Avaliador de condicoes em tempo real
   - Cruzamento de indicadores (ex: EMA9 > EMA21)
   - Suporte a operadores: `>`, `<`, `>=`, `<=`, `==`, `&&`, `||`
   - Variaveis dinamicas: `MEMORY['BTCUSDT:RSI_14']`
   - Quantidade dinamica baseada em saldo/indicador
   - Condicoes de entrada e saida (buy + sell vinculados)

2. **Grid Trading (Backend)**
   - Definicao de faixa de preco (upper/lower)
   - Calculo automatico de niveis do grid
   - Posicionamento de ordens nos niveis
   - Reposicionamento automatico apos execucao
   - Controle de lucro acumulado

3. **Trailing Stop (Backend)**
   - Monitoramento de preco em real-time
   - Calculo dinamico do stop (% ou valor fixo)
   - Atualizacao automatica conforme preco sobe
   - Ativacao do stop quando preco reverte
   - Integracao com automacoes

4. **Agendamento de Ordens (Backend)**
   - Execucao em horario programado (cron)
   - Recorrencia (DCA - Dollar Cost Averaging)
   - Saque automatico programado

5. **Wildcard Automations (Backend)**
   - Symbol `*` para aplicar a todos os pares
   - Filtro dinamico de moedas elegíveis
   - Replicacao de estrategia em multiplos pares

6. **Alertas Service (Backend)**
   - Envio de email via Nodemailer/SendGrid
   - Envio de mensagem via Telegram Bot API
   - Push notifications (web)
   - Templates de mensagem customizaveis

7. **Automations UI (Frontend)**
   - Tabela de automacoes com status
   - **Conditions Builder** (interface visual):
     - Dropdown de indicadores disponíveis
     - Operadores logicos
     - Valores dinamicos ou fixos
     - Preview da expressao
   - **Actions Builder**:
     - Selecao de order template
     - Configuracao de alertas
   - Modal de Grid trading
   - Configuracao de Trailing Stop
   - Logs de execucao por automacao

#### Estimativa de Complexidade: MUITO ALTA (modulo mais complexo)

---

### MODULO 6 - Finalizacao

**Objetivo**: Agente IA, relatorios e deploy profissional.

#### Tarefas de Desenvolvimento:

1. **Agente de IA (Backend)**
   - Integracao com OpenAI API (GPT-4)
   - System prompt especializado em cripto/trading
   - Contexto com dados do mercado atual
   - Funcionalidades:
     - Consultor de mercado (analise tecnica)
     - Sugestao de estrategias baseadas nos indicadores
     - Explicacao de padroes graficos detectados
     - Auxilio na configuracao de automacoes
   - Historico de conversas
   - Rate limiting de chamadas

2. **AI Chat UI (Frontend)**
   - Interface de chat responsiva
   - Markdown rendering nas respostas
   - Loading states
   - Historico de conversas

3. **Relatorios (Backend + Frontend)**
   - Performance por periodo (diario, semanal, mensal)
   - Historico de ordens com filtros
   - Lucro/prejuizo por moeda e por estrategia
   - Graficos de evolucao do portfolio
   - Exportacao CSV/PDF

4. **Deploy Profissional**
   - Configuracao VPS Digital Ocean (Ubuntu)
   - Docker Compose para producao
   - Nginx como reverse proxy
   - SSL/TLS com Let's Encrypt (Certbot)
   - PM2 para process management
   - Firewall (UFW) e seguranca do servidor
   - Backup automatizado do banco de dados
   - Monitoramento com logs (PM2 logs)

#### Estimativa de Complexidade: MEDIA-ALTA

---

## 7. APIs e Endpoints REST

### 7.1. Autenticacao
```
POST   /api/auth/login          # Login (retorna JWT)
POST   /api/auth/logout         # Logout (invalida token)
POST   /api/auth/refresh        # Refresh token
```

### 7.2. Settings
```
GET    /api/settings            # Obter configuracoes
PATCH  /api/settings            # Atualizar configuracoes
```

### 7.3. Symbols
```
GET    /api/symbols             # Listar symbols
GET    /api/symbols/:symbol     # Detalhes de um symbol
PATCH  /api/symbols/:symbol     # Atualizar (favorito, etc)
POST   /api/symbols/sync        # Sync com Binance
```

### 7.4. Orders
```
GET    /api/orders              # Listar ordens (paginado)
GET    /api/orders/:id          # Detalhes de uma ordem
POST   /api/orders              # Nova ordem
DELETE /api/orders/:symbol/:orderId  # Cancelar ordem
POST   /api/orders/sync/:symbol      # Sync ordens com Binance
```

### 7.5. Order Templates
```
GET    /api/ordertemplates      # Listar templates
GET    /api/ordertemplates/:id  # Detalhes template
POST   /api/ordertemplates      # Criar template
PATCH  /api/ordertemplates/:id  # Atualizar template
DELETE /api/ordertemplates/:id  # Deletar template
```

### 7.6. Monitors
```
GET    /api/monitors            # Listar monitores
GET    /api/monitors/:id        # Detalhes monitor
POST   /api/monitors            # Criar monitor
PATCH  /api/monitors/:id        # Atualizar monitor
DELETE /api/monitors/:id        # Deletar monitor
POST   /api/monitors/:id/start  # Iniciar monitor
POST   /api/monitors/:id/stop   # Parar monitor
```

### 7.7. Automations
```
GET    /api/automations         # Listar automacoes
GET    /api/automations/:id     # Detalhes automacao
POST   /api/automations         # Criar automacao
PATCH  /api/automations/:id     # Atualizar automacao
DELETE /api/automations/:id     # Deletar automacao
POST   /api/automations/:id/start  # Ativar automacao
POST   /api/automations/:id/stop   # Desativar automacao
```

### 7.8. Dashboard / Wallet
```
GET    /api/dashboard           # Dados gerais do dashboard
GET    /api/wallet              # Saldos da carteira
GET    /api/wallet/balance      # Balance atualizado da Binance
```

### 7.9. Reports
```
GET    /api/reports/orders      # Historico de ordens
GET    /api/reports/profit      # Lucro/prejuizo
GET    /api/reports/performance # Performance por estrategia
```

### 7.10. AI Agent
```
POST   /api/ai/chat             # Enviar mensagem para IA
GET    /api/ai/history          # Historico de conversas
DELETE /api/ai/history          # Limpar historico
```

---

## 8. WebSocket Events

### 8.1. Server -> Client (Backend -> Frontend)
```
balance         # Atualizacao de saldo
miniTicker      # Mini ticker de todas as moedas
bookTicker      # Book de ofertas atualizado
kline           # Candlestick atualizado
execution       # Ordem executada/atualizada
automation      # Status de automacao alterado
notification    # Alerta/notificacao geral
indexes         # Indicadores atualizados
```

### 8.2. Binance -> Backend (Exchange Monitor)
```
<symbol>@miniTicker     # Ticker resumido por moeda
!miniTicker@arr         # Ticker de TODAS as moedas
<symbol>@bookTicker     # Book de ofertas
<symbol>@kline_<int>    # Candlestick por intervalo
<listenKey>             # User Data Stream (ordens, saldo)
```

---

## 9. Indicadores Tecnicos Suportados (60+)

### Medias Moveis e Tendencia
| # | Indicador | Descricao |
|---|-----------|-----------|
| 1 | SMA | Simple Moving Average |
| 2 | EMA | Exponential Moving Average |
| 3 | WEMA | Wilder's EMA |
| 4 | MACD | Moving Average Convergence Divergence |
| 5 | ADX | Average Directional Index |
| 6 | PSAR | Parabolic SAR |
| 7 | Ichimoku | Ichimoku Cloud |

### Momentum
| # | Indicador | Descricao |
|---|-----------|-----------|
| 8 | RSI | Relative Strength Index |
| 9 | Stochastic RSI | Stochastic RSI |
| 10 | Stochastic | Stochastic Oscillator |
| 11 | CCI | Commodity Channel Index |
| 12 | ROC | Rate of Change |
| 13 | Williams %R | Williams Percent Range |
| 14 | Awesome Oscillator | Awesome Oscillator |
| 15 | KST | Know Sure Thing |
| 16 | TRIX | Triple Exponential Average |

### Volume
| # | Indicador | Descricao |
|---|-----------|-----------|
| 17 | OBV | On Balance Volume |
| 18 | ADL | Accumulation Distribution Line |
| 19 | MFI | Money Flow Index |
| 20 | Force Index | Force Index |
| 21 | VWAP | Volume Weighted Average Price |
| 22 | Volume Profile | Volume Profile |

### Volatilidade
| # | Indicador | Descricao |
|---|-----------|-----------|
| 23 | Bollinger Bands | Bollinger Bands |
| 24 | ATR | Average True Range |

### Padroes de Candlestick (36+)
| # | Padrao | Tipo |
|---|--------|------|
| 25 | Hammer | Altista |
| 26 | Inverted Hammer | Altista |
| 27 | Bullish Engulfing | Altista |
| 28 | Bullish Harami | Altista |
| 29 | Bullish Harami Cross | Altista |
| 30 | Morning Star | Altista |
| 31 | Morning Doji Star | Altista |
| 32 | Piercing Line | Altista |
| 33 | Three White Soldiers | Altista |
| 34 | Tweezer Bottom | Altista |
| 35 | Abandoned Baby (Bull) | Altista |
| 36 | Hanging Man | Baixista |
| 37 | Shooting Star | Baixista |
| 38 | Bearish Engulfing | Baixista |
| 39 | Bearish Harami | Baixista |
| 40 | Bearish Harami Cross | Baixista |
| 41 | Evening Star | Baixista |
| 42 | Evening Doji Star | Baixista |
| 43 | Dark Cloud Cover | Baixista |
| 44 | Three Black Crows | Baixista |
| 45 | Tweezer Top | Baixista |
| 46 | Abandoned Baby (Bear) | Baixista |
| 47 | Downside Tasuki Gap | Baixista |
| 48 | Doji | Neutro |
| 49 | Dragonfly Doji | Neutro |
| 50 | Gravestone Doji | Neutro |
| 51 | Spinning Top (Bull) | Neutro |
| 52 | Spinning Top (Bear) | Neutro |
| 53 | Inside Candle | Neutro |
| 54 | Marubozu (Bull) | Neutro |
| 55 | Marubozu (Bear) | Neutro |
| 56 | Bear Pinbar | Baixista |
| 57 | Bull Pinbar | Altista |

---

## 10. Seguranca

### 10.1. Autenticacao e Autorizacao
- **JWT** com access token (curta duracao ~15min) + refresh token (longa duracao ~7d)
- **Bcrypt** com salt rounds >= 12 para hash de senhas
- Middleware de autenticacao em todas as rotas protegidas

### 10.2. Protecao de Dados Sensiveis
- **AES-256-CBC** para criptografia das API keys da Binance
- Chave de criptografia via variavel de ambiente (nunca no codigo)
- API keys nunca retornadas em plain text na API

### 10.3. Seguranca da API REST
- **CORS** configurado para dominio especifico (nao wildcard `*`)
- **Helmet** para headers de seguranca (XSS, HSTS, etc.)
- **Rate limiting** para prevenir brute force
- Validacao de input em todas as rotas (express-validator)

### 10.4. Seguranca do WebSocket
- Autenticacao via token no handshake
- **HMAC/SHA256** para verificacao de integridade
- Conexao via **WSS** (WebSocket Secure) em producao

### 10.5. Seguranca de Infraestrutura
- **SSL/TLS** via Let's Encrypt (Nginx termination)
- Firewall (UFW) com portas minimas abertas
- SSH apenas via chave publica
- Variaveis de ambiente para todas as credenciais

---

## 11. Plano de Desenvolvimento (Roadmap)

### Fase 1 - Fundacao (Modulo 1)
```
Semana 1-2:
[ ] Setup do projeto (monorepo, Docker, configs)
[ ] Modelagem do banco (migrations, seeders, models)
[ ] Sistema de autenticacao (JWT + Bcrypt)
[ ] CRUD Settings e Symbols
[ ] Exchange Service (wrapper Binance REST)
[ ] Testes unitarios da camada de dados
```

### Fase 2 - Dashboard (Modulo 2)
```
Semana 3-4:
[ ] Exchange Monitor (streams WebSocket Binance)
[ ] WebSocket Server (backend -> frontend)
[ ] Mini Ticker + Book Ticker streams
[ ] User Data Stream (balance, orders)
[ ] Dashboard UI (layout, TradingView, ticker cards)
[ ] WebSocket Client + hooks React
[ ] Wallet panel real-time
```

### Fase 3 - Orders (Modulo 3)
```
Semana 5-6:
[ ] Orders Service (6 tipos de ordens)
[ ] Validacao de ordens (lotSize, minNotional, precision)
[ ] Sync de ordens com Binance
[ ] User Data Stream processing (executionReport)
[ ] Orders UI (tabela, modal, filtros)
[ ] Order Templates (CRUD + expressoes dinamicas)
```

### Fase 4 - Monitors (Modulo 4)
```
Semana 7-9:
[ ] Monitor Manager (CRUD + lifecycle)
[ ] Klines/Candlestick stream processing
[ ] Indicators Service (24 indicadores tecnicos)
[ ] Patterns Service (33+ padroes de candles)
[ ] Brain/Indexes (armazenamento em memoria)
[ ] Monitors UI (tabela, painel de indexes)
[ ] Testes dos calculos de indicadores
```

### Fase 5 - Automations (Modulo 5) -- CORE
```
Semana 10-13:
[ ] Automations Engine (parser + avaliador de condicoes)
[ ] Cruzamento de indicadores
[ ] Quantidade dinamica e multiplicadores
[ ] Grid Trading service
[ ] Trailing Stop service
[ ] Agendamento de ordens (cron)
[ ] Wildcard automations
[ ] Alertas (Email + Telegram + Push)
[ ] Automations UI (Conditions Builder, Actions Builder)
[ ] Testes intensivos do motor de automacoes
```

### Fase 6 - Finalizacao (Modulo 6)
```
Semana 14-16:
[ ] Agente IA (OpenAI integration)
[ ] AI Chat UI
[ ] Relatorios (performance, historico, P&L)
[ ] Reports UI (graficos, exportacao)
[ ] Deploy Digital Ocean (Docker + Nginx + SSL)
[ ] Testes end-to-end
[ ] Documentacao da API
```

---

## 12. Consideracoes Finais

### 12.1. Pontos Criticos de Atencao
1. **Rate Limiting da Binance**: Respeitar 1200 requests/minuto (REST) e 5 mensagens/segundo (WS). Implementar queue e backoff.
2. **Precisao Numerica**: Usar DECIMAL no banco e bibliotecas de precisao arbitraria para calculos financeiros. Nunca usar float/double para dinheiro.
3. **Reconexao WebSocket**: Streams da Binance podem cair. Implementar reconnect automatico com backoff exponencial.
4. **Memory Management**: Com centenas de moedas e 60+ indicadores, o consumo de memoria pode ser alto. Monitorar e otimizar.
5. **Testnet First**: Sempre desenvolver e testar na Binance Testnet antes de usar dinheiro real.
6. **Idempotencia**: Ordens devem usar `clientOrderId` para evitar duplicatas em caso de retry.

### 12.2. Melhorias Futuras (Pos-MVP)
- Backtesting de estrategias com dados historicos
- Suporte a Binance Futures (margem)
- Dashboard mobile (PWA ou React Native - Pallas)
- Multi-tenant SaaS (Hydra)
- Machine Learning para predicao de padroes
- Integracao com mais exchanges (Bybit, Coinbase)

---

*Documento gerado em: Fevereiro 2026*
*Autor: Adson Patrick (Discovery assistido por Claude)*
