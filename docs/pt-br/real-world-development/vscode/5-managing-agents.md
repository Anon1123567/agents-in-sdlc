---
title: "Exercício 5 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um botão de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design faz outro pedido: também quer um botão de modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão atual para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos que ele explora, comandos que executa e alterações que propõe. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e comandos de terminal cuja execução foi solicitada.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Selecione também o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá o pedido **Allow**. Aprove apenas comandos que você considera seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele exibirá uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste pronto, você ampliará a mesma conversa para adicionar um botão de modo claro. Como continuará na mesma sessão, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos botões de uma maneira que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de acompanhamento para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro botão; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois modos").

## Revise os arquivos alterados

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se seguem os padrões de acessibilidade definidos pelo agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar ou rejeitar edições individuais.
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações criadas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Como usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iteração no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
