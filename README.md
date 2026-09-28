# Qualidade e Refatoração com Skills

Material de apoio do curso **Construção de Software: Codificação Assistida por IA Generativa**.

Este repositório reúne *skills* e um *agente* para o GitHub Copilot que conduzem refatorações
com disciplina: primeiro registrar o que o código faz hoje, depois mudar uma coisa só e,
por fim, revisar o diff antes de qualquer merge. A ideia é usar a IA como assistente de um
processo de engenharia conhecido, não como substituta dele.

> Refatorar é mudar a estrutura sem mudar o comportamento. Se você não sabe qual é o
> comportamento atual, não tem como garantir que ele foi preservado.

## Por que isso importa

Assistentes de código geram e alteram muito código rapidamente. O risco não é a IA errar
de vez em quando. O risco é aceitar um diff grande, que mistura várias mudanças e
"correções oportunistas", sem uma rede de segurança que mostre o que quebrou.

As skills daqui aplicam três práticas clássicas:

1. **Testes de caracterização**: documentar de forma executável o comportamento atual,
   inclusive o que parece defeito, antes de mexer em qualquer coisa.
2. **Um movimento por vez**: cada passo de refatoração é pequeno, com diff mínimo, e a
   suíte de caracterização precisa continuar passando **sem alterações**.
3. **Revisão classificada**: o diff é revisado com apontamentos `[bloqueia]`, `[melhora]`
   e `[pergunta]` e uma checagem fixa de segurança. A revisão não aprova nem rejeita:
   a decisão continua com quem revisa.

## Conteúdo

```
.github/
├── agents/
│   └── refatoracao-guiada.agent.md      # agente que encadeia as três etapas
└── skills/
    ├── caracterizar-comportamento/      # 1. rede de segurança de testes
    ├── refatorar-um-movimento/          # 2. um único passo de refatoração
    ├── revisar-diff/                    # 3. primeira revisão do diff
    └── refatorar-com-escopo/            # refatoração a partir de uma revisão
```

### Skills

| Skill | Para que serve | Entrada | Saída |
|---|---|---|---|
| [`caracterizar-comportamento`](.github/skills/caracterizar-comportamento/SKILL.md) | Registrar o comportamento **atual** do código, sem corrigir nada. Defeitos aparentes são capturados e marcados com `possível defeito a revisar`. | Código-alvo e, opcionalmente, usos conhecidos | Suíte de testes, instruções de execução, comportamentos registrados, perguntas para o time |
| [`refatorar-um-movimento`](.github/skills/refatorar-um-movimento/SKILL.md) | Aplicar **um** movimento (renomear, extrair método, trocar estrutura de dados, eliminar duplicação). Movimentos de desempenho exigem benchmark antes e depois, com a mesma entrada e semente. | Código, suíte de caracterização e o problema estrutural alvo | Diff, justificativa, comandos de verificação |
| [`revisar-diff`](.github/skills/revisar-diff/SKILL.md) | Fazer a primeira passada de revisão: resumo, apontamentos classificados, comparação com o escopo declarado e seção de segurança. | Diff e descrição da mudança | Quatro seções, na ordem do procedimento |
| [`refatorar-com-escopo`](.github/skills/refatorar-com-escopo/SKILL.md) | Corrigir apenas os itens `[bloqueia]` e `[melhora]` de uma revisão anterior, substituir construções inseguras e transformar ambiguidades em perguntas. | Código e a revisão anterior | Escopo, código refatorado, testes, tabela de itens tratados × em aberto |

Todas as skills são genéricas: funcionam em qualquer linguagem e detectam o framework de
teste do projeto. Sem runner disponível, geram uma verificação executável com asserções.

### Agente `Refatoracao Guiada`

O [agente](.github/agents/refatoracao-guiada.agent.md) executa um ciclo completo, nesta ordem:

```
caracterizar-comportamento  ──►  refatorar-um-movimento  ──►  revisar-diff
   (suíte passa no código         (suíte continua passando      (somente leitura:
    atual, sem corrigir nada)      sem mudar nenhum teste)       aponta, não decide)
```

Pontos importantes do fluxo:

- Só avança para a refatoração quando a suíte de caracterização passa de verdade.
  Instruções de execução não substituem uma execução bem-sucedida.
- Se o problema estrutural não for informado, lista candidatos por impacto e **para**
  para você escolher.
- Se houver regressão, corrige apenas a implementação. Nunca ajusta os testes para
  acomodar a mudança.
- Não cria commits nem branches sem pedido e preserva alterações que já existiam.
- Encerra após a revisão. Um novo movimento exige uma nova solicitação.

## Como usar

Requisitos: VS Code com GitHub Copilot Chat e suporte a agentes e skills personalizados.

1. Copie a pasta `.github/` para a raiz do seu projeto (ou abra este repositório no VS Code).
2. Opcionalmente, crie um `regras-do-projeto.md` na raiz com as convenções do time.
   As skills também procuram `AGENTS.md` e `.github/copilot-instructions.md`.
3. No Copilot Chat:
   - selecione o agente **Refatoracao Guiada** para rodar o ciclo completo; ou
   - invoque uma skill isolada pelo nome, por exemplo `/caracterizar-comportamento`
     com o arquivo que você quer proteger aberto no editor.

Exemplo de pedido ao agente:

```
Refatore src/pedidos/calculo_frete.py: a função calcular() tem três blocos
duplicados de arredondamento. Quero eliminar essa duplicação.
```

## Sugestão de exercício

1. Pegue um trecho de código legado ou gerado por IA, de preferência sem testes.
2. Rode `caracterizar-comportamento` e leia a lista de **perguntas para o time**.
   Quantas decisões do código ninguém tinha especificado?
3. Escolha um único movimento e rode `refatorar-um-movimento`. O diff está pequeno o
   bastante para ser revisado em poucos minutos?
4. Rode `revisar-diff` e compare os apontamentos com a sua própria revisão. O que a
   ferramenta pegou e o que só você, com contexto de domínio, conseguiria ver?

## Observações

- `refatorar-com-escopo` espera como entrada a saída de uma skill `revisar-legibilidade`,
  que não faz parte deste repositório. Você pode usar a saída de `revisar-diff` ou uma
  revisão feita à mão, desde que siga a mesma classificação.
- As skills não corrigem divergências entre o código e as regras do projeto por conta
  própria: registram essas divergências como perguntas para o time.
