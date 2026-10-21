# Workflow: CRM Kanban & Meta Conversions API (CAPI)

A integração entre o CRM da Chronos Dock e a Meta CAPI fecha o ciclo de retroalimentação algorítmica: cada lead que avança na vida real treina os robôs de anúncio da Meta para buscar mais pessoas parecidas.

---

## Mapeamento de Eventos Padrão

| Etapa no CRM Chronos | Evento Meta CAPI | Objetivo Algorítmico |
|---|---|---|
| `Novo Lead (WhatsApp)` | `Lead` | Confirma recebimento do contato vindo do anúncio |
| `Triagem Concluída` | `SubmitApplication` | Sinaliza que o usuário respondeu à qualificação de perfil |
| `Áudio Enviado` | `Contact` | Marca engajamento pessoal do atendente humano |
| `Vaga Pré-Reservada (PIX)` | `Schedule` | Sinaliza alta intenção e pedido de reserva |
| `Atendimento Especial` | `Contact` | Trata casos personalizados fora do funil padrão |
| `Vaga Confirmada (Ganho)` | `Purchase` | Evento de conversão final de alto valor com receita atribuída |

---

## Otimização Baseada em Valor (Value-Based Bidding)

Para clientes de ticket médio alto (viagens, consultorias, serviços premium), disparar apenas o evento `Purchase` sem valor monetário impede que o Gerenciador de Anúncios use estratégias de ROAS Alvo.

### Como Configurar
1. Em `Configurações > Conversões Meta > Regras por Etapa do CRM`:
   - Selecione a etapa de fechamento (`Vaga Confirmada (Ganho)`).
   - Evento: `Purchase`.
   - Moeda: `BRL`.
   - Valor fixo: Ticket do produto (ex: `6900.00`).
2. Quando o vendedor move o card para `Ganho`:
   - O Chronos envia o payload com `value: 6900, currency: "BRL"`.
   - Na Meta, as colunas de "Valor de conversão de compras" e "ROAS" passam a ser preenchidas em tempo real.
