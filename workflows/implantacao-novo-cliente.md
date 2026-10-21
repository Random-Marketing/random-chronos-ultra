# Workflow: Implantação do Chronos Dock em Novo Cliente

Este procedimento padroniza o setup de WhatsApp, CRM e rastreamento CAPI em qualquer cliente da agência em menos de 30 minutos.

---

## Passo 1: Criação do Pipeline no CRM
1. Acesse `CRM > Configurações > Origens` e crie a origem dedicada da campanha (ex: `Nome do Produto / Campanha`).
2. Defina as 9 etapas canônicas com suas cores e eventos Meta:
   - `Novo Lead (WhatsApp)` (`#64748b`) -> Evento: `Lead`
   - `Triagem Concluída` (`#3b82f6`) -> Evento: `SubmitApplication`
   - `Áudio Enviado` (`#8b5cf6`) -> Evento: `Contact`
   - `Vaga Pré-Reservada (PIX)` (`#14b8a6`) -> Evento: `Schedule`
   - `Atendimento Especial` (`#f97316`) -> Evento: `Contact`
   - `Follow-up (Urgência)` (`#eab308`) -> Sem evento
   - `Vaga Confirmada (Ganho)` (`#22c55e`) -> Evento: `Purchase` (Ganho 100%)
   - `Perdido / Desistência` (`#ef4444`) -> Perda (0%)
   - `Lead falso` (`#94a3b8`) -> Perda (`FakeLead_Chronos`)
3. Marque a origem como **Padrão** se for a campanha principal.

---

## Passo 2: Roteamento de Anúncios Click-to-WhatsApp (CTWA)
1. Vá em `Configurações > Rastreamento de anúncios`.
2. Em **Origem padrão para anúncios**, selecione a origem criada.
3. Em **Etapa inicial**, selecione `Novo Lead (WhatsApp)`.
4. Salve para garantir que qualquer lead vindo de tráfego pago caia automaticamente no início da esteira.

---

## Passo 3: Meta Conversions API (CAPI)
1. Obtenha o Pixel ID e a chave de acesso do sistema da Meta (via cofre do cliente).
2. Acesse `Configurações > Conversões Meta` (`/settings/conversoes-meta`).
3. Insira o Pixel ID, o token CAPI e ative o switch de envio.
4. Crie a regra de valor para fechamento:
   - Origem: A origem do cliente
   - Etapa: `Vaga Confirmada (Ganho)`
   - Evento Meta: `Purchase`
   - Moeda: `BRL`
   - Valor: Valor do ticket do cliente (ex: R$ 6.900,00)

---

## Passo 4: Construção do Funil de Triagem
1. Importe o template de 30 blocos (`templates/funil-30-blocos-soultrek.json`).
2. Ajuste o texto das mensagens com base no produto do cliente.
3. Conecte os blocos `move_crm` para as etapas certas.
4. Conecte as saídas `Sem resposta` (timeout 24h) para recuperação.
5. Ative o funil e confirme status verde (0 blocos pendentes).

---

## Passo 5: Conexão do Aparelho & Alertas
1. Gere o link público de conexão único (`/conectar-whatsapp/<token>`).
2. Envie o link para o cliente escanear o QR Code no celular sem precisar de login.
3. Configure o grupo de WhatsApp para onde serão despachados os alertas em tempo real.
