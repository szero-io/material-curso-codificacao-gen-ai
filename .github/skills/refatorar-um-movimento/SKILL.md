---
name: refatorar-um-movimento
description: 'Executa UM único movimento de refatoração por vez (renomear, extrair método, trocar estrutura de dados, eliminar duplicação), preservando o comportamento externo: a suíte de caracterização precisa continuar passando sem alterações. Movimentos de desempenho exigem benchmark antes/depois com a mesma entrada e semente. Use depois da skill caracterizar-comportamento. Genérica: qualquer linguagem e projeto.'
argument-hint: 'código + suíte de caracterização + problema estrutural alvo deste passo'
---

# Um movimento de refatoração

Mudanças grandes produzem diffs impossíveis de revisar e escondem qual alteração introduziu o problema. Esta skill dá UM passo por execução.

## Parâmetros

1. **Código atual** (obrigatório): caminho ou trecho; sem argumento, o arquivo ativo.
2. **Suíte de caracterização** (obrigatória): caminho ou colada. Sem ela, PARE e oriente rodar antes a skill `caracterizar-comportamento`.
3. **Problema estrutural alvo** (obrigatório): o movimento deste passo. Se não for informado, liste os candidatos em ordem de impacto e PARE para a escolha.
4. **Regras do projeto** (automático): `regras-do-projeto.md` na raiz (fallbacks: `AGENTS.md`, `.github/copilot-instructions.md`).

## Regras invioláveis

- Apenas UM movimento de refatoração por execução.
- Preserve o comportamento externo; a suíte de caracterização continua passando SEM alterações. Se o movimento exigir alterar um teste, não prossiga: explique primeiro por quê.
- Não introduza novas regras de negócio nem "aproveite para corrigir" nada fora do movimento.
- Movimento motivado por desempenho: exija medição antes e depois, com a mesma entrada e a mesma semente, e reporte os dois números; sem ganho medido, não é melhoria.
- Diff mínimo: nada de reformatar o arquivo inteiro.

## Procedimento

1. Confirme o movimento e o que ele melhora (uma frase).
2. Apresente o diff correspondente ao movimento único.
3. Informe como verificar: comando da suíte e, se for o caso, do benchmark.

## Formato de saída

Diff; justificativa breve; comandos de verificação (suíte e benchmark quando aplicável).
