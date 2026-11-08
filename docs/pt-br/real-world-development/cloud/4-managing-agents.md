---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao Copilot cloud agent que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas do agente com pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do cloud agent.
- orientar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já inclui atualizações de estilo do site e uma opção para alternar modos, parece lógico incluir essa funcionalidade. Você vai orientar o agente para garantir que ele adicione um modo claro além do modo de alto contraste.

## Revise as tarefas do Copilot cloud agent

Vamos verificar o status atual de todas as tarefas atribuídas ao Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações já executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR) à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o cloud agent

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que corresponde à criação do modo de alto contraste. O título exato varia de acordo com o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que o agente tenha concluído a configuração e começado o trabalho. Você saberá que isso aconteceu quando vir mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do cloud agent na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do GitHub Copilot cloud agent. Com esse painel, você acompanha tarefas ativas e concluídas, revisa o trabalho em andamento e redireciona sessões para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- usou o painel de controle e a página de agentes para monitorar tarefas do cloud agent.
- redirecionou uma sessão em andamento para solicitar uma funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iteração no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
