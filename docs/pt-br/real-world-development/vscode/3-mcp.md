---
title: "Exercício 3 - Teste sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real. Depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request do trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite isso: é um padrão para que o agente se comunique com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e da interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, os agentes podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e recursos são acessados por um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. Ele permite que o agente crie e atualize repositórios e gerencie issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: fornece recursos de automação de navegador com o Playwright. Ele permite que o agente navegue por páginas Web, preencha formulários e selecione botões.

Há muitos outros servidores MCP que fornecem acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do template do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente. O VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicione o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, ideal para testar sua funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário em um Codespace sem ambiente gráfico.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie o servidor e confirme que ele é confiável

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas do servidor, o VS Code o iniciará e solicitará que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha confiar nele para permitir que seja iniciado.

Iniciar o servidor manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor e pedirá sua confirmação na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece um navegador real que o Copilot pode controlar. Em vez de navegar pelo aplicativo para verificar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar o comportamento da funcionalidade sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente interpreta elementos estruturados e rotulados (botões, links, itens de lista) como a tecnologia assistiva faria. Uma verificação funcional rápida também pode funcionar como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas ou peça ao Copilot que corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você interrompeu o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que a funcionalidade funciona, abra um pull request (PR) para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. No Codespace, acesse o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Selecione o ícone **+** para preparar as alterações.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, como usar github.com ou a interface de linha de comando do GitHub. Como você já está trabalhando com o GitHub Copilot, deixe que ele crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. No menu de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar-se no GitHub. Siga as instruções para permitir a autenticação.
5. Observe como o Copilot pesquisa as issues, encontra a issue correta e cria o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente a filtragem no navegador antes de publicá-la.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade — mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando dele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
