---
title: "Lição 5 - Personalizar e usar uma habilidade quality-checks"
description: "Explore a habilidade quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código é apenas uma parte do trabalho. Já confirmamos manualmente que o código funciona e usamos arquivos de instruções para garantir que ele siga os padrões. Mas e os testes, o lint e os outros processos de integração contínua (CI)?

Para esses tipos de tarefa, as **habilidades de agente** são ideais. As habilidades ajudam o Copilot a entender como executar operações desse tipo corretamente.

Nesta lição, você vai:

- explorar a habilidade existente `quality-checks`;
- personalizar o formato dos resultados;
- recarregar e executar a habilidade.

## Cenário

A Tailspin Toys usa a habilidade `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As habilidades de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega quando necessário. Em sua forma básica, uma habilidade é uma pasta com o nome dela e um arquivo Markdown chamado `SKILL.md`. O Markdown inclui frontmatter com um nome e uma descrição que definem a habilidade, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode incluir subpastas com scripts e outros recursos que a habilidade utiliza.

> [!NOTE]
> A habilidade não precisa de pastas ou arquivos adicionais. A habilidade `quality-checks` da Tailspin Toys contém somente `SKILL.md` porque usa os comandos existentes do projeto.

As habilidades podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta global de habilidades do usuário, `~/.copilot/skills`.

## Explore a habilidade

Vamos explorar a habilidade criada pela equipe Tailspin Toys para executar testes unitários, lint e verificações de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` na parte superior. A descrição ajuda o Copilot a saber quando chamar a habilidade.
3. Leia as instruções e observe como elas orientam o Copilot no processo de execução dos testes e do lint.
4. Observe que a habilidade ainda não tem uma seção **Results output formatting**.

## Execute a habilidade antes de fazer alterações

As habilidades podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Vamos pedir ao Copilot para executar as três verificações da habilidade.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie o prompt:

   ```plaintext
   Execute a habilidade quality-checks para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório final.

## Personalize o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e quais resultados as ferramentas apresentaram. Vamos atualizar a habilidade para produzir esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a habilidade atualizada

Agora que fizemos a alteração, vamos testá-la. O Copilot CLI pode recarregar habilidades editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie exatamente o mesmo prompt:

   ```plaintext
   Execute a habilidade quality-checks para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximas etapas

Você personalizou e usou uma habilidade de agente existente. Nesta lição, você:

- explorou a habilidade `quality-checks` para testes unitários, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a habilidade.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [usando o servidor MCP do Playwright][next-lesson].

## Mais exemplos de habilidades

Estes exemplos da comunidade servem como referência, não são tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
