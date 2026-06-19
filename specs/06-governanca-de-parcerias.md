# Spec: Governança de Parcerias

## Contexto

A Loomi tem parcerias com plataformas de inovação, aceleradoras e hubs tecnológicos. Hoje essas parcerias ficam espalhadas em planilhas, e-mails e na cabeça de cada pessoa. Não existe um lugar centralizado que mostre quais parcerias estão ativas, o que cada uma oferece e quais oportunidades estão abertas.

**Hoje sem essa feature:** oportunidades de negócio perdidas porque o time não sabia que uma parceria podia ajudar, ou porque a parceria foi esquecida e ficou inativa.

**Com essa feature:** o time abre uma central única, vê todos os parceiros, oportunidades abertas, e recebe alertas quando uma parceria precisa de atenção.

---

## O que fazer

Uma **central de parcerias** onde o time vê tudo num só lugar: quem são os parceiros, o que cada um oferece, quais oportunidades estão abertas, e recebe alertas quando algo precisa de atenção.

### 1. Cadastro de Parceiros
O time registra cada parceiro com informações completas:
- Nome e tipo (aceleradora, hub de inovação, plataforma de licitação, canal de vendas, etc.)
- Status: ativo, em negociação, inativo
- Quem é o contato responsável na Loomi
- Quais benefícios essa parceria oferece (ex: "acesso a editais de inovação", "leads qualificados de indústria", "desconto em eventos")
- Data de início e vencimento da parceria
- Histórico: quantas oportunidades essa parceria já gerou

### 2. Central de Oportunidades
Cada parceiro pode gerar oportunidades de negócio. Todas ficam numa **lista única**, fácil de filtrar:
- Descrição da oportunidade
- Prazo para responder/participar
- Valor estimado
- Quem do time comercial é o responsável
- Status: nova, em análise, proposta enviada, ganha, perdida

O time consegue filtrar por parceiro e por status — ex: *"me mostra todas as oportunidades novas do hub CESAR"*.

### 3. Alertas Automáticos
O sistema envia e-mails de alerta em 3 situações:
- **Parceria vencendo:** 30 dias antes do vencimento, alerta o responsável para renovar ou encerrar
- **Nova oportunidade:** quando alguém cadastra uma oportunidade vinda de um parceiro, o time recebe aviso
- **Parceria esquecida:** se uma parceria ativa não teve nenhuma atividade em 60 dias, alerta para reativação

### 4. Resumo IA por Parceiro
Ao abrir a página de um parceiro específico, o vendedor pode clicar em **"Gerar resumo IA"**. A IA (Claude) analisa o histórico daquela parceria e gera um resumo:

> *"Parceria com CESAR ativa desde jan/2025. 12 oportunidades geradas, 4 convertidas (taxa de 33%). Maior deal: Projeto X por R$ 180k. A parceria está ativa mas sem novas oportunidades nos últimos 45 dias. Sugestão: agendar reunião de alinhamento para reativar o fluxo."*

Esse resumo é gerado **sob demanda** (quando o vendedor clica), não automaticamente — para controlar custos de IA.

---

## Restrições

- Nesta primeira versão, os dados são inseridos manualmente pelo time (sem integração com sistemas externos dos parceiros)
- Os alertas por e-mail usam o módulo de e-mail já configurado no sistema
- O resumo da IA é gerado apenas quando alguém pede (clique no botão), não automaticamente
- Todo parceiro precisa ter no mínimo: nome, tipo, status e contato responsável

---

## Critérios de sucesso

- [ ] Todo parceiro ativo está cadastrado com status, contato e benefícios
- [ ] O time recebe alerta por e-mail quando uma parceria está próxima do vencimento
- [ ] Oportunidades abertas de todos os parceiros aparecem numa única lista ordenada por prazo
- [ ] O resumo IA de um parceiro específico é gerado em menos de 10 segundos
- [ ] É possível filtrar oportunidades por status e por parceiro

---

## Detalhamento

### Informações que o sistema guarda
- **Parceiro:** nome, tipo (aceleradora, hub de inovação, plataforma de licitação, canal de vendas, etc.), status (ativo, em negociação, inativo), contato responsável (nome, e-mail, telefone), benefícios oferecidos, etiquetas (ex.: "inovação", "indústria", "saúde"), data de início, data de vencimento (pode não ter) e observações.
- **Oportunidade:** o parceiro de origem, título, descrição, valor estimado, prazo, responsável comercial e status (nova, em análise, proposta enviada, ganha, perdida).

### O que dá pra fazer
- Cadastrar, editar e desativar parceiros (desativar não apaga — só marca como inativo).
- Listar parceiros com filtro por status, por tipo e busca por nome, mostrando quantas oportunidades cada um tem.
- Abrir um parceiro e ver todos os dados, suas oportunidades e a contagem por status.
- Cadastrar uma oportunidade vinculada a um parceiro (isso dispara o aviso ao time).
- Ver todas as oportunidades de todos os parceiros numa lista única, filtrando por status e por parceiro, ordenada por prazo.
- Pedir o resumo da IA de um parceiro (sob demanda, no clique do botão).

### Os alertas automáticos (uma vez por dia)
- **Vencimento:** parcerias ativas que vencem nos próximos 30 dias → e-mail ao responsável lembrando de renovar.
- **Inatividade:** parcerias ativas sem nenhuma oportunidade nova há 60 dias → e-mail sugerindo reativação.

### O que pedimos para a IA
Gerar um resumo executivo curto (até ~80 palavras) sobre o histórico e o estado atual de uma parceria, com números concretos (oportunidades geradas, convertidas, valores) e uma sugestão de próximo passo. A IA usa os dados do parceiro, o total de oportunidades, a divisão por status, as ganhas e a data da última oportunidade.

### As telas
- **Lista de parceiros:** tabela com nome, tipo, status (com cor), contato, número de oportunidades e vencimento. Filtros no topo e um botão "Novo parceiro".
- **Página do parceiro:** dados do parceiro, botão "Gerar resumo IA", o resumo (quando já gerado), a lista de oportunidades com filtro por status e um botão "Nova oportunidade".
- **Central de oportunidades:** lista única de todas as oportunidades, com filtros por status e por parceiro, ordenada por prazo.

### Do que depende
- Não depende de nenhuma outra feature — pode ser construída sozinha.

---

## Prompt para o Claude Code

> Copie e cole no chat do Claude Code para começar a implementar:

```
Leia o arquivo specs/06-governanca-de-parcerias.md por completo. Ele descreve a
feature do ponto de vista de negócio — o cadastro, as oportunidades, os alertas,
as regras e os critérios de sucesso. Você decide a melhor forma de implementar
tecnicamente (banco de dados, telas, alertas por e-mail e IA).

Implemente a feature de ponta a ponta, back-end e front-end, seguindo a spec.
Se ficar em dúvida sobre alguma regra de negócio, me pergunte antes de assumir.
```
