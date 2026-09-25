# 🏛️ Hermes Agent Local (Docker Compose + OpenRouter + Telegram)

Ambiente completo para executar o **Hermes Agent** (da Nous Research) localmente no Windows utilizando **Docker Compose**, conectado a modelos de linguagem avançados pelo **OpenRouter** e acessível em qualquer lugar pelo **Telegram**.

---

## 🚀 Pré-requisitos

1. **Docker Desktop:** Certifique-se de que o Docker Desktop está aberto e em execução no Windows.
2. **Conta no OpenRouter:** Sua chave de API obtida no painel [OpenRouter Keys](https://openrouter.ai/keys).
3. **Conta no Telegram:** Para criar seu bot e obter seu ID de usuário.

---

## 🛠️ Passo a Passo de Configuração

### 1. Criar o Bot no Telegram
1. Abra o Telegram e pesquise pelo bot oficial [**@BotFather**](https://t.me/BotFather).
2. Envie a mensagem: `/newbot`
3. Digite um **nome** para o seu agente (ex: `Meu Hermes Agent`).
4. Digite um **username** único terminando em `bot` (ex: `hermes_leoberne_bot`).
5. O BotFather responderá com uma mensagem de sucesso contendo o seu **HTTP API Token** (formato: `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ`).
6. Copie esse token.

### 2. Descobrir seu Telegram User ID (Segurança Crítica)
Para impedir que qualquer usuário na internet converse com seu bot e consuma seus créditos do OpenRouter, restringimos o acesso apenas ao seu ID numérico:
1. No Telegram, pesquise pelo bot [**@userinfobot**](https://t.me/userinfobot) ou [**@raw_data_bot**](https://t.me/raw_data_bot).
2. Clique em **Start** (ou envie qualquer mensagem).
3. Ele responderá com o seu **Id** (um número como `123456789`).
4. Copie esse número.

### 3. Preencher o Arquivo `.env`
Abra o arquivo `.env` (ou copie de `.env.example`) e preencha:

```bash
OPENROUTER_API_KEY=sk-or-v1-sua-chave-completa-aqui
HERMES_MODEL=anthropic/claude-3.5-sonnet
HERMES_PROVIDER=openrouter

TELEGRAM_BOT_TOKEN=seu_token_do_botfather_aqui
TELEGRAM_ALLOWED_USERS=seu_id_numerico_aqui

HERMES_HOME=/opt/data
```

> 💡 **Modelos Recomendados no OpenRouter:**
> - `anthropic/claude-3.5-sonnet` (Melhor capacidade analítica e de ferramentas)
> - `nousresearch/hermes-3-llama-3.1-405b` (Modelo emblemático da Nous Research)
> - `deepseek/deepseek-chat` (Excelente performance com custo muito baixo)

---

## 🐳 Executando com Docker Compose

### Modo A: Iniciar o Bot em Background (Recomendado)
Para subir o bot diretamente em segundo plano:
```powershell
docker compose up -d hermes-bot
```

Para acompanhar as mensagens e logs do bot:
```powershell
docker compose logs -f hermes-bot
```

### Modo B: Rodar o Wizard Interativo de Setup (Opção C)
Se desejar rodar a configuração interativa oficial do Hermes:
```powershell
docker compose run --rm hermes-setup setup
```

Ou para validar o gateway de mensageria:
```powershell
docker compose run --rm hermes-setup gateway setup
```

### Comandos de Manutenção
- **Parar o bot:** `docker compose down`
- **Reiniciar o bot:** `docker compose restart hermes-bot`
- **Verificar status do container:** `docker compose ps`

---

## 💾 Persistência de Dados
Todos os dados, histórico de conversas, memórias e habilidades aprendidas pelo Hermes são persistidos automaticamente na pasta local `./data/`, que é montada dentro do container em `/opt/data`.

---

## 🔒 Versionamento Git Seguro (`LeoBerne/Hermes-agent`)

O repositório já está configurado com `.gitignore` estrito para garantir que suas chaves nunca sejam enviadas ao GitHub.

Para conectar ao seu repositório no GitHub:

1. Crie um repositório vazio no GitHub com o nome **`Hermes-agent`** sob a conta **`LeoBerne`** (sem inicializar com README ou .gitignore).
2. No terminal desta pasta, execute:

```powershell
git remote add origin https://github.com/LeoBerne/Hermes-agent.git
git push -u origin main
```
