---
title: "Exercício 2 - Adicione novas funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como a execução de testes. Em geral, uma pessoa desenvolvedora precisa localizar todos os arquivos relevantes, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou faltando e fazer novas alterações. E assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para agir com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem atua como uma pessoa parceira de programação assíncrona, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo agente do GitHub Copilot pode implementar funcionalidades na interface do usuário e na camada de dados.
- revisar as alterações e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender o que ele já faz.

O site é uma plataforma de financiamento coletivo de jogos de tabuleiro com tema de desenvolvimento de software. Ele permite listar jogos e exibir os detalhes de cada um. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM; não há uma API de back-end separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro, com escopo definido e APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao codespace. Você deverá estar no branch `main` após o exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione um branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site com este comando:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, deverá aparecer um aviso com a URL, semelhante ao seguinte:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando o servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Home Page**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Game Details Page**: quando você seleciona um jogo, abre uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa os indicará quando necessário.

A implementação inicial do site funciona, mas queremos adicionar recursos. Comece explorando o backlog. Quando você criou o projeto a partir do template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses issues.

1. No menu suspenso de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores do workshop não estão expressando preferência por um modelo. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para tentar manter as sugestões relativamente consistentes e oferecer uma boa experiência. No entanto, como os modelos de linguagem grandes (LLMs) são probabilísticos, as sugestões podem ser diferentes das indicadas. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda assim concluir a tarefa.

3. Envie este prompt ao Copilot para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todos os issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos maiores em partes menores e incluir exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, é necessário fazer pelo menos três alterações distintas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento dos filtros.
- atualizar a página de listagem de jogos com a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes da mesclagem. O modo agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de um exemplo de seleção do arquivo de instruções UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com o issue relacionado no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade. Ele deverá encontrar os helpers da camada de dados, a interface e os testes; em seguida, começará a alterar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho ainda está em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas nos testes durante o processo. Isso é normal: o Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão concluída do modo agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você viu como:

- o modo agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados.
- o modo agente explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo modo agente antes de mesclá-los à base de código.

Agora, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra de exploração — implemente paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Com as habilidades deste exercício, peça ao Copilot que implemente a paginação no site. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis.
- decidir como implementar a paginação e se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias para criar a implementação desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: funções e quando usá-los][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
