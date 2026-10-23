---
title: "Lição 2 - Adicione avaliações por estrelas: uma primeira melhoria"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revisar o resultado em um navegador encaminhado e mesclar o primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, faça sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada com o Copilot em um branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a alteração em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo do Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente que exiba a avaliação existente em cada card: uma alteração pequena e independente, ideal para sua primeira sessão.

## Entenda uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa comum da CLI usa o repositório e o branch do Git que estão selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a discussão mais tarde, enquanto os arquivos e o branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas enquanto o agente explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e peça a alteração

Inicie uma nova conversa para começar a implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos que exibem os detalhes dos jogos e cria o código necessário. Agora você adicionou uma funcionalidade com o Copilot CLI.

## Revise o diff

Toda alteração gerada por IA deve ser revisada antes da mesclagem, mesmo quando for pequena. Faça essa revisão no próprio Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card do jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico; portanto, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Peça ao Copilot que inicie o site para você examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards dos jogos exibem as avaliações de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Agora que você criou a funcionalidade, abra um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar o PR e exibe a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa focada com o Copilot em um branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pelo issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
