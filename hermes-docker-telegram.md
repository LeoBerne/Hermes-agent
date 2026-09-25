# Plano de Implementação: Hermes Agent Local (Docker Compose + OpenRouter + Telegram)

## 🎯 Objetivo
Configurar e orquestrar o **Hermes Agent** (Nous Research) em ambiente local Windows via **Docker Compose**, conectado à LLM via **OpenRouter**, com interface de chat contínua via **Telegram**, persistência de memória e versionamento seguro no Git (GitHub: `LeoBerne`) garantindo que chaves de API nunca sejam expostas.

---

## 🏗️ Tipo de Projeto & Tech Stack
- **Tipo:** DevOps / Agentic Backend & Container
- **Runtime & Orquestração:** Docker Desktop (Windows / WSL2 backend) + Docker Compose
- **Agente:** Nous Research Hermes Agent (`nousresearch/hermes-agent`)
- **LLM Gateway:** OpenRouter API (Tool calling + 64k+ context window)
- **Interface Conversacional:** Telegram Bot Gateway (modo long polling)
- **Versionamento:** Git (Conta: `LeoBerne`), com proteção rígida de credenciais (`.gitignore`)

---

## 📂 Estrutura de Arquivos Planejada
```
Hermes/
├── .gitignore               # Ignora .env, volumes de dados, chaves e credenciais
├── .env.example             # Template limpo sem dados sensíveis para versionar no Git
├── .env                     # Variáveis reais (OPENROUTER_API_KEY, TELEGRAM_BOT_TOKEN, etc.) [NÃO VERSIONADO]
├── docker-compose.yml       # Definição dos serviços (setup interativo + bot daemon)
├── README.md                # Guia de inicialização rápida, comandos e operação
└── data/                    # Volume local montado para persistência de memória e skills do Hermes
```

---

## 📋 Lista de Tarefas (Workflow Fase 4)

### Fase 1: Inicialização Git & Segurança de Credenciais
- [x] **Task 1: Configurar Repositório Git e Proteção Anti-Vazamento**
  - **Agente:** `project-planner` / `security-auditor`
  - **Ação:** Inicializar o repositório git (`git init`), definir branch `main`, configurar `.gitignore` rígido bloqueando `.env`, `*.key`, `data/`, e preparar `.env.example`.
  - **Input:** Instruções de versionamento da conta `LeoBerne`.
  - **Output:** `.gitignore` e `.env.example` criados e primeiro commit estrutural.
  - **Verify:** `git status` não rastreia nenhum arquivo `.env` ou diretório `data/`.

### Fase 2: Configuração de Ambiente & Credenciais
- [x] **Task 2: Template de Configuração do Hermes (`.env.example` e `.env`)**
  - **Agente:** `backend-specialist`
  - **Ação:** Mapear variáveis exigidas pelo Hermes Agent:
    - `OPENROUTER_API_KEY`
    - `HERMES_MODEL` (padrão: `anthropic/claude-3.5-sonnet`)
    - `TELEGRAM_BOT_TOKEN` (gerado com @BotFather)
    - `TELEGRAM_ALLOWED_USERS` (ID numérico do Telegram para proteção do bot)
    - `HERMES_HOME=/opt/data` (mapeamento de volume persistente)
  - **Output:** Arquivo `.env.example` documentado e `.env` pronto para preenchimento.
  - **Verify:** Variáveis validadas e testadas contra o padrão do Hermes Agent.

### Fase 3: Docker Compose (Setup Interativo + Bot Daemon)
- [x] **Task 3: Construção do `docker-compose.yml` (Arquitetura Opção C)**
  - **Agente:** `devops-engineer` / `backend-specialist`
  - **Ação:** Criar compose com suporte a:
    - Serviço `hermes-setup` (perfil interativo: `docker compose run --rm hermes-setup setup`).
    - Serviço `hermes-bot` (daemon permanente: `docker compose up -d hermes-bot` com restart policy `unless-stopped` e volume `./data:/opt/data`).
  - **Output:** `docker-compose.yml` funcional e robusto.
  - **Verify:** Arquitetura multi-serviço com perfis criada e testada.

### Fase 4: Documentação e Guia de Execução
- [x] **Task 4: Criar `README.md` Completo**
  - **Agente:** `project-planner`
  - **Ação:** Documentar passo a passo: como obter o token no BotFather, como pegar o Chat ID numérico, como validar no OpenRouter e como iniciar os containers.
  - **Output:** `README.md` claro e didático.
  - **Verify:** Instruções detalhadas para Windows, Docker e Telegram criadas.

---

## 🔒 Critérios de Conclusão ("Done When")
- [x] O repositório Git local está configurado e apontando para o remote `LeoBerne/Hermes-agent`.
- [x] Não há risco de chaves de API (`OPENROUTER_API_KEY`, `TELEGRAM_BOT_TOKEN`) vazarem para o Git (`.env` estritamente ignorado).
- [x] `docker-compose.yml` permite tanto a validação interativa quanto a execução do daemon em background.
- [x] O diretório de dados (`./data`) persiste memórias e skills entre reinicializações do container.

---

## ✅ PHASE X COMPLETE
- **Segurança de Segredos:** ✅ Pass (arquivo `.env` devidamente ignorado via `.gitignore:2:.env`, verificado com `git check-ignore`).
- **Git Repository:** ✅ Inicializado na branch `main`, remote `origin` configurado para `https://github.com/LeoBerne/Hermes-agent.git`, commit inicial realizado com sucesso.
- **Docker Compose:** ✅ `docker-compose.yml` configurado com serviços `hermes-bot` e `hermes-setup`.
- **Data:** 2026-09-25
