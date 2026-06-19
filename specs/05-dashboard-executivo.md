# Spec: Dashboard Executivo

## Contexto

A liderança comercial não tem visibilidade em tempo real do pipeline. Os dados existem no Pipedrive e em planilhas, mas nunca estão consolidados num lugar só. Reuniões de resultados dependem de alguém preparar uma apresentação manual toda semana.

**Hoje sem essa feature:** decisões comerciais baseadas em dados desatualizados ou incompletos. Ninguém sabe responder "como está o pipeline?" sem abrir 3 ferramentas diferentes.

**Com essa feature:** a liderança abre uma única tela e vê pipeline, receita, margem, pagamentos e um resumo executivo gerado pela IA — tudo atualizado automaticamente.

---

## O que fazer

Um **painel visual em tempo real** (dashboard) com os números que importam para a liderança comercial — sem planilha, sem preparação manual, tudo num só lugar.

### O que o dashboard mostra

O dashboard é dividido em **5 painéis**, cada um respondendo uma pergunta da liderança:

#### 1. "Como está o pipeline?" — Visão do Pipeline
- Total de deals em cada estágio: prospecção, proposta enviada, negociação, fechado
- Valor total (R$) em cada estágio — quanto dinheiro está "parado" em cada fase
- Taxa de conversão entre estágios — de cada 10 deals em prospecção, quantos viram proposta? De cada 10 propostas, quantas fecham?

#### 2. "Vamos bater a meta?" — Business Case
- **Receita prevista:** soma dos deals em negociação, ponderada pela probabilidade de fechar (um deal de R$ 100k com 50% de chance = R$ 50k previsto)
- **Receita confirmada:** contratos já assinados
- **Meta vs. realizado:** barra de progresso mostrando quanto falta para bater a meta do mês/trimestre

#### 3. "Os projetos estão dando lucro?" — Margem Real vs. Vendida
- Para cada projeto em andamento: a margem que foi estimada na venda comparada com a margem real (quanto de fato está custando)
- **Alertas automáticos** quando um projeto começa a custar mais que o previsto (margem caindo)

#### 4. "Como estão os pagamentos?" — Status de Pagamentos
- Faturas emitidas e pendentes de pagamento
- Faturas em atraso (vencidas) — destaque visual
- Previsão de caixa: quanto entra nos próximos 30, 60 e 90 dias

#### 5. "Resumo executivo" — Resumo IA
- Um parágrafo gerado pela IA (Claude) resumindo o estado geral da operação comercial:
  - O que está indo bem
  - O que precisa de atenção urgente
  - Recomendação de próximo passo

Exemplo de resumo gerado:
> *"Pipeline saudável com R$ 2.3M em negociação. A taxa de conversão de proposta para fechamento caiu 8% este mês — investigar. Dois projetos (Empresa X e Empresa Y) estão com margem 12% abaixo do estimado. Ação recomendada: revisar escopo com os PMs antes da próxima entrega."*

---

## Restrições

- Os dados devem estar atualizados com no máximo 15 minutos de atraso
- Apenas gestores e admins visualizam o dashboard (vendedores individuais não)
- O sistema usa os dados que já existem no banco (propostas, cálculos de margem, dados do Pipedrive) — não precisa digitar nada novo
- O resumo da IA deve ser contextualizado com os números reais, não genérico
- Os gráficos devem ser simples e legíveis — nada de dashboard poluído com 20 indicadores

---

## Critérios de sucesso

- [ ] Liderança consegue ver o estado do pipeline sem abrir o Pipedrive
- [ ] Os números de receita prevista vs. realizada estão corretos e atualizados
- [ ] Alertas de projetos com margem caindo aparecem automaticamente
- [ ] O resumo da IA é útil e contextualizado (menciona projetos e números reais, não frases genéricas)
- [ ] Dashboard carrega em menos de 3 segundos

---

## Detalhamento

### De onde vêm os números
O dashboard não pede pra ninguém digitar nada — ele reaproveita o que já existe no sistema:
- **Pipeline e conversões:** dos deals do CRM (spec 01) e das propostas (spec 02).
- **Receita prevista e confirmada:** dos deals em negociação (ponderados pela chance de fechar) e dos contratos já assinados.
- **Margem real vs. vendida:** dos cálculos de margem (spec 04). Um projeto vira alerta quando a margem cai mais de 5 pontos em relação ao estimado.
- **Pagamentos:** das faturas — quais estão pendentes, quais estão vencidas e a previsão de entrada para os próximos 30, 60 e 90 dias.

### Informações que o sistema guarda
- **Metas:** a meta de receita mensal e a trimestral (configuráveis pelo admin).
- **Faturas:** cliente, valor, data de vencimento, data de pagamento e status (pendente, paga, vencida).

### O que dá pra fazer
- Ver os 5 painéis sempre atualizados (no máximo 15 minutos de atraso).
- Pedir uma nova versão do resumo da IA quando quiser.
- O admin pode atualizar as metas mensal e trimestral.

### O que pedimos para a IA
Gerar um resumo executivo curto (até ~100 palavras), em português, sobre o estado da operação comercial: o que vai bem, o que precisa de atenção e uma recomendação de ação. O resumo precisa citar números concretos vindos dos outros painéis — nada genérico.

### A tela
Uma página de dashboard (só para gestores e admins), com os painéis organizados num grid: pipeline (gráfico de barras por estágio), business case (números grandes + barra de progresso da meta), margem (tabela com semáforo por projeto), pagamentos (pendentes, vencidas e previsão de caixa) e o resumo da IA num bloco que ocupa a largura toda, com a data da última atualização e um botão "Atualizar resumo". A tela se atualiza sozinha a cada 15 minutos.

### Do que depende
- **Usa dados de:** CRM (spec 01), Gerador de Proposta (spec 02) e Calculadora de Margem (spec 04).
- O dashboard funciona com dados parciais — se uma feature ainda não existir, o painel correspondente mostra "Sem dados disponíveis".
- A parte de pagamentos é independente das outras specs e pode começar com dados de exemplo para teste.

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/05-dashboard-executivo.md por completo. Ele descreve a
feature do ponto de vista de negócio — os 5 painéis, de onde vêm os números,
as regras e os critérios de sucesso. Você decide a melhor forma de implementar
tecnicamente (banco de dados, telas, gráficos, agregação dos dados e IA).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Se ficar em dúvida sobre alguma regra de negócio, me pergunte antes de assumir.
```
