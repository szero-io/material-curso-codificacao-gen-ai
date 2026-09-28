---
name: refatorar-com-escopo
description: 'Refatoração com escopo fechado a partir de uma revisão de código: corrige apenas itens [bloqueia] e [melhora], preserva comportamento determinável, transforma ambiguidades em perguntas (nunca inventa regra de negócio), substitui construções inseguras (SQL por concatenação, segredos no código, catch vazio, validação fail-open) e entrega testes. Use depois da skill revisar-legibilidade, com a revisão como entrada. Genérica: qualquer linguagem e projeto.'
argument-hint: 'código a refatorar + revisão anterior (caminhos ou colados)'
---

# Refatoração com escopo

A revisão anterior define o escopo. Nada fora dela entra no diff.

## Parâmetros

1. **Código original** (obrigatório): caminho ou trecho passado como argumento; sem argumento, o arquivo ativo.
2. **Revisão anterior** (obrigatório): a saída da skill `revisar-legibilidade` (caminho de arquivo salvo ou texto colado). Se não for fornecida, PARE e peça-a — refatorar sem revisão é outro fluxo.
3. **Regras do projeto** (automático): `regras-do-projeto.md` na raiz (fallbacks: `AGENTS.md`, `.github/copilot-instructions.md`).

## Regras invioláveis

- Corrija apenas os itens classificados como `[bloqueia]` e `[melhora]`; ignore `[estilo]` salvo pedido explícito.
- Preserve o comportamento de negócio que puder ser determinado pelo código e pelo contexto fornecido.
- Comportamento ambíguo (valor mágico, marcador, regra sem explicação registrada): preserve-o isolado em função nomeada e liste como **pergunta aberta** — não invente a regra nem a remova.
- Substitua construções inseguras: comandos/SQL sempre parametrizados; segredos saem do código e de artefatos gerados (variável de ambiente); exceções específicas com mensagem descritiva (nunca catch vazio nem validação que falha aberta); entradas validadas nos limites.
- Estado global mutável e dependências implícitas de relógio ou aleatoriedade viram parâmetros injetáveis.
- Inclua testes para os comportamentos preservados e para os problemas corrigidos, no framework do projeto; sem runner disponível, entregue uma classe/função de verificação executável com asserções.

## Procedimento

1. Cruze a revisão com o código e liste o escopo (itens que serão tratados).
2. Apresente a versão refatorada.
3. Apresente os testes.
4. Feche com a tabela: apontamentos **tratados** × **em aberto** (com o motivo — inclui as perguntas ao time).

## Formato de saída

Quatro seções, na ordem do procedimento.
