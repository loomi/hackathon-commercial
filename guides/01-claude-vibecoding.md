# Claude Code e Vibecoding

## O que é o Claude Code?

O **Claude Code** é um assistente de IA que funciona dentro do seu editor de código (VSCode). Diferente de um chatbot comum, ele consegue **ler, criar e modificar arquivos reais do seu projeto** — não apenas conversar.

Na prática, ele atua como um desenvolvedor que entende o contexto do projeto e executa o que você pede **em português**.

---

## O que é Vibecoding?

**Vibecoding** é construir software descrevendo o que você quer — sem precisar escrever código na mão.

Em vez de abrir arquivo, localizar função, digitar lógica, formatar e salvar, você descreve o objetivo:

> "Cria uma página de geração de propostas comerciais. O vendedor preenche nome do cliente, escopo do projeto, valor e prazo. Quando ele enviar, o sistema salva e gera um PDF para download."

O Claude entende, gera o código e aplica as mudanças. **Você revisa e aprova.**

O papel de quem usa esse fluxo é **direcionar, revisar e iterar**. A responsabilidade pelo produto continua sendo sua.

---

## Os modelos do Claude — qual usar?

O Claude tem versões diferentes, cada uma com um equilíbrio próprio entre velocidade e capacidade:

| Modelo | Velocidade | Capacidade | Quando usar |
|--------|-----------|------------|-------------|
| **Opus 4.6** | Mais lento | O mais inteligente | Quando uma tarefa complexa não sai do jeito certo com o Sonnet |
| **Sonnet 4.6** | Rápido | Muito capaz | O padrão — use para tudo no dia a dia |
| **Haiku 4.5** | Muito rápido | Mais simples | Perguntas rápidas tipo "o que esse trecho faz?" |

> **Dica:** no hackathon, use o **Sonnet 4.6** para quase tudo. Se ele travar em algo complexo, mude para o Opus.

Para trocar de modelo no Claude Code, digite `/model` no chat e escolha.

---

## Boas práticas de prompt

### 1. Seja específico — dê contexto real

**Prompt vago:**
> "Cria uma tela de proposta."

**Prompt com contexto:**
> "Cria uma página de geração de proposta comercial. O vendedor preenche: nome do cliente, escopo do projeto (texto livre), valor estimado em R$ e prazo de entrega. Quando enviar, os dados são salvos e o sistema gera um PDF profissional pro vendedor baixar."

O segundo economiza várias rodadas de correção.

---

### 2. Use as skills do projeto

Este projeto tem **comandos prontos** (chamados de *skills*) que já sabem como o código está organizado. É como um atalho: em vez de explicar tudo do zero, o comando já carrega as instruções.

Para usar, basta digitar no chat do Claude:

```
/back-add-feature criar módulo de propostas
/front-design página de geração de proposta com formulário bonito
```

> **Dica:** a lista completa de skills está no [README.md](../README.md) do projeto.

---

### 3. Itere — peça em passos, não tudo de uma vez

Construa de forma incremental:

1. Peça uma versão inicial
2. Veja o resultado
3. Peça ajustes: *"Ficou bom, mas adiciona um campo de CNPJ e muda a cor do botão pra azul"*
4. Repita até ficar certo

**Não tente resolver tudo em um prompt gigante.** Divida em etapas.

---

### 4. Para tarefas grandes, peça um plano antes

Se a mudança é grande (ex: criar um módulo inteiro), peça ao Claude para **planejar antes de executar**:

> "Me mostra um plano de como você faria a integração com o Pipedrive antes de escrever qualquer código."

Isso evita surpresas e te deixa validar a abordagem antes de ele começar.

---

### 5. Revise antes de aprovar

O Claude mostra o que vai fazer antes de aplicar. **Leia.** Não porque ele erra muito — mas porque ele não sabe as regras de negócio da sua área. Você pega coisas que ele não tem como saber.

---

### 6. Se ele errou, diga o que está errado

O Claude não acerta 100% das vezes. Quando algo sair diferente do esperado, **não comece de novo** — corrija:

> "O formulário ficou bom, mas o campo de valor está aceitando números negativos. Corrige pra aceitar só valores acima de R$ 1.000."

> "O PDF tá sem a logo da Loomi no cabeçalho. Adiciona."

Quanto mais específico o feedback, mais rápido ele corrige.

---

## O que evitar

| Comportamento | Por quê é problema |
|--------------|-------------------|
| Prompts vagos ("faz uma tela de vendas") | Resultado genérico, muito retrabalho |
| Aprovar sem ler o que ele fez | Pode ter lógica errada pro seu contexto |
| Pedir tudo de uma vez | O Claude perde o fio; melhor em etapas |
| Não dar contexto de negócio | Ele não sabe as regras da sua empresa |
| Desistir no primeiro erro | Corrija com um prompt específico — é mais rápido que recomeçar |

---

## Ordem sugerida de implementação

As features se conectam entre si. Comece pelas independentes e vá avançando:

```
1.  specs/02 — Gerador de Proposta         (independente — base para várias outras)
2.  specs/04 — Calculadora de Margem       (se integra à proposta)
3.  specs/03 — Follow-up Inteligente       (dispara após enviar proposta)
4.  specs/01 — CRM com IA Nativa           (independente — pode ser em paralelo)
5.  specs/06 — Governança de Parcerias     (independente — pode ser em paralelo)
6.  specs/05 — Dashboard Executivo         (usa dados das outras — faça por último)
```

> **Dica:** o Dashboard (05) funciona mesmo com dados parciais — se uma feature ainda não estiver pronta, o painel correspondente mostra "Sem dados disponíveis".

---

## Fluxo recomendado no hackathon

```
1. Identifique a dor que quer resolver
       ↓
2. Abra o arquivo da feature em specs/ — ele tem a spec pronta
       ↓
3. Copie a spec pro Claude ou peça pra ele ler o arquivo
       ↓
4. Peça um plano se a tarefa for grande
       ↓
5. Revise e aprove a execução
       ↓
6. Teste o resultado
       ↓
7. Ajuste com prompts de refinamento
       ↓
8. Repita até ficar pronto
```

---

## Resumo rápido

- Claude Code **cria e modifica arquivos reais** no seu projeto — não é só chat
- Vibecoding é **direcionar + revisar**, não sumir
- Prompts **específicos e com contexto** economizam tempo
- Use as **skills** do projeto (comandos que começam com `/`)
- **Itere** em passos pequenos em vez de um mega-prompt
- Sempre **revise** antes de aprovar
- Se errou, **corrija com feedback específico** — não comece do zero
