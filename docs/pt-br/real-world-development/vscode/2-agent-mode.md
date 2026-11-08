---
title: "Exercício 2 - Adicione uma funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente envolvem vários arquivos e tarefas, como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer alterações, executar testes, depurar, descobrir qual arquivo ficou de fora, fazer outra alteração e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o cloud agent foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, o que permite ciclos rápidos de feedback e interação. O cloud agent atua como um parceiro de programação que trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, vai concluir as alterações restantes na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site da Tailspin Toys

Antes de fazer alterações, vamos explorar o site da Tailspin Toys para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. As pessoas podem listar jogos e ver seus detalhes. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM. Não há uma API de backend separada nem uma estrutura de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um `<script>` Astro pequeno e específico que usa APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, a tarefa `npm run dev` inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-lo no GitHub Codespace:

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, ao selecionar um link de localhost no terminal, você será redirecionado automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indica as opções relevantes.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. A configuração do projeto a partir do template criou automaticamente um conjunto de issues. Peça ao GitHub Copilot para exibi-las.

1. Na visualização Chat, selecione **Agent** no menu de agentes. O agente planeja e implementa alterações de forma autônoma, em vários arquivos, executa comandos do terminal e chama ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em detrimento de outro. Usamos o Claude Sonnet 4.5 para criar o workshop e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes. Como os LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP, mas ainda deverá conseguir concluir a tarefa.

3. Envie o prompt a seguir para pedir ao Copilot que mostre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto. Vamos usar o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a implementação de funcionalidades no site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as diretrizes gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, como:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, você pode reutilizar orientações, dividir instruções complexas em partes menores e usar exemplos e templates.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, é preciso fazer pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser aprovados antes de mesclar as alterações à base de código. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra conversa.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na visualização Chat, confirme que **Agent** continua selecionado no menu de agentes.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Use o prompt a seguir para pedir ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos relevantes. Ele deverá encontrar os helpers da camada de dados, a interface e os testes. Depois, começará a modificar arquivos e executar testes.

   ![Copilot explora os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as alterações; não precisa fazer isso enquanto o trabalho ainda estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. Não há problema: o Copilot alterna entre geração de código e testes até concluir a tarefa e não detectar erros.

   ![Sessão completa do Copilot Chat com o modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site da Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente antes de mesclá-los à base de código.

Agora vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício de exploração extra – Implemente paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Com base no que aprendeu neste exercício, peça ao Copilot que atualize o site para implementar paginação. Considere:

- seguir as práticas recomendadas existentes, inclusive usando os arquivos de instruções do projeto.
- definir como a paginação deve funcionar: permitir que a pessoa usuária selecione o tamanho da página ou usar um valor fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos Ask, Edit e Agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
