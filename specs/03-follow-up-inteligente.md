# Spec: Follow-up Inteligente

## Contexto

O time comercial perde negócios por **falta de follow-up no momento certo**. Após enviar uma proposta, o vendedor depende da própria memória (ou de sticky notes) para lembrar de retornar ao cliente. Com muitos deals em paralelo, inevitavelmente alguns caem no esquecimento.

**Hoje sem essa feature:** leads quentes esfriando por falta de contato no momento certo. Negócios perdidos por puro esquecimento.

**Com essa feature:** o sistema envia follow-ups automáticos com e-mails personalizados pela IA, e o vendedor acompanha tudo numa lista.

---

## O que fazer

Um sistema que **automatiza o follow-up** com cadência inteligente — e-mails personalizados pela IA, enviados automaticamente nos intervalos certos, sem que o vendedor precise lembrar de nada.

### Fluxo passo a passo

1. O vendedor envia uma proposta para o cliente (via o Gerador de Proposta ou manualmente)

2. O sistema automaticamente inicia uma **cadência de follow-up** para aquele deal:
   - **D+3** (3 dias depois): se o cliente não respondeu, o sistema envia o primeiro e-mail de follow-up
   - **D+7**: segundo follow-up
   - **D+14**: terceiro e último follow-up automático

3. Todo dia de manhã às 8h, uma **tarefa agendada** (que roda sozinha, como um despertador do sistema) verifica quais deals precisam de follow-up naquele dia

4. Para cada deal identificado, o sistema:
   - Usa a **IA (Claude)** para escrever um e-mail personalizado, baseado no histórico do deal e no escopo da proposta — cada e-mail é diferente, nada de template genérico
   - Envia o e-mail para o contato do cliente
   - Registra o follow-up no histórico
   - Avisa o vendedor responsável

5. O vendedor acessa uma **lista de acompanhamento** onde vê todos os seus deals com:
   - Status da cadência: em andamento / cliente respondeu / 3 follow-ups enviados sem resposta
   - Quando será o próximo follow-up
   - Histórico de todos os e-mails já enviados

### O papel da IA aqui

Em vez de um template genérico tipo *"Olá, estou passando para saber se teve a oportunidade de analisar nossa proposta"*, a IA escreve algo contextualizado:

> *"Olá Ricardo, na semana passada conversamos sobre o projeto de automação do backoffice para a Empresa X. Sei que vocês estavam validando internamente com o time financeiro. Caso já tenham algum retorno, ficaria feliz em agendar 20 minutos para alinharmos os próximos passos."*

---

## Restrições

- Máximo 1 follow-up por deal por semana — nunca virar spam
- Não enviar se o deal já foi marcado como ganho ou perdido
- O vendedor pode pausar a cadência de um deal específico (ex: o cliente pediu pra retornar só mês que vem)
- Finais de semana: não envia. Se caiu no sábado/domingo, compensa na segunda-feira
- Após os 3 follow-ups automáticos, o deal fica marcado como "precisa de atenção humana" na lista
- O vendedor recebe um resumo matinal dos follow-ups enviados naquele dia

---

## Critérios de sucesso

- [ ] Nenhum deal com proposta enviada fica sem follow-up por mais de 4 dias
- [ ] Os e-mails gerados parecem escritos por um humano — não genéricos
- [ ] O vendedor recebe um resumo matinal dos follow-ups enviados no dia
- [ ] A lista de acompanhamento mostra claramente quais deals precisam de atenção humana
- [ ] O vendedor consegue pausar a cadência de um deal específico com um clique

---

## Detalhamento

### Informações que o sistema guarda
- Para cada deal em acompanhamento: o contato (nome e e-mail), o status da cadência (em andamento, pausada, cliente respondeu, esgotada), em qual etapa está (nenhuma, 1ª, 2ª ou 3ª) e quando será o próximo follow-up.
- Para cada e-mail enviado: a etapa, o assunto, o corpo do e-mail gerado pela IA e a data de envio — formando o histórico do deal.

### Como funciona a cadência
- Quando uma proposta é enviada, o sistema agenda o primeiro follow-up para 3 dias úteis depois.
- As etapas seguem o ritmo D+3, D+7 e D+14, sempre contando a partir do envio da proposta.
- Sábados e domingos não contam: se uma data cair no fim de semana, ela é empurrada para a segunda-feira.
- Depois do 3º follow-up sem resposta, a cadência é encerrada e o deal entra na lista de "precisa de atenção humana".

### O que dá pra fazer
- Acompanhar todos os follow-ups numa lista, filtrando por status.
- Pausar a cadência de um deal e retomá-la depois (ao retomar, o próximo follow-up é recalculado a partir do dia).
- Marcar um deal como "cliente respondeu" para parar os envios.
- Criar um follow-up manualmente, caso a proposta não tenha vindo do Gerador de Proposta.

### O que pedimos para a IA
Escrever um e-mail de follow-up pessoal, direto e respeitoso — que nunca pareça um robô, nunca pressione o cliente e sempre ofereça um próximo passo concreto (uma call curta, por exemplo). A IA usa o nome do contato, a empresa, o escopo da proposta, a data de envio e os follow-ups anteriores. O corpo deve ter no máximo 150 palavras.

### A tela
Uma lista de cards, um por deal. Cada card mostra cliente, escopo resumido, status (com cor), data do próximo follow-up e em qual etapa está (1/3, 2/3, 3/3), com botões para pausar/retomar e marcar como respondido. Expandir o card mostra o histórico de e-mails enviados.

### Do que depende
- **Depende do** Gerador de Proposta (spec 02) — a cadência começa quando uma proposta é enviada.
- Se a spec 02 ainda não existir, dá pra começar criando o follow-up manualmente, informando os dados do deal.

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/03-follow-up-inteligente.md por completo. Ele descreve a
feature do ponto de vista de negócio — o problema, o fluxo, as regras de
cadência e os critérios de sucesso. Você decide a melhor forma de implementar
tecnicamente (banco de dados, telas, tarefa agendada, envio de e-mail e IA).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Se ficar em dúvida sobre alguma regra de negócio, me pergunte antes de assumir.
```
