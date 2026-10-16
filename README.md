# 📨 Automação de Processo Seletivo com n8n

Fluxo de automação desenvolvido no **n8n** envio automatizado de feedbacks individuais para candidatos aprovados com controle de taxa (*rate limiting*) e consolidação de relatório de reprovados para o time interno.

---

## 🎯 Objetivo

Eliminar o trabalho manual de envio de comunicados em processos seletivos, garantindo:
- Envio pontual e personalizado de mensagens de aprovação;
- Proteção da reputação do remetente de e-mail por meio de controle de cadência (*rate limiting*);
- Geração de relatório dos candidatos reprovados em um único disparo.

---

## 🗺️ Arquitetura do Fluxo

<img width="1362" height="605" alt="Captura de tela 2026-10-15 232007" src="https://github.com/user-attachments/assets/f2bc216a-a988-45d4-9926-17de2cacfd97" />

1. **Schedule Trigger (`Dispara no dia...`):** Gatilho temporal agendado para o encerramento da etapa do processo seletivo.
2. **Google Sheets (`Obtem Candidatos`):** Faz a leitura dos registros e status dos candidatos na planilha base.
3. **If (`Candidato Aprovado?`):** Roteador que segmenta o fluxo com base no campo de aprovação.
4. **Trilha de Aprovados (Branch True):**
   - **Looping (`Para cada Candidato`):** Itera candidato a candidato.
   - **E-mail (`E-mail para Candidato`):** Dispara o e-mail com as instruções da próxima etapa.
   - **Wait (`Aguarda 5s`):** Pausa programada de 5 segundos para evitar bloqueios por spam e respeitar quotas do servidor SMTP.
5. **Trilha de Reprovados (Branch False):**
   - **Item Lists / Code (`Gera Array de Nomes`):** Agrupa os nomes de todos os reprovados em um único array estruturado.
   - **E-mail (`Envia Lista de Reprovados`):** Dispara um único e-mail com a listagem consolidada para os responsáveis pela vaga.

---

## 🛠️ Tecnologias e Conceitos Utilizados

- **[n8n](https://n8n.io/):** Orquestrador de fluxos low-code / automação de processos.
- **Google Sheets API:** Integração e extração de dados tabulares.
- **Serviço de E-mail (SMTP/Gmail):** Notificações transacionais e relatórios.
- **Lógica e Controle de Fluxo:** `Loops`, `Conditionals`, manipulação de arrays e *Throttling/Rate Limiting*.

---

## 🚀 Como Importar e Usar este Fluxo

1. Baixe o arquivo `automacao-processo-seletivo-aula.json` deste repositório.
2. No seu n8n, clique em **Workflows** > **Import from File...** e selecione o arquivo baixado.
3. Configure as credenciais necessárias:
   - Credencial do **Google Sheets** (OAuth2 ou Service Account).
   - Credencial do serviço de **E-mail** (SMTP, Gmail, etc.).
4. Atualize o ID da sua planilha no nó **Obtem Candidatos**.
5. Substitua os placeholders de e-mail pelo seu remetente/destinatário desejado.
6. Ative o workflow.
