---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do template, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele é baseado no GitHub Copilot CLI e se integra nativamente ao GitHub, para que seus repositórios, branches e pipelines de CI funcionem sem configuração adicional. O aplicativo foi desenvolvido para fluxos de trabalho nos quais você orienta vários agentes em paralelo — cada um em seu próprio espaço de trabalho isolado — em vez de fazer todo o trabalho por conta própria e automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar o projeto ao aplicativo por meio do repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo template.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro é preciso instalá-lo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu nome de host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial solicitar a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do template. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e depois selecione **Add GitHub repository**. Use o nome do proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que seu repositório aparece na barra lateral. Neste layout, as pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New**: como você pode imaginar, aqui você inicia uma nova sessão de chat com o Copilot.
- **Pull requests**: seus pull requests, exibidos por meio da integração nativa com o GitHub. Nessa área, você pode procurar e filtrar pull requests, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues**: suas issues, exibidas da mesma forma. Nessa área, você pode procurar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations**: tarefas de agente salvas, executadas em uma agenda ou sob demanda. Elas são úteis para gerenciar listas de tarefas, manutenção regular do projeto ou outras atividades repetitivas. O resumo apresenta links para automações como próximos passos, não como outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo Copilot na forma de servidores MCP, plugins, skills e outros componentes. Você usará esta área para configurar o Playwright MCP.
- **Chats**: conversas leves para perguntas e brainstorming que não precisam de uma branch ou espaço de trabalho. Esta seção aparece depois que você inicia seu primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar o primeiro chat.
- **Sessions**: agrupa pastas de repositórios e sessões de agentes. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização dele; ou selecione uma sessão abaixo do repositório para voltar à conversa. Cada sessão é executada em seu próprio espaço de trabalho isolado, permitindo executar várias sessões ao mesmo tempo sem conflito entre as alterações. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, pergunte. Ele ajudará você.

### Localize o backlog criado para você

É provável que não exista projeto sem backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado automaticamente quando você gerou o repositório a partir do template.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Depois, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas de itens em **Issues** e **Pull requests** são filtradas automaticamente para exibir somente itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso. Você pode fazer uma pergunta ou trocar ideias sem criar uma branch ou worktree, o que torna o chat rápido perfeito para uma pergunta descartável, sem precisar de uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat. **Chats** aparecerá depois disso.
2. Pergunte ao aplicativo como as sessões funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em sua própria worktree isolada do Git — isso permite executar vários agentes em paralelo sem que as alterações entrem em conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou seu projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar na sua conta do GitHub.
- adicionar um projeto por meio do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado para você em **Issues**.
- usar um chat rápido para fazer uma pergunta descartável.

Em seguida, você vai [iniciar sua primeira sessão de agente][next-lesson] e usá-la para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
