---
title: "Lição 7 - Crie e use um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos das issues, a habilidade quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a habilidade `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem em um navegador. Agora, reunirá esses recursos em um agente personalizado com um processo de QA claramente definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, habilidades e ferramentas MCP;
- criar e inspecionar um perfil reutilizável de garantia de qualidade;
- selecionar o agente de QA e revisar as conclusões com base na issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A habilidade `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e apresentar as conclusões. Ele não substitui os outros recursos nem exige uma nova conversa.

## Crie o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot para criar um perfil reutilizável de QA. O perfil definirá tanto as verificações quanto os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos de acordo com as issues e os requisitos definidos, seguir as instruções do repositório, executar a habilidade quality-checks, usar o MCP do Playwright para verificar o comportamento e adicionar testes quando a cobertura estiver incompleta.

   Ele deve informar cada requisito como aprovado, reprovado ou bloqueado e incluir evidências de apoio. Precisa pedir permissão antes de alterar o código de implementação e não pode fazer commit de alterações nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites pretendidos. Assim, você evita que um agente incompleto ou com escopo amplo demais altere o recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas ajuda a dar ao agente um nome claro na interface.
3. Leia as instruções do perfil e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a habilidade `quality-checks` e usa o MCP do Playwright.
4. Confirme se o QA apresenta evidências, pede permissão antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão para revisá-lo antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione o QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com o seguinte comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, ele está disponível e será ativado pelo comando acima.

2. Envie o prompt a seguir para pedir ao agente de QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem de acordo com a issue e as decisões do nosso plano. Está pronto para um PR?
   ```

3. O agente de QA começará a trabalhar.
4. Quando terminar, leia o relatório.

## Resumo e próximas etapas

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, habilidades e ferramentas MCP;
- criou e inspecionou um perfil reutilizável de garantia de qualidade;
- selecionou o agente de QA e revisou as conclusões com base na issue de filtragem.

Agora você tem a implementação, a atualização da habilidade, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você vai [reunir tudo em um PR do recurso e usar o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
