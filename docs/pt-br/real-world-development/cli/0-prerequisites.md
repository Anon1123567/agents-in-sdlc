---
title: "Lição 0 - Pré-requisitos"
description: "Crie sua cópia da Tailspin Toys e prepare um GitHub Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare tudo. Você criará uma cópia do repositório Tailspin Toys e iniciará um [Codespace][codespaces]. Na próxima lição, você usará o terminal integrado para instalar e executar o Copilot CLI.

Nesta lição, você vai:

- criar sua própria cópia do projeto Tailspin Toys a partir do template.
- criar um Codespace e confirmar que o projeto está pronto.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório template][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [template da Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando **Use this template** e depois **Create a new repository**.
3. Se você estiver participando de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), pois você precisará dele mais adiante no workshop.

> [!NOTE]
> Ao criar o repositório a partir do template, você recebe automaticamente um backlog de issues do GitHub. Você trabalhará com essas issues durante todo o workshop — não precisa criar nenhuma.

Use uma cópia nova do template do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, a skill `quality-checks` e o backlog necessário. Se usar uma cópia antiga, confirme com a pessoa que está facilitando se ela contém os arquivos necessários.

## Crie um Codespace

Em seguida, você usará um Codespace para concluir o workshop.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e depois **Create codespace on main**.
4. Aguarde o fim da configuração do Codespace. O template instalará as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se o prompt **Do you trust the authors of the files in this folder?** aparecer, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys carrega.
8. Volte ao terminal e interrompa o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumo e próximos passos

O ambiente está pronto. Nesta lição, você:

- criou sua cópia do projeto Tailspin Toys a partir do template.
- criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, você vai [instalar o GitHub Copilot CLI][next-lesson] no Codespace e autenticá-lo com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
