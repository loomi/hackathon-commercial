# Spec: CRM com IA Nativa

## Contexto

O time comercial usa o **Pipedrive** como CRM, mas a ferramenta não conversa com IA. Informações valiosas ficam presas em notas, e-mails e histórico de negociações sem nunca serem analisadas de forma inteligente. O vendedor precisa manualmente vasculhar o Pipedrive para entender o estado de cada deal.

**Hoje sem essa feature:** o vendedor gasta horas relendo histórico de negociações para entender onde cada deal está e qual é o próximo passo mais inteligente.

**Com essa feature:** o vendedor abre o sistema e vê um resumo gerado pela IA de cada deal — o que aconteceu, onde está, e o que fazer agora.

---

## O que fazer

Um sistema que **escuta o Pipedrive em tempo real** e gera resumos inteligentes de cada deal automaticamente.

### Fluxo passo a passo

1. O vendedor adiciona uma nota no Pipedrive: *"Reunião com o diretor de operações. Interessado, mas quer aprovação do financeiro. Pedi retorno até sexta."*

2. No mesmo instante, o Pipedrive envia uma **notificação automática** (chamada de **webhook** — funciona como uma notificação push do celular) para o nosso sistema, avisando que algo mudou naquele deal

3. O nosso sistema recebe essa notificação e guarda num **data lake** — um banco de dados que acumula todo o histórico de interações de cada deal, como um diário completo

4. A **IA (Claude)** lê todo o histórico acumulado daquele deal e gera um resumo automático:
   > *"Deal com Empresa X — estágio: negociação. Última interação: reunião positiva com diretor de operações, pendente aprovação financeira. Próximo passo: cobrar retorno na sexta."*

5. O vendedor abre o nosso sistema e vê a **lista de todos os seus deals com resumos atualizados**, sem precisar entrar no Pipedrive e reler tudo

```
Pipedrive → webhook → nosso sistema → data lake → IA → resumo → tela do vendedor
```

### Glossário rápido

- **Webhook:** quando algo acontece no Pipedrive (nota adicionada, status mudado), ele automaticamente avisa o nosso sistema. O sistema não precisa ficar "perguntando" se algo mudou — ele é avisado na hora.
- **Data lake:** banco de dados que acumula todos os eventos de cada deal. A IA lê esse histórico inteiro para gerar o resumo.

---

## Restrições

- Se o Pipedrive enviar a mesma notificação duas vezes (acontece), o sistema ignora a duplicata
- O resumo deve sempre incluir: **status atual**, **últimas interações** e **próximo passo sugerido**
- A tela deve ser simples — lista de deals com o resumo ao lado, sem necessidade de design elaborado nesta primeira versão
- A IA usada para gerar os resumos é o Claude (modelo Sonnet 4.6)

---

## Critérios de sucesso

- [ ] Quando um deal é atualizado no Pipedrive, o evento aparece no sistema em menos de 5 segundos
- [ ] O resumo do deal é gerado automaticamente após cada novo evento
- [ ] O vendedor consegue ver o resumo de qualquer deal em menos de 10 segundos
- [ ] O resumo inclui: status atual, últimas interações, próximo passo sugerido
- [ ] Se o Pipedrive enviar o mesmo evento duas vezes, o sistema ignora a duplicata

---

## Detalhamento

### Informações que o sistema guarda
- Todo evento que acontece num deal — nota adicionada, mudança de status, e-mail registrado — fica salvo no histórico, com data e conteúdo.
- Para cada deal, o sistema mantém um resumo atualizado: título, contato, estágio atual, o texto do resumo da IA, o próximo passo sugerido e a data da última interação.

### O que dá pra fazer
- Receber automaticamente os avisos do Pipedrive sempre que algo muda num deal.
- Ver a lista de todos os deals com seus resumos, filtrando por estágio (ex.: só os que estão em negociação) ou buscando por nome.
- Abrir um deal específico e ver o resumo completo junto com todo o histórico de interações.

### O que pedimos para a IA
A IA lê todo o histórico do deal e devolve um resumo curto em português, sempre com 3 partes: status atual (em 1 frase), as 3 interações mais recentes resumidas e 1 próximo passo concreto. Linguagem comercial, sem jargão técnico.

### A tela
Uma página de CRM com a lista de deals. Cada linha mostra o deal, o contato, o estágio (com uma cor), o resumo encurtado e a data da última interação. Clicar no deal abre o resumo completo e o histórico.

### Do que depende
- Não depende de nenhuma outra feature — pode ser construída sozinha.
- Alguém precisa configurar o Pipedrive para enviar os avisos ao sistema (o mentor faz essa parte).

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/01-crm-ia-nativa.md por completo. Ele descreve a feature
do ponto de vista de negócio — o problema, o fluxo, as regras e os critérios
de sucesso. Você decide a melhor forma de implementar tecnicamente (banco de
dados, telas, integração com o Pipedrive e com a IA).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Se ficar em dúvida sobre alguma regra de negócio, me pergunte antes de assumir.
```
