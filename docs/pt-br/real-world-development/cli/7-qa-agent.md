---
title: "Lição 7 - Crie e use um agente de QA"
description: "Crie um agente personalizado de QA que reúna os requisitos do issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem em um navegador. Agora, reúna esses recursos em um agente personalizado com um processo de garantia de qualidade (QA) bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as conclusões dele com base no issue de filtragem.

## Cenário

Antes de abrir um pull request (PR), a Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador. Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até agora têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar requisitos e apresentar conclusões. Ele não substitui esses recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir o PR da funcionalidade, peça ao Copilot para criar um perfil reutilizável de QA. O perfil definirá as verificações realizadas por QA e os limites que devem ser respeitados.

1. Volte ao seu Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot registrou o fluxo de QA e os limites desejados. Assim, você evita que um agente incompleto ou com escopo amplo demais altere a funcionalidade quando você quer apenas verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. O campo `description` é obrigatório; `name` é opcional, mas oferece ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que QA apresenta evidências, pede autorização antes de alterar o código da implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base no issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione QA para que o agente possa usar o issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use este prompt para pedir ao agente de QA que revise a funcionalidade:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. O agente de QA começará a trabalhar.
4. Quando terminar, leia o relatório apresentado.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o trabalho dela. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões dele com base no issue de filtragem.

Agora, você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, [reúna tudo no PR da funcionalidade e use o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
