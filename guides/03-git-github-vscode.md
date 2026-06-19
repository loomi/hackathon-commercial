# Git, GitHub e VSCode — o essencial

## Git

### O que é?

**Git** é um sistema que **guarda o histórico de tudo que foi alterado no projeto**. Se algo quebrar, você volta atrás. Se quiser ver o que mudou ontem, ele mostra.

Funciona como o histórico de versões de um Google Docs — mas para o projeto inteiro, não só um arquivo.

### Dois conceitos para memorizar

| Conceito | O que é | Analogia |
|----------|---------|---------|
| **Repositório** | A pasta do projeto com todo o histórico | A pasta do Google Drive com todos os arquivos |
| **Commit** | Um "save" com mensagem explicando o que mudou | Salvar versão do documento com o nome: "adicionei formulário de proposta" |

É basicamente isso. O resto você vai absorvendo com o uso.

---

### A árvore de commits — o histórico visual

Cada commit nasce a partir do anterior, formando uma **linha do tempo**. Esse encadeamento de versões é o que chamamos de **árvore de commits**: o histórico completo do projeto, do início até agora.

```
   ● a7f3c9   "gera PDF da proposta"             ◀── você está aqui (mais recente)
   │
   ● c2b8e1   "adiciona calculadora de margem"
   │
   ● 9d4f0a   "adiciona formulário de proposta"
   │
   ● 1e6b7d   "estrutura inicial do projeto"     ◀── primeiro commit (mais antigo)
```

Como ler o desenho:

- Cada **●** é um commit — um "save" com descrição.
- O **código** ao lado (ex: `a7f3c9`) é o identificador único daquele commit.
- A **linha vertical** que liga os pontos é o histórico: toda versão parte da anterior.
- **Voltar atrás** é simplesmente retornar a um ponto mais abaixo da linha.

A cada commit, a árvore cresce **um ponto para cima** — é assim que o projeto vai ganhando histórico, do começo até a versão atual.

> No VSCode, você não desenha isso à mão: a aba **Source Control** (e a extensão **Git Graph**, se instalada) mostra essa árvore pronta e clicável.

---

### Comandos essenciais

Neste primeiro momento, o trabalho é **local** — tudo acontece na sua máquina. São só 3 comandos no terminal do VSCode:

```bash
# 1. Ver o que mudou no seu projeto
git status

# 2. Marcar todas as mudanças para salvar
git add .

# 3. Salvar com uma mensagem (o "commit") — fica guardado na sua máquina
git commit -m "adiciona formulário de proposta comercial"
```

> **Dica:** não é necessário decorar esses comandos. O Claude Code faz commits por você. Basta pedir: *"Faz o commit das mudanças atuais"* ou usar o comando `/conventional-commit`.

---

### Fluxo do dia a dia no hackathon

```
1. [trabalha com o Claude Code]
2. git add .             → marca as mudanças
3. git commit -m "..."   → salva na sua máquina, com mensagem
```

Repita esse ciclo a cada parte concluída. Cada commit é um ponto novo na sua árvore de histórico.

Ou, mais fácil ainda: **faz tudo pelo VSCode sem digitar nenhum comando** (ver seção abaixo).

---

### Branches — trabalhar sem quebrar o que já funciona

Até agora a árvore cresceu numa **linha única**. Mas e se você quer construir uma feature nova sem correr o risco de bagunçar a versão que já está funcionando? É para isso que existe a **branch** (em português, "ramo").

Uma branch é uma **linha de trabalho paralela**: ela parte da versão principal, você faz seus commits ali isolado, e quando a feature está pronta você **junta de volta** (isso se chama *merge*).

**Analogia:** é como duplicar um documento do Google Docs para escrever uma versão grande sem mexer no original. Se der certo, você substitui o original pela versão nova. Se der errado, é só descartar a cópia — o original nunca foi tocado.

```
                   ●──● f1a2  "calculadora pronta"      ◀── branch: feature/calculadora
                  /
●──●──●──●──●──●                                        ◀── main (versão principal, sempre estável)
                  \
                   ●──● b3c4  "dashboard em progresso"  ◀── branch: feature/dashboard
```

- A linha do meio é a **`main`** — a versão principal, que deve estar sempre funcionando.
- Cada **desvio** é uma branch com uma feature sendo construída em paralelo, sem afetar a `main`.
- Quando a feature fica pronta, ela é **juntada (merge)** de volta na `main`.

| Conceito | O que é | Analogia |
|----------|---------|---------|
| **`main`** | A linha principal, a versão "oficial" do projeto | O documento original |
| **Branch** | Uma cópia da linha do tempo onde você trabalha isolado | A cópia do documento para rascunhar |
| **Merge** | Juntar o que você fez na branch de volta na `main` | Substituir o original pela versão pronta |

**Por que isso importa no hackathon?** Cada pessoa (ou cada feature) trabalha na **sua própria branch**. Assim, duas pessoas mexem no projeto ao mesmo tempo sem uma atrapalhar a outra — e a `main` continua sempre funcionando para a demonstração.

Os comandos, se quiser usar o terminal:

```bash
# criar uma branch nova e já mudar para ela
git checkout -b feature/calculadora-de-margem

# ver em qual branch você está (a atual fica marcada com *)
git branch

# voltar para a linha principal
git checkout main
```

> **Dica:** você não precisa decorar isso. Peça ao Claude Code: *"Cria uma branch pra eu trabalhar na calculadora de margem"* — ele cria e já muda para ela. No VSCode, o nome da branch atual aparece no **canto inferior esquerdo**; clicando ali dá pra trocar de branch com o mouse.

> Juntar as branches de volta (merge) e revisar o trabalho em equipe acontece num passo posterior — o mentor mostra o fluxo completo na hora. Por enquanto, basta entender: **branch = linha de trabalho isolada**, que não afeta a versão principal.

---

## GitHub — de onde veio o projeto

### O que é?

**GitHub** é onde o projeto fica guardado na nuvem. Foi de lá que você baixou o código para a sua máquina.

- **Git** = o sistema de histórico que roda na sua máquina (local)
- **GitHub** = a cópia do projeto na nuvem

Analogia: se o Git é o Word com "Ctrl+Z" e histórico de versões, o GitHub é o Google Drive onde o arquivo original fica guardado.

### Clonando o projeto (só faz uma vez)

Para baixar o projeto do GitHub para sua máquina:

```bash
git clone <url-do-repositorio>
cd hackathon-commercial
```

> O mentor vai te passar a URL exata do repositório na hora.

A partir daí, **todo o seu trabalho acontece localmente**, na sua máquina. Enviar as mudanças de volta para a nuvem e trabalhar em equipe é um passo posterior — quando chegar lá, o mentor mostra o fluxo.

---

## VSCode — seu editor de código + Claude

O **VSCode** é o editor onde você vai trabalhar. É onde os arquivos ficam, onde o Claude funciona e onde você salva suas mudanças.

### Interface básica

```
┌──────────────────────────────────────────────┐
│  Barra lateral        │  Editor              │
│  (seus arquivos)      │  (conteúdo aberto)   │
│                       │                      │
│  📁 back-end          │                      │
│  📁 front-end         │                      │
│  📁 specs             │                      │
├───────────────────────┴──────────────────────┤
│  Terminal (onde roda comandos)                │
│  $ _                                         │
└──────────────────────────────────────────────┘
```

- **Barra lateral (Explorer):** navega pelos arquivos e pastas
- **Editor (centro):** onde você vê e edita os arquivos
- **Terminal (parte de baixo):** onde digita comandos como `git status`

### Atalhos que você vai usar

| Atalho | O que faz |
|--------|-----------|
| `` Ctrl+` `` | Abre/fecha o terminal |
| `Ctrl+S` | Salva o arquivo |
| `Ctrl+Z` | Desfaz a última alteração |
| `Ctrl+P` | Busca um arquivo pelo nome |

---

### Extensão Claude Code no VSCode

O Claude Code funciona **dentro do VSCode** através de uma extensão.

**Como instalar:**
1. Na barra lateral, clique no ícone de quadradinho (extensões) — ou pressione `Ctrl+Shift+X`
2. Busque por **"Claude Code"**
3. Clique em **Install**
4. Um ícone do Claude aparece na barra lateral

**Como usar:**
1. Clique no ícone do Claude na barra lateral
2. O chat do Claude abre
3. Digite seu prompt em português — ele tem acesso a todos os arquivos do projeto
4. As mudanças que ele propõe aparecem na tela para você aprovar ou rejeitar

---

### Fazendo commit pelo VSCode (sem digitar comandos)

Você **não precisa usar o terminal** para salvar suas mudanças. O VSCode faz tudo com cliques:

1. Na barra lateral, clique no ícone de **Source Control** (parece um grafo com 3 bolinhas)
2. Os arquivos que mudaram aparecem listados
3. Clique no **+** ao lado de cada arquivo (ou clique em **+** no topo para marcar todos)
4. Digite uma mensagem descrevendo o que mudou (ex: "adiciona calculadora de margem")
5. Clique no botão **Commit** (o "check" ✓)

Pronto — sua mudança virou um commit na sua máquina. Sem terminal, sem decorar comandos.

---

## Resumo visual do fluxo

```
GitHub (nuvem) — você baixou o projeto daqui
    │
    │  git clone (só na primeira vez)
    ▼
Sua máquina (VSCode)
    │
    │  Claude Code cria e modifica arquivos
    ▼
Arquivos modificados
    │
    │  Commit (salva na sua máquina, com mensagem)
    ▼
Histórico local crescendo:   ●──●──●──●
```

> Trabalhar em equipe e enviar tudo de volta para a nuvem vem depois — por enquanto, o ciclo é só **modificar → commitar**, tudo local.

---

## Dúvidas frequentes

**"Fiz algo errado e quero voltar ao que estava antes."**

Peça ao Claude: *"Desfaz as últimas mudanças que você fez."*

Ou, se já fez commit, peça: *"Reverte o último commit."*

**"Não sei se salvei minhas mudanças (fiz o commit)."**

Rode `git status` no terminal. Se aparecer "nothing to commit, working tree clean", está tudo commitado. Se aparecer arquivos listados, ainda falta dar o commit.

**"Quero ver o histórico do que já foi feito."**

Abra a aba **Source Control** no VSCode — ela mostra a árvore de commits. Ou peça ao Claude: *"Mostra os últimos commits do projeto."*

**"Quero construir uma feature nova sem risco de quebrar o que já funciona."**

Trabalhe numa **branch**. Peça ao Claude: *"Cria uma branch pra essa feature."* Você faz seus commits ali isolado e, quando estiver pronta, o mentor mostra como juntar (merge) na `main`.
