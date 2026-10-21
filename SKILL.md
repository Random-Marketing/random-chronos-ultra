---
name: random-chronos-ultra
description: "Meta Skill de automação de WhatsApp, CRM e Meta Conversions API (CAPI) na Chronos Dock: triagem inteligente em cascata, ancoragem de preço, entrega de PDF, roteamento de anúncios Click-to-WhatsApp (CTWA), Kanban com 9 etapas e movimentação autônoma (move_crm), timeouts de 24h para recuperação de inativos, alertas em grupo de WhatsApp e exclusão de compradores. Use quando pedirem configurar, auditar, clonar ou operar Chronos Dock, funil de WhatsApp, automação de CRM ou CAPI para qualquer cliente."
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

### 1. Funil de Triagem em Cascata (Zero Curiosos)
* **Mensagem 1 (Boas-vindas + Ancoragem de Preço + Opções):**
  Apresenta a autoridade de quem atende, enquadra o diferencial do serviço e revela o valor logo na largada (ex: "O pacote começa em R$ 6.900 ou R$ 9.900"). O lead seleciona a data ou produto por botão numérico (1, 2, 3, 4).
* **Mensagem 2 (Filtro de Qualificação):**
  Identifica o perfil exato do comprador (experiência prévia, condicionamento, momento de compra).
* **Mensagem 3 (Entrega de Ativo de Alto Valor):**
  Dispara o documento oficial diagramado (Roteiro PDF, Catálogo ou Proposta) com MIME type `document`.
* **Mensagem 4 (Transbordo Humano com Alerta):**
  Alerta a equipe por mensagem de voz ou direct, notificando que o lead já está quente e pronto para fechamento.

### 2. Kanban Automatizado com 9 Etapas Canônicas
O CRM opera sobre uma esteira padronizada de 9 estágios:
1. `Novo Lead (WhatsApp)` (Cor `#64748b`) -> Dispara evento Meta `Lead`
2. `Triagem Concluída` (Cor `#3b82f6`) -> Dispara evento Meta `SubmitApplication`
3. `Áudio Enviado` (Cor `#8b5cf6`) -> Dispara evento Meta `Contact`
4. `Vaga Pré-Reservada (PIX)` (Cor `#14b8a6`) -> Dispara evento Meta `Schedule`
5. `Atendimento Especial` (Cor `#f97316`) -> Dispara evento Meta `Contact`
6. `Follow-up (Urgência)` (Cor `#eab308`) -> Sem evento
7. `Vaga Confirmada (Ganho)` (Cor `#22c55e`) -> Dispara evento Meta `Purchase`
8. `Perdido / Desistência` (Cor `#ef4444`) -> Etapa de Perda
9. `Lead falso` (Cor `#94a3b8`) -> Fixo: `FakeLead_Chronos`

### 3. Movimentação Autônoma (`move_crm`)
Em cada nó de resposta do funil, insere-se um bloco `move_crm` para arrastar o lead sozinho no painel:
* Lead respondeu as perguntas de qualificação -> `move_crm` para `Triagem Concluída`.
* Lead pediu atendimento fora do escopo -> `move_crm` para `Atendimento Especial`.
* Lead parou de responder -> `move_crm` para `Follow-up (Urgência)`.

### 4. Rastreamento e Meta Conversions API (CAPI)
* Roteamento de anúncios: todo clique em anúncio Click-to-WhatsApp (CTWA) é direcionado automaticamente para a Origem do produto e cai na coluna `Novo Lead (WhatsApp)`.
* Eventos sincronizados: cada avanço de etapa no CRM dispara o evento padronizado de volta para a Meta, alimentando o algoritmo de otimização da campanha.
* Valor monetário (Value-Based): nas etapas de fechamento (`Purchase`), sempre registrar o valor real da venda (`value: 6900`, `currency: BRL`) para liberar otimização por ROAS Alvo.

### 5. Recuperação de Inativos (Timeout de 24h)
* Nos blocos de espera por resposta, o pino `Sem resposta` é configurado para 24 horas.
* Ao estourar o prazo, o funil envia uma mensagem amigável de escassez e move o card no CRM para `Follow-up (Urgência)`, evitando que o lead se perca.

---

## Alertas em Tempo Real & Grupo de Notificações
* Link de Grupo de WhatsApp: pode ser cadastrado em `Configurações > WhatsApp` ou no nó de notificação do funil.
* Link de Pareamento Rápido: gere o link com token único de 24h (`/conectar-whatsapp/<token>`) para o cliente ler o QR Code sem login na plataforma.

---

## Regras Não Negociáveis

1. **Zero mensagens genéricas sem preço:** O funil sempre ancora o valor antes de gastar o tempo do atendente humano.
2. **PDF sempre como documento oficial:** Anexos devem ser disponibilizados em URL HTTPS pública e permanente.
3. **Nenhum funil sobe com bloco pendente:** Todo pino de saída deve ter destino (inclusive o pino de timeout).
4. **Respeito estrito à voz do cliente:** Seguir as diretrizes da skill `/nei` para qualquer texto comercial ou de atendimento.
