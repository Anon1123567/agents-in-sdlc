---
title: "Exercício 1 - Instruções personalizadas (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Copilot cloud agent →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos de instruções específicos de caminhos.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não executará um prompt *antes e depois* neste exercício. O Copilot cloud agent trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto em tempo real. Você verá como o arquivo de instruções influencia o resultado mais adiante, ao revisar os pull requests criados pelo cloud agent.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar modo escuro e ter uma aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que explique sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas ajudam a orientar sugestões e trechos de código, especificando convenções, bibliotecas e até os tipos de comentários desejados. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas no chat ou no CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e outras diretrizes globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem fornecer diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para conclusões de código ou sugestões da próxima edição.
>
> Ao gerar código, o Copilot Chat, o Copilot CLI e o Copilot cloud agent usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções de outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado nas diretrizes do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as diretrizes claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefas, como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. A forma de fazer isso varia conforme o ambiente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição do ambiente usado explicará o processo quando for relevante.
>
> Para encontrar templates ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções deste repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as diretrizes aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicione um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionará uma regra para que a documentação seja incluída no código na forma de comentários TSDoc e cabeçalhos nos arquivos. O cloud agent usará essa regra quando trabalhar nas issues que você atribuir mais adiante neste percurso.

Antes de editar, crie uma branch (o cloud agent lê as instruções da branch de destino da issue, mas você fará um commit organizado):

1. No terminal do Codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra `.github/copilot-instructions.md` novamente.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela descreve os padrões de código, mas ainda não inclui uma regra de documentação no código — por isso, o helper gerado não tinha comentários de documentação.
3. Logo abaixo dos padrões existentes, adicione estas linhas Markdown para orientar o Copilot a incluir cabeçalhos nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para fornecer diretrizes globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões globais, como a regra de comentários de documentação que você acabou de adicionar.

## Faça commit, envie e mescle a atualização das instruções

O cloud agent lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, as instruções precisam estar em `main` para serem usadas pelo cloud agent.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Em github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O cloud agent lerá essas instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore as etapas da branch e faça commit diretamente em `main`. A branch foi incluída para que o workshop reflita o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o cloud agent gerar durante o restante do percurso.

Em seguida, você vai [atribuir sua primeira issue ao Copilot cloud agent][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
