---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua cópia do repositório a partir do template."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que funciona como ponto central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e permite desenvolver com o GitHub Copilot. Durante este workshop, você trabalhará localmente no aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Antes de começar, vamos instalar o Node.js localmente e depois instalar o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para executar os testes do projeto no seu computador.
- criar sua própria cópia do projeto Tailspin Toys a partir do template.

## Instale o Node.js

Em várias lições, você pedirá a um agente que crie funcionalidades e execute localmente os testes do Tailspin Toys. Para isso, é necessário o [Node.js][nodejs], o único runtime exigido pelo projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. No sistema operacional, abra um terminal usando o Windows Terminal, o Terminal do macOS ou o aplicativo que você costuma usar.
2. Execute estes comandos para verificar as versões instaladas do Node.js e do npm:

    ```shell
    node --version
    npm --version
    ```

3. Se os dois comandos forem executados e o Node.js atender aos requisitos do README e do `package.json` do projeto, avance para a próxima seção.

> [!TIP]
> Conclua estas etapas somente se você ainda não tiver o Node instalado ou precisar atualizá-lo.

4. Abra a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha selecionada a opção **Add to PATH**.
7. Depois da instalação, abra uma nova janela do terminal.
8. Na nova janela, confirme a instalação:

    ```bash
    node --version
    npm --version
    ```

9. Você deverá ver as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa executar `node --version` e `npm --version`. O fato de os comandos funcionarem no seu terminal habitual não garante que estejam disponíveis para o aplicativo. Feche o aplicativo completamente e abra-o novamente depois de instalar o Node.js ou alterar o PATH. Se um dos comandos continuar falhando em uma sessão, siga as [etapas para solucionar problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para verificações E2E. Siga o README do repositório Tailspin Toys ao preparar uma worktree e revise qualquer solicitação de instalação antes de aprová-la.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório template][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório; você o conectará ao aplicativo durante a instalação.

1. Em uma nova janela do navegador, acesse o repositório do GitHub deste laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua cópia do repositório selecionando **Use this template** na página do repositório do laboratório. Depois, selecione **Create a new repository**.

    ![Botão Use this template com a opção Create a new repository selecionada no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Formulário Create a new repository com github-samples/tailspin-toys definido como template e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante no laboratório.

> [!NOTE]
> Ao criar o repositório a partir do template, você recebe automaticamente um backlog de issues do GitHub. Você trabalhará com essas issues durante todo o workshop — não precisa criar nenhuma.

Use uma cópia nova do template do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, uma skill de verificações de qualidade e uma extensão de canvas existente. Você personalizará a skill e criará um agente de QA durante o workshop. Se usar uma cópia antiga, confirme com a pessoa que está facilitando se ela contém os arquivos necessários.

## Resumo e próximos passos

O ambiente está pronto. Nesta lição, você:

- instalou o Node.js para que o projeto possa ser compilado e testado no seu computador.
- criou sua cópia do repositório Tailspin Toys a partir do template.

Em seguida, você vai [instalar o aplicativo GitHub Copilot][next-lesson], conectar o repositório recém-criado e conhecer o espaço de trabalho.

## Recursos

- [Baixe o Node.js][node-download]
- [Crie um repositório a partir de um template][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicie-uma-sessao-a-partir-da-issue
