---
title: "Lição 9 - Comandos slash do GitHub Copilot CLI"
description: "Explore comandos slash para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente para a nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos slash para interagir com ele. Esses comandos dão acesso a recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação para o agente na nuvem.

## Cenário

Você concluiu o fluxo principal do CLI. Agora vamos conhecer outros recursos: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e delegação opcional para o [Copilot cloud agent][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas grandes ou complexas, você pode chegar ao limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, e você pode inspecionar ou compactar o contexto com comandos slash.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma grande mudança se a conversa ainda for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se preenche. Use `/compact` para escolher quando fazer isso. Use `/clear` ou `/new` para iniciar uma conversa limpa quando mudar para uma tarefa sem relação com a atual.

## Escolha seu modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa desenvolvedora tem suas preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a aproveitar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist do GitHub.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino mais adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não queira compartilhar.

## Opcional: amplie o trabalho ou delegue

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree isolada do Git para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao Copilot cloud agent, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, comece uma tarefa nova e bem delimitada e revise o resultado pelo fluxo normal. Para saber mais sobre o trabalho assíncrono dos agentes, continue com o [workshop do cloud agent][cloud-workshop].

## Resumo e próximos passos

Os comandos slash do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e ver o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para conhecer os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação para o agente na nuvem.

Há muitos outros comandos slash e recursos para explorar no Copilot CLI. Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e os próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
