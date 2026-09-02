<!-- Baseado em nexus/TEMPLATE-PROJETO.md -->
# NOME DO PROJETO: Desafio Shape de Noiva

## 🎯 01. Visão Geral / Escopo
**O que é o projeto?**
Infoproduto low ticket (R$47,90) — desafio de emagrecimento de 21 dias para noivas com casamento marcado. Formato: quiz funnel interativo (sem VSL nesta fase — VSL fica por conta do usuário, feita separadamente).

**Por que estamos fazendo isso?**
Pesquisa de mercado (squad Spy, `*market`) identificou esse subnicho como oceano azul: ~970 mil noivas/ano no Brasil, dor emocional documentada (ansiedade pré-casamento leva a comer mais), e **nenhum infoproduto de escala** competindo diretamente nesse posicionamento no Hotmart/Kiwify — diferente do "desafio de verão genérico", que está saturado.

---

## 🚦 02. Status Atual (Ponto de Retomada)
> **Última Atualização:** 2026-09-01
> **Onde paramos:** Pesquisa de mercado feita → estrutura do produto definida (3 semanas, R$47,90) → quiz de 9 perguntas + 4 perfis de resultado escrito → e-mails de follow-up escritos → site do quiz construído (`site/index.html`) com imagens reais licenciadas (Pexels).

**PDFs prontos pra subir no checkout:** `entrega-checkout/Desafio-Shape-de-Noiva-Ebook.pdf` (~2MB) e `entrega-checkout/Checklist-21-Dias.pdf` — ambos gerados via Edge headless. Cada um traz um QR code + link levando pra versão web interativa (checklist com checkbox real e progresso salvo), então a cliente tem as duas experiências: papel pra imprimir, link pra usar no celular.

**Próxima Etapa Focada:**
- [ ] Usuário revisa o ebook e o checklist e aprova
- [ ] Montar fluxo n8n: quiz → webhook → Google Sheets (leads) → disparo dos 3 e-mails
- [ ] Configurar checkout real (Hotmart/Kiwify) e plugar o link no CTA
- [ ] VSL fica em segundo plano por enquanto (combinado com o usuário) — quando pronta, entra entre `.tip-box` e `.offer-card` no resultado do quiz
- [ ] Gamma não conectou nesta sessão (timeout) — se o usuário quiser tentar usá-lo depois, reconectar via `/mcp`

---

## 🧠 03. Registro de Decisões e Agentes Usados
- **Agente/Squad Solicitado:** `spy` (`*market`) → pesquisa de oportunidade do subnicho "noivas" vs. "SOP" vs. "desafio verão genérico"
- **Decisão:** entrar direto no subnicho noivas (sem parceria de credencial, diferente do caso SOP — risco regulatório baixo-médio, não é condição médica)
- **Agente/Squad Solicitado:** `frontend-dev` (padrão consultado) → mobile-first, HTML semântico, JS mínimo, performance
- **Decisão:** site construído como single-file leve, sem framework pesado, seguindo o padrão do cargo
- **Agente/Squad Solicitado:** `creative-director` (padrão consultado) → hierarquia visual que guia do headline ao CTA sem desvio
- **Decisão:** aplicado no fluxo do quiz — cada tela tem 1 pergunta, 1 ação, sem distração

**Nota sobre imagens:** usuário pediu para "pegar imagens reais do Google" — não fiz isso (uso de Google Images para produto comercial é risco de direito autoral, já sinalizado antes nesta conversa). Usei fotos reais licenciadas do Pexels (uso comercial livre, sem risco), baixadas e salvas localmente em `site/images/`.

---

## 📦 04. Assets / Tarefas Pendentes
- [x] Pesquisa de mercado (`docs/pesquisa-mercado.md`)
- [x] Estrutura do produto (`docs/produto-estrutura.md`)
- [x] Estrutura do quiz — 9 perguntas + 4 perfis (`docs/quiz-estrutura.md`)
- [x] 3 e-mails de follow-up (`emails/`)
- [x] Site do quiz funcional (`site/index.html`) — preview ao vivo: https://claude.ai/code/artifact/4ff8a634-b805-413b-a046-be8cf95139c9
- [x] Ebook completo e expandido (`ebook/ebook.html`) — 21 dias individuais (não rotação genérica), 45+ receitas, 7 treinos variados, Semana 0 (preparação), Capítulo Mentalidade, lista de compras por semana, FAQ, glossário, manutenção pós-casamento — preview: https://claude.ai/code/artifact/9cb29b0e-5219-43fd-a5a7-20dfc7c824de
- [x] Kit Anti-Ansiedade Pré-Casamento — 5 técnicas, entregue como seção bônus dentro do ebook
- [x] Checklist 21 dias **interativo** (checkbox real, progresso salvo via localStorage) (`bonus-checklist/checklist-21-dias.html`) — preview: https://claude.ai/code/artifact/16c33357-d22d-4d8d-a074-f35ca4ae650e
- [ ] VSL (fica com o usuário, em segundo plano por enquanto)
- [ ] Link de checkout real (Hotmart/Kiwify)
- [ ] Fluxo n8n de captura de lead + disparo de e-mail
- [ ] Publicar/hospedar em domínio final
