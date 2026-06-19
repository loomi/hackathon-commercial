# Setup inicial — Git, VSCode e Claude

Antes de começar a construir qualquer coisa, você precisa instalar 4 ferramentas na sua máquina. Esta configuração é feita **uma única vez**.

Não é necessário entender o funcionamento interno de cada ferramenta agora — os próximos guias cobrem o uso. O objetivo aqui é **deixar tudo instalado e funcionando.**

> Tempo estimado: 05 a 15 minutos. Se travar em algum passo, acione um mentor.

---

## O que vamos instalar (e por quê)

| Ferramenta | Para que serve |
|-----------|----------------|
| **Git** | Guarda o histórico de tudo que você altera no projeto |
| **VSCode** | O editor onde você trabalha |
| **Claude Code** | O assistente de IA que escreve o código |
| **Extensão Claude no VSCode** | Integra o Claude ao editor |

A ordem importa: instale **nesta sequência**.

---

## Passo 1 — Instalar o Git

O **Git** é o que permite baixar o projeto e guardar o histórico das suas mudanças.

### Windows

1. Acesse **https://git-scm.com/download/win**
2. O download começa sozinho. Abra o instalador.
3. Clique em **Next** em todas as telas (os padrões já servem) e por fim em **Install**.
4. Ao terminar, clique em **Finish**.

### Mac

1. Abra o aplicativo **Terminal** (busque por "Terminal" no Spotlight com `Cmd+Espaço`)
2. Digite o comando abaixo e aperte Enter:
   ```bash
   git --version
   ```
3. Se o Git não estiver instalado, o Mac vai oferecer instalar as "ferramentas de linha de comando". Clique em **Instalar** e aguarde.

### Como saber se deu certo (Windows e Mac)

Abra o terminal e digite:

```bash
git --version
```

Se aparecer algo como `git version 2.43.0`, a instalação está concluída.

---

## Passo 2 — Instalar o VSCode

O **VSCode** é o editor onde tudo acontece: os arquivos, o terminal e o Claude.

1. Acesse **https://code.visualstudio.com**
2. Clique no botão grande de download (ele detecta seu sistema automaticamente)
3. Abra o instalador e siga avançando com os padrões
   - **No Windows**, marque a opção **"Adicionar ao PATH"** se ela aparecer (geralmente já vem marcada)
4. Abra o VSCode ao terminar

Esse é o ambiente onde você vai trabalhar durante o hackathon.

---

## Passo 3 — Instalar o Claude Code

O **Claude Code** é o assistente de IA. Ele se instala via terminal.

### Abra o terminal dentro do VSCode

No VSCode, aperte:

```
Ctrl + `   (Windows)      ou      Cmd + `   (Mac)
```

> É a tecla de crase, geralmente abaixo do `Esc`. Abre uma faixa preta na parte de baixo — esse é o terminal.

### Rode o comando de instalação

Cole o comando abaixo no terminal e aperte Enter:

```bash
npm install -g @anthropic-ai/claude-code
```

> **Deu erro dizendo que `npm` não existe?** Você precisa instalar o **Node.js** primeiro. Acesse **https://nodejs.org**, baixe a versão **LTS**, instale com os padrões, **feche e reabra o VSCode** e rode o comando de novo.

### Verifique se instalou

No terminal, digite:

```bash
claude --version
```

Se aparecer um número de versão, a instalação está concluída.

---

## Passo 4 — Instalar a extensão do Claude no VSCode

A extensão coloca o Claude **dentro do editor**, com uma janela de chat ao lado do código.

1. No VSCode, na barra lateral esquerda, clique no ícone de **quadradinhos** (Extensões) — ou aperte `Ctrl+Shift+X` (`Cmd+Shift+X` no Mac)
2. Na busca, digite **"Claude Code"**
3. Clique em **Install** na extensão oficial da **Anthropic**
4. Um ícone do Claude aparece na barra lateral

---

## Passo 5 — Fazer login no Claude

Agora conecte o Claude à sua conta.

1. Clique no **ícone do Claude** na barra lateral do VSCode
2. A janela de chat abre e pede para você fazer login
3. Clique em **Sign in** / **Login** — o navegador abre
4. Entre com a conta indicada pelo mentor e autorize o acesso
5. Volte para o VSCode — o chat agora está pronto para uso

> Se o mentor passar uma chave de API em vez de login, ele te mostra onde colá-la na hora.

---

## Passo 6 — Baixar o projeto do hackathon

Por fim, traga o projeto para sua máquina.

> Repositório do projeto: **https://github.com/loomi/hackathon-commercial**

1. Abra o terminal no VSCode (`` Ctrl+` ``)
2. Rode o comando abaixo:
   ```bash
   git clone https://github.com/loomi/hackathon-commercial
   ```
3. Entre na pasta do projeto:
   ```bash
   cd hackathon-commercial
   ```
4. No VSCode, vá em **File → Open Folder** e abra essa pasta

> A partir daqui, o Claude enxerga todos os arquivos do projeto e está pronto para trabalhar.

---

## Checklist final

Antes de seguir para o próximo guia, confirme que tudo está ok:

- [ ] `git --version` mostra uma versão
- [ ] VSCode abre normalmente
- [ ] `claude --version` mostra uma versão
- [ ] O ícone do Claude aparece na barra lateral do VSCode
- [ ] Você fez login e o chat do Claude abre
- [ ] O projeto foi clonado e está aberto no VSCode

Com todos os itens marcados, o ambiente está configurado.

---

## Deu algum problema? (FAQ)

**"O comando `npm` ou `claude` não é reconhecido."**
Geralmente é o Node.js faltando ou o VSCode precisando reiniciar. Instale o Node.js (https://nodejs.org, versão LTS), **feche e reabra o VSCode** e tente de novo.

**"O `git clone` pediu usuário e senha."**
Use a conta do GitHub indicada pelo mentor. Se pedir senha, normalmente é um *token* — peça ajuda ao mentor.

**"A extensão do Claude não aparece depois de instalar."**
Feche e reabra o VSCode. Se ainda não aparecer, confira em Extensões se ela está realmente **instalada e habilitada**.

**"Travei em algum passo."**
Acione um mentor. A configuração inicial costuma ser a etapa mais trabalhosa; a partir dela, o fluxo flui.

---

## Próximo passo

Com tudo instalado, siga para o **[Guia 01 — Claude Code e Vibecoding](./01-claude-vibecoding.md)** e aprenda a colocar o Claude para trabalhar.
