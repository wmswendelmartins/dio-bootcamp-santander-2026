# Automação de Publicação e Notificação com n8n e Telegram

Fluxo de automação desenvolvido no **n8n** para agendar, publicar e rastrear envios de artigos/conteúdos diretamente em canais do Telegram a partir de uma base de dados no Google Sheets.

---

## 🗺️ Arquitetura do Fluxo

<img width="1366" height="600" alt="Captura de tela 2026-10-17 110627" src="https://github.com/user-attachments/assets/7fcba300-9d0b-4306-b5a3-a3fe5fdca0d0" />

1. **Schedule Trigger**: Executa de segunda a sexta-feira, às 09:00.
2. **Google Sheets (Leitura)**: Busca os registros que estão com o status `Não Enviado`.
3. **Item Lists / Limit**: Seleciona um artigo por execução para evitar disparos em massa.
4. **Telegram (Publicação)**: Formata e envia o título, descrição e link para o canal principal.
5. **If (Validação)**: Confirma se o disparo foi concluído com sucesso.
6. **Atualização & Alerta**:
   - Atualiza o status da linha na planilha para `Enviado`.
   - Envia uma notificação de confirmação no canal de monitoramento interno.

---

## 🛠️ Tecnologias Utilizadas

- **n8n** (Self-hosted ou Cloud)
- **Google Sheets API**
- **Telegram Bot API**

---

## 📋 Pré-requisitos

1. **Bot no Telegram**: Criado via [@BotFather](https://t.me/botfather) com token de acesso.
2. **Canais no Telegram**:
   - Canal de publicação (o bot precisa ter permissão de administrador para postar).
   - Canal/grupo de monitoramento interno.
3. **Planilha no Google Sheets** estruturada com as seguintes colunas:
   - `TITULO`
   - `DESCRIÇÃO`
   - `URL`
   - `STATUS` (ex: `Não Enviado` / `Enviado`)

---

## 🚀 Como Usar

1. Clone este repositório ou baixe o arquivo `sistema-de-notificação-via-telegram.json`.
2. No seu n8n, clique em **Add Workflow** > **Import from File...** e selecione o JSON.
3. Configure as credenciais:
   - Credencial OAuth2 ou Service Account do **Google Sheets**.
   - Credencial do **Telegram API** com o token do seu bot.
4. Ajuste as variáveis:
   - Substitua o ID da planilha nos nós do Google Sheets pelo ID da sua planilha.
   - Atualize os campos `Id` nos nós do Telegram com os identificadores dos seus canais (ex: `@seu_canal`).
5. Ative o workflow.
