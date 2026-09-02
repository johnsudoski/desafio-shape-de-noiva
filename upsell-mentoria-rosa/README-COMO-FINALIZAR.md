# Rosa — Como Deixar 100% Real

Os 3 workflows estão prontos e a "cabeça" da Rosa (o prompt) está escrita. O que falta é só conectar as peças — nenhuma delas eu consigo fazer por você, porque todas exigem login/pagamento em contas que são suas.

---

## ✅ O que já está pronto

- [x] `n8n-rosa-conversa-reativa.json` — Rosa responde perguntas, gera receita/cronograma na hora, respeita restrição alimentar, trava em sintoma médico
- [x] `n8n-rosa-drip-diario.json` — manda o plano do dia sozinha, todo dia às 7h, sem repetir o mesmo texto (a IA gera variação real a cada dia)
- [x] `n8n-rosa-reengajamento.json` — se a noiva sumir 2+ dias, manda mensagem de carinho sozinha
- [x] System prompt com os guardrails (sem promessa de kg, sem conselho médico, admite ser IA se perguntada)
- [x] Persona, tom de voz, copy de venda do upsell (arquivo `persona-e-fluxo.md`)

---

## ❌ O que falta — em ordem de execução

### 1. Conta na Z-API (WhatsApp) — R$0 a ~R$100/mês dependendo do plano
- Acesse [z-api.io](https://z-api.io), crie a conta
- Conecte um número de WhatsApp (recomendo um número novo, não o seu pessoal — a Rosa vai mandar/receber muita mensagem)
- Pegue o **Instance ID** e o **Token** — vão substituir `COLE_SEU_INSTANCE_ID` e `COLE_SEU_TOKEN` nos 3 arquivos JSON
- Configure o webhook de "mensagem recebida" apontando pra URL do node "Webhook - Mensagem Recebida" (aparece depois de importar e ativar o Workflow A no n8n)

### 2. Chave da API da Anthropic (o cérebro da Rosa) — pago por uso
- Acesse [console.anthropic.com](https://console.anthropic.com), crie a conta, adicione um cartão
- Gere uma API key
- No n8n, cria uma credencial do tipo "Header Auth" com o header `x-api-key` = sua chave (usada no node "Chamar Rosa (Claude API)")
- **Estimativa de custo:** cada conversa/mensagem custa poucos centavos (modelo usado é o Sonnet 5, o mais em conta pra esse tipo de uso). Com volume baixo no começo, deve ficar bem abaixo de R$50/mês.

### 3. Atualizar sua planilha de Leads
Adicione essas colunas na planilha que já existe (a mesma do fluxo de e-mail, ou uma nova só pra mentoria):

```
nome | telefone | data_compra | restricoes_alimentares | mentoria_ativa | ultima_interacao
```

- `telefone`: no formato que a Z-API usa (com DDI, ex: 5511999999999)
- `mentoria_ativa`: escreva "sim" pra cada noiva que comprou o upsell
- `data_compra`: data que ela comprou (usada pra calcular em que dia do desafio ela está)
- `restricoes_alimentares` e `ultima_interacao`: começam vazias, a Rosa/o fluxo preenche sozinho

### 4. Importar os 3 workflows no n8n
`n8n → Workflows → Import from File` pra cada um dos 3 `.json` desta pasta.

### 5. Colar as credenciais em cada node
Em cada arquivo, procure e substitua:
- `COLE_AQUI_O_ID_DA_PLANILHA_DE_LEADS` → ID da sua planilha (está na URL dela)
- `COLE_SEU_INSTANCE_ID` e `COLE_SEU_TOKEN` → dados da Z-API
- Conecte a credencial da Anthropic no node "Chamar Rosa (Claude API)" / "Rosa Gera a Mensagem do Dia"
- Conecte sua conta Google nos nodes de Google Sheets

### 6. TESTE ANTES DE VENDER (não pule isso)
- Ative só o Workflow A primeiro
- Manda mensagem do seu próprio WhatsApp pro número da Rosa, simulando uma noiva
- Testa pelo menos:
  - Pergunta normal ("posso comer arroz hoje?")
  - Pedido de receita nova ("me dá uma receita vegetariana pro almoço")
  - Pergunta fora do escopo, tipo sintoma médico (confirma que ela trava e manda procurar médico)
  - Pergunta "você é humana?" (confirma que ela responde a verdade com carinho)
- Só depois de testar por uns dias, ativa os Workflows B (drip) e C (reengajamento) e libera a venda de verdade

### 7. Acompanhamento (recomendo fortemente)
Como ninguém revisa as respostas da Rosa em tempo real, vale registrar toda conversa numa aba separada da planilha (ou um log simples) pra você dar uma olhada de vez em quando — garante que ela não está saindo do roteiro com ninguém. Não construí isso ainda porque não sei se você quer — me avisa se quiser que eu adicione esse log nos workflows.

---

## Resumo — o que é seu, o que é meu

| Etapa | Quem faz |
|---|---|
| Escrever os workflows, o prompt, a persona | ✅ Eu já fiz |
| Criar conta Z-API + pagar | 🔲 Você (precisa de cartão/CPF seu) |
| Criar conta Anthropic + pagar | 🔲 Você (precisa de cartão seu) |
| Importar os workflows | 🔲 Você (2 minutos, é só um upload) |
| Colar as credenciais | 🔲 Você (copiar/colar) |
| Testar antes de vender | 🔲 Você — **essa etapa é a mais importante de todas** |

Assim que você tiver a conta Z-API e a chave da Anthropic, me chama que eu reviso os arquivos com você linha por linha, campo por campo, até funcionar.
