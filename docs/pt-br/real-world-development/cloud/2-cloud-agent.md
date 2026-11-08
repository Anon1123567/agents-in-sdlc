---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se houver alguma — não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O cloud agent do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um PR de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades de maior impacto.

Neste exercício, você vai explorar como o Copilot cloud agent permite:

- personalizar o ambiente usado para gerar código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado ruim — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Por enquanto, a equipe não precisa de uma implementação refinada; basta algo que possa usar em testes de aceitação da experiência. Isso é um bloqueio, mas outras issues têm prioridade maior no momento.

Esses dois exemplos de tarefas podem acabar ficando em segundo plano e são ótimos candidatos para o Copilot cloud agent. Ele pode trabalhar neles de forma assíncrona enquanto você se dedica a outras atividades. Depois, você pode revisar o trabalho e confirmar que o resultado está correto.

## Conheça o GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa, há várias maneiras de delegar uma tarefa, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não é mágico — você ainda precisa pensar. Até operações aparentemente simples podem ficar complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático de instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao Copilot cloud agent, leia os arquivos de instruções deste repositório sem alterá-los para que você possa identificar seus efeitos mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao cloud agent na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos nos arquivos do pull request resultante. Esses elementos vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do Copilot cloud agent

Criar código normalmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto, independentemente de quem fará o trabalho. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O cloud agent usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar o ambiente criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas e dependências necessárias. O workflow já está configurado para o laboratório. Ele fornece Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona sem esperar o Copilot executá-lo.

## Adicione documentação

Embora todo mundo saiba que a documentação é importante, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ser ignorado, reduzindo a produtividade, dificultando a manutenção da base de código e tornando mais difícil integrar novas pessoas à equipe. Felizmente, o Copilot pode ajudar a criar documentação, e esta é uma issue ideal para atribuir ao Copilot cloud agent. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: ele explora o código e o contexto, pausa para planejar e tomar decisões e, depois, cria código.

Isso provavelmente levará alguns minutos. Um dos objetivos principais do Copilot cloud agent é executar tarefas de forma assíncrona para que você possa se dedicar a outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao cloud agent e, enquanto isso, escreveremos código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Uma das grandes vantagens do GitHub Copilot cloud agent é dividir o trabalho para que você possa se concentrar em uma tarefa enquanto ele cuida de outra. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não leve muito tempo, você poderia usar esse tempo para outras atividades. Vamos atribuir a tarefa ao Copilot cloud agent.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando no segundo pedido. O cloud agent atua de maneira semelhante a uma pessoa engenheira de software. Você não precisa acompanhá-lo ativamente; basta revisar o trabalho quando terminar. Agora vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [GitHub Copilot cloud agent][copilot-agents]. Com o cloud agent, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalização do ambiente usado para gerar código.
- execução segura de operações.
- importância de issues bem delimitadas.
- atribuição de issues ao Copilot.

Enquanto o cloud agent trabalha em segundo plano, vamos criar e usar agentes personalizados. O [Copilot cloud agent também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos nos módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuição de issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
