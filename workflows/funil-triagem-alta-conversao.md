# Workflow: Funil de Triagem de Alta Conversão no WhatsApp

O objetivo deste funil é filtrar 100% dos curiosos sem orçamento antes de gastar o tempo da equipe humana.

---

## Estrutura dos Blocos no Canvas

```
[Trigger: Mensagem recebida / Anúncio CTWA]
  ↓
[Mensagem 1: Boas-vindas + Ancoragem de Preço + Opções de Compra]
  ↓
[Aguardar Resposta 1] ──(Timeout 24h)──> [move_crm: Follow-up] ──> [Msg Resgate]
  ├── Opção 1, 2, 3: [Tag: Opção] ──> [Mensagem 2: Filtro de Perfil]
  └── Opção 4: [Tag: Especial] ──> [move_crm: Atendimento Especial] ──> [Notificar Equipe]
                                            ↓
                             [Aguardar Resposta 2] ──(Timeout 24h)──> [move_crm: Follow-up] ──> [Msg Resgate]
                               ├── Opção 1, 2, 3: [Tag: Perfil]
                                            ↓
                             [move_crm: Triagem Concluída]
                                            ↓
                             [Mensagem 3: Entrega do Documento PDF]
                                            ↓
                             [Notificar Equipe: Áudio de Fechamento]
```

---

## Diretrizes de Redação das Mensagens

1. **Apresentação de Autoridade:**
   Sempre use a primeira pessoa do profissional ou fundador (ex: "Aqui quem fala é o Marcos Junior"). Isso humaniza o bot e aumenta a taxa de resposta.
2. **Ancoragem Imediata de Preço:**
   Nunca esconda o preço para dizer no final. Fale o valor do investimento e a condição facilitada (ex: "Entrada de R$ 1.500 + 8x no PIX"). Quem não tem orçamento já para aí, sem consumir a equipe.
3. **Opções Fáceis com Emojis Numéricos:**
   Peça para o usuário responder apenas 1, 2 ou 3. Isso facilita a digitação no celular e reduz o atrito para menos de 5 segundos.
4. **Entrega de Ativo Físico:**
   O envio do PDF oficial diagramado consolida o valor percebido e fornece ao lead todas as informações técnicas para a decisão em família ou com parceiro.
