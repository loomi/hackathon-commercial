# Spec: Calculadora de Margem

## Contexto

Antes de fechar um contrato, o time comercial precisa saber se o negócio é **financeiramente saudável** para a Loomi. Hoje esse cálculo é feito no Excel — e muitas vezes não é feito, resultando em projetos vendidos abaixo do custo real.

**Hoje sem essa feature:** vendedores fecham contratos sem saber a margem real, descobrindo o problema só depois que o projeto começou.

**Com essa feature:** antes de gerar a proposta, o vendedor vê exatamente quanto o projeto vai custar, qual é a margem esperada, e um semáforo visual indicando se o negócio é saudável.

---

## O que fazer

Uma **calculadora integrada à tela de criação de proposta** que mostra em tempo real quanto o projeto vai custar e qual é a margem esperada — com um semáforo visual que muda conforme o vendedor ajusta os valores.

### Fluxo passo a passo

1. Na mesma página onde o vendedor cria a proposta, ele encontra uma seção de **"Calculadora de Margem"**

2. Preenche os **custos estimados**:
   - Horas por perfil de profissional (ex: tech lead: 40h, dev sênior: 80h, designer: 20h)
   - Custo/hora de cada perfil (já vem preenchido com os valores padrão configurados pelo admin)
   - Impostos aplicáveis (ISS, PIS, COFINS — o % já vem configurado)
   - Outros custos diretos: ferramentas, infraestrutura, subcontratados

3. O sistema **calcula automaticamente** e mostra na tela:
   - **Custo total** = soma de todas as horas x custo/hora + outros custos
   - **Custo com impostos** = custo total + impostos
   - **Margem bruta** = quanto sobra do valor vendido após pagar todos os custos (em %)
   - **Ponto de equilíbrio** = o valor mínimo que você pode vender sem ter prejuízo

4. Exibe um **semáforo visual** que muda em tempo real conforme o vendedor ajusta o valor:
   - Verde: margem acima de 30% — negócio saudável
   - Amarelo: margem entre 15% e 30% — atenção
   - Vermelho: margem abaixo de 15% — risco de prejuízo

5. Quando o vendedor ajusta o valor da proposta, **os cálculos atualizam na hora** — sem precisar clicar em nada. Dá pra simular cenários rapidamente.

6. O cálculo de margem fica **salvo junto com a proposta**, criando um histórico de margem por projeto

7. **Futuro (opcional):** ao final do projeto, comparar a margem que foi estimada na venda com a margem real (quanto de fato custou vs. quanto foi cobrado)

### Por que não usar o Excel?
Porque o Excel vive separado do sistema. O vendedor esquece de preencher, ou preenche com dados desatualizados. Com a calculadora integrada, ele vê a margem **antes** de gerar a proposta, usando os custos reais configurados pelo admin.

---

## Restrições

- Os custos/hora por perfil e os % de impostos são configuráveis pelo admin — o vendedor usa os valores que já vêm preenchidos, mas o admin pode atualizá-los quando mudar
- A calculadora fica embutida na página de proposta, não numa página separada — o vendedor vê margem e proposta lado a lado
- Os cálculos são feitos no servidor (não só no navegador) para garantir precisão nos valores monetários
- As faixas do semáforo (30% / 15%) começam fixas nesta versão

---

## Critérios de sucesso

- [ ] O vendedor consegue calcular a margem em menos de 2 minutos preenchendo o formulário
- [ ] O semáforo visual está correto conforme as faixas de margem definidas
- [ ] Ao alterar o valor da proposta, os cálculos atualizam em tempo real (sem recarregar a página)
- [ ] Um admin consegue atualizar o custo/hora de cada perfil sem precisar de um desenvolvedor
- [ ] O cálculo de margem fica salvo junto com a proposta para consulta futura

---

## Detalhamento

### Informações que o sistema guarda
- **Tabela de custos por perfil** (configurada pelo admin): Tech Lead, Dev Sênior, Dev Pleno, Designer, PM — cada um com seu custo/hora.
- **Tabela de impostos** (configurada pelo admin): ISS, PIS, COFINS — cada um com seu percentual.
- **O cálculo de cada proposta:** as horas e o custo/hora de cada perfil, os outros custos, o valor de venda, e os resultados (custo total, impostos, custo com impostos, margem em %, ponto de equilíbrio e a cor do semáforo).

### Valores iniciais sugeridos
- Perfis: Tech Lead (R$ 180/h), Dev Sênior (R$ 150/h), Dev Pleno (R$ 110/h), Designer (R$ 120/h), PM (R$ 140/h)
- Impostos: ISS (5%), PIS (1,65%), COFINS (7,6%)

### Como o cálculo funciona (em linguagem simples)
- **Custo total** = soma de (horas × custo/hora de cada perfil) + outros custos.
- **Impostos** = custo total multiplicado pela soma dos percentuais de imposto.
- **Custo com impostos** = custo total + impostos.
- **Margem (%)** = quanto sobra do valor de venda depois de pagar o custo com impostos, dividido pelo valor de venda.
- **Ponto de equilíbrio** = o custo com impostos (vender abaixo disso dá prejuízo).
- **Semáforo:** verde acima de 30%, amarelo entre 15% e 30%, vermelho abaixo de 15%.

### O que dá pra fazer
- Calcular a margem em tempo real enquanto preenche, sem salvar nada (modo simulação).
- Salvar o cálculo vinculado a uma proposta.
- O admin pode atualizar o custo/hora de cada perfil e o percentual de cada imposto.

### As telas
- **Dentro da página de Nova Proposta:** uma tabela editável de perfis (perfil, horas, custo/hora, subtotal), um campo de "outros custos" e os totais recalculados na hora conforme o vendedor digita. Um semáforo grande e visível mostra a margem em % e o rótulo (Saudável / Atenção / Risco).
- **Página de configuração (admin):** tabela de perfis com custo/hora editável e tabela de impostos com percentual editável.

### Do que depende
- **Depende do** Gerador de Proposta (spec 02) — a calculadora fica embutida na mesma página e o cálculo é salvo junto com a proposta.
- Se a spec 02 ainda não existir, pode ser feita como uma página separada por enquanto.

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/04-calculadora-de-margem.md por completo. Ele descreve a
feature do ponto de vista de negócio — o problema, o fluxo, as fórmulas em
linguagem simples e os critérios de sucesso. Você decide a melhor forma de
implementar tecnicamente (banco de dados, telas, cálculos no servidor).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Garanta precisão nos valores monetários. Se ficar em dúvida sobre alguma regra
de negócio, me pergunte antes de assumir.
```
