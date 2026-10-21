# Workflow: Automação de Recuperação de Inativos (Follow-up)

Entre 35% e 55% dos leads em anúncios Click-to-WhatsApp abrem a conversa, mas param de responder na primeira ou segunda pergunta por distração do cotidiano. O bloco de timeout de 24 horas recupera até um terço desses contatos sem esforço humano.

---

## Configuração Técnica do Timeout

1. No bloco de espera (`Aguardar resposta`):
   - Ative a opção de tempo limite.
   - Defina o prazo para `24 horas`.
2. Saída `Sem resposta` (`no_reply`):
   - Conecte primeiro ao bloco `move_crm` com destino `Follow-up (Urgência)`.
   - Conecte em seguida ao bloco de envio de mensagem de recuperação.

---

## Mensagem Padrão de Resgate

A mensagem de resgate nunca deve ser acusatória ("você sumiu"). Ela deve usar o princípio da escassez legítima e do serviço consultivo:

> "Olá! Passando para te avisar que as vagas deste lote estão quase esgotadas. Se você ainda tem interesse em garantir seu lugar conosco, me avisa por aqui para segurar sua condição antes de abrirmos para a lista de espera! 🏔️"

---

## Transbordo Após Resposta

Se o usuário responder à mensagem de recuperação:
- O robô retoma o funil do ponto onde parou, ou transfere imediatamente para a fila de atendimento humano prioritário.
