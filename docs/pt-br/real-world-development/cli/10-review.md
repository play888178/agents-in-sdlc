---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e localizou o issue de filtragem já criado.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente seu primeiro pull request (PR).
- começou pelo issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa de filtragem e autorizou o Agent Merge para o PR da funcionalidade.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em seu próprio branch criado a partir de `main` atualizado:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes correspondentes.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e o mesmo branch. O trabalho foi reunido em um PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: testes automatizados, inspeção do site no navegador e exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. QA reuniu esses resultados com a revisão dos requisitos e da cobertura dos testes antes do PR.

Os testes adicionados devem corrigir lacunas reais. Uma execução de QA que não exige novos testes também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas após mudanças.

## Práticas recomendadas

O contexto e as ferramentas que você fornece influenciam o trabalho do Copilot. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para definir a abordagem antes de implementar, **Interactive** para acompanhar alterações específicas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e outro mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer criar, *por que* e *como* influencia muito o resultado.

## Explore outros recursos

Você concluiu o fluxo de trabalho principal. Veja alguns outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de se aprimorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você planeja criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com as experiências das outras pessoas. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue na série opcional [Backer Concierge com Foundry][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
