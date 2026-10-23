---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use o Plan para definir a abordagem, o Autopilot para criar a filtragem a partir de um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho da organização e ajudar a criar as funcionalidades certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo comum de desenvolvimento com agentes: começará com um issue para criar uma funcionalidade, confirmará que o código é válido e que a funcionalidade se comporta como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma conversa e o mesmo branch ao longo do fluxo de trabalho da funcionalidade. Em geral, equipes usam branches ou PRs diferentes para os vários tipos de arquivos, mas aqui faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de um issue do GitHub.
- definir requisitos no modo Plan.
- implementar a nova funcionalidade com o modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

À medida que continuar o trabalho na funcionalidade, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. O issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias ainda precisam ser definidos antes de escrever o código. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não altera os fundamentos — na verdade, torna-os ainda mais importantes. O fluxo da maioria das pessoas desenvolvedoras é parecido com este:

1. Começar por um issue aberto que descreva o que precisa ser feito.
2. Planejar o que será criado.
3. Criar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue uma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, depois, use o Autopilot para implementá-lo.

## Comece pelo issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e prepare o repositório e o terminal.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR das avaliações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie um branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os issues.
8. Use as teclas de seta para destacar o issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` ou um número semelhante. O caractere `#` permite adicionar ao contexto um issue ou pull request (PR) do GitHub.

## Planeje a funcionalidade de filtragem

O planejamento permite definir como implementar uma funcionalidade ou realizar uma tarefa antes de delegá-la ao Copilot. É sempre bom dedicar algum tempo ao planejamento de tarefas complexas. Vamos mudar para o modo Plan e pedir que o Copilot crie um plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano. Primeiro, explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o comportamento dos filtros. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano criado e revisado, implemente-o. Deixe o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que a implementação está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano antes de aprová-lo. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois de gerar o código, revise-o antes da mesclagem, como faria com qualquer outro código. Revise a implementação e execute o site para confirmar que tudo está correto.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o issue e com as decisões tomadas durante o planejamento.
4. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explore a nova funcionalidade

O código parece bom, mas será que funciona? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde ao issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado por ele.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um issue do GitHub.
- definiu requisitos no modo Plan.
- implementou a nova funcionalidade usando o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, descubra como o código é gerado e garanta que siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
