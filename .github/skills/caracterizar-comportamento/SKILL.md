---
name: caracterizar-comportamento
description: 'Testes de caracterização: registram o comportamento ATUAL de um código antes de refatorar, sem corrigir nada. Use quando pedirem para caracterizar, proteger comportamento ou criar rede de segurança de testes para código legado ou gerado. Captura casos de limite, decisões não especificadas e defeitos aparentes (marcados como possível defeito a revisar). Genérica: qualquer linguagem e projeto.'
argument-hint: 'código-alvo (sem argumento: arquivo aberto) + usos conhecidos (opcional)'
---

# Testes de caracterização

O objetivo NÃO é decidir se o código está correto: é documentar, de forma executável, o que ele faz hoje, antes de qualquer mudança.

## Parâmetros

1. **Código-alvo** (obrigatório): caminho ou trecho passado como argumento; sem argumento, o arquivo ativo. Leia o arquivo inteiro.
2. **Usos conhecidos** (opcional): entradas e chamadas reais, se fornecidas.
3. **Framework de teste** (automático): detecte o do projeto (JUnit, pytest etc.). Sem runner disponível, gere uma classe/função de verificação executável com asserções e instruções de compilação e execução.
4. **Regras do projeto** (automático): `regras-do-projeto.md` na raiz (fallbacks: `AGENTS.md`, `.github/copilot-instructions.md`).

## Regras invioláveis

- NÃO corrija o comportamento existente, nem de leve.
- Registre os comportamentos observáveis, incluindo casos de limite que já são tratados ou ignorados.
- Se algum comportamento parecer um defeito, capture-o assim mesmo e marque o teste com o comentário `possível defeito a revisar`.
- Não invente requisitos que não estejam no código ou no contexto fornecido.
- A suíte deve PASSAR contra a implementação atual; se alguma verificação falhar, o problema está na verificação.

## Procedimento

1. Leia o código e liste os comportamentos observáveis (caminho principal, limites, entradas inválidas, ordem de saída, arredondamentos, estado).
2. Gere a suíte de caracterização em um único arquivo, um caso por comportamento, com nomes que descrevem a regra registrada.
3. Informe como compilar e executar a suíte no projeto.
4. Feche com a lista de decisões que parecem não especificadas (valores, tipos aceitos, silêncios): são perguntas para o time, não correções.

## Formato de saída

Arquivo de testes/verificação; instruções de execução; lista "comportamentos registrados"; lista "perguntas para o time".
