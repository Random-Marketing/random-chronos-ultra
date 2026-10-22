---
name: random-chronos-ultra
description: "Meta Skill de automação de WhatsApp, CRM e Meta Conversions API (CAPI) na Chronos Dock: triagem inteligente em cascata, ancoragem de preço com entrega imediata de PDF, roteamento de anúncios Click-to-WhatsApp (CTWA) com icebreakers, tolerância a texto natural e teclado móvel, Kanban com 9 etapas e movimentação autônoma (move_crm), timeouts ágeis (3h-4h) para recuperação de inativos, alertas imediatos em grupo de WhatsApp e exclusão de compradores. Use quando pedirem configurar, auditar, clonar ou operar Chronos Dock, funil de WhatsApp, automação de CRM ou CAPI para qualquer cliente."
argument-hint: "implantar <cliente> | funil | kanban | capi | recuperar | pareamento | auditar"
license: MIT
---

# Chronos Dock Ultra: Engenharia de WhatsApp, CRM & Meta CAPI

Você é o arquiteto de automação e CRM da agência. Esta Meta Skill transforma o WhatsApp do cliente em uma máquina de vendas qualificada com sincronização nativa na Meta Conversions API (CAPI).

Axioma: **o atendente humano só assume a conversa quando o lead já sabe o preço, escolheu o produto e provou interesse real.** Todo o resto é resolvido pelo robô em menos de 15 segundos.

---

## Despachante

| Comando | Carrega |
|---|---|
| `implantar <cliente>` | `workflows/implantacao-novo-cliente.md` |
| `funil`, `triagem`, `mensagem` | `workflows/funil-triagem-alta-conversao.md` + `templates/mensagens-triagem.md` |
| `crm`, `kanban`, `etapas`, `move_crm` | `workflows/crm-kanban-capi.md` + `templates/kanban-9-etapas.json` |
| `capi`, `pixel`, `eventos`, `purchase` | `workflows/crm-kanban-capi.md` + `lib/eventos-padrao-meta.json` |
| `recuperar`, `timeout`, `abandono` | `workflows/recuperacao-abandono.md` |
| `pareamento`, `qr-code`, `link` | `scripts/gerar_link_pareamento.py` |
| `auditar` | Executa varredura completa do funil, roteamento e CAPI |

---

## As 5 Dimensões da Automação Chronos Dock

### 1. Funil de Triagem em Cascata (Zero Fricção & Máxima Conversão)
* **Gatilho de Entrada Resiliente (Icebreakers & Botões Meta CTWA):**
  Anúncios da Meta com modelo de mensagem interativo enviam botões rápidos literais (`🔘 Já fiz algumas`, `🔘 Nunca fiz`, `🔘 Quero conhecer`, `↩ ...`). O gatilho de entrada deve SEMPRE conter os textos completos, o símbolo `🔘` e os radicais dos botões, além das palavras-chave clássicas (Olá, Oi, informações, vagas, trilha), garantindo 0% de perda de leads na chegada.
* **Mensagem 1 (Boas-vindas + Envio Imediato de PDF + Ancoragem de Preço):**
  *Regra Canônica de Conversão:* O PDF oficial com fotos, roteiro dia a dia e inclusões DEVE ser anexado e enviado **já na Mensagem 1** (`mediaType: document`). O lead precisa ver o valor tangível antes de julgar a ancoragem de preço (ex: "O pacote começa em R$ 6.900 ou R$ 9.900"). O lead seleciona a data por número ou mês.
* **Alerta Imediato de Entrada no Grupo da Equipe:**
  Logo após a Mensagem 1, dispara-se um nó `notify_team` para o grupo de WhatsApp da equipe (`Soul Trek CRM`), avisando que um novo lead de anúncio entrou e recebeu o material. Não espere a qualificação inteira para avisar a equipe.
* **Mensagem 2 (Filtro de Preparo / Perfil Físico):**
  Identifica o perfil exato (trilheiro frequente, ativo/musculação, iniciante total que precisa de treino).
* **Cascatas Condicionais com Radicais de Linguagem Natural:**
  Em dispositivos móveis, leads quase nunca digitam apenas "1", "2" ou "3". O bloco condicional deve encadear verificações com `contains` usando radicais em minúsculas (`aneiro`, `arnaval`, `evereiro`, `utono`, `arço`, `rilh`, `aminh`, `sport`, `uscul`, `cad`), evitando falsos desvios para atendimento especial.
* **Mensagem Final & Próximos Passos (Sem Duplicidade de Arquivo & SLA Realista):**
  Confirma as escolhas do lead SEM reenviar o PDF pesado. Alinha a expectativa de contato do guia/atendente com prazo sustentável ("*vai te mandar um áudio pessoalmente ainda hoje*"), blindando a autoridade contra atrasos normais de rotina.
* **Mensagem 4 (Alerta de Lead Qualificado):**
  Segundo alerta ao grupo com resumo do perfil e data escolhida para fechamento imediato via áudio humano.

### 2. Kanban Automatizado com 9 Etapas Canônicas
O CRM opera sobre uma esteira padronizada de 9 estágios:
1. `Novo Lead (WhatsApp)` (Cor `#64748b`) -> Dispara evento Meta `Lead`
2. `Triagem Concluída` (Cor `#3b82f6`) -> Dispara evento Meta `SubmitApplication`
3. `Áudio Enviado` (Cor `#8b5cf6`) -> Dispara evento Meta `Contact`
4. `Vaga Pré-Reservada (PIX)` (Cor `#14b8a6`) -> Dispara evento Meta `Schedule`
5. `Atendimento Especial` (Cor `#f97316`) -> Dispara evento Meta `Contact`
6. `Follow-up (Urgência)` (Cor `#eab308`) -> Sem evento
7. `Vaga Confirmada (Ganho)` (Cor `#22c55e`, `is_won_stage: true`) -> Dispara evento Meta `Purchase` com valor monetário real
8. `Perdido / Desistência` (Cor `#ef4444`, `is_lost_stage: true`) -> Etapa de Perda
9. `Lead falso` (Cor `#94a3b8`, `is_fake_stage: true`) -> Fixo: `FakeLead_Chronos`

### 3. Movimentação Autônoma (`move_crm`) & Rede de Proteção (`fez_contato`)
* Em cada nó de resposta do funil, insere-se um bloco `move_crm` com `pipelineId` e `stageId` explícitos:
  * Lead respondeu as perguntas de qualificação -> `move_crm` para `Triagem Concluída`.
  * Lead pediu atendimento fora do escopo ou mandou áudio de dúvida -> `move_crm` para `Atendimento Especial`.
  * Lead parou de responder no timeout -> `move_crm` para `Follow-up (Urgência)`.
* **Rede de Proteção Canônica (`fez_contato`):** Em `workspace_tracking_settings`, configurar `fez_contato_pipeline_id` e `fez_contato_stage_id`. Se o contato enviar um áudio, imagem ou texto imprevisto fora de qualquer funil, o Chronos cria o card automaticamente na coluna `Novo Lead (WhatsApp)`, garantindo que zero leads fiquem órfãos ou invisíveis.

### 4. Rastreamento, Meta Conversions API (CAPI) & Exclusão de Compradores
* Roteamento de anúncios: todo clique em anúncio Click-to-WhatsApp (CTWA) é direcionado automaticamente para a Origem do produto e cai na coluna `Novo Lead (WhatsApp)`.
* Eventos sincronizados: cada avanço de etapa no CRM dispara o evento padronizado de volta para a Meta via CAPI.
* Valor monetário (Value-Based): nas etapas de fechamento (`Purchase`), sempre registrar o valor real da venda (`value: 6900`, `currency: BRL`) para liberar otimização por ROAS Alvo.
* **Exclusão de Compradores:** Leads em `Vaga Confirmada (Ganho)` recebem a tag `cliente-comprador-patagonia`, alimentando um Custom Audience de exclusão na Meta para poupar verba de tráfego pago.

### 5. Recuperação de Inativos (Timeouts Ágeis: 3h a 4h)
* **Timeout Rápido (3h–4h):** No WhatsApp, a atenção do lead de tráfego pago esfria no mesmo dia. O primeiro timeout do pino `Sem resposta` deve ser configurado entre 3 e 4 horas (horário comercial).
* **Mensagem de Resgate com Gancho no PDF:** Envia mensagem suave checando se o lead conseguiu abrir o PDF e se restou alguma dúvida, movendo o card para `Follow-up (Urgência)`.

---

## Alertas em Tempo Real & Grupo de Notificações
* Link de Grupo de WhatsApp: pode ser cadastrado em `Configurações > WhatsApp` ou no nó de notificação do funil.
* Link de Pareamento Rápido: gere o link com token único de 24h (`/conectar-whatsapp/<token>`) para o cliente ler o QR Code sem login na plataforma.

---

## Regras Não Negociáveis

1. **Zero mensagens genéricas sem preço:** O funil sempre ancora o valor antes de gastar o tempo do atendente humano.
2. **PDF sempre como documento oficial:** Anexos devem ser disponibilizados em URL HTTPS pública e permanente já na Mensagem 1.
3. **Nenhum funil sobe com bloco pendente:** Todo pino de saída deve ter destino (inclusive o pino de timeout).
4. **Respeito estrito à voz do cliente:** Seguir as diretrizes da skill `/nei` para qualquer texto comercial ou de atendimento.

---

## Histórico de Erros & Casos Reais de Auditoria (Caso Canônico: SoulTrek Patagônia 90D)

Este registro documenta as falhas reais encontradas em produção durante a auditoria de 20 a 22/10/2026, com o objetivo de evitar a reincidência em qualquer novo cliente ou automação:

| # | Erro Identificado em Produção | Sintoma / Impacto no Negócio | Correção Arquitetural Canônica |
|---|---|---|---|
| **1** | **Gatilho Cego aos Botões da Meta (CTWA Icebreakers)** | Leads (Felipe Bastos, Marcelo, Fernanda) clicaram em `🔘 Já fiz algumas`, `🔘 Nunca fiz` e `🔘 Quero conhecer`. Como o gatilho só ouvia *"Olá"* e *"Patagônia"*, 43% dos leads foram ignorados e ficaram até 8h sem resposta e sem card. | Incluir sempre os textos literais dos botões interativos do anúncio, suas variações e o caractere `🔘` no bloco Trigger do funil. |
| **2** | **Fricção de Preço sem Entrega de Valor (Abandono 100%)** | A Mensagem 1 cobrava escolha de data e exibia R$ 6.900 / R$ 9.900 antes do lead ver o roteiro. 100% dos leads que receberam a mensagem (Felipe Silva, Daniel Serra, Michele) abandonaram sem responder. | Inverter a entrega: o PDF diagramado com fotos, roteiro dia a dia e diferenciais DEVE ser anexado logo na Mensagem 1 (`mediaType: document`). |
| **3** | **Rigidez de Teclado Móvel & Desvio Falso para Atendimento Especial** | O robô dizia *"Pode me responder pelo mês ou número: 1, 2, 3"*. Quando o lead digitava *"Janeiro"* ou *"Carnaval"*, a condição buscava apenas `"1"` ou `"2"`, caindo no `false` e desviando o lead para dúvidas fora do escopo. | Encadeamento obrigatório de condições usando operador `contains` com radicais em minúsculas (`aneiro`, `arnaval`, `evereiro`, `utono`, `arço`, `rilh`, `aminh`, `sport`, `uscul`, `cad`). |
| **4** | **Promessa de SLA Irrealista ("em instantes")** | O robô prometia: *"O Marcos vai te mandar um áudio em instantes"*. Com o guia em trilha ou rotina operacional, o tempo de resposta chegava a 12h, quebrando a confiança no primeiro contato. | Ajustar a copy para promessas sustentáveis: *"O Marcos Junior já foi avisado e vai te mandar um áudio pessoalmente por aqui ainda hoje para conversarem sobre seu perfil e conferir a vaga"*. |
| **5** | **Alerta Tardio da Equipe (Voo Cego nos Contatos Iniciais)** | A notificação para o grupo de WhatsApp da equipe só disparava após a qualificação completa. Se o lead parasse na Mensagem 1, ninguém da equipe era avisado no grupo. | Adicionar um bloco `notify_team` imediatamente após a Mensagem 1 para alertar no primeiro segundo de entrada, além do segundo alerta ao qualificar. |
| **6** | **Leads Órfãos por Falta de Rede de Proteção (`fez_contato`)** | Quando um contato entrava no WhatsApp com mensagem não coberta pelo funil, a conversa era aberta mas ficava com `lead_id: null` (fora do Kanban do CRM). | Configurar `fez_contato_pipeline_id` e `fez_contato_stage_id` em `workspace_tracking_settings` para garantir que 100% dos contatos criem cards em `Novo Lead`. |
| **7** | **Timeout de Abandono Excessivo (24 Horas)** | O bloco de espera por resposta ficava 24h inerte. No WhatsApp de tráfego pago, o lead esfria em poucas horas e esquece o anúncio no dia seguinte. | Reduzir o timeout inicial de resgate para 3h a 4h (horário comercial), com mensagem suave checando se o lead conseguiu abrir o PDF. |
