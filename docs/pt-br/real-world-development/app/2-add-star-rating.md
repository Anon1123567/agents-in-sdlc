---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cards dos jogos e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você explorou o espaço de trabalho e usou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. A mudança será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada.
- pedir ao agente que faça uma pequena alteração focada no projeto.
- revisar a alteração na visualização de diff do espaço de trabalho.
- executar o aplicativo localmente para confirmar a alteração no navegador.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Porém, os cards da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão tem uma **worktree do Git e uma branch dedicadas**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar uma funcionalidade, outra para corrigir um bug — sem que as alterações entrem em conflito. As sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma delas para voltar à conversa correspondente.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** usadas enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório do GitHub. Agora criaremos uma sessão para esse repositório e solicitaremos a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Abaixo da caixa de prompt, escolha uma **new working tree** e o modo **Interactive**. Use o prompt a seguir para solicitar a alteração:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando uma nova worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados e cria o código necessário. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel, à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os cards dos jogos. Ele será semelhante ao exemplo a seguir: um pequeno bloco que mostra a avaliação quando ela existe e usa "No rating yet" quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar, mas deve ser relativamente parecido com o exemplo.

## Verifique as alterações

Antes de abrir um navegador, revise os resultados das verificações automatizadas do agente. Confirme que os testes cobrem um `starRating` numérico e o estado alternativo `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que abra o site para que possamos examinar a interface atualizada. Para isso, pediremos que ele inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do próprio aplicativo Copilot. Você explorará canvases personalizados e criará um mais adiante, mas agora usaremos o canvas de navegador integrado.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme que os cards de jogos avaliados exibem a nota de um total de cinco.
4. Quando terminar, peça ao Copilot que interrompa o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abra e mescle seu primeiro pull request

Você criou a funcionalidade. Agora é hora de abrir um pull request (PR) para mesclar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar.
3. Se o Copilot informar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o pull request no painel de revisão. Faça as revisões necessárias nesse painel.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para mesclar o PR.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o aplicativo GitHub Copilot. Em especial, você:

- iniciou uma sessão de agente e aprendeu como as sessões são estruturadas.
- orientou o agente a fazer uma pequena alteração focada nos cards dos jogos.
- revisou a alteração na visualização de diff do espaço de trabalho.
- executou o aplicativo localmente para confirmar a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez a mesclagem explicitamente.

Em seguida, você vai [começar pela issue de filtragem e usar os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
