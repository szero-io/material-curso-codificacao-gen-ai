---
name: revisar-diff
description: 'Primeira revisão automatizada de um diff ou pull request: não aprova nem rejeita; classifica apontamentos em [bloqueia]/[melhora]/[pergunta], compara o diff com a descrição declarada (escopo a mais ou a menos) e verifica a seção fixa de segurança (validação de entradas, segredos, dependências novas, comandos ou SQL por concatenação). Use para revisar mudanças, diffs acumulados de refatoração ou PRs. Genérica: qualquer linguagem e projeto.'
argument-hint: 'diff (colado, arquivo, ou peça para rodar git diff) + descrição da mudança'
---

# Primeira revisão do diff

Primeira passada, não última palavra: a máquina reconhece padrões; domínio e consequência continuam com quem revisa.

## Parâmetros

1. **Diff** (obrigatório): colado, em arquivo, ou obtido com `git diff` no repositório.
2. **Descrição da mudança** (recomendado): texto do PR ou objetivo declarado. Se ausente, infira do contexto e DECLARE que foi inferida.
3. **Regras do projeto** (automático): `regras-do-projeto.md` na raiz (fallbacks: `AGENTS.md`, `.github/copilot-instructions.md`).

## Regras invioláveis

- Não aprove nem rejeite a mudança: aponte, classifique e pergunte.
- Classifique cada item:
  - `[bloqueia]` afeta comportamento, segurança ou contrato;
  - `[melhora]` afeta leitura ou estrutura;
  - `[pergunta]` depende de contexto que não está disponível.
- Compare o diff com a descrição: mudanças além ou aquém do escopo declarado devem ser apontadas.
- Não invente regras de negócio.

## Procedimento

1. Resuma em até 5 linhas o que o diff realmente modifica.
2. Liste os apontamentos com localização, justificativa e classificação.
3. Verifique a seção fixa de segurança: validação de entradas; segredos no código; dependências novas; consultas ou comandos construídos por concatenação.
4. Liste as perguntas que dependem de contexto humano.

## Formato de saída

Quatro seções, na ordem do procedimento.
