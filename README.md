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

# 📚 Agent School v1.0.0 – N8N Workflow

Este projeto cria um **professor virtual de inglês** usando **n8n**, **Supabase**, e **WhatsApp via VenomBot/Baileys**, capaz de:

1. Enviar **3 perguntas diárias** (8h, 12h, 19h)
2. **Corrigir automaticamente** as respostas
3. **Dar feedback motivador** e atribuir pontuação
4. **Registrar dados** no Supabase (perguntas, respostas e notas semanais)
5. Entrar em **modo professor** quando receber a palavra secreta **"Marte"**

---

## 📂 Estrutura do Projeto

agentschool/
├── agentschool.json # Fluxo do n8n completo
├── agentschool.md # Documentação técnica do fluxo
├── README.md # Guia do projeto
└── sql/
└── create_tables.sql # Script para criação das tabelas no Supabase


---

## ⚙️ Dependências

- **n8n** >= 1.60
- **Supabase** (PostgreSQL)
- **OpenAI / Local AI** para feedback automático
- **VenomBot** ou **Baileys** para integração WhatsApp

---

## 🗄️ Estrutura do Banco (Supabase)

O fluxo usa 4 tabelas principais:

1. **students** – Cadastro de alunos  
2. **questions** – Perguntas com nível de dificuldade  
3. **answers** – Respostas enviadas pelos alunos  
4. **scores** – Notas semanais

> **SQL completo em** `sql/create_tables.sql`

---

## 🛠️ Nós Principais do `agentschool.json`

| **Node**                    | **Função** |
|-----------------------------|------------|
| **Webhook WhatsApp**         | Recebe mensagens dos alunos |
| **Filter Marte**             | Ativa o modo professor com palavra secreta |
| **Supabase Get Student**     | Busca dados do aluno pelo número |
| **Supabase Get Question**    | Seleciona uma pergunta do nível correto |
| **Cron Send Questions**      | Dispara perguntas em 8h, 12h e 19h |
| **Prepare Question**         | Formata pergunta e opções para envio |
| **Function Check Answer**    | Compara resposta com gabarito e calcula nota |
| **Supabase Save Answer**     | Salva resposta e pontuação no banco |
| **AI Feedback**              | Gera feedback motivador para o aluno |

---

## ⏱️ Agendamento de Perguntas

- **8:00** → Pergunta 1  
- **12:00** → Pergunta 2  
- **19:00** → Pergunta 3

As respostas são **corrigidas automaticamente** e salvas na tabela `answers`.

---

## 🔑 Modo Professor

- Enviar **"Marte"** no WhatsApp  
- O agente entra em **modo professor**, podendo consultar:
  - Pontuação semanal
  - Estatísticas de engajamento
  - Respostas de todos os alunos

---

## 🚀 Como Usar

1. **Importe** `agentschool.json` no n8n
2. Configure suas **credenciais do Supabase**
3. Configure **VenomBot ou Baileys** apontando para o **Webhook WhatsApp**
4. Configure seu **token do OpenAI** para feedbacks
5. Execute o fluxo e teste com mensagens no WhatsApp

---

## 🔄 Próximos Passos

- Criar fluxo **scores-weekly.json** para agregação semanal automática
- Adicionar relatórios no **modo professor** para listar alunos e notas

---

## 📌 Observações

- Este fluxo está pronto para rodar em **EC2 com Chatwoot + Evolution**  
- **WhatsApp oficial Cloud API** pode ser implementado como alternativa  
- **Feedback AI** pode ser adaptado para qualquer LLM compatível

---

