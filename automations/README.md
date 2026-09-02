# Automação de Leads — Desafio Shape de Noiva

Dois workflows do n8n, prontos pra importar. Resolvem: onde os leads ficam salvos, e o reenvio automático de e-mail pra quem não comprou (parando sozinho quando a pessoa compra).

## Arquivos

| Arquivo | O que faz |
|---|---|
| `n8n-lead-nurture.json` | Recebe o lead do quiz → salva na planilha → dispara os 3 e-mails com 24h de intervalo, checando antes de cada um se a pessoa já comprou |
| `n8n-mark-purchased.json` | Recebe aviso de compra do Hotmart/Kiwify → marca `comprou: sim` na planilha, o que interrompe os próximos e-mails automaticamente |

## Passo a passo pra ativar

### 1. Crie a planilha de leads

Google Sheets com uma aba chamada **Leads** e estas colunas na primeira linha:

```
timestamp | nome | email | whatsapp | perfil | dor_principal | comprou | emails_enviados
```

### 2. Importe os dois workflows no seu n8n

`n8n → Workflows → Import from File` para cada um dos dois `.json` desta pasta.

### 3. Configure as credenciais em cada node

- **Google Sheets** (nos dois workflows): conecte sua conta Google, cole o ID da planilha (está na URL dela) no lugar de `COLE_AQUI_O_ID_DA_SUA_PLANILHA`
- **E-mail** (`n8n-lead-nurture.json`): configure SMTP — pode ser Gmail, Brevo, ou qualquer provedor. Troque `contato@seudominio.com` pelo seu e-mail real
- Nos 3 e-mails, troque `COLE_AQUI_O_LINK_DE_CHECKOUT` pelo link real do Hotmart/Kiwify

### 4. Ative o webhook de lead no quiz

O arquivo `site/index.html` já está preparado — só falta colar a URL do seu webhook n8n (ela aparece no node "Webhook - Novo Lead" depois de importar e ativar o workflow) na constante `LEAD_WEBHOOK_URL` no topo do `<script>`.

### 5. Configure o postback do Hotmart/Kiwify

No painel do produtor (Hotmart: Configurações → Postback / Kiwify: Webhooks), aponte pra URL do node "Webhook - Compra Aprovada" do segundo workflow (`n8n-mark-purchased.json`), disparando no evento de **compra aprovada**.

### 6. Ative os dois workflows

Sem isso, ficam salvos mas não rodam. Vire o toggle "Active" em cada um.

---

## Como funciona na prática

```
Lead responde o quiz
        │
        ▼
Webhook (lead) → Planilha (comprou: não) → E-mail 1 (na hora)
                                                  │
                                          espera 24h, checa planilha
                                                  │
                                    comprou? ──sim──> PARA (não envia mais nada)
                                          │
                                         não
                                          │
                                          ▼
                                     E-mail 2 → espera 24h → checa de novo → E-mail 3

Em paralelo, a qualquer momento:
Hotmart/Kiwify avisa compra → Webhook (compra) → marca "comprou: sim" na planilha
```

## Observação honesta

Escrevi esses workflows sem ter n8n conectado nesta sessão pra testar o import de verdade — a estrutura (nodes, conexões, parâmetros) segue o padrão real do n8n, mas nomes exatos de campo podem variar um pouco conforme a versão do seu n8n. Se algum node reclamar ao importar, me manda o erro que eu ajusto.
