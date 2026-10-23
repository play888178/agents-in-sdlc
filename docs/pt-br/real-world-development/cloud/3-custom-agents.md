---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimentos específicos do domínio para orientar o Copilot. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que verifica a conformidade com as [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm alguns pontos em comum: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. A distinção mais simples é: o **agente personalizado** é quem trabalha; as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes como parte do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com diretrizes de acessibilidade. Durante esse trabalho, ele pode chamar skills como uma skill de fluxo de trabalho para pull requests ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como acontece com outras ferramentas de IA, teste e itere até encontrar o que funciona para seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com agentes personalizados, você explorará:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, queremos implementar isso o mais rápido possível. Use um agente personalizado para gerar a funcionalidade.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise o conteúdo para entender como ele orienta o Copilot.

Volte ao seu codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Crie e atribua um issue

O centro de controle é o lugar central para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar o trabalho e redirecionar tarefas ou fornecer orientações adicionais. Comece atribuindo ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o issue.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Caixa de diálogo de atribuição do agente de nuvem, com o agente personalizado Accessibility destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Nesta lição, você conheceu os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis para orientar o trabalho do Copilot.

Você explorou como:

- agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, comece a [acompanhar e orientar a sessão do agente][next-lesson] no centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
