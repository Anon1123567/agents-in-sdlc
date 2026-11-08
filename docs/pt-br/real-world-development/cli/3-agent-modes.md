---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para combinar uma abordagem, o Autopilot para implementar a filtragem descrita em uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a criar o que é necessário da maneira adequada. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para criar uma funcionalidade, verificará se o código está correto e se o comportamento esperado foi implementado e, por fim, fará a mesclagem no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante todo o fluxo da funcionalidade. Normalmente, você usaria branches ou PRs diferentes para cada tipo de arquivo, mas vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação por MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas é necessário combinar detalhes — como a combinação de categorias — antes de escrever o código. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

A introdução de agentes de programação de IA no fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles ficam ainda mais importantes. O fluxo da maioria dos desenvolvedores se parece com este:

1. Comece por uma issue aberta que descreve o trabalho necessário.
2. Crie um plano do que precisa ser implementado.
3. Crie e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, faça a mesclagem.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma versão desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR das avaliações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` adiciona o contexto de uma issue ou pull request (PR) do GitHub.

## Planeje a funcionalidade de filtragem

O planejamento permite definir a abordagem de uma funcionalidade ou tarefa antes de delegá-la ao Copilot. Para qualquer trabalho complexo, é recomendável reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano: primeiro explorará o projeto e depois determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot para revisar qualquer parte que não corresponda às decisões que você tomou.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere até considerar que o problema foi resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano aprovado, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot para resolver eventuais falhas.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros de categoria, de editora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos nos aprofundar na geração de código e garantir que ela siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
