# Projeto: Agente Schoolv1.0.0

## Objetivo
Automatizar o envio de perguntas de inglês para alunos via WhatsApp, corrigir respostas, registrar notas e consolidar métricas semanais.

## Estrutura
- **Entrada:** WhatsApp via Webhook (VenomBot/Baileys ou WhatsApp Cloud API)
- **Processamento:** n8n + Supabase + AI Agent
- **Saída:** Registro no banco e feedback ao aluno

### Principais Nós do n8n
1. **Webhook WhatsApp** – Recebe mensagens do aluno.
2. **Supabase Insert** – Salva respostas.
3. **Function Node** – Avalia se resposta está correta e atribui pontuação.
4. **AI Agent (OpenAI Node)** – Gera feedback e mensagens motivacionais.
5. **Scheduler (Cron Node)** – Dispara perguntas em 8h, 12h e 19h.
6. **Supabase Query** – Recupera perguntas do nível do aluno.
7. **Aggregate Weekly Scores** – Soma pontos e salva na tabela `scores`.
8. **Professor Mode** – Se receber "Marte", retorna estatísticas do aluno.

### Regras
- Cada aluno recebe 3 perguntas por dia.
- Respostas são corrigidas automaticamente.
- Notas semanais são armazenadas na tabela `scores`.
- Palavra secreta **"Marte"** ativa o modo professor.

