# Random Chronos Ultra ⚡🦎

> **Meta-Skill & Sistema de Engenharia de WhatsApp, CRM & Meta Conversions API (CAPI) na Chronos Dock**
> Desenvolvido para a Random e clientes sob gestão da agência.

---

## 🎯 Por que esta Skill Existe?

Em campanhas Click-to-WhatsApp de alto valor, a maioria das agências sofre com três gargalos críticos:
1. **Atendentes sobrecarregados com curiosos sem orçamento:** Vendedores perdem horas respondendo quem não tem capacidade de compra.
2. **Perda massiva por inatividade (35% a 55% de abandono):** Leads abrem a conversa, são interrompidos e nunca mais respondem.
3. **Cegueira algorítmica da Meta:** O algoritmo da Meta só otimiza para o primeiro clique no WhatsApp, sem saber quais leads realmente compraram ou avançaram no pipeline.

O **Random Chronos Ultra** resolve essa tríade de ponta a ponta:
- **Ancoragem de Preço Instantânea:** O funil apresenta o valor monetário logo no início antes de transbordar para humanos.
- **Recuperação Autônoma de 24h:** O timeout resgata leads parados sem intervenção manual.
- **Loop Fechado de Conversão (CAPI Value-Based):** O avanço no CRM dispara eventos de volta para a Meta (`Lead`, `SubmitApplication`, `Contact`, `Schedule`, `Purchase` com `value: 6900 BRL`), treinando a inteligência da campanha com base em receita real.
- **Arquitetura Visual Camaleão:** O layout dos blocos no canvas da Chronos Dock é estruturado na topologia do Camaleão da Random, facilitando leitura modular, manutenibilidade e estética impecável de UX.

---

## 📁 Estrutura do Repositório

```
random-chronos-ultra/
├── SKILL.md                          # Instruções da Meta-Skill e despacho de comandos
├── README.md                         # Documentação geral de engenharia e operação
├── workflows/
│   ├── implantacao-novo-cliente.md   # Setup do zero em <30 minutos
│   ├── funil-triagem-alta-conversao.md# Arquitetura dos blocos de conversa
│   ├── crm-kanban-capi.md            # 9 etapas do CRM + mapeamento CAPI
│   └── recuperacao-abandono.md       # Configuração de timeouts e resgate
├── templates/
│   ├── kanban-9-etapas.json          # Estrutura JSON importável das etapas
│   ├── mensagens-triagem.md          # Copies validadas por segmento
│   └── funil-chameleon-layout.json   # Matriz de coordenadas do Camaleão
└── scripts/
    └── setup_cliente.py              # Script utilitário para bootstrap
```

---

## 🚀 Como Usar com o Agente de IA

Você pode invocar os fluxos no chat:
- `implantar <cliente>`: Executa o setup completo de pipeline, roteamento e CAPI.
- `funil`: Configura ou audita as mensagens e a ancoragem de preço.
- `kanban`: Cria ou padroniza as 9 etapas canônicas.
- `capi`: Sincroniza os eventos e valores monetários com a Meta.
- `recuperar`: Ativa as saídas de 24 horas para resgate de inativos.
- `auditar`: Verifica integridade de conexões, blocos pendentes e status ativo.

---

## 🔒 Regras Não Negociáveis

1. **Preço na Primeira Mensagem:** Nunca esconder o valor do serviço/produto.
2. **Zero Blocos Pendentes:** Nenhum funil é publicado com saídas desconectadas.
3. **Eventos CAPI com Valor Real:** Compras confirmadas devem conter `value` e `currency: BRL`.
4. **Voz Humana & Anti-IA:** Proibido uso de clichês robóticos, travessões artificiais ou construções 'não é X, é Y'.
