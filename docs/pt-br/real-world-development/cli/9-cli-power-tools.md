---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra. Eles dão acesso a recursos avançados, informações internas e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, conheça outros recursos: gerenciamento de contexto, alternância de modelos, compartilhamento de sessões e delegação opcional para o [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você pode chegar ao limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, mas você também pode inspecionar ou compactar o contexto usando comandos de barra.

1. Volte ao seu Codespace e inicie o Copilot CLI na raiz do repositório, caso ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. Talvez não haja uma grande diferença se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela fica cheia. Use `/compact` para escolher quando isso acontecer. Use `/clear` ou `/new` quando mudar para uma tarefa sem relação e quiser iniciar uma conversa limpa.

## Escolha o modelo

Os modelos têm recursos diferentes, assim como as preferências de cada pessoa desenvolvedora. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todas as pessoas a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist do GitHub.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretende compartilhar.

## Opcional: amplie o trabalho ou delegue

O workshop principal foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir tarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar um worktree do Git isolado para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, comece uma tarefa nova e bem definida e revise os resultados pelo fluxo de trabalho normal. Para saber mais sobre trabalho assíncrono com agentes, continue no [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurá-lo, compartilhar sessões e entender o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há outros comandos de barra e recursos do Copilot CLI para explorar. Agora, [revise o que você aprendeu][next-lesson] e veja os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
