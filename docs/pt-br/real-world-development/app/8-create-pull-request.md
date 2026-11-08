---
title: "Lição 8 - Crie e mescle o PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você mesclou o PR das avaliações por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar o PR completo da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização da mesclagem sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o processo — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz a mesclagem assim que o GitHub permite. Ele trabalha em segundo plano, continua após reinicializações do aplicativo e é desativado quando o pull request é mesclado.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas a capacidade de editar código e mesclar ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão para mesclar.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione a seta ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. A escolha da opção no menu, por si só, não inicia o processo.

O processo do Agent Merge começa. Ele vai:

- criar o pull request com título e descrição.
- incluir no corpo da descrição uma referência à issue correspondente, se você tiver iniciado a sessão por uma issue.
- fazer rebase ou lidar com possíveis conflitos de mesclagem com a branch de destino.
- monitorar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar o PR em busca de comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer alterações para resolvê-los.
- opcionalmente, mesclar o PR automaticamente quando todas as verificações forem concluídas com sucesso.

Vamos permitir que o Agent Merge também faça a mesclagem do PR quando todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione a seta ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para fazer a mesclagem:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios relatados. Confirme que o status do PR é **Merged** antes de continuar. Ter habilitado a permissão de mesclagem ou um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou o PR completo da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerencie issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
