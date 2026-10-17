# Prompt Final do Desafio Criativo N8N:

Atue como um especialista em N8N.

Crie uma automação para Alerta de Chamados 

### Público:
* Equipe de suporte/TI 

### Ferramentas envolvidas:
* **Gmail:** trigger de recebimento e leitura das mensagens.
* **n8n:** orquestrador do fluxo e aplicação das regras de filtragem.
* **Telegram:** canal de recebimento da notificação instantânea.

### Fluxo:
1. **Disparo:** (Node Trigger Gmail) monitora a caixa de entrada e é ativado assim que um novo e-mail não lido é recebido.
2. **Validação (Node IF):** O n8n avalia se o assunto ou o corpo do e-mail contém termos críticos (ex.: "URGENTE", "CHAMADO", "PROBLEMA") ou se veio de um remetente prioritário.
3. **Formatação do Alerta (Node Edit Fields):** Extrai os dados principais (remetente, assunto, data/hora e um trecho do corpo da mensagem) e monta o texto da notificação.
4. **Envio da Mensagem:** (Node Telegram) envia o alerta estruturado diretamente para o chat ou canal da equipe de suporte.
5. **Atualização do E-mail:** (Node Gmail) Aplica uma label no e-mail com "Notificado" para evitar duplicidade.

### Regras:
* **Controle de Duplicidade:** Processar apenas e-mails não lidos aplicar uma label após o envio para não reenviar alertas a cada ciclo de verificação.
* **Filtro Case-Insensitive:** A validação das palavras-chave deve ignorar maiúsculas e minúsculas (ex.: aceitar "urgente", "URGENTE" e "Urgente").
* **Limite de Texto:** Limitar o resumo do corpo do e-mail a cerca de 200–300 caracteres para não estourar o limite de caracteres da mensagem nem poluir o chat.
* **Tratamento de Exceções:** Caso um e-mail não atenda aos critérios do filtro (caminho "false" do Node IF), o fluxo não disparar mensagem.
