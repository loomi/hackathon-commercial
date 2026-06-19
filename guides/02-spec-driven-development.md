# Especifique antes de gerar — Spec-Driven Development

## A ideia central

Antes de pedir para a IA gerar qualquer coisa, escreva uma **especificação clara** do que você quer. Esse conceito se chama **Spec-Driven Development** (desenvolvimento guiado por especificação).

Parece óbvio, mas a maioria das pessoas pula essa etapa — e paga caro em retrabalho.

---

## Por que isso importa com IA?

A IA é muito boa em preencher lacunas. O problema é que quando você deixa lacunas, **ela inventa** — e inventa com base em padrões genéricos, não nas regras do seu negócio.

**Analogia do dia a dia:**

Considere a diferença entre pedir uma peça de marketing assim:

> "Faz uma peça de campanha pra nós."

vs.

> "Cria um e-mail de follow-up para leads que abriram proposta mas não responderam em 3 dias. Tom: direto, sem pressão. CTA: reagendar uma call de 20 minutos. Público: diretores de operações de médias empresas."

O segundo gera algo utilizável de primeira. O primeiro vai render 3 rodadas de revisão.

Com IA é a mesma coisa. **Quanto mais contexto você dá, menos vai precisar corrigir.**

---

## Anatomia de uma boa spec

Uma boa especificação tem 4 partes:

### 1. Contexto
*Quem usa? Qual o problema real?*

> "O time comercial precisa gerar propostas sem depender de um designer. Hoje o processo leva 2 horas; a meta é reduzir para 15 minutos."

### 2. O que fazer
*O resultado esperado — descreva o que o usuário final vai ver e fazer.*

> "Uma página onde o vendedor preenche um formulário com nome do cliente, escopo, valor e prazo. Ao enviar, recebe uma proposta em PDF bonita, pronta para mandar ao cliente."

### 3. Restrições
*O que não pode mudar. Limites, formato, regras de negócio.*

> "O PDF precisa ter a logo da Loomi. Valor mínimo de proposta: R$ 5.000. O vendedor não pode editar a proposta depois de gerada — precisa gerar outra."

### 4. Critérios de sucesso
*Como você vai saber que ficou bom?*

> "O vendedor consegue gerar e baixar a proposta em menos de 3 minutos. O PDF tem logo, dados do cliente e tabela de investimento. Funciona no Chrome e Safari."

---

## Exemplos lado a lado

### Feature: Gerador de proposta

**Prompt vago:**
```
Cria um gerador de proposta comercial.
```

**Prompt com spec:**
```
Contexto: vendedores da Loomi perdem 2h montando propostas manualmente 
no Word. Preciso automatizar isso.

O que fazer: uma página com formulário onde o vendedor preenche nome do 
cliente, CNPJ, escopo do projeto (texto livre), valor em R$ e prazo em 
dias. Ao enviar, o sistema gera um PDF profissional com logo da Loomi 
no cabeçalho, dados do cliente, escopo, tabela de investimento e rodapé 
com contatos.

Restrições: valor mínimo da proposta é R$ 5.000. O PDF não pode ter mais 
de 3 páginas. Usar as cores da Loomi (#1A1A2E e #E94560).

Critério de sucesso: vendedor preenche em menos de 3 minutos e faz 
download de um PDF que pode enviar direto ao cliente.
```

O segundo prompt gera algo funcional de primeira. O primeiro exige 5+ rodadas de "não era isso".

---

### Feature: Follow-up automático

**Prompt vago:**
```
Faz um sistema de follow-up de leads.
```

**Prompt com spec:**
```
Contexto: vendedores esquecem de retornar para leads que não responderam 
após o envio da proposta. Leads quentes esfriam.

O que fazer: o sistema verifica automaticamente todo dia de manhã quais 
leads receberam proposta há mais de 3 dias e não responderam. Para cada 
um, envia um e-mail de follow-up personalizado e avisa o vendedor 
responsável.

Restrições: máximo 1 follow-up por lead por semana (não pode virar 
spam). Se o lead já respondeu ou o deal já fechou, não envia nada.

Critério de sucesso: nenhum lead fica sem follow-up por mais de 4 dias. 
O vendedor recebe um resumo do que foi enviado automaticamente.
```

---

## Template reutilizável

Copie e adapte para cada feature que você for construir:

```
Contexto:
[Quem vai usar? Qual é a dor? O que acontece hoje sem essa feature?]

O que fazer:
[Descreva o resultado final. O que o usuário vê? O que ele faz? O que 
acontece quando ele interage?]

Restrições:
- [Regra de negócio 1]
- [Regra de negócio 2]
- [Limitação técnica, se souber]

Critérios de sucesso:
- [ ] [Comportamento 1 que deve funcionar]
- [ ] [Comportamento 2 que deve funcionar]
- [ ] [Métrica, se houver — ex: "em menos de 2 minutos"]
```

> **Dica:** os arquivos dentro da pasta `specs/` já usam esse template. Consulte-os antes de pedir qualquer coisa ao Claude — eles têm a spec pronta para cada feature do hackathon.

---

## Como usar specs com o Claude Code

1. **Abra o arquivo da feature** em `specs/` que você quer construir
2. **Peça ao Claude para ler** o arquivo:
   > "Leia o arquivo specs/02-gerador-de-proposta.md e implemente a feature."
3. **Ou copie a spec** e cole direto no chat do Claude
4. **Valide** contra os critérios de sucesso antes de considerar concluído

---

## Resumo

| Sem spec | Com spec |
|----------|----------|
| Resultado genérico | Resultado alinhado ao negócio |
| Muitas rodadas de revisão | Funciona de primeira (ou quase) |
| IA "inventa" regras | IA segue suas regras |
| Frustração | Velocidade |

**Uma boa spec é um investimento de 5 minutos que economiza 1 hora de retrabalho.**
