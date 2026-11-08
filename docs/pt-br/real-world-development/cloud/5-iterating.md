---
title: "Exercício 5: Iterando no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos explorar as alterações e, se necessário, enviar comentários para que o Copilot aprimore o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Portanto, vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pela equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent executa tarefas de forma assíncrona e sem supervisão, existem algumas restrições de segurança para manter o ambiente protegido:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O cloud agent é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Os workflows do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar explorando o primeiro pull request (PR) criado pelo GitHub Copilot cloud agent: a adição de documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso informando que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos, inclusive servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc e outros elementos de documentação. As alterações exatas variam.

    Ao analisar as alterações, procure os comentários TSDoc, as convenções de TypeScript e os cabeçalhos nos arquivos. Eles vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma interação de mão única. Você pode mencionar o Copilot nos comentários do PR — como faria com uma pessoa da equipe — ou em comentários no código. O Copilot verá esses comentários e iniciará outra sessão para atender ao pedido. Como os resultados não são determinísticos, não podemos indicar um texto exato para enviar. Algumas ideias de alterações que você pode pedir:

- adicionar cabeçalhos com uma breve descrição da finalidade no início de cada arquivo de código.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário sobre a documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou peça outra melhoria de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para implementar as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Volte ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marque o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Vamos voltar ao PR criado pelo Copilot para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você verá um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode selecionar esta branch no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados. Como antes, você pode iterar com o Copilot cloud agent e solicitar atualizações. Por exemplo, peça ao Copilot que ajuste a quantidade de jogos relacionados ou adicione cabeçalhos e comentários TSDoc (lembre-se: a issue foi atribuída **antes** da atualização das instruções personalizadas). Assim como nos PRs anteriores, basta adicionar um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes de interface para alternar os modos.
   - uso do armazenamento local do navegador para persistir as preferências.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.
6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprove e execute os workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode selecionar a branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se funcionam como esperado e se as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se notar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e peça as atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O cloud agent funciona melhor quando você atribui itens reais do backlog e se dedica a outras atividades. Para criar esse hábito, abra mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso de cloud agent. Nessas lições, você:

- **Inspecionou os arquivos de instruções personalizadas** do repositório para observar seus efeitos no resultado do cloud agent.
- **Atribuiu issues ao Copilot cloud agent** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e ajustou os pull requests do cloud agent**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do cloud agent. Para continuar explorando, veja os outros percursos que complementam o que você praticou:

- 🖥️ **[Percurso de VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso de CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, agente personalizado de QA, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o cloud agent.

No seu repositório, experimente estas ideias:

- atribua ao cloud agent uma issue de refatoração ou de cobertura de testes.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuição de issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]
- [Configure o firewall do Copilot cloud agent][agent-firewall]

---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
