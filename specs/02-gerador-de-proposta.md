# Spec: Gerador de Proposta

## Contexto

Montar uma proposta comercial hoje leva em média **2 horas**: o vendedor abre um template no Word/Google Docs, preenche os dados do cliente, ajusta o escopo manualmente, pede ajuda ao time de design para deixar bonito, e só então envia.

**Hoje sem essa feature:** processo manual, demorado e inconsistente. Cada proposta fica diferente dependendo de quem fez.

**Com essa feature:** vendedor preenche um formulário em 3 minutos e recebe uma proposta profissional em PDF pronta para enviar ao cliente.

---

## O que fazer

Uma página web onde o vendedor preenche um formulário curto e recebe uma **proposta profissional em PDF** — com o texto do escopo enriquecido pela IA.

### Fluxo passo a passo

1. O vendedor acessa a página de "Nova Proposta" no sistema

2. Preenche um **formulário** com:
   - Nome do cliente / empresa
   - CNPJ (opcional)
   - Escopo do projeto (texto livre — descreve em 2-3 parágrafos o que será feito)
   - Valor total estimado (R$)
   - Prazo de entrega (em dias ou data)
   - Nome do responsável comercial (quem está vendendo)

3. Clica em "Gerar proposta"

4. O sistema faz duas coisas nos bastidores:
   - **Salva os dados** no banco de dados (para consulta futura)
   - **Envia o texto do escopo para a IA (Claude)**, que reescreve com linguagem mais profissional e gera uma seção de "benefícios esperados"

5. Gera um **PDF profissional** com:
   - Logo da Loomi no cabeçalho
   - Dados do cliente
   - Escopo enriquecido pela IA (mais elaborado que o texto digitado)
   - Tabela de investimento (valor + prazo)
   - Assinatura do responsável comercial
   - Rodapé com contatos da Loomi

6. O vendedor **faz download do PDF** direto da tela e envia ao cliente

### O papel da IA aqui

O vendedor escreve o escopo de forma direta: *"Vamos desenvolver um app mobile para gestão de frota com rastreamento em tempo real."*

A IA transforma em algo mais comercial: *"Desenvolvimento de aplicativo mobile nativo para gestão inteligente de frotas, incluindo rastreamento em tempo real via GPS, painel de indicadores operacionais e alertas configuráveis para otimização de rotas e redução de custos operacionais."*

Além disso, gera uma seção de benefícios: *"Com esta solução, sua empresa poderá reduzir custos operacionais de logística, aumentar a visibilidade da operação em campo e tomar decisões baseadas em dados em tempo real."*

---

## Restrições

- Campos obrigatórios: nome do cliente, escopo, valor e prazo
- CNPJ é opcional (nem todo lead tem CNPJ na hora da proposta)
- O PDF deve ter entre 1 e 3 páginas — proposta enxuta, não um documento de 20 páginas
- Qualquer vendedor logado pode gerar propostas (sem aprovação de gestor nesta versão)
- Valores monetários precisam ser armazenados com precisão (sem arredondamentos estranhos)

---

## Critérios de sucesso

- [ ] Vendedor preenche o formulário em menos de 3 minutos
- [ ] PDF é gerado em menos de 30 segundos após enviar o formulário
- [ ] PDF baixado tem logo, dados do cliente, escopo, valor e prazo
- [ ] O texto do escopo no PDF é mais elaborado que o texto digitado (enriquecido pela IA)
- [ ] Funciona no Chrome e Safari

---

## Detalhamento

### Informações que o sistema guarda
Para cada proposta: nome do cliente, CNPJ (quando houver), o escopo original digitado pelo vendedor, a versão enriquecida pela IA, a seção de benefícios, o valor total, o prazo, o nome do responsável comercial, o arquivo PDF gerado e o status (rascunho, gerada, enviada).

### O que dá pra fazer
- Criar uma proposta nova a partir do formulário.
- Ver a lista de todas as propostas já criadas, da mais recente para a mais antiga.
- Abrir uma proposta específica e rever todos os dados.
- Baixar o PDF de qualquer proposta quando quiser.

### O que pedimos para a IA
Duas coisas, a partir do escopo digitado: (1) reescrever o escopo com linguagem comercial e profissional, sem inventar funcionalidades que não foram mencionadas, em no máximo 3 parágrafos; e (2) gerar uma seção "Benefícios Esperados" com 3 a 5 pontos, focados em resultado de negócio (economia, eficiência, visibilidade) e não em tecnologia.

### As telas
- **Nova proposta:** o formulário com os 6 campos, validação dos obrigatórios e um botão "Gerar proposta" com indicação de carregamento. Ao terminar, mostra um preview e o botão "Baixar PDF".
- **Lista de propostas:** tabela com cliente, valor, prazo, status e data. Clicar abre o detalhe com a opção de baixar o PDF de novo.

### Do que depende
- Não depende obrigatoriamente de nenhuma outra feature.
- A Calculadora de Margem (spec 04) se conecta opcionalmente a esta página — as duas podem aparecer lado a lado.

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/02-gerador-de-proposta.md por completo. Ele descreve a
feature do ponto de vista de negócio — o problema, o fluxo, as regras e os
critérios de sucesso. Você decide a melhor forma de implementar tecnicamente
(banco de dados, telas, geração do PDF e uso da IA).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Se ficar em dúvida sobre alguma regra de negócio, me pergunte antes de assumir.
```
